# Glosario — Herramientas y Flujo de Trabajo

## Editores gráficos

**`Inkscape`** — editor vectorial libre y multiplataforma. Usa SVG como formato nativo (no como exportación). Añade namespaces `inkscape:` y `sodipodi:` que SVGO limpia.

**`Plain SVG`** — opción de `Save As` en Inkscape que elimina la mayoría de metadatos específicos del editor antes de guardar. Primer paso antes de SVGO.

**`Illustrator`** — editor vectorial de Adobe. Históricamente genera SVG verboso; ofrece opciones al exportar (`Presentation Attributes`, `Decimal Places`, `Convert to Outlines`) que influyen en el tamaño final.

**`Presentation Attributes`** — modo de exportación de Illustrator que escribe los estilos como atributos HTML (`fill="red"`) en lugar de CSS interno. Más fácil de manipular desde JS.

**`Figma`** — herramienta de diseño colaborativa basada en navegador. Exporta SVG razonablemente limpio con `Copy as SVG` o el panel Export.

**`SVGO Compressor`** — plugin oficial de Figma que ejecuta SVGO antes de exportar. Conveniencia, no sustituto del SVGO del pipeline de código.

**`Sketch`** — editor de diseño UI exclusivo de macOS. Flujo de exportación SVG similar a Figma.

**`Affinity Designer`** — alternativa de pago único a Illustrator (Serif). Exporta SVG de calidad comparable.

---

## Editores de código

**`jock.svg`** — extensión de VS Code que añade preview en vivo, IntelliSense, hover docs y snippets para SVG. La más completa del ecosistema VS Code.

**`Emmet`** — sintaxis abreviada para generar markup (HTML, XML, SVG) con expansión por Tab. `svg>circle[cx=50]` → `<svg><circle cx="50"/></svg>`.

**`Prettier`** — formateador multilenguaje que trata SVG como XML. `formatOnSave` evita diffs de espacios en PRs.

**`HTMLHint`** / **`markuplint`** — linters que pueden detectar errores en SVG inline dentro de HTML (atributos sin valor, elementos sin cerrar, accesibilidad).

---

## Herramientas online

**`SVGOMG`** — `jakearchibald.github.io/svgomg/`. Frontend web oficial de SVGO con preview en tiempo real y toggles por plugin. Ideal para tareas puntuales sin pipeline local.

**`SVG Path Editor`** — `yqnn.github.io/svg-path-editor/`. Editor visual del atributo `d` con manipulación de nodos punto a punto.

**`SVG Path Visualizer`** — `svg-path-visualizer.netlify.app`. Explica cada comando de un `path` paso a paso (M, L, C, A…). Indispensable para aprender y debuggear.

**`Boxy SVG`** — `boxy-svg.com`. Editor SVG online completo, alternativa a Inkscape sin instalación.

**`Method Draw`** — editor SVG online minimalista, ideal para empezar.

**`SVGR Playground`** — `react-svgr.com/playground`. Convierte SVG a componente React online con todas las opciones de SVGR disponibles.

**`Haikei`** — `haikei.app`. Generador de fondos orgánicos SVG (blobs, ondas, partículas). Útil para heros de landing pages.

**`Pattern Monster`** — generador de patrones SVG repetibles (geométricos, tramas) exportables como archivo o data URI CSS.

**`URL Encoder for SVG`** — herramienta (yoksel.github.io) que escapa caracteres de un SVG para usarlo dentro de `url("data:image/svg+xml,...")` en CSS.

---

## DevTools del navegador

**`Panel Elements`** — pestaña de DevTools que muestra el árbol DOM. El SVG inline es navegable como cualquier otro nodo; los atributos se editan en vivo.

**`Panel Animations`** — herramienta de Chrome DevTools (`Cmd+Shift+P` → "Show Animations") que captura CSS animations y Web Animations API. Permite ralentizar a 0.1x/0.25x y pausar individualmente.

**`getBBox()`** — método DOM de elementos SVG que devuelve el bounding box real en el sistema de coordenadas del propio SVG. Incluye transformaciones aplicadas internamente.

**`getTotalLength()`** — método DOM de `<path>`, `<line>`, `<polyline>`, `<polygon>`, `<circle>`, `<rect>` que devuelve la longitud total del trazado. Imprescindible para configurar `stroke-dasharray`.

**`pauseAnimations()`** / **`unpauseAnimations()`** — métodos del elemento `<svg>` raíz que pausan o reanudan todas las animaciones SMIL del documento. Útil para depurar.

**`$0`** — variable global en la consola de DevTools que referencia el último elemento seleccionado en el panel Elements. `$0.getBBox()` aplica al nodo actual.

