# Seccion 19: Estilado con CSS

> CSS y SVG son complementarios. CSS puede controlar casi cualquier propiedad visual
> de SVG usando los mismos mecanismos que en HTML. Entender la cascada SVG y
> que propiedades son animables via CSS abre posibilidades enormes.

---

## 19.1 Tres formas de aplicar CSS a SVG

### Forma 1: Atributos de presentacion

```xml
<circle fill="steelblue" stroke="#333" stroke-width="2" r="40" cx="50" cy="50" />
```

**Caracteristicas**
- Se escriben directamente como atributos XML en el elemento
- Son la forma "nativa" de SVG: todos los editores graficos los generan
- Tienen la **menor prioridad** en la cascada (pueden ser sobreescritos por CSS)
- Solo aceptan valores SVG (no funciones CSS modernas como `calc()`, variables, etc.)

**Atributos validos como presentacion**
- Propiedades de pintura: `fill`, `stroke`, `stroke-width`, `opacity`, etc.
- Propiedades de fuente: `font-family`, `font-size`, `font-weight`, etc.
- No todas las propiedades CSS tienen equivalente como atributo de presentacion

### Forma 2: Estilo inline

```xml
<circle style="fill: steelblue; stroke: #333; stroke-width: 2px;" r="40" cx="50" cy="50" />
```

**Caracteristicas**
- El atributo `style` acepta CSS normal (incluyendo funciones, variables, etc.)
- Tiene alta prioridad en la cascada (solo superado por `!important`)
- Acepta **todas** las propiedades CSS que aplican a SVG, no solo las de presentacion
- Permite usar `var()`, `calc()`, y otras funciones CSS modernas

**Diferencia con atributos de presentacion**
- `fill="steelblue"` (atributo) puede ser sobreescrito por CSS
- `style="fill: steelblue;"` (inline) solo puede ser sobreescrito por `!important`

### Forma 3: Hoja de estilos

**Bloque `<style>` dentro del SVG**
```xml
<svg viewBox="0 0 100 100">
  <style>
    circle { fill: steelblue; }
    .destacado { stroke: red; stroke-width: 3; }
    #principal { opacity: 0.8; }
  </style>
  <circle r="40" cx="50" cy="50" class="destacado" id="principal" />
</svg>
```

**CSS externo (solo para SVG inline en HTML)**
```css
/* En el archivo CSS del documento */
svg circle { fill: steelblue; }
.mi-svg .destacado { stroke: red; }
```

**Caracteristicas de la hoja de estilos**
- Usa toda la potencia de CSS: selectores, media queries, animaciones, variables
- Reutilizable: un estilo aplicado a muchos elementos sin repeticion
- Especificidad CSS normal: clase > tipo de elemento, `id` > clase
- La mejor opcion para SVGs con muchos elementos del mismo tipo

---

## 19.2 Prioridad de estilos (de menor a mayor)

### La cascada SVG completa

```
1. Valor inicial del navegador (lo mas bajo)
        ↓
2. Herencia del elemento padre
        ↓
3. Atributos de presentacion  ← fill="red", stroke="blue"
        ↓
4. Reglas CSS en hoja de estilos (por especificidad CSS)
        ↓
5. Estilo inline  ← style="fill: red;"
        ↓
6. !important (lo mas alto)
```

### El punto mas importante y menos intuitivo

**Los atributos de presentacion tienen MENOR prioridad que el CSS**

Esto significa que:
```xml
<!-- El fill="blue" sera sobreescrito por el CSS -->
<circle fill="blue" class="mi-clase" />
```
```css
.mi-clase { fill: red; }  /* Esto gana: CSS > atributo de presentacion */
```
El circulo sera **rojo**, no azul.

**Por que importa**
- Permite "tematizar" un SVG generado por un editor grafico con CSS externo, sin modificar el SVG
- Un icono de Figma con `fill="#005ea5"` puede cambiar de color con `.btn:hover svg path { fill: currentColor; }`
- Si el SVG usa `style="fill: #005ea5;"` (inline), **no se puede sobreescribir** con CSS normal (solo con `!important`)

### Prioridad entre reglas CSS

Dentro de las reglas CSS, se aplica la especificidad CSS estandar:
- Selector de tipo: `circle` — especificidad (0,0,1)
- Selector de clase: `.mi-clase` — especificidad (0,1,0)
- Selector de id: `#mi-id` — especificidad (1,0,0)
- Selector compuesto: `.contenedor circle` — especificidad (0,1,1)

### Estrategia practica segun el caso

