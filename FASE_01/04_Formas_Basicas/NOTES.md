# Notas y consejos — Formas Básicas

---

## Compatibilidad de formas básicas

Todas las formas primitivas tienen soporte universal en navegadores modernos y han sido estables desde SVG 1.1 (2003).

| Forma | Chrome | Firefox | Safari | Edge |
|-------|--------|---------|--------|------|
| `<rect>` | ✓ | ✓ | ✓ | ✓ |
| `<circle>` | ✓ | ✓ | ✓ | ✓ |
| `<ellipse>` | ✓ | ✓ | ✓ | ✓ |
| `<line>` | ✓ | ✓ | ✓ | ✓ |
| `<polyline>` | ✓ | ✓ | ✓ | ✓ |
| `<polygon>` | ✓ | ✓ | ✓ | ✓ |
| `stroke-dasharray` | ✓ | ✓ | ✓ | ✓ |
| `stroke-linecap` | ✓ | ✓ | ✓ | ✓ |

---

## El stroke se centra sobre el borde geométrico

**Siempre.** No hay forma de cambiarlo solo con atributos SVG básicos.

Un `stroke-width="10"` se reparte 5px hacia dentro y 5px hacia fuera del borde declarado.

**Consecuencias prácticas:**
- Un rectángulo `x=0, y=0` con `stroke-width=10` tendrá 5px recortados en los bordes del `viewBox`.
- Solución: añadir padding de `stroke-width / 2` al posicionar elementos en los bordes.
- Alternativa avanzada: `vector-effect="non-scaling-stroke"` (útil cuando se escala el SVG).

---

## `currentColor` — enlace con el CSS exterior

`fill="currentColor"` o `stroke="currentColor"` toma el valor de la propiedad CSS `color` del elemento padre (o del propio elemento si se define vía CSS). Ideal para iconos SVG inline que deben adaptarse al color del texto.

```
<!-- En HTML -->
<button style="color: #3b82f6;">
  <svg ...>
    <path fill="currentColor" d="..."/>   <!-- hereda el azul del botón -->
  </svg>
</button>
```

---

## Polígonos regulares: generación programática

Los puntos de polígonos regulares se calculan mejor con JavaScript o un generador:

```
// n lados, radio r, centro (cx, cy)
function regularPolygon(n, r, cx = 0, cy = 0) {
  return Array.from({ length: n }, (_, i) => {
    const angle = (2 * Math.PI * i / n) - Math.PI / 2;
    return `${cx + r * Math.cos(angle)},${cy + r * Math.sin(angle)}`;
  }).join(' ');
}

// Hexágono en (150, 100) radio 50
regularPolygon(6, 50, 150, 100)
// → "150,50 193.3,75 193.3,125 150,150 106.7,125 106.7,75"
```

---

## `stroke-dashoffset` — animar trazos

`stroke-dashoffset` controla el punto de inicio del patrón de guiones. Combinado con `stroke-dasharray`, permite la técnica de "dibujar" un trazo progresivamente con CSS animation:

```
/* Truco: dasharray = longitud del path, offset animado de longitud→0 */
.draw-animation {
  stroke-dasharray: 500;
  stroke-dashoffset: 500;
  animation: draw 2s ease forwards;
}
@keyframes draw {
  to { stroke-dashoffset: 0; }
}
```

---

## Herramienta: generadores online

- **SVG Path Editor** (yqnn.github.io/svg-path-editor) — para paths
- **Boxy SVG** (boxy-svg.com) — editor visual SVG
- **SVGOMG** (jakearchibald.github.io/svgomg) — optimizador SVG
- **Heroicons / Phosphor** — librerías de iconos SVG abiertos

---

## Diferencias entre `opacity`, `fill-opacity` y `stroke-opacity`

| Propiedad | Afecta | Se acumula con grupos |
|-----------|--------|-----------------------|
| `opacity` | fill + stroke + hijos | Sí, multiplicando |
| `fill-opacity` | solo el relleno | No directamente |
| `stroke-opacity` | solo el trazo | No directamente |

**Regla práctica:** usa `fill-opacity` / `stroke-opacity` para controlar relleno y trazo de forma independiente. Usa `opacity` solo cuando quieras tratar el elemento completo como una unidad semitransparente.

---

## `shape-rendering` — calidad vs rendimiento

El atributo `shape-rendering` controla el antialiasing:

- `auto` — el navegador decide (por defecto)
- `crispEdges` — bordes nítidos sin antialiasing (ideal para píxeles exactos, grids)
- `geometricPrecision` — máxima precisión geométrica
- `optimizeSpeed` — prioriza velocidad sobre calidad

```svg
<!-- Cuadrícula de píxeles nítida -->
<rect x="10" y="10" width="20" height="20"
      fill="#3b82f6" shape-rendering="crispEdges"/>
```

---

## Debugging rápido: hacer visible lo invisible

Técnica para depurar SVGs con elementos mal posicionados:

```svg
<!-- Añadir temporalmente un fondo rojo al viewBox completo -->
<rect x="0" y="0" width="100%" height="100%" fill="rgba(255,0,0,0.1)"/>
<!-- Marcar el origen -->
<circle cx="0" cy="0" r="5" fill="red"/>
```
