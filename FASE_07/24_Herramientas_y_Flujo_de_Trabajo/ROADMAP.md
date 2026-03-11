# Seccion 24: Herramientas y Flujo de Trabajo

> El ecosistema de herramientas para SVG es amplio: desde editores graficos de escritorio
> hasta librerias JavaScript especializadas. Elegir las herramientas correctas segun el
> tipo de proyecto es tan importante como conocer el lenguaje SVG en si mismo.

---

## 24.1 Editores graficos

### Cuanod usar un editor grafico vs escribir SVG a mano

| Caso de uso | Herramienta recomendada |
|-------------|------------------------|
| Iconos e ilustraciones vectoriales | Editor grafico → exportar → limpiar |
| SVG generado programaticamente | Codigo directo (o D3.js) |
| Prototipo rapido | SVGOMG + edicion manual |
| Sistema de iconos grande | Editor grafico + SVGR |
| Animaciones complejas | Editor + GSAP/Lottie |

### Inkscape

**Que es:** editor de graficos vectoriales gratuito y open source

**Ventajas para SVG:**
- Trabaja con SVG como formato nativo (no es un formato de exportacion, es el formato interno)
- La exportacion SVG es la mas "limpia" entre los editores graficos gratuitos
- Herramientas especializadas: editor de nodos de paths, editor XML integrado
- El editor XML incorporado permite ver y editar el SVG mientras se edita graficamente
- Soporte de filtros SVG, gradientes, patrones y la mayoria de caracteristicas SVG

**Desventajas:**
- Agrega namespaces propios (`inkscape:`, `sodipodi:`) que conviene limpiar con SVGO
- La interfaz de usuario puede ser compleja para principiantes
- Rendimiento mas lento que Figma/Illustrator con archivos complejos

**Flujo recomendado con Inkscape:**
1. Disenar en Inkscape
2. `File > Save As > Plain SVG` (elimina la mayoria de datos de Inkscape)
3. Pasar por SVGO para limpieza final

**Editor XML integrado:**
- `Edit > XML Editor` (Ctrl+Shift+X)
- Permite ver y editar todos los atributos directamente en el SVG
- Util para entender que genera Inkscape y para hacer ajustes finos

### Adobe Illustrator

**Que es:** el editor vectorial profesional de referencia en la industria

**El problema con la exportacion SVG:**
- Historicamente generaba SVGs muy verbosos con muchos metadatos de Illustrator
- Versiones recientes han mejorado, pero sigue requiriendo post-procesamiento
- Agrega declaraciones XML, namespaces `illustrator:` y metadatos no necesarios en la web

**Opciones de exportacion:**
- `File > Export > Export As... > SVG` — para uso en la web
- `File > Save As > SVG` — mas orientado a preservar la fidelidad del diseno

**Opciones criticas en el dialogo de exportacion SVG:**
- `Styling: Presentation Attributes` — genera atributos, mas facil de manipular con JS
- `Styling: Internal CSS` — genera un bloque `<style>`, mas eficiente para muchos elementos
- `Font: Convert to Outlines` — convierte texto a paths (no depende de fuentes)
- `Images: Embed` — incluye imagenes como base64 (archivo mas grande, pero autocontenido)
- `Decimal Places: 1-2` — reducir la precision excesiva de los decimales

**Post-procesamiento necesario:** pasar siempre por SVGO tras exportar de Illustrator.

### Figma

**Que es:** herramienta de diseno UI/UX colaborativa basada en browser

**Exportacion SVG en Figma:**
- Click derecho sobre el elemento o frame → `Copy/Paste as > Copy as SVG`
- O: panel de capas → icono de exportar → elegir formato SVG
- Para iconos: aplanar capas antes de exportar para SVG mas limpio

**Lo que genera Figma:**
- Generalmente SVG mas limpio que Illustrator
- Puede incluir IDs autogenerados crípticos
- Los efectos avanzados (sombras, difuminados) se convierten a filtros SVG
- El texto puede quedar como `<text>` o convertido a paths segun la configuracion

**Plugin recomendado:** `SVGO Compressor` — aplica SVGO directamente dentro de Figma antes de exportar

**Figma y el diseno de sistemas de iconos:**
- Los frames de 24x24 con auto layout son ideales para iconos consistentes
- Componentes de Figma → exportar como SVG → procesar con SVGR → componentes React/Vue

### Sketch

**Que es:** herramienta de diseno UI exclusiva para macOS

**Exportacion SVG:**
- Soportada nativamente para cualquier capa o artboard
- La calidad de la exportacion es comparable a Figma
- Tambien genera algunos datos propietarios que SVGO limpia bien

**Nota:** Sketch tiene menor adoption que Figma en proyectos nuevos. Si el equipo usa Sketch, el flujo es identico: exportar → SVGO.

