# CHALLENGE — Buenas Prácticas y Patrones Comunes

Esta es la sección final del roadmap. Los retos son **integradores**: cada uno toca varias áreas (estructura, A11y, rendimiento, seguridad, patrones) y te fuerza a aplicar todo lo aprendido al mismo tiempo. El último reto es un mini-proyecto completo que ejercita casi todo el roadmap.

---

## Reto 1 — Auditar un SVG real con los checklists

**Objetivo**: aplicar los checklists de la sección 25.7 a un SVG del mundo real.

**Pasos**:
1. Elige un SVG de algún proyecto que uses (Heroicons, un logo de empresa, un icono de tu app).
2. Ejecuta los checklists de estructura, A11y, optimización, seguridad, rendimiento y compatibilidad.
3. Escribe en un archivo `audit.md` cada punto con ✓ / ✗ y una explicación de por qué.
4. Para cada ✗ encontrado, describe cómo lo arreglarías.
5. **Extra**: corrige al menos 3 ✗ directamente en el SVG y haz un diff del antes/después.

**Criterio de éxito**: tienes un diagnóstico escrito que otra persona podría entender sin contexto.

---

## Reto 2 — Spinner accesible desde cero

**Objetivo**: construir un patrón común con todas las buenas prácticas aplicadas.

**Pasos**:
1. Crea un spinner SVG con `<circle>` + `stroke-dasharray` + rotación CSS.
2. Implementa `currentColor` para que herede el color del texto padre.
3. Añade `role="status"` al contenedor y `aria-label="Cargando"` al SVG.
4. Añade un `<span class="sr-only">Cargando…</span>` como texto para lectores de pantalla.
5. Implementa `@media (prefers-reduced-motion: reduce)` para desactivar la rotación y mostrar solo un pulse de opacidad.
6. Prueba con un lector de pantalla (VoiceOver en Mac, NVDA en Windows, TalkBack en Android).
7. **Extra**: mide el FPS con DevTools en throttled 4x CPU — debería quedarse en 60.

**Criterio de éxito**: el spinner se ve bien, se anuncia correctamente por screen reader, y respeta la preferencia de movimiento reducido.

---

## Reto 3 — Sanitizar SVG de usuario

**Objetivo**: construir una función de sanitización que sea segura contra los vectores XSS vistos en 25.5.

**Pasos**:
1. Crea un HTML simple con un `<textarea>` donde el usuario pega código SVG.
2. Añade un botón "Mostrar" que tome el contenido y lo inserte en el DOM con `innerHTML`.
3. **Sin sanitizar todavía**, prueba estos 5 payloads:
   - `<svg><script>alert('XSS')</script></svg>`
   - `<svg><circle onclick="alert(1)" cx="50" cy="50" r="20"/></svg>`
   - `<svg><image onerror="alert(1)" href="x"/></svg>`
   - `<svg><foreignObject><img src=x onerror="alert(1)"></foreignObject></svg>`
   - `<svg><a href="javascript:alert(1)"><text>clic</text></a></svg>`
4. Confirma cuáles ejecutan el alert.
5. Añade `DOMPurify.sanitize()` con el perfil SVG. Repite los 5 payloads.
6. Verifica que ninguno ejecuta alert ahora.
7. **Extra**: añade una CSP `script-src 'self'` a la página y confirma que bloquea los payloads restantes incluso sin DOMPurify.

**Criterio de éxito**: tienes evidencia concreta (capturas o GIFs) de los 5 payloads antes/después, y entiendes por qué DOMPurify los detiene.

---

## Reto 4 — Barra de progreso con transiciones suaves

**Objetivo**: implementar un patrón común animando solo propiedades GPU-friendly.

**Pasos**:
1. Crea un SVG con un `<rect>` de fondo y un `<rect>` de progreso.
2. Primera versión: anima el `width` del rect de progreso. Mide el rendimiento con DevTools Performance.
3. Segunda versión: mantén `width="100"` fijo y usa `transform: scaleX()` + `transform-origin: left`. Mide de nuevo.
4. Compara en Performance tab: layout thrashing vs composition puro.
5. Implementa la segunda versión como componente reutilizable con props `value` (0-100) y `color`.
6. Añade `aria-valuenow`, `aria-valuemin="0"`, `aria-valuemax="100"`, `role="progressbar"` al SVG.
7. **Extra**: añade un `<text>` con el porcentaje que se actualiza junto con la barra, y asegúrate de que los lectores de pantalla anuncian el cambio.

**Criterio de éxito**: la versión con `scaleX` no muestra violet bars (layout) en el Performance panel; la barra es accesible.

---

