# Notas — Estilado de SVG con CSS

---

## Compatibilidad

- **Atributos de presentación**: universales desde IE9.
- **Bloque `<style>` dentro de SVG**: universal desde IE9. CSS moderno (variables, `calc()`, anidación) según soporte general del navegador.
- **Variables CSS en SVG**: Chrome 49+, Firefox 31+, Safari 9.1+, todos los navegadores modernos.
- **`currentColor`**: universal desde IE9. Es uno de los rasgos más antiguos y mejor soportados.
- **CSS `transform` en SVG**: Chrome 36+, Firefox 55+, Safari 9.1+. Antes solo funcionaba el atributo `transform=""`.
- **`mix-blend-mode` en SVG**: Chrome 41+, Firefox 32+, Safari 8+. Necesita un contexto de mezcla (a veces `isolation: isolate`).
- **`vector-effect: non-scaling-stroke`**: universal desde Chrome 7, Firefox 15, Safari 6.
- **CSS `clip-path: url(#id)`**: universal. La función inline (`clip-path: circle(50%)`) tiene soporte algo más reciente.
- **CSS animation `@keyframes` en SVG**: universal. Lo que cambia es **qué propiedades** son animables (ver más abajo).

---

## Propiedades animables vía CSS en SVG

Buena parte de las propiedades SVG son animables con `transition` y `@keyframes`:

- `fill`, `stroke`, `stroke-width`, `stroke-opacity`, `fill-opacity`, `opacity`
- `stroke-dasharray`, `stroke-dashoffset` (la base del efecto "dibujar")
- `transform` (rotate, scale, translate, skew)
- `r` (radio de `<circle>`) — Chrome 65+, no en todos los navegadores antiguos
- `cx`, `cy`, `x`, `y` — soporte parcial
- `d` del `<path>` — Chrome 80+, Firefox 79+

**Truco**: si una propiedad geométrica no se anima en CSS, anímala con SMIL (`<animate>`) o cambia el `transform` en su lugar.

---

## El problema del `transform-origin`

En SVG, **`transform-origin` por defecto es `0 0`**, no el centro como en HTML. Esto rompe la intuición de cualquiera que venga de CSS de HTML:

```css
/* En HTML: rota desde el centro */
.elem { transform: rotate(45deg); }

/* En SVG: rota desde la esquina del sistema de coordenadas */
.elem-svg { transform: rotate(45deg); }  /* el círculo "se va volando" */
```

**Soluciones**:

1. Especifica las coordenadas exactas del SVG:
   ```css
   .elem-svg { transform-origin: 50px 50px; }
   ```

2. Usa keywords si el elemento ocupa el viewBox completo:
   ```css
   .elem-svg { transform-origin: center; }
   ```

3. Envuelve en un `<g transform="translate(50 50)">` y rota desde 0 0 (la solución vieja).

---

## Cuándo usar `style="..."` y cuándo NO

**Sí** para:
- Valores que dependen de `var()` o `calc()` y no quieres declarar reglas CSS.
- Estilos que NO deben poder sobrescribirse desde fuera (caso raro).
- Asignar variables CSS por instancia: `<use style="--c1: red;">`.

**No** para:
- Defaults de un SVG exportado de un editor (rompe la tematización).
- Valores estáticos que valdría usar como atributo de presentación.
- Cualquier estilo que un consumidor del SVG quiera personalizar.

---

## SVG inline vs externo: qué CSS funciona

| Método | CSS externo del padre | `<style>` interno | `style=""` inline |
|---|---|---|---|
| Inline en HTML | sí | sí | sí |
| `<img src="x.svg">` | **no** | sí | sí |
| `background-image` | **no** | sí | sí |
| `<object>` | **no** | sí | sí |
| `<iframe>` | **no** | sí | sí |
| Data URI | **no** | sí | sí |

**Conclusión**: solo el SVG inline recibe CSS del documento padre. Si necesitas estilizar desde fuera, **inlinéalo** o pásate a variables CSS pasadas vía `style=""` (no funciona en `<img>`) o atributos del elemento contenedor (`fill="..."` heredable en `<object>` no aplica).

