# Seccion 23: Optimizacion de SVG

> Un SVG sin optimizar de un editor grafico puede pesar 10x mas de lo necesario.
> La optimizacion correcta reduce el tamano, mejora el rendimiento de renderizado
> y produce codigo mas limpio y mantenible.

---

## 23.1 Por que optimizar SVG

### Lo que los editores graficos generan

Cuando se exporta un SVG desde Illustrator, Figma o Inkscape, el archivo incluye:
- Declaraciones de namespace innecesarias para la web
- Metadatos del editor: nombre del archivo, version del programa, datos de capas
- Atributos con valores por defecto (que no necesitan estar presentes)
- Comentarios generados automaticamente
- IDs y clases generadas automaticamente (a menudo crípticas: `path4532`, `Layer_1`)
- Coordenadas con precision excesiva: `cx="49.9999999832"` en lugar de `cx="50"`
- Grupos vacios (`<g></g>`) creados por capas del editor sin contenido
- Estilos redundantes o duplicados

### El impacto real

Un icono simple exportado de Figma puede pesar 3-8KB.
El mismo icono optimizado con SVGO: 0.3-0.8KB.
La reduccion es tipicamente del **60-80%** en tamano de archivo.

### Mas alla del tamano: legibilidad

Un SVG bien optimizado escrito o limpiado manualmente:
- Tiene IDs descriptivos que tienen sentido
- Elimina los grupos innecesarios
- Usa formas basicas (`<circle>`, `<rect>`) en vez de paths equivalentes donde corresponde
- Tiene una estructura que un humano puede leer y entender

---

## 23.2 SVGO (SVG Optimizer)

### Que es SVGO

- La herramienta de optimizacion de SVG mas usada en el ecosistema web
- Escrita en JavaScript/Node.js
- Funciona con un sistema de **plugins**: cada plugin realiza una optimizacion especifica
- Se puede configurar para activar/desactivar cada plugin segun las necesidades

### Instalacion y uso basico

**CLI global**
```bash
npm install -g svgo
svgo icono.svg                    # optimiza, sobreescribe el original
svgo icono.svg -o icono.min.svg  # guarda en nuevo archivo
svgo *.svg                        # optimiza todos los SVG de la carpeta
svgo -f ./src/icons -o ./dist/icons  # carpeta origen y destino
```

**Como dependencia de proyecto**
```bash
npm install --save-dev svgo
# En package.json scripts:
# "optimize-svg": "svgo -f ./src/assets/svg -o ./public/svg"
```

### Los plugins de SVGO y que hacen

**Limpieza de metadatos**
- `removeDoctype`: elimina `<!DOCTYPE svg ...>`
- `removeXMLProcInst`: elimina `<?xml version="1.0"?>`
- `removeComments`: elimina todos los comentarios XML
- `removeMetadata`: elimina el elemento `<metadata>` (datos de Dublin Core, RDF)
- `removeEditorsNSData`: elimina namespaces y datos de editores (Inkscape: `inkscape:`, Illustrator: `illustrator:`, etc.)
- `cleanupAttrs`: normaliza espacios en blanco en valores de atributos

**Limpieza de estilos**
- `mergeStyles`: combina multiples bloques `<style>` en uno
- `inlineStyles`: convierte reglas CSS a atributos de presentacion (reduce el CSS a zero)
- `removeUnknownsAndDefaults`: elimina atributos desconocidos y valores que son el default (ej: `display="inline"`)
- `removeNonInheritableGroupAttrs`: elimina atributos de grupos que no son heredables y no tienen efecto

**Limpieza de IDs y referencias**
- `cleanupIds`: acorta IDs (a letras como `a`, `b`, `c`) o los elimina si no se referencian
- `removeUselessDefs`: elimina `<defs>` vacios o con elementos sin referencia

**Optimizacion numerica**
- `cleanupNumericValues`: reduce la precision de los numeros (`49.9999` → `50`, `0.5` → `.5`)
- `convertColors`: convierte colores al formato mas corto (`rgb(255,0,0)` → `#ff0000` → `red`)
- `convertPathData`: optimiza los datos del atributo `d` (coordenadas relativas, comandos implicitos, reduccion de precision)

**Optimizacion de formas**
- `convertShapeToPath`: convierte `<rect>`, `<circle>`, `<ellipse>` a `<path>` equivalente
  - **Precaucion**: reduce el tamano pero pierde la semantica de las formas
  - Desactivar si el SVG sera manipulado por JavaScript que busca elementos por tipo
- `mergePaths`: combina paths adyacentes con los mismos atributos de estilo en un unico path
- `convertEllipseToCircle`: convierte elipses con `rx=ry` a circulos

