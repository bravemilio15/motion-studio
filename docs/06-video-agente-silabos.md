# Video: Agente-Sílabos

Referencia: `escenas_1_2.mp4` (36 s, 1280×720, 30 fps, h264, **sin audio**).
No se versiona (ver `.gitignore`); guardarlo en `videos/agente-silabos/refs/`.

## Qué es

Explainer sobre Agente-Sílabos (homologación curricular asistida por IA
local, Universidad Nacional de Loja). Cinco escenas:

1. El estudiante llega con su solicitud de homologación.
2. Administración recibe y registra el expediente.
3. Los expedientes esperan turno; avisos de retraso y de errores manuales.
4. El agente lee el sílabo, lo compara con el catálogo y redacta el informe.
5. El agente aligera la carga; la decisión sigue siendo del docente.
   Cierre: tarjeta de título.

Estilo: vectorial plano, beige / pizarra / azul / ámbar.

## Problemas observados

- Fade cruzado hacia la escena 4: se ve un fantasma del escritorio y el
  reloj tras la tarjeta blanca.
- La burbuja "Comparando con el catálogo UNL" superpone los puntos de carga
  con la "L" final.
- Datos de relleno: las tres filas del catálogo dicen "Asignatura de la
  UNL"; no se ve comparación real ni coincidencias/diferencias.
- Tarjeta "Informe de homologación" diminuta; el resultado casi no se lee.
- Ritmo desigual: escenas 1–3 con poco movimiento, escena 4 concentra el
  contenido.
- Cierre de texto centrado sobre fondo liso.
- Sin audio, sin cámara.

## Plan de mejora

- 1920×1080, 60 fps, `seek(t)` determinista, resortes cerrados, 4 subframes.
- Sin fades cruzados: match cuts (cámara entrando en la hoja del expediente
  hasta convertirse en la tarjeta del sílabo).
- Escena 4 como núcleo: asignatura de ejemplo con contenidos, resultados de
  aprendizaje y horas; líneas de conexión que se dibujan; barras de
  similitud; coincidencias en verde y diferencias en ámbar.
- Informe legible con zoom de cámara, porcentaje animado y sello final de
  "Decisión del docente".
- Escenas 1–3 más cortas, un gag visual por escena (pila de expedientes que
  se tambalea).
- Audio sintetizado: teclas, sellos, whooshes y base suave, a 100–120 BPM.
- Cierre en movimiento: el título se arma desde el robot y elementos previos,
  con loop al primer frame.
- Entregables: MP4 16:9 y 9:16 (reencuadrado), póster, contact sheet.

## Advertencia sobre datos

Porcentajes, horas y resultados mostrados serían **datos de muestra**.
Rotular como "ejemplo ilustrativo" o sustituir por un caso real de la tesis
si el video va a una defensa o presentación formal.

## `brief/source.md`

Plantilla en `videos/agente-silabos/brief/source.md`.
