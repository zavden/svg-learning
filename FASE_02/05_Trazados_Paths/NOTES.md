# Notas y consejos — Trazados (Paths)

---

## Compatibilidad

`<path>` y todos sus comandos (incluyendo `A`) tienen soporte universal en todos los navegadores modernos desde hace más de una década. No hay preocupaciones de compatibilidad.

---

## No escribas paths a mano para formas complejas

Los paths generados por editores gráficos (Figma, Inkscape, Illustrator) son casi ilegibles pero correctos:

```
d="M8.5,2C4.9,2,2,4.9,2,8.5S4.9,15,8.5,15c1.7,0,3.2-0.6,4.4-1.6l4.6,4.6l1.1-1.1
   L14,12.3C15.1,11.1,15.8,9.4,15.8,7.5H15c0,3.6-2.9,6.5-6.5,6.5S2,11.1,2,7.5
   S4.9,1,8.5,1..."
```

Escribe paths a mano solo para formas simples (líneas, rectángulos, flechas sencillas). Para formas complejas, usa un editor visual.

---

## Herramientas para trabajar con paths

- **SVG Path Editor** (yqnn.github.io/svg-path-editor) — editor visual interactivo de paths
- **Figma / Inkscape** — dibuja visualmente, exporta como SVG con paths
- **SVGOMG** (jakearchibald.github.io/svgomg) — optimiza paths (convierte absolutas a relativas, elimina decimales innecesarios)
- **svg-path-commander** (npm) — librería JS para manipular paths programáticamente

---

## Paths relativos son más compactos

SVGO (optimizador SVG) convierte los comandos a minúsculas (relativas) porque los números suelen ser más pequeños:

```
Absoluto:  M 150 200 L 200 250 L 250 200  (22 chars)
Relativo:  M150 200l50 50l50-50           (17 chars)
```

---

## `path` puede reemplazar cualquier forma primitiva

Equivalencias exactas:

| Primitiva | Equivalente con `<path>` |
|-----------|--------------------------|
| `<rect x="10" y="10" w="80" h="60"/>` | `M10 10 H90 V70 H10 Z` |
| `<circle cx="50" cy="50" r="30"/>` | `M50 20 A30 30 0 1 1 49.99 20 Z` *(aproximación)* |
| `<line x1="0" y1="0" x2="100" y2="50"/>` | `M0 0 L100 50` |
| `<polygon points="0,0 100,0 50,80"/>` | `M0 0 L100 0 L50 80 Z` |
| `<polyline points="0,50 50,0 100,50"/>` | `M0 50 L50 0 L100 50` |

**No los reemplaces.** Usa las primitivas cuando sean suficientes — son más legibles y semánticas.

---

## Animación de paths con `stroke-dashoffset`

La técnica de "drawing animation" usa paths:

```
/* 1. Medir la longitud del path con JS:
      const len = path.getTotalLength();
   2. Configurar dasharray = len, offset = len
   3. Animar offset de len → 0 */

.animated-path {
  stroke-dasharray: 500;
  stroke-dashoffset: 500;
  animation: draw 2s ease forwards;
}
@keyframes draw {
  to { stroke-dashoffset: 0; }
}
```

---

## `getTotalLength()` y `getPointAtLength()` en JavaScript

Los paths tienen métodos del DOM para operaciones geométricas:

```javascript
const path = document.querySelector('path');

// Longitud total del path
const len = path.getTotalLength();

// Punto en una posición específica (útil para animar objetos a lo largo del path)
const point = path.getPointAtLength(len * 0.5); // punto a la mitad
```

---

## Curvas de Bézier: visualizar los puntos de control

Para entender y crear curvas, practica con herramientas interactivas:
- **cubic-bezier.com** — editor de curvas CSS con visualización
- **bezier.method.ac** — tutorial interactivo de Bézier
- El panel "Path" de Figma muestra los handles visualmente (los cuadraditos de cada vértice)

---

## `clipPathUnits` y paths como máscaras

Los paths también se usan en `<clipPath>` para recortar elementos con formas personalizadas:

```svg
<defs>
  <clipPath id="mi-clip">
    <path d="M 0 0 Q 150 50 300 0 L 300 200 L 0 200 Z"/>
  </clipPath>
</defs>
<image href="foto.jpg" clip-path="url(#mi-clip)" .../>
```

Esto recorta la imagen con una forma de ola — imposible con `<rect>` o primitivas básicas.
