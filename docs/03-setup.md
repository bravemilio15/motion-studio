# Setup

Los comandos provienen del hilo base y **no están verificados**; comprobar
versiones y nombres de paquetes antes de depender de ellos.

## Runtime

```bash
# macOS (en Linux: apt install)
brew install node ffmpeg python
pip install numpy librosa soundfile

npm init -y
npm i -D playwright && npx playwright install chromium
```

Requisitos citados: Node 22+, ffmpeg, Python con numpy/librosa/soundfile.

## Claude Code

```bash
claude --model claude-opus-5-5
# dentro: /model y elegir esfuerzo
```

| Esfuerzo | Uso |
|---|---|
| medio | retoques y re-renders |
| xhigh | videos nuevos |
| max | piezas donde los primeros 3 s deben cargar todo |

## Skills (opcionales)

Por defecto el modelo usa la ruta A (un `index.html` con `seek(t)`,
Playwright, ffmpeg) y **ignora** Remotion y HyperFrames salvo petición
explícita. Se recomienda empezar sin skills.

```bash
npx skills add remotion-dev/skills                  # React, series/plantillas
npx skills add heygen-com/hyperframes               # HTML + GSAP
claude plugin marketplace add buildwithhanif/claude-animation-skill
claude plugin install claude-animation@claude-animation-skill
```

## Permisos sugeridos

El hilo no los especifica; esto es una recomendación: permitir `node`, `npx`,
`ffmpeg`, `python` y escritura dentro de este repo. API keys (ElevenLabs,
FAL) en `.env` (ya ignorado por git); indicar a Claude solo el nombre de la
variable, nunca pegar la clave en el prompt.

## Pendiente de crear

`lib/motion.js`, `tools/render.mjs`, `tools/beats.py`, `tools/sfx.mjs`
(código de referencia en `docs/05-pipeline-tecnico.md`).
