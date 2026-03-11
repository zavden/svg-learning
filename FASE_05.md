# FASE 5: Integracion Web
> Secciones 17-19 del Roadmap SVG Basico-Intermedio
> SVG responsivo, accesibilidad, metodos de integracion y estilado con CSS.

---

## 17. SVG Responsivo y Accesibilidad

### 17.1 SVG responsivo basico
- Usar `viewBox` sin `width`/`height` fijos: el SVG se adapta al contenedor
- Usar `width="100%"` y `height="auto"` (o viceversa)
- El viewBox mantiene la proporcion del contenido
- `preserveAspectRatio` controla como se ajusta el contenido

### 17.2 Tecnicas de SVG fluido
- SVG inline en HTML: se comporta como un elemento de bloque
- Eliminar width/height del elemento `<svg>` y controlarlo con CSS
- Tecnica del padding-hack para aspect ratio fijo (legacy, reemplazada por aspect-ratio CSS)
- `aspect-ratio` en CSS para mantener proporciones

### 17.3 SVG adaptativo (responsive con media queries)
- Se pueden usar media queries CSS dentro de SVG
- `<style>` dentro del SVG con @media
- Ocultar/mostrar elementos segun el tamanio del viewport
- Simplificar graficos en pantallas pequenias
- NOTA: las media queries dentro de SVG responden al tamanio del viewport del SVG, no al del navegador (cuando se usa como archivo externo)

### 17.4 Accesibilidad en SVG
- `role="img"` en el elemento `<svg>` para indicar que es una imagen
- `<title>` como primer hijo: equivalente al atributo `alt` en `<img>`
- `<desc>`: descripcion larga del contenido
- `aria-labelledby`: referencia al `<title>` y/o `<desc>` por sus ids
- `aria-hidden="true"`: para SVGs puramente decorativos
- `role="presentation"`: similar a aria-hidden para SVGs decorativos
- `focusable="false"`: evita que el SVG reciba foco con teclado (IE/Edge legacy)
- Texto en SVG es accesible automaticamente
- Grupos semanticos con `role` y `aria-label`
- Patron de accesibilidad completo:
  ```xml
  <svg role="img" aria-labelledby="title desc">
    <title id="title">Titulo breve</title>
    <desc id="desc">Descripcion detallada</desc>
    <!-- contenido -->
  </svg>
  ```

### 17.5 SVG y alto contraste / modo oscuro
- Usar `currentColor` para que los colores sigan el tema
- Usar `@media (prefers-color-scheme: dark)` dentro del SVG
- Variables CSS (custom properties) para tematizacion

---

## 18. Integracion de SVG en la Web

### 18.1 SVG inline en HTML
- Se inserta directamente el codigo `<svg>` en el HTML
- Ventajas: acceso completo al DOM, estilizable con CSS, manipulable con JS
- Desventajas: aumenta el tamanio del HTML, no se cachea como archivo separado
- No necesita el namespace xmlns cuando esta inline en HTML5
- Es el metodo mas versatil

### 18.2 SVG como imagen con `<img>`
- `<img src="imagen.svg" alt="descripcion">`
- Ventajas: simple, se cachea, familiar
- Desventajas: no se puede manipular con CSS/JS externo, no se puede interactuar con elementos internos
- Las animaciones SMIL internas SI funcionan
- Los scripts internos NO se ejecutan (por seguridad)
- Los estilos externos NO se aplican

### 18.3 SVG como fondo CSS
- `background-image: url("imagen.svg")`
- Mismas limitaciones que `<img>`: sin interaccion CSS/JS
- Se puede controlar tamanio con `background-size`
- Util para patrones decorativos

### 18.4 SVG con `<object>`
- `<object type="image/svg+xml" data="imagen.svg"></object>`
- Ventajas: permite interaccion con el DOM del SVG desde JS externo (con getSVGDocument())
- Se puede incluir contenido fallback entre las etiquetas
- Tiene su propio contexto de documento

### 18.5 SVG con `<embed>`
- Similar a `<object>` pero mas antiguo
- Menos recomendado que `<object>` o inline

