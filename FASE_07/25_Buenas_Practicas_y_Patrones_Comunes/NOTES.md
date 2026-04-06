# Notes — Buenas Prácticas y Patrones Comunes

## Compatibilidad de las características clave (2026)

| Característica | Chrome | Firefox | Safari | Edge | IE11 |
|----------------|--------|---------|--------|------|------|
| viewBox | ✓ | ✓ | ✓ | ✓ | ✓ |
| `<use>` con href | ✓ | ✓ | ✓ | ✓ | ✗ (xlink:href) |
| Variables CSS | ✓ | ✓ | ✓ | ✓ | ✗ |
| currentColor | ✓ | ✓ | ✓ | ✓ | ✓ |
| SMIL (`<animate>`) | ⚠ | ✓ | ✓ | ⚠ | ✗ |
| CSS animations en SVG | ✓ | ✓ | ✓ | ✓ | Parcial |
| `prefers-reduced-motion` | ✓ | ✓ | ✓ | ✓ | ✗ |
| paint-order | ✓ | ✓ | ✓ | ✓ | ✗ |
| foreignObject | ✓ | ✓ | ✓ | ✓ | Parcial |
| feGaussianBlur | ✓ | ✓ | ✓ | ✓ | ✓ |
| feDisplacementMap | ✓ | ✓ | ✓ | ✓ | Parcial |

---

## Las 5 reglas que se vienen repitiendo en todo el roadmap

A estas alturas deberías reconocerlas todas. Si las interiorizas, el 80% de los problemas comunes se esquivan solos:

1. **`viewBox` siempre, sin `width`/`height` fijos** — la base de la escalabilidad.
2. **`currentColor` para iconos** — tematización gratis al heredar del texto.
3. **Animar solo `transform` y `opacity`** — el resto fuerza layout y mata el FPS.
4. **A11y de entrada, no a posteriori** — `aria-hidden` o `role="img"+title` desde el principio.
5. **SVGO en el pipeline, no a mano** — config versionada, commit hook, build check.

Estas 5 reglas juntas cubren estructura, tematización, rendimiento, accesibilidad y mantenibilidad. Son la "navaja suiza" del SVG profesional.

---

## Diferencias reales entre Chrome/Firefox/Safari con SVG

Hay pequeñas cosas que divergen entre navegadores evergreen. No son deal-breakers, pero vale conocerlas:

**Filtros**
- Safari calcula regiones de filtro ligeramente distinto — una sombra puede aparecer cortada por 1-2px.
- `feColorMatrix` con `type="hueRotate"` da tonalidades casi idénticas pero no bit-exact.

**Texto**
- Renderizado de fuentes difiere entre SO (macOS vs Windows vs Linux) más que entre navegadores.
- `font-kerning` tiene defaults distintos — si importa, setearlo explícito.
- `text-rendering: geometricPrecision` puede desactivar optimizaciones y verse distinto.

**Animaciones CSS**
- Easings spring/bounce custom se comportan igual en los 4 evergreens.
- Las transiciones de `stroke-dashoffset` son suaves en Firefox, a veces entrecortadas en Safari iOS.

**Regla práctica**: nunca des por bueno un SVG complejo sin abrirlo en al menos Chrome + Safari iOS + Firefox Android. El 99% funciona, pero el 1% te muerde cuando menos lo esperas.

---

## La accesibilidad que nadie implementa (pero debería)

El punto más ignorado del roadmap completo es **`prefers-reduced-motion`**. Todos los spinners, animaciones de hover, morphings de iconos, transitions de secciones — si no respetan esa preferencia, están fallando a usuarios con:

- Migrañas vestibulares
- Trastornos del equilibrio
- Problemas de atención que se saturan con movimiento
- TEPT con triggers visuales

No es opcional. Es como no poner `alt` a las imágenes: excusable por olvido la primera vez, inexcusable después de saberlo. Pon esto al principio de cualquier CSS con animaciones:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

Es fuerza bruta, pero mejor que nada. Lo refinado viene después — deshabilitar keyframes concretos y reemplazar con fades, por ejemplo.

---

## Lo que este roadmap NO cubre (y dónde buscar)

Este roadmap llega hasta un nivel **intermedio sólido**. Las siguientes áreas son avanzadas y necesitarían sus propios roadmaps:

**Filtros SVG avanzados**
- `feDiffuseLighting`, `feSpecularLighting`, `feFlood`, `feComposite`
- Dónde: spec W3C + blog de Sara Soueidan + "SVG Filter Effects" series

**Morphing avanzado**
- Interpolación de paths con distinto número de puntos
- Dónde: GSAP MorphSVG docs + Flubber.js (librería de path interpolation)

**SVG generativo**
- Algoritmos: Voronoi, Delaunay, Perlin noise, sistemas de partículas
- Dónde: Tyler Hobbs blog + "Generative Design" libro + Observable notebooks

**Mapas y geoJSON**
- Proyecciones, topojson, D3 geo
- Dónde: d3-geo docs + Observable notebooks de Mike Bostock

**Web Components + Shadow DOM con SVG**
- Encapsulación de estilos, tematización multi-instancia
- Dónde: MDN Web Components + lit-html docs

**Tests visuales**
- Regresión visual automatizada de iconos/ilustraciones
- Dónde: Chromatic, Percy, Playwright visual comparisons

---

## La visión larga: SVG en 2030

Predicciones razonables basadas en tendencias actuales:

- **CSS tomará más terreno**: `@property`, `view-timeline`, `scroll-timeline`, shape-morphing nativo. Vivus.js y parte de GSAP perderán relevancia.
- **View Transitions API** sustituirá muchos trucos de animación entre estados.
- **SMIL desaparecerá lentamente** — sin nueva implementación, pero mantenido en evergreens por legacy. No escribas SMIL nuevo.
- **WebGPU crecerá** para visualizaciones masivas; D3+SVG seguirá siendo el rey en volumen medio.
- **AI-generated SVG** será cada vez más común en flujos de diseño — lo importante será saber leerlo y limpiarlo, no escribirlo a mano.
- **SVG 2.0 ratificado por fin** (ya lleva años en borrador). Poco impacto real porque los navegadores ya implementan casi todo.

La inversión segura es dominar los fundamentos de este roadmap. Todo lo demás cambia; los fundamentos persisten.

---

## El cierre del roadmap: qué hacer después

Si has llegado hasta aquí, tienes una base sólida en SVG. Los siguientes pasos más productivos, por orden:

1. **Construir algo real** — un sistema de iconos para un proyecto tuyo, una visualización de datos que te interese. La teoría sin práctica se evapora.
2. **Leer código de referencia** — Heroicons, Phosphor Icons, Tabler Icons tienen repos públicos con miles de SVGs bien optimizados. Lee cómo están estructurados.
3. **Contribuir a algo** — corregir la accesibilidad de iconos en un proyecto open source es una primera contribución ideal.
4. **Profundizar en el camino que te atraiga** — D3 si te gustan los datos, GSAP si te gusta la animación, WebGL si te va el 3D.

Y una última cosa: este roadmap se queda obsoleto. No de golpe, sino gradualmente. Cuando lo releas en 3 años, partes estarán desfasadas. Es normal. El objetivo no era memorizarlo — era darte el marco mental para evaluar lo que venga.

Gracias por el viaje.
