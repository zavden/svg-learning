# GLOSARIO — Sprites SVG e Iconos

Vocabulario imprescindible para hablar de sistemas de iconos en SVG: qué es un sprite, qué es un `<symbol>`, en qué se diferencia un icon font, qué hace SVGR, y por qué `currentColor` es casi una religión.

---

## Fundamentos

**`sprite SVG`** — Archivo (o bloque inline) que empaqueta muchos iconos como `<symbol>` dentro de un único `<svg>` padre. Cada consumidor los referencia por id con `<use>`. Es el formato estándar para bibliotecas de iconos.

**`<symbol>`** — Elemento contenedor de SVG pensado para definir plantillas reusables. **No se renderiza** directamente en el lugar donde se declara: solo aparece cuando alguien lo instancia con `<use>`. Tiene su propio `viewBox`, lo que permite que escale automáticamente al tamaño del `<use>` que lo consume.

**`<use>`** — Elemento que clona otro nodo del documento (típicamente un `<symbol>`) en su posición. El clon vive en un **shadow tree** que el CSS del documento no puede atravesar con selectores arbitrarios.

**`<defs>`** — Contenedor para elementos que se definen pero no se renderizan. Los `<symbol>` pueden ir dentro de `<defs>`, aunque no es obligatorio: `<symbol>` ya es por naturaleza no-renderizable.

**`href`** (en `<use>`) — Atributo que apunta al id del elemento a clonar. Formato `#id` si está en el mismo documento, o `ruta/sprite.svg#id` si es externo. Reemplaza al obsoleto `xlink:href`.

---

## Tipos de sistemas

**`icon font`** — Fuente tipográfica donde cada glifo es un icono (FontAwesome, Material Icons en formato ttf). Se consume como texto: `<i class="fa fa-home"/>`. Tiene problemas serios: un solo color, accesibilidad pobre, alineación caprichosa, y depende del render de fuentes del navegador.

**`raster icons`** — Iconos como archivos PNG/JPG/WebP. No escalan (requieren `@2x`, `@3x` para HiDPI), no se pueden recolorear con CSS, y hay que duplicar archivos para cada tema.

**`sprite PNG`** — Técnica antigua: todos los iconos pegados en una sola imagen PNG, y cada elemento muestra un trozo con `background-position`. Funciona, pero arrastra todos los problemas de raster.

**`sprite SVG externo`** — El sprite vive en un archivo `.svg` separado, cacheable por el navegador. Se referencia con `<use href="/sprite.svg#icon"/>`. Restricción: está sujeto a la política **same-origin** y necesita CORS si es cross-domain.

**`sprite SVG inline`** — El bloque `<svg>` con los `<symbol>` va dentro del propio HTML (normalmente al inicio del `<body>` con `display:none`). No hay request extra, pero se descarga en cada página.

---

## Color y tematización

**`currentColor`** — Valor CSS especial que significa "el color de texto del contexto actual". Un `<path fill="currentColor"/>` se pinta con el `color` del elemento padre. Es el mecanismo fundamental para que un solo icono se pueda usar con cualquier color sin duplicar archivos.

**`fill`** — Atributo/propiedad CSS que define el color de relleno de una forma SVG. En iconos se suele declarar `fill="currentColor"` para heredar el color del contexto.

**`stroke`** — Atributo/propiedad que define el color del trazo. Para iconos "outline" (estilo Heroicons outline, Lucide) se usa `stroke="currentColor"` con `fill="none"`.

**`var(--name)`** — Función CSS que lee el valor de una variable (custom property). En iconos bicolor, `<path fill="var(--bg, #ccc)"/>` permite que desde fuera se tematice: `.theme { --bg: red; }`.

**`icon bicolor`** — Icono con dos paths separados, cada uno pintado con una variable CSS distinta. Permite tener "fondo" y "forma" con colores independientes, todo controlado desde el consumidor.

---

## Anatomía del icono

**`viewBox`** — Sistema de coordenadas interno del `<symbol>`. El estándar de facto es `0 0 24 24`, compatible con Material Design, Heroicons, Lucide, Tabler, etc. Mezclar viewBoxes distintos en el mismo sistema causa tamaños visuales inconsistentes.

**`base size`** — Tamaño lógico en el que se dibujan los iconos antes de escalarlos. El clásico es 24×24, pero también existen 16×16, 20×20 y 32×32. Una vez elegido, hay que respetarlo para todo el set.