**Limpieza estructural**
- `collapseGroups`: elimina grupos `<g>` sin atributos significativos (fusiona con los hijos)
- `removeEmptyAttrs`: elimina atributos con valor vacio (`style=""`, `class=""`)
- `removeEmptyContainers`: elimina contenedores vacios (`<g></g>`, `<defs></defs>`)
- `removeEmptyText`: elimina elementos `<text>` vacios
- `removeUselessStrokeAndFill`: elimina `fill` y `stroke` en elementos que no los necesitan

**Otras optimizaciones**
- `sortAttrs`: ordena los atributos de los elementos (mejora la compresion gzip al hacer el texto mas predecible)
- `removeOffCanvasPaths`: elimina paths que estan completamente fuera del viewport

### Configuracion de SVGO: `svgo.config.js`

Para proyectos donde se necesita configuracion especifica:

```javascript
// svgo.config.js
module.exports = {
  plugins: [
    // Activar plugins por defecto con configuracion personalizada
    {
      name: 'cleanupNumericValues',
      params: { floatPrecision: 2 }   // 2 decimales en vez de los por defecto
    },

    // Desactivar un plugin especifico
    { name: 'convertShapeToPath', active: false },

    // Desactivar cleanupIds si los IDs son importantes (ej: para <use> o CSS)
    { name: 'cleanupIds', active: false },

    // Desactivar removeViewBox si se necesita el viewBox
    { name: 'removeViewBox', active: false }
  ]
};
```

### Plugins que pueden romper funcionalidad

**Desactivar segun el caso:**

| Plugin | Cuando desactivar |
|--------|-----------------|
| `cleanupIds` | Si el SVG usa `<use>` con IDs, o si CSS referencia IDs |
| `convertShapeToPath` | Si JS busca elementos por tipo (`querySelector('circle')`) |
| `removeViewBox` | Siempre desactivar — el viewBox es critico para SVG responsivo |
| `inlineStyles` | Si hay animaciones CSS que dependen de los estilos en `<style>` |
| `removeHiddenElems` | Si hay elementos ocultos que se muestran con JS |
| `mergePaths` | Si los paths tienen IDs o clases individuales que se usan en JS/CSS |

---

## 23.3 Optimizacion manual

### Reducir decimales en coordenadas

Los editores graficos generan coordenadas con alta precision que rara vez es necesaria:
- `d="M 49.9998 100.0002 L 149.9995 200.001"` → `d="M 50 100 L 150 200"`
- Para iconos y graficos de UI: 1-2 decimales es suficiente
- Para ilustraciones donde la precision importa: 3 decimales generalmente es el maximo necesario
- SVGO hace esto automaticamente con `cleanupNumericValues`

### Usar coordenadas relativas en paths

Los paths con coordenadas relativas producen numeros mas pequenios porque los desplazamientos son menores:
- Coordenadas absolutas: `M 150 200 L 200 250 L 250 200`
- Coordenadas relativas equivalentes: `M 150 200 l 50 50 l 50 -50`
- Los numeros son menores → el texto es mas corto → mejor compresion gzip

SVGO optimiza esto con `convertPathData`.

### Eliminar grupos innecesarios

Un `<g>` sin atributos no aporta nada:
```xml
<!-- Antes -->
<g>
  <g>
    <circle cx="50" cy="50" r="30" />
  </g>
</g>

<!-- Despues -->
<circle cx="50" cy="50" r="30" />
```

### Combinar paths con los mismos estilos

En vez de multiples paths con `fill="red"`:
```xml
<!-- Antes -->
<path fill="red" d="M..." />
<path fill="red" d="M..." />
<path fill="red" d="M..." />

<!-- Despues: un solo path con sub-trazados -->
<path fill="red" d="M... M... M..." />
```

### Usar `<use>` para elementos repetidos

Para elementos que aparecen multiples veces con el mismo diseno:
```xml
<!-- Antes: 5 circulos identicos -->
<circle cx="10" cy="50" r="5" fill="blue" />
<circle cx="30" cy="50" r="5" fill="blue" />
<circle cx="50" cy="50" r="5" fill="blue" />

<!-- Despues: definir una vez, usar muchas -->
<defs>
  <circle id="punto" r="5" fill="blue" />
</defs>
<use href="#punto" x="10" y="50" />
<use href="#punto" x="30" y="50" />
<use href="#punto" x="50" y="50" />
```

### Preferir formas basicas sobre paths equivalentes

Para elementos que pueden expresarse como formas basicas:
```xml
<!-- Path equivalente a un rectangulo: no legible -->
<path d="M 10 10 H 90 V 90 H 10 Z" fill="blue" />

<!-- Mas legible y semantico -->
<rect x="10" y="10" width="80" height="80" fill="blue" />
```