## Reto 5 — Icono con estado (favorite toggle)

**Objetivo**: implementar el patrón de icono con estado de 25.6 con todas las capas.

**Pasos**:
1. Crea un `<symbol id="icon-heart">` con dos `<path>`: contorno y relleno.
2. Úsalo en un `<button>` mediante `<use href="#icon-heart">`.
3. CSS: por defecto, `.relleno { opacity: 0; transition: opacity 0.2s }`.
4. Al añadir `.activo` al botón, el relleno aparece con transición suave.
5. JavaScript: el botón togglea `.activo` al hacer click.
6. Accesibilidad: el `<button>` tiene `aria-pressed="true/false"` que refleja el estado.
7. Screen reader: cambia el `aria-label` entre "Añadir a favoritos" y "Quitar de favoritos".
8. **Extra 1**: añade una micro-animación de "bounce" al marcar (con `@keyframes` + `transform: scale`).
9. **Extra 2**: respeta `prefers-reduced-motion` → el bounce se convierte en un fade simple.
10. **Extra 3**: añade un `<span class="sr-only">` que se actualiza con "Añadido a favoritos" / "Eliminado de favoritos" y tiene `aria-live="polite"`.

**Criterio de éxito**: el toggle funciona visualmente, es accesible por teclado, y los screen readers anuncian el cambio de estado.

---

## Reto 6 — Mini-proyecto: sistema de iconos completo

**Objetivo**: cerrar el roadmap construyendo un pipeline end-to-end que integre casi todo lo aprendido.

**Brief**: construye un sistema de iconos React con 10+ iconos, optimizado, accesible, tematizable, con sprite externo como fallback.

**Pasos**:
1. **Diseño**: elige 10 iconos de Heroicons, Phosphor o Tabler. Descárgalos crudos a `src/icons/`.
2. **Optimización**: configura SVGO con `removeViewBox: false` y `cleanupIds: false`. Automatiza con `npm run optimize`.
3. **Build**: setup Vite + React + TypeScript + `vite-plugin-svgr`.
4. **Componentes**: importa cada icono como componente React con `currentColor` y `width="1em"`.
5. **Tipado**: crea un tipo `IconName = 'close' | 'search' | ...` y un componente `<Icon name={...} />` que mapea al componente correcto.
6. **Tematización**: los iconos heredan el color del padre. Un prop `size` modifica el `font-size`.
7. **A11y**: el `<Icon>` acepta `label` opcional:
   - Si hay `label`: `role="img"` + `aria-label`.
   - Si no hay `label`: `aria-hidden="true"`.
8. **Sprite externo**: escribe un script Node que concatene todos los SVGs en `public/sprite.svg` con `<symbol>` para cada uno.
9. **Fallback**: crea un `<IconSprite name={...} />` alternativo que use `<use href="/sprite.svg#icon-close" />` para casos donde el tree-shaking importa menos que el caching.
10. **Demo page**: muestra los 10 iconos en 3 tamaños (sm, md, lg) y 3 colores (text, primary, danger).
11. **Tests**: un test visual básico (manual está bien) en Chrome, Firefox, Safari.
12. **Rendimiento**: `bundle analyzer` — confirma que solo los iconos usados entran al bundle.
13. **Pre-commit hook**: husky + lint-staged corre SVGO en cada commit.
14. **README**: documenta cómo usar cada enfoque (`<Icon>` vs `<IconSprite>`), cuándo elegir uno u otro.

**Criterio de éxito**:
- `<Icon name="close" />` funciona y es accesible.
- Los iconos heredan color del texto y tamaño relativo.
- El sistema es tree-shakeable.
- El sprite externo funciona como alternativa.
- SVGO corre automáticamente en commits.
- Has tomado decisiones conscientes en cada paso y las has documentado.

**Bonus final**: publica el sistema como paquete npm privado o como GitHub Gist. Si lo vas a usar, úsalo en un proyecto real y mide el impacto a lo largo de unas semanas. El aprendizaje viene de las pequeñas cosas que fallan cuando el código sale al mundo.

---

## El cierre

Si has completado los 6 retos y has leído con calma las secciones 1-25, tienes ahora un conocimiento intermedio sólido de SVG. No un conocimiento enciclopédico — eso viene con los años — pero sí el marco mental para decidir, depurar, optimizar y colaborar en proyectos que usen SVG sin sentirte perdido.

El siguiente paso no es otro roadmap. Es usar SVG en algo que te importe. Un blog personal con iconos propios, una visualización de datos sobre un tema que te interese, una herramienta que necesites. La teoría sin práctica decae; la práctica sin teoría se estanca. Con ambas, creces.

Buen viaje.
