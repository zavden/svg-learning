# Glosario — Gradientes

---

**`<linearGradient>`**
Elemento SVG que define una transición de colores a lo largo de una línea recta. Se coloca en `<defs>` y se referencia con `fill="url(#id)"` o `stroke="url(#id)"`. La dirección se controla con `x1`, `y1`, `x2`, `y2`.

---

**`<radialGradient>`**
Elemento SVG que define una transición de colores que irradia desde un punto central hacia el exterior. Produce efectos de esfera, luz y resplandor. Controlado por `cx`, `cy`, `r` (círculo exterior) y `fx`, `fy` (punto focal).

---

**`<stop>`**
Hijo de `linearGradient` o `radialGradient`. Define un punto de color en el gradiente. Atributos: `offset` (posición 0–1 o 0%–100%), `stop-color` (color), `stop-opacity` (opacidad). El gradiente interpola suavemente entre stops consecutivos.

---

**offset**
Atributo de `<stop>` que define la posición del punto de color dentro del gradiente. `0` o `0%` = inicio, `1` o `100%` = final. Dos stops en el mismo offset crean un cambio abrupto de color.

---

**stop-color**
Color del `<stop>` en su posición. Acepta todos los formatos CSS. Si se omite: negro por defecto.

---

**stop-opacity**
Opacidad del `<stop>` en su posición, de `0` a `1`. Permite gradientes que van de un color opaco a transparente. Si se omite: `1` (opaco) por defecto.

---

**x1, y1, x2, y2**
Atributos de `<linearGradient>` que definen los puntos de inicio y fin de la línea del gradiente. Con `gradientUnits="objectBoundingBox"` (por defecto), se expresan como fracciones (0–1) o porcentajes (0%–100%) del bounding box del elemento.

---

**cx, cy, r**
Atributos de `<radialGradient>`. `cx` y `cy` definen el centro del círculo exterior (donde termina el último stop). `r` es el radio de ese círculo. Por defecto: `50%, 50%, 50%`.

---

**fx, fy**
Atributos de `<radialGradient>` que definen el punto focal: el punto desde el que irradia el gradiente. Por defecto coinciden con `cx`, `cy`. Al desplazarlos se crea efecto de luz lateral (highlight de esfera).

---

**gradientUnits**
Controla el sistema de coordenadas de los atributos de posición del gradiente. `objectBoundingBox` (por defecto): relativo al bounding box del elemento que usa el gradiente (0–1). `userSpaceOnUse`: coordenadas absolutas del viewport. Usar `userSpaceOnUse` para `<line>` (bounding box degenerado) o para gradientes continuos entre múltiples formas.

---

**spreadMethod**
Define el comportamiento del gradiente más allá de sus límites. `pad` (default): extiende el color del borde. `reflect`: repite en espejo (sin saltos). `repeat`: reinicia desde el inicio (salto abrupto si el primer y último color son distintos).

---

**gradientTransform**
Aplica una transformación SVG al gradiente completo. El uso más común: `rotate(deg, 0.5, 0.5)` para obtener un gradiente en un ángulo arbitrario sin calcular coordenadas trigonométricas. El punto `(0.5, 0.5)` es el centro del bounding box.

---

**href**
Atributo que permite que un gradiente herede los stops y atributos de otro. Permite crear variantes (distinta dirección, distinto spreadMethod) sin repetir los stops. Sintaxis: `href="#idDelGradienteBase"`.

---

**url(#id)**
Función CSS/SVG que referencia un elemento por su `id`. Usada como valor de `fill` o `stroke` para aplicar un gradiente definido en `<defs>`. Sintaxis: `fill="url(#miGradiente)"`.

---

**bounding box**
Rectángulo mínimo que encierra un elemento SVG. Con `gradientUnits="objectBoundingBox"`, las coordenadas `0,0` corresponden a la esquina superior izquierda del bounding box y `1,1` a la inferior derecha. Los gradientes se adaptan automáticamente al tamaño de cada elemento.
