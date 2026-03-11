# Seccion 7: Relleno y Trazo (Fill & Stroke)

> Fill y stroke son las dos propiedades de pintura fundamentales en SVG.
> Controlan casi todo lo que se ve en un grafico. Sus detalles son mas ricos de lo que parecen.

---

## 7.1 Propiedades de relleno (fill)

### Que es el fill
- El **fill** (relleno) es el color que ocupa el **interior** de una forma
- Para formas cerradas (`<rect>`, `<circle>`, `<polygon>`, paths cerrados): es el area interior
- Para formas abiertas (`<line>`, `<polyline>`, paths abiertos): el fill tiene un efecto visual peculiar (se "cierra" imaginariamente entre el inicio y el fin)
- El fill se pinta **antes** que el stroke (el stroke queda encima del fill por defecto)

### Propiedad `fill`

**Valores aceptados**
- Cualquier color CSS: nombre (`red`), hex (`#FF0000`), `rgb()`, `hsl()`, etc.
- `none`: sin relleno (el interior de la forma es completamente transparente)
- `transparent`: tecnicamente tiene relleno pero invisible (diferente a `none` en cuanto a eventos)
- `currentColor`: hereda el valor de la propiedad CSS `color` del elemento o sus ancestros
- `url(#id)`: referencia a un gradiente, patron u otro elemento de pintura definido en `<defs>`

**Valor por defecto: `black`**
- Todas las formas SVG tienen `fill="black"` por defecto
- Esto sorprende a principiantes: un SVG sin fill explicito muestra todas las formas en negro
- Siempre especificar `fill` explicitamente para evitar sorpresas

**`fill="none"` — Cuando usarlo**
- Formas que solo deben mostrar su contorno (stroke), sin area de color interior
- `<polyline>` casi siempre necesita `fill="none"` para verse correctamente
- Paths que representan lineas o contornos sin area

### Propiedad `fill-opacity`

- Controla la **opacidad del relleno** de forma independiente al stroke
- Rango: `0` (completamente transparente) a `1` (completamente opaco)
- Fracciones: `0.5` es 50% de opacidad
- Es diferente a `opacity`: `fill-opacity` solo afecta al relleno, no al stroke ni a los hijos

**`fill-opacity` vs `opacity`**
| Propiedad | Afecta a |
|-----------|---------|
| `fill-opacity` | Solo el relleno |
| `stroke-opacity` | Solo el trazo |
| `opacity` | Todo el elemento (fill + stroke + hijos) |

### Propiedad `fill-rule`

Determina que partes del interior de un path se consideran "dentro" cuando hay sub-trazados o cruces:
- `nonzero` (por defecto): regla de no-cero (ver seccion 5.10 para el algoritmo)
- `evenodd`: regla par-impar; produce agujeros en formas anidadas

---

## 7.2 Propiedades de trazo (stroke)

### Que es el stroke
- El **stroke** (trazo) es la linea que se dibuja **sobre o alrededor del borde** de una forma
- Para formas cerradas: es el contorno perimetral
- Para `<line>` y `<polyline>`: es la unica parte visible (no tienen fill significativo)
- El stroke se dibuja **centrado** sobre el borde geometrico de la forma

**El stroke es centrado: implicacion importante**
- Un `<rect width="100" height="100">` con `stroke-width="10"` tiene su trazo centrado en el borde
- 5px del trazo quedan **dentro** del rectangulo y 5px **fuera**
- El "area visual" del rectangulo es de 110x110px, pero el bounding box geometrico sigue siendo 100x100
- Para que el stroke no salga del bounding box: usar `stroke-width` <= 2*min_distance_to_edge

### Propiedad `stroke`

**Valor por defecto: `none`**
- Las formas no tienen trazo visible a menos que se especifique
- `<line>` sin `stroke` = completamente invisible

**Valores aceptados**
- Mismos que `fill`: nombres de color, hex, rgb, hsl, `none`, `currentColor`, `url(#id)`

### Propiedad `stroke-width`