### Affinity Designer

**Que es:** alternativa profesional y asequible a Illustrator (disponible en macOS, Windows, iPad)

**Ventajas para SVG:**
- Exportacion SVG de buena calidad
- Precio de compra unico (sin suscripcion)
- Soporte de guias de recorte, mascaras y efectos SVG

**Consideracion:** la adoption es menor que Illustrator o Figma, lo que puede afectar la colaboracion en equipos mixtos.

---

## 24.2 Editores de codigo

### VS Code y SVG

VS Code es el editor mas popular para desarrolladores web y tiene buen soporte para SVG.

**Extensiones esenciales:**

| Extension | Funcion |
|-----------|---------|
| `SVG` (jock.svg) | Preview en vivo, IntelliSense, snippets, documentacion hover |
| `SVG Preview` | Preview en panel lateral al guardar |
| `SVG Snippets` | Snippets para elementos SVG comunes |
| `Svg Preview` (Simon Siefke) | Preview interactivo con zoom |

**Flujo con VS Code:**
1. Abrir el `.svg` — preview automatico con la extension
2. Editar atributos — IntelliSense sugiere valores validos
3. Guardar — el preview se actualiza en tiempo real
4. Ver errores de sintaxis — subrayado inline

**Emmet y SVG:**
Emmet funciona dentro de archivos `.svg` y archivos HTML para la parte SVG:
```
svg>circle[cx=50 cy=50 r=40]
```
Genera:
```xml
<svg>
  <circle cx="50" cy="50" r="40"></circle>
</svg>
```

**Snippets utiles de Emmet para SVG:**
- `svg` → elemento raiz
- `path` → elemento path
- `g` → grupo
- Los atributos se escriben entre corchetes

**Formato y linting:**
- `Prettier` formatea SVG correctamente (lo trata como XML)
- `HTMLHint` o `markuplint` puede detectar errores en SVG inline en HTML

### Otros editores

- **WebStorm / IntelliJ**: soporte SVG integrado similar a VS Code con plugins
- **Vim/Neovim**: edicion eficiente del XML, pero sin preview visual integrado
- **Notepad++**: soporte basico de resaltado de sintaxis XML para SVG

---

## 24.3 Herramientas online

### SVGOMG (jakearchibald.github.io/svgomg/)

La herramienta online mas importante para SVG. Interfaz web para SVGO con visualizacion en tiempo real.

**Como usarla:**
1. Arrastrar el archivo SVG o pegar el codigo
2. Ajustar los toggles de plugins en el panel derecho
3. Ver el resultado en tiempo real con comparacion antes/despues
4. Descargar el SVG optimizado o copiar el codigo

**Plugins criticos a revisar en SVGOMG:**
- `Remove viewBox` → **siempre desactivar** (el viewBox es critico)
- `Cleanup IDs` → desactivar si el SVG usa `<use>` con IDs
- `Remove title` → desactivar si el SVG necesita accesibilidad
- `Convert shapes to paths` → desactivar si JS busca elementos por tipo

**Tip de workflow:** SVGOMG muestra el porcentaje de reduccion y el tamano antes/despues. Util para saber cuanto "ruido" tenia el SVG original.

### Herramientas para edicion de paths

**SVG Path Editor (yqnn.github.io/svg-path-editor/)**
- Editor visual de paths con control punto a punto
- Permite ver y editar el atributo `d` con visualizacion en tiempo real
- Util para ajustar paths generados por editores graficos

**SVG Path Visualizer (svg-path-visualizer.netlify.app)**
- Muestra paso a paso como se dibuja un path
- Anota cada comando del atributo `d`
- Excelente para aprender y para debuggear paths complejos

### Otras herramientas online

| Herramienta | Uso |
|-------------|-----|
| **Boxy SVG** (boxy-svg.com) | Editor SVG online completo, alternativa a Inkscape |
| **Method Draw** (editor.method.ac) | Editor SVG online simple, ideal para empezar |
| **SVG Viewer** (svgviewer.dev) | Preview y optimizacion, convierte a React |
| **URL Encoder for SVG** | Convierte SVG a data URI para uso en CSS |
| **SVGR Playground** (react-svgr.com/playground) | Convierte SVG a componente React online |
| **Haikei** (haikei.app) | Generador de fondos y formas SVG |
| **Pattern Monster** | Generador de patrones SVG |

---

## 24.4 Herramientas de desarrollo del navegador

### Inspeccionar SVG en DevTools

El SVG inline en HTML es accesible como cualquier elemento DOM en las DevTools.

**Chrome/Edge DevTools:**

**Panel Elements:**
- Expandir el SVG en el arbol DOM para ver todos los elementos
- Click en cualquier elemento SVG para ver sus propiedades en el panel de estilos
- Doble click en un atributo para editarlo en tiempo real
- Los cambios se reflejan inmediatamente en el canvas del navegador

