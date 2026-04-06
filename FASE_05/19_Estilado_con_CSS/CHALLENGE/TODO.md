# Retos — Estilado de SVG con CSS

Cada reto explora una capa distinta de la cascada SVG. Lee el enunciado, intenta resolverlo y compáralo con tu intuición previa: muchos enseñan algo contraintuitivo.

---

## Reto 1: Demostración de la cascada

Crea un solo SVG con **un círculo** y aplica las cinco fuentes de estilo en orden, comentando la salida visual esperada en cada paso:

1. Atributo de presentación `fill="blue"`.
2. Una regla `circle { fill: green; }` en `<style>`.
3. Una clase `.activo { fill: orange; }` añadida al círculo.
4. Un id `#principal { fill: purple; }` añadido al círculo.
5. Un `style="fill: red;"` añadido al círculo.
6. (Bonus) Una regla `circle { fill: black !important; }`.

Documenta en comentarios HTML qué color se ve en cada paso y por qué. El objetivo es interiorizar que "el atributo pierde contra todas las reglas CSS".

---

## Reto 2: Logo tematizable con variables CSS

Diseña un logo SVG simple (3-5 elementos) que use **únicamente variables CSS** para todos sus colores. El SVG debe:

- Funcionar "por defecto" sin que el padre defina nada (gracias a `var(--c, fallback)`).
- Aceptar al menos 3 propiedades públicas: `--logo-primario`, `--logo-acento`, `--logo-fondo`.
- Documentar las variables en un comentario al inicio del SVG, como una "API".

Crea **tres versiones del mismo logo** en una página HTML, cada una con un tema distinto aplicado vía clase del padre (`.tema-claro`, `.tema-oscuro`, `.tema-verde`). Verifica que el archivo SVG es **idéntico** en los tres casos.

---

## Reto 3: Sprite bicolor con `<symbol>` y variables

Crea un sprite con un `<symbol id="bicolor">` que contenga **dos elementos** (por ejemplo, un cuerpo y un detalle). Cada elemento debe tener su color como variable CSS distinta (`--c1`, `--c2`).

Renderiza **5 instancias** del mismo símbolo con `<use>`, cada una con una combinación distinta de colores aplicados vía `style="--c1: ...; --c2: ...;"`.

**Pregunta**: ¿por qué este patrón funciona aunque los selectores CSS normales no penetran en `<use>`?

---

## Reto 4: Botón interactivo solo con CSS

Construye un botón SVG (puede ser un círculo grande con un ícono dentro) que reaccione a `:hover`, `:focus` y `:active` **únicamente con CSS**, sin JavaScript. Requisitos:

- En `:hover`: cambia de color y escala 1.1.
- En `:active`: escala 0.95 y oscurece.
- En `:focus-visible`: añade un outline azul de 2px.
- Transiciones suaves de 200ms en todos los cambios.
- `transform-origin` correctamente definido (cuidado con el defecto de SVG).

Recuerda añadir `tabindex="0"` y `role="button"` para que sea accesible.

---

## Reto 5: Auditoría de un SVG exportado

Toma un SVG real exportado de un editor (Figma, Illustrator, Inkscape, o descarga un ícono de [heroicons.com](https://heroicons.com)) y analiza:

1. ¿Usa atributos de presentación (`fill="..."`) o estilo inline (`style="fill: ..."`)?
2. Si usa estilo inline, **conviértelo** a atributos para que sea tematizable.
3. Reemplaza los colores hardcodeados por `currentColor` o por una variable CSS.
4. Verifica que ahora puedes cambiarle el color desde fuera con una sola regla CSS.

Documenta el "antes" y el "después" en un README. Anota qué tuviste que cambiar.

---

## Reto 6: Modo oscuro automático con `prefers-color-scheme`

Crea un SVG standalone (archivo `.svg` que se pueda abrir directo en el navegador) con `<style>` interno y `<![CDATA[]]>` que **cambie de paleta automáticamente** según `prefers-color-scheme`. Debe:

- Tener al menos un fondo, un trazo y un texto.
- En modo claro: fondo blanco, trazo oscuro, texto oscuro.
- En modo oscuro: fondo oscuro, trazo claro, texto claro.
- Usar variables CSS y `@media (prefers-color-scheme: dark)` dentro del bloque `<style>`.
- Funcionar tanto al abrirlo directamente como al cargarlo con `<img>` desde una página HTML.

**Prueba**: cambia el modo de tu sistema operativo y observa que el SVG cambia sin recargar (en navegadores compatibles).
