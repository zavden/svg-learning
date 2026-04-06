# GLOSARIO — Interactividad con JavaScript

---

**`SVGElement`** — Clase base de la que heredan todos los elementos SVG. Hereda a su vez de `Element`, `Node` y `EventTarget`, por lo que acepta `addEventListener`, `querySelector`, `classList`, etc. No hereda de `HTMLElement`.

**`SVGCircleElement`, `SVGPathElement`, `SVGRectElement`…** — Interfaces DOM específicas de cada tipo de elemento SVG. Cada una añade propiedades tipadas particulares: `SVGPathElement.getTotalLength()`, `SVGCircleElement.cx.baseVal.value`, etc.

**`createElementNS(ns, tag)`** — Método obligatorio para crear elementos SVG desde JavaScript. El namespace SVG es `"http://www.w3.org/2000/svg"`. Usar `createElement` (sin NS) produce un `HTMLUnknownElement` invisible.

**Namespace SVG** — Identificador que distingue elementos SVG de HTML en el DOM. Literal: `http://www.w3.org/2000/svg`. Es una URL histórica, no se conecta a nada.

**`getAttribute` / `setAttribute`** — Métodos DOM genéricos para leer y escribir atributos. Funcionan siempre, pero devuelven/aceptan strings — hay que hacer `parseFloat()` si se quiere aritmética.

**`SVGAnimatedLength`** — Wrapper tipado para atributos SVG numéricos. Contiene dos subpropiedades: `baseVal` (el valor declarado en el atributo) y `animVal` (el valor actual incluyendo animaciones SMIL activas). Se accede como `circle.cx.baseVal.value`.

**`baseVal` vs `animVal`** — `baseVal` es el valor "estático" definido en el atributo; `animVal` es el valor "vivo" mientras una animación SMIL está corriendo. Útil para inspeccionar animaciones en curso.

**`getBBox()`** — Devuelve un `SVGRect` con la caja envolvente del elemento en sus **coordenadas locales** (sin considerar transformaciones del padre). Útil para centrar, medir o posicionar otros elementos respecto al original.

**`getCTM()`** — *Current Transformation Matrix*: la matriz acumulada de todas las transformaciones aplicadas al elemento dentro del SVG (pero sin contar el layout del documento).

**`getScreenCTM()`** — Como `getCTM()`, pero con todas las transformaciones añadidas hasta llegar a **coordenadas de pantalla** (pixels CSS). Es la matriz clave para convertir eventos del mouse a coordenadas SVG.

**`DOMPoint`** — Interfaz moderna para representar un punto en coordenadas. Tiene un método `matrixTransform(matriz)` que aplica una transformación y devuelve un nuevo punto. Sustituye a `createSVGPoint()`.

**`matrixTransform(m)`** — Aplica una matriz a un punto y devuelve el resultado. Usado para convertir coordenadas de pantalla a coordenadas SVG: `pt.matrixTransform(svg.getScreenCTM().inverse())`.

**`createSVGPoint()` / `createSVGMatrix()`** — Fábricas del elemento `<svg>` raíz para crear puntos y matrices compatibles con las APIs SVG. Equivalentes más antiguos de `DOMPoint` y `DOMMatrix`; siguen funcionando.

**Pointer Events** — API moderna (`pointerdown`, `pointermove`, `pointerup`, `pointercancel`) que unifica mouse, touch y stylus en un solo conjunto de eventos. Recomendada sobre los eventos `mouse*` clásicos.

**`setPointerCapture(pointerId)`** — Fija un puntero a un elemento específico: todos los eventos de ese puntero van a ese elemento aunque el cursor salga de él. Imprescindible para drag and drop fiable.

**`hasPointerCapture` / `releasePointerCapture`** — Complementos de `setPointerCapture`. El navegador libera la captura automáticamente en `pointerup`, pero puedes forzarla antes.

**`pointerId`** — Identificador único de cada puntero activo. Distinto para cada dedo en multi-touch y para el mouse. Clave para manejar interacción simultánea.

**`pointer-events` (propiedad CSS/atributo SVG)** — Controla qué partes del elemento responden a eventos. Valores útiles: `none` (el elemento es invisible al mouse), `all` (responde en toda su área), `visiblePainted` (default, fill o stroke visibles).

**Delegación de eventos** — Patrón de poner un único listener en un elemento padre y usar `e.target` + `.closest()` o `.matches()` para distinguir qué hijo recibió el evento. Útil para SVGs con muchos elementos.

**`e.target` vs `e.currentTarget`** — `target` es el elemento que originó el evento; `currentTarget` es el que tiene el listener. Durante la burbuja, `target` no cambia pero `currentTarget` sí.

**`getTotalLength()`** — Método de `SVGGeometryElement` (path, circle, line, rect…). Devuelve la longitud total de la geometría en unidades del viewBox. Base para el efecto de line drawing.

**`getPointAtLength(d)`** — Devuelve un `DOMPoint` con las coordenadas del punto a distancia `d` desde el inicio del path. Útil para animar objetos a lo largo de una trayectoria con JavaScript.

**`isPointInFill(pt)` / `isPointInStroke(pt)`** — Tests geométricos: devuelven `true` si el punto está dentro del área rellena o del trazo. Alternativa precisa a `getBBox` para hit testing en formas no rectangulares.

**`<script>` SVG** — Elemento que permite incluir JavaScript dentro de un archivo `.svg`. En un archivo XML hay que envolver el código en `<![CDATA[ ... ]]>` para que `<` y `&` no rompan el parser; en SVG inline en HTML5 no es necesario.

**CDATA (`<![CDATA[...]]>`)** — Sección XML que marca su contenido como texto literal. El parser XML no interpreta `<`, `>` o `&` dentro, permitiendo escribir JavaScript sin escapar caracteres.

**Sanitización SVG** — Proceso de eliminar `<script>`, atributos de evento (`onclick`, `onload`…) y otros vectores XSS de un SVG antes de incrustarlo inline en una página. Librería habitual: DOMPurify.

**`querySelector` / `querySelectorAll`** — Los mismos métodos de selección CSS que en HTML funcionan en SVG, incluyendo selectores de atributo (`[data-id]`), clase (`.activo`) e id.

**`classList`** — Propiedad con métodos `add`, `remove`, `toggle`, `contains`, `replace`. Forma idiomática de manipular clases en SVG — prefiérela sobre `setAttribute('class', ...)`.
