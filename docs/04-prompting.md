# Niveles de prompt

El prompt es ~10 % del resultado; el 90 % es el arnés (motor de render,
referencias, bucle de crítica).

| Nivel | Qué se entrega | Para qué sirve |
|---|---|---|
| 1. One-liner | "showreel de motion designer de 15 s, go all out" | Probar que el motor funciona. Produce resultados que se parecen entre sí |
| 2. Producto | URL + "usa capturas, logo y assets reales" + "debe tener música" | Videos de producto |
| 3. Referencia | Un frame, un video o una carpeta de imágenes | Dar ritmo, tipografía y transiciones |
| 4. Spec XML | 8–12 estados de UI, datos reales, paleta, pista ~120 BPM, formatos | Morphs de UI de una sola forma |
| 5. Brief de director | Logline, refs, herramientas, personaje, beat sheet, puertas, crítica, entregables | Producciones largas y multisesión |

## Referencia: qué tomar y qué no

- Frame: indicar qué tomar (paleta, tipografía, grano) y qué no (sujeto).
- Video: pedir extraer frames con ffmpeg y describir ritmo plano a plano
  antes de escribir código.
- Carpeta propia: pedir primero un `style_guide.md`.
- Pedir `docs/style_guide.md` y `docs/shotlist.md` y **esperar el OK** antes
  de escribir código.
- Dejar que el modelo elija la técnica; fijar el look y las restricciones,
  no la librería (salvo que se necesite reutilizar un framework).

## Plantilla: producto

```
Make a dynamic 20-second motion graphics video for [PRODUCT] ([URL]).
Assets: visitar el sitio, usar capturas reales (Playwright), logo, colores y
fuentes reales; guardar en ./assets y listar antes de animar.
Nunca redibujar la UI del producto de memoria.
Story (un beat de 2–4 s): 1 hook 2 producto aparece 3 tres funciones
4 una cifra que prueba 5 logo + CTA.
Sound: música original 120 BPM sintetizada en código; clics y whooshes al beat.
Format: 9:16 primero, luego 1:1 y 16:9 desde la misma línea de tiempo.
Antes del render final, mostrar contact sheet de un frame por beat.
```

## Plantilla: brief de director (esqueleto)

1. Película en una línea (logline y objetivo emocional).
2. Referencias y entradas (qué tomar, qué no).
3. Herramientas y keys (skills, APIs en `.env`, presupuesto).
4. Look (paleta, tipografía, textura, cámara, looks prohibidos).
5. Beat sheet con tiempos; un payoff visual cada 3–5 s; gancho en los 2 s.
6. Flujo con puertas: plan → rig → stills → animatic → pasada completa →
   pulido → audio → render.
7. Bucle de crítica: renderizar stills, puntuar, listar los 3 peores
   problemas, corregir, repetir hasta 8+.
8. Entregables: `final.mp4`, `loop_check.mp4`, `poster.png`, `contact.png`,
   README.

Pedir por nombre `docs/ANIMATION_GUIDE.md` (para subagentes) y
`docs/STORYBOARD.md`.

## Limitaciones del método (según el hilo)

- Funciona para lo dibujable en código: tipografía cinética, motion de UI,
  formas, personajes 2D, estilos como acuarela.
- Para física o personajes complejos, un caso usó un modelo de video para
  tomas base y redibujó encima en JavaScript.
- No cubre video fotorrealista.
- "One prompt" suele ser exagerado: hay casos con cientos de llamadas y
  varias horas, y prompts de ~10k caracteres con skills y keys.

## Empaquetar como skill

Al estabilizar el flujo, guardar el pipeline como skill
(`.claude/skills/motion-reel/SKILL.md`) con: entradas a recopilar, pasos del
pipeline (assets → style_guide → beats → shotlist con OK → `index.html` →
crítica ≥3 rondas → render y mezcla a −14 LUFS → entrega) y reglas duras
(UI real, sin `Math.random`, sin timers, sin transiciones CSS en render).
