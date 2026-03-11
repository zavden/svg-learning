# Retos — Relleno y Trazo

Ideas de ejercicios para practicar los conceptos de esta sección.

---

1. **Paleta de opacidades**: dibujar 5 círculos solapados, todos del mismo color pero con `fill-opacity` de 0.2, 0.4, 0.6, 0.8 y 1.0. Observar cómo las zonas de solapamiento se acumulan visualmente.

2. **Galería de linecaps y linejoin**: crear un SVG con tres secciones. En cada sección, dibujar la misma polilínea en forma de zigzag con `stroke-width="12"` usando las tres variantes de `linecap` y `linejoin`. Añadir etiquetas.

3. **Tarjeta con stroke exterior**: diseñar una tarjeta de 200×120 con `rx="12"`, fill blanco y un stroke azul de 4px. Usar `paint-order="stroke fill"` para que el stroke sea puramente exterior. Comparar con y sin `paint-order`.

4. **Animación de línea que se dibuja**: crear un path con una curva Bézier larga, calcular su longitud aproximada (o con `getTotalLength()`), y animarla con CSS usando `stroke-dasharray` y `stroke-dashoffset` de longitud a 0 en 2 segundos.

5. **Grid técnico con non-scaling-stroke**: crear un grid de 5×5 líneas usando `vector-effect="non-scaling-stroke"`. Añadir un `transform="scale(2)"` al grupo y verificar que las líneas siguen siendo finas mientras las formas que las cruzan se escalan.

6. **Icono con currentColor**: diseñar un icono SVG simple (estrella, corazón, o campana) que use `fill="currentColor"`. Embeber el icono en tres contextos diferentes con distintos valores de `color` CSS para ver que el icono adopta el color del contexto.
