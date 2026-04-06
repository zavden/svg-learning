# Notas — Recorte y Máscaras

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `<clipPath>` con formas básicas | ✓ | ✓ | ✓ | ✓ |
| `<clipPath>` con `<path>` y `<text>` | ✓ | ✓ | ✓ | ✓ |
| `clipPathUnits="userSpaceOnUse"` | ✓ | ✓ | ✓ | ✓ |
| `clipPathUnits="objectBoundingBox"` | ✓ | ✓ | ✓ | ✓ |
| `<mask>` con formas | ✓ | ✓ | ✓ | ✓ |
| `<mask>` con gradientes | ✓ | ✓ | ✓ | ✓ |
| `mask-type` (alpha/luminance) | ✓ | ✓ | ✓ | ✓ |
| CSS `clip-path: circle()` | ✓ | ✓ | ✓ (-webkit-) | ✓ |
| CSS `clip-path: polygon()` | ✓ | ✓ | ✓ (-webkit-) | ✓ |
| CSS `clip-path: url(#id)` | ✓ | ✓ | ✓ | ✓ |
| CSS `clip-path: path()` | ✓ | ✓ | ✓ | ✓ |
| Animación de `clip-path` con CSS | ✓ | ✓ | ✓ | ✓ |
| CSS `mask: url(#id)` | ✓ | ✓ | ✓ (-webkit-) | ✓ |

Safari históricamente requiere el prefijo `-webkit-` para `clip-path` y `mask` en algunas versiones. En proyectos con soporte amplio: incluir ambas formas.

---

## `mask-type`: alpha vs luminance

Por defecto, las máscaras SVG usan **luminance**: el brillo del contenido determina la opacidad. Se puede cambiar al modo **alpha**, donde se usa directamente el canal alfa del contenido:

```svg
<mask id="m-alpha" mask-type="alpha">
  <rect fill-opacity="0.5" fill="black" width="100" height="100"/>
</mask>
```

- **`luminance`** (por defecto): blanco = visible, negro = oculto. Lo que llamamos "máscara" en este documento.
- **`alpha`**: el canal alfa del contenido es la máscara directamente. Más intuitivo si vienes del mundo HTML/CSS.

En CSS, `mask-mode: alpha` es equivalente. Safari fue pionero en esto; el resto del ecosistema se ha puesto al día.

---

## Trucos y patrones no obvios

### Agujero real en un path con `fill-rule="evenodd"`

En lugar de anidar clipPaths para crear un agujero, un único `<path>` con dos subtrazados y `fill-rule="evenodd"` resuelve el problema:

```svg
<clipPath id="donut">
  <path d="M 0 0 H 200 V 200 H 0 Z M 100 100 m -30 0 a 30 30 0 1 0 60 0 a 30 30 0 1 0 -60 0"
        fill-rule="evenodd"/>
</clipPath>
```

El rectángulo grande menos el círculo interior produce una "dona" como recorte.

### Máscara con `<image>`

Puedes usar una imagen rasterizada como máscara. El brillo de cada píxel define la visibilidad:

```svg
<mask id="photo-mask">
  <image href="textura-blanco-negro.png" width="300" height="200"/>
</mask>
```

Útil para efectos pictóricos: texturas de papel rasgado, bordes irregulares, grunge.

### Texto como recorte

Un truco clásico para el efecto "texto relleno con imagen":

```svg
<clipPath id="txt">
  <text x="50" y="100" font-size="80" font-weight="900">HOLA</text>
</clipPath>
<image href="foto.jpg" clip-path="url(#txt)"/>
```

El texto actúa como ventana a través de la cual se ve la imagen.

### Múltiples máscaras con `<g>`

CSS permite varias máscaras con `mask: url(#a), url(#b);`, pero en SVG puro hay que aplicarlas en capas:

```svg
<g mask="url(#a)">
  <g mask="url(#b)">
    <rect .../>
  </g>
</g>
```

Cada nivel aplica su máscara sobre el resultado del anterior.

---

## Rendimiento

- **`<clipPath>` es más rápido que `<mask>`.** El motor solo tiene que hacer un test geométrico por píxel. La máscara, en cambio, debe rasterizar su contenido y usar la luminancia resultante.
- **Filtros dentro de máscaras son especialmente costosos.** Un `feGaussianBlur` en el contenido de una máscara obliga a rasterizar, desenfocar y luego mezclar.
- **Animar `clip-path` con CSS es barato** (el navegador lo optimiza). Cambiar el contenido de un `<clipPath>` o `<mask>` con JS/SMIL es más pesado.
- **Para muchos elementos con el mismo recorte**, usar `objectBoundingBox` permite reutilizar una única definición.

---

## CSS `mask` — sintaxis moderna

La propiedad CSS `mask` (y sus variantes) funciona similar a `background`:

```css
.elemento {
  mask-image: url(#mi-mask);
  mask-mode: luminance;      /* o alpha */
  mask-size: 100% 100%;
  mask-repeat: no-repeat;
  mask-position: center;
}
```

Con el shorthand: `mask: url(#mi-mask) center / 100% no-repeat luminance;`

Safari aún requiere el prefijo `-webkit-mask`. En 2025 la situación ha mejorado pero sigue siendo recomendable duplicar.

---

## Animar recorte: dos estrategias

### 1. CSS transitions / keyframes

```css
.revelar {
  clip-path: inset(0 100% 0 0);
  transition: clip-path 0.6s ease;
}
.revelar.visible {
  clip-path: inset(0 0 0 0);
}
```

Simple, fluido, acelerado por GPU.

### 2. Cambiar el `href` del `<clipPath>` con JS

```js
element.setAttribute('clip-path', 'url(#clip-state-2)');
```

Útil para saltar entre formas muy distintas que no son interpolables.

### Limitación de `polygon()`

Al animar entre dos `polygon()`, ambos deben tener **el mismo número de vértices**. Los vértices interpolan uno a uno en orden. Si los cuentas distintos, el navegador no sabe cómo mapearlos.

---

## Debugging

- **La máscara no aparece**: ¿tiene fondo blanco? ¿las formas internas son blancas sobre fondo negro?
- **El clipPath no recorta**: ¿está definido antes o referenciado correctamente? ¿El `id` coincide?
- **El recorte se deforma**: probablemente `objectBoundingBox` aplicado a un rectángulo no cuadrado — cambia a `userSpaceOnUse`.
- **No ves el efecto en HTML**: los SVG inline sí aceptan `clip-path: url(#id)`. Para elementos HTML es más delicado, requiere que el `<clipPath>` exista en un SVG en el documento.

---

## Fragmentos útiles

### Fade horizontal en borde

```svg
<linearGradient id="fade-edge" x1="0.85" y1="0" x2="1" y2="0">
  <stop offset="0" stop-color="white"/>
  <stop offset="1" stop-color="black"/>
</linearGradient>
<mask id="m-edge">
  <rect width="100%" height="100%" fill="url(#fade-edge)"/>
</mask>
```

### Viñeta suave

```svg
<radialGradient id="vig" cx="0.5" cy="0.5" r="0.5">
  <stop offset="0.6" stop-color="white"/>
  <stop offset="1"   stop-color="black"/>
</radialGradient>
<mask id="m-vig">
  <rect width="100%" height="100%" fill="url(#vig)"/>
</mask>
```

### Recorte responsivo que se adapta

```svg
<clipPath id="c-adapt" clipPathUnits="objectBoundingBox">
  <path d="M 0.5 0 L 1 0.5 L 0.5 1 L 0 0.5 Z"/>
</clipPath>
```

Un diamante que siempre se ajusta al tamaño del elemento, independientemente de sus dimensiones.