**`Inspector de accesibilidad de Firefox`** — panel que muestra el árbol ARIA real de los elementos SVG, incluidos `aria-label`, `role` y la exposición a tecnologías asistivas.

**`Inspector de filtros de Firefox`** — panel que permite editar visualmente filtros SVG complejos, incluida la matriz de `feColorMatrix`.

---

## Librerías JavaScript

**`D3.js`** — `d3js.org`. Librería de visualización de datos. Su filosofía es unir datos al DOM (data join), no dibujar directamente. Ideal cuando los datos dictan la forma.

**`data join`** — patrón central de D3: asociar un array de datos a una selección del DOM mediante `.data()`, y gestionar los nodos con `.enter()`, `.update()`, `.exit()`.

**`GSAP`** — *GreenSock Animation Platform*. La librería de animación JS de referencia. Anima cualquier atributo SVG o propiedad CSS, con timeline sofisticado y rendimiento excepcional.

**`DrawSVGPlugin`** — plugin de GSAP (Club GreenSock) que anima el trazado progresivo de paths sin tener que calcular `stroke-dasharray` manualmente.

**`MorphSVGPlugin`** — plugin de GSAP (Club GreenSock) que interpola la forma de un `path` a otro, incluso con distinto número de puntos.

**`Club GreenSock`** — licencia de pago anual que desbloquea los plugins avanzados de GSAP (Draw, Morph, SplitText, MotionPath, etc.).

**`Anime.js`** — `animejs.com`. Librería de animación ligera (~17KB) con buena cobertura de SVG. Alternativa a GSAP cuando el bundle size importa.

**`Vivus.js`** — librería especializada en una sola cosa: animar `stroke-dashoffset` de cada `<path>` para producir el efecto de "dibujo automático". Requiere SVGs con trazados (sin fill).

**`Lottie`** — `airbnb.io/lottie`. Renderizador de animaciones de After Effects en el navegador. Lee un JSON exportado por Bodymovin y lo renderiza en SVG o Canvas.

**`Bodymovin`** — plugin para After Effects que exporta animaciones al formato JSON que Lottie consume.

**`lottie-web`** — librería JS del lado del navegador que renderiza los JSON de Bodymovin. Pesa ~250KB; `dotLottie` es un formato más compacto.

**`SVG.js`** — `svgjs.dev`. Librería ligera (~13KB) para crear y manipular SVG con API fluida. "jQuery para SVG", sin el concepto de data binding de D3.

**`Snap.svg`** — librería creada por Adobe con diseño similar a SVG.js. Menos mantenida; preferir SVG.js en proyectos nuevos.

**`Paper.js`** — `paperjs.org`. Framework para Canvas con modelo vectorial y su propio lenguaje (PaperScript). Pensado para apps de dibujo interactivas y geometría computacional.

---

## Build tools

**`Vite`** — bundler moderno basado en esbuild (dev) y Rollup (prod). Importa SVG como URL por defecto, como string con `?raw`, como componente con `vite-plugin-svgr`.

**`?raw`** — query suffix de Vite que importa cualquier archivo como string. `import svg from './icon.svg?raw'`.

**`vite-plugin-svgr`** — plugin de Vite que convierte SVG a componente React usando `@svgr/core` internamente. Activa el query `?react`.

**`vite-svg-loader`** — plugin equivalente para Vue, que convierte SVG a componente Vue mediante el query `?component`.

**`@svgr/webpack`** — loader de Webpack que convierte SVG a componente React durante el build. Aplica SVGO internamente antes de la conversión.

**`svg-inline-loader`** — loader de Webpack que incrusta el SVG como string en el bundle, disponible como `import svgString from './icon.svg'`.

**`asset modules`** — sistema de Webpack 5 que reemplaza `url-loader` y `file-loader`. Tipos: `asset/resource` (emite archivo), `asset/inline` (data URI), `asset` (automático según tamaño), `asset/source` (string raw).

**`SVGR`** — `react-svgr.com`. Herramienta que convierte SVG a componentes React (o Vue). Pasa el SVG por SVGO y luego lo envuelve en JSX tipado.

**`data URI`** — formato `data:image/svg+xml,<contenido>` que incrusta el SVG directamente en CSS o HTML sin un fetch adicional. Requiere escapar caracteres especiales (`#`, `<`, `>`, `"`).

**`sprite externo`** — archivo SVG único que contiene varios `<symbol>`, referenciados desde HTML con `<svg><use href="sprite.svg#icon-name"/></svg>`. Caché agresivo y carga única.

**`tree-shaking`** — eliminación de código no usado durante el bundle. Requiere imports específicos (`import Icon from './icon'`) en lugar de barrel exports (`import { Icon } from './icons'`).