**Panel de estilos (Styles):**
- Ver que estilos CSS se aplican al elemento SVG seleccionado
- Ver la cascada: CSS, atributos de presentacion, valores heredados
- Activar/desactivar estilos con el checkbox

**Visualizacion de bounding boxes:**
- Al seleccionar un elemento SVG en Elements, se resalta en la pagina
- El panel de modelo de caja (Box Model) muestra dimensiones
- Las transformaciones se visualizan con las guias de seleccion

### Panel de Animaciones

`Chrome DevTools > More tools > Animations` (o `Ctrl+Shift+P` → "Show Animations")

- Captura animaciones CSS y Web Animations API en SVG
- Muestra la timeline de cada animacion
- Permite ralentizar (0.1x, 0.25x) para depurar
- Pausa y reanuda animaciones individualmente

**Tip:** para depurar animaciones SMIL, usar `element.pauseAnimations()` y `element.unpauseAnimations()` desde la consola.

### Consola para SVG

Tecnicas utiles desde la consola del navegador:

```javascript
// Seleccionar un elemento SVG por ID
const circulo = document.getElementById('mi-circulo');

// Ver todos los atributos
Array.from(circulo.attributes).forEach(attr =>
  console.log(`${attr.name}: ${attr.value}`)
);

// Modificar un atributo en tiempo real
circulo.setAttribute('fill', 'red');

// Ver el bounding box de un elemento SVG
const bbox = circulo.getBBox();
console.log(bbox); // { x, y, width, height }

// Obtener la longitud de un path (util para animaciones stroke-dasharray)
const path = document.querySelector('path');
console.log(path.getTotalLength());
```

### Firefox DevTools para SVG

Firefox tiene algunas ventajas para SVG:
- El inspector de fuentes funciona bien con `<text>` SVG
- El panel de accesibilidad muestra el arbol ARIA de elementos SVG
- Mejor soporte para inspeccionar filtros SVG

---

## 24.5 Librerias JavaScript para SVG

### D3.js (d3js.org)

**Que es:** la libreria de referencia para visualizacion de datos en SVG

**Filosofia:** D3 une datos a elementos del DOM. En lugar de dibujar directamente, se describe la relacion entre datos y representacion visual.

**Para que usar D3:**
- Graficos estadisticos (barras, lineas, areas, dispersion)
- Mapas geograficos con GeoJSON y proyecciones
- Graficos de red y jerarquias (dendrogramas, treemaps)
- Cualquier visualizacion donde los datos dictan la forma del SVG

**Para que NO usar D3:**
- Ilustraciones estaticas — usar editores graficos
- Animaciones simples — GSAP o CSS son mas simples
- Iconos — sprites SVG o SVGR son mas adecuados

**Curva de aprendizaje:** alta. D3 requiere entender su modelo de datos, las escalas, los layouts y los generadores de paths.

### GSAP (GreenSock Animation Platform)

**Que es:** la libreria de animaciones JavaScript mas potente y con mejor rendimiento

**Ventajas para SVG:**
- Anima cualquier atributo SVG, propiedad CSS, o valor numerico
- Plugin especifico: `GSAPSVGPlugin` y `DrawSVGPlugin` (para animaciones de trazado)
- Timeline sofisticado para secuenciar animaciones
- Rendimiento excepcional (optimizado internamente)
- Soporte de `morphSVG` para animar la forma de un path a otro

**Licencia:** GSAP es gratuita para proyectos personales y muchos comerciales. Los plugins avanzados requieren licencia Club GreenSock.

**Cuando elegir GSAP:**
- Animaciones SVG complejas y secuenciadas
- Cuando el rendimiento es critico
- Para efectos de morphing entre paths
- Para animaciones controladas por scroll

### Anime.js (animejs.com)

**Que es:** libreria de animaciones ligera (~17KB) con buena cobertura de SVG

**Ventajas:**
- Mucho mas ligera que GSAP
- API intuitiva
- Anima atributos SVG, CSS, y objetos JavaScript
- Soporte de easing personalizado, staggering de elementos

**Cuando elegir Anime.js:**
- Proyectos donde el peso del bundle importa
- Animaciones moderadamente complejas
- Cuando no se necesitan los plugins avanzados de GSAP

### Vivus.js

**Que es:** libreria especializada en una sola cosa: la animacion de "line drawing" (dibujo de trazados)

**Como funciona:**
1. Se necesita un SVG con paths (sin fill, solo stroke)
2. Vivus anima `stroke-dashoffset` en cada path de 0 a la longitud del path
3. El efecto: el SVG parece dibujarse a si mismo

**Cuando usar Vivus:**
- Logotipos que se dibujan solos
- Ilustraciones en estilo sketch/lineal
- Efectos de escritura a mano

