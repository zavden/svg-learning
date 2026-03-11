# FASE 7: Produccion
> Secciones 22-25 del Roadmap SVG Basico-Intermedio
> Sprites, optimizacion, herramientas y buenas practicas.

---

## 22. Sprites SVG e Iconos

### 22.1 Sistema de iconos con SVG
- Ventajas sobre icon fonts: multicolor, precision en pixeles, accesibilidad, menos hacks
- Ventajas sobre imagenes raster: escalabilidad, estilizabilidad, menor tamanio

### 22.2 SVG Sprite con `<symbol>` y `<use>`
- Definir todos los iconos como `<symbol>` dentro de un unico SVG
- Cada `<symbol>` tiene su propio `id` y `viewBox`
- Referenciar con `<use href="#iconId">`
- El SVG sprite puede estar inline en el HTML (oculto con `display:none` o `hidden`)
- O puede ser un archivo externo referenciado con `<use href="sprites.svg#iconId">`

### 22.3 SVG Sprite externo
- Archivo `.svg` con multiples `<symbol>`
- Referencia: `<use href="sprites.svg#iconId">`
- Limitaciones cross-origin: no funciona entre dominios diferentes sin CORS
- No funciona en IE/Edge legacy con archivos externos (se necesita polyfill como svg4everybody)
- No se puede estilizar el contenido interno con CSS externo

### 22.4 SVG inline como componentes (React, Vue, etc.)
- Cada icono como un componente que retorna SVG inline
- Maximo control de estilizacion y animacion
- Patron comun en frameworks modernos
- Herramientas: SVGR (React), vue-svg-loader, etc.

### 22.5 Organizacion de un sistema de iconos
- Convenciones de naming para ids
- Tamanio base consistente (16x16, 24x24, etc.)
- Uso de `viewBox="0 0 24 24"` como estandar comun
- currentColor como fill por defecto para heredar color del contexto

---

## 23. Optimizacion de SVG

### 23.1 Por que optimizar SVG
- Los editores graficos generan SVG con metadatos innecesarios
- Reducir tamanio de archivo para mejor rendimiento web
- Eliminar informacion redundante o innecesaria
- Mejorar legibilidad del codigo si se va a editar manualmente

### 23.2 SVGO (SVG Optimizer)
- Herramienta principal de optimizacion
- Uso por CLI: `svgo archivo.svg`
- Plugins de SVGO y que hacen:
  - `removeDoctype`: elimina DOCTYPE
  - `removeXMLProcInst`: elimina declaracion XML
  - `removeComments`: elimina comentarios
  - `removeMetadata`: elimina `<metadata>`
  - `removeEditorsNSData`: elimina namespaces de editores (Inkscape, Illustrator)
  - `cleanupAttrs`: limpia atributos con espacios extra
  - `mergeStyles`: combina bloques `<style>`
  - `inlineStyles`: convierte estilos en atributos de presentacion
  - `removeUselessDefs`: elimina `<defs>` vacios
  - `cleanupNumericValues`: reduce precision de numeros
  - `convertColors`: convierte colores a formato mas corto
  - `removeUnknownsAndDefaults`: elimina atributos desconocidos y valores por defecto
  - `removeNonInheritableGroupAttrs`: elimina atributos no heredables de grupos
  - `removeUselessStrokeAndFill`: elimina fill/stroke innecesarios
  - `cleanupIds`: acorta o elimina ids no usados
  - `convertShapeToPath`: convierte formas basicas a paths (reduce tamanio pero pierde semantica)
  - `mergePaths`: combina paths adyacentes con los mismos atributos
  - `convertPathData`: optimiza los datos del atributo d
  - `removeEmptyAttrs`, `removeEmptyContainers`, `removeEmptyText`
  - `collapseGroups`: elimina grupos innecesarios
  - `sortAttrs`: ordena atributos (mejora compresion gzip)
- Configuracion de SVGO: `svgo.config.js`
- Precauciones: algunos plugins pueden romper animaciones o interactividad

### 23.3 Optimizacion manual
- Reducir decimales en coordenadas (2-3 decimales suele ser suficiente)
- Usar coordenadas relativas en paths (suelen producir numeros mas pequenios)
- Eliminar grupos innecesarios (`<g>` sin atributos)
- Combinar paths con los mismos estilos
- Usar `<use>` para elementos repetidos
- Simplificar trazados (menos puntos de control)
- Preferir formas basicas sobre paths equivalentes (mas legible)

### 23.4 Compresion
- SVG se comprime muy bien con gzip/brotli (es texto)
- Formato SVGZ: SVG comprimido con gzip (requiere header Content-Encoding: gzip)
- Configurar el servidor para servir SVG con compresion

### 23.5 Rendimiento de renderizado
- Filtros complejos son costosos (especialmente feGaussianBlur con stdDeviation grande)
- Muchos elementos SVG pueden degradar el rendimiento
- Clip-paths y masks tienen costo de renderizado
- `will-change` en CSS para optimizar animaciones
- Considerar Canvas o WebGL para graficos con miles de elementos

---

## 24. Herramientas y Flujo de Trabajo

