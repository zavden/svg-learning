# GLOSARIO — Optimización de SVG

Términos clave del mundo de la optimización: herramientas (SVGO, SVGZ), plugins, algoritmos de compresión, métricas de rendimiento y decisiones de arquitectura (Canvas vs SVG).

---

## Herramientas

**`SVGO`** — SVG Optimizer. Herramienta oficial del ecosistema Node.js para optimizar SVGs. Funciona con un sistema de plugins donde cada uno hace una tarea concreta. Es el estándar de facto y está detrás de casi cualquier pipeline serio (SVGR, svgo-loader, lint-staged).

**`preset-default`** — Conjunto curado de ~35 plugins de SVGO que funcionan bien juntos sin romper la mayoría de SVGs. Es lo que se ejecuta si no pasas configuración. Permite overrides para ajustar plugins individuales.

**`svgo.config.js`** — Archivo de configuración de SVGO. Define qué plugins ejecutar, con qué parámetros, y si `multipass` debe aplicar las optimizaciones varias veces hasta que no haya más cambios.

**`multipass`** — Opción de SVGO que re-ejecuta los plugins hasta que el resultado se estabiliza. Un plugin puede habilitar oportunidades para otro: por ejemplo, `collapseGroups` puede dejar un elemento sin atributos que `removeEmptyAttrs` eliminará en el segundo pase.

**`@svgr/cli`, `@svgr/vite`, `@svgr/webpack`** — Plugins del ecosistema SVGR que convierten archivos `.svg` en componentes React y en el proceso los optimizan con SVGO.

**`svgo-loader`** — Loader de Webpack que optimiza SVGs al pasar por el bundler. Útil cuando los `.svg` se importan directamente en JS y se quieren optimizar en build.

**`Inkscape`** — Editor vectorial de código abierto. Incluye un "Optimized SVG" export que limpia la mayoría de metadatos al guardar.

**`picosvg`** — CLI alternativa escrita en Python (no Node). Menos popular que SVGO pero útil cuando el entorno no tiene Node disponible.

---

## Plugins de SVGO

**`removeDoctype`** — Elimina la declaración `<!DOCTYPE svg ...>`. Innecesaria en SVGs web modernos.

**`removeXMLProcInst`** — Quita la instrucción `<?xml version="1.0"?>`. Solo útil si el SVG se sirve como `.svg` standalone, no en documentos HTML.

**`removeComments`** — Elimina todos los comentarios XML (`<!-- ... -->`). Los comentarios del editor (autor, fecha, versión) suelen ocupar varias líneas.

**`removeMetadata`** — Elimina el elemento `<metadata>` con datos RDF, Dublin Core, etc. Totalmente irrelevante para la web.

**`removeEditorsNSData`** — Elimina namespaces propietarios (`xmlns:inkscape=`, `xmlns:sodipodi=`, `xmlns:illustrator=`) y todos los atributos de esos namespaces. Uno de los mayores ahorros en SVGs recién exportados.

**`cleanupAttrs`** — Normaliza espacios en blanco dentro de valores de atributos. Ejemplo: `d="M 10   20  L 30   40"` → `d="M 10 20 L 30 40"`.

**`mergeStyles`** — Combina varios bloques `<style>` consecutivos en uno solo.

**`inlineStyles`** — Convierte reglas CSS dentro de `<style>` a atributos de presentación en cada elemento. Elimina el `<style>` por completo. **Cuidado**: rompe animaciones CSS que dependen de selectores.

**`removeUnknownsAndDefaults`** — Elimina atributos desconocidos por la especificación y los que tienen el valor por defecto (ejemplo: `display="inline"`, `fill-opacity="1"`).

**`cleanupIds`** — Minifica IDs (`id="planeta"` → `id="a"`) y elimina los que no se referencian. **Peligroso** si el JS o el CSS los usan por nombre.

**`cleanupNumericValues`** — Reduce la precisión de los números según `floatPrecision` (default: 3). `49.9998` → `50`, `0.50` → `.5`.

**`convertColors`** — Normaliza colores al formato más corto: `rgb(255,0,0)` → `#ff0000` → `red`, `#FFFFFF` → `#fff`.

**`convertPathData`** — El plugin con más impacto. Optimiza el atributo `d`: convierte absolutas a relativas donde sean más cortas, elimina comandos redundantes, reduce decimales, usa comandos implícitos.

**`convertShapeToPath`** — Convierte `<rect>`, `<circle>`, `<ellipse>` a `<path>` equivalente cuando ahorra bytes. **Cuidado**: pierde la semántica de las formas.

**`convertEllipseToCircle`** — Si una `<ellipse>` tiene `rx == ry`, la convierte en `<circle>`. Ahorro pequeño pero gratis.

**`mergePaths`** — Combina `<path>` adyacentes con los mismos atributos de estilo en un único path con múltiples sub-trazados (`M ... M ... M ...`).

**`collapseGroups`** — Elimina `<g>` sin atributos significativos, aplanando la jerarquía. Frecuente en SVGs de editores gráficos que envuelven capas en grupos vacíos.

**`removeEmptyContainers`** — Elimina `<g></g>`, `<defs></defs>` y otros contenedores sin hijos.

**`removeUselessStrokeAndFill`** — Quita `fill` y `stroke` en elementos donde no hacen nada (ejemplo: un `<g>` cuyo único hijo ya redefine esos atributos).