### 18.6 SVG con `<iframe>`
- `<iframe src="imagen.svg"></iframe>`
- El SVG tiene su propio documento y viewport
- Permite scripting si es del mismo origen

### 18.7 SVG como data URI
- En `<img>`: `<img src="data:image/svg+xml,<svg>...</svg>">`
- En CSS: `background-image: url("data:image/svg+xml,<svg>...</svg>")`
- Se puede codificar en base64 o URL-encoded
- Util para SVGs pequenios para evitar peticiones HTTP extra

### 18.8 Comparativa de metodos de integracion

| Metodo | CSS externo | JS externo | Animacion | Cache | Interactivo |
|--------|-------------|------------|-----------|-------|-------------|
| Inline | Si | Si | Si | No | Si |
| `<img>` | No | No | SMIL si | Si | No |
| CSS bg | No | No | SMIL si | Si | No |
| `<object>` | Interno | getSVGDocument | Si | Si | Si |
| `<iframe>` | Interno | postMessage | Si | Si | Si |

---

## 19. Estilado con CSS

### 19.1 Tres formas de aplicar CSS a SVG
1. **Atributos de presentacion**: directamente en el elemento (`fill="red"`)
2. **Estilo inline**: atributo `style` (`style="fill: red;"`)
3. **Hoja de estilos**: bloque `<style>` dentro del SVG o CSS externo (si es inline en HTML)

### 19.2 Prioridad de estilos (de menor a mayor)
1. Herencia del padre
2. Atributos de presentacion
3. Estilos de hoja de estilos (especificidad normal CSS)
4. Estilo inline (`style="..."`)
5. `!important`

### 19.3 Propiedades CSS especificas de SVG
- Propiedades de relleno: `fill`, `fill-opacity`, `fill-rule`
- Propiedades de trazo: `stroke`, `stroke-width`, `stroke-opacity`, `stroke-linecap`, `stroke-linejoin`, `stroke-dasharray`, `stroke-dashoffset`, `stroke-miterlimit`
- Propiedades de texto: `text-anchor`, `dominant-baseline`, `font-*`, `letter-spacing`, etc.
- Propiedades de marcadores: `marker-start`, `marker-mid`, `marker-end`
- Otras: `clip-path`, `mask`, `filter`, `opacity`, `visibility`, `display`, `pointer-events`, `paint-order`, `vector-effect`

### 19.4 Propiedades CSS generales que funcionan en SVG
- `display`: none/block/inline (ocultar elementos)
- `visibility`: visible/hidden (ocultar sin quitar del layout)
- `cursor`: cambiar el cursor al pasar sobre elementos
- `pointer-events`: controlar la interactividad (ver seccion 21)
- `transform` y `transform-origin`
- `transition` y `animation` (para animaciones CSS)
- `mix-blend-mode`: modos de mezcla
- `isolation`: contexto de mezcla
- Variables CSS (`--custom-property`)

### 19.5 Selectores CSS en SVG
- Todos los selectores CSS funcionan: clase, id, tipo, atributo, pseudo-clases
- Pseudo-clases interactivas: `:hover`, `:focus`, `:active`, `:focus-visible`
- Pseudo-clases estructurales: `:first-child`, `:nth-child()`, etc.
- Seleccion por atributo: `[fill="red"]`, `circle[r="50"]`
- No se puede seleccionar dentro de elementos `<use>` (shadow DOM)

### 19.6 Variables CSS (Custom Properties) en SVG
- Se pueden definir y usar variables CSS
- Muy util para tematizacion de SVGs
- Las variables se heredan del contexto HTML cuando el SVG es inline
- Ejemplo: `fill: var(--icon-color, currentColor)`

### 19.7 Bloque `<style>` dentro de SVG
- Se coloca tipicamente dentro de `<defs>` (pero puede ir en cualquier parte)
- Acepta la misma sintaxis CSS que HTML
- Se puede usar `<![CDATA[ ... ]]>` para escapar caracteres especiales en archivos SVG standalone
- Media queries dentro del SVG