La forma basica no es necesariamente mas pequenio en bytes, pero si mas mantenible.

### Simplificar trazados

- Reducir el numero de puntos de control en curvas complejas
- Muchos editores graficos tienen herramientas de "simplificar trazado"
- Inkscape: `Path > Simplify`
- El objetivo: el minimo de puntos que mantiene la forma reconocible

---

## 23.4 Compresion

### SVG y la compresion de texto

SVG es texto plano con muchos patrones repetitivos (`<path`, `fill`, `stroke`, etc.).
Los algoritmos de compresion de texto (gzip, brotli) trabajan muy bien con estos patrones.

**Reduccion tipica:**
- SVG sin comprimir: 10KB
- SVG con gzip: 2-3KB (reduccion del 70-80%)
- SVG con brotli: 1.5-2KB (ligeramente mejor que gzip)

### Configuracion del servidor

**nginx**
```nginx
gzip on;
gzip_types image/svg+xml text/plain application/javascript;
```

**Apache (.htaccess)**
```apache
AddOutputFilterByType DEFLATE image/svg+xml
```

**Vite/webpack**: la compresion se maneja automaticamente en produccion.

### El formato SVGZ

SVGZ es simplemente un SVG comprimido con gzip:
- Extension: `.svgz`
- El servidor debe enviar el header: `Content-Encoding: gzip`
- El archivo ya esta comprimido: el servidor NO debe volver a comprimirlo
- Tiene el mismo efecto que servir un `.svg` con gzip, pero el archivo en disco ya esta comprimido
- Menos comun hoy en dia porque la compresion en el servidor es automatica

### Doble compresion: el error

Si el servidor tiene gzip activado Y el archivo ya es `.svgz` (gzip), se comprimira dos veces.
El resultado es un archivo **mas grande** (la doble compresion no funciona bien).
Configurar el servidor para no re-comprimir `.svgz`.

---

## 23.5 Rendimiento de renderizado

### El costo de los filtros

Los filtros SVG son computacionalmente costosos porque requieren rasterizacion:
- `feGaussianBlur` con `stdDeviation` grande: muy costoso, especialmente en elementos grandes
- Cada pixel del elemento (y del area del filtro) se procesa
- En pantallas Retina: el numero de pixeles se cuadruplica → el costo tambien

**Regla practica:** no aplicar filtros costosos (`feGaussianBlur`, `feTurbulence`, `feDisplacementMap`) en:
- Elementos animados
- Elementos que cambian frecuentemente
- Elementos de gran tamano

**Alternativa CSS:** `filter: blur()`, `filter: drop-shadow()` — el navegador puede acelerarlos por GPU

### `will-change` para animaciones

```css
.elemento-animado {
  will-change: transform, opacity;
}
```

- Indica al navegador que ese elemento va a cambiar
- El navegador puede pre-optimizarlo (crear una capa de composicion separada)
- Usar con moderacion: cada elemento con `will-change` consume memoria de GPU
- Solo activarlo cuando la animacion esta activa y desactivarlo al terminar

### El numero de nodos del DOM

- Cada elemento SVG es un nodo del DOM: consume memoria y tiempo de parseo
- Para graficos con cientos de elementos: el rendimiento puede degradarse
- Regla practica: menos de 1000 elementos SVG para rendimiento fluido
- Para graficos de datos densos (miles de puntos): considerar Canvas

### Canvas vs SVG: cuando hacer el cambio

| Escenario | SVG | Canvas |
|-----------|-----|--------|
| < 1000 elementos | ✅ Mejor | ❌ |
| > 1000 elementos | ⚠️ Degradacion | ✅ Mejor |
| Interactividad por elemento | ✅ Nativo (eventos DOM) | ❌ Manual (hit testing) |
| Escalado responsivo | ✅ Nativo | ❌ Requiere recalculo |
| Accesibilidad | ✅ Nativo | ❌ Requiere ARIA manual |
| Animacion de muchos elementos | ⚠️ | ✅ |
| Graficos en tiempo real | ⚠️ | ✅ |

### Optimizaciones de rendimiento en animaciones SVG

- **Animar solo `transform` y `opacity`**: estos dos se pueden acelerar por GPU
- **Evitar animar `d`, `fill`, `stroke-width`**: fuerzan recalculo del layout
- **Usar `requestAnimationFrame`** para animaciones JavaScript
- **Evitar acceder al DOM en cada frame**: calcular posiciones fuera del loop de animacion
- **Usar CSS transitions/animations** en vez de JavaScript cuando sea posible: el navegador las optimiza mejor