### 24.1 Editores graficos
- **Inkscape**: gratuito, open source, exporta SVG limpio
- **Adobe Illustrator**: profesional, necesita optimizar la salida SVG
- **Figma**: diseno web/UI, exporta SVG, plugin de optimizacion
- **Sketch**: macOS, popular para diseno UI
- **Affinity Designer**: alternativa a Illustrator

### 24.2 Editores de codigo
- Cualquier editor de texto puede editar SVG
- VS Code con extensiones SVG (preview, snippets, linting)
- Emmet para generar SVG rapidamente

### 24.3 Herramientas online
- **SVGOMG**: interfaz web para SVGO (jakearchibald.github.io/svgomg/)
- **SVG Path Editor**: editores visuales de paths
- **SVG Viewer**: previsualizar SVGs en el navegador
- **Boxy SVG**: editor online completo
- **Method Draw**: editor SVG online simple
- **URL Encoder for SVG**: para data URIs

### 24.4 Herramientas de desarrollo
- DevTools del navegador: inspeccionar SVG inline como cualquier elemento HTML
- Visualizar bounding boxes, transformaciones, filtros
- Editar atributos en tiempo real
- Animaciones: panel de animaciones en Chrome DevTools

### 24.5 Librerias JavaScript para SVG
- **D3.js**: visualizacion de datos (bindea datos a SVG)
- **Snap.svg**: manipulacion de SVG (sucesor de Raphael)
- **SVG.js**: libreria ligera para crear y animar SVG
- **GreenSock (GSAP)**: animaciones de alto rendimiento (soporta SVG)
- **Anime.js**: libreria de animaciones ligera
- **Vivus.js**: animacion de line-drawing
- **Lottie**: reproduce animaciones After Effects exportadas como JSON (puede usar SVG como renderer)
- **Paper.js**: graficos vectoriales con canvas y SVG

### 24.6 Integracion con build tools
- Importar SVG en Webpack (svg-inline-loader, @svgr/webpack)
- Importar SVG en Vite (vite-plugin-svgr, vite-svg-loader)
- PostCSS plugins para SVG en CSS

---

## 25. Buenas Practicas y Patrones Comunes

### 25.1 Estructura y organizacion
- Usar `<defs>` para todo lo reutilizable
- Agrupar elementos relacionados con `<g>` y dar ids/clases descriptivos
- Mantener un orden logico en el codigo: defs primero, luego contenido
- Usar viewBox siempre para garantizar escalabilidad
- Preferir coordenadas relativas al viewBox sobre valores absolutos

### 25.2 Nombrado y semantica
- Ids descriptivos para elementos que se referencian
- Clases CSS para estilizado (igual que en HTML)
- `<title>` y `<desc>` para accesibilidad
- Comentarios XML para secciones complejas

### 25.3 Rendimiento
- Mantener el numero de nodos bajo (menos de ~1000 para rendimiento optimo)
- Evitar filtros complejos en elementos animados
- Usar CSS transitions en lugar de SMIL cuando sea posible
- Considerar alternativas (Canvas, WebGL) para graficos muy complejos
- Lazy loading de SVGs pesados

### 25.4 Compatibilidad
- Probar en multiples navegadores
- Usar prefijos de vendor cuando sea necesario para CSS
- Tener en cuenta IE11 si se necesita soporte legacy:
  - No soporta SVG 2
  - Necesita `xlink:href` en vez de `href`
  - Problemas con `<use>` externo
  - Necesita width/height explicitos
- Fallbacks: `<img>` con PNG como alternativa dentro de `<object>`

### 25.5 Seguridad
- SVG puede contener `<script>`: cuidado con SVG de fuentes no confiables
- SVG en `<img>` y CSS `background-image` son seguros (scripts no se ejecutan)
- Sanitizar SVG subido por usuarios (eliminar scripts, event handlers, foreignObject)
- Nunca confiar en SVG externo para mostrarlo inline sin sanitizar
- XSS a traves de SVG: los atributos de evento (onclick, onload) son vectores de ataque

### 25.6 Patrones de diseno comunes
- **Icono adaptativo**: SVG que cambia de detalle segun tamanio (media queries internas)
- **Logo responsive**: multiples versiones del logo que se muestran segun el espacio
- **Grafico de datos**: combinar formas basicas y texto para charts simples
- **Ilustracion interactiva**: hover effects, tooltips, animaciones al hacer click
- **Loader/spinner**: animacion de carga con rotate + stroke-dasharray
- **Progress bar**: barra de progreso con stroke-dashoffset animado
- **Mapa interactivo**: regiones como paths con eventos hover/click
- **Texto decorativo**: texto en paths curvos con gradientes

### 25.7 Checklist antes de publicar
- [ ] viewBox definido correctamente
- [ ] Tamanios width/height adecuados o responsivos
- [ ] Accesibilidad: title, desc, role, aria-labels
- [ ] Optimizado con SVGO o similar
- [ ] Colores usando currentColor donde sea apropiado
- [ ] Sin metadatos innecesarios del editor
- [ ] Funciona en los navegadores objetivo
- [ ] Sin scripts si se va a usar como `<img>` o background
- [ ] Clases/ids limpios y descriptivos
- [ ] Compresion gzip/brotli configurada en el servidor
