# Pipeline técnico

## Idea central

El modelo no emite video: escribe un programa. Una función `window.seek(t)`
pinta el frame exacto de cualquier instante; un navegador headless la llama
N veces, captura cada frame y ffmpeg los une. Sin timers, el render es
idéntico en cada corrida y cambiar algo es editar una línea y re-renderizar.

## Contrato de render

- Todo es función pura del tiempo.
- Prohibidos: transiciones CSS, `setTimeout`, `requestAnimationFrame` en modo
  render, estado entre frames.
- Ruido solo con semilla (mulberry32), nunca `Math.random`.
- H.264 yuv420p, CRF 16.

## Resortes de forma cerrada

Un resorte amortiguado 0→1 como función de `t` evita simular frames previos:
se puede renderizar el frame 812 sin simular del 0 al 811.

```js
function spring(t, k = 170, d = 26) {
  if (t <= 0) return 0;
  const w0 = Math.sqrt(k), z = d / (2 * w0);
  if (z < 1) {
    const wd = w0 * Math.sqrt(1 - z * z);
    return 1 - Math.exp(-z * w0 * t) *
      (Math.cos(wd * t) + (z * w0 / wd) * Math.sin(wd * t));
  }
  return 1 - Math.exp(-w0 * t) * (1 + w0 * t); // z >= 1: crítico
}

// Valor con varios objetivos: un resorte por cada cambio, cada uno desde su instante
function track(t, keys, k = 170, d = 26) {
  let v = keys[0][1];
  for (let i = 1; i < keys.length; i++)
    v += (keys[i][1] - keys[i - 1][1]) * spring(t - keys[i][0], k, d);
  return v;
}
```

Guía de rigidez: UI nítida (botones, bordes de ataque) alta; contenedores y
cámara por defecto; tipografía grande y logos pesados; mascotas con rebote
visible. Sobreimpulso mínimo en UI, ninguno en tipografía.

## Render con motion blur

- 60 fps con 4 subframes por frame, mezclados en ffmpeg con `tmix`.
- Esperar `document.fonts.ready` antes de capturar (texto en canvas).
- Vista previa en vivo solo si `!navigator.webdriver`.

```
node tools/render.mjs videos/<nombre> --fps 60 --dur 36 --sub 4
```

## Audio

- Con pista propia: medir con librosa (`beats`, `downbeats`, `hits`) y
  guardar `beats.json`; los cambios de estado van en beats, los momentos
  grandes en downbeats, los SFX en picos medidos. Empezar en un downbeat.
- Sin pista: sintetizar música y SFX en código sobre la misma línea de
  tiempo que la imagen (clic, pop, thump, whoosh).
- Mezcla objetivo: −14 LUFS.
- Un agente puede analizar el audio numéricamente (picos, sincronía), pero
  el criterio auditivo es del usuario.

## Bucle de crítica

El modelo mira sus propios frames. Es lo que separa un resultado mediocre
de uno publicable.

```bash
# Contact sheet: 2 fps, 6 columnas
ffmpeg -i out/final.mp4 -vf "fps=2,scale=270:-1,tile=6x5" -frames:v 1 out/contact.png
# Tira de 12 frames alrededor de una acción rápida (4.2 s)
ffmpeg -ss 4.1 -i out/final.mp4 -vf "scale=320:-1,tile=12x1" -frames:v 1 out/strip.png
# Prueba a 360 px de ancho (lectura en teléfono)
ffmpeg -i out/final.mp4 -vf "fps=1,scale=360:-1,tile=5x3" -frames:v 1 out/phone.png
# Revisión del bucle (costura)
ffmpeg -stream_loop 1 -i out/final.mp4 -c copy out/loop_check.mp4
```

Puntuar de 1 a 10: gancho en 2 s, legibilidad a tamaño teléfono, calidad de
movimiento, variedad (algo nuevo cada 2–4 s), composición, fidelidad de
marca, sincronía de sonido. Listar los 3 peores problemas con marca de
tiempo, corregir, re-renderizar solo los segundos afectados, repetir hasta
8+.

Buscar en concreto: texto superpuesto durante cambios, elementos que se
deslizan en vez de usar resorte, etiquetas de esquina y marcos, tomas
centradas sobre degradado, texto escalado borroso, compases muertos y
saltos en la costura del bucle.

Comprobación de determinismo: renderizar dos veces el mismo tramo y comparar
hash.

## Formatos

Escribir las escenas contra una función de layout, no píxeles fijos. Reencuadrar
tipografía y UI por formato (9:16, 1:1, 16:9); no recortar un 16:9.

## Código de referencia

El hilo base incluye implementaciones completas de `render.mjs`, `beats.py`
y `sfx.mjs`. Pedir a Claude Code que las genere dentro de `tools/` según este
documento y `CLAUDE.md`.
