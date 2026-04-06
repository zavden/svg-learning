# Retos — Integración de SVG en la web

Cada reto pide aplicar uno o varios métodos de integración con sus pros y contras conscientes. Usa el método correcto para cada caso, no el más cómodo.

---

## Reto 1: Sistema de íconos con tres modalidades

Construye una página HTML con **tres versiones del mismo ícono** (un corazón) usando tres métodos distintos:

1. **SVG inline** que cambie de color al pasar el ratón con CSS hover.
2. **`<img>`** con `loading="lazy"` y `alt="Me gusta"`.
3. **`background-image` CSS** con un data URI URL-encoded.

Para cada uno, anota debajo en un párrafo cuándo elegirlas en un proyecto real.

**Bonus**: añade un cuarto botón que cargue el SVG con `<object>` y use `getSVGDocument()` para cambiar el `fill` desde JavaScript del padre.

---

## Reto 2: Logo con fallback PNG y accesibilidad

Crea una cabecera con un logo de marca incrustado con `<object type="image/svg+xml">`. Requisitos:

- Si el SVG falla (renombra el archivo para forzarlo), debe mostrar un PNG fallback con `alt`.
- Debe tener `aria-label="Inicio"` y estar dentro de un enlace `<a href="/">`.
- El PNG fallback debe ser de tamaño correcto (no aparecer pixelado).

Documenta qué pasa cuando inspeccionas el `<object>` en DevTools mientras el SVG carga vs cuando falla.

---

## Reto 3: Patrón de fondo decorativo

Diseña un patrón geométrico (puntos, líneas diagonales, hexágonos…) y úsalo como `background-image` repetido en una sección de tu página. Reglas:

- El SVG **debe ir como data URI URL-encoded en el CSS**, no como archivo externo.
- Debe pesar menos de 500 bytes una vez codificado.
- Debe parametrizar el color principal a través de una variable CSS (vía `currentColor` no funciona en background; tendrás que generar dos versiones o usar `<img>` + `mask-image`).

**Pregunta para el ejercicio**: ¿qué ventaja tiene aquí el data URI sobre un archivo `.svg` separado?

---

## Reto 4: Editor SVG en `<iframe>` con `postMessage`

Crea dos archivos:

1. `editor.svg`: contiene un círculo grande con un `<script>` interno que escucha `message` events. Cuando recibe `{ tipo: 'cambiar-color', color: '#xxx' }`, cambia el `fill` del círculo.
2. `index.html`: incrusta `editor.svg` con `<iframe>` (con `title` accesible) y tiene tres botones que envían colores distintos vía `postMessage`.

**Pregunta**: ¿por qué este caso necesita `<iframe>` y no se puede hacer con `<object>`? Justifícalo en un párrafo en el README del reto.

---

## Reto 5: Comparación de pesos en disco

Toma un mismo SVG mediano (entre 5 y 15 KB, por ejemplo un mapa simplificado o una ilustración isométrica) y mide cuánto pesa cuando lo incrustas como:

- Archivo externo `.svg` cargado con `<img>`.
- SVG inline en HTML.
- Data URI URL-encoded en CSS.
- Data URI base64 en CSS.

Crea una tabla con los cuatro pesos y otra tabla con el número de peticiones HTTP que dispara cada método. Discute el resultado en términos de cache, primer render y peso del HTML inicial.

---

## Reto 6: Migración de íconos a sprite con `<use>`

Parte de una página con 6 íconos SVG inline duplicados. Refactoriza para que:

- Exista un único `icons.svg` con 6 `<symbol id="...">`.
- Cada uso de un ícono sea `<svg><use href="/icons.svg#nombre"/></svg>`.
- Los íconos hereden color con `fill="currentColor"` desde el CSS de cada contexto.

Compara con DevTools el peso del HTML antes y después, y el número de peticiones HTTP. Discute qué ganas (cache de iconos compartida) y qué pierdes (no puedes estilizar partes individuales del símbolo desde fuera).
