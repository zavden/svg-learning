# Seccion 3: Sistema de Coordenadas y Viewport

> El sistema de coordenadas es el concepto mas confuso para los principiantes de SVG.
> Entenderlo bien es la diferencia entre luchar contra SVG y dominarlo.

---

## 3.1 El viewport

### Que es el viewport
- El **viewport** es la "ventana" a traves de la cual se ve el contenido SVG
- Es el **area de visualizacion fisica** del SVG: cuanto espacio ocupa en la pantalla
- Se define con los atributos `width` y `height` del elemento `<svg>`
- Si el viewport es `width="400" height="300"`, el SVG ocupa 400x300 pixeles en la pantalla

### Viewport vs. contenido
- El viewport define **donde se muestra** el SVG, no **como es el contenido internamente**
- El contenido puede tener su propio sistema de coordenadas (el viewBox) distinto al viewport
- Es como la diferencia entre el tamano de una ventana (viewport) y el paisaje que se ve a traves de ella (viewBox)

### Unidades aceptadas en width y height
| Unidad | Descripcion | Ejemplo |
|--------|-------------|---------|
| Sin unidad | Pixeles por defecto | `width="400"` |
| `px` | Pixeles CSS | `width="400px"` |
| `%` | Porcentaje del contenedor | `width="100%"` |
| `em` | Relativo al font-size del elemento | `width="10em"` |
| `rem` | Relativo al font-size raiz | `width="25rem"` |
| `cm` | Centimetros | `width="10cm"` |
| `mm` | Milimetros | `width="100mm"` |
| `in` | Pulgadas | `width="4in"` |
| `pt` | Puntos (1pt = 1/72in) | `width="288pt"` |
| `pc` | Picas (1pc = 12pt) | `width="24pc"` |

### Viewport sin dimensiones explicitas
- Si se omiten `width` y `height` en SVG inline: el elemento `<svg>` ocupa `300x150px` por defecto (comportamiento CSS de elementos inline)
- Con CSS externo: se puede controlar con `width: 100%; height: auto;`
- Patron responsivo: omitir width/height del `<svg>` y controlarlo con CSS del contenedor

### El viewport como "recorte"
- Si el contenido SVG es mas grande que el viewport, **se recorta** (como `overflow: hidden`)
- Si el contenido es mas pequenio, queda espacio vacio alrededor (segun `preserveAspectRatio`)
- Se puede cambiar este comportamiento con la propiedad CSS `overflow: visible` en el SVG

---

## 3.2 El atributo `viewBox`

### El concepto fundamental
- `viewBox` define el **sistema de coordenadas interno** del SVG
- Es el "mapa" que describe la region del espacio de usuario que se va a mostrar
- Los elementos SVG se posicionan en el sistema de coordenadas del `viewBox`, no en el viewport
- El navegador **mapea** automaticamente el viewBox al viewport

### Sintaxis
```
viewBox="min-x min-y ancho alto"
```
Los cuatro valores separados por espacios o comas:

**`min-x`** — Coordenada X del borde izquierdo del viewBox
- Normalmente `0`, pero puede ser negativo o positivo
- Cambiar este valor desplaza el contenido horizontalmente (como un pan/scroll)
- Valor positivo: el contenido se "mueve a la izquierda" (se corta el lado izquierdo)
- Valor negativo: aparece espacio vacio a la izquierda

**`min-y`** — Coordenada Y del borde superior del viewBox
- Normalmente `0`, misma logica que min-x pero en vertical

**`ancho`** — Ancho del sistema de coordenadas
- Cuantas "unidades de usuario" caben horizontalmente en el viewport
- Si `width="400"` y `viewBox="0 0 100 50"`: cada unidad de usuario = 4px
- Si `width="400"` y `viewBox="0 0 400 200"`: cada unidad de usuario = 1px

**`alto`** — Alto del sistema de coordenadas
- Misma logica que el ancho pero en vertical

### Efectos visuales de modificar el viewBox

**viewBox mas pequenio que el viewport → Zoom in**
- Viewport: `width="400" height="400"`
- viewBox: `viewBox="0 0 100 100"`
- Efecto: el contenido se ve 4x mas grande (cada unidad de usuario = 4px)
- Un circulo de radio 10 unidades ocupa 40px en pantalla

**viewBox mas grande que el viewport → Zoom out**
- Viewport: `width="400" height="400"`
- viewBox: `viewBox="0 0 800 800"`
- Efecto: el contenido se ve 2x mas pequenio (cada unidad de usuario = 0.5px)
- Un circulo de radio 100 unidades ocupa 50px en pantalla

**viewBox desplazado → Panning (desplazamiento)**
- Viewport: `width="400" height="400"`
- viewBox: `viewBox="100 100 400 400"` (min-x=100, min-y=100)
- Efecto: el area visible empieza en la coordenada (100,100) del espacio del usuario
- Los elementos posicionados en (0,0) a (100,100) quedan fuera del area visible