| Objetivo | Tecnica recomendada |
|----------|-------------------|
| SVG generado por editor, estilos base | Dejar atributos de presentacion como estan |
| Sobreescribir estilos del editor con CSS | Usar selectores CSS (mayor prioridad que atributos) |
| Estilos que no deben poder sobreescribirse | Usar `style=""` inline |
| Tematizacion dinamica | Variables CSS en hoja de estilos |

---

## 19.3 Propiedades CSS especificas de SVG

Propiedades que existen solo en SVG (no en HTML normal):

### Propiedades de relleno
- `fill`: color de relleno
- `fill-opacity`: opacidad del relleno (0-1)
- `fill-rule`: regla de relleno (`nonzero` | `evenodd`)

### Propiedades de trazo
- `stroke`: color del trazo
- `stroke-width`: grosor del trazo
- `stroke-opacity`: opacidad del trazo
- `stroke-linecap`: forma de los extremos (`butt` | `round` | `square`)
- `stroke-linejoin`: forma de las uniones (`miter` | `round` | `bevel`)
- `stroke-dasharray`: patron de guiones
- `stroke-dashoffset`: desplazamiento del patron de guiones
- `stroke-miterlimit`: limite de la punta miter

### Propiedades de texto SVG
- `text-anchor`: alineacion horizontal (`start` | `middle` | `end`)
- `dominant-baseline`: alineacion vertical de la linea base
- `alignment-baseline`: alineacion dentro de elementos inline SVG

### Propiedades de marcadores
- `marker-start`: marcador al inicio del path
- `marker-mid`: marcador en vertices intermedios
- `marker-end`: marcador al final del path
- `marker`: atajo para los tres anteriores

### Propiedades de renderizado y efectos
- `clip-path`: recorte con clipPath
- `mask`: mascara de visibilidad
- `filter`: filtros SVG
- `paint-order`: orden de pintado (fill, stroke, markers)
- `vector-effect`: efectos de vector (`non-scaling-stroke`)
- `color-interpolation`: espacio de color para gradientes
- `shape-rendering`: calidad de renderizado de formas
- `text-rendering`: calidad de renderizado de texto

---

## 19.4 Propiedades CSS generales que funcionan en SVG

Propiedades CSS de HTML que tambien aplican a SVG:

### Visibilidad y layout
- `display`: `none` oculta el elemento completamente (sin ocupar espacio); `block`, `inline`, etc.
- `visibility`: `hidden` oculta visualmente pero mantiene el espacio en el layout
- `overflow`: controla que pasa con el contenido fuera de los limites del SVG/grupo

### Interaccion
- `cursor`: cambia el cursor al pasar sobre elementos SVG
- `pointer-events`: controla que partes del elemento responden a eventos (ver seccion 21.6)

### Transformaciones y animaciones
- `transform` y `transform-origin`: misma sintaxis que en HTML
- `transition`: anima cambios de propiedades SVG
- `animation` y `@keyframes`: animaciones complejas de propiedades SVG

### Modos de mezcla
- `mix-blend-mode`: como se mezcla el elemento con el contenido debajo (`multiply`, `screen`, `overlay`, etc.)
- `isolation`: crea un contexto de mezcla para el elemento y sus hijos

### Variables CSS
- `--mi-variable: valor;` — definir variables en cualquier elemento SVG o en `:root`
- `var(--mi-variable)` — usar la variable en cualquier propiedad CSS
- Las variables se heredan del HTML al SVG inline

---

## 19.5 Selectores CSS en SVG

### Todos los selectores CSS funcionan

**Selector de tipo**
```css
circle { fill: steelblue; }
rect { fill: none; stroke: black; }
text { font-family: sans-serif; }
```

**Selector de clase**
```css
.activo { fill: #e74c3c; }
.etiqueta { font-size: 12px; text-anchor: middle; }
```

**Selector de id**
```css
#fondo { fill: #f5f5f5; }
#titulo { font-weight: bold; font-size: 18px; }
```

**Selector de atributo**
```css
[fill="red"] { stroke: darkred; }          /* elementos con fill exactamente "red" */
[data-estado="activo"] { opacity: 1; }     /* atributos de datos */
circle[r="50"] { fill: gold; }             /* tipo + atributo */
```

**Selectores de relacion**
```css
g > circle { fill: blue; }         /* circulos hijos directos de g */
.grupo circle { fill: blue; }      /* circulos descendientes de .grupo */
circle + text { font-size: 10px; } /* texto hermano inmediato de circulo */
```

### Pseudo-clases interactivas

