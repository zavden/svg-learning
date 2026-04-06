# NOTES — Interactividad con JavaScript

Detalles prácticos, trampas poco documentadas y trucos que complementan lo explicado en los THEORY.

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `createElementNS` | ✓ | ✓ | ✓ | ✓ |
| `querySelector` en SVG | ✓ | ✓ | ✓ | ✓ |
| `classList` en SVG | ✓ | ✓ | ✓ | ✓ |
| Pointer Events | ✓ | ✓ (60+) | ✓ (13+) | ✓ |
| `setPointerCapture` | ✓ | ✓ | ✓ | ✓ |
| `DOMPoint` / `DOMMatrix` | ✓ | ✓ | ✓ | ✓ |
| `getScreenCTM` | ✓ | ✓ | ✓ | ✓ |
| `getTotalLength` / `getPointAtLength` | ✓ | ✓ | ✓ | ✓ |
| `isPointInFill` / `isPointInStroke` | ✓ | ✓ | ✓ | ✓ |

En la práctica: todo lo de esta sección funciona en cualquier navegador de 2020 en adelante.

---

## `svgEl`, `svgCoords`, `makeDraggable` — el kit mínimo

Estos tres helpers cubren el 80% del código de interactividad SVG:

```js
const NS = 'http://www.w3.org/2000/svg';

function svgEl(tag, attrs = {}) {
  const el = document.createElementNS(NS, tag);
  for (const [k, v] of Object.entries(attrs)) el.setAttribute(k, v);
  return el;
}

function svgCoords(evt, svg) {
  const pt = new DOMPoint(evt.clientX, evt.clientY);
  return pt.matrixTransform(svg.getScreenCTM().inverse());
}

function makeDraggable(el, svg, { xAttr = 'x', yAttr = 'y' } = {}) {
  let ox, oy;
  el.addEventListener('pointerdown', e => {
    el.setPointerCapture(e.pointerId);
    const p = svgCoords(e, svg);
    ox = p.x - parseFloat(el.getAttribute(xAttr));
    oy = p.y - parseFloat(el.getAttribute(yAttr));
  });
  el.addEventListener('pointermove', e => {
    if (!el.hasPointerCapture(e.pointerId)) return;
    const p = svgCoords(e, svg);
    el.setAttribute(xAttr, p.x - ox);
    el.setAttribute(yAttr, p.y - oy);
  });
}
```

Copia estos tres en cualquier proyecto y ya tienes base suficiente para editores SVG sencillos.

---

## Por qué `getBBox()` a veces devuelve `0,0,0,0`

`getBBox()` solo funciona cuando el elemento está **renderizado**. Si llamas a `getBBox` antes de que el elemento se añada al DOM (o mientras está dentro de un `<defs>`, o con `display: none`), devolverá ceros.

**Soluciones**:
- Añadir el elemento al DOM primero, llamar `getBBox` después.
- Usar `getBoundingClientRect()` si quieres coordenadas de pantalla (funciona incluso fuera del flujo visible de SVG).
- Para texto: forzar una medida después de asignar `textContent`.

---

## `getBoundingClientRect()` vs `getBBox()`

- **`getBBox()`**: caja en coordenadas **del propio SVG** (sin tener en cuenta transformaciones del padre).
- **`getBoundingClientRect()`**: caja en coordenadas **de pantalla** (pixels CSS). Considera zoom, scroll, transformaciones CSS y SVG.

Regla práctica:
- ¿Quieres centrar algo dentro del mismo SVG? → `getBBox()`
- ¿Quieres posicionar un tooltip HTML flotante encima del elemento? → `getBoundingClientRect()`

---

## Animaciones: ¿`setAttribute` o librería?

Cambiar un atributo con `setAttribute` dentro de `requestAnimationFrame` funciona bien para animaciones simples, pero:

- Para animaciones complejas, encadenadas o con timing fino → **GSAP**, **Motion One**, **Anime.js**.
- Para rendering masivo o actualizaciones frecuentes → **D3.js** o **custom con requestAnimationFrame**.
- Para 100+ elementos animados simultáneamente → considera reducir a CSS transform en un grupo padre.

---

## Modificar el viewBox = zoom y pan

