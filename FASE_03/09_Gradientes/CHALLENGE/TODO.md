# Retos — Gradientes

Ideas de ejercicios para practicar los conceptos de esta sección.

---

1. **Botón con gradiente**: crear un botón SVG (rect con rx, texto centrado) con un gradiente lineal diagonal en el fill. Al definir un segundo gradiente idéntico rotado 180°, usarlo como "estado hover" cambiando la clase con CSS.

2. **Esfera 3D**: crear un círculo que parezca una esfera usando `radialGradient` con `fx` y `fy` desplazados hacia la esquina superior izquierda. Añadir un stop de blanco semitransparente al inicio para el highlight.

3. **Galería de spreadMethod**: definir un gradiente pequeño (del 25% al 75%) y aplicarlo con los tres valores de `spreadMethod` a tres rectángulos idénticos. Observar la diferencia en las zonas fuera del rango.

4. **Texto con gradiente multicolor**: crear un título grande con `fill="url(#id)"` usando un gradiente de 4+ stops. Probar con `gradientUnits="objectBoundingBox"` (adapta al texto) y con `gradientUnits="userSpaceOnUse"` (posición fija).

5. **Overlay de sombra**: sobre un rectángulo de color sólido, superponer un segundo rectángulo con `fill="url(#shadow)"` donde el gradiente va de negro semitransparente (abajo) a transparente (arriba). Simular la sombra interior de una tarjeta.

6. **Herencia de gradiente con href**: definir un gradiente base con 3 stops de colores. Crear dos variantes heredando con `href`: una horizontal y una vertical. Aplicar las tres variantes a tres formas distintas y verificar que comparten los mismos colores.
