# Seccion 10: Patrones (Patterns)

> Un patron es un "sello" que se repite para rellenar una forma.
> Cualquier contenido SVG puede ser un patron: lineas, circulos, texto, incluso otros SVGs.

---

## 10.1 El elemento `<pattern>`

### Que es un patron
- Un patron define una **celda grafica** que se repite (como un azulejo) para rellenar una forma
- La celda puede contener cualquier elemento SVG: formas, paths, texto, imagenes, gradientes
- El patron se repite en una cuadricula hasta cubrir completamente el area de la forma
- Es el equivalente SVG de `background-image: repeat` en CSS

### Donde se define
- Siempre dentro de `<defs>` — es un recurso, no un elemento grafico directo
- Requiere un `id` unico para referenciarse
- No produce ningun output visual hasta que un elemento lo usa en su `fill` o `stroke`

### Atributos principales

**`x` y `y` — Posicion de inicio del patron**
- Definen donde se coloca la primera celda del patron
- Por defecto: `x="0"` `y="0"`
- Un desplazamiento en `x` o `y` hace que el patron empiece desplazado desde el borde de la forma
- Util para alinear el patron con un punto especifico del elemento

**`width` y `height` — Tamanio de la celda**
- Definen las dimensiones de la celda que se repite
- **Obligatorios**: sin ellos el patron no funciona
- El navegador repite la celda horizontalmente cada `width` unidades y verticalmente cada `height` unidades
- Una celda mas pequenio que el contenido: el contenido se recorta en cada repeticion

**`viewBox` — Sistema de coordenadas interno del patron**
- Si se especifica, los elementos dentro del patron usan el sistema de coordenadas del viewBox
- Permite escalar el contenido del patron independientemente de las dimensiones de la celda
- Misma logica que el `viewBox` del elemento `<svg>`
- Muy util para reutilizar contenido SVG de dimensiones distintas a la celda del patron

---

## 10.2 patternUnits

### Que controla
- El sistema de coordenadas que usa el patron para sus atributos de posicion y tamano: `x`, `y`, `width`, `height`
- Determina si la celda del patron tiene dimensiones relativas o absolutas

### `objectBoundingBox` (valor por defecto)

**Como funciona**
- Las dimensiones `width` y `height` son **relativas al bounding box del elemento** que usa el patron
- Rango tipico: 0 a 1 (o 0% a 100%)
- `width="0.2"`: la celda ocupa el 20% del ancho del elemento en cada repeticion → 5 repeticiones horizontales

**Ventajas**
- El patron escala automaticamente con el elemento
- Un patron de "5 columnas y 3 filas" siempre tendra 5 columnas y 3 filas sin importar el tamano del elemento

**Limitacion**
- El contenido interno del patron (si no tiene viewBox) usa coordenadas en el espacio del bounding box (valores entre 0 y 1), lo que puede ser contraintuitivo para valores absolutos

### `userSpaceOnUse`

**Como funciona**
- Las dimensiones `width` y `height` son en **unidades absolutas del sistema de coordenadas del usuario**
- `width="20"` significa que la celda tiene 20 unidades de usuario de ancho, independientemente del elemento

**Ventajas**
- El patron tiene un tamano fisico fijo (ej: cada cuadrado del grid mide siempre 10x10 unidades)
- Mas intuitivo cuando se trabaja con medidas especificas

**Desventaja**
- El numero de repeticiones cambia segun el tamano del elemento
- Si el elemento se redimensiona, el patron no se ajusta proporcionalmente

---

## 10.3 patternContentUnits

### Que controla
- El sistema de coordenadas que usan los **elementos dentro del patron** para sus coordenadas y tamanos
- Independiente de `patternUnits`: son dos configuraciones separadas

### `userSpaceOnUse` (valor por defecto)

**Como funciona**
- Los elementos dentro del patron usan las **unidades absolutas del espacio del usuario**
- Una linea `<line x1="0" y1="0" x2="10" y2="10">` dentro del patron ocupa 10 unidades de usuario

**Ventaja**
- Mas intuitivo para dibujar el contenido del patron con coordenadas normales

**Limitacion cuando se combina con `patternUnits="objectBoundingBox"`**
- Si la celda mide `0.2 x 0.2` (relativa al bounding box) pero el contenido usa unidades absolutas, hay un desajuste de escala
- El contenido puede quedar diminuto o enorme respecto a la celda

### `objectBoundingBox`

**Como funciona**
- Los elementos dentro del patron usan coordenadas relativas al bounding box del elemento que usa el patron
- Menos comun; produce coordenadas entre 0 y 1 para el contenido

**Combinaciones tipicas**

| patternUnits | patternContentUnits | Resultado |
|-------------|---------------------|-----------|
| `objectBoundingBox` | `userSpaceOnUse` | Celda relativa, contenido absoluto — lo mas comun |
| `userSpaceOnUse` | `userSpaceOnUse` | Todo absoluto — mas intuitivo, patron de tamano fijo |
| `objectBoundingBox` | `objectBoundingBox` | Todo relativo — raro, coordenadas entre 0 y 1 para todo |

---

## 10.4 patternTransform

### Que hace
- Aplica una transformacion SVG al patron completo: su posicion, orientacion y escala
- Acepta los mismos valores que el atributo `transform` de los elementos: `rotate`, `scale`, `translate`, `skewX`, `skewY`, `matrix`
- Se aplica al patron antes de usarlo para rellenar la forma

