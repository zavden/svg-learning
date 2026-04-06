# Glosario — Buenas Prácticas y Patrones Comunes

## Estructura y organización

**`viewBox`** — atributo del elemento `<svg>` raíz que define el sistema de coordenadas interno. Sintaxis: `viewBox="min-x min-y ancho alto"`. Sin viewBox no hay SVG escalable.

**`<defs>`** — elemento contenedor para recursos reutilizables (gradientes, patrones, filtros, clipPaths, masks, symbols). Todo lo que esté dentro de `<defs>` es invisible hasta que algo lo referencie con `url(#id)` o `<use>`.

**`<g>` (group)** — elemento contenedor lógico. Permite aplicar `transform`, `opacity`, `filter`, `clip-path` o estilos a varios elementos a la vez. Un `<g>` sin atributos que envuelve un solo elemento es basura eliminable.

**`z-order en SVG`** — SVG no tiene `z-index`: el orden del DOM determina qué se pinta encima. Los últimos elementos del árbol se renderizan por último y quedan visualmente encima.

---

## Accesibilidad (A11y)

**`aria-hidden="true"`** — atributo ARIA que excluye un elemento del árbol de accesibilidad. Iconos decorativos deben llevarlo para no ser leídos por tecnologías asistivas.

**`focusable="false"`** — atributo SVG que evita que el SVG reciba foco por teclado en IE/Edge legacy. Se añade junto a `aria-hidden="true"` por compatibilidad.

**`role="img"`** — atributo ARIA que marca el SVG como imagen semántica. Necesario cuando el SVG tiene significado y debe anunciarse con su título.

**`<title>`** — primer hijo del SVG que contiene el texto equivalente al SVG. Usado junto con `aria-labelledby` para exponer el nombre a tecnologías asistivas. No confundir con `title` HTML (tooltip).

**`<desc>`** — descripción más larga del SVG. Complementa a `<title>` para SVGs informativos (gráficos de datos, ilustraciones explicativas).

**`aria-labelledby`** — atributo ARIA que apunta a uno o varios IDs cuyo texto describe el elemento. En SVG suele apuntar a los IDs del `<title>` y `<desc>`.

**`aria-label`** — atributo ARIA con el texto directo (sin apuntar a un ID). Útil en botones sin texto visible que contienen un icono SVG: `<button aria-label="Cerrar"><svg aria-hidden="true">…</svg></button>`.

**`prefers-reduced-motion`** — media query CSS que detecta si el usuario ha pedido reducir el movimiento. Toda animación debe respetarla (puede causar mareos, migrañas, crisis vestibulares reales).

**`WCAG AA`** — nivel de conformidad de Web Content Accessibility Guidelines. Mínimo: contraste 4.5:1 para texto normal, 3:1 para texto grande y elementos UI.

---

## Rendimiento

**`nodos DOM`** — cada elemento SVG es un nodo del árbol DOM. Cuesta parseo, layout, paint y memoria. Umbrales prácticos: < 1.000 sin problema, > 5.000 considera Canvas.

**`layout / reflow`** — fase del pipeline de renderizado que calcula las dimensiones y posición de los elementos. Animar `width`, `height`, `x`, `y`, `stroke-width` o el atributo `d` de un path la dispara en cada frame.

**`paint / repaint`** — fase que rellena los píxeles de una capa. Animar `fill` o `stroke` puede forzarlo. Más barato que layout pero aún así evitable en loops de 60fps.

**`composition / compositing`** — fase final donde el GPU combina las capas. `transform` y `opacity` se resuelven aquí sin tocar layout ni paint — por eso son "baratas" de animar.

**`GPU-acelerado`** — propiedades que el compositor puede animar sin repintar: `transform`, `opacity`, `filter` (en algunos casos). Ideal para animaciones de 60fps.

**`will-change`** — propiedad CSS que avisa al navegador de que un elemento va a cambiar pronto. Promociona el elemento a su propia capa GPU. Consume memoria — usar solo durante animación activa, desactivar después.

**`Intersection Observer`** — API de JS que notifica cuándo un elemento entra o sale del viewport. Útil para cargar SVGs pesados solo cuando son visibles (lazy loading manual).

**`loading="lazy"`** — atributo nativo de `<img>` que delega el lazy loading al navegador. Funciona con `<img src="foo.svg">`.

---

## Seguridad

**`XSS`** — *Cross-Site Scripting*. Ataque donde un atacante inyecta código JavaScript que se ejecuta en el navegador de otros usuarios. SVG inline sin sanitizar es un vector clásico.

**`XSS almacenado`** — variante donde el payload se guarda en el servidor (ej. un SVG subido como avatar) y se dispara con cada visitante. Más peligroso que el reflejado.

