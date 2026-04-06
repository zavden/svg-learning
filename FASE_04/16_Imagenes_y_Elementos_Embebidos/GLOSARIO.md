# Glosario — Imágenes y elementos embebidos

---

**`<image>`** — Elemento SVG que incrusta una imagen raster o vectorial externa (PNG, JPEG, GIF, WebP, SVG). Requiere `x`, `y`, `width`, `height` y `href` explícitos.

**`href`** — Atributo que indica la ruta, URL o data URI de una imagen o recurso. Reemplazó a `xlink:href` en SVG 2.

**`xlink:href`** — Forma antigua de `href`, heredada de XLink. Todavía funcional por compatibilidad pero deprecada; los navegadores modernos aceptan ambos.

**`preserveAspectRatio`** — Atributo que controla cómo escalar una imagen (o un SVG con `viewBox`) dentro de su caja contenedora. Combina una alineación (`xMidYMid`, `xMinYMin`, etc.) con una política (`meet`, `slice`, `none`).

**`meet`** — Política de `preserveAspectRatio` que escala la imagen para que **quepa completa** dentro de la caja, dejando bordes vacíos si el ratio no coincide.

**`slice`** — Política de `preserveAspectRatio` que escala la imagen para que **cubra todo** el contenedor, recortando lo que sobresalga.

**`none`** — Política de `preserveAspectRatio` que **estira** la imagen para llenar la caja sin conservar proporciones.

**`<foreignObject>`** — Elemento SVG que permite incrustar contenido de otro namespace (típicamente HTML/XHTML) dentro de un SVG. Abre la puerta a texto multilínea, formularios y cualquier elemento del DOM HTML.

**Namespace XHTML** — `http://www.w3.org/1999/xhtml`. Debe declararse en el primer elemento HTML dentro de un `<foreignObject>` para que el navegador lo procese correctamente.

**SVG anidado** — Un elemento `<svg>` dentro de otro `<svg>`. Crea un **nuevo viewport** y, si tiene `viewBox`, un **nuevo sistema de coordenadas** aislado del padre.

**Viewport** — Región rectangular de dibujo visible de un SVG. El SVG raíz tiene uno, y cada SVG anidado crea uno adicional.

**Sistema de coordenadas de usuario** — El espacio numérico en el que se posicionan los elementos dentro de un SVG. Queda definido por el `viewBox` o, en su ausencia, por las dimensiones del viewport.

**`overflow`** — Propiedad que determina si el contenido que rebasa el viewport de un SVG anidado se muestra (`visible`) o se recorta (`hidden`, valor por defecto).

**Aislamiento de coordenadas** — Propiedad de los SVG anidados por la que los elementos internos usan su propio sistema numérico, independiente del tamaño del padre.

**Data URI** — Esquema de URI definido por RFC 2397 que permite embeber datos directamente en el atributo `href` (o `src`) en lugar de referenciar un archivo externo. Formato: `data:[mime-type][;base64],[datos]`.

**Base64** — Codificación binario-a-texto que representa datos arbitrarios usando 64 caracteres imprimibles. Añade aproximadamente un 33% de overhead al tamaño original.

**URL-encoding** — Codificación alternativa al base64, más eficiente para contenido textual como SVG. Reemplaza caracteres especiales por `%XX` donde XX es el código hexadecimal.

**MIME type** — Identificador estandarizado del tipo de contenido. En data URIs se usa para declarar el formato: `image/png`, `image/jpeg`, `image/svg+xml`, etc.

**CORS (Cross-Origin Resource Sharing)** — Política del navegador que restringe el acceso a recursos de otro origen. Afecta a imágenes externas cuando se procesan con canvas; los data URIs evitan este problema al no ser "externos".

**Viewport coincidente (`<svg>` raíz)** — El `<svg>` más externo cuyo viewport coincide con las dimensiones del contenedor HTML en el que vive.

**`<g>`** — Elemento de agrupación simple. A diferencia de `<svg>` anidado, no crea nuevo viewport ni sistema de coordenadas; solo permite aplicar transformaciones y estilos conjuntamente.

**Renderizado pixelado (`image-rendering`)** — Propiedad CSS que indica cómo suavizar (o no) una imagen raster escalada. `pixelated` desactiva el suavizado — útil para pixel art.
