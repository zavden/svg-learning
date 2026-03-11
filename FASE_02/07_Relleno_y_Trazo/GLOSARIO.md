# Glosario — Relleno y Trazo

---

**fill**
Propiedad que define el color de relleno del interior de una forma. El valor por defecto es `black`. Acepta colores CSS, `none` (sin relleno), `currentColor`, o `url(#id)` para gradientes y patrones.

---

**fill-opacity**
Controla la opacidad únicamente del relleno (`fill`), de `0` (transparente) a `1` (opaco). A diferencia de `opacity`, no afecta al stroke ni a los elementos hijos.

---

**fill-rule**
Determina qué áreas del interior de un path con sub-trazados se consideran "dentro". Valores: `nonzero` (por defecto, regla de no-cero) y `evenodd` (par-impar, produce agujeros en formas anidadas).

---

**stroke**
Propiedad que define el color del trazo (contorno) de una forma. El valor por defecto es `none` (sin trazo visible). Una `<line>` sin `stroke` es completamente invisible.

---

**stroke-width**
Grosor del trazo en unidades de usuario. El stroke se dibuja centrado sobre el borde geométrico: `stroke-width/2` queda dentro y `stroke-width/2` fuera de la forma. Se escala con las transformaciones.

---

**stroke-opacity**
Controla la opacidad únicamente del trazo, de `0` a `1`. Permite combinar fill opaco con trazo semitransparente, o viceversa.

---

**stroke-linecap**
Define la forma de los extremos (inicio y fin) de líneas abiertas. Valores: `butt` (plana, sin extensión), `round` (semicircular), `square` (rectangular, igual extensión que `round`). Solo aplica a líneas abiertas, no a formas cerradas.

---

**stroke-linejoin**
Define la forma de la esquina donde dos segmentos se unen. Valores: `miter` (punta aguda), `round` (redondeada), `bevel` (achaflanada). Aplica a vértices de `<polyline>`, `<polygon>` y paths.

---

**stroke-miterlimit**
Límite para el valor de `miter` en `stroke-linejoin`. Si la longitud de la punta supera `miterlimit × stroke-width`, la unión cambia automáticamente a `bevel`. Por defecto: `4`. Valor `1` equivale a siempre usar `bevel`.

---

**stroke-dasharray**
Convierte una línea continua en una línea discontinua. Define el patrón de trazos y espacios que se repite. Un valor: trazo=espacio. Dos valores: trazo, espacio. Múltiples: trazo, espacio, trazo, espacio...

---

**stroke-dashoffset**
Desplaza el punto de inicio del patrón de guiones a lo largo de la línea. Positivo: el patrón se "retrasa". Negativo: se "adelanta". Base de la técnica de animación *line drawing*: animar de longitud-total a `0`.

---

**paint-order**
Controla el orden en que se pintan fill, stroke y markers. Por defecto: `fill stroke markers` (el stroke queda encima del fill). Con `paint-order="stroke fill"`, el stroke se pinta primero y el fill queda encima, haciendo que el stroke actúe como contorno exterior sin cubrir el interior.

---

**vector-effect**
Modifica cómo se aplican las transformaciones al stroke. `non-scaling-stroke` hace que el grosor del trazo permanezca constante en píxeles de pantalla, sin escalar con las transformaciones del elemento. Útil para grids y diagramas con zoom.

---

**currentColor**
Valor especial de color que hereda el valor de la propiedad CSS `color` del elemento o sus ancestros. Permite que el SVG "recoja" el color de texto del contexto HTML donde está embebido.

---

**cascada SVG**
Orden de prioridad de los estilos en SVG (de menor a mayor): herencia → atributos de presentación (`fill="..."`) → reglas CSS → `style` inline → `!important`. Los atributos de presentación tienen menor prioridad que las reglas CSS normales.

---

**Modelo del pintor**
Principio de renderizado SVG: cada elemento se pinta sobre los anteriores. El último elemento en el código queda visualmente encima. Dentro de un mismo elemento, el fill se pinta antes que el stroke (modificable con `paint-order`).
