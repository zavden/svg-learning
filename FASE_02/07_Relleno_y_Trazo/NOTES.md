# Notas — Relleno y Trazo

---

## Compatibilidad

| Propiedad | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `fill`, `stroke`, `stroke-width` | ✓ | ✓ | ✓ | ✓ |
| `stroke-linecap`, `stroke-linejoin` | ✓ | ✓ | ✓ | ✓ |
| `stroke-dasharray`, `stroke-dashoffset` | ✓ | ✓ | ✓ | ✓ |
| `paint-order` | ✓ | ✓ | ✓ | ✓ |
| `vector-effect="non-scaling-stroke"` | ✓ | ✓ | ✓ | ✓ (verificar versiones antiguas) |

Todas las propiedades de fill y stroke son ampliamente soportadas. `vector-effect` tiene soporte bueno pero puede tener comportamientos sutiles en transformaciones complejas (rotar + escalar) en Edge antiguo.

---

## El stroke siempre es centrado — nunca interior ni exterior

SVG no tiene propiedades nativas de `stroke-alignment` (a diferencia de Figma o Illustrator). El stroke siempre se centra en el borde geométrico.

Para simular stroke interior: usar `overflow="hidden"` en el SVG o aplicar `clipPath` con la misma forma.

Para simular stroke exterior: duplicar la forma debajo con `fill="none"` y `stroke-width` doble.

---

## Animación de line drawing — cómo obtener la longitud del path

La técnica requiere saber la longitud total del path:

```js
const path = document.querySelector('path');
const length = path.getTotalLength();
// Usar como valor de stroke-dasharray y stroke-dashoffset inicial
```

Para paths estáticos conocidos, se puede medir una vez y hardcodear el valor. Para paths dinámicos, calcularlo en JavaScript al cargar.

---

## `currentColor` — el "color heredado" del contexto

```svg
<!-- Si el SVG se embebe con color: tomato en el CSS padre... -->
<svg style="color: tomato;">
  <circle cx="50" cy="50" r="40" fill="currentColor"/>
  <!-- El círculo será tomato -->
</svg>
```

Muy útil para iconos SVG inline: el icono adopta automáticamente el color del texto del botón o enlace donde está.

---

## Variables CSS con fill y stroke

```svg
<svg>
  <style>
    :root { --brand: #3b82f6; }
    .icon { fill: var(--brand); }
  </style>
  <circle class="icon" cx="50" cy="50" r="40"/>
</svg>
```

Las variables CSS (`var()`) funcionan con fill y stroke. Esto permite temas (dark mode, brand colors) sin cambiar el SVG.

---

## opacity vs fill-opacity vs stroke-opacity

```
opacity: 0.5          → todo el elemento al 50%: fill + stroke + hijos
fill-opacity: 0.5     → solo el relleno al 50%
stroke-opacity: 0.5   → solo el trazo al 50%
```

Si hay un `<g>` con `opacity: 0.5` que contiene formas, las formas se renderizan completamente y luego se aplica la opacidad al grupo como imagen compuesta. Esto evita que los solapamientos dentro del grupo sean "dobles".

---

## Truco: puntos con stroke-dasharray

```svg
<line x1="10" y1="50" x2="290" y2="50"
      stroke="#3b82f6" stroke-width="6"
      stroke-dasharray="0,12"
      stroke-linecap="round"/>
```

`stroke-dasharray="0,12"` con `stroke-linecap="round"`: trazo de longitud 0 (un punto) seguido de un espacio de 12. El linecap `round` convierte el punto en un círculo de diámetro igual a `stroke-width`. El espacio efectivo entre centros es 12.

---

## stroke-width="0" vs stroke="none"

Ambos ocultan el trazo, pero:
- `stroke="none"`: la propiedad `stroke` no está definida (no se pinta nada)
- `stroke-width="0"`: hay un stroke de color definido pero de grosor cero

Para eliminar el trazo limpiamente, prefer `stroke="none"`. Para animaciones donde el trazo puede crecer de 0 a un valor, usar `stroke-width` (más fácil de animar con CSS).

---

## paint-order — forma abreviada

```svg
<!-- Equivalentes: -->
paint-order="stroke fill markers"
paint-order="stroke fill"
paint-order="stroke"
<!-- Los elementos no mencionados mantienen el orden relativo predeterminado -->
```

La forma más compacta habitual es `paint-order="stroke fill"`.

---

## vector-effect no se hereda

A diferencia de `fill` o `stroke`, `vector-effect="non-scaling-stroke"` **no se hereda** de padres a hijos. Debe especificarse en cada elemento que lo necesite.

```svg
<!-- MAL: solo el <g> lo tiene, los hijos NO lo heredan -->
<g vector-effect="non-scaling-stroke">
  <line .../> <!-- sin non-scaling-stroke -->
</g>

<!-- BIEN: en cada elemento -->
<g>
  <line vector-effect="non-scaling-stroke" .../>
  <rect vector-effect="non-scaling-stroke" .../>
</g>
```

---

## stroke-miterlimit — cuándo ajustarlo

El valor por defecto (`4`) funciona para la mayoría de casos. Solo ajustar si:
- Hay polilíneas con ángulos muy agudos (< 29°) y se quieren puntas afiladas → aumentar el valor
- Se quieren esquinas siempre achaflanadas → bajar a `1`
- Se está diseñando para impresión técnica donde las puntas largas son un problema

---

## Depuración visual rápida

Para ver el bounding box de elementos mientras se depura:
```svg
<style>
  * { outline: 1px solid red; }      /* CSS outline */
  /* o añadir temporalmente: */
  rect { fill: rgba(255,0,0,0.1); }
</style>
```

Para ver dónde cae exactamente el stroke, añadir una línea guía fina en el borde geométrico del elemento.
