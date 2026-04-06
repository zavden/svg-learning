# CHALLENGE — Interactividad con JavaScript

Seis retos para consolidar el kit de interactividad SVG. Empieza por el 1 y sube progresivamente.

---

## Reto 1 — "Click para pintar"

Haz un SVG vacío que se rellene con círculos al hacer click:
- Usa `svgCoords(e, svg)` para obtener las coordenadas correctas en el viewBox.
- Crea un `<circle>` con `createElementNS` en esas coordenadas.
- Asigna un color aleatorio y un radio pequeño.
- Bonus: si mantienes el mouse pulsado y mueves (`pointermove`), deja un rastro de puntos.

**Criterio**: los círculos aparecen exactamente bajo el cursor aunque el SVG esté escalado o con padding.

---

## Reto 2 — "Drag and drop con snap"

Crea tres cuadrados de colores arrastrables:
- Implementa `makeDraggable` usando `setPointerCapture`.
- Añade snap a una rejilla de 20px: `Math.round(x / 20) * 20`.
- Muestra la rejilla de fondo como líneas discretas (`pointer-events: none` para que no roben los clicks).
- Cuando el cuadrado se suelte, anima suavemente con CSS transition hasta la celda final.

**Criterio**: soltar el cuadrado lo "imanta" a la celda más cercana, incluso si la soltaste cerca del borde.

---

## Reto 3 — "Conectar puntos" (mini editor de grafos)

Un SVG donde puedes:
- Click en vacío → crea un nodo (círculo numerado).
- Click en dos nodos seguidos → crea una línea entre ellos.
- Arrastrar un nodo → todas sus conexiones se actualizan en tiempo real.
- Tecla `Delete` sobre un nodo seleccionado → lo elimina junto a sus conexiones.

**Conceptos**: creación dinámica, delegación de eventos, estado compartido, actualización coordinada de múltiples elementos.

---

## Reto 4 — "Scatter plot con tooltip"

Dada una lista de `{x, y, label, value}`:
- Renderiza cada punto como un `<circle>` con `data-*` que guarde la info.
- Un único listener de `pointerover` en el SVG (delegación): muestra un tooltip HTML flotante junto al punto.
- Usa `getBoundingClientRect()` del círculo para posicionar el tooltip en coordenadas de pantalla.
- El tooltip se oculta en `pointerout`.
- Bonus: al hacer click en un punto, lo "fijas" como seleccionado con una clase CSS.

**Criterio**: funciona aunque pases el mouse muy rápido sobre muchos puntos (delegación eficiente).

---

## Reto 5 — "Zoom y pan con la rueda"

Implementa navegación libre sobre un SVG grande:
- **Pan**: arrastrar con el botón medio (o `Space` + arrastre izquierdo) mueve el viewBox.
- **Zoom**: la rueda del mouse cambia el tamaño del viewBox, centrado en el cursor.
- Mantén la posición del mundo bajo el cursor constante durante el zoom (zoom "to cursor").
- Botón "Reset" que vuelve al viewBox original.

**Concepto clave**: el viewBox es la herramienta idiomática para transformar todo el contenido de una sola vez.

---

## Reto 6 — "Animación a lo largo de un path"

Dado un `<path>` curvo:
- Usa `getTotalLength()` para obtener su longitud.
- Anima un objeto (un coche/cometa) recorriendo el path con `requestAnimationFrame`.
- En cada frame, llama a `getPointAtLength(distancia)` para obtener la posición.
- Calcula la tangente con dos puntos consecutivos y rota el objeto para que "mire hacia adelante".
- Añade un slider HTML para controlar manualmente la posición en el path (0 a 1).

**Criterio**: es un equivalente manual a `<animateMotion rotate="auto"/>` de SMIL, pero todo en JS.

---

## Extras opcionales

- **Kit propio**: agrupa `svgEl`, `svgCoords`, `makeDraggable` en un módulo `svgkit.js` y publícalo como `npm pack` local. Reutilízalo entre los retos.
- **Sanitización real**: integra DOMPurify en un editor que permita pegar SVG ajeno — comprueba que los `<script>` maliciosos desaparecen.
- **Accesibilidad**: haz que el Reto 2 sea totalmente navegable con teclado (flechas para mover, Enter para soltar).