**Limitacion:** requiere que el SVG sea solo de trazados. No funciona con fills.

### Lottie (airbnb.io/lottie)

**Que es:** renderizador de animaciones After Effects en el navegador

**Como funciona:**
1. El animador trabaja en After Effects con el plugin Bodymovin
2. Exporta la animacion como JSON (no SVG directamente)
3. Lottie lee el JSON y lo renderiza en el navegador (puede usar SVG o Canvas como renderer)

**Ventajas:**
- Las animaciones se crean visualmente en After Effects
- Soporta animaciones muy complejas que serian imposibles de escribir a mano
- El renderer SVG de Lottie mantiene la calidad vectorial

**Cuando usar Lottie:**
- Iconos animados complejos
- Animaciones de onboarding/loading elaboradas
- Cuando el equipo tiene animadores que trabajan con After Effects

### SVG.js (svgjs.dev)

**Que es:** libreria ligera (~13KB) para crear y manipular SVG con JavaScript

**Diferencia con D3:** SVG.js es mas simple y directo. No tiene el concepto de binding de datos. Es mas parecido a un jQuery para SVG.

**Cuando usar SVG.js:**
- Crear SVG dinamicamente con una API fluida
- Manipulacion e interactividad sin la complejidad de D3
- Animaciones basicas con la API de animacion integrada

### Snap.svg (snapsvg.io)

**Que es:** libreria de manipulacion SVG, creada por Adobe

**Estado:** menos mantenida que SVG.js. Para proyectos nuevos, SVG.js es una mejor alternativa.

### Paper.js (paperjs.org)

**Que es:** framework de scripting vectorial que corre sobre Canvas (con soporte de SVG)

**Caracteristica unica:** tiene su propio lenguaje de scripting (PaperScript) y modelo de objetos basado en el modelo de Illustrator.

**Cuando usar Paper.js:**
- Aplicaciones de dibujo interactivo
- Geometria computacional compleja
- Cuando se necesita la potencia de Canvas con una API vectorial

---

## 24.6 Integracion con build tools

### Vite

**Importar SVG como URL (comportamiento por defecto):**
```javascript
import logoUrl from './logo.svg';
// logoUrl es la URL del asset: '/assets/logo-abc123.svg'
```

**Importar SVG como string (raw):**
```javascript
import logoSvg from './logo.svg?raw';
// logoSvg es el contenido del archivo SVG como string
```

**Importar SVG como componente React con vite-plugin-svgr:**
```javascript
// vite.config.js
import svgr from 'vite-plugin-svgr';
export default { plugins: [svgr()] };

// En el componente
import { ReactComponent as Logo } from './logo.svg';
```

**vite-svg-loader para Vue:**
```javascript
import svgLoader from 'vite-svg-loader';
export default { plugins: [svgLoader()] };

import LogoSvg from './logo.svg?component';
```

### Webpack

**@svgr/webpack** — convierte SVG a componentes React durante el build:
```javascript
// webpack.config.js
{
  test: /\.svg$/,
  use: ['@svgr/webpack']
}
```

**svg-inline-loader** — incrusta el SVG como string en el bundle:
```javascript
{
  test: /\.svg$/,
  loader: 'svg-inline-loader'
}
```

**url-loader / asset modules** — para SVG como data URI o URL:
```javascript
{
  test: /\.svg$/,
  type: 'asset/resource' // emite el archivo y da la URL
  // o
  type: 'asset/inline' // convierte a data URI
}
```

### SVGO en el flujo de build

**Ejecutar SVGO como parte del build:**
```json
// package.json
{
  "scripts": {
    "optimize-svg": "svgo -f ./src/assets/svg -o ./public/svg",
    "build": "npm run optimize-svg && vite build"
  }
}
```

**svgo en Node.js para pipelines personalizados:**
```javascript
import { optimize } from 'svgo';
import { readFileSync, writeFileSync } from 'fs';

const svgString = readFileSync('./input.svg', 'utf-8');
const result = optimize(svgString, {
  plugins: [
    { name: 'removeViewBox', active: false },
    { name: 'cleanupIds', active: false }
  ]
});
writeFileSync('./output.svg', result.data);
```

### Decision: que enfoque de integracion usar

| Enfoque | Cuando usar |
|---------|-------------|
| SVG como URL (`/assets/logo.svg`) | Logos, imagenes decorativas, SVG que no necesita tematizacion |
| SVG inline en HTML | SVG que necesita CSS/JS del documento padre |
| SVG como componente (SVGR) | Sistema de iconos en React/Vue, tree-shaking, props de color/tamano |
| SVG sprite externo | Muchos iconos compartidos entre paginas, caching del sprite |
| SVG data URI en CSS | Backgrounds, bullets, patrones decorativos en CSS |