**`trazo` / `stroke-width`** — Grosor del contorno en unidades del viewBox. En un icono 24×24 se suele usar 1.5 o 2. Si escalas a 48×48, el trazo se ve de 3 o 4 píxeles: coherente porque todo crece proporcionalmente.

**`icon-sm` / `icon-md` / `icon-lg`** — Convención CSS común para exponer varios tamaños de pantalla del mismo icono. Los valores típicos son 16, 24, 32, 48px. No se modifica el SVG: solo cambia el `width`/`height` del contenedor.

---

## Componentes en frameworks

**`SVGR`** — Herramienta oficial de React que convierte archivos `.svg` en componentes React automáticamente, optimizándolos con SVGO y añadiendo soporte para props, refs y accesibilidad. Existe como plugin para Vite, Webpack, Next.js y Rollup.

**`SVGO`** — Optimizador de SVGs (SVG Optimizer). Elimina metadatos, redondea decimales, simplifica paths y reduce el tamaño de archivo significativamente. Se ejecuta normalmente como paso de build.

**`tree-shaking`** — Técnica de bundlers modernos (Vite, Webpack, Rollup) que elimina del bundle final el código que no se usa. Con iconos como componentes, solo los iconos importados llegan al usuario.

**`componente de icono`** — Función React/Vue/Svelte que retorna un `<svg>` inline con los paths del icono. Acepta props típicas: `size`, `color`, `className`, y hace spread del resto con `...props` para máxima flexibilidad.

---

## Accesibilidad

**`aria-hidden="true"`** — Atributo que oculta un elemento a los lectores de pantalla. En un icono decorativo (junto a texto visible) se pone en el `<svg>` para evitar que el icono se anuncie como información extra.

**`focusable="false"`** — Atributo específico de SVG en Internet Explorer (y a veces en Edge Legacy) que evita que el `<svg>` reciba foco al tabular. Redundante en navegadores modernos, pero no daña y protege contra edge cases.

**`aria-label`** — Atributo que proporciona un nombre accesible a un elemento. Cuando un botón-icono no tiene texto visible (solo el icono), el `aria-label` va en el **botón**, no en el `<svg>`, y el `<svg>` lleva `aria-hidden="true"`.

**`role="img"`** — Cuando un SVG sí tiene significado por sí mismo (no es decorativo), se le puede poner `role="img"` junto con un `<title>` interno o un `aria-label`. Menos común que el patrón "icono oculto + label en el botón".

---

## Restricciones técnicas

**`shadow tree`** — Subárbol oculto donde el navegador clona el contenido del `<symbol>` cuando se referencia con `<use>`. Los selectores CSS del documento principal **no cruzan** esta frontera: por eso `.sobre { fill: red; }` no funciona dentro de un `<use>`.

**`herencia` (en SVG)** — Mecanismo por el cual propiedades como `color`, `fill`, `stroke`, `font-size` y las CSS custom properties **sí** cruzan del padre al contenido del `<use>`. Es la única vía para tematizar un símbolo desde fuera.

**`CORS` / `same-origin`** — Política del navegador que bloquea fetches de otro origen salvo que el servidor lo permita explícitamente con la cabecera `Access-Control-Allow-Origin`. Afecta a sprites externos cuando el sprite vive en un CDN distinto al del HTML.

**`display: none`** — Valor CSS que retira un elemento del layout. Es la forma moderna y recomendada de ocultar el `<svg>` contenedor del sprite inline sin impedir que sus `<symbol>` sean referenciables por `<use>`.

---

## Organización

**`catálogo de iconos`** — Página visual donde se muestran todos los iconos disponibles con su id/nombre. Puede ser una Storybook, un MDX, o una demo estática. Sin catálogo, el sistema se rompe: nadie encuentra lo que hay.

**`changelog de iconos`** — Registro de qué iconos se añaden, renombran o marcan como deprecated en cada versión. Para iconos eliminados, se suele anunciar con antelación y dar fecha de eliminación definitiva.

**`convención de nombres`** — Acuerdo sobre cómo se llaman los ids: `icon-home`, `ic-arrow-left`, `i-chevron-down`. Lo importante no es cuál, sino que sea **consistente en todo el repo**. Los ids no pueden empezar por número ni contener espacios.

**`deprecated icon`** — Icono marcado como obsoleto: aún funciona, pero no se recomienda usar en código nuevo. Se documenta en el changelog y normalmente apunta al icono que lo reemplaza.
