# CHALLENGE — Sprites SVG e Iconos

Seis ejercicios que construyen tu propio sistema de iconos desde cero. Van de "crea un sprite con tres símbolos" a "publica un paquete npm con componentes React tipados y catálogo Storybook". Haz los que te reten.

---

## 1. Sprite inline básico

**Objetivo:** Crear un sprite inline con 4 iconos y consumirlos desde HTML.

**Qué construir:**
- Un `<svg style="display:none">` al inicio del `<body>` con 4 `<symbol>` de viewBox 24×24: `icon-home`, `icon-user`, `icon-mail`, `icon-close`.
- Cada símbolo con un `<path fill="currentColor"/>` (puedes robar el path de Heroicons o dibujarlo tú).
- 4 botones que los usen: uno con texto visible ("Inicio"), otro como botón-solo-icono (aria-label), uno dentro de un `<a>` y otro con color distinto via CSS.
- Una clase `.icon` con `width: 1.25em`, `height: 1.25em`, `fill: currentColor`, `vertical-align: -0.15em`.

**Qué practicas:** `<symbol>`, `<use>`, `currentColor`, accesibilidad decorativa vs con significado.

**Criterio de éxito:** Cambias el `color` del botón en CSS y el icono lo hereda sin tocar el SVG.

---

## 2. Sprite externo con cache-control

**Objetivo:** Mover el sprite a un archivo separado y servirlo con cabeceras correctas.

**Qué construir:**
- Mueve los 4 símbolos a `public/sprite.svg` (servidor estático, Vite, Express, lo que prefieras).
- Cambia el HTML a `<use href="/sprite.svg#icon-home"/>`.
- Configura el servidor para que envíe:
  - `Content-Type: image/svg+xml`
  - `Cache-Control: public, max-age=31536000, immutable`
- Añade un hash en el nombre del archivo (`sprite.a7b3.svg`) y actualiza las referencias por script.
- Verifica en la pestaña Network que el sprite se cachea tras el primer request.

**Qué practicas:** sprite externo, versionado por hash, cabeceras HTTP.

**Criterio de éxito:** En el DevTools Network, tras recargar, el sprite dice "from disk cache".

---

## 3. Iconos bicolor con CSS variables

**Objetivo:** Diseñar iconos con dos colores independientes controlados por variables CSS.

**Qué construir:**
- 3 iconos bicolor: `mail` (sobre + relleno), `folder` (carpeta + pestaña), `shield` (forma + check interior).
- Cada `<symbol>` tiene dos `<path>`: uno con `fill="var(--icon-bg, #e2e8f0)"`, otro con `fill="var(--icon-fg, currentColor)"`.
- 3 temas CSS (`.theme-light`, `.theme-dark`, `.theme-brand`) que redefinen las variables.
- Una página que muestre los 3 iconos en los 3 temas (matriz 3×3).
- Añade hover: cambia `--icon-bg` al pasar el mouse.

**Qué practicas:** CSS custom properties dentro de símbolos, tematización sin duplicar archivos.

**Criterio de éxito:** Los mismos 3 símbolos renderizan 9 combinaciones visuales sin tocar el SVG.

---

## 4. Componente React Icon con SVGR

**Objetivo:** Montar un pipeline donde los `.svg` se convierten automáticamente en componentes React.

**Qué construir:**
- Proyecto Vite + React + TypeScript.
- Instala `vite-plugin-svgr` y configúralo en `vite.config.ts`.
- Guarda 8 SVGs en `src/assets/icons/` (descárgalos de Heroicons).
- Crea un componente wrapper `<Icon name="home" size={24} label="Inicio"/>` que acepte: `name` (tipo union de los 8 nombres), `size` (number, default 24), `label` (opcional, si viene se añade aria-label; si no, aria-hidden).
- Tipa `name` con un literal union generado automáticamente con un script (lee los archivos de `icons/` y genera `type IconName = "home" | "user" | ...`).

**Qué practicas:** SVGR, TypeScript, props API, accesibilidad por props.

**Criterio de éxito:** Autocompletado de nombres de icono en el IDE. Tree-shaking verificado: el bundle solo incluye los iconos realmente usados.

---

## 5. Catálogo visual con Storybook

**Objetivo:** Publicar un catálogo navegable de tu sistema de iconos.

**Qué construir:**
- Amplía el proyecto anterior con Storybook.
- Crea una story `Icons/Catalog` que renderice una grid con todos los iconos, sus nombres, sus tamaños (sm/md/lg/xl) y un botón "copiar nombre" en cada uno.
- Añade una story por icono (`Icons/Home`, etc.) con controls para size, color, label.
- Una página MDX explicando: instalación, uso básico, tabla de props, ejemplos con texto y solo-icono, política de contribución.
- Deploy a Chromatic o GitHub Pages.

**Qué practicas:** documentación del sistema, Storybook, MDX, controls interactivos.

**Criterio de éxito:** Alguien nuevo en el equipo encuentra cualquier icono en menos de 30 segundos sin leer código.

---

## 6. Paquete npm publicable con changelog

**Objetivo:** Convertir tu sistema en una biblioteca reutilizable versionada.

**Qué construir:**
- Configura el repo como paquete npm: `package.json` con `main`, `module`, `types`, `exports`, `sideEffects: false`.
- Build con `tsup` o `rollup` que genera ESM + CJS + tipos.
- Dos exports:
  - `@tu-nombre/icons` → componentes React (`import { HomeIcon } from '@tu-nombre/icons'`)
  - `@tu-nombre/icons/sprite` → el archivo sprite.svg para consumo vanilla
- Changelog automático con `changesets` o commits convencionales.
- Un icono marcado como deprecated: añade un `console.warn` en desarrollo cuando se importa, con fecha de eliminación.
- Publica en npm (o en un registry local/GitHub Packages si prefieres no publicar).

**Qué practicas:** packaging, tree-shaking en bibliotecas, exports condicionales, gestión de deprecations, changelog.

**Criterio de éxito:** Instalas tu paquete en un proyecto de prueba, importas 2 iconos, y el bundle final solo contiene esos 2. El icono deprecated imprime warning con instrucciones de migración.
