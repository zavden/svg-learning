# Glosario — Agrupación y Reutilización

---

**`<g>`**
Elemento contenedor (group) sin representación visual propia. Agrupa elementos SVG como una unidad lógica, propaga atributos de presentación heredables a sus hijos y permite aplicar un `transform`, `filter`, `clip-path` o `mask` a todo el conjunto.

---

**`<use>`**
Instancia visual de un elemento ya definido en el documento. Crea una copia del elemento referenciado por `href` o `xlink:href`, sin duplicar el código fuente. Cambios en el original se reflejan en todas las instancias.

---

**`<symbol>`**
Plantilla para contenido reutilizable. Como `<g>` pero con su propio `viewBox` y `preserveAspectRatio`, lo que permite que cada instancia `<use>` se escale al `width`/`height` indicado. Nunca se renderiza directamente, solo a través de `<use>`.

---

**`<defs>`**
Contenedor para definiciones que no se renderizan directamente: gradientes, patrones, filtros, clipPaths, masks, symbols, markers y grupos referenciados por `<use>`. Actúa como una "biblioteca" del SVG.

---

**`href` / `xlink:href`**
Atributo de `<use>` (y otros elementos de referencia) que apunta al `id` del elemento a instanciar, con la sintaxis `#id`. `href` es el estándar SVG 2; `xlink:href` es la forma antigua de SVG 1.1 y requiere declarar el namespace `xmlns:xlink`.

---

**Shadow DOM de `<use>`**
Copia interna y oculta del contenido referenciado por un `<use>`. Sus elementos no son accesibles desde CSS externo (un selector como `use circle` no penetra el shadow DOM). Los estilos que sí entran son los heredados (`fill`, `stroke`, etc.) y `currentColor` o variables CSS.

---

**Atributo de presentación**
Atributo XML que expresa una propiedad CSS, como `fill="red"` o `stroke-width="2"`. Equivale a una regla CSS con muy baja especificidad: se puede sobrescribir con cualquier regla CSS (incluso de tipo o clase).

---

**Atributo heredable**
Atributo de presentación cuyo valor se propaga automáticamente a los hijos de un elemento. Incluye `fill`, `stroke`, `stroke-*`, `font-family`, `font-size`, `color`, `visibility`, `cursor`, entre otros.

---

**Atributo no heredable**
Atributo que aplica solo al elemento en el que se declara y no se propaga a los hijos. Incluye los atributos geométricos (`x`, `y`, `width`, `height`, `cx`, `cy`, `r`), `opacity`, `transform`, `clip-path`, `mask`, `filter`, `display`, `overflow`.

---

**`currentColor`**
Palabra clave CSS que equivale al valor actual de la propiedad `color`. Usar `fill="currentColor"` dentro de un `<symbol>` permite que el color lo fije el `<use>` o un ancestro mediante la propiedad CSS `color`.

---

**Sprite SVG**
Archivo o bloque SVG que contiene múltiples `<symbol>` — uno por icono — con IDs únicos. Se utiliza desde el HTML con `<use href="sprite.svg#id-icon">`, centralizando todos los iconos en un único lugar.

---

**Cascada (en SVG)**
Orden de prioridad con el que se resuelve el valor final de una propiedad. De menor a mayor: valor inicial del navegador → herencia del padre → atributo de presentación → regla CSS → `style` inline → `!important`.

---

**`preserveAspectRatio`**
Atributo de `<svg>` y `<symbol>` que controla cómo se ajusta el contenido al `viewBox` cuando las proporciones del contenedor no coinciden con las del contenido. `xMidYMid meet` (por defecto) preserva el aspecto; `none` estira para rellenar.

---

**Herencia en cadena**
Mecanismo por el que un nieto recibe un atributo desde el abuelo a través de la jerarquía de padres, siempre que ningún nivel intermedio lo sobrescriba. Permite definir una paleta en el `<svg>` raíz y que se propague a todos los elementos.

---

**`<marker>`**
Elemento que se define dentro de `<defs>` y se referencia desde atributos `marker-start`, `marker-mid`, `marker-end` de líneas, polilíneas o paths. No es parte estricta de este tema pero vive conceptualmente en la misma categoría de "definiciones reutilizables".

---

**Jerarquía semántica**
Uso de `<g>` con `id` o `class` descriptivos para estructurar lógicamente el SVG (`<g id="cabeza">`, `<g id="torso">`). Hace el código legible, facilita la manipulación desde JavaScript y mejora la accesibilidad.
