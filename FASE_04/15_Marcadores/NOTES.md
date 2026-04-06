# Notas — Marcadores

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `<marker>` básico | ✓ | ✓ | ✓ | ✓ |
| `marker-start` / `marker-mid` / `marker-end` | ✓ | ✓ | ✓ | ✓ |
| Atajo `marker="url(#id)"` | ✓ | ✓ | ✓ | ✓ |
| `orient="auto"` | ✓ | ✓ | ✓ | ✓ |
| `orient="auto-start-reverse"` | ✓ | ✓ 60+ | ✓ 12.1+ | ✓ |
| `markerUnits="strokeWidth"` | ✓ | ✓ | ✓ | ✓ |
| `markerUnits="userSpaceOnUse"` | ✓ | ✓ | ✓ | ✓ |
| `fill="context-stroke"` | ✓ 97+ | ✓ 78+ | ✓ | ✓ |
| `fill="context-fill"` | ✓ 97+ | ✓ 78+ | ✓ | ✓ |
| `fill="currentColor"` | ✓ | ✓ | ✓ | ✓ |

Antes de 2020, `context-stroke` solo funcionaba en Safari. Ahora es seguro en navegadores modernos. Para soporte amplio heredado, `currentColor` sigue siendo la opción más compatible.

---

## Cómo calcular `refX` y `refY`

La regla mental: **¿qué punto del contenido del marker quiero que toque exactamente el vértice del path?**

### Flecha apuntando a la derecha

```
viewBox="0 0 10 10"
contenido: path d="M 0 0 L 10 5 L 0 10 z"  ← triángulo con pico en (10, 5)
refX="10" refY="5"  ← el pico es el extremo
```

### Círculo centrado en el vértice

```
viewBox="0 0 10 10"
contenido: circle cx="5" cy="5" r="4"
refX="5" refY="5"  ← el centro
```

### Cuadrado centrado en el vértice

```
viewBox="0 0 10 10"
contenido: rect x="0" y="0" width="10" height="10"
refX="5" refY="5"  ← el centro del rect
```

### Símbolo desplazado

```
Si quieres que el marker "flote" ligeramente por delante del vértice:
refX = extremo_del_marker - margen_deseado
```

---

## `markerUnits="strokeWidth"`: el cálculo real

Con `markerUnits="strokeWidth"` (el valor por defecto), las dimensiones del viewport se multiplican por el stroke del elemento:

```
tamaño_real_px = markerWidth × stroke-width
```

Ejemplo: `markerWidth="6"` en una línea con `stroke-width="2"` → marker de `12px` reales. En una línea con `stroke-width="4"` → marker de `24px`.

**Cuándo es útil**: gráficos donde quieres que la flecha siempre luzca proporcional al trazo.

**Cuándo es un problema**: cuando reutilizas el mismo marker en líneas de grosores muy distintos o cuando aplicas transformaciones de escala que alteran el `stroke-width` calculado.

---

## Herencia de color — la tabla

| Declaración en el marker | Comportamiento |
|---|---|
| `fill="#000"` | Color fijo, no hereda nunca |
| `fill="context-stroke"` | Toma el `stroke` del elemento padre (SVG 2) |
| `fill="context-fill"` | Toma el `fill` del elemento padre (SVG 2) |
| `fill="currentColor"` | Toma la propiedad CSS `color` del elemento padre |
| Sin `fill` (por defecto) | Usa el valor por defecto del elemento SVG: `fill="black"` |

**Recomendación moderna**: usar `context-stroke` en flechas. **Recomendación amplia**: `currentColor` con `color` CSS en el padre — funciona en todo.

---

## Patrones no obvios

### Acortar el path para que la flecha no asome

Otra alternativa a ajustar `refX`: acortar el path para que termine antes de donde empieza el triángulo. Útil cuando el stroke es muy grueso:

```svg
<!-- Línea original -->
<line x1="20" y1="50" x2="200" y2="50" stroke-width="8" marker-end="url(#arr)"/>

<!-- Acortada 10px: el end del trazo queda bajo el triángulo -->
<line x1="20" y1="50" x2="190" y2="50" stroke-width="8" marker-end="url(#arr)"/>
```

### Marker animado mediante CSS

Los elementos dentro de un marker pueden llevar animaciones SMIL o CSS. El marker "late" cada vez que se instancia:

```svg
<marker id="pulse" ...>
  <circle cx="5" cy="5" r="4" fill="red">
    <animate attributeName="r" values="4;6;4" dur="1s" repeatCount="indefinite"/>
  </circle>
</marker>
```

