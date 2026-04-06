# Glosario — Estilado de SVG con CSS

---

**Atributo de presentación**
Atributo XML escrito directamente en el elemento SVG (`fill="red"`, `stroke-width="2"`) que define una propiedad visual. Es la forma "nativa" de SVG y la que generan los editores gráficos. Tiene la **menor prioridad** en la cascada: cualquier regla CSS lo sobrescribe.

**Estilo inline (atributo `style`)**
Cadena CSS escrita en el atributo `style="..."` del elemento. Acepta toda la sintaxis CSS moderna (`var()`, `calc()`, etc.). Tiene **alta prioridad**: solo `!important` lo supera. En SVG actúa igual que en HTML.

**Bloque `<style>`**
Elemento SVG que contiene reglas CSS aplicables al SVG completo. Puede ir en cualquier parte del SVG; los estilos no dependen de su posición. Soporta selectores, variables, `@keyframes`, `@media` y todo lo que CSS sabe hacer.

**Cascada SVG**
Algoritmo que resuelve qué valor gana cuando varias fuentes definen la misma propiedad. De menor a mayor: valor inicial → herencia → atributos de presentación → CSS por especificidad → estilo inline → `!important`.

**Especificidad CSS**
Sistema de puntuación que ordena reglas CSS de igual fuente. Selector de tipo (0,0,1) < clase (0,1,0) < id (1,0,0). Vale tanto en SVG como en HTML, pero **no** afecta a los atributos de presentación, que están "fuera" de la tabla.

**Propiedad de presentación**
Cualquier propiedad CSS que aplica visualmente a SVG. Algunas son **específicas de SVG** (`fill`, `stroke`, `text-anchor`); otras son **CSS comunes** que también funcionan en SVG (`opacity`, `transform`, `cursor`, `display`).

**`fill`**
Color (o referencia a gradiente / patrón) que rellena el interior de una forma. Acepta cualquier valor `<color>`, `url(#id)`, `currentColor`, `none`. Es heredable: si lo defines en un `<g>`, sus hijos lo reciben.

**`stroke`**
Color del trazo o "borde" de una forma. Junto con `stroke-width`, `stroke-linecap`, `stroke-linejoin` y `stroke-dasharray` controla todo lo relativo al perfilado.

**`fill-rule`**
Algoritmo que decide qué partes de un path con auto-intersecciones son "interior". Valores: `nonzero` (defecto, "regla del giro distinto de cero") y `evenodd` ("par-impar"). Importa en paths con huecos o estrellas.

**`stroke-dasharray`**
Patrón de guiones del trazo, expresado como serie de longitudes alternas (línea/hueco). `stroke-dasharray="5 3"` produce líneas de 5 unidades separadas por huecos de 3.

**`stroke-dashoffset`**
Desplazamiento inicial del patrón de guiones. Animarlo desde la longitud total del path hasta 0 produce el efecto clásico de "dibujar" el trazo.

**`text-anchor`**
Alineación horizontal del texto respecto a su punto de anclaje (`x`, `y`). Valores: `start` (defecto), `middle`, `end`. Reemplaza al `text-align` de HTML.

**`dominant-baseline`**
Línea base vertical del texto. Valores típicos: `alphabetic` (defecto), `middle`, `hanging`, `text-top`. Importante para centrar texto verticalmente sobre un punto.

**`vector-effect: non-scaling-stroke`**
Hace que el grosor del trazo **no se escale** cuando el SVG (o el elemento) se transforma. Útil para que líneas finas no se conviertan en gruesas al hacer zoom.

**`paint-order`**
Orden en que se pintan `fill`, `stroke` y `markers`. Por defecto: `fill stroke markers`. Cambiar a `stroke fill` evita que un trazo grueso "coma" el relleno (útil en texto con borde).

**`currentColor`**
Valor especial que referencia el `color` heredado del elemento padre. `<path fill="currentColor">` adopta el color del texto del contexto. Permite estilizar íconos con una sola propiedad CSS desde fuera.

**Variable CSS (Custom Property)**
Propiedad personalizada con prefijo `--`, definible en cualquier elemento. Se usa con `var(--nombre, fallback)`. En SVG son la herramienta principal de tematización: cruzan la frontera HTML→SVG inline e incluso entran en el shadow DOM de `<use>`.

**`var(--nombre, fallback)`**
Función CSS que sustituye el valor de una variable. El segundo argumento es el valor por defecto si la variable no está definida. Permite que un SVG funcione "out of the box" mientras acepta personalización opcional.

**Shadow DOM de `<use>`**
Subárbol clonado e invisible para el CSS del documento padre. Los selectores no penetran (`use circle { ... }` no funciona), pero **sí** se heredan `currentColor` y las custom properties.

**`!important`**
Marca CSS que eleva la prioridad de una declaración por encima de todas las demás. Útil como último recurso para sobrescribir un `style="..."` que no puedes editar (ej. SVG exportado por un editor).

**`transform-origin`**
Punto desde el que se aplican las transformaciones (`rotate`, `scale`). En HTML por defecto es `50% 50%` (centro). En SVG por defecto es **`0 0`** (esquina superior izquierda del sistema de coordenadas), lo que sorprende a quien viene de CSS de HTML.

**`pointer-events`**
Controla qué partes del elemento responden a eventos de ratón/touch. Valores SVG-específicos: `visiblePainted`, `visibleFill`, `visibleStroke`, `painted`, `fill`, `stroke`, `all`, `none`. Permite, por ejemplo, hacer un círculo "vacío" clickeable solo en el borde.

**`mix-blend-mode`**
Modo de mezcla del elemento con el contenido debajo. Valores: `multiply`, `screen`, `overlay`, etc. Funciona en SVG igual que en HTML, pero a veces requiere `isolation: isolate` en un ancestro para crear un contexto de mezcla.

**`@keyframes` en SVG**
Animaciones CSS definidas dentro del bloque `<style>` del SVG. Funcionan exactamente igual que en HTML y pueden animar propiedades específicas de SVG (`fill`, `stroke-dashoffset`, `r`, etc.).

**`<![CDATA[]]>`**
Sección XML que marca el contenido como "datos de caracteres" no parseable. Necesario en archivos `.svg` standalone cuando el bloque `<style>` contiene caracteres reservados de XML (`<`, `&`). En HTML5 inline no hace falta.
