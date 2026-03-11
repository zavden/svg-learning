# FASE 4: Reutilizacion y Efectos
> Secciones 12-16 del Roadmap SVG Basico-Intermedio
> Agrupacion, recorte, mascaras, filtros, marcadores e imagenes embebidas.

---

## 12. Agrupacion y Reutilizacion

### 12.1 El elemento `<g>` (grupo)
- Agrupa elementos graficos como una unidad
- Permite aplicar atributos comunes a todos los hijos (fill, stroke, transform, opacity, etc.)
- Los atributos de presentacion se heredan a los hijos
- Un hijo puede sobrescribir los atributos heredados del grupo
- Se puede aplicar transformaciones a todo el grupo
- Se puede aplicar filtros, clipPath y mask a todo el grupo
- No tiene atributos de posicion propios (no tiene x, y, width, height)

### 12.2 El elemento `<use>`
- Reutiliza un elemento definido en otro lugar del documento
- Atributos:
  - `href` (o `xlink:href`): referencia al id del elemento original
  - `x`, `y`: posicion donde se coloca la copia
  - `width`, `height`: dimensiones (solo aplica a `<svg>` y `<symbol>` referenciados)
- Crea una copia visual (shadow DOM) del elemento original
- Los cambios en el original se reflejan en todas las copias
- Limitaciones: no se pueden estilizar los hijos internos de la copia directamente con CSS
- Truco con `currentColor` y variables CSS para estilizar `<use>`

### 12.3 El elemento `<symbol>`
- Similar a `<g>` pero no se renderiza directamente
- Se define en `<defs>` (o fuera, pero igualmente no se muestra)
- Puede tener sus propios `viewBox` y `preserveAspectRatio`
- Se instancia con `<use>`
- Ideal para definir iconos y graficos reutilizables
- Diferencia clave con `<g>`: tiene viewBox propio, lo que permite escalado independiente

### 12.4 El elemento `<defs>`
- Contenedor para elementos que se definen pero no se renderizan
- Todo elemento dentro de `<defs>` es invisible hasta que se referencia
- Contenido tipico: gradientes, patrones, filtros, clipPaths, masks, symbols, markers

### 12.5 Herencia de atributos de presentacion
- Reglas de herencia en SVG (similares a CSS)
- Atributos que se heredan: fill, stroke, font-family, font-size, visibility, etc.
- Atributos que NO se heredan: x, y, width, height, opacity, clip-path, filter, mask
- Orden de prioridad: style inline > CSS externo > atributos de presentacion > herencia

---

## 13. Recorte y Mascaras

### 13.1 Recorte con `<clipPath>`
- Define una region visible: todo fuera del clipPath se oculta
- Se define en `<defs>` con un `id`
- Se aplica con el atributo `clip-path="url(#idDelClipPath)"`
- El contenido del clipPath son formas que definen la region visible
- El recorte es binario: un pixel esta dentro o fuera (sin transicion suave)
- El relleno y trazo de las formas dentro del clipPath no importan; solo importa su geometria

### 13.2 clipPathUnits
- `userSpaceOnUse` (por defecto): las formas del clipPath usan coordenadas del usuario
- `objectBoundingBox`: coordenadas relativas al bounding box del elemento recortado

### 13.3 Mascaras con `<mask>`
- Define la visibilidad usando la luminancia o el canal alfa
- Se define en `<defs>` con un `id`
- Se aplica con el atributo `mask="url(#idDeLaMask)"`
- A diferencia del clipPath, la mascara permite transiciones suaves (semi-transparencia)
- Blanco = visible, negro = invisible, grises = parcialmente visible
- El color (luminancia) de los elementos dentro de la mascara determina la opacidad

### 13.4 Atributos de `<mask>`
- `x`, `y`, `width`, `height`: area de la mascara
- `maskUnits`: unidades del area de la mascara (userSpaceOnUse, objectBoundingBox)
- `maskContentUnits`: unidades del contenido de la mascara

### 13.5 Diferencias entre clipPath y mask
- clipPath: recorte duro (binario), mejor rendimiento, solo usa geometria
- mask: recorte suave (gradual), usa luminancia/alfa, permite degradados de visibilidad
- Cuando usar cada uno

### 13.6 Recorte con CSS
- `clip-path` como propiedad CSS
- Funciones CSS de recorte: `circle()`, `ellipse()`, `polygon()`, `inset()`, `path()`
- Combinacion de funciones CSS con `url()` que referencia un `<clipPath>` SVG

---

## 14. Filtros SVG

### 14.1 El elemento `<filter>`
- Se define en `<defs>` con un `id`
- Se aplica con `filter="url(#idDelFiltro)"`
- Atributos de region:
  - `x`, `y`, `width`, `height`: area donde se aplica el filtro (por defecto: -10%, -10%, 120%, 120%)
  - `filterUnits`: unidades del area del filtro
- Los filtros procesan la imagen rasterizada del elemento (bitmaps), no las geometrias vectoriales

### 14.2 Primitivas de filtro basicas

#### 14.2.1 `<feGaussianBlur>`
- Aplica desenfoque gaussiano
- `stdDeviation`: cantidad de desenfoque (un valor = X e Y iguales, dos valores = separados)
- `in`: fuente de entrada (SourceGraphic, SourceAlpha, etc.)

#### 14.2.2 `<feOffset>`
- Desplaza la imagen
- `dx`, `dy`: desplazamiento en X e Y
- Util para sombras

