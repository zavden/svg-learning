# Ideas de desafíos — Transformaciones

---

## 1. Icono reposicionable con `translate`

Diseñar un icono (flecha, casa, estrella) con sus coordenadas internas centradas en `(0,0)`, dentro de un `<g>`. Luego colocarlo en cuatro posiciones distintas del canvas usando solo `translate` en el `<g>`, sin tocar las coordenadas internas. Demostrar que cambiar el `translate` es suficiente para reposicionar todo el grupo.

---

## 2. Rueda de marcas tipo reloj con `rotate`

Crear 12 marcas de hora (rectángulos alargados) distribuidas en círculo usando `rotate(n * 30, cx, cy)` para `n` de 0 a 11. Diferenciar visualmente las marcas de los cuartos (12, 3, 6, 9) con un color o tamaño distinto. Opcionalmente añadir agujas con rotaciones adicionales.

---

## 3. Pétalo reflejado con `scale(-1,1)`

Dibujar un pétalo asimétrico (una forma con `<path>`) y crear su espejo usando `scale(-1, 1)` con el patrón `translate(2*cx, 0) scale(-1, 1)`. Combinar el original y el reflejo para formar una figura simétrica (flor, mariposa, hoja doble). Un solo path, dos instancias con transformación.

---

## 4. Sombra en perspectiva con `skewX`

Tomar un elemento (texto o forma geométrica) y crear debajo una versión "sombra proyectada" aplicando `skewX(-30)` más una escala vertical reducida y un color oscuro semitransparente. Ajustar el ángulo del skew para que la sombra apunte en la dirección correcta respecto a una fuente de luz imaginaria.

---

## 5. Demostración del orden de encadenamiento

Crear dos copias idénticas de un rectángulo pequeño y aplicarles las mismas dos transformaciones pero en orden inverso:
- Copia A: `translate(80, 0) rotate(45)`
- Copia B: `rotate(45) translate(80, 0)`

Mostrar ambas con etiquetas y líneas guía que expliquen visualmente por qué las posiciones finales son distintas a pesar de usar los mismos valores.

---

## 6. Panel de tarjetas con `matrix`

Recrear una rejilla de 4–6 tarjetas usando únicamente `matrix(a, b, c, d, e, f)` para posicionar y rotar cada tarjeta (sin usar `translate` ni `rotate` por separado). Calcular los parámetros de la matriz para cada posición y ángulo deseado. Incluir un comentario en el SVG con el `rotate + translate` equivalente a cada matriz, para verificar que los resultados coinciden.