### La relacion entre viewport y viewBox
- Sin viewBox: los elementos usan las mismas coordenadas que los pixeles del viewport
- Con viewBox: hay dos sistemas de coordenadas, el navegador hace la conversion automaticamente
- La formula de conversion: `pixel_en_pantalla = (coordenada_usuario / tamanio_viewBox) * tamanio_viewport`

### Casos de uso practicos de viewBox

**SVG escalable y responsivo**
```
viewBox="0 0 24 24" sin width/height fijos
```
El SVG se adapta a cualquier tamano que le de el CSS, siempre con las mismas proporciones.

**Recortar una parte de un SVG**
```
viewBox="50 50 200 200" para ver solo la region central de un grafico grande
```

**Crear efectos de zoom programatico**
- Cambiar el viewBox con JavaScript simula zoom y pan
- Esto es la base de muchas librerias de visualizacion de datos

---

## 3.3 El atributo `preserveAspectRatio`

### El problema que resuelve
- Cuando las proporciones del `viewBox` y del `viewport` son distintas, el navegador necesita decidir que hacer
- Sin indicaciones: ¿estira el contenido? ¿lo centra? ¿lo recorta?
- `preserveAspectRatio` da esas instrucciones

### Sintaxis
```
preserveAspectRatio="<align> <meetOrSlice>"
```
Dos partes separadas por espacio: como alinear y que hacer con el espacio sobrante/faltante.

### Parte 1: Alineacion (`<align>`)

Combina la alineacion horizontal (xMin, xMid, xMax) con la vertical (YMin, YMid, YMax):

| Valor | Horizontal | Vertical |
|-------|-----------|----------|
| `xMinYMin` | Izquierda | Arriba |
| `xMidYMin` | Centro | Arriba |
| `xMaxYMin` | Derecha | Arriba |
| `xMinYMid` | Izquierda | Centro |
| `xMidYMid` | Centro | Centro ← **valor por defecto** |
| `xMaxYMid` | Derecha | Centro |
| `xMinYMax` | Izquierda | Abajo |
| `xMidYMax` | Centro | Abajo |
| `xMaxYMax` | Derecha | Abajo |
| `none` | — | Estira para llenar (distorsiona) |

### Parte 2: Meet o Slice (`<meetOrSlice>`)

**`meet`** (por defecto)
- El viewBox se escala para caber COMPLETAMENTE dentro del viewport
- Puede quedar espacio vacio (letterboxing/pillarboxing)
- Ningun contenido se recorta
- Analogo a `background-size: contain` en CSS
- Ideal cuando es importante ver todo el contenido

**`slice`**
- El viewBox se escala para CUBRIR completamente el viewport
- El contenido puede recortarse por los bordes
- No hay espacio vacio
- Analogo a `background-size: cover` en CSS
- Ideal cuando hay que llenar el espacio y algo de recorte es aceptable

### Valor especial: `none`
- Desactiva la preservacion de aspecto
- El viewBox se estira para llenar exactamente el viewport
- El contenido se distorsiona (como una imagen estirada)
- Util para algunos efectos intencionados o cuando el viewBox y viewport tienen las mismas proporciones

### Combinaciones mas usadas en practica

**Logo que debe verse completo y centrado**
```
preserveAspectRatio="xMidYMid meet"   (es el valor por defecto)
```

**Imagen de fondo que cubre todo el area**
```
preserveAspectRatio="xMidYMid slice"
```

**Icono alineado a la izquierda**
```
preserveAspectRatio="xMinYMid meet"
```

**Contenido que debe distorsionarse para llenar** (raro, pero existe)
```
preserveAspectRatio="none"
```

---

## 3.4 Sistema de coordenadas del usuario

### Caracteristicas del sistema de coordenadas SVG

**Origen en la esquina superior izquierda**
- El punto (0,0) esta en la esquina **superior izquierda** del viewBox
- Diferente al sistema matematico convencional donde (0,0) es el centro o la esquina inferior izquierda

**El eje Y esta invertido respecto a las matematicas**
- En matematicas: Y crece hacia arriba
- En SVG (y en pantallas en general): Y crece hacia **abajo**
- Un elemento con `y="50"` esta 50 unidades hacia **abajo** del borde superior
- Esto es consistente con HTML y CSS, pero puede confundir a quienes vienen de matematicas o graficos

**El eje X es convencional**
- X crece hacia la **derecha** (igual que en matematicas)

### Unidades del usuario
- Las coordenadas en SVG son "unidades de usuario" (user units), sin unidad de medida explicita
- Por defecto, 1 unidad de usuario = 1 pixel CSS (cuando no hay viewBox, o cuando viewBox coincide con las dimensiones)
- Con viewBox, el tamano real de una unidad de usuario depende de la escala entre viewBox y viewport
- Los elementos SVG definen sus posiciones y tamanos en estas unidades abstractas