- Grosor del trazo en unidades de usuario
- Valor por defecto: `1` (cuando hay stroke definido)
- Acepta valores decimales: `stroke-width="0.5"` para trazos muy finos
- Se escala con las transformaciones (a menos que se use `vector-effect="non-scaling-stroke"`)
- Un `stroke-width="0"` hace el trazo invisible

### Propiedad `stroke-opacity`

- Opacidad del trazo de forma independiente al fill
- Rango: `0` a `1`
- Permite tener un fill opaco con un trazo semitransparente, o viceversa

---

## 7.3 Terminaciones de linea (stroke-linecap)

### Que controla `stroke-linecap`
- La forma del **extremo de una linea abierta** (inicio y fin)
- Solo aplica a `<line>`, `<polyline>`, y paths abiertos (sin `Z`)
- No aplica a formas cerradas (`<rect>`, `<circle>`, paths con `Z`)

### Los tres valores

**`butt`** (por defecto)
- La linea termina **exactamente en el punto final** de la coordenada
- No hay extension mas alla del punto
- La terminacion es plana, perpendicular a la direccion de la linea
- El extremo no agrega longitud adicional a la linea

**`round`**
- La linea termina con un **semicirculo** cuyo diametro es igual al `stroke-width`
- El semicirculo se extiende `stroke-width/2` mas alla del punto final
- Hace la linea visualmente mas larga que con `butt`
- Util para trazos que deben verse "redondeados" en sus extremos (lapices, pinceles)

**`square`**
- La linea termina con un **cuadrado** (rectangulo) cuya mitad se extiende mas alla del punto final
- La extension es `stroke-width/2` mas alla del punto, igual que `round`
- A diferencia de `round`, la terminacion es plana (no curva)
- Visualmente: igual longitud que `round` pero con esquinas rectas

**Diferencia visual entre `round` y `square`**
- Ambos extienden la linea la misma distancia mas alla del punto final
- `round`: terminacion semicircular (suave)
- `square`: terminacion rectangular (angular)
- `butt`: sin extension (la mas corta)

**Cuando importa `stroke-linecap`**
- Para trazos gruesos donde la forma del extremo es visible
- Para crear efectos de "pincelada" o "lapiz" con `round`
- Para garantizar que dos lineas que se unen visualmente se toquen exactamente (con `butt`)

---

## 7.4 Uniones de linea (stroke-linejoin)

### Que controla `stroke-linejoin`
- La forma de la **esquina donde dos segmentos de linea se unen**
- Aplica a vertices de `<polyline>`, `<polygon>`, y paths con multiples segmentos
- Aplica cuando el camino "dobla" en un punto (no cuando termina)

### Los valores

**`miter`** (por defecto)
- Union en **punta**: los dos bordes exteriores del trazo se extienden hasta encontrarse en un punto agudo
- Produce esquinas "afiladas" o "puntiagudas"
- Para angulos muy cerrados, la punta puede volverse extremadamente larga → controlada por `stroke-miterlimit`

**`round`**
- Union **redondeada**: se coloca un arco circular en la esquina exterior
- Produce esquinas suaves y organicas
- El radio del arco es igual a `stroke-width/2`

**`bevel`**
- Union **achaflanada**: se corta la punta y se reemplaza por una linea recta
- Produce esquinas "cortadas" o "planas"
- Es como `miter` pero con la punta cortada

### La propiedad `stroke-miterlimit`

**El problema de las puntas en `miter`**
- En angulos muy agudos, la punta del `miter` puede extenderse muchisimo
- Una flecha apuntando con 10° de apertura tendria una punta enorme
- `stroke-miterlimit` limita cuanto puede crecer esa punta

**Como funciona**
- Valor: una proporcion. Si la longitud de la punta supera `stroke-miterlimit * stroke-width`, la union cambia automaticamente a `bevel`
- Valor por defecto: `4` (la punta puede ser hasta 4 veces el grosor del trazo)
- Valor `1`: la punta nunca puede crecer → equivale a siempre usar `bevel`
- Valores grandes permiten puntas mas pronunciadas

