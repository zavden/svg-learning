# Notas — Transformaciones

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `transform` (translate, rotate, scale, skew, matrix) | ✓ | ✓ | ✓ | ✓ |
| Encadenamiento de transformaciones | ✓ | ✓ | ✓ | ✓ |
| `transform-origin` en SVG inline | ✓ | ✓ | ✓ | ✓ |
| `transform-origin` en SVG externo (.svg) | Parcial | Parcial | Parcial | Parcial |
| CSS `transform` en elementos SVG | ✓ (55+) | ✓ | ✓ (14+) | ✓ |

El atributo `transform` con las funciones SVG tiene soporte universal. `transform-origin` con CSS funciona bien en SVG inline en HTML moderno.

---

## CSS `transform` vs atributo `transform` en SVG

Desde CSS Transforms Level 2, los elementos SVG también aceptan `style="transform: rotate(45deg)"` (con unidades CSS como `deg`). El atributo `transform="rotate(45)"` usa grados sin unidad.

```svg
<!-- Los dos son equivalentes para SVG inline en HTML moderno: -->
<rect transform="rotate(45, 150, 75)"/>
<rect style="transform: rotate(45deg); transform-origin: 150px 75px;"/>
```

El atributo SVG es más compatible en SVGs standalone (archivos .svg). El CSS es más potente para animaciones y estados.

---

## Animaciones con CSS transitions/animations

Las transformaciones SVG se pueden animar con CSS:

```css
.rotating {
  transform-origin: 50% 50%; /* solo si es SVG inline en HTML */
  animation: spin 2s linear infinite;
}
@keyframes spin {
  to { transform: rotate(360deg); }
}
```

Para `transform-origin: 50% 50%` en SVG inline en HTML funciona como se espera (centro del elemento). Para SVG standalone, usar coordenadas absolutas en px.

---

## Animaciones con SMIL `animateTransform`

Para SVGs standalone o donde CSS no es suficiente, SMIL ofrece `<animateTransform>`:

```svg
<rect x="40" y="40" width="20" height="20">
  <animateTransform attributeName="transform"
                    type="rotate"
                    from="0 50 50" to="360 50 50"
                    dur="2s" repeatCount="indefinite"/>
</rect>
```

Soporte universal en navegadores. Más verboso que CSS pero funciona en SVG standalone.

---

## Grupos como contenedores de transformación

El patrón más limpio para posicionar y transformar conjuntos de elementos:

```svg
<!-- Todos los elementos del logo en coordenadas locales (0,0 como centro) -->
<g id="logo" transform="translate(150, 75)">
  <circle cx="0" cy="0" r="30" fill="blue"/>
  <text x="0" y="5" text-anchor="middle">LOGO</text>
</g>
```

Ventaja: para mover el logo a otra posición, solo cambia el `translate`. Las coordenadas internas permanecen iguales.

---

## Calcular el centro de un bounding box con JavaScript

```js
const bbox = element.getBBox();
const cx = bbox.x + bbox.width / 2;
const cy = bbox.y + bbox.height / 2;
element.setAttribute('transform', `rotate(45, ${cx}, ${cy})`);
```

`getBBox()` devuelve el bounding box del elemento en su espacio de coordenadas local, sin incluir las transformaciones del padre.

---

## `vector-effect="non-scaling-stroke"` con transformaciones

Cuando se usa `scale()`, el `stroke-width` también se escala. Para mantener el grosor del trazo constante:

```svg
<rect transform="scale(3)" vector-effect="non-scaling-stroke"
      stroke="blue" stroke-width="1"/>
```

El trazo siempre se verá de 1px visual, independientemente de la escala aplicada. No se hereda a los hijos.

---

## Transformaciones para crear elementos repetidos radialmente

El patrón de `rotate(n * (360/total), cx, cy)` es muy útil para crear círculos de pétalos, marcas de reloj, radios de rueda, etc.:

```svg
<!-- 8 elementos distribuidos en círculo -->
<g>
  <!-- Crear con loop en JS: -->
  <!-- for (let i = 0; i < 8; i++) { -->
  <!--   angle = i * 45; -->
  <!--   transform="rotate(angle, cx, cy)" -->
  <!-- } -->
</g>
```

En SVG estático, repetir el elemento con distintos ángulos manualmente o generarlo con JS/template.
