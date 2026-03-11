# Notas — Gradientes

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `linearGradient`, `radialGradient` | ✓ | ✓ | ✓ | ✓ |
| `stop-color`, `stop-opacity` | ✓ | ✓ | ✓ | ✓ |
| `gradientUnits`, `spreadMethod` | ✓ | ✓ | ✓ | ✓ |
| `gradientTransform` | ✓ | ✓ | ✓ | ✓ |
| `fx`, `fy` (punto focal) | ✓ | ✓ | ✓ | ✓ |
| `href` (herencia) | ✓ | ✓ | ✓ | ✓ |
| `fr` (radio focal, SVG 2) | Parcial | Parcial | No | Parcial |

Los gradientes básicos tienen soporte universal. `fr` (radio focal) es SVG 2 y tiene soporte limitado — evitar en producción.

---

## Gradiente como sombra interior (simular)

SVG no tiene `box-shadow`. Para simular sombra interior, superponer un gradiente encima de la forma:

```svg
<!-- Forma base -->
<rect id="card" x="10" y="10" width="200" height="120" rx="8" fill="#3b82f6"/>

<!-- Sombra superior (overlay) -->
<rect x="10" y="10" width="200" height="120" rx="8"
      fill="url(#inner-shadow)" pointer-events="none"/>
```

Con el gradiente yendo de negro semitransparente (arriba) a transparente (abajo).

---

## `href` vs `xlink:href`

En SVG 1.1, la herencia de gradientes usaba `xlink:href`. En SVG 2 (navegadores modernos), se usa simplemente `href`. Para máxima compatibilidad con código antiguo o herramientas que generan SVG 1.1:

```svg
<!-- SVG 1.1 (legacy) -->
<linearGradient id="child" xlink:href="#parent" .../>

<!-- SVG 2 (moderno) -->
<linearGradient id="child" href="#parent" .../>
```

Los navegadores modernos aceptan ambos. `xlink:href` está obsoleto pero aún funciona.

---

## Gradiente animado con CSS

Animar los `stop-color` o `stop-opacity` directamente con CSS es posible pero con soporte limitado. La técnica más compatible: animar `gradientTransform` para hacer rotar o mover el gradiente.

```svg
<style>
  #animated-grad {
    animation: spin 3s linear infinite;
  }
  @keyframes spin {
    to { gradientTransform: rotate(360, 0.5, 0.5); }
  }
</style>
```

Para animar colores de stop, usar SMIL o JavaScript (modificar directamente los atributos del DOM).

---

## Gradientes en SVG embebido en HTML — herencia de currentColor

Los stops pueden usar `currentColor` como valor de `stop-color`. Esto permite que el gradiente cambie automáticamente según el color del contexto HTML:

```svg
<linearGradient id="dynamic">
  <stop offset="0%" stop-color="currentColor" stop-opacity="1"/>
  <stop offset="100%" stop-color="currentColor" stop-opacity="0"/>
</linearGradient>
```

El gradiente va del color de texto actual a transparente. Útil para overlays adaptativos.

---

## Gradiente en icono SVG reutilizable — problema de id

Si se inline el mismo SVG de icono múltiples veces en HTML, los `id` de los gradientes se duplican en el DOM. El resultado es que todos los iconos referencian el mismo gradiente (el del primero).

Soluciones:
1. Usar `linearGradient` dentro de `<defs>` en cada icono con ids únicos generados dinámicamente.
2. Usar un sprite SVG con `<use>` y definir el gradiente una sola vez.
3. Usar CSS gradients en lugar de SVG gradients para iconos que se repiten.

---

## Performance: gradientes complejos

Los gradientes SVG se calculan por el motor gráfico del navegador. Para SVGs con muchos gradientes o con animaciones:

- Preferir gradientes simples (2–3 stops) sobre complejos (5+ stops) cuando sea posible.
- `will-change: transform` en el elemento animado puede promover a GPU y mejorar rendimiento.
- Los gradientes radiales con `fx`/`fy` desplazados son más costosos que los centrados.

---

## Convertir ángulo a x1/y1/x2/y2

Si se necesita un ángulo exacto sin `gradientTransform`:

```
Para ángulo θ (en grados, medido desde horizontal):
x1 = 50% - cos(θ) × 50%
y1 = 50% - sin(θ) × 50%
x2 = 50% + cos(θ) × 50%
y2 = 50% + sin(θ) × 50%
```

En la práctica, es mucho más fácil usar `gradientTransform="rotate(θ, 0.5, 0.5)"`.
