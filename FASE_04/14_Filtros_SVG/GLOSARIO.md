# Glosario — Filtros SVG

---

**`<filter>`**
Elemento SVG que define un pipeline de procesamiento de imagen. Se declara siempre dentro de `<defs>` y se aplica a un elemento o grupo mediante `filter="url(#id)"` o la propiedad CSS `filter: url(#id)`. Internamente rasteriza el elemento a pixels y aplica una cadena de primitivas sobre esos pixels.

---

**Primitiva de filtro (`fe...`)**
Cada una de las operaciones atómicas que componen un filtro. Todas tienen el prefijo `fe` (*filter effect*): `feGaussianBlur`, `feOffset`, `feFlood`, `feComposite`, `feMerge`, `feColorMatrix`, `feMorphology`, etc. Reciben una imagen de entrada (`in`) y producen una imagen de salida.

---

**Pipeline / cadena de filtro**
Secuencia de primitivas ejecutadas en orden. La salida de una primitiva (`result="nombre"`) puede ser la entrada (`in="nombre"`) de otra posterior. Si una primitiva no especifica `in`, toma la salida de la primitiva anterior por defecto.

---

**Rasterización**
Proceso por el que el elemento vectorial se convierte en un bitmap (imagen de pixels) antes de que el filtro lo procese. Es la razón por la que los filtros son costosos: operan sobre pixels, no sobre paths.

---

**Región del filtro**
Área rectangular definida por los atributos `x`, `y`, `width`, `height` del `<filter>`. Indica dónde se procesa el filtro. Por defecto `-10% -10% 120% 120%` (10% de margen extra para que blurs y sombras no se recorten). Si el efecto se sale, queda cortado.

---

**`filterUnits`**
Atributo de `<filter>` que define el sistema de coordenadas de `x`, `y`, `width`, `height` del propio filtro. Valores: `objectBoundingBox` (por defecto, relativo al bounding box del elemento) o `userSpaceOnUse` (coordenadas absolutas).

---

**`primitiveUnits`**
Atributo de `<filter>` que define el sistema de coordenadas de las primitivas internas (por ejemplo los `dx`/`dy` de un `feOffset`). Valores: `userSpaceOnUse` (por defecto) o `objectBoundingBox`.

---

**`in` (atributo)**
Indica qué imagen recibe como entrada una primitiva. Puede ser un valor predefinido (`SourceGraphic`, `SourceAlpha`…) o el nombre de un `result` definido por otra primitiva anterior. Si se omite, se toma la salida de la primitiva inmediatamente anterior.

---

**`result` (atributo)**
Nombra la salida de una primitiva para poder referenciarla después con `in="nombre"`. El nombre es local al `<filter>`, no necesita ser un id global.

---

**`SourceGraphic`**
Entrada predefinida que representa el elemento completo tal como se vería sin filtro (con su fill, stroke, colores). Es el valor por defecto cuando se omite `in` en la primera primitiva de un filtro.

---

**`SourceAlpha`**
Entrada predefinida que representa solo el canal alfa del elemento — la silueta en negro sin colores. Es la base ideal para construir sombras proyectadas: la sombra no hereda los colores del elemento.

---

**`FillPaint` / `StrokePaint`**
Entradas predefinidas que representan el color (o gradiente, o patrón) del fill o del stroke del elemento como una imagen plena, independientemente de la geometría. Útiles para separar el color del elemento de su forma.

---

**`feGaussianBlur`**
Primitiva que aplica un desenfoque gaussiano. Su atributo clave es `stdDeviation`, que controla la cantidad de blur (un valor o dos separados por espacio para X e Y). Es la primitiva más costosa computacionalmente.

---

**`feOffset`**
Primitiva que desplaza la imagen de entrada sin modificarla. `dx` desplaza en horizontal (positivo = derecha), `dy` en vertical (positivo = abajo). Aporta la dirección a una sombra proyectada.

---

**`feFlood`**
Primitiva que genera una imagen de un color sólido (`flood-color` y `flood-opacity`) que llena toda la región del filtro. Normalmente se combina con `feComposite operator="in"` para teñir la silueta del elemento.

---

**`feComposite`**
Primitiva que combina dos imágenes (`in` + `in2`) según un operador de Porter-Duff: `over`, `in`, `out`, `atop`, `xor`, `arithmetic`. Muy usada para recortar una imagen a la silueta de otra.

---

**`feMerge` / `feMergeNode`**
Primitivas que apilan múltiples imágenes en capas. El primer `feMergeNode` queda abajo y cada uno siguiente se dibuja encima. Equivale a un "grupo de capas" en un editor gráfico.

---

**`feColorMatrix`**
Primitiva que transforma los colores del elemento. `type="saturate"` ajusta saturación (0 = grises), `type="hueRotate"` rota el tono en grados, `type="matrix"` acepta una matriz 4×5 completa para cualquier transformación, `type="luminanceToAlpha"` convierte brillo en canal alfa.

---

**`feDropShadow`**
Primitiva de atajo (SVG 2) que genera una sombra proyectada en un solo paso. Equivale internamente a la cadena `feGaussianBlur` + `feOffset` + `feFlood` + `feComposite` + `feMerge`. Parámetros: `dx`, `dy`, `stdDeviation`, `flood-color`, `flood-opacity`.

---

**`feMorphology`**
Primitiva que expande o contrae la silueta del elemento. `operator="dilate"` engorda los bordes, `operator="erode"` los adelgaza. Útil para crear contornos exteriores sin modificar el path original.

---

**`feTurbulence`**
Primitiva que genera ruido procesal (Perlin o turbulencia fractal) sin necesidad de imagen de entrada. Con `baseFrequency` y `numOctaves` permite crear texturas de agua, nubes, papel, fuego, etc.

---

**`feDisplacementMap`**
Primitiva que desplaza los pixels de una imagen usando otra imagen como "mapa". Combinada con `feTurbulence` produce efectos de distorsión orgánica (cristal, agua, calor).

---

**`feDiffuseLighting` / `feSpecularLighting`**
Primitivas de iluminación 3D que usan el canal alfa como mapa de relieve (*bump map*). Requieren una fuente de luz hija (`feDistantLight`, `fePointLight`, `feSpotLight`). Simulan luz difusa o brillos especulares para dar aspecto tridimensional a formas planas.

---

**Drop shadow (sombra proyectada)**
Efecto que proyecta una copia desenfocada y desplazada del elemento detrás de él. En SVG puede construirse manualmente (cadena de 5 primitivas) o con `feDropShadow`. En CSS: `filter: drop-shadow(x y blur color)`.

---

**`filter: drop-shadow()` (CSS)**
Función CSS que aplica una sombra proyectada siguiendo el contorno real del elemento (a diferencia de `box-shadow`, que sigue el bounding box rectangular). No requiere definición SVG previa.
