# Glosario — Patrones

---

**`<pattern>`**
Elemento SVG que define una celda gráfica que se repite para rellenar una forma. Se define en `<defs>` y se referencia con `fill="url(#id)"`. Equivalente SVG de `background-image: repeat` en CSS.

---

**celda (tile)**
La unidad mínima del patrón que se repite. Definida por los atributos `width` y `height`. El navegador tesela la celda (la repite en cuadrícula) hasta cubrir el área de la forma. El contenido que supere los bordes de la celda se recorta.

---

**tesela / teselar**
Rellenar un área sin huecos ni solapamientos con la repetición de una unidad. Los patrones SVG teselan la celda en horizontal y vertical hasta cubrir el bounding box del elemento.

---

**`width` y `height` (de `<pattern>`)**
Dimensiones de la celda repetida. Son obligatorios: sin ellos el patrón no produce ningún output. En `patternUnits="userSpaceOnUse"`, son unidades absolutas. En `patternUnits="objectBoundingBox"`, son fracciones del bounding box (0–1).

---

**`patternUnits`**
Controla el sistema de coordenadas de los atributos de posición y tamaño de la celda (`x`, `y`, `width`, `height`). `objectBoundingBox` (default): relativo al elemento que usa el patrón. `userSpaceOnUse`: coordenadas absolutas del viewport. La diferencia clave: con `objectBoundingBox`, el número de celdas es constante; con `userSpaceOnUse`, el tamaño de las celdas es constante.

---

**`patternContentUnits`**
Controla el sistema de coordenadas de los **elementos dentro** del patrón. Independiente de `patternUnits`. `userSpaceOnUse` (default): coordenadas absolutas. `objectBoundingBox`: relativas al elemento que usa el patrón. La combinación más práctica: `patternUnits="userSpaceOnUse"` + `patternContentUnits="userSpaceOnUse"`, con todo en unidades absolutas.

---

**`patternTransform`**
Transforma el patrón completo antes de usarlo: `rotate(deg)`, `scale(n)`, `translate(x,y)`, etc. Permite crear rayas diagonales (`rotate(45)`) o ajustar la densidad (`scale`) sin cambiar la definición de la celda.

---

**`x`, `y` (de `<pattern>`)**
Desplazan el punto de inicio de la tesela. Por defecto `0,0`. Con un valor distinto de cero, la primera celda empieza desplazada respecto al borde de la forma.

---

**`viewBox` (en `<pattern>`)**
Sistema de coordenadas interno del contenido del patrón. Permite diseñar el contenido con coordenadas normales (ej: espacio de 100×100) aunque la celda tenga dimensiones distintas. El contenido se escala para caber en la celda, igual que el `viewBox` del elemento `<svg>`.

---

**`url(#id)`**
Función SVG/CSS que referencia un elemento por su `id`. Usado como valor de `fill` o `stroke` para aplicar un patrón: `fill="url(#miPatron)"`. El patrón tesela el interior de la forma.

---

**patrón de rayas**
Celda rectangular donde un rectángulo ocupa la mitad del ancho (rayas verticales) o del alto (rayas horizontales). La repetición de la celda crea el efecto de rayas. Con `patternTransform="rotate(45)"` se obtienen rayas diagonales.

---

**polka dots**
Patrón de puntos en cuadrícula. Celda cuadrada con un círculo centrado (`cx = width/2`, `cy = height/2`). El radio determina el tamaño del punto y, combinado con el tamaño de la celda, el espacio entre puntos.

---

**patrón de cuadrícula**
Celda cuadrada con solo el borde derecho y el borde inferior pintados. Al repetirse, los bordes se unen y forman una cuadrícula completa. Técnica eficiente: 2 líneas por celda en lugar de 4.
