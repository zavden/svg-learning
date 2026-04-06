# Notes — Herramientas y Flujo de Trabajo

## Compatibilidad de las herramientas

| Herramienta | Plataforma | Precio | Estado 2026 |
|-------------|-----------|--------|-------------|
| Inkscape | Linux/Mac/Win | Gratis | Mantenido activamente |
| Illustrator | Mac/Win | Suscripción | Estándar industria |
| Figma | Web/Mac/Win | Freemium | Dominante en UI/UX |
| Sketch | macOS | Pago + sub | En declive vs Figma |
| Affinity Designer | Mac/Win/iPad | Pago único | Serif (ahora Canva) |
| Boxy SVG | Web/Mac/Chrome | Gratis/pago | Activo |
| VS Code + jock.svg | Multi | Gratis | Recomendado |
| WebStorm | Multi | Suscripción | Nativo SVG |
| D3.js v7+ | Web | Gratis | Maduro |
| GSAP 3+ | Web | Free/Club | Estándar animación |
| Anime.js | Web | Gratis | Mantenido |
| Lottie-web | Web | Gratis | Airbnb/Meta |
| SVG.js v3+ | Web | Gratis | Activo |
| Snap.svg | Web | Gratis | Poco mantenido |

---

## El hilo conductor: separar diseño de código

El error repetido del que partes cuando te vuelves senior en SVG es entender que **el `.svg` es un formato de intercambio**, no un archivo editable por dos sitios a la vez. El flujo sano trata al SVG como un artefacto:

1. **Origen único**: el diseñador exporta desde Figma/Illustrator. No se toca el archivo después.
2. **Transformación automática**: SVGO + SVGR lo procesan determinísticamente en cada build.
3. **Modificaciones en la capa de código**: si quieres añadir clases, IDs, eventos, se hace en el wrapper (componente React, función JS) o en la config del transformador.

Esto evita el escenario de "alguien editó el logo.svg a mano y la siguiente exportación lo borró sin avisar". Si el pipeline es puro (input → output determinístico), puedes regenerar todo desde el origen sin miedo.

---

## Bundle size — comparativa real

```
D3.js (completo)     ~250KB    gzipped
D3 módulos sueltos    ~20KB    gzipped (scale + shape + selection)
GSAP core              ~70KB    gzipped
GSAP + plugins        ~120KB    gzipped
Anime.js               ~17KB    gzipped
Vivus.js                ~6KB    gzipped
lottie-web            ~250KB    gzipped
SVG.js                 ~13KB    gzipped
Snap.svg               ~80KB    gzipped
Paper.js              ~200KB    gzipped
SVGR runtime            ~0KB    (transforma en build, no en runtime)
```

**Regla implícita:** una librería de SVG que pesa más que tu CSS crítico necesita justificación. D3 y Lottie valen su peso cuando resuelven problemas que a mano serían imposibles. Vivus o SVG.js son "mercancía común" — casi siempre hay una forma más ligera.

---

## El truco del "cuándo migrar"

Si estás usando jQuery + Snap.svg, la pregunta no es "¿migro?" sino "¿cuándo rompe algo?":
- **Si funciona y no toca nada**: no migres. Code de 2017 no se oxida por sí solo.
- **Si el bundle pesa mucho**: sustituye Snap por SVG.js o por código puro.
- **Si el navegador target es moderno**: la mayoría de lo que hace jQuery+Snap se resuelve con `document.querySelectorAll` y `setAttribute` directos.

La migración prematura cuesta más que la deuda técnica. El umbral es: ¿afecta al bundle crítico? ¿afecta a una nueva feature? Si la respuesta es sí, migra la pieza tocada, no todo.

---

## DevTools: las 4 cosas que salvan tiempo

1. **Editar atributos con flechas + modificadores** (`Shift+↑`, `Alt+↑`, `Cmd+↑`) — para afinar un `stroke-width` o un `transform` sin escribir números.
2. **`$0` en consola** — apunta al nodo seleccionado. `$0.getBBox()` en un instante.
3. **Panel Animations con slow motion al 10%** — imprescindible para ver en qué frame ocurre algo raro.
4. **Copy as fetch** en el tab Network — útil cuando un sprite SVG no carga y quieres reproducir el request en consola.

Estas cuatro cosas acortan sesiones de debugging de 20 minutos a 2. Son más valiosas que la mayoría de plugins de VS Code combinados.

---

## Figma Copy as SVG vs Export panel

Hay diferencias sutiles:
- **Copy as SVG (Cmd+Opt+Shift+C)**: conveniente, genera XML que puedes pegar en código. No pasa por plugins. El resultado tiene IDs autogenerados y suele llevar `fill` duplicados.
- **Export panel**: más configurable (formato, escala, ID incluido). Si tienes el plugin SVGO Compressor activo, lo aplica aquí (no en Copy as SVG).

**Regla:** para desarrollo, usa Copy as SVG y pasa SVGO en el pipeline. Para entregar assets a otro equipo sin pipeline, usa el Export panel con plugin activo.

---

## Por qué SVGR domina en React

Antes de SVGR, el ecosistema React tenía 3 caminos mediocres:
1. `<img src="icon.svg" />` — pierdes control sobre color, no accesible, un HTTP extra.
2. SVG inline copiado a JSX — verboso, duplicación, nadie actualiza.
3. `svg-inline-loader` — string dentro de `dangerouslySetInnerHTML`, feo y peligroso.

SVGR resolvió el problema correcto: cada `.svg` se trata como si fuera un componente React. El SVG pasa por SVGO primero (limpieza), luego se envuelve en JSX con `forwardRef`, se añaden props (color, size) y se exporta. El resultado es tree-shakeable, tipado y conserva el archivo `.svg` como fuente de verdad.

Por eso es el default tanto en Create React App (originalmente) como en `vite-plugin-svgr` y `@svgr/webpack`. No hay alternativa mejor hoy.

---

## Cuándo D3 es overkill

Nadie discute que D3 es potente. Pero el coste de aprenderlo es alto y el bundle se nota. Casos donde NO deberías usar D3:

- **Un gráfico estático con datos fijos** — dibújalo en Figma, exporta y acabas.
- **Un gráfico con 3 o 4 puntos de datos** — genera el SVG con una función JS de 20 líneas.
- **Una animación basada en tiempo, no en datos** — GSAP o CSS resuelven mejor.
- **Un icono** — ni se te ocurra.

Usa D3 cuando tengas escalas dinámicas, proyecciones, layouts jerárquicos o cientos/miles de elementos atados a datos que cambian. Para el resto, el ratio esfuerzo/beneficio no compensa.

---

## El ecosistema cambia rápido — qué vigilar

- **CSS @property + registered properties**: cada vez más cosas que antes necesitaban JS se pueden animar con CSS puro. Vivus.js y parte de GSAP pierden terreno.
- **View Transitions API**: para transiciones entre estados, empieza a sustituir trucos de GSAP con paths.
- **Web Components**: sprites SVG como `<custom-icon>` están ganando tracción.
- **Bun / Rolldown**: bundlers nuevos que cambiarán los plugins SVG disponibles. El pipeline de SVGO + SVGR probablemente siga, pero el loader específico cambiará.

La apuesta segura para el próximo ciclo: mantén el origen (Figma) y la limpieza (SVGO) estables, que lo que envuelve puede cambiar cada 2-3 años.