```css
circle:hover { fill: #e74c3c; cursor: pointer; }
circle:active { fill: #c0392b; }
.btn:focus { outline: 2px solid blue; }
path:focus-visible { stroke: #005ea5; stroke-width: 2; }
```

**Importante**: `:hover` funciona en elementos SVG individuales (no solo en el SVG completo), incluyendo paths complejos y grupos

### Pseudo-clases estructurales

```css
g:first-child { opacity: 0.5; }
rect:nth-child(even) { fill: #f0f0f0; }
circle:last-child { stroke: red; }
path:not(.excluido) { fill: steelblue; }
```

### La limitacion de `<use>` y el shadow DOM

Los selectores CSS **no pueden penetrar** en el shadow DOM de `<use>`:
```css
/* Esto NO funciona para el contenido dentro de <use> */
use circle { fill: red; }
use .elemento { stroke: blue; }
```
Alternativas para estilizar contenido de `<use>`: variables CSS heredadas, `currentColor`, o atributos directos en el `<use>`.

---

## 19.6 Variables CSS (Custom Properties) en SVG

### Definir variables

**En el SVG (`:root` del SVG)**
```xml
<svg viewBox="0 0 100 100">
  <style>
    :root {
      --color-primario: steelblue;
      --color-acento: #e74c3c;
      --grosor-linea: 2;
    }
    circle { fill: var(--color-primario); stroke: var(--color-acento); stroke-width: var(--grosor-linea); }
  </style>
</svg>
```

**En el HTML (heredadas por SVG inline)**
```css
/* CSS del documento */
:root {
  --color-marca: #005ea5;
  --radio-icono: 40px;
}
```
```xml
<!-- SVG inline puede usar las variables del documento -->
<circle r="40" style="fill: var(--color-marca);" />
```

### Tematizacion con variables CSS

El patron mas potente para SVGs tematizables:

```xml
<svg viewBox="0 0 100 100">
  <style>
    :root {
      --c-relleno: white;
      --c-trazo: #333;
    }
  </style>
  <circle fill="var(--c-relleno)" stroke="var(--c-trazo)" r="40" cx="50" cy="50" />
</svg>
```

En CSS externo (si el SVG es inline):
```css
/* Tema por defecto */
.componente svg { --c-relleno: white; --c-trazo: #333; }

/* Tema oscuro */
.componente.oscuro svg { --c-relleno: #1a1a2e; --c-trazo: #e0e0e0; }

/* Estado hover */
.componente:hover svg { --c-relleno: steelblue; }
```

### Variables en `<use>` para multiples colores

```xml
<symbol id="icono-bicolor" viewBox="0 0 24 24">
  <rect fill="var(--ic-color-1, currentColor)" ... />
  <circle fill="var(--ic-color-2, currentColor)" ... />
</symbol>

<!-- Cada instancia tiene sus propios colores -->
<use href="#icono-bicolor" style="--ic-color-1: red; --ic-color-2: blue;" />
<use href="#icono-bicolor" style="--ic-color-1: green; --ic-color-2: yellow;" />
```

---

## 19.7 Bloque `<style>` dentro de SVG

### Donde colocarlo
- Puede ir en cualquier parte del SVG, pero la convencion es al inicio (dentro o fuera de `<defs>`)
- Los estilos aplican a todo el SVG independientemente de donde este el `<style>`
- Multiples bloques `<style>` en el mismo SVG: los estilos se combinan (como en HTML)

### Sintaxis CSS normal
- Acepta la misma sintaxis que un archivo CSS externo
- Variables CSS, media queries, `@keyframes`, etc.

### La trampa del `<![CDATA[]]>` en archivos SVG standalone

En archivos `.svg` que seran parseados como XML (no como HTML):
```xml
<style>
  <![CDATA[
    circle { fill: red; }
    /* El caracter < y & no necesitan escaparse dentro de CDATA */
    .clase { content: "<valor>"; }
  ]]>
</style>
```

**Por que es necesario en XML**
- XML requiere que `<` y `&` dentro de texto sean escapados (`&lt;`, `&amp;`)
- El CSS puede contener estos caracteres (en selectores, valores, etc.)
- `<![CDATA[...]]>` indica que el contenido es "datos de caracteres" y no se interpreta como XML
- En HTML (SVG inline en HTML5), el parser no requiere CDATA: el contenido de `<style>` se trata como texto

**Cuando usarlo**
- Solo en archivos `.svg` standalone que se abriran directamente o procesaran como XML
- No necesario en SVG inline dentro de HTML5
