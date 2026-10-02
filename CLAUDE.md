# Motion studio: reglas globales

## Contrato de render
- Cada película es una función pura del tiempo: `window.seek(t)` pinta el
  frame t.
- Sin transiciones CSS, sin `setTimeout`, sin `requestAnimationFrame` en modo
  render, sin estado entre frames. Ruido solo con semilla (mulberry32), nunca
  `Math.random`.
- Render con `node tools/render.mjs videos/<nombre>`; H.264 yuv420p, CRF 16.
- Resortes de forma cerrada desde `lib/motion.js`; un valor con varios
  objetivos usa `track()`.

## Look
- Prohibido por defecto: título centrado sobre degradado, todo apareciendo
  con fade, etiquetas de esquina y marcos, brillo en UI, estallidos de
  partículas genéricos.
- Una tipografía de display, una de UI; un solo color de acento salvo que el
  brief diga otra cosa.
- Cada 2–4 s debe ocurrir algo nuevo en pantalla.

## Sonido
- Música y SFX sintetizados en código salvo que se entregue una pista.
- Golpes sobre la cuadrícula de beats medida (`beats.json`). −14 LUFS.

## Bucle antes de mostrar nada
1. Renderizar un frame por beat como contact sheet y MIRARLO.
2. Puntuar 1–10: gancho en 2 s, legibilidad a tamaño teléfono, calidad de
   movimiento, variedad, fidelidad de marca, sincronía de sonido.
3. Corregir los 3 peores problemas. Repetir hasta 8+ en todo.
4. Solo entonces, el render completo.

## Datos y marca
- Nunca redibujar la UI de un producto de memoria: usar capturas/assets
  reales.
- Cifras y resultados mostrados: reales o rotulados "ejemplo ilustrativo".
- Referencias: tomar la gramática, nunca el contenido, logos ni personajes.

## Trabajo en git
- Una rama por video: `video/<nombre>`; mejoras de `lib/` en commits
  separados con prefijo `lib(...)`.
- No versionar `out/`, `frames/`, `.env` ni videos de referencia.
- Flujo completo en `docs/02-git-flujo.md`.

## Alcance
- Trabajar dentro de `videos/<nombre>/` salvo cambios deliberados en `lib/`
  o `tools/`. No explorar todo el repo sin necesidad.