#### 14.2.3 `<feFlood>`
- Rellena el area del filtro con un color solido
- `flood-color`: color
- `flood-opacity`: opacidad

#### 14.2.4 `<feComposite>`
- Combina dos imagenes usando operaciones de composicion
- `operator`: over, in, out, atop, xor, arithmetic
- `in`, `in2`: las dos fuentes a combinar

#### 14.2.5 `<feMerge>` y `<feMergeNode>`
- Apila multiples capas de filtro
- Cada `<feMergeNode>` define una capa con su atributo `in`

#### 14.2.6 `<feColorMatrix>`
- Transforma los colores usando una matriz
- `type`: matrix, saturate, hueRotate, luminanceToAlpha
- `values`: la matriz o el valor del tipo

#### 14.2.7 `<feDropShadow>`
- Atajo para crear sombras (equivale a feGaussianBlur + feOffset + feFlood + feComposite + feMerge)
- `dx`, `dy`: desplazamiento
- `stdDeviation`: desenfoque
- `flood-color`, `flood-opacity`

### 14.3 Entradas de filtro predefinidas
- `SourceGraphic`: la imagen original del elemento
- `SourceAlpha`: solo el canal alfa (silueta) del elemento
- `BackgroundImage`: la imagen del fondo detras del elemento
- `BackgroundAlpha`: el canal alfa del fondo
- `FillPaint`: el color de relleno actual
- `StrokePaint`: el color de trazo actual

### 14.4 Encadenamiento de filtros
- El atributo `result` nombra la salida de una primitiva
- El atributo `in` o `in2` referencia la salida de otra primitiva
- Las primitivas se ejecutan en orden del codigo fuente

### 14.5 Creacion de sombras proyectadas (drop shadow)
- Combinacion manual: feGaussianBlur + feOffset + feMerge
- Uso de feDropShadow (SVG 2)
- Alternativa CSS: `filter: drop-shadow()`

### 14.6 Otras primitivas de filtro (nivel intermedio)
- `<feBlend>`: mezcla dos imagenes (normal, multiply, screen, darken, lighten)
- `<feMorphology>`: erosiona o dilata (operator: erode, dilate; radius)
- `<feTurbulence>`: genera ruido Perlin/turbulencia (para texturas procedurales)
- `<feDisplacementMap>`: desplaza pixeles segun un mapa
- `<feConvolveMatrix>`: aplica un kernel de convolucion (deteccion de bordes, nitidez)
- `<feComponentTransfer>`: ajusta canales de color individuales (brillo, contraste, gamma)
- `<feDiffuseLighting>` y `<feSpecularLighting>`: simulan iluminacion 3D
- `<feImage>`: inserta una imagen externa como fuente de filtro
- `<feTile>`: repite la entrada como mosaico

---

## 15. Marcadores (Markers)

### 15.1 El elemento `<marker>`
- Se define en `<defs>` con un `id`
- Es un pequenio grafico que se coloca en los vertices de lineas, polilinas, poligonos y paths
- Atributos:
  - `markerWidth`, `markerHeight`: tamanio del viewport del marcador
  - `refX`, `refY`: punto del marcador que se alinea con el vertice
  - `viewBox`: sistema de coordenadas interno
  - `orient`: orientacion del marcador
    - `auto`: se rota automaticamente para seguir la direccion de la linea
    - `auto-start-reverse`: auto pero invertido en el marcador de inicio
    - Un angulo fijo en grados
  - `markerUnits`:
    - `strokeWidth` (por defecto): el marcador se escala segun el stroke-width
    - `userSpaceOnUse`: tamanio fijo en unidades del usuario

### 15.2 Aplicacion de marcadores
- `marker-start`: marcador al inicio del trazado
- `marker-mid`: marcador en vertices intermedios
- `marker-end`: marcador al final del trazado
- `marker`: atajo para aplicar el mismo marcador en las tres posiciones

### 15.3 Usos comunes
- Flechas en lineas y paths
- Puntos en vertices de poligonos
- Decoraciones personalizadas en graficos de datos

---

## 16. Imagenes y Elementos Embebidos

### 16.1 El elemento `<image>`
- Incrusta imagenes rasterizadas (PNG, JPEG, GIF, WebP) dentro del SVG
- Atributos:
  - `href` (o `xlink:href`): URL de la imagen
  - `x`, `y`: posicion
  - `width`, `height`: dimensiones de visualizacion
  - `preserveAspectRatio`: como se ajusta la imagen
- La imagen puede ser una URL externa o datos inline (data URI / base64)

### 16.2 El elemento `<foreignObject>`
- Permite incrustar contenido no-SVG (tipicamente HTML) dentro del SVG
- Atributos: `x`, `y`, `width`, `height`
- Requiere el namespace correcto para el contenido embebido
- Usos: texto con formato HTML, formularios, contenido web complejo dentro de SVG
- Limitaciones: soporte variable entre navegadores para ciertos contenidos

### 16.3 SVGs anidados
- Un elemento `<svg>` puede contener otros elementos `<svg>`
- Cada SVG anidado crea su propio viewport y sistema de coordenadas
- Util para composiciones complejas con diferentes viewBoxes

### 16.4 Imagenes base64 embebidas
- Sintaxis: `href="data:image/png;base64,iVBOR..."`
- Ventaja: archivo SVG autocontenido (no necesita archivos externos)
- Desventaja: aumenta significativamente el tamanio del archivo SVG
