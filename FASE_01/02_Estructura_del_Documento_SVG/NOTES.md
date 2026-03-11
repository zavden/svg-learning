# Notas de compatibilidad y referencias — Sección 02

---

## Accesibilidad SVG en 2025 — estado del arte

La accesibilidad SVG ha mejorado mucho, pero sigue teniendo particularidades por navegador y lector de pantalla:

### Soporte de `<title>` y `<desc>` por combinación browser/AT

| Combinación | `<title>` | `<desc>` | Notas |
|---|---|---|---|
| Chrome + JAWS | Sí | Sí | El más consistente |
| Chrome + NVDA | Sí | Parcial | Depende de la versión |
| Firefox + NVDA | Sí | Sí | Buen soporte |
| Safari + VoiceOver | Sí | Sí | Buen soporte en iOS/macOS |
| Edge + Narrator | Sí | Parcial | Mejoró en versiones recientes |

**Patrón más robusto** (funciona en todas las combinaciones):
```
<svg role="img" aria-labelledby="t1 d1" ...>
  <title id="t1">Título breve</title>
  <desc id="d1">Descripción larga</desc>
  ...
</svg>
```

---

## `xmlns:xlink` — ¿sigue siendo necesario?

El namespace XLink (`xmlns:xlink="http://www.w3.org/1999/xlink"`) era necesario en SVG 1.1 para el atributo `xlink:href` (referencias a elementos, gradientes, etc.).

**SVG 2 reemplaza `xlink:href` por `href` simple**. Todos los navegadores modernos soportan `href` sin namespace.

| Atributo | Estado | Cuándo usar |
|---|---|---|
| `xlink:href` | Deprecado en SVG 2 | SVGs legacy / compatibilidad IE11 |
| `href` | Recomendado | Proyectos modernos |

**Conclusión práctica:** en proyectos nuevos, usa `href` sin namespace. Solo añade `xmlns:xlink` si necesitas compatibilidad con SVGs muy antiguos o IE11.

---

## SVG inline vs `<img>` — cuándo importa la diferencia

La elección entre inline y `<img src="*.svg">` tiene consecuencias reales:

**CSS del documento padre:**
```css
/* Este CSS NO afecta a un SVG cargado como <img> */
svg .mi-clase { fill: red; }

/* Sí afecta a SVG inline */
```

**JavaScript:**
```js
// Esto NO funciona con <img src="*.svg"> — el DOM del SVG no es accesible
document.querySelector('img svg circle').setAttribute('fill', 'blue');

// Sí funciona con SVG inline
document.querySelector('svg circle').setAttribute('fill', 'blue');
```

---

## Rendimiento: SVG inline vs externo

| Factor | SVG inline | SVG como `<img>` |
|---|---|---|
| HTTP request | No (en el HTML) | Sí (por cada SVG) |
| Caché del browser | No | Sí |
| Tiempo hasta primer render | Inmediato | Espera el request |
| Impacto en el HTML | Aumenta el tamaño del HTML | Cero |
| Compresión | Comprimido con el HTML (si gzip) | Comprimido por separado |

**Para iconos que se repiten muchos veces en la página:** usar `<symbol>` + `<use>` o SVG sprite externo referenciado con `<use href="sprite.svg#icono">`.

---

## SVGO y la limpieza de metadatos

SVGO (SVG Optimizer) es la herramienta estándar para limpiar SVGs exportados de editores. Por defecto elimina:

- `<metadata>` con datos RDF/Dublin Core
- DOCTYPE declarations
- Comentarios del editor
- `xml:space="preserve"` innecesarios
- Atributos redundantes

**Antes de poner un SVG de Inkscape/Illustrator en producción**: pasar por SVGO o SVGOMG (versión web).

---

## Cosas a revisar cuando algo no funciona

| Síntoma | Causa probable | Solución |
|---|---|---|
| Gradiente / filtro no aparece | `url(#id)` mal escrito o ID no existe | Verificar que el id en `<defs>` coincide exactamente |
| Dos elementos con la misma apariencia pero uno no responde a JS | IDs duplicados | Cada elemento debe tener un ID único en todo el SVG |
| El SVG se ve negro cuando se esperaba un gradiente | Se definió en `<defs>` pero no se referencia | Añadir `fill="url(#idDelGradiente)"` |
| El título del SVG no lo anuncia el lector de pantalla | `role="img"` o `aria-labelledby` ausente | Añadir ambos atributos al `<svg>` |
| El SVG en `<img>` no cambia de color con CSS | CSS del padre no accede al SVG externo | Usar SVG inline o `mask-image` |
| El texto del SVG se muestra detrás de otras formas | Orden incorrecto en el código (Painter's Model) | Mover `<text>` al final del SVG |

---

## Recursos para profundizar

- **MDN: SVG element reference** — documentación completa de todos los elementos SVG incluyendo atributos globales
- **Accessible SVG** — Léonie Watson y Paciello Group: artículos sobre patrones de accesibilidad SVG testeados con AT reales
- **SVGOMG** — herramienta online basada en SVGO para optimizar SVGs
- **SVG 2 specification** — W3C: qué cambia respecto a SVG 1.1 (especialmente el reemplazo de `xlink:href`)
- **CSS-Tricks: Using SVG** — serie extensa de artículos sobre distintas formas de usar SVG en la web

---

## Temas avanzados para revisitar

- **`<use>` y Shadow DOM** — cuando se instancia un `<symbol>` con `<use>`, el contenido vive en un Shadow DOM. Esto tiene implicaciones para CSS (Fase 5).
- **SVG Sprites externos** — referenciar `<use href="sprite.svg#id">` con un SVG externo. Requiere CORS configurado correctamente (Fase 5).
- **`z-index` en SVG 2** — hay una propiedad `z-index` propuesta en SVG 2 que algunos navegadores ya implementan parcialmente. Por ahora: reordenar el DOM.
- **`will-change` en SVG** — para animaciones performantes, igual que en CSS. Se trata en la sección de animaciones (Fase 6).