### Implicaciones para el posicionamiento

**`x`, `y` en la mayoria de formas**
- En `<rect>`: `x`, `y` son la esquina superior izquierda
- En `<text>`: `x`, `y` es el punto de inicio de la linea base del texto
- En `<use>`: `x`, `y` es el desplazamiento adicional del elemento

**`cx`, `cy` en circulos y elipses**
- Son las coordenadas del **centro** del circulo/elipse
- Un circulo con `cx="50" cy="50" r="30"` esta centrado en (50,50)

**Coordenadas relativas vs absolutas en `<path>`**
- Comandos en mayuscula (M, L, C...): coordenadas absolutas respecto al origen del sistema
- Comandos en minuscula (m, l, c...): coordenadas relativas al punto actual del trazado

---

## 3.5 Sistemas de coordenadas anidados

### SVG dentro de SVG
- Un elemento `<svg>` puede contener otro elemento `<svg>`
- El SVG hijo tiene sus **propios atributos** `x`, `y`, `width`, `height` y `viewBox`
- Crea un **nuevo viewport** y, si tiene `viewBox`, un nuevo sistema de coordenadas

### Como se relacionan padre e hijo
- El SVG hijo se posiciona usando las coordenadas del padre (atributos `x` e `y`)
- El `width` y `height` del hijo definen su tamano en el espacio de coordenadas del padre
- El `viewBox` del hijo (si existe) crea un nuevo sistema de coordenadas independiente para su contenido

### Casos de uso de SVGs anidados
- Composicion de graficos complejos con distintas escalas
- Reutilizacion de un SVG externo como parte de uno mayor
- Crear sub-regiones con su propio sistema de coordenadas para facilitar el calculo de posiciones

### Transformaciones y coordenadas anidadas
- Las transformaciones (`transform="..."`) tambien crean sistemas de coordenadas locales
- Un `translate(50,50)` en un `<g>` hace que todos los hijos usen (50,50) como nuevo origen relativo
- Los sistemas de coordenadas se pueden anidar profundamente
- La **Current Transformation Matrix (CTM)** es la composicion de todas las transformaciones aplicadas

---

## 3.6 Unidades en SVG

### Unidades absolutas
Son independientes del contexto (siempre tienen el mismo tamano fisico en pantalla o impresion):

| Unidad | Equivalencia | Uso tipico |
|--------|-------------|------------|
| `px` | 1px CSS = 1/96 pulgada | Web (el mas comun) |
| `in` | 1in = 96px | Impresion |
| `cm` | 1cm = ~37.8px | Impresion |
| `mm` | 1mm = ~3.78px | Impresion |
| `pt` | 1pt = 1/72in ≈ 1.33px | Tipografia, impresion |
| `pc` | 1pc = 12pt ≈ 16px | Tipografia, impresion |

### Unidades relativas
Dependen de un valor de referencia del contexto:

| Unidad | Referencia | Ejemplo |
|--------|-----------|---------|
| `em` | font-size del elemento actual | `font-size: 2em` = doble del font-size |
| `ex` | altura de la letra "x" del font actual | Menos comun |
| `%` | Depende del atributo | Ver tabla abajo |

### Como se interpretan los porcentajes
Los porcentajes en SVG tienen referencias distintas segun el atributo:

| Atributo | El 100% equivale a... |
|----------|-----------------------|
| `x`, `width` en `<svg>` | Ancho del viewport del padre |
| `y`, `height` en `<svg>` | Alto del viewport del padre |
| `cx`, `cy` en circulos | Se calcula con la raiz cuadrada de (ancho²+alto²)/2 del viewport |
| `r` en circulos | La misma formula que cx/cy |
| `width`, `height` en `<rect>` | Ancho/alto del viewport actual |
| `offset` en `<stop>` | Longitud del gradiente (0% al 100%) |

### Unidades del usuario (sin sufijo)
- Son las mas comunes en SVG para definir coordenadas y tamanos de formas
- `<rect width="100" height="50">` — sin unidad, son "unidades de usuario"
- Equivalen a pixeles CSS **solo cuando** no hay escala introducida por viewBox
- Con viewBox, las unidades de usuario estan en el espacio de coordenadas del viewBox

### Recomendaciones practicas
- **Para SVGs responsivos**: usar unidades del usuario (sin sufijo) con viewBox; nunca px fijos dentro de las formas
- **Para SVGs de impresion**: usar `mm`, `cm` o `pt`
- **Para font-size en SVG**: preferir `em` o unidades del usuario sobre `px` para que escale
- **Para `width`/`height` del SVG raiz en web**: usar `%` o controlarlo con CSS externo
