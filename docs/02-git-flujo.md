# Flujo de git: ramas, guardados y tags

## Modelo

- `main`: base del estudio (`lib/`, `tools/`, `CLAUDE.md`, skill) + videos
  terminados en `videos/`.
- `video/<nombre>`: rama **temporal** mientras se trabaja un video.
- Tag al cerrar: `<nombre>-v1`, `<nombre>-v2`...

### Por qué NO ramas permanentes por video

- Las ramas sirven para versiones divergentes del mismo código, no para
  entregables independientes.
- Las mejoras de `lib/` hechas en una rama no llegan a las otras (requiere
  `cherry-pick`/`merge` manual).
- Se pierde la reutilización, que es el objetivo del estudio.
- No se ven todos los videos a la vez; hay que hacer `checkout`.
- `.gitignore` y dependencias derivan entre ramas.

Única excepción razonable: una rama larga para una versión alternativa que
diverge mucho (p. ej. vertical vs 16:9). Normalmente se resuelve mejor con un
parámetro de formato dentro de la misma carpeta.

## Ciclo de un video

```bash
# 1. Crear la rama
git switch main && git pull
git switch -c video/agente-silabos

# 2. Trabajar y guardar (ver "Guardados")
git add videos/agente-silabos && git commit -m "video(agente-silabos): escena 4 con comparación"

# 3. Mejoras de lib/ en commits SEPARADOS
git add lib && git commit -m "lib(motion): añade swapAlpha()"

# 4. Integrar al terminar
git switch main
git merge --no-ff video/agente-silabos     # conserva commits separados
git push origin main

# 5. Congelar la versión
git tag -a agente-silabos-v1 -m "Versión cerrada, 1080p60, 16:9"
git push origin --tags

# 6. Limpiar la rama
git branch -d video/agente-silabos
```

### `--no-ff` vs `--squash`

- `--no-ff`: mantiene el historial, incluidos los commits de `lib/` por
  separado. Recomendado.
- `--squash`: un solo commit limpio, pero **junta los cambios de `lib/` con
  los del video**, y la rama no queda marcada como fusionada (se borra con
  `git branch -D`). Si se usa squash, integrar primero las mejoras de `lib/`
  por una rama aparte (`lib/<tema>`) y luego el video.

## Guardados (cómo no perder trabajo)

- **Commit frecuente** en la rama del video; el historial ruidoso ahí es
  aceptable.
- **Convención de mensajes:** `video(<nombre>): ...`, `lib(<modulo>): ...`,
  `tools(<herramienta>): ...`, `docs: ...`.
- **Push de la rama** al remoto para respaldo: `git push -u origin video/<nombre>`.
- **Trabajo a medias:** `git stash push -m "descripcion"` y luego
  `git stash pop`.
- **Punto de retorno antes de un cambio arriesgado** (p. ej. editar `lib/`):
  `git tag -a punto-<tema> -m "antes de ..."` en la rama.
- **Descartar un experimento:** `git switch main && git branch -D video/<nombre>`.
- **Recuperar un estado:** `git switch --detach <tag>`.

## Tags

- Usar tags **anotados** (`-a`), con mensaje: formato, resolución, notas.
- Nombre: `<video>-v<N>`.
- Reproducir un render exacto aunque `lib/` haya cambiado:
  ```bash
  git switch --detach agente-silabos-v1
  node tools/render.mjs videos/agente-silabos
  ```
- Alternativa a tags: copiar `lib/` dentro de la carpeta del video al
  cerrarlo (congelado total, a costa de duplicar código).
- Los tags no se suben con `git push` normal: usar `git push origin --tags`.

## Dos videos en paralelo: `git worktree`

```bash
git worktree add ../studio-otro -b video/otro-video
# abrir una segunda sesión de Claude Code en ../studio-otro
git worktree remove ../studio-otro       # al terminar
```

Comparte el mismo `.git`, sin duplicar el repo ni hacer `checkout`
constante.

## Archivos pesados

- `out/`, `frames/` y videos de referencia están en `.gitignore`.
- Para conservar los MP4 finales: Git LFS (`git lfs track "*.mp4"`) en una
  carpeta dedicada, o almacenamiento externo con un enlace en el README del
  video.

## Primer commit del repo

```bash
git init -b main
git add . && git commit -m "chore: estructura inicial del estudio"
git remote add origin git@github.com:<usuario>/motion-studio.git
git push -u origin main
```
