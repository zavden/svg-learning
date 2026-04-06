# Notas — SVG responsivo y accesibilidad

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `viewBox` + `width: 100%` | ✓ | ✓ | ✓ | ✓ |
| `height: auto` (calculado por viewBox) | ✓ | ✓ | ✓ | ✓ |
| `aspect-ratio` CSS | ✓ 88+ | ✓ 89+ | ✓ 15+ | ✓ |
| `@media` dentro de `<svg>` | ✓ | ✓ | ✓ | ✓ |
| `prefers-color-scheme` | ✓ 76+ | ✓ 67+ | ✓ 12.1+ | ✓ |
| `prefers-contrast` | ✓ 96+ | ✓ 101+ | ✓ 14.1+ | ✓ |
| `prefers-reduced-motion` | ✓ | ✓ | ✓ | ✓ |
| `currentColor` en fill/stroke | ✓ | ✓ | ✓ | ✓ |
| Variables CSS (`--*`) en SVG inline | ✓ | ✓ | ✓ | ✓ |
| `role="img"` | ✓ | ✓ | ✓ | ✓ |
| `<title>` leído por AT | ✓ | ✓ | ✓ | ✓ |
| `aria-labelledby` en SVG | ✓ | ✓ | ✓ | ✓ |

**Notas**:
- En SVG como `<img src>`, las media queries responden al tamaño del propio SVG, no al del navegador. Esto es útil para diagramas que se adaptan a su espacio disponible, pero sorprende si no se conoce.
- Las variables CSS heredadas del documento HTML solo funcionan si el SVG está **inline**, no como archivo externo.

---

## `viewBox` vs. `width`/`height`: la tabla de comportamiento

| Declaración | Responsivo | Tamaño por defecto | Recomendado para |
|---|---|---|---|
| Solo `viewBox` | ✓ | 300×150 (replaced element) | Componentes con CSS |
| `viewBox` + `width`/`height` | ✓ (con CSS) | El declarado | Assets sueltos |
| Solo `width`/`height` | ✗ | El declarado | Nunca |
| Ninguno | ✗ | 300×150 | Nunca |

La combinación **`viewBox` + sin dimensiones fijas + CSS externo** es la más flexible para sistemas de diseño. **`viewBox` + dimensiones** es mejor cuando el SVG debe tener un tamaño razonable sin CSS (íconos que pueden ser descargados por el usuario).

---

## Lectores de pantalla: matices

### Orden recomendado del patrón accesible

```svg
<svg role="img" aria-labelledby="svg-title svg-desc" viewBox="...">
  <title id="svg-title">Título corto</title>
  <desc id="svg-desc">Descripción larga opcional</desc>
  <!-- contenido visual -->
</svg>
```

- `<title>` y `<desc>` deben ser **primeros hijos**.
- El orden `title` antes de `desc` se respeta en el orden de lectura.
- `aria-labelledby` usa los dos ids separados por espacio.

### Lectores vs. títulos tooltip

`<title>` tiene un doble rol:
1. Texto accesible para lectores de pantalla.
2. Tooltip al pasar el cursor (comportamiento de navegador).

En **elementos internos** del SVG (`<g>`, `<path>`, `<circle>`…), un `<title>` hijo actúa como tooltip visual. Esto es útil para gráficos interactivos pero **no sustituye** la accesibilidad global del SVG.

### `aria-label` vs. `<title>`

```svg
<!-- Opción A: title interno -->
<svg role="img" aria-labelledby="t"><title id="t">Logo</title></svg>

<!-- Opción B: aria-label directo (más simple) -->
<svg role="img" aria-label="Logo">...</svg>
```

`aria-label` es más conciso pero **no es accesible a herramientas de traducción** del navegador. Si el SVG viaja entre idiomas, `<title>` es preferible porque puede ser traducido.

---

## Cuándo las media queries internas son "al revés"

### SVG inline en HTML

```html
<body>
  <svg viewBox="0 0 200 100">
    <style>
      @media (max-width: 600px) { .label { display: none; } }
    </style>
    ...
  </svg>
</body>
```

Aquí `600px` se refiere a la **ventana del navegador**. Cuando el usuario reduce la ventana, la media query se activa. Comportamiento predecible.

### SVG externo (`<img>`, `<object>`, `background`)

```html
<img src="diagrama.svg" width="200">
```

Aquí `600px` en `@media (max-width: 600px)` dentro del SVG se refiere al **tamaño del SVG en sí**, que mide 200px. La media query siempre estará activa, sin importar la ventana del navegador.

**Consecuencia práctica**: si tu SVG se usará como archivo externo, tus breakpoints son los del SVG (algo como 50px, 100px, 200px), no los del navegador (600px, 768px, 1024px).

---

## `prefers-reduced-motion`

