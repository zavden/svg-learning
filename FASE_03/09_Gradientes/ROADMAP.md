# Seccion 9: Gradientes

> Los gradientes permiten transiciones suaves entre colores. En SVG se definen como
> recursos reutilizables en `<defs>` y se referencian con `url(#id)`.

---

## 9.1 Gradiente lineal `<linearGradient>`

### Naturaleza del gradiente lineal
- Transicion de colores a lo largo de una **linea recta imaginaria**
- La linea tiene un punto de inicio y un punto final
- Los colores van del inicio al final siguiendo esa direccion
- Todo lo que esta "perpendicular" a la linea tiene el mismo color en ese punto

### Donde se define
- Siempre dentro de `<defs>` — es un recurso, no un elemento grafico
- Requiere un `id` unico para poder referenciarlo
- No produce nada visible por si solo hasta que se usa en un `fill` o `stroke`

### Atributos de direccion

**`x1`, `y1` — Punto de inicio del gradiente**
- Coordenadas del punto donde empieza el primer color
- Valor por defecto: `x1="0%"` `y1="0%"` (esquina superior izquierda del bounding box del elemento)

**`x2`, `y2` — Punto final del gradiente**
- Coordenadas del punto donde termina el ultimo color
- Valor por defecto: `x2="100%"` `y2="0%"` (esquina superior derecha → gradiente horizontal)

### Direcciones comunes y sus valores

| Direccion | x1 | y1 | x2 | y2 |
|-----------|----|----|----|----|
| Horizontal (izq → der) | `0%` | `0%` | `100%` | `0%` |
| Horizontal (der → izq) | `100%` | `0%` | `0%` | `0%` |
| Vertical (arr → abj) | `0%` | `0%` | `0%` | `100%` |
| Vertical (abj → arr) | `0%` | `100%` | `0%` | `0%` |
| Diagonal (↘) | `0%` | `0%` | `100%` | `100%` |
| Diagonal (↗) | `0%` | `100%` | `100%` | `0%` |

### Angulos arbitrarios
- Para un angulo exacto que no sea diagonal, se pueden usar coordenadas trigonometricas
- O mas practico: usar `gradientTransform="rotate(angulo, 0.5, 0.5)"` sobre un gradiente horizontal base
- El centro de rotacion `(0.5, 0.5)` en `objectBoundingBox` es el centro del elemento

---

## 9.2 Gradiente radial `<radialGradient>`

### Naturaleza del gradiente radial
- Transicion de colores que **irradia desde un centro** hacia el exterior
- El color del centro (o del punto focal) va al color del borde exterior
- Produce efectos de "esfera", "luz", "resplandor"

### Atributos principales

**`cx`, `cy` — Centro del gradiente**
- Coordenadas del centro del circulo exterior del gradiente
- Por defecto: `cx="50%"` `cy="50%"` (centro del bounding box del elemento)
- El color final de los stops se alcanza en este circulo exterior

**`r` — Radio del gradiente**
- Radio del circulo exterior donde termina el gradiente
- Por defecto: `r="50%"` (la mitad de la dimension del bounding box)
- Mas alla de este radio: el color del ultimo stop se extiende (segun `spreadMethod`)

**`fx`, `fy` — Punto focal**
- El punto desde el que "irradia" el gradiente
- Por defecto: igual a `cx` y `cy` (el gradiente irradia desde el centro)
- Al desplazar `fx`/`fy` del centro: el gradiente se vuelve asimetrico, como una luz que no esta centrada
- Produce efectos de iluminacion lateral, highlights en esferas

**`fr` — Radio focal (SVG 2)**
- Radio del circulo focal (el circulo donde empieza el primer color)
- Por defecto: `fr="0"` (el primer color parte de un punto)
- Con `fr > 0`: el primer color ocupa un circulo en el centro antes de la transicion
- Util para crear anillos de color o efectos de "donuts" luminosos

### Efecto del punto focal desplazado

Cuando `fx != cx` o `fy != cy`:
- El color del primer stop aparece concentrado alrededor del punto focal
- El gradiente se "estira" desde el focal hasta el borde del circulo definido por cx/cy/r
- Crea la ilusion de una fuente de luz que no esta en el centro
- Clasico uso: highlights de esferas 3D (punto focal desplazado hacia arriba-izquierda)

