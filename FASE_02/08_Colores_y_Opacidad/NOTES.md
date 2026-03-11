# Notas — Colores y Opacidad

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| Nombres de color CSS | ✓ | ✓ | ✓ | ✓ |
| `#RRGGBB`, `#RGB` | ✓ | ✓ | ✓ | ✓ |
| `rgb()`, `rgba()`, `hsl()`, `hsla()` | ✓ | ✓ | ✓ | ✓ |
| `#RRGGBBAA`, `#RGBA` (hex con alfa) | ✓ (66+) | ✓ (49+) | ✓ (10+) | ✓ (79+) |
| `rgb(R G B / a)` sin comas | ✓ (65+) | ✓ (52+) | ✓ (12.1+) | ✓ (79+) |
| `currentColor` | ✓ | ✓ | ✓ | ✓ |
| `opacity`, `fill-opacity`, `stroke-opacity` | ✓ | ✓ | ✓ | ✓ |
| `oklch()`, `oklab()` | ✓ (111+) | ✓ (113+) | ✓ (15.4+) | ✓ (111+) |
| Colores del sistema (`Canvas`, etc.) | ✓ | ✓ | ✓ | ✓ |

Para producción en navegadores actuales, todos los formatos son seguros. Para soporte de navegadores muy antiguos (IE11), usar `#RRGGBB` y `rgba()` (no hex con alfa).

---

## Elegir el formato correcto según el caso

```
Prototipar rápido    → nombres CSS (red, blue, tomato)
Herramientas/Figma   → hex (#RRGGBB) — lo que exportan
Paletas manuales     → hsl() — variar L para oscuro/claro
Con transparencia    → rgba() o hsla() — lo más compatible
Variables CSS        → var(--color) con cualquier formato
Dark mode adaptativo → colores del sistema (Canvas, etc.)
```

---

## Opacidad: regla de decisión

```
¿Quiero que fill Y stroke sean semitransparentes juntos?
  → opacity en el elemento

¿Quiero fill semitransparente pero stroke opaco?
  → fill-opacity

¿Quiero stroke semitransparente pero fill opaco?
  → stroke-opacity

¿Tengo formas solapadas y no quiero "doble opacidad"?
  → opacity en el grupo padre <g>

¿Quiero hacer fade-in/out con CSS animation?
  → opacity (más fácil de animar)
```

---

## Modo oscuro con colores del sistema

```svg
<svg>
  <rect fill="Canvas" stroke="CanvasText"/>
  <!-- Canvas = color de fondo del sistema (blanco/oscuro según preferencia) -->
  <!-- CanvasText = color de texto del sistema -->
</svg>
```

Los colores del sistema se adaptan automáticamente al modo claro/oscuro del SO. Útil para SVGs embebidos que deben respetar el tema del usuario sin media queries.

---

## Variables CSS para temas

```svg
<svg>
  <style>
    :root {
      --color-primary: #3b82f6;
      --color-surface: white;
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --color-primary: #93c5fd;
        --color-surface: #1e293b;
      }
    }
  </style>
  <rect fill="var(--color-surface)" stroke="var(--color-primary)"/>
</svg>
```

Las variables CSS dentro del `<style>` del SVG funcionan exactamente igual que en HTML. El SVG puede tener su propio `:root` o heredar las variables del documento HTML padre.

---

## currentColor + variables CSS para iconos multicolor

```svg
<!-- Icono con 2 colores configurables desde el contexto -->
<svg>
  <circle fill="var(--icon-bg, currentColor)" .../>
  <path fill="var(--icon-fg, white)" .../>
</svg>

<!-- Uso en HTML: -->
<div style="--icon-bg: #3b82f6; --icon-fg: white; color: #3b82f6;">
  <!-- el icono usa los colores del div -->
</div>
```

El patrón `var(--variable, fallback)` con `currentColor` como fallback garantiza comportamiento razonable si las variables no están definidas.

---

## Interpolación de colores en animaciones

Al animar `fill` o `stroke` con CSS transitions, el navegador interpola entre los dos colores. El resultado depende del espacio de color:

- `rgb()` → interpolación en espacio RGB (puede pasar por gris)
- `hsl()` → interpolación en espacio HSL (más "natural" visualmente)
- `oklch()` → interpolación perceptualmente uniforme (mejor para gradientes)

Para animaciones de color, `hsl()` suele dar mejores resultados visuales que `rgb()`.

---

## Depuración de opacidad inesperada

Si un elemento parece más transparente de lo esperado:
1. Revisar si algún padre tiene `opacity` definido
2. Verificar si hay `fill-opacity` o `stroke-opacity` además de `opacity`
3. Usar DevTools → panel Styles para ver qué propiedades se aplican y desde dónde se heredan

---

## Performance: `opacity` en grupos

Aplicar `opacity` a un `<g>` es una operación de composición: el navegador renderiza el grupo como una imagen y luego aplica la transparencia. Para grupos con muchos elementos complejos o filtros, esto puede ser costoso. En ese caso, considerar aplicar `fill-opacity` a los elementos individuales en lugar de `opacity` al grupo.
