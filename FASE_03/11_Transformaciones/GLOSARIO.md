# Glosario — Transformaciones

---

**`transform`**
Atributo SVG que modifica el sistema de coordenadas local del elemento. Acepta una o más funciones de transformación separadas por espacios: `translate()`, `rotate()`, `scale()`, `skewX()`, `skewY()`, `matrix()`. Se aplica al elemento y a todos sus descendientes.

---

**sistema de coordenadas local**
Cada elemento transformado vive en su propio sistema de referencia. Sus coordenadas internas (`x`, `y`, `cx`, `cy`) se interpretan dentro de ese sistema. Las transformaciones del padre se acumulan en el sistema de los hijos.

---

**`translate(tx, ty)`**
Desplaza el sistema de coordenadas `tx` unidades en X y `ty` en Y. Si se omite `ty`, vale 0. No cambia las coordenadas internas del elemento; mueve el "origen" del espacio. Ideal para posicionar grupos.

---

**`scale(sx, sy)`**
Escala el sistema de coordenadas desde el origen `(0,0)`. Si se omite `sy`, vale `sx` (escala uniforme). Valores negativos producen efecto espejo: `scale(-1,1)` = espejo horizontal. La escala también afecta a `stroke-width` y `font-size`.

---

**`rotate(angle, cx, cy)`**
Rota el sistema de coordenadas `angle` grados en sentido horario (eje Y invertido en SVG). Sin `cx`, `cy`: rota desde el origen `(0,0)`. Con `cx`, `cy`: rota desde ese punto de pivote. Equivale a `translate(cx,cy) rotate(angle) translate(-cx,-cy)`.

---

**`skewX(angle)`**
Inclina el sistema de coordenadas en el eje X. Cada punto se desplaza horizontalmente en proporción a su coordenada Y. Produce efecto de perspectiva lateral y paralelogramos.

---

**`skewY(angle)`**
Inclina el sistema de coordenadas en el eje Y. Cada punto se desplaza verticalmente en proporción a su coordenada X.

---

**`matrix(a, b, c, d, e, f)`**
Transformación expresada directamente como matriz 2D. Los parámetros `e` y `f` son la traslación; `a` y `d` la escala; `b`, `c` la rotación/skew. Todas las demás funciones son casos especiales de `matrix`. Se usa principalmente en código generado o calculado programáticamente.

---

**encadenamiento de transformaciones**
Múltiples transformaciones en el mismo atributo `transform`. El orden de escritura importa: `translate(50,0) rotate(45)` es diferente a `rotate(45) translate(50,0)`. La regla: las transformaciones se aplican de derecha a izquierda al sistema de coordenadas (o de izquierda a derecha desde la perspectiva del elemento).

---

**`transform-origin`**
Propiedad CSS que define el punto de pivote para las transformaciones (rotate, scale, skew). En SVG 2, acepta valores en px o `center`. Importante: los porcentajes en SVG se calculan respecto al viewport del SVG, no del elemento (diferente al comportamiento en HTML).

---

**pivote (punto de pivote)**
El punto alrededor del cual se aplica una transformación de rotación o escala. Por defecto en SVG es el origen `(0,0)`. Se puede cambiar con `rotate(a, cx, cy)`, con el patrón `translate/rotate/translate`, o con `transform-origin`.

---

**CTM (Current Transformation Matrix)**
La matriz acumulada que combina todas las transformaciones de la jerarquía hasta un elemento dado. Se obtiene con `element.getCTM()` en JavaScript. Útil para convertir coordenadas del mouse (en píxeles de pantalla) a coordenadas del elemento SVG.

---

**herencia de transformaciones**
Los hijos de un elemento transformado heredan su sistema de coordenadas. Un `<g transform="translate(100,100)">` hace que todos sus hijos tengan `(100,100)` como nuevo origen, sin que los hijos necesiten especificar ninguna transformación.

---

**espejo (reflexión)**
Transformación que invierte un eje. `scale(-1,1)` refleja horizontalmente, `scale(1,-1)` verticalmente. Útil para reutilizar formas asimétricas (una flecha izquierda es la misma que una derecha con `scale(-1,1)`).
