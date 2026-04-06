# Glosario — SVG responsivo y accesibilidad

---

**SVG responsivo** — Un SVG que se adapta al tamaño de su contenedor sin perder calidad. Requiere al menos `viewBox` declarado y ausencia de `width`/`height` fijos en el elemento raíz.

**SVG adaptativo** — Un SVG que no solo escala sino que **cambia su contenido** según el tamaño disponible, ocultando o mostrando elementos mediante media queries internas.

**`viewBox`** — Atributo que define la caja lógica interna del SVG como `x y width height`. Clave para la responsividad: sin él, el navegador no puede calcular cómo escalar el contenido al redimensionar.

**Aspect ratio (proporción)** — Relación ancho/alto del viewBox. Un `viewBox="0 0 400 300"` define una proporción 4:3. El navegador la usa para calcular `height: auto`.

**Tamaño intrínseco** — El tamaño "preferido" del SVG, declarado con `width` y `height` en el elemento raíz. Sirve como fallback si el CSS no se aplica.

**`height: auto`** — Valor CSS que indica al navegador que calcule la altura manteniendo la proporción del viewBox. Solo funciona si hay `viewBox` declarado.

**`aspect-ratio`** — Propiedad CSS moderna que fuerza una proporción específica a un elemento, reemplazando el padding-hack tradicional. Soporte universal desde 2021.

**Padding-hack** — Técnica legado para mantener proporciones antes de `aspect-ratio`. Usa `padding-bottom` en porcentaje sobre un contenedor vacío con `height: 0`.

**`display: block`** — Declaración CSS que convierte el SVG en un bloque, eliminando el "espacio fantasma" reservado para descendentes tipográficos de los elementos inline.

**Media query interna** — Bloque `@media` dentro del `<style>` del propio SVG. Las condiciones se evalúan contra el viewport donde vive el SVG (ventana si inline, el propio SVG si externo).

**`prefers-color-scheme`** — Media query que detecta si el usuario prefiere modo claro u oscuro en el sistema operativo. Usado en SVG para adaptar paletas automáticamente.

**`currentColor`** — Valor especial de CSS que toma la propiedad `color` del elemento padre. En SVG permite que el ícono herede el color del texto del botón/enlace contenedor.

**`role="img"`** — Atributo ARIA que indica a tecnologías asistivas que el SVG debe ser tratado como una imagen unitaria. Necesario porque algunos lectores no reconocen `<svg>` como imagen por defecto.

**`<title>`** — Elemento hijo del SVG que proporciona un texto alternativo corto, equivalente al `alt` de `<img>`. Debe ser el primer hijo del elemento al que describe.

**`<desc>`** — Descripción larga del SVG, usada cuando el `<title>` no basta. Contiene datos concretos, tendencias o explicación estructural del contenido visual.

**`aria-labelledby`** — Atributo ARIA que conecta un elemento con uno o más ids que proporcionan su **nombre accesible**. En SVG se usa para vincular el contenido visual con `<title>` y `<desc>`.

**`aria-describedby`** — Atributo ARIA que conecta con una **descripción adicional** (no el nombre). Se anuncia después del nombre del elemento.

**`aria-hidden="true"`** — Atributo que oculta completamente un elemento de las tecnologías asistivas. Se usa en SVGs decorativos o cuando el contenido ya está disponible por otra vía.

**`focusable="false"`** — Atributo que impide que el SVG reciba foco por teclado. Relevante en IE/Edge legacy donde los SVGs podían ser focalizables por defecto.

**`.sr-only` (screen reader only)** — Convención de clase CSS para ocultar visualmente un elemento manteniéndolo disponible para lectores de pantalla. Típicamente: `position:absolute; width:1px; height:1px; overflow:hidden; clip:rect(0,0,0,0);`

**Nombre accesible** — El texto que un lector de pantalla anuncia al encontrar un elemento. En SVG se determina por `aria-label`, `aria-labelledby`, o el `<title>` interno.

**Alto contraste** — Modo del sistema operativo (especialmente Windows) que sobreescribe colores para maximizar la legibilidad. Afecta a los colores CSS pero no siempre a los atributos SVG fijos.

**Variables CSS (custom properties)** — Mecanismo `--nombre: valor` que permite definir valores reutilizables y cambiarlos dinámicamente. Dentro de un SVG inline permiten tematización centralizada heredada del documento HTML.