**Calculo del limite**
- La longitud de la punta miter = `stroke-width / sin(angulo/2)`
- Para un angulo de 90°: punta = `stroke-width / sin(45°)` ≈ `1.41 * stroke-width` (bien dentro del limite por defecto)
- Para un angulo de 28.96°: punta = `2 * stroke-width` = el limite default 4 comienza a actuar a ≈ 29°

---

## 7.5 Lineas discontinuas (stroke-dasharray)

### Que es `stroke-dasharray`
- Convierte una linea continua en una linea **punteada o discontinua**
- Define un **patron de trazos y espacios** que se repite a lo largo de la linea
- Es una de las propiedades mas usadas para efectos decorativos y animaciones

### Sintaxis y valores

**Un solo valor**
- `stroke-dasharray="5"`: trazos de 5 unidades y espacios de 5 unidades (el espacio se asume igual al trazo)

**Dos valores**
- `stroke-dasharray="5 2"`: trazos de 5, espacios de 2
- `stroke-dasharray="10 3"`: trazos largos con espacios pequenios

**Multiples valores**
- `stroke-dasharray="5 2 8 4"`: patron de 4 partes: trazo 5, espacio 2, trazo 8, espacio 4
- Si el numero de valores es impar, el patron se duplica para crear un numero par: `"5 3"` = `"5 3 5 3"` efectivamente

**Valor `0` para espacios**
- `stroke-dasharray="5 0"`: trazos continuos (equivalente a no tener dasharray)
- `stroke-dasharray="0 5"`: "invisible" salvo los puntos si `stroke-linecap="round"`

**Crear puntos con dasharray**
- `stroke-dasharray="0 5"` con `stroke-linecap="round"`: produce una linea de puntos circulares
- Los puntos tienen diametro = `stroke-width`
- El espacio entre puntos = 5 unidades menos el diametro de cada punto

### Propiedad `stroke-dashoffset`

**Que hace**
- Desplaza el inicio del patron de guiones a lo largo de la linea
- Permite "mover" el patron sin cambiar su definicion

**Valores**
- Positivo: el patron se desplaza "hacia atras" (como si se empezara mas tarde en el patron)
- Negativo: el patron se desplaza "hacia adelante"
- El desplazamiento es ciclico: un offset igual a la longitud del patron tiene el mismo efecto visual que `0`

**Uso principal: animacion de line-drawing**
- Tecnica para simular que una linea se "dibuja sola":
  1. Calcular la longitud total del path con `getTotalLength()`
  2. Establecer `stroke-dasharray` = longitud total (un solo "trazo" que cubre todo el path)
  3. Establecer `stroke-dashoffset` = longitud total (el trazo esta completamente "desplazado", invisible)
  4. Animar `stroke-dashoffset` de la longitud total hasta `0` → el trazo aparece progresivamente

---

## 7.6 Trazo y relleno como atributos vs CSS

### Las tres formas de aplicar estilos de pintura

**1. Atributos de presentacion (en el elemento)**
```
<rect fill="red" stroke="blue" stroke-width="2" />
```
- Directamente en el elemento XML como atributos
- La forma mas "SVG pura"
- Menor especificidad en la cascada de estilos

**2. Estilo inline (atributo `style`)**
```
<rect style="fill: red; stroke: blue; stroke-width: 2px;" />
```
- Propiedad CSS `style` aplicada al elemento
- Misma sintaxis que CSS en HTML
- Mayor especificidad que los atributos de presentacion

**3. Hoja de estilos (`<style>` o CSS externo)**
```css
rect { fill: red; stroke: blue; stroke-width: 2; }
.mi-clase { fill: green; }
```
- Se puede definir dentro del SVG en un bloque `<style>` o en el CSS externo del documento
- Usa selectores CSS normales: clase, id, tipo de elemento, atributo, etc.
- Permite reutilizar estilos y mantener separacion entre estructura y presentacion

### La cascada de estilos en SVG (de menor a mayor prioridad)

