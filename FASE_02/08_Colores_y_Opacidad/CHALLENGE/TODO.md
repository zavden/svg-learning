# Retos — Colores y Opacidad

Ideas de ejercicios para practicar los conceptos de esta sección.

---

1. **Paleta HSL**: generar programáticamente (o manualmente) una paleta de 7 tonos del mismo color usando `hsl(220, 70%, L%)` con L desde 15% hasta 95%. Mostrarlos en fila como swatches de color con etiquetas.

2. **Círculos solapados con y sin grupo**: crear dos conjuntos de 3 círculos solapados del mismo color. En el primero, aplicar `fill-opacity="0.5"` a cada círculo. En el segundo, usar `opacity="0.5"` en el `<g>` que los contiene. Observar y anotar la diferencia.

3. **Icono con currentColor**: diseñar un icono SVG simple (corazón, estrella o campana) usando `fill="currentColor"`. Embeber el mismo icono tres veces en tres contextos HTML con distintos valores de `color` CSS. Verificar que el icono adopta el color de cada contexto.

4. **Zona de click invisible**: crear un botón pequeño (60×30) centrado en un rect de 300×80. Añadir un segundo `<rect fill="transparent">` más grande (120×60) centrado en el mismo punto. Demostrar que el hover se activa en el área más grande.

5. **Modo oscuro automático**: crear un SVG con colores del sistema (`Canvas`, `CanvasText`) y verificar que cambia automáticamente entre modo claro y oscuro del SO sin necesidad de media queries adicionales.

6. **Fade-in con opacity**: animar un grupo `<g>` con `opacity` de 0 a 1 usando CSS `@keyframes`. Verificar que fill, stroke e hijos se animan juntos. Comparar con animar `fill-opacity` en el elemento directamente.
