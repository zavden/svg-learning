# GLOSARIO — Animaciones en SVG

---

**SMIL** — *Synchronized Multimedia Integration Language*. Estándar del W3C de animación declarativa integrado en SVG. Permite animar atributos con etiquetas hijo (`<animate>`, `<animateMotion>`, `<animateTransform>`, `<set>`) sin código externo. Pasó por un intento de deprecación en 2015 revertido en 2016; hoy sigue soportado universalmente.

**Animación declarativa** — Se describe *qué* se anima y *cuándo*, no *cómo* con un bucle. SMIL y CSS son declarativos; JavaScript imperativo es lo contrario. La ventaja del declarativo: el navegador puede optimizar y pausar animaciones fuera de pantalla automáticamente.

**`<animate>`** — Elemento SMIL hijo de otro elemento SVG que interpola un atributo entre valores. Atributos clave: `attributeName`, `from`/`to` o `values`, `dur`, `begin`, `fill="freeze"`/`"remove"`, `repeatCount`.

**`<animateMotion>`** — Variante de SMIL que mueve un elemento a lo largo de un path. Puede recibir el path inline (`path="M..."`) o referenciar uno existente con `<mpath href="#id"/>`. `rotate="auto"` orienta el objeto en la dirección del trazado.

**`<animateTransform>`** — Anima una transformación (`translate`, `rotate`, `scale`, `skewX`, `skewY`) en lugar de un atributo simple. Para componer varias transformaciones al mismo tiempo se usa `additive="sum"`.

**`<set>`** — Primo "instantáneo" de `<animate>`: no interpola, salta directamente al valor final en `begin`. Ideal para mostrar/ocultar elementos o reaccionar a eventos sin código JS.

**`attributeName`** — Atributo que indica *qué* propiedad se anima. Debe coincidir con el nombre exacto del atributo SVG (`fill`, `cx`, `stroke-width`, `d`, etc.).

**`from` / `to`** — Pareja de valores inicial y final de una animación simple. Alternativa más potente: `values="v1;v2;v3;..."` para pasar por varios estados.

**`values`** — Lista de valores separados por `;` que define los puntos intermedios por los que pasa la animación. Se combina con `keyTimes` para controlar el ritmo entre ellos.

**`keyTimes`** — Lista normalizada entre 0 y 1 que dice *cuándo* se alcanza cada uno de los `values`. `values="a;b;c"` + `keyTimes="0;0.3;1"` → el valor `b` ocurre al 30% de la duración.

**`keySplines`** — Curvas de Bézier que definen la aceleración entre dos valores, equivalente a `cubic-bezier()` en CSS. Solo tiene sentido si `calcMode="spline"`.

**`calcMode`** — Cómo interpolar entre `values`. Valores: `linear` (por defecto), `discrete` (saltos), `paced` (ritmo constante en el espacio), `spline` (curvas de Bézier).

**`fill="freeze"` vs `"remove"`** — Determina qué pasa *después* de la animación. `freeze` conserva el último valor; `remove` (por defecto) vuelve al valor original. Causa frecuente de "flashes" al final de fade-ins.

**`repeatCount`** — Número de veces que se repite la animación. `indefinite` = loop infinito.

**`begin` / `end`** — Cuándo arranca o termina la animación. Acepta tiempos (`2s`), referencias a otras animaciones (`anim1.end`), eventos (`click`, `mouseover`), u `indefinite` para control manual.

**`beginElement()` / `endElement()`** — Métodos JS que permiten arrancar o terminar una animación SMIL declarada con `begin="indefinite"`. Puente entre lo declarativo y lo imperativo.

**Morphing** — Animación del atributo `d` de un path. Requiere que los dos paths tengan *el mismo número de comandos y puntos* — si no, la interpolación se rompe. Solo SMIL y JS pueden hacerlo; CSS no interpola `d`.

**Line drawing** — Técnica de "dibujar" un trazo progresivamente usando `stroke-dasharray` igual a la longitud total y animando `stroke-dashoffset` de la longitud a 0. Clásico efecto de "handwriting".

**`stroke-dasharray`** — Patrón de guiones y huecos a lo largo del trazo. Si es igual a la longitud total del path, crea un único guión del tamaño de la forma.

**`stroke-dashoffset`** — Desplazamiento del patrón de dasharray. Animarlo permite "correr" el guión fuera del path y luego hacerlo entrar.

**`pathElement.getTotalLength()`** — Método DOM que devuelve la longitud total de un path en unidades del usuario. Imprescindible para calcular exactamente `stroke-dasharray` sin adivinar.

**`@keyframes`** — Sintaxis CSS equivalente a `<animate>` con `values`. Define los estados por los que pasa una animación en porcentajes (`0%`, `50%`, `100%`).

**`prefers-reduced-motion`** — Media query CSS que detecta si el usuario activó "reducir movimiento" en su sistema. Obligación ética y WCAG: los loops infinitos deben respetarla.

**`transform-origin`** — Punto alrededor del cual se aplica una transformación CSS. El valor por defecto en SVG NO es el centro del elemento sino el origen del viewBox — fuente de bugs en rotaciones.

**Spinner** — Patrón de UI: un arco rotando indefinidamente. Se consigue con `stroke-dasharray` (mitad pintada, mitad vacía) + rotación infinita por CSS o `animateTransform`.

**Circular progress bar** — Círculo con `stroke-dasharray` = circunferencia y `stroke-dashoffset` = circunferencia × (1 − porcentaje/100). Un `rotate(-90)` lo arranca desde arriba en vez de la derecha.

**GSAP / Motion One / Anime.js** — Librerías JS populares para animaciones complejas. Superan a SMIL y CSS en sincronización, morphing inteligente y rendimiento.

**Flubber** — Librería especializada en morphing de paths que resuelve el problema del "mismo número de puntos" interpolando automáticamente.

**Web Animations API** — API nativa del navegador (`element.animate()`) que permite animaciones imperativas con sintaxis similar a `@keyframes`. Alternativa estándar a librerías como GSAP para casos simples.