---

## El selector `:root` dentro de `<svg>`

Dentro del bloque `<style>` de un SVG, `:root` selecciona el propio elemento `<svg>`, no el `<html>` del documento. Útil para definir variables locales al SVG sin colisionar con el resto:

```svg
<svg viewBox="0 0 100 100">
  <style>
    :root { --color-marca: blue; }
  </style>
  <circle fill="var(--color-marca)"/>
</svg>
```

Si el SVG es **inline en HTML**, las variables del `:root` del HTML también son accesibles porque la herencia funciona:

```css
/* HTML */
:root { --color-marca: red; }
```

```svg
<svg><circle fill="var(--color-marca)"/></svg>  <!-- usa el rojo del HTML -->
```

---

## El truco del `pointer-events: none`

Por defecto, **todas** las partes visibles de un SVG reciben eventos. Esto puede arruinar tu UI cuando una decoración intercepta clicks pensados para algo debajo:

```css
.decoracion { pointer-events: none; }   /* ignora clicks completamente */
.boton-vacio { pointer-events: stroke; } /* solo el borde es clickeable */
.area-completa { pointer-events: bounding-box; } /* el rectángulo entero */
```

`bounding-box` es un valor SVG-2 con soporte aún parcial pero útil.

---

## Patrones de tematización

### Patrón 1: API por variables CSS

Define las "propiedades públicas" del SVG con valores fallback:

```svg
<svg viewBox="0 0 24 24">
  <path
    fill="var(--icono-color, currentColor)"
    stroke="var(--icono-trazo, none)"
    stroke-width="var(--icono-grosor, 0)"
    d="..."/>
</svg>
```

### Patrón 2: Modo oscuro automático

```svg
<svg viewBox="0 0 100 100">
  <style>
    :root { --bg: white; --fg: #1e293b; }
    @media (prefers-color-scheme: dark) {
      :root { --bg: #1e293b; --fg: white; }
    }
    rect { fill: var(--bg); }
    text { fill: var(--fg); }
  </style>
</svg>
```

### Patrón 3: Estados con clases en el padre

```css
/* HTML */
.boton svg { fill: gray; }
.boton.activo svg { fill: blue; }
.boton:hover svg { fill: lightblue; }
```

```html
<button class="boton">
  <svg><path d="..."/></svg>
</button>
```

---

## Debugging

- **"Mi CSS no aplica"**: probablemente el SVG está cargado con `<img>` o `background-image`. El CSS del documento no entra.
- **"El elemento `transform: rotate()` se va de la pantalla"**: te falta `transform-origin`.
- **"Los selectores `use circle` no funcionan"**: shadow DOM. Usa `currentColor` o variables CSS.
- **"Mi `style="fill: red"` no se ve sobrescrito por la regla CSS"**: el inline gana al CSS externo. Conviértelo a atributo de presentación.
- **"El SVG se ve perfecto en HTML pero pierde colores en `<img>`"**: probablemente uses `currentColor`, que necesita un `color` heredado del padre. En `<img>` no hay padre. Hardcodea el color o usa variables CSS pasadas vía data URI.
- **"Animación CSS no se reproduce"**: comprueba que la propiedad sea animable en el navegador objetivo. `r`, `cx`, `cy` aún no funcionan en todos.
- **"El CSS del bloque `<style>` no aplica al abrir el .svg directamente"**: falta `<![CDATA[]]>` o el archivo se está sirviendo como texto plano (mira el `Content-Type`).

---

## Herramientas

- **SVGO**: optimizador. Configurable para preservar atributos de presentación, eliminar `style=""` innecesarios, mantener IDs, etc.
- **SVGOMG**: GUI web de SVGO con preview.
- **Figma**: en exportación, marca "Use presentation attributes" para evitar `style=""` inline.
- **DevTools del navegador**: el panel "Computed" muestra qué fuente ganó cada propiedad en SVG igual que en HTML.
- **Inspector de cascada**: Firefox tiene uno especialmente útil para ver qué reglas se aplicaron y cuáles fueron descartadas.