Todas las instancias del marker comparten la misma animación — cuidado con markers muy repetidos en paths grandes, puede ser costoso.

### Marker con gradiente

El contenido del marker admite gradientes definidos dentro del propio `<defs>`. Pero **los gradientes definidos fuera** pueden no funcionar en todos los navegadores, así que es más seguro definirlos dentro del SVG principal y referenciarlos con `url(#id)`.

### Distribuir símbolos a lo largo de una curva

`marker-mid` no te da puntos equidistantes en una curva. Para eso:

```js
const path = document.querySelector('path');
const len = path.getTotalLength();
const spacing = 20;
for (let d = 0; d <= len; d += spacing) {
  const pt = path.getPointAtLength(d);
  // coloca un <circle> en pt.x, pt.y
}
```

Alternativa sin JS: dividir el path en muchos segmentos pequeños y poner `marker-mid`. Feo pero funciona.

---

## Rendimiento

- **Markers muy repetidos** (polyline con 1000 vértices, cada uno con marker): coste medible. El navegador tiene que instanciar el contenido del marker en cada punto.
- **Markers animados**: multiplican el coste. Si el marker tiene un `feGaussianBlur` interno, cada instancia lo recalcula.
- **`markerUnits="userSpaceOnUse"`**: ligeramente más barato porque no tiene que consultar `stroke-width` del padre en cada instancia.
- **Reutilizar** un solo marker es **mucho más eficiente** que definir varios con pequeñas variaciones.

---

## Alternativas cuando los markers no bastan

### `<use>` en posiciones calculadas manualmente

Si necesitas control total sobre cada símbolo (color distinto, tamaño variable, animación independiente), usa `<use>` apuntando a un `<symbol>` y calcula las posiciones con JS o directamente en SVG. Es más verboso pero totalmente flexible.

### `stroke-dasharray` para "puntos" en la línea

Para un punteado simple, `stroke-dasharray` es más eficiente que markers individuales:

```svg
<line stroke-dasharray="4,6" .../>  <!-- 4px trazo, 6px gap -->
```

### Texto a lo largo del path con `<textPath>`

Si quieres emojis o caracteres como decoración a lo largo de una curva, un `<textPath>` con espaciado controlado puede hacerlo:

```svg
<text>
  <textPath href="#curva">• • • • • • • • •</textPath>
</text>
```

---

## Debugging

- **El marker no aparece**: comprobar que el `id` coincide, que está en `<defs>` (o al menos definido antes de ser referenciado), y que el atributo `marker-end` (o similar) está en la `<line>`/`<path>`, no en un `<g>` hijo.
- **La flecha apunta al lado incorrecto**: si es el `marker-start`, usar `orient="auto-start-reverse"`. Si es un `marker-end` invertido, reasignar `refX` al extremo derecho.
- **La flecha está demasiado lejos del vértice**: `refX` probablemente está mal. Visualiza el marker en un `<svg>` aparte de tamaño conocido para calibrar.
- **El marker sale del color equivocado**: agregar `fill="context-stroke"` dentro del marker.
- **El marker es enorme**: si `markerUnits="strokeWidth"` y el stroke es grueso (por ejemplo `stroke-width="10"`), el marker se multiplica por 10. Cambiar a `userSpaceOnUse`.

---

## Fragmentos útiles

### Flecha limpia estándar

```svg
<marker id="arrow" viewBox="0 0 10 10" refX="10" refY="5"
        markerWidth="6" markerHeight="6" orient="auto">
  <path d="M 0 0 L 10 5 L 0 10 z" fill="context-stroke"/>
</marker>
```

### Flecha bidireccional con un solo marker

```svg
<marker id="arr" viewBox="0 0 10 10" refX="10" refY="5"
        markerWidth="6" markerHeight="6" orient="auto-start-reverse">
  <path d="M 0 0 L 10 5 L 0 10 z" fill="context-stroke"/>
</marker>

<line ... marker-start="url(#arr)" marker-end="url(#arr)"/>
```

### Dot de tamaño fijo para gráficos

```svg
<marker id="dot" viewBox="0 0 10 10" refX="5" refY="5"
        markerWidth="6" markerHeight="6"
        markerUnits="userSpaceOnUse">
  <circle cx="5" cy="5" r="4" fill="white" stroke="context-stroke" stroke-width="2"/>
</marker>
```

### Flecha con "cola" (tipo dibujo técnico)

```svg
<marker id="tech" viewBox="0 0 14 10" refX="13" refY="5"
        markerWidth="10" markerHeight="7" orient="auto">
  <path d="M 0 0 L 14 5 L 0 10 L 4 5 z" fill="context-stroke"/>
</marker>
```
