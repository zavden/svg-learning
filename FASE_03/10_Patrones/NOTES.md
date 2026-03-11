# Notas — Patrones

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `<pattern>` básico | ✓ | ✓ | ✓ | ✓ |
| `patternUnits`, `patternContentUnits` | ✓ | ✓ | ✓ | ✓ |
| `patternTransform` | ✓ | ✓ | ✓ | ✓ |
| `viewBox` en pattern | ✓ | ✓ | ✓ | ✓ |
| Animación de `patternTransform` con CSS | Parcial | Parcial | Parcial | Parcial |

Los patrones tienen soporte universal en navegadores modernos. La animación de `patternTransform` con CSS tiene soporte inconsistente; para animaciones de textura, SMIL o JavaScript son más fiables.

---

## Regla práctica: `userSpaceOnUse` para todo

La combinación más predecible y menos propensa a errores:

```svg
<pattern
  width="20" height="20"
  patternUnits="userSpaceOnUse"
  patternContentUnits="userSpaceOnUse">
  <!-- contenido con coordenadas normales -->
</pattern>
```

Con esta combinación, todo usa el mismo sistema de coordenadas absoluto. El número de repeticiones varía con el tamaño del elemento, pero no hay conversiones mentales de escala.

---

## Patrón de checker (tablero) para debug de transparencia

Un patrón checker es la forma estándar de visualizar áreas transparentes:

```svg
<defs>
  <pattern id="checker" width="10" height="10" patternUnits="userSpaceOnUse">
    <rect width="5" height="5" fill="#e2e8f0"/>
    <rect x="5" y="5" width="5" height="5" fill="#e2e8f0"/>
  </pattern>
</defs>
<!-- Fondo checker visible detrás de elementos transparentes -->
<rect width="100%" height="100%" fill="url(#checker)"/>
```

Útil en herramientas de diseño, previsualizadores de SVG y demos de `stop-opacity`.

---

## Patrón con gradiente en el contenido

El contenido de un patrón puede referenciar gradientes definidos en `<defs>`:

```svg
<defs>
  <linearGradient id="g">
    <stop offset="0%" stop-color="#3b82f6"/>
    <stop offset="100%" stop-color="#8b5cf6"/>
  </linearGradient>
  <pattern id="grad-stripe" width="40" height="40" patternUnits="userSpaceOnUse">
    <rect x="0" y="0" width="40" height="20" fill="url(#g)"/>
  </pattern>
</defs>
```

Permite texturas con rayas de color gradiente, fondos de malla compleja, etc.

---

## Performance: patrones complejos y anidados

Los patrones se recalculan en cada frame de animación y por cada elemento que los usa. Para SVGs con muchos elementos con patrón:

- Preferir patrones simples (1–3 elementos dentro)
- Evitar patrones anidados (patrón dentro de patrón) en elementos críticos de rendimiento
- Para fondos grandes, considerar `<image>` con una textura rasterizada

---

## Patrón animado con SMIL

Para animar el desplazamiento de un patrón (efecto de textura en movimiento):

```svg
<pattern id="moving" width="20" height="20" patternUnits="userSpaceOnUse" x="0">
  <animateTransform attributeName="patternTransform"
                    type="translate"
                    from="0 0" to="20 0"
                    dur="1s" repeatCount="indefinite"/>
  <circle cx="10" cy="10" r="7" fill="#3b82f6"/>
</pattern>
```

Con `type="translate"` y `to="20 0"` (igual al `width`), la textura parece desplazarse indefinidamente sin saltos.

---

## Texto dentro de un patrón

El contenido de un patrón puede incluir `<text>`. Útil para marcas de agua:

```svg
<pattern id="watermark" width="120" height="40" patternUnits="userSpaceOnUse"
         patternTransform="rotate(-30)">
  <text x="0" y="25" font-size="12" fill="#94a3b8" fill-opacity="0.4">
    BORRADOR
  </text>
</pattern>
```

El patrón repite el texto en diagonal sobre el área.

---

## Usar `fill-rule` con patrones

En paths con subpaths (agujeros), `fill-rule="evenodd"` hace que el patrón no aparezca en las zonas "hueco". Esto es el comportamiento esperado: el patrón solo rellena las zonas que "están dentro" según la regla.