---

## 9.3 Stops de color `<stop>`

### Que es un stop
- Cada `<stop>` define un **punto de color** a lo largo del gradiente
- El gradiente interpola suavemente entre stops consecutivos
- Son hijos directos del elemento `<linearGradient>` o `<radialGradient>`

### Atributos del `<stop>`

**`offset` — Posicion en el gradiente**
- Donde se situa este color dentro del gradiente
- Valores aceptados:
  - Fraccion decimal: `0`, `0.5`, `1`
  - Porcentaje: `0%`, `50%`, `100%`
  - Ambos son equivalentes: `0.5` = `50%`
- `offset="0"` o `offset="0%"`: inicio del gradiente
- `offset="1"` o `offset="100%"`: final del gradiente
- Valores intermedios: puntos exactos de color a lo largo de la transicion

**`stop-color` — Color en este punto**
- El color que el gradiente tiene en la posicion definida por `offset`
- Acepta todos los formatos de color: nombres, hex, rgb, hsl, `currentColor`
- Si se omite: negro por defecto

**`stop-opacity` — Opacidad en este punto**
- Opacidad del color en este stop
- Rango: `0` (transparente) a `1` (opaco)
- Permite gradientes que van de un color opaco a transparente
- Si se omite: `1` (totalmente opaco) por defecto

### Reglas para los stops

**Minimo de dos stops**
- Con un solo stop, no hay transicion: se obtiene un color solido
- El gradiente mas simple tiene un stop al inicio y otro al final

**Orden ascendente de offset**
- Los stops deben declararse con offsets en orden creciente
- Si los offsets estan desordenados, el comportamiento es indefinido

**Stops con el mismo offset**
- Dos stops en el mismo `offset`: crea un cambio abrupto de color en ese punto
- Util para crear gradientes "duro" sin transicion suave entre dos zonas

**Stops fuera del rango 0-1**
- Los stops con `offset < 0` o `offset > 1` existen pero quedan fuera del rango visible del gradiente
- Pueden ser utiles junto con `spreadMethod` para controlar el inicio/fin de la transicion

### Gradientes de transparencia
- `stop-opacity="0"` en un stop crea una zona transparente en el gradiente
- Combinar `stop-color="negro" stop-opacity="0.5"` en el inicio con `stop-opacity="0"` al final: sombra que se desvanece
- Los gradientes de transparencia son muy usados para sombras, reflejos y overlays

---

## 9.4 Atributo `gradientUnits`

### El problema que resuelve
- Los gradientes necesitan un sistema de coordenadas para sus puntos (x1, y1, cx, cy, etc.)
- `gradientUnits` define en que sistema de coordenadas estan esas posiciones

### `objectBoundingBox` (valor por defecto)

**Que significa**
- Las coordenadas son **relativas al bounding box del elemento** que usa el gradiente
- Rango tipico: 0 a 1 (o 0% a 100%)
- `x1="0"` = borde izquierdo del elemento, `x1="1"` = borde derecho

**Ventajas**
- El gradiente se adapta automaticamente al tamano de cada elemento que lo usa
- Un rectangulo de 200x100 y un circulo de radio 50 pueden compartir el mismo gradiente y ambos se ven correctamente
- Es el comportamiento mas intuitivo para la mayoria de casos

**Limitacion**
- Si el elemento tiene tamano cero en una dimension (altura o ancho = 0), el gradiente falla
- No sirve si se quiere que el gradiente tenga un tamano fijo independiente del elemento

### `userSpaceOnUse`

**Que significa**
- Las coordenadas son absolutas en el **sistema de coordenadas del usuario** (el viewBox)
- Se usan los mismos numeros que para posicionar elementos SVG

**Cuando usarlo**
- Cuando varios elementos deben compartir el **mismo gradiente continuo** (como si fueran partes de una sola forma)
- Cuando el gradiente debe tener dimensiones fisicas especificas independientes del elemento
- Para gradientes que van del punto (0,0) al punto (100,100) del viewBox, sin importar el tamano del elemento

**Desventaja**
- Si el elemento se mueve o redimensiona, el gradiente no se mueve con el: hay que actualizar las coordenadas del gradiente

---

## 9.5 Atributo `spreadMethod`