**`<script>` en SVG`** — elemento válido en SVG que ejecuta JavaScript cuando el SVG se renderiza inline o dentro de `<object>` / `<iframe>`. En `<img>` no ejecuta — por eso `<img>` es seguro.

**`event handlers en atributos`** — `onload`, `onclick`, `onerror`, `onmouseover`, etc. escritos como atributos en elementos SVG (`<circle onload="...">`). Ejecutan código arbitrario cuando el SVG se renderiza inline.

**`<foreignObject>`** — elemento SVG que permite embeber HTML (u otro XML) dentro del SVG. Puede contener `<img src=x onerror=...>` u otros vectores XSS.

**`DOMPurify`** — librería JS que sanitiza HTML/SVG eliminando scripts, event handlers y elementos peligrosos. Estándar de facto en el cliente. Tiene perfiles específicos `svg` y `svgFilters`.

**`CSP (Content Security Policy)`** — cabecera HTTP que restringe qué recursos puede cargar el navegador. `script-src 'self'` bloquea scripts inline y de otros orígenes, añadiendo una red de seguridad contra XSS.

**`magic bytes`** — primeros bytes de un archivo que identifican su tipo real, independientemente de la extensión. Un archivo `.svg` que empieza con `<?php` no es un SVG — validar en el backend.

---

## Patrones de diseño

**`stroke-dasharray`** — atributo/propiedad CSS que define el patrón de guiones del trazo. Junto con `stroke-dashoffset` permite dibujar arcos parciales y animar trazados.

**`stroke-dashoffset`** — atributo/propiedad CSS que desplaza el inicio del patrón de guiones. Base de las animaciones de "dibujo automático" y de los spinners.

**`spinner pattern`** — círculo con `fill="none"` y `stroke` parcial (vía `stroke-dasharray`) que rota continuamente. Clasicazo de los loaders modernos.

**`progress bar SVG`** — patrón de dos elementos (fondo + progreso) donde el progreso se controla ajustando `width` de un `<rect>` o `stroke-dashoffset` de un path.

**`icono con estado`** — patrón donde dos paths (contorno + relleno) comparten `<symbol>`, y una clase CSS alterna su visibilidad con `opacity` o `display`.

**`mapa interactivo`** — SVG donde cada región es un `<path>` con un `id` único. Los eventos se manejan con **delegación** desde el grupo padre en lugar de N listeners.

**`<textPath>`** — elemento SVG que renderiza texto siguiendo un `<path>`. Permite texto en curva, en círculo, en ondas, etc.

**`delegación de eventos`** — patrón donde un único listener en el elemento padre maneja eventos de sus descendientes vía `event.target`. Crítico para mapas o listas SVG grandes.

---

## Compatibilidad

**`navegadores evergreen`** — navegadores con actualización automática continua (Chrome, Firefox, Safari, Edge). El 99% del SVG funciona igual en todos.

**`xlink:href`** — forma legacy (SVG 1.1) de referenciar en `<use>`. Necesaria para IE11. En SVG 2 se usa `href` simple.

**`svg4everybody`** — polyfill de JS que hace funcionar `<use>` con sprites externos en IE11. Sustituye la referencia externa por el contenido inline en el DOM.

**`SMIL`** — *Synchronized Multimedia Integration Language*. Sistema de animación nativo de SVG 1.1 (`<animate>`, `<animateTransform>`). Funciona en evergreens pero Chrome lo marcó como deprecado en 2015 — sigue vivo pero sin futuro claro.

**`SVG 2`** — última versión de la especificación. Añade `href` sin xlink, `paint-order`, `context-fill`, `context-stroke`. Soportado en evergreens.

**`@supports`** — regla CSS que permite aplicar estilos solo si el navegador soporta cierta característica. Usada para fallbacks visuales cuando una propiedad moderna no existe.

---

## Decisión y ecosistema

**`Canvas`** — API HTML de dibujo bitmap. No genera DOM por elemento — todo es una imagen rasterizada. Apropiada cuando hay miles de elementos o el rendimiento es crítico.

**`WebGL / WebGPU`** — APIs de gráficos 3D en el navegador. Para escenas con luces, sombras, materiales, o visualizaciones masivas. Much more heavy-lift que SVG.

**`Three.js`** — librería de alto nivel sobre WebGL para 3D. Estándar de facto cuando se necesita 3D real en web.

**`Observable Plot`** — librería de visualización declarativa construida sobre D3. API más simple que D3 para gráficos estadísticos estándar.

**`visx`** — librería de React de AirBnB que expone primitivas de D3 como componentes React. Útil cuando ya usas React y quieres D3 sin abandonar el paradigma.

**`Recharts`** — librería de gráficos para React basada en SVG. Alto nivel, API simple, cubre casos comunes sin la complejidad de D3.
