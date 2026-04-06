# CHALLENGE — Optimización de SVG

Seis retos que van de "ejecuta SVGO sobre 10 archivos" a "monta un pipeline con CI que falla si un PR introduce SVGs sin optimizar". Cada uno añade una capa de automatización o rigor.

---

## 1. Primer pase con SVGO por CLI

**Objetivo:** Optimizar 10 SVGs exportados de Figma/Illustrator y medir el ahorro real.

**Qué construir:**
- Descarga/exporta 10 iconos de un editor (Figma, Inkscape, Illustrator) en una carpeta `src/`.
- Instala SVGO globalmente y ejecuta `svgo -f src -o dist` con la configuración por defecto.
- Haz una tabla (en un `.md` o en una hoja de cálculo) con: nombre, tamaño original, tamaño optimizado, porcentaje de ahorro.
- Abre uno optimizado en un editor de texto. Observa qué se eliminó: comentarios, `inkscape:`, `metadata`, grupos vacíos, decimales.
- Render ambos en una página HTML side-by-side. Confirma que son pixel-perfect idénticos.

**Qué practicas:** CLI básica de SVGO, comparación antes/después, validación visual.

**Criterio de éxito:** Ahorro medio por encima del 60%. Ningún icono se rompe visualmente.

---

## 2. Configuración `svgo.config.js` segura

**Objetivo:** Crear una configuración propia que no rompa SVGs con IDs o viewBox importantes.

**Qué construir:**
- Un `svgo.config.js` que:
  - Use `preset-default` como base.
  - Desactive `removeViewBox` y `cleanupIds`.
  - Active `removeDimensions` (para iconos que escalan al contenedor).
  - Active `sortAttrs` para mejor compresión gzip.
  - Establezca `floatPrecision: 2`.
- Prepara un set de test con 3 casos problemáticos:
  1. Un SVG que usa `<use>` interno con IDs.
  2. Un SVG con CSS externo que apunta a `.miclase`.
  3. Un SVG con `<animate>` que referencia un path por ID.
- Ejecuta tu config y verifica que los 3 siguen funcionando tras optimizar.

**Qué practicas:** overrides de plugins, comprensión de qué rompe qué.

**Criterio de éxito:** Los 3 casos siguen funcionando; los SVGs "normales" ahorran 60%+.

---

## 3. Hook pre-commit con lint-staged

**Objetivo:** Automatizar la optimización al hacer commit para que el repo esté siempre limpio.

**Qué construir:**
- Instala `husky` y `lint-staged` en un proyecto.
- Configura `lint-staged` para que ejecute `svgo --config svgo.config.js` sobre cualquier `.svg` modificado.
- Haz un commit intencionalmente con un SVG "sucio" recién exportado. Verifica que se optimiza antes de entrar al stage.
- Documenta el setup en un `CONTRIBUTING.md` para que el equipo sepa cómo funciona.

**Qué practicas:** git hooks, pre-commit automation, DX.

**Criterio de éxito:** Todos los SVGs del repo están optimizados, sin necesidad de recordar ejecutar SVGO manualmente.

---

## 4. Check en CI que bloquea PRs

**Objetivo:** Añadir un workflow de GitHub Actions que falle si el PR trae un SVG sin optimizar.

**Qué construir:**
- Un workflow `.github/workflows/svg-check.yml` que:
  1. Instale Node y SVGO.
  2. Ejecute `svgo --check` sobre `src/icons/*.svg`.
  3. Si hay algún archivo que no esté optimizado, el job falla.
- Añade un comentario automático en el PR (con `actions/github-script`) que diga qué archivos están mal y cómo corregirlos.
- Abre un PR con un SVG sin optimizar para verificar que el check falla y el comentario aparece.

**Qué practicas:** GitHub Actions, CI gates, feedback automático en PRs.

**Criterio de éxito:** Un PR con SVGs sucios no se puede mergear sin corregirlos.

---

## 5. Auditoría de rendimiento con Performance API

**Objetivo:** Medir con código el impacto de optimizar un SVG en el parseo y primer render.

**Qué construir:**
- Una página HTML que cargue un SVG "pesado" (10KB+) inline.
- Usa `performance.mark()` antes y después de insertar el SVG en el DOM.
- Reporta los tiempos en una tabla visible.
- Compara dos versiones: el SVG sin optimizar y el mismo tras SVGO.
- Grafica (con el propio SVG, por ejemplo) el ahorro en ms.
- Extra: mide también con `PerformanceObserver` el `first-contentful-paint` si el SVG está above-the-fold.

**Qué practicas:** Performance API, medición cuantitativa, gráficas con SVG.

**Criterio de éxito:** Números concretos que demuestran la ganancia: "parseo de 12ms → 3ms".

---

## 6. Pipeline completo: Figma → optimized sprite → build

**Objetivo:** Automatizar un flujo end-to-end desde el export hasta el sprite listo para producción.

**Qué construir:**
- Carpeta `src/icons/` donde el diseñador pone los SVGs recién exportados de Figma.
- Un script `build-sprite.js` (Node) que:
  1. Lea todos los `.svg` de `src/icons/`.
  2. Los optimice con SVGO (API programática, no CLI).
  3. Genere un `sprite.svg` con todos como `<symbol>` (usa `svg-sprite` o compón a mano).
  4. Genere un `sprite.hash.svg` con hash del contenido en el nombre.
  5. Emita un `icons.d.ts` con un union type de los nombres disponibles.
  6. Cree un `catalog.html` con un preview visual de cada icono y su id.
- Añade el script al `package.json` como `"build:icons"`.
- Extra: hazlo idempotente (si nada cambió, no regenerar).

**Qué practicas:** SVGO API programática, generación de assets, tipos automáticos, DX de bibliotecas.

**Criterio de éxito:** Alguien mete un SVG nuevo en `src/icons/`, ejecuta `npm run build:icons`, y obtiene: sprite optimizado, tipos actualizados, catálogo nuevo — todo en un solo comando.
