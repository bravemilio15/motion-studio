# Estructura y decisiones

## Decisión 1: repo aparte del repo de la tesis

El video se hace en un repo distinto al de la tesis. Motivos:

- **`CLAUDE.md`:** Claude Code lo carga en cada sesión. Las reglas de video
  contaminarían el trabajo sobre RAG, FastAPI, n8n o el anteproyecto.
- **Peso en git:** `node_modules`, Chromium de Playwright, frames y MP4 de
  render crecen rápido; cada iteración del bucle de crítica genera uno nuevo.
- **Stack distinto:** tesis en Python; video en Node + ffmpeg + librosa.
- **Permisos:** el bucle de render exige permisos amplios (`node`, `ffmpeg`,
  `npx`, escritura). Más seguro en un repo desechable.
- **Historial limpio:** un agente iterando horas deja commits ruidosos que no
  deben entrar al repo que se entrega o se defiende.

En el repo de la tesis solo queda el entregable final (`final.mp4`, póster,
README de cómo se generó) o un enlace a este repo.

## Decisión 2: un repo de estudio, no un repo por video

Un solo repo con muchos videos permite reutilizar librerías, personajes y
setup. Un repo por video solo tiene sentido si otras personas lo mantienen o
si se publica como proyecto abierto independiente.

## Layout

```
motion-studio/
├── CLAUDE.md                 reglas globales
├── package.json              una sola instalación de Playwright
├── lib/
│   ├── motion.js             spring(), track(), indicator(), swapAlpha(), loopT()
│   ├── draw.js               helpers de canvas (tarjetas, texto, sombras)
│   └── characters/           robot, personas (reutilizables)
├── tools/
│   ├── render.mjs            node tools/render.mjs videos/<nombre>
│   ├── beats.py              análisis de beats (librosa)
│   └── sfx.mjs               SFX sintetizados
├── .claude/skills/motion-reel/   skill empaquetado (ver docs/04)
└── videos/
    └── <nombre>/
        ├── CLAUDE.md         reglas específicas (opcional)
        ├── brief/source.md   única fuente de verdad de textos y datos
        ├── refs/             referencias (no versionar videos pesados)
        ├── index.html        importa ../../lib/*
        └── out/              salidas, en .gitignore
```

`CLAUDE.md` funciona en capas: Claude Code lee el de la raíz y el de la
carpeta donde se trabaja.

## Relación con el repo de la tesis

- La fuente de verdad conceptual es la tesis. Copiar lo necesario a
  `videos/<nombre>/brief/source.md` (textos de pantalla, nombres de
  componentes, datos de ejemplo).
- Alternativa: dar acceso de lectura con `claude --add-dir ../repo-tesis`.
  **Verificar la bandera con `claude --help`**; no está confirmada.
- Cualquier cifra del video (porcentajes, horas) debe ser un caso real de la
  tesis o rotularse "ejemplo ilustrativo".

## Qué cuidar

- Cambios en `lib/` pueden alterar el render de videos ya terminados. Se
  mitiga con tags (`docs/02-git-flujo.md`) o congelando una copia de `lib/`.
- No versionar MP4 de salida ni frames. Para conservar finales: Git LFS o
  almacenamiento externo.
- En repos grandes, pedir a Claude que trabaje solo en la carpeta del video.
