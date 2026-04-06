# Glosario — Integración de SVG en la web

---

**SVG inline**
SVG escrito directamente dentro del HTML como etiquetas `<svg>`. El navegador lo parsea como parte del DOM del documento. Permite CSS externo, JavaScript global y acceso total a los elementos. Es la forma más flexible, pero aumenta el peso del HTML y no se cachea por separado.

**`<img>` con SVG**
Método que trata al SVG como una imagen estática. El SVG vive en un archivo externo referenciado por `src`. Hereda todo el ecosistema de imágenes (lazy loading, `srcset`, cache, `alt`), pero queda **sandboxed**: scripts y CSS externo no aplican.

**Sandboxing**
Aislamiento del SVG respecto al documento padre cuando se carga con `<img>`. Las reglas CSS del padre no alcanzan al interior del SVG y sus scripts no se ejecutan. Es una decisión de seguridad para prevenir XSS a través de imágenes.

**`background-image` con SVG**
Uso del SVG como fondo vía CSS (`background-image: url('x.svg')`). Compatible con `repeat`, `position`, `size`. El SVG queda **completamente invisible** para lectores de pantalla, por lo que solo debe usarse en decoración pura.

**`<object>`**
Elemento HTML que incrusta un recurso externo. Para SVG ofrece dos cosas que `<img>` no: **contenido fallback** (se muestra si el SVG no carga) y **ejecución de scripts internos** en su propio contexto. Su API `getSVGDocument()` da acceso al DOM del SVG desde el padre.

**`getSVGDocument()`**
Método del elemento `<object>` que devuelve el `Document` del SVG cargado. Permite al JavaScript del padre leer y modificar el SVG interno. Solo funciona tras el evento `load` y respetando la política del mismo origen.

**`<embed>`**
Elemento legado para incrustar recursos externos. Originalmente pensado para plugins (Flash, Java). Sin fallback, sin acceso DOM limpio, comportamiento inconsistente entre navegadores. **No se recomienda** para SVG en proyectos nuevos.

**`<iframe>`**
Elemento que incrusta un documento completo con su propio contexto de navegación: `window`, `document`, historial, proceso. Es el método más pesado pero ofrece **aislamiento total**. Apropiado para SVGs grandes con su propio JavaScript o procedentes de otros dominios.

**Data URI**
Esquema `data:mime/tipo,contenido` que incrusta el contenido directamente en la URL. Para SVG: `data:image/svg+xml,...` con URL-encoding o `data:image/svg+xml;base64,...`. Evita una petición HTTP a costa de romper la cache independiente.

**URL-encoding (percent-encoding)**
Codificación que reemplaza caracteres especiales por `%XX` donde `XX` es el código hexadecimal. Para SVG en data URI hay que escapar al menos `<` → `%3C`, `>` → `%3E`, `#` → `%23`, `"` → `%22`, `%` → `%25`.

**Base64**
Codificación binaria a texto usando 64 caracteres imprimibles. Útil para PNG/JPEG, pero para SVG (que ya es texto) infla el tamaño ~33% y pierde legibilidad. Solo vale la pena si el SVG contiene muchos caracteres especiales.

**`currentColor`**
Valor CSS/SVG que referencia el `color` heredado del elemento padre. Al usar `fill="currentColor"` en un SVG inline, el ícono adopta automáticamente el color del texto circundante. Permite cambiar el color con una sola regla CSS.

**`filter: hue-rotate()` (truco del color)**
Función CSS que rota el matiz de una imagen. Permite cambiar el color aproximado de un SVG cargado con `<img>` sin poder modificar su contenido. Técnica imperfecta pero útil cuando el SVG viene de un tercero.

**`postMessage`**
API del navegador para enviar mensajes entre ventanas (incluyendo `<iframe>`). Única vía limpia de comunicación entre un SVG aislado en iframe y su documento padre. Respeta la política de origen mediante el parámetro de `targetOrigin`.

**Contexto de navegación**
Entorno de ejecución de un documento: `window`, `document`, historial, origen, política de seguridad. Un `<iframe>` crea uno completo; un `<object>` uno parcial; `<img>` no crea ninguno (el SVG no es un documento navegable).

**Fallback (contenido alternativo)**
Contenido que se muestra cuando el recurso principal no carga. En `<object>` va dentro de las etiquetas; en `<img>` se usa el atributo `alt`. `<embed>` no acepta fallback.

**Lazy loading**
Carga diferida de imágenes hasta que están cerca del viewport (`loading="lazy"`). Es nativo en `<img>` pero no en SVG inline, que se descarga siempre con el HTML.

**`aria-label` en `<object>` e `<iframe>`**
Atributo ARIA que proporciona un nombre accesible al elemento. En `<object>` e `<iframe>` suple el rol de `alt` de `<img>`. El atributo `title` del `<iframe>` también cumple esta función y es obligatorio.

**Build tool**
Herramienta que procesa el código fuente antes de entregarlo al navegador (Vite, webpack, Astro, Parcel). Para SVG pueden: optimizar con SVGO, convertir a componentes JSX, inlinar automáticamente o generar sprite sheets.

**SVGO**
Optimizador de SVG que elimina metadatos innecesarios, comentarios de editor, atributos redundantes y simplifica paths. Un archivo SVG pasa a menudo de 15 KB a 2 KB sin pérdida visual.

**Sprite sheet SVG**
Archivo SVG único que contiene varios íconos dentro de `<symbol>` con `id`. Se referencian con `<use href="#id">` desde cualquier parte del HTML. Reduce peticiones y permite cachear todos los íconos juntos.

**XSS (Cross-Site Scripting)**
Vulnerabilidad en la que un atacante inyecta scripts en una página para ejecutarse con el contexto de la víctima. La prohibición de scripts dentro de SVGs cargados con `<img>` es una mitigación directa contra XSS vía imágenes.

**Mismo origen (Same-origin policy)**
Regla del navegador que restringe cómo un documento puede interactuar con recursos de otros orígenes. Afecta a `getSVGDocument()`: solo funciona si el SVG viene del mismo dominio, protocolo y puerto que el padre.