### Para que sirve
- Define que ocurre mas alla de los limites del gradiente (fuera del rango 0-100%)
- Solo relevante cuando el gradiente no cubre todo el area del elemento (rango de stops menor que el area)

### `pad` (valor por defecto)

**Comportamiento**
- El color del primer stop se extiende hacia el inicio
- El color del ultimo stop se extiende hacia el final
- El gradiente tiene "relleno" de color solido mas alla de sus limites

**Cuando se ve**
- Con gradientes mas pequenios que el elemento: el primer/ultimo color rellena las zonas sin gradiente

### `reflect`

**Comportamiento**
- El gradiente se repite en **espejo** cada vez que llega a su limite
- Si el gradiente va de azul a rojo: repeticion es rojo→azul, luego azul→rojo, etc.
- Las repeticiones son siempre suaves (sin saltos de color en las uniones)

**Efecto visual**
- Crea patrones ondulantes de color
- Util para efectos de onda, seda, o reflejos multiples

### `repeat`

**Comportamiento**
- El gradiente se repite desde el inicio cada vez que llega a su limite
- La repeticion crea un salto abrupto si el primer y ultimo color son diferentes
- Si el primer y ultimo color son iguales, la repeticion es suave

**Efecto visual**
- Bandas de color repetidas
- Util para efectos de rayas, vibrato, o patrones repetitivos de color

---

## 9.6 Transformaciones de gradientes

### El atributo `gradientTransform`
- Aplica una transformacion SVG al gradiente completo
- Acepta los mismos valores que el atributo `transform` de los elementos: `translate`, `scale`, `rotate`, `skewX`, `skewY`, `matrix`
- Se aplica en el espacio de coordenadas del gradiente

### Casos de uso comunes

**Rotar un gradiente**
- `gradientTransform="rotate(45)"`: gradiente a 45° sin tener que calcular x1/y1/x2/y2 diagonales
- Mas conveniente que calcular las coordenadas exactas para angulos arbitrarios
- Cuando se usa con `gradientUnits="objectBoundingBox"`, el centro de rotacion es (0,0): hay que ajustar el centro si se quiere rotar desde el centro del elemento

**Rotar desde el centro del elemento (objectBoundingBox)**
- Centrar en (0.5, 0.5): `gradientTransform="rotate(45, 0.5, 0.5)"`
- El segundo y tercer parametro de `rotate` son el centro de rotacion

**Escalar o sesgar el gradiente**
- `gradientTransform="scale(0.5)"`: el gradiente ocupa solo la mitad del espacio
- `gradientTransform="skewX(20)"`: inclina el gradiente horizontalmente

---

## 9.7 Referenciar gradientes

### Como se usa un gradiente definido

**En el atributo `fill`**
- `fill="url(#idDelGradiente)"`: el interior de la forma usa el gradiente como color
- Aplica a cualquier forma: `<rect>`, `<circle>`, `<path>`, `<text>`, etc.

**En el atributo `stroke`**
- `stroke="url(#idDelGradiente)"`: el trazo de la forma usa el gradiente
- Util para lineas y contornos con transicion de color

**En CSS**
- `fill: url(#idDelGradiente);` en una regla CSS o en el atributo `style`
- Misma sintaxis que como atributo, pero en el contexto CSS

### Herencia de gradientes con `href`

**Reutilizar un gradiente como base**
- Un gradiente puede heredar las propiedades de otro con `href="#idDelGradientePadre"` (SVG 2) o `xlink:href="#id"` (SVG 1.1)
- Solo se sobreescriben los atributos especificados explicitamente en el hijo
- Los stops tambien se pueden heredar o sobreescribir

**Caso de uso tipico**
- Definir un gradiente base con sus stops y colores
- Crear variantes del mismo gradiente cambiando solo la direccion (x1/y1/x2/y2) o el spreadMethod
- Evita repetir los stops para gradientes con la misma paleta pero diferente orientacion

### Gradientes en texto
- El texto SVG puede tener `fill="url(#gradiente)"`: el texto muestra el gradiente
- Con `gradientUnits="objectBoundingBox"`: el gradiente se adapta al bounding box del texto completo
- Con `gradientUnits="userSpaceOnUse"`: el gradiente tiene posicion fija en el viewport
- El resultado es texto con transicion de color: efecto muy usado en logotipos y titulos
