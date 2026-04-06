# Glosario — Marcadores

---

**`<marker>`**
Elemento SVG que define un gráfico pequeño reutilizable para colocarlo automáticamente en los extremos o vértices de una línea, polilínea o path. Se declara dentro de `<defs>` con un `id` único y se instancia mediante los atributos `marker-start`, `marker-mid` o `marker-end` del elemento que lo usa.

---

**`marker-start`**
Atributo que indica qué marker colocar en el **primer punto** del path (el punto inicial tras el primer comando `M`). Valor: `url(#id)` o `none`.

---

**`marker-mid`**
Atributo que indica qué marker colocar en **cada vértice intermedio** del path (ni el primero ni el último). Los markers aparecen en los anclas explícitas del path, no a lo largo de las curvas.

---

**`marker-end`**
Atributo que indica qué marker colocar en el **último punto** del path. Es el más común para las cabezas de flecha.

---

**`marker` (atributo abreviado)**
Atajo que aplica el mismo marker a las tres posiciones (`start`, `mid`, `end`) a la vez. Útil cuando quieres el mismo símbolo en todos los puntos.

---

**`markerWidth` / `markerHeight`**
Dimensiones del **viewport** del marker — el área donde se dibuja su contenido. Por defecto `3` × `3`. Si `markerUnits="strokeWidth"`, estas dimensiones se multiplican por el `stroke-width` del elemento padre.

---

**`refX` / `refY`**
El **punto de anclaje** del marker: el punto del marker que se coloca exactamente en el vértice del path. Se expresa en las coordenadas del `viewBox` del marker. Por defecto `(0, 0)` (esquina superior izquierda). Para una flecha apuntando a la derecha, `refX` suele ser el extremo derecho del viewBox.

---

**`viewBox` del marker**
Sistema de coordenadas interno del marker, idéntico al `viewBox` del `<svg>`. Permite definir el contenido del marker en cualquier escala y que se ajuste al viewport (`markerWidth` × `markerHeight`) automáticamente.

---

**`orient`**
Atributo que controla la rotación del marker al colocarse en el path:
- `auto`: el marker rota para alinearse con la tangente de la línea en ese punto.
- `auto-start-reverse`: como `auto`, pero el `marker-start` se rota 180° adicionales (imprescindible para flechas bidireccionales).
- Un ángulo fijo en grados (ej. `90`): el marker siempre tiene esa orientación, independiente de la dirección.

---

**`auto-start-reverse`**
Valor de `orient` que permite usar **un solo marker** para flechas en ambos extremos de una línea. El `marker-end` se alinea con la tangente y el `marker-start` se voltea automáticamente 180° para apuntar hacia afuera.

---

**`markerUnits`**
Atributo que determina el sistema de unidades del viewport del marker:
- `strokeWidth` (por defecto): `markerWidth` y `markerHeight` se multiplican por el `stroke-width` del elemento padre. El marker escala con el grosor de la línea.
- `userSpaceOnUse`: dimensiones absolutas en el espacio del usuario. El marker tiene tamaño fijo independientemente del stroke.

---

**Tangente**
Dirección de la línea en un punto específico. `orient="auto"` alinea el marker con la tangente local del path en el vértice donde se coloca.

---

**Punto de anclaje**
El punto del marker que "toca" el vértice del path. Se define con `refX` y `refY`. Si es incorrecto, el marker aparece desplazado o el trazo del path asoma por delante del marker.

---

**`context-stroke`**
Valor especial de `fill` o `stroke` (SVG 2) que toma el color del stroke del elemento padre. Permite que un marker (por ejemplo, una punta de flecha) herede automáticamente el color de la línea a la que está asociado.

---

**`context-fill`**
Valor equivalente a `context-stroke` pero para el fill: el marker hereda el fill del elemento padre en lugar de su stroke.

---

**`currentColor`**
Palabra clave CSS/SVG que referencia el valor actual de la propiedad CSS `color` del elemento. Alternativa a `context-stroke` compatible con SVG 1.1: si un elemento dentro del marker usa `fill="currentColor"`, hereda la `color` CSS del elemento padre.

---

**Vértice explícito**
Un punto del path que corresponde a un ancla de los comandos de dibujo (`L`, `M`, `C`, `Q`, `T`, `S`, etc.). Los markers solo aparecen en estos puntos, nunca en posiciones interpoladas a lo largo de una curva.

---

**Cabeza de flecha**
Marker típico en forma de triángulo o `path` similar, colocado en un extremo de una línea para indicar dirección. El diseño habitual tiene el pico del triángulo alineado con `refX = markerWidth` para que toque exactamente el vértice del path.

---

**Flecha bidireccional**
Línea que muestra dirección en ambos extremos. Se puede implementar con dos markers distintos o, más elegante, con un único marker usando `orient="auto-start-reverse"`.

---

**Crow's foot notation**
Notación gráfica usada en diagramas entidad-relación donde distintos "pies" (líneas múltiples divergentes, círculos, barras) indican la cardinalidad de la relación. Cada variante se implementa como un marker distinto en SVG.

---

**`getPointAtLength()`**
Método de la interfaz `SVGGeometryElement` de JavaScript que devuelve el punto `(x, y)` a una distancia dada a lo largo de un path. Es la forma estándar de colocar símbolos a intervalos uniformes sobre una curva, ya que `marker-mid` solo aparece en vértices explícitos.
