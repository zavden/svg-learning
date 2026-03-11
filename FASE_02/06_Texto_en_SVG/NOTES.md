# Notas y consejos — Texto en SVG

---

## Compatibilidad

`<text>`, `<tspan>` y `<textPath>` tienen soporte universal en todos los navegadores modernos. `dominant-baseline` tiene soporte completo desde hace años. `<foreignObject>` es soportado en todos los navegadores modernos pero no funciona en SVG como `<img>`.

---

## El patrón de centrado perfecto

Para centrar texto tanto horizontal como verticalmente en un punto (cx, cy):

```
<text x="cx" y="cy"
      text-anchor="middle"
      dominant-baseline="middle">
  Texto
</text>
```

Esto funciona para centrar texto en círculos, rectángulos, badges, botones, etc. Es el patrón más usado en gráficos SVG.

---

## Interlineado con `em`

Siempre usa `dy="1.4em"` (o `1.5em` para más espacio) en lugar de `dy="18"` o un valor fijo en px. Los valores en `em` son relativos al `font-size`, así que si cambias el tamaño de la fuente, el interlineado se ajusta automáticamente.

```
<!-- Frágil: si cambias font-size, el interlineado no cambia -->
<tspan dy="20">línea 2</tspan>

<!-- Robusto: el interlineado siempre es proporcional -->
<tspan dy="1.4em">línea 2</tspan>
```

---

## Fuentes web con `@font-face` en SVG

Puedes embeber una fuente directamente en el SVG con un bloque `<style>`:

```svg
<svg xmlns="http://www.w3.org/2000/svg">
  <style>
    @font-face {
      font-family: 'MiFuente';
      src: url('data:font/woff2;base64,...') format('woff2');
    }
  </style>
  <text font-family="MiFuente">Texto independiente de fuente</text>
</svg>
```

Esto hace el SVG portable pero aumenta el tamaño del archivo. Para iconos SVG pequeños, convierte el texto a paths en su lugar.

---

## Texto SVG y CSS externo

Los atributos tipográficos SVG son también propiedades CSS. Puedes controlarlos desde una hoja de estilos externa:

```css
/* En tu hoja CSS externa */
.etiqueta-grafico {
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  fill: #64748b;
}
```

```svg
<text class="etiqueta-grafico" x="100" y="50">Ventas</text>
```

Esto es especialmente útil para mantener la tipografía consistente entre SVGs y el resto de la página.

---

## `paint-order="stroke"` para texto con contorno

Por defecto, el stroke se dibuja **encima** del fill (modelo del pintor). Para texto con borde ancho, esto recorta el fill. Usar `paint-order="stroke fill"` pinta el stroke debajo:

```svg
<!-- Sin paint-order: stroke cubre el fill -->
<text stroke="black" stroke-width="4" fill="white">SVG</text>

<!-- Con paint-order: stroke debajo, fill encima (resultado más limpio) -->
<text stroke="black" stroke-width="4" fill="white" paint-order="stroke fill">SVG</text>
```

---

## `textLength` y `lengthAdjust`

Para hacer que el texto encaje exactamente en un ancho dado:

```
<text textLength="200" lengthAdjust="spacing">Texto ajustado</text>
<text textLength="200" lengthAdjust="spacingAndGlyphs">Texto ajustado</text>
```

- `spacing`: ajusta solo el espacio entre caracteres
- `spacingAndGlyphs`: estira también los caracteres

Útil para etiquetas que deben encajar en un espacio exacto (tablas, mapas).

---

## Medir el ancho de texto con JavaScript

SVG no tiene una forma nativa de saber cuánto ancho ocupa un texto en el SVG. Con JS:

```javascript
const textEl = document.querySelector('text');
const bbox = textEl.getBBox();
// bbox.width = ancho del texto
// bbox.height = altura del texto
// bbox.x, bbox.y = posición del bounding box
```

Esto permite posicionar dinámicamente elementos alrededor del texto (como fondo de etiquetas).

---

## Texto en `<img src="...svg">` no es accesible

Si el SVG se usa como `<img>`, el texto no es seleccionable ni accesible directamente. Opciones:
1. Usar SVG inline para texto importante
2. Añadir `alt` en el `<img>` que describa el contenido del texto
3. Convertir el SVG a un elemento `<object>` (que sí permite acceso al DOM del SVG en algunos navegadores)