Para SVGs con animaciones, respetar la preferencia de movimiento reducido del usuario:

```svg
<style>
  .anim { animation: spin 2s linear infinite; }
  @media (prefers-reduced-motion: reduce) {
    .anim { animation: none; }
  }
</style>
```

Esto es especialmente importante en loaders, gráficos animados y decoraciones. La web accesible respeta a usuarios con trastornos vestibulares.

---

## `prefers-contrast`

Para SVGs que deben adaptarse al modo de alto contraste:

```svg
<style>
  path { stroke: #64748b; stroke-width: 1; }
  @media (prefers-contrast: more) {
    path { stroke: black; stroke-width: 2; }
  }
</style>
```

Aumenta el grosor de trazos y usa colores con contraste máximo cuando el usuario lo solicita.

---

## Gráficos de datos: patrones de accesibilidad

### Opción 1 — Tabla oculta visualmente (preferida)

```html
<figure>
  <svg aria-hidden="true" viewBox="...">...</svg>
  <figcaption class="sr-only">
    <table>
      <caption>Ventas trimestrales 2024</caption>
      <tr><th>T1</th><td>45</td></tr>
      ...
    </table>
  </figcaption>
</figure>
```

Los datos reales están en HTML, totalmente accesibles. El SVG solo presenta la vista gráfica.

### Opción 2 — `<desc>` descriptivo

Cuando no se puede adjuntar tabla, incluir los datos principales en el `<desc>`:

```svg
<desc>
  Gráfico de barras con cuatro columnas: Enero 45, Febrero 62,
  Marzo 78, Abril 91 mil euros. Tendencia ascendente continua.
</desc>
```

### Opción 3 — `<title>` por elemento

Para gráficos interactivos donde cada elemento tiene significado:

```svg
<g role="listitem">
  <title>Enero: 45 mil euros</title>
  <rect x="..." y="..." .../>
</g>
```

Útil con `role="list"` en el contenedor, pero requiere cuidado: las tecnologías asistivas no siempre lo anuncian consistentemente.

---

## Patrones no obvios

### Ocultar visual pero conservar clickeable

```css
.visually-hidden {
  position: absolute;
  width: 1px; height: 1px;
  padding: 0; margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

Estándar de facto para "screen-reader only".

### SVG heredando variables del HTML

```html
<html style="--color-primario: #3b82f6;">
  <body>
    <svg viewBox="...">
      <circle fill="var(--color-primario)"/>
    </svg>
  </body>
</html>
```

Solo funciona con SVG **inline**. Permite tematización centralizada: cambias `--color-primario` en un solo lugar y todos los SVGs actualizan.

### Responsive sin JavaScript: `vmin`/`vmax` dentro del SVG

Rara vez necesario, pero posible: las unidades de viewport (`vw`, `vh`, `vmin`) funcionan dentro del SVG y se resuelven contra el viewport del navegador (en SVG inline) o del propio SVG (en externo).

---

## Rendimiento y accesibilidad

- **`<title>` y `<desc>` no afectan rendimiento**. Son elementos ligeros.
- **Media queries internas tienen coste mínimo** — el motor CSS del SVG es el mismo que el del HTML.
- **Variables CSS en SVG**: sin penalty medible frente a valores hardcoded.
- **Recalcular proporciones al resize**: el navegador ya optimiza esto; el `viewBox` no añade trabajo extra.

---

## Debugging

- **El SVG no escala**: ¿tiene `viewBox`? ¿Eliminaste `width`/`height` del `<svg>` o declaraste `width: 100%` en CSS?
- **El SVG aparece con un espacio debajo**: falta `display: block`.
- **Las media queries no responden al navegador**: ¿el SVG es externo? Entonces responden al tamaño del propio SVG.
- **El lector de pantalla no anuncia nada**: ¿tiene `role="img"` + `<title>`? ¿El `aria-labelledby` apunta a ids existentes?
- **El lector anuncia contenido confuso**: probablemente falta `role="img"` y está leyendo cada elemento interno.
- **El modo oscuro no se aplica**: ¿el SVG es inline? Las variables CSS del documento solo se heredan en SVG inline.
- **El color no cambia entre contextos**: ¿usaste `currentColor` en lugar de un color fijo?

---

## Fragmentos útiles

### Detección con JavaScript del tema actual

```js
const isDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
const highContrast = window.matchMedia('(prefers-contrast: more)').matches;
```

### Toggle manual de tema que sobrescribe la preferencia del sistema

```html
<html data-theme="light"> <!-- o "dark" -->
```

```css
svg {
  --bg: white;
  --fg: black;
}
[data-theme="dark"] svg {
  --bg: #1a1a2e;
  --fg: white;
}
```

### Clase `.sr-only` canónica

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```
