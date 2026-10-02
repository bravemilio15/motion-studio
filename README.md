# motion-studio

Repo único para producir videos de motion design renderizados desde código
(`seek(t)` + Playwright + ffmpeg). Cada video vive en `videos/<nombre>/` y
reutiliza `lib/` y `tools/`.

## Mapa de documentación

| Archivo | Contenido |
|---|---|
| `docs/01-estructura.md` | Por qué un repo de estudio, aparte del repo de la tesis |
| `docs/02-git-flujo.md` | Ramas, guardados, merge, tags, worktrees |
| `docs/03-setup.md` | Instalación (Node, ffmpeg, Python, Playwright, Claude Code) |
| `docs/04-prompting.md` | Niveles de prompt: one-liner → brief de director |
| `docs/05-pipeline-tecnico.md` | `seek(t)`, resortes, beats, bucle de crítica |
| `docs/06-video-agente-silabos.md` | Diagnóstico del video de referencia y plan de mejora |
| `CLAUDE.md` | Reglas globales que Claude Code lee en cada sesión |

## Inicio rápido

```bash
git switch -c video/<nombre>      # rama de trabajo del video
# ... iterar con Claude Code dentro de videos/<nombre>/ ...
git switch main && git merge --no-ff video/<nombre>
git tag -a <nombre>-v1 -m "versión cerrada"
git push origin main --tags
```

Detalle completo en `docs/02-git-flujo.md`.

## Nota de verificación

Parte del contenido proviene de un hilo de X sobre Opus 5.5 que no se pudo
abrir directamente (el texto fue pegado por el usuario). Comandos, repos y
métricas citados no están verificados: comprobar antes de depender de ellos.
