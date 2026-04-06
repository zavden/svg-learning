# CHALLENGE — Herramientas y Flujo de Trabajo

6 retos progresivos. Desde montar un entorno básico hasta construir un pipeline completo de sistema de iconos. Cada reto añade una pieza del flujo profesional.

---

## Reto 1 — Explorar el panel Animations

**Objetivo**: ganar fluidez con el panel de Animations de DevTools.

**Pasos**:
1. Crea un HTML con un `<svg>` que contenga un `<circle>` animado con CSS (`@keyframes`) y otro animado con la Web Animations API (`element.animate()`).
2. Abre DevTools → `Cmd+Shift+P` → "Show Animations".
3. Captura las dos animaciones y ponlas a 25% de velocidad.
4. Pausa una sola de ellas. Cambia los keyframes en vivo desde el panel.
5. Añade una tercera animación con SMIL (`<animate>`) y verifica que el panel NO la captura (esperado).
6. Desde la consola, ejecuta `document.querySelector('svg').pauseAnimations()` y comprueba que SMIL se para.

**Criterio de éxito**: entiendes qué tipos de animación el panel captura y cuáles no, y sabes parar SMIL desde la consola.

---

## Reto 2 — Limpiar un SVG "sucio" con SVGOMG

**Objetivo**: aprender a leer un SVG de editor y decidir qué conservar.

**Pasos**:
1. En Figma (o Inkscape), crea una ilustración con 3-4 formas, grupos, y un texto.
2. Exporta como SVG. Mira el resultado crudo: busca namespaces, IDs, metadatos, decimales largos.
3. Pega el SVG en SVGOMG (`jakearchibald.github.io/svgomg/`).
4. Anota el peso inicial. Optimiza con los defaults. Anota el peso final y el %.
5. Ahora **desactiva**: `removeViewBox`, `cleanupIds`, `removeTitle`. Vuelve a optimizar. Compara.
6. Copia el resultado y comprueba visualmente que se ve igual en un visor HTML.

**Criterio de éxito**: reducción > 50% sin romper nada visible, entendiendo qué plugins tocaste y por qué.

---

## Reto 3 — Pipeline SVGO con config versionada

**Objetivo**: establecer un flujo de optimización reproducible en un proyecto.

**Pasos**:
1. Crea un proyecto nuevo: `npm init -y && npm i -D svgo`.
2. Crea `svgo.config.js` con `preset-default` y overrides para desactivar `removeViewBox` y `cleanupIds`.
3. Crea `src/svg/` con 3 SVGs crudos (copiados de Figma o SVGOMG sin optimizar).
4. Añade script `"optimize": "svgo -f src/svg -o dist/svg"` a `package.json`.
5. Ejecuta `npm run optimize`. Verifica que los archivos de `dist/svg` son más pequeños.
6. Añade un cuarto SVG a `src/svg/` con un `<title>` deliberadamente, vuelve a optimizar y comprueba que el `<title>` se conserva (no lo has desactivado, pero el default de `preset-default` debería preservarlo si tiene texto).

**Criterio de éxito**: tienes un script `npm run optimize` que produce output determinístico y la config está en Git.

---

## Reto 4 — Pre-commit hook con husky + lint-staged

**Objetivo**: imponer la optimización automáticamente antes de cada commit.

**Pasos**:
1. En el proyecto del Reto 3: `npm i -D husky lint-staged`.
2. `npx husky init`.
3. En `package.json`, añade:
   ```json
   "lint-staged": { "*.svg": "svgo" }
   ```
4. En `.husky/pre-commit`, escribe: `npx lint-staged`.
5. Añade un SVG sucio al repo, haz `git add icon.svg` y luego `git commit -m "test"`.
6. Comprueba que el archivo committeado es el optimizado, no el original.
7. **Extra**: en CI, ejecuta `svgo -r src/svg && git diff --exit-code` para asegurar que nadie commitea algo no-optimizado.

**Criterio de éxito**: es imposible committear un SVG sucio sin que SVGO intervenga.

---

## Reto 5 — Sistema de iconos con SVGR + Vite

**Objetivo**: montar un sistema de iconos React con tree-shaking.

**Pasos**:
1. Crea un proyecto Vite + React: `npm create vite@latest icons-demo -- --template react-ts`.
2. Instala: `npm i -D vite-plugin-svgr svgo`.
3. Añade `svgr()` al `vite.config.ts`.
4. Crea `src/icons/` con 5 SVGs (Heroicons, por ejemplo — arrastrar del sitio).
5. Añade `svgo.config.js` con tu config segura.
6. Importa uno: `import CloseIcon from './icons/close.svg?react'`.
7. Úsalo: `<CloseIcon className="size-6 text-red-500" />`.
8. Construye: `npm run build`. Mira `dist/assets/` — busca si los iconos no usados aparecen.
9. Cambia el import a un barrel (`./icons/index.ts` con `export * from ...`) y rebuildea. Observa cuánto crece el bundle.

**Criterio de éxito**: entiendes por qué los imports directos dan tree-shaking y los barrel exports no siempre.

---

## Reto 6 — Pipeline completo: Figma → sprite → web

**Objetivo**: construir el pipeline completo de un sistema de iconos real, con SVGO programático y sprite final.

**Pasos**:
1. Exporta 10+ iconos desde Figma a `raw/icons/`.
2. Escribe `scripts/build-sprite.js` en Node que:
   - Lee todos los `.svg` de `raw/icons/`.
   - Los pasa por `optimize()` de la API programática de SVGO con tu config segura.
   - Convierte cada uno a un `<symbol id="icon-{nombre}">…</symbol>` (quitar el `<svg>` wrapper, quedarte con el interior).
   - Concatena todo en un único `<svg><defs>...</defs></svg>` y lo guarda en `public/sprite.svg`.
3. Añade `"build:icons": "node scripts/build-sprite.js"` a `package.json`.
4. En HTML, usa un icono del sprite: `<svg><use href="/sprite.svg#icon-close" /></svg>`.
5. Mide el peso total: un único `sprite.svg` vs los iconos sueltos. Mide el número de requests HTTP.
6. **Extra**: añade brotli en el servidor y compara el peso final.
7. **Extra avanzado**: integra el script en el `build` de Vite como un plugin propio (función con hook `buildStart`).

**Criterio de éxito**: un único fetch trae todos los iconos, pesan mucho menos que los archivos sueltos, y el proceso es reproducible desde `raw/` hasta `public/sprite.svg` con un comando.

---

## Bonus — Comparar librerías de animación

**Objetivo**: interiorizar la diferencia de esfuerzo entre librerías.

**Tarea**: implementa la misma animación (un círculo que se mueve por un path con `stroke-dashoffset` de 0 a full) en:
1. SVG + CSS puro (`@keyframes`).
2. Anime.js.
3. GSAP.
4. Vivus.js.

Mide para cada uno: líneas de código, peso del bundle, dificultad subjetiva, control sobre el resultado. Escribe 5 líneas sobre cuál elegirías en qué caso real.