### Casos de uso

**Rotar el patron**
- `patternTransform="rotate(45)"`: el patron se inclina 45°
- Un patron de rayas horizontales se convierte en rayas diagonales
- Un patron de cuadricula se convierte en una cuadricula en diagonal (patron de rombos)

**Escalar el patron**
- `patternTransform="scale(2)"`: el patron se hace el doble de grande
- Util para ajustar la densidad del patron sin cambiar los valores de `width`/`height`

**Desplazar el patron**
- `patternTransform="translate(5, 5)"`: desplaza todo el patron 5 unidades
- Diferente a los atributos `x`/`y` del patron: la transformacion afecta a toda la tesela

**Combinar transformaciones**
- `patternTransform="rotate(30) scale(1.5)"`: rotacion y escala combinadas
- El orden de aplicacion es de derecha a izquierda (igual que en `transform`)

---

## 10.5 Creacion de patrones comunes

### Rayas horizontales

**Estructura de la celda**
- Celda rectangular: `width` amplio, `height` = distancia entre rayas
- Dentro de la celda: un rectangulo que cubre solo la mitad superior (o inferior)
- La otra mitad queda transparente (con fill none o del color del fondo)

**Variables a controlar**
- Ancho de cada raya (la parte coloreada)
- Espacio entre rayas (la parte transparente/de fondo)
- Color de las rayas
- La suma de ancho + espacio = `height` de la celda

### Rayas verticales

- Misma logica que horizontales pero con los ejes invertidos
- El rectangulo de la celda es alto y estrecho
- `width` de la celda = ancho de una raya + espacio

### Rayas diagonales

**Metodo 1: `patternTransform="rotate(45)"`**
- Definir rayas horizontales o verticales normales
- Aplicar `patternTransform="rotate(45)"` para rotarlas
- Las esquinas de la celda pueden quedar sin cubrir: ajustar el tamano de la celda para compensar

**Metodo 2: Path diagonal directo**
- Dibujar la diagonal directamente dentro de la celda
- Requiere entender como los bordes de la celda se unen al repetirse

### Puntos (polka dots)

**Estructura de la celda**
- Celda cuadrada: `width` = `height` = separacion entre centros de los puntos
- Dentro de la celda: un circulo centrado en el centro de la celda (`cx = width/2`, `cy = height/2`)
- El radio del circulo determina el tamano del punto (y el espacio entre puntos)

**Variante: puntos en cuadricula desfasada (hexagonal)**
- Dos celdas de puntos, la segunda desplazada en `width/2` horizontalmente y `height/2` verticalmente
- Produce un patron de puntos en disposicion hexagonal, mas compacto

### Cuadriculas (grids)

**Cuadricula con lineas**
- Celda cuadrada con una linea en el borde derecho y una en el borde inferior
- Solo se necesitan dos lineas por celda; el resto se crea por repeticion
- `stroke` de las lineas = color de la cuadricula; `fill="none"` en la celda de fondo

**Cuadricula con rectangulos de fondo alternados (tablero de ajedrez)**
- Celda de 2x2 unidades logicas con dos rectangulos coloreados y dos transparentes
- Los cuatro cuadros se alternan en los cuadrantes de la celda

### Patrones de ladrillos

**Ladrillos horizontales**
- Cada fila de ladrillos desplazada a la mitad respecto a la anterior
- Celda que contiene dos "medios ladrillos": uno centrado en la fila superior y uno en la inferior
- Requiere calcular con cuidado el desfase horizontal

**Ladrillos verticales**
- Misma logica pero con orientacion vertical

### Patrones complejos anidados

- Un patron puede usar otro patron como `fill` de sus elementos internos
- Un patron de circulos donde cada circulo tiene un patron de rayas interno
- La limitacion es el rendimiento: patrones anidados pueden ser costosos de renderizar
- Util para texturas complejas en SVGs illustrativos

---

## 10.6 Referenciar patrones

### Sintaxis de referencia

**En el atributo `fill`**
- `fill="url(#idDelPatron)"`: el interior de la forma usa el patron como relleno
- Identica sintaxis a los gradientes
- Aplica a cualquier forma: `<rect>`, `<circle>`, `<path>`, `<text>`, etc.

**En el atributo `stroke`**
- `stroke="url(#idDelPatron)"`: el trazo de la forma usa el patron
- Util para contornos con patron, aunque es un caso de uso menos comun

**En CSS**
- `fill: url(#idDelPatron);` en una regla CSS
- Mismo resultado que como atributo directo

### Comportamiento del patron con distintas formas

**Con formas rectangulares**
- El patron empieza en la esquina superior izquierda del bounding box (con `patternUnits="objectBoundingBox"`)
- Se repite hasta cubrir toda el area

**Con formas circulares**
- El patron rellena el circulo completo
- Las partes del patron fuera del circulo se recortan automaticamente

**Con paths complejos**
- El patron rellena toda el area interior del path (segun el fill-rule)
- Las partes dentro de agujeros (con fill-rule evenodd) no muestran el patron

### Actualizar el patron dinamicamente
- Los patrones son parte del DOM y pueden modificarse con JavaScript
- Cambiar `patternTransform` via JS: animar la rotacion o el desplazamiento del patron
- Cambiar los colores de los elementos dentro del patron
- Util para efectos de animacion de texturas