**`removeViewBox`** — Elimina el `viewBox` si puede deducirse de `width`/`height`. **Casi siempre se debe desactivar**: sin `viewBox`, el SVG no escala responsivamente.

**`removeDimensions`** — Al contrario: elimina `width` y `height` y deja solo el `viewBox`. Útil si quieres que el SVG se escale al tamaño del contenedor por CSS.

**`sortAttrs`** — Ordena los atributos de los elementos en un orden fijo. Mejora la compresión gzip porque el texto es más predecible.

**`removeOffCanvasPaths`** — Elimina `<path>` completamente fuera del viewport visible (ninguno de sus puntos está dentro del viewBox).

---

## Compresión

**`gzip`** — Algoritmo de compresión sin pérdida basado en DEFLATE. Universal, rápido, y reduce SVG entre 60-80%. Lo soportan todos los navegadores y servidores.

**`brotli`** — Algoritmo de compresión más moderno desarrollado por Google. Comprime un 10-15% mejor que gzip en texto. Soportado en navegadores evergreen. `Content-Encoding: br`.

**`SVGZ`** — SVG comprimido con gzip de manera estática. Extensión `.svgz`. Requiere que el servidor envíe `Content-Encoding: gzip`. Menos común hoy porque los servidores comprimen al vuelo.

**`Content-Encoding`** — Cabecera HTTP que indica cómo está codificado el cuerpo de la respuesta (`gzip`, `br`, `identity`). El navegador lo decodifica antes de usarlo.

**`Accept-Encoding`** — Cabecera que envía el navegador para indicar qué codificaciones acepta (`gzip, deflate, br`). El servidor elige la mejor que ambos soportan.

**`doble compresión`** — Error de configuración: servir un `.svgz` (ya comprimido) a través de un servidor con gzip activo. El resultado es un archivo más grande, no más pequeño.

**`floatPrecision`** — Parámetro de `cleanupNumericValues` y `convertPathData` que define cuántos decimales conservar. Valores típicos: 1 para iconos 24×24, 3 para ilustraciones complejas.

---

## Rendimiento

**`layout thrashing`** — Patrón anti-performance donde el código fuerza al navegador a recalcular el layout varias veces por frame (leer/escribir/leer/escribir estilos). En SVG se da al animar `x`, `y`, `width`, `height` sin agrupar los cambios.

**`repaint`** — Fase del ciclo de renderizado donde el navegador vuelve a pintar los píxeles afectados. Animar `fill` o `stroke` fuerza repaint; `transform` no.

**`composition`** — Fase final del renderizado donde el navegador combina las capas en la pantalla. Solo `transform` y `opacity` se resuelven únicamente en composición, por eso son "gratis" para animar.

**`will-change`** — Propiedad CSS que avisa al navegador de que un elemento va a cambiar pronto. El navegador puede promoverlo a capa de composición. Hay que usarla con cuidado: cada capa consume memoria de GPU.

**`GPU layer` / `compositor layer`** — Capa que el navegador gestiona en la GPU de forma independiente. Animaciones de `transform` y `opacity` sobre capas propias se ejecutan a 60fps sin cargar el CPU.

**`feGaussianBlur`** — Filtro SVG que aplica un desenfoque gaussiano. **Muy costoso** porque requiere rasterización. Cada pixel del área filtrada se recalcula en cada frame. Evitar en elementos animados o grandes.

**`feTurbulence`** — Filtro que genera ruido procedural (textura de ruido, nubes, mármol). Extremadamente costoso en tiempo real.

**`feDisplacementMap`** — Filtro que desplaza píxeles basándose en otro mapa de colores. Muy costoso. Útil solo para efectos estáticos pre-renderizados.

**`requestAnimationFrame`** — API del navegador para sincronizar animaciones con el refresco de pantalla (típicamente 60fps). Se debe usar en lugar de `setInterval` o `setTimeout` para animaciones JS.

**`DOM node count`** — Número total de elementos en el árbol del documento. Para SVG, la regla práctica es <1000 nodos para rendimiento fluido. Más allá, considerar Canvas.

**`Canvas API`** — API 2D alternativa a SVG basada en pintado imperativo sobre un bitmap. No hay DOM por elemento. Mejor para gráficos con miles de elementos o animaciones masivas.

**`WebGL`** — API 3D acelerada por GPU. Para visualizaciones con decenas de miles de elementos o efectos 3D. Curva de aprendizaje mucho mayor que SVG o Canvas 2D.

---

## Métricas

**`LCP` (Largest Contentful Paint)** — Métrica de Core Web Vitals. Tiempo hasta que el elemento más grande del viewport se renderiza. Los SVG grandes sin optimizar pueden impactar negativamente esta métrica.

**`TTFB` (Time To First Byte)** — Tiempo que tarda el servidor en empezar a enviar la respuesta. SVG más pequeños = menos tiempo de transferencia total, indirectamente mejor TTFB percibido.

**`FPS`** — Frames por segundo. La meta es 60 en pantallas estándar, 120 en pantallas ProMotion. Animaciones que bajan de 30 FPS son claramente perceptibles como "laggy".

**`jank`** — Término informal para describir animaciones que pierden frames. Visualmente se nota como tirones o saltos. Medible en la pestaña Performance de DevTools.

**`bundle size`** — Tamaño total de los assets que llegan al navegador tras build y compresión. Los iconos inline contribuyen, por eso el tree-shaking de componentes y la optimización SVGO son importantes.
