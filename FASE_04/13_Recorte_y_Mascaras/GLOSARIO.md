# Glosario — Recorte y Máscaras

---

**`<clipPath>`**
Elemento SVG que define una región de recorte. Los elementos que lo referencian solo muestran las partes que caen dentro de la geometría definida. El recorte es binario: un píxel está visible o no, sin gradaciones intermedias.

---

**`clip-path` (atributo/propiedad)**
Forma de aplicar un recorte a un elemento. Como atributo SVG: `clip-path="url(#id)"`. Como propiedad CSS: además acepta funciones nativas como `circle()`, `polygon()`, `inset()`, `path()`.

---

**`<mask>`**
Elemento SVG que controla la visibilidad de cada píxel de un elemento usando la luminancia de su contenido. Permite transiciones suaves: blanco = visible, negro = oculto, gris = semitransparente.

---

**Recorte binario**
Tipo de recorte en el que cada píxel está completamente visible o completamente oculto, sin estados intermedios. Es el comportamiento de `<clipPath>`.

---

**Luminancia**
Cantidad de luz (brillo) de un color, calculada ponderando los canales RGB. En las máscaras SVG, la luminancia del contenido determina la opacidad del elemento enmascarado — por eso los gradientes de blanco a negro producen desvanecimientos.

---

**`clipPathUnits`**
Atributo de `<clipPath>` que indica en qué sistema de coordenadas se interpretan las formas internas. Valores: `userSpaceOnUse` (coordenadas absolutas, por defecto) u `objectBoundingBox` (coordenadas relativas 0-1 respecto al elemento recortado).

---

**`maskUnits`**
Atributo de `<mask>` que indica en qué sistema de coordenadas se interpretan sus `x`, `y`, `width` y `height` (el área activa de la máscara). Valores: `userSpaceOnUse` u `objectBoundingBox` (por defecto).

---

**`maskContentUnits`**
Atributo de `<mask>` que indica en qué sistema de coordenadas se interpretan las **formas dentro** de la máscara. Valores: `userSpaceOnUse` (por defecto) u `objectBoundingBox`.

---

**`userSpaceOnUse`**
Sistema de coordenadas que usa las mismas unidades que el SVG principal. Un `cx="50"` dentro de un `<clipPath>` con este modo se sitúa en la coordenada 50 del espacio del SVG.

---

**`objectBoundingBox`**
Sistema de coordenadas relativo al bounding box del elemento al que se aplica el clip o la máscara. Las coordenadas van de 0 a 1: `(0,0)` es la esquina superior izquierda del bbox, `(1,1)` la inferior derecha.

---

**Bounding box**
Rectángulo mínimo que encierra un elemento SVG. Se usa como referencia para unidades relativas (`objectBoundingBox`) y para posicionar efectos como máscaras y filtros.

---

**Unión de formas (en clipPath)**
Cuando un `<clipPath>` contiene varias formas, la región visible es la **unión** de todas ellas. No hay intersección ni resta directa — para eso hay que anidar clipPaths o usar un único `<path>` con subtrazados y `fill-rule`.

---

**Fondo de máscara**
Comportamiento por el que lo que no está cubierto por contenido dentro de una `<mask>` se considera negro (invisible). Por eso los ejemplos suelen empezar con un `<rect width="100%" height="100%" fill="white"/>` de base.

---

**Fade / desvanecimiento**
Transición suave de un elemento visible a invisible. Se consigue con una máscara cuyo contenido es un gradiente de blanco a negro. Imposible con `<clipPath>`.

---

**Viñeta**
Efecto fotográfico en el que los bordes se oscurecen o se vuelven transparentes, dejando el centro visible. Se consigue con una `<mask>` que contiene un `radialGradient` de blanco en el centro a negro en los bordes.

---

**`clip-path: path()`**
Función CSS que permite usar la sintaxis de un `<path>` SVG directamente como recorte: `clip-path: path('M 0 0 L 100 0 L 50 100 Z')`. Soporte amplio en navegadores modernos.

---

**`clip-path: url()`**
Función CSS que referencia un `<clipPath>` definido en un SVG o en el HTML. Permite usar la potencia completa de SVG (paths complejos, texto, múltiples formas) desde CSS.

---

**`fill-rule="evenodd"`**
Regla de relleno que permite crear agujeros en un `<path>` con múltiples subtrazados. Al usarlo dentro de un `<clipPath>`, permite definir regiones con "huecos" reales sin necesidad de anidación.