1. **Estilos heredados del padre** (la herencia)
2. **Atributos de presentacion** (`fill="red"` en el elemento)
3. **Reglas CSS en hojas de estilo** (por especificidad CSS normal)
4. **Estilo inline** (`style="fill: red;"`)
5. **`!important`** en reglas CSS

**Punto critico**: los atributos de presentacion (`fill="red"`) tienen **menor prioridad** que las reglas CSS normales. Esto significa que un selector CSS simple puede sobreescribir un atributo de presentacion sin necesidad de `!important`.

**Implicacion practica**: si se aplica `fill="red"` como atributo y luego hay una regla CSS `rect { fill: blue; }`, el rectangulo sera azul (no rojo), porque el CSS tiene mayor especificidad.

### Cuándo usar cada forma

| Uso | Forma recomendada |
|-----|-----------------|
| SVG generado por editor grafico | Atributos (es lo que generan los editores) |
| SVG escrito a mano, estilos unicos | Atributos (mas conciso) |
| SVG con multiples elementos del mismo tipo | Hoja de estilos (reutilizacion) |
| SVG que necesita cambios via JavaScript | CSS con clases (facilita el toggle) |
| SVG tematizable con variables CSS | Hoja de estilos con `var()` |
| SVG responsivo (adaptar estilos al tamano) | Hoja de estilos con `@media` |

---

## 7.7 paint-order

### El problema que resuelve
- Por defecto, SVG pinta el fill primero, luego el stroke encima del fill
- El stroke se centra en el borde: la mitad queda dentro del fill, la mitad fuera
- La mitad interior del stroke "tapa" el borde del fill
- Si se quiere que el stroke quede completamente fuera del fill (para no tapar nada del fill), hay que usar `paint-order`

### Valores de `paint-order`

**Por defecto: `fill stroke markers`**
- Primero se pinta el fill, luego el stroke encima, luego los markers
- El stroke cubre el borde exterior del fill

**`paint-order="stroke fill markers"` (o simplemente `paint-order="stroke"`)**
- Primero se pinta el stroke, luego el fill encima
- El stroke queda debajo del fill: solo la mitad exterior del stroke es visible
- Efecto: el stroke parece "crecer hacia afuera" sin cubrir el interior del fill

**Uso practico comun**
- Texto con contorno: con `paint-order="stroke"`, el stroke del texto no cubre las partes interiores de los caracteres → el texto se ve mas limpio
- Formas donde se quiere preservar la nitidez del fill y que el stroke sea solo exterior

### Valores parciales
- `paint-order="stroke"` — stroke primero, fill y markers mantienen su orden relativo
- `paint-order="markers fill"` — markers primero, fill segundo, stroke al final
- El orden no especificado se infiere

---

## 7.8 vector-effect

### El problema que resuelve
- Cuando se escala un SVG (con `transform="scale(2)"` o via CSS), el `stroke-width` tambien se escala
- Un trazo de 1px se vuelve 2px al duplicar la escala
- Para muchos casos (bordes de interfaz, grids, anotaciones) se quiere un trazo de **grosor fijo independiente de la escala**

### `vector-effect="non-scaling-stroke"`

**Que hace**
- El grosor del stroke **no se escala** con las transformaciones del elemento
- Un `stroke-width="1"` con `vector-effect="non-scaling-stroke"` siempre se ve como 1px en pantalla, sin importar cuanto se escale el elemento

**Donde aplica**
- En el propio elemento con el stroke
- No se hereda automaticamente a los hijos (hay que especificarlo en cada elemento que lo necesite)

**Casos de uso**
- Grids de diagramas y esquemas donde las lineas deben verse siempre finas
- Bordes de formas en SVGs que el usuario puede hacer zoom
- Anotaciones y guias que deben tener grosor consistente

**Limitacion**
- Solo afecta al stroke, no al fill
- No afecta al desplazamiento del stroke (el centro del stroke sigue estando en el borde geometrico)
- El soporte es amplio en navegadores modernos pero puede tener comportamientos sutiles en casos complejos