Cambiar el atributo `viewBox` del SVG raíz es la forma idiomática de implementar zoom y pan:

```js
function zoomTo(x, y, scale) {
  const w = baseWidth / scale;
  const h = baseHeight / scale;
  svg.setAttribute('viewBox', `${x - w/2} ${y - h/2} ${w} ${h}`);
}
```

**Ventajas**: una sola mutación mueve/escala todo el contenido, el navegador optimiza, y los eventos de mouse se ajustan automáticamente gracias a `getScreenCTM`.

**Detalle**: si animas el viewBox, `transition: viewBox 0.3s` no funciona en Chrome/Firefox. Hay que animarlo con JS o con SMIL (`<animate attributeName="viewBox"/>`).

---

## `requestAnimationFrame` para redibujo eficiente

Si un evento `pointermove` se dispara muy rápido, actualizar el DOM en cada uno puede ser más frecuente que el refresh del monitor. Mejor es acumular el último valor y actualizar en el próximo `rAF`:

```js
let pending = null;
svg.addEventListener('pointermove', e => {
  pending = svgCoords(e, svg);
  if (!rAFid) rAFid = requestAnimationFrame(flush);
});
function flush() {
  rAFid = null;
  if (!pending) return;
  cursor.setAttribute('cx', pending.x);
  cursor.setAttribute('cy', pending.y);
  pending = null;
}
```

Para edición de muchos puntos o interacciones fluidas, esto marca la diferencia.

---

## Sanitización con DOMPurify

```js
import DOMPurify from 'dompurify';
const svgSeguro = DOMPurify.sanitize(svgDeUsuario, { USE_PROFILES: { svg: true, svgFilters: true } });
container.innerHTML = svgSeguro;
```

DOMPurify con el profile `svg` elimina automáticamente `<script>`, `onclick`, `javascript:` URIs y otros vectores. **Siempre** úsalo antes de inyectar inline un SVG que venga de una fuente no controlada.

---

## No mezcles `.style` con atributos presentación para la misma propiedad

Recuerda la regla de cascada de la Sección 19: una regla CSS (incluido `.style` inline) **siempre gana** sobre un atributo de presentación. Esto es contraintuitivo cuando esperarías lo contrario.

```js
// si el SVG tiene <circle fill="blue"/>
circle.style.fill = 'red';    // gana → rojo
circle.setAttribute('fill', 'green'); // NO cambia nada visible → sigue rojo
```

Para volver al color original hay que hacer `circle.style.fill = ''` (limpiar el estilo inline), no cambiar el atributo.

---

## `<use>` y JavaScript: la frontera del shadow DOM

Los elementos dentro de un `<use>` viven en un shadow DOM invisible. Consecuencias:

- `document.querySelector('#dentroDelUse')` no los encuentra.
- No puedes poner listeners a elementos individuales dentro de un `<use>`.
- El click hace target al propio `<use>`, no al elemento interno.
- Workaround: si necesitas interacción granular, renderiza con `cloneNode(true)` en vez de `<use>`.

---

## Hit test: `elementFromPoint` también funciona con SVG

```js
const el = document.elementFromPoint(clientX, clientY);
if (el?.tagName === 'circle') { /* ... */ }
```

Devuelve el elemento SVG más "arriba" en el z-order visual en esas coordenadas de pantalla. Alternativa a `isPointInFill` cuando quieres saber "qué hay debajo del cursor" sin pasar por eventos.

---

## Resumen práctico

- **Crear elementos**: `createElementNS` con el namespace SVG, nunca `createElement`.
- **Leer atributos numéricos**: `parseFloat(el.getAttribute(...))` o `el.cx.baseVal.value`.
- **Coordenadas del mouse**: `svgCoords(e, svg)` con `DOMPoint` + `getScreenCTM().inverse()`.
- **Drag and drop**: `pointerdown` + `setPointerCapture` + `pointermove` + offset calculado al inicio.
- **Muchos elementos**: delegación de eventos en el SVG padre.
- **Scripts en `.svg` autónomos**: envuélvelos en `<![CDATA[ ]]>`.
- **SVG de terceros inline**: sanear con DOMPurify sí o sí.
