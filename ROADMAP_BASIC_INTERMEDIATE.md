# ROADMAP SVG: Nivel Basico-Intermedio

> Guia exhaustiva para dominar SVG (Scalable Vector Graphics) desde cero hasta un nivel intermedio solido.

---

## Indice General

1. [Fundamentos y Conceptos Previos](#1-fundamentos-y-conceptos-previos)
2. [Estructura del Documento SVG](#2-estructura-del-documento-svg)
3. [Sistema de Coordenadas y Viewport](#3-sistema-de-coordenadas-y-viewport)
4. [Formas Basicas](#4-formas-basicas)
5. [Trazados (Paths)](#5-trazados-paths)
6. [Texto en SVG](#6-texto-en-svg)
7. [Relleno y Trazo (Fill & Stroke)](#7-relleno-y-trazo-fill--stroke)
8. [Colores y Opacidad](#8-colores-y-opacidad)
9. [Gradientes](#9-gradientes)
10. [Patrones (Patterns)](#10-patrones-patterns)
11. [Transformaciones](#11-transformaciones)
12. [Agrupacion y Reutilizacion](#12-agrupacion-y-reutilizacion)
13. [Recorte y Mascaras](#13-recorte-y-mascaras)
14. [Filtros SVG](#14-filtros-svg)
15. [Marcadores (Markers)](#15-marcadores-markers)
16. [Imagenes y Elementos Embebidos](#16-imagenes-y-elementos-embebidos)
17. [SVG Responsivo y Accesibilidad](#17-svg-responsivo-y-accesibilidad)
18. [Integracion de SVG en la Web](#18-integracion-de-svg-en-la-web)
19. [Estilado con CSS](#19-estilado-con-css)
20. [Animaciones en SVG](#20-animaciones-en-svg)
21. [Interactividad con JavaScript](#21-interactividad-con-javascript)
22. [Sprites SVG e Iconos](#22-sprites-svg-e-iconos)
23. [Optimizacion de SVG](#23-optimizacion-de-svg)
24. [Herramientas y Flujo de Trabajo](#24-herramientas-y-flujo-de-trabajo)
25. [Buenas Practicas y Patrones Comunes](#25-buenas-practicas-y-patrones-comunes)

---

## 1. Fundamentos y Conceptos Previos

### 1.1 Que es SVG
- Definicion: Scalable Vector Graphics, formato de imagen vectorial basado en XML
- Especificacion del W3C (version actual: SVG 1.1 Second Edition, SVG 2 en desarrollo)
- Diferencia entre graficos vectoriales y graficos rasterizados (bitmap)
- Historia breve: SVG 1.0 (2001), SVG 1.1 (2003), SVG Tiny 1.2 (2008), SVG 2 (en progreso)

### 1.2 Ventajas de SVG
- Escalabilidad infinita sin perdida de calidad
- Tamanio de archivo pequenio comparado con formatos raster para graficos simples
- Es texto plano (XML): se puede editar con cualquier editor de texto
- Se puede estilizar con CSS
- Se puede manipular con JavaScript y el DOM
- Es indexable y accesible (lectores de pantalla)
- Soporte nativo en todos los navegadores modernos
- Se puede animar de multiples formas (SMIL, CSS, JS)

### 1.3 Cuando usar SVG y cuando no
- **Ideal para**: iconos, logotipos, ilustraciones, graficos de datos, mapas, diagramas, animaciones UI, fondos decorativos
- **No ideal para**: fotografias, imagenes con millones de colores y detalles complejos, texturas fotograficas
- Comparacion con PNG, JPEG, GIF, WebP: casos de uso de cada uno

### 1.4 Graficos Vectoriales vs Rasterizados
- Como funcionan los vectores: puntos, lineas, curvas definidas matematicamente
- Como funcionan los raster: cuadricula de pixeles
- Impacto en el escalado: vectores mantienen nitidez, raster se pixelan
- Impacto en el tamanio de archivo segun complejidad del grafico

### 1.5 XML como base de SVG
- Que es XML: lenguaje de marcado extensible
- Reglas de sintaxis XML que aplican a SVG:
  - Todos los elementos deben cerrarse (`<rect />` o `<rect></rect>`)
  - Los atributos deben ir entre comillas
  - Es case-sensitive (distingue mayusculas de minusculas)
  - Debe estar bien formado (well-formed)
- Namespaces XML en SVG

---

## 2. Estructura del Documento SVG

### 2.1 El elemento raiz `<svg>`
- Atributos fundamentales:
  - `xmlns="http://www.w3.org/2000/svg"` (namespace obligatorio en archivos `.svg`)
  - `xmlns:xlink="http://www.w3.org/1999/xlink"` (namespace para enlaces, legacy)
  - `version` (valor tipico: `"1.1"`)
  - `width` y `height` (dimensiones del viewport)
  - `viewBox` (sistema de coordenadas interno)
- Diferencia entre usar SVG como archivo independiente vs inline en HTML

### 2.2 Prologo XML
- Declaracion XML: `<?xml version="1.0" encoding="UTF-8"?>`
- Declaracion DOCTYPE (opcional, rara vez usada hoy)
- Cuando se necesita y cuando se omite

### 2.3 Elementos de metadatos
- `<title>`: titulo del SVG (importante para accesibilidad)
- `<desc>`: descripcion del contenido SVG
- `<metadata>`: metadatos adicionales en otros formatos (RDF, Dublin Core)

### 2.4 El elemento `<defs>`
- Proposito: definir elementos reutilizables que no se renderizan directamente
- Que va dentro de `<defs>`: gradientes, patrones, filtros, clipPaths, simbolos, marcadores
- Diferencia entre definir y referenciar

### 2.5 Orden de renderizado (Painter's Model)
- SVG usa el modelo del pintor: los elementos se dibujan en orden del codigo fuente
- Los elementos posteriores se pintan encima de los anteriores
- No existe z-index nativo en SVG (se controla con el orden del DOM)

### 2.6 Estructura minima de un SVG valido
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <title>Descripcion breve</title>
  <desc>Descripcion detallada del contenido</desc>
  <!-- Contenido grafico aqui -->
</svg>
```

---

## 3. Sistema de Coordenadas y Viewport

### 3.1 El viewport
- Definicion: la ventana visible a traves de la cual se ve el contenido SVG
- Se define con `width` y `height` en el elemento `<svg>`
- Unidades del viewport: px, em, rem, %, cm, mm, in, pt, pc
- Viewport por defecto cuando no se especifican dimensiones

### 3.2 El atributo `viewBox`
- Sintaxis: `viewBox="min-x min-y width height"`
- Los cuatro valores y que significan:
  - `min-x`: coordenada X del punto superior izquierdo
  - `min-y`: coordenada Y del punto superior izquierdo
  - `width`: ancho del sistema de coordenadas interno
  - `height`: alto del sistema de coordenadas interno
- Relacion entre viewBox y viewport: como se mapea el contenido
- Efecto de un viewBox mas pequenio que el viewport (zoom in)
- Efecto de un viewBox mas grande que el viewport (zoom out)
- Efecto de desplazar min-x y min-y (panning)

### 3.3 El atributo `preserveAspectRatio`
- Sintaxis: `preserveAspectRatio="<align> [<meetOrSlice>]"`
- Valores de alineacion:
  - `none`: estira el contenido para llenar el viewport (distorsiona)
  - `xMinYMin`, `xMidYMin`, `xMaxYMin`
  - `xMinYMid`, `xMidYMid` (valor por defecto), `xMaxYMid`
  - `xMinYMax`, `xMidYMax`, `xMaxYMax`
- Valores de meet/slice:
  - `meet` (por defecto): todo el viewBox es visible, puede haber espacio vacio (como `background-size: contain`)
  - `slice`: el viewBox cubre todo el viewport, puede recortarse contenido (como `background-size: cover`)
- Combinaciones comunes y sus efectos visuales

### 3.4 Sistema de coordenadas del usuario
- Origen (0,0) en la esquina superior izquierda
- Eje X crece hacia la derecha
- Eje Y crece hacia abajo (diferente a las matematicas convencionales)
- Unidades del usuario: sin unidad especifica, mapeadas al viewBox

### 3.5 Sistemas de coordenadas anidados
- Cada elemento `<svg>` anidado crea un nuevo viewport y sistema de coordenadas
- Atributos `x`, `y`, `width`, `height` del SVG anidado
- Como interactuan los viewBox anidados

### 3.6 Unidades en SVG
- Unidades absolutas: px, cm, mm, in, pt, pc
- Unidades relativas: em, ex, %
- Unidades del usuario (sin sufijo): equivalentes a px por defecto
- Como se interpretan los porcentajes en distintos contextos

---

## 4. Formas Basicas

### 4.1 Rectangulo `<rect>`
- Atributos:
  - `x`, `y`: posicion de la esquina superior izquierda
  - `width`, `height`: dimensiones
  - `rx`, `ry`: radios de las esquinas redondeadas
- Comportamiento cuando solo se define `rx` o solo `ry`
- Creacion de cuadrados (width = height)
- Creacion de rectangulos completamente redondeados (rx = width/2)

### 4.2 Circulo `<circle>`
- Atributos:
  - `cx`, `cy`: coordenadas del centro
  - `r`: radio
- Valores por defecto de cx y cy (0 si no se especifican)

### 4.3 Elipse `<ellipse>`
- Atributos:
  - `cx`, `cy`: coordenadas del centro
  - `rx`: radio horizontal
  - `ry`: radio vertical
- Relacion con `<circle>` (un circulo es una elipse con rx = ry)

### 4.4 Linea `<line>`
- Atributos:
  - `x1`, `y1`: punto de inicio
  - `x2`, `y2`: punto final
- La linea solo es visible si tiene un `stroke` definido (no tiene fill)

### 4.5 Polilinia `<polyline>`
- Atributo `points`: lista de coordenadas x,y separadas por espacios o comas
- Formatos validos: `"0,0 50,25 100,0"` o `"0 0 50 25 100 0"`
- La polilinia no se cierra automaticamente
- El fill por defecto es negro: hay que poner `fill="none"` si solo se quiere el trazo

### 4.6 Poligono `<polygon>`
- Atributo `points`: misma sintaxis que polyline
- Diferencia con polyline: el poligono cierra automaticamente la forma (conecta el ultimo punto con el primero)
- Creacion de triangulos, pentagonos, estrellas y formas regulares

### 4.7 Atributos de presentacion comunes a todas las formas
- `fill`: color de relleno
- `stroke`: color del borde
- `stroke-width`: grosor del borde
- `opacity`: opacidad general del elemento
- Estos atributos se detallan en las secciones 7 y 8

---

## 5. Trazados (Paths)

### 5.1 Introduccion a `<path>`
- El elemento mas poderoso y versatil de SVG
- Puede crear cualquier forma que las formas basicas pueden hacer, y mucho mas
- Atributo principal: `d` (data del trazado)

### 5.2 Sintaxis del atributo `d`
- Comandos en mayuscula: coordenadas absolutas
- Comandos en minuscula: coordenadas relativas (respecto al punto actual)
- Los espacios y comas entre numeros son intercambiables
- Los espacios entre un comando y sus parametros son opcionales

### 5.3 Comandos de movimiento
- `M x y` / `m dx dy`: Move To - mueve el cursor sin dibujar
- Establece el punto de inicio del trazado
- Multiples pares de coordenadas despues de M se tratan como L implicitos

### 5.4 Comandos de linea
- `L x y` / `l dx dy`: Line To - dibuja una linea recta hasta el punto
- `H x` / `h dx`: Horizontal Line To - linea horizontal
- `V y` / `v dy`: Vertical Line To - linea vertical

### 5.5 Comando de cierre
- `Z` / `z`: Close Path - dibuja una linea recta desde el punto actual hasta el inicio del sub-trazado
- No distingue mayusculas/minusculas (Z y z hacen lo mismo)

### 5.6 Curvas de Bezier cubicas
- `C x1 y1, x2 y2, x y` / `c dx1 dy1, dx2 dy2, dx dy`
  - (x1,y1): primer punto de control
  - (x2,y2): segundo punto de control
  - (x,y): punto final de la curva
- `S x2 y2, x y` / `s dx2 dy2, dx dy`: Smooth Cubic Bezier
  - Genera automaticamente el primer punto de control como reflejo del anterior
  - Ideal para encadenar curvas suaves

### 5.7 Curvas de Bezier cuadraticas
- `Q x1 y1, x y` / `q dx1 dy1, dx dy`
  - (x1,y1): unico punto de control
  - (x,y): punto final
- `T x y` / `t dx dy`: Smooth Quadratic Bezier
  - Genera automaticamente el punto de control como reflejo del anterior

### 5.8 Arcos
- `A rx ry x-rotation large-arc-flag sweep-flag x y`
- `a rx ry x-rotation large-arc-flag sweep-flag dx dy`
- Parametros:
  - `rx`, `ry`: radios de la elipse
  - `x-rotation`: rotacion de la elipse en grados
  - `large-arc-flag`: 0 = arco pequenio, 1 = arco grande
  - `sweep-flag`: 0 = antihorario, 1 = horario
  - `x`, `y`: punto final del arco
- Los cuatro arcos posibles entre dos puntos (combinaciones de large-arc y sweep)
- Usos comunes: sectores de torta, bordes curvos, formas organicas

### 5.9 Sub-trazados (Sub-paths)
- Un path puede contener multiples sub-trazados usando multiples comandos M
- Cada M inicia un nuevo sub-trazado
- Z cierra el sub-trazado actual, no todo el path
- Utilidad: crear formas con agujeros (usando fill-rule)

### 5.10 fill-rule en paths
- `nonzero` (por defecto): regla de relleno no-cero
- `evenodd`: regla de relleno par-impar
- Como afecta a formas con sub-trazados superpuestos
- Creacion de formas con agujeros usando evenodd

---

## 6. Texto en SVG

### 6.1 El elemento `<text>`
- Atributos de posicion: `x`, `y` (punto de inicio de la linea base)
- El texto se renderiza como graficos vectoriales (no como texto HTML)
- El texto en SVG es seleccionable y accesible

### 6.2 Atributos tipograficos
- `font-family`: familia tipografica
- `font-size`: tamanio de fuente
- `font-weight`: peso (normal, bold, 100-900)
- `font-style`: estilo (normal, italic, oblique)
- `font-variant`: variantes (normal, small-caps)
- `text-decoration`: decoracion (underline, overline, line-through)
- `letter-spacing`: espaciado entre caracteres
- `word-spacing`: espaciado entre palabras

### 6.3 Alineacion y posicionamiento
- `text-anchor`: alineacion horizontal (start, middle, end)
- `dominant-baseline`: alineacion vertical (auto, middle, hanging, text-before-edge, etc.)
- `alignment-baseline`: alineacion de elementos inline
- `dx`, `dy`: desplazamiento relativo (acepta lista de valores para mover cada caracter)
- `rotate`: rotacion de caracteres individuales (acepta lista de valores)

### 6.4 El elemento `<tspan>`
- Sub-elemento de `<text>` para estilizar partes del texto de forma diferente
- Puede reposicionar texto dentro del flujo
- Atributos propios: `x`, `y`, `dx`, `dy`, `rotate`
- Anidamiento de `<tspan>` dentro de otros `<tspan>`

### 6.5 Texto en trazados `<textPath>`
- Atributo `href` (o `xlink:href` en SVG 1.1): referencia al path
- Atributo `startOffset`: donde comienza el texto a lo largo del path (valor o porcentaje)
- Atributo `method`: como se ajusta el texto (align, stretch)
- Atributo `spacing`: espaciado (auto, exact)
- El path referenciado puede estar en `<defs>` para que no sea visible

### 6.6 Limitaciones del texto en SVG
- No hay salto de linea automatico (word-wrap)
- No hay parrafos nativos
- Para texto multilinea: usar multiples `<tspan>` con posiciones y/dy
- Dependencia de fuentes: el texto depende de que la fuente este disponible
- Alternativa: convertir texto a paths para independencia tipografica

### 6.7 Seleccion y busqueda de texto
- El texto SVG es seleccionable en el navegador
- Es indexable por motores de busqueda
- Lectores de pantalla pueden leer texto SVG

---

## 7. Relleno y Trazo (Fill & Stroke)

### 7.1 Propiedades de relleno (fill)
- `fill`: color de relleno (nombre, hex, rgb, hsl, url a gradiente/patron)
- `fill-opacity`: opacidad del relleno (0 a 1)
- `fill-rule`: regla de relleno para formas complejas (nonzero, evenodd)
- Valor por defecto: `fill="black"` (todas las formas tienen relleno negro por defecto)

### 7.2 Propiedades de trazo (stroke)
- `stroke`: color del trazo
- `stroke-width`: grosor del trazo (por defecto: 1)
- `stroke-opacity`: opacidad del trazo (0 a 1)
- Valor por defecto: `stroke="none"` (las formas no tienen trazo por defecto)

### 7.3 Terminaciones de linea (stroke-linecap)
- `butt` (por defecto): la linea termina exactamente en el punto final
- `round`: la linea termina con un semicirculo
- `square`: la linea termina con un cuadrado que extiende mas alla del punto final

### 7.4 Uniones de linea (stroke-linejoin)
- `miter` (por defecto): union en punta (angulo agudo)
- `round`: union redondeada
- `bevel`: union achaflanada (cortada)
- `miter-limit` / `stroke-miterlimit`: limita la longitud de la punta en uniones miter

### 7.5 Lineas discontinuas (stroke-dasharray)
- Sintaxis: lista de valores que alternan entre trazo y espacio
- Ejemplos:
  - `stroke-dasharray="5"`: trazos y espacios iguales de 5
  - `stroke-dasharray="5 2"`: trazos de 5, espacios de 2
  - `stroke-dasharray="5 2 8 4"`: patron mas complejo
- `stroke-dashoffset`: desplazamiento del inicio del patron de guiones
  - Valores positivos: desplaza el patron hacia atras
  - Valores negativos: desplaza el patron hacia adelante

### 7.6 Trazo y relleno como atributos vs CSS
- Pueden especificarse como atributos XML o como propiedades CSS
- Atributo: `<rect fill="red" />`
- CSS inline: `<rect style="fill: red;" />`
- CSS externo: `rect { fill: red; }`
- Prioridad: CSS externo/interno > style inline > atributos de presentacion

### 7.7 paint-order
- Controla el orden en que se pintan fill, stroke y markers
- Por defecto: fill, stroke, markers
- Ejemplo: `paint-order="stroke"` pinta el stroke debajo del fill

### 7.8 vector-effect
- `non-scaling-stroke`: el grosor del trazo no se escala con las transformaciones
- Util para mantener trazos de 1px independientemente del zoom

---

## 8. Colores y Opacidad

### 8.1 Formatos de color soportados
- Nombres de color: `red`, `blue`, `steelblue`, etc. (mismos nombres CSS)
- Hexadecimal: `#FF0000`, `#F00` (forma corta)
- RGB: `rgb(255, 0, 0)` o `rgb(100%, 0%, 0%)`
- RGBA: `rgba(255, 0, 0, 0.5)`
- HSL: `hsl(0, 100%, 50%)`
- HSLA: `hsla(0, 100%, 50%, 0.5)`
- `currentColor`: hereda el valor de la propiedad CSS `color` del elemento o su padre
- `transparent`: totalmente transparente
- `none`: sin relleno/trazo (diferente de transparent)

### 8.2 Opacidad
- `opacity`: opacidad de todo el elemento (incluyendo fill, stroke e hijos)
- `fill-opacity`: solo la opacidad del relleno
- `stroke-opacity`: solo la opacidad del trazo
- Rango: 0 (invisible) a 1 (totalmente opaco)
- La opacidad es multiplicativa: un hijo con opacity 0.5 dentro de un grupo con opacity 0.5 resulta en 0.25

### 8.3 currentColor
- Palabra clave especial que toma el valor de la propiedad `color` del contexto
- Muy util para crear SVGs que heredan el color del texto CSS
- Patron comun en iconos SVG: `fill="currentColor"`

### 8.4 Diferencia entre `none` y `transparent`
- `none`: el elemento no tiene relleno/trazo (no responde a eventos de click en la zona sin fill)
- `transparent`: tiene relleno/trazo pero es invisible (si responde a eventos segun pointer-events)

---

## 9. Gradientes

### 9.1 Gradiente lineal `<linearGradient>`
- Se define dentro de `<defs>`
- Requiere un `id` para ser referenciado
- Atributos de direccion:
  - `x1`, `y1`: punto de inicio (por defecto: 0%, 0%)
  - `x2`, `y2`: punto final (por defecto: 100%, 0% = horizontal izquierda a derecha)
- Ejemplos de direcciones comunes:
  - Horizontal: `x1="0%" y1="0%" x2="100%" y2="0%"`
  - Vertical: `x1="0%" y1="0%" x2="0%" y2="100%"`
  - Diagonal: `x1="0%" y1="0%" x2="100%" y2="100%"`

### 9.2 Gradiente radial `<radialGradient>`
- Atributos:
  - `cx`, `cy`: centro del gradiente (por defecto: 50%, 50%)
  - `r`: radio del gradiente (por defecto: 50%)
  - `fx`, `fy`: punto focal (por defecto: igual que cx, cy)
  - `fr`: radio focal (SVG 2)
- El punto focal desplazado crea gradientes asimetricos

### 9.3 Stops de color `<stop>`
- Elementos hijos de los gradientes
- Atributos:
  - `offset`: posicion en el gradiente (0% a 100%, o 0 a 1)
  - `stop-color`: el color en esa posicion
  - `stop-opacity`: opacidad del color en esa posicion
- Minimo dos stops para crear un gradiente visible
- Los stops deben ir en orden creciente de offset

### 9.4 Atributo `gradientUnits`
- `objectBoundingBox` (por defecto): coordenadas relativas al bounding box del elemento (0 a 1 o 0% a 100%)
- `userSpaceOnUse`: coordenadas absolutas en el espacio del usuario

### 9.5 Atributo `spreadMethod`
- `pad` (por defecto): los colores extremos se extienden mas alla del gradiente
- `reflect`: el gradiente se refleja (espejo) al llegar al final
- `repeat`: el gradiente se repite desde el inicio

### 9.6 Transformaciones de gradientes
- `gradientTransform`: aplica transformaciones al gradiente (rotate, skew, etc.)
- Permite rotar gradientes sin cambiar x1/y1/x2/y2

### 9.7 Referenciar gradientes
- Se usan con `fill="url(#idDelGradiente)"` o `stroke="url(#idDelGradiente)"`
- Se pueden heredar con `href` o `xlink:href` de otro gradiente

---

## 10. Patrones (Patterns)

### 10.1 El elemento `<pattern>`
- Se define dentro de `<defs>`
- Requiere un `id` para ser referenciado
- Atributos principales:
  - `x`, `y`: posicion de inicio del patron
  - `width`, `height`: tamanio de cada celda del patron
  - `viewBox`: sistema de coordenadas interno del patron

### 10.2 patternUnits
- `objectBoundingBox` (por defecto): las dimensiones del patron son relativas al elemento que lo usa
- `userSpaceOnUse`: las dimensiones son absolutas en el espacio del usuario
- Diferencias practicas entre ambos

### 10.3 patternContentUnits
- `userSpaceOnUse` (por defecto): el contenido del patron usa coordenadas del usuario
- `objectBoundingBox`: el contenido usa coordenadas relativas al bounding box
- Combinaciones con patternUnits y sus efectos

### 10.4 patternTransform
- Aplica transformaciones al patron completo
- Ejemplo: rotar un patron 45 grados para crear diagonales

### 10.5 Creacion de patrones comunes
- Rayas horizontales/verticales/diagonales
- Puntos (polka dots)
- Cuadriculas (grids)
- Patrones de ladrillos
- Patrones complejos anidados (patron dentro de patron)

### 10.6 Referenciar patrones
- `fill="url(#idDelPatron)"` o `stroke="url(#idDelPatron)"`

---

## 11. Transformaciones

### 11.1 El atributo `transform`
- Se puede aplicar a cualquier elemento grafico o grupo
- Multiples transformaciones se aplican de derecha a izquierda (la ultima escrita se aplica primero)
- Las transformaciones afectan al sistema de coordenadas del elemento

### 11.2 translate(tx, ty)
- Mueve el elemento tx unidades en X y ty unidades en Y
- Si solo se da un valor, ty = 0
- Equivalente a cambiar la posicion sin modificar x/y del elemento

### 11.3 scale(sx, sy)
- Escala el elemento sx veces en X y sy veces en Y
- Si solo se da un valor, sy = sx (escalado uniforme)
- Valores negativos reflejan el elemento
- La escala se aplica desde el origen del sistema de coordenadas (0,0), no desde el centro del elemento
- Para escalar desde el centro: translate al centro, scale, translate de vuelta

### 11.4 rotate(angle, cx, cy)
- Rota el elemento `angle` grados en sentido horario
- `cx`, `cy` (opcionales): centro de rotacion
- Sin cx/cy, rota alrededor del origen (0,0)
- Con cx/cy, equivale a: translate(cx,cy) rotate(angle) translate(-cx,-cy)

### 11.5 skewX(angle) y skewY(angle)
- `skewX`: inclina el elemento en el eje X
- `skewY`: inclina el elemento en el eje Y
- El angulo se especifica en grados

### 11.6 matrix(a, b, c, d, e, f)
- Transformacion generica usando una matriz 3x3
- Los 6 valores representan: `[a c e; b d f; 0 0 1]`
- Cualquier combinacion de translate/scale/rotate/skew se puede expresar como una sola matrix
- Correspondencias:
  - translate(tx,ty) = matrix(1, 0, 0, 1, tx, ty)
  - scale(sx,sy) = matrix(sx, 0, 0, sy, 0, 0)
  - rotate(a) = matrix(cos(a), sin(a), -sin(a), cos(a), 0, 0)

### 11.7 Encadenamiento de transformaciones
- Sintaxis: `transform="translate(50,50) rotate(45) scale(2)"`
- Orden de aplicacion: scale primero, luego rotate, luego translate (derecha a izquierda)
- El orden importa: rotate + translate != translate + rotate

### 11.8 transform-origin
- Define el punto de referencia para las transformaciones
- Sintaxis CSS: `transform-origin: 50% 50%` (centro del elemento)
- En SVG 1.1 no existia; se emulaba con translate/transform/translate
- Soportado en SVG 2 y en CSS aplicado a SVG

### 11.9 Transformaciones y sistema de coordenadas
- Cada transformacion modifica el sistema de coordenadas local del elemento
- Los hijos heredan el sistema de coordenadas transformado del padre
- Esto afecta a stroke-width, font-size y otros valores con unidades

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

---

## 17. SVG Responsivo y Accesibilidad

### 17.1 SVG responsivo basico
- Usar `viewBox` sin `width`/`height` fijos: el SVG se adapta al contenedor
- Usar `width="100%"` y `height="auto"` (o viceversa)
- El viewBox mantiene la proporcion del contenido
- `preserveAspectRatio` controla como se ajusta el contenido

### 17.2 Tecnicas de SVG fluido
- SVG inline en HTML: se comporta como un elemento de bloque
- Eliminar width/height del elemento `<svg>` y controlarlo con CSS
- Tecnica del padding-hack para aspect ratio fijo (legacy, reemplazada por aspect-ratio CSS)
- `aspect-ratio` en CSS para mantener proporciones

### 17.3 SVG adaptativo (responsive con media queries)
- Se pueden usar media queries CSS dentro de SVG
- `<style>` dentro del SVG con @media
- Ocultar/mostrar elementos segun el tamanio del viewport
- Simplificar graficos en pantallas pequenias
- NOTA: las media queries dentro de SVG responden al tamanio del viewport del SVG, no al del navegador (cuando se usa como archivo externo)

### 17.4 Accesibilidad en SVG
- `role="img"` en el elemento `<svg>` para indicar que es una imagen
- `<title>` como primer hijo: equivalente al atributo `alt` en `<img>`
- `<desc>`: descripcion larga del contenido
- `aria-labelledby`: referencia al `<title>` y/o `<desc>` por sus ids
- `aria-hidden="true"`: para SVGs puramente decorativos
- `role="presentation"`: similar a aria-hidden para SVGs decorativos
- `focusable="false"`: evita que el SVG reciba foco con teclado (IE/Edge legacy)
- Texto en SVG es accesible automaticamente
- Grupos semanticos con `role` y `aria-label`
- Patron de accesibilidad completo:
  ```xml
  <svg role="img" aria-labelledby="title desc">
    <title id="title">Titulo breve</title>
    <desc id="desc">Descripcion detallada</desc>
    <!-- contenido -->
  </svg>
  ```

### 17.5 SVG y alto contraste / modo oscuro
- Usar `currentColor` para que los colores sigan el tema
- Usar `@media (prefers-color-scheme: dark)` dentro del SVG
- Variables CSS (custom properties) para tematizacion

---

## 18. Integracion de SVG en la Web

### 18.1 SVG inline en HTML
- Se inserta directamente el codigo `<svg>` en el HTML
- Ventajas: acceso completo al DOM, estilizable con CSS, manipulable con JS
- Desventajas: aumenta el tamanio del HTML, no se cachea como archivo separado
- No necesita el namespace xmlns cuando esta inline en HTML5
- Es el metodo mas versatil

### 18.2 SVG como imagen con `<img>`
- `<img src="imagen.svg" alt="descripcion">`
- Ventajas: simple, se cachea, familiar
- Desventajas: no se puede manipular con CSS/JS externo, no se puede interactuar con elementos internos
- Las animaciones SMIL internas SI funcionan
- Los scripts internos NO se ejecutan (por seguridad)
- Los estilos externos NO se aplican

### 18.3 SVG como fondo CSS
- `background-image: url("imagen.svg")`
- Mismas limitaciones que `<img>`: sin interaccion CSS/JS
- Se puede controlar tamanio con `background-size`
- Util para patrones decorativos

### 18.4 SVG con `<object>`
- `<object type="image/svg+xml" data="imagen.svg"></object>`
- Ventajas: permite interaccion con el DOM del SVG desde JS externo (con getSVGDocument())
- Se puede incluir contenido fallback entre las etiquetas
- Tiene su propio contexto de documento

### 18.5 SVG con `<embed>`
- Similar a `<object>` pero mas antiguo
- Menos recomendado que `<object>` o inline

### 18.6 SVG con `<iframe>`
- `<iframe src="imagen.svg"></iframe>`
- El SVG tiene su propio documento y viewport
- Permite scripting si es del mismo origen

### 18.7 SVG como data URI
- En `<img>`: `<img src="data:image/svg+xml,<svg>...</svg>">`
- En CSS: `background-image: url("data:image/svg+xml,<svg>...</svg>")`
- Se puede codificar en base64 o URL-encoded
- Util para SVGs pequenios para evitar peticiones HTTP extra

### 18.8 Comparativa de metodos de integracion

| Metodo | CSS externo | JS externo | Animacion | Cache | Interactivo |
|--------|-------------|------------|-----------|-------|-------------|
| Inline | Si | Si | Si | No | Si |
| `<img>` | No | No | SMIL si | Si | No |
| CSS bg | No | No | SMIL si | Si | No |
| `<object>` | Interno | getSVGDocument | Si | Si | Si |
| `<iframe>` | Interno | postMessage | Si | Si | Si |

---

## 19. Estilado con CSS

### 19.1 Tres formas de aplicar CSS a SVG
1. **Atributos de presentacion**: directamente en el elemento (`fill="red"`)
2. **Estilo inline**: atributo `style` (`style="fill: red;"`)
3. **Hoja de estilos**: bloque `<style>` dentro del SVG o CSS externo (si es inline en HTML)

### 19.2 Prioridad de estilos (de menor a mayor)
1. Herencia del padre
2. Atributos de presentacion
3. Estilos de hoja de estilos (especificidad normal CSS)
4. Estilo inline (`style="..."`)
5. `!important`

### 19.3 Propiedades CSS especificas de SVG
- Propiedades de relleno: `fill`, `fill-opacity`, `fill-rule`
- Propiedades de trazo: `stroke`, `stroke-width`, `stroke-opacity`, `stroke-linecap`, `stroke-linejoin`, `stroke-dasharray`, `stroke-dashoffset`, `stroke-miterlimit`
- Propiedades de texto: `text-anchor`, `dominant-baseline`, `font-*`, `letter-spacing`, etc.
- Propiedades de marcadores: `marker-start`, `marker-mid`, `marker-end`
- Otras: `clip-path`, `mask`, `filter`, `opacity`, `visibility`, `display`, `pointer-events`, `paint-order`, `vector-effect`

### 19.4 Propiedades CSS generales que funcionan en SVG
- `display`: none/block/inline (ocultar elementos)
- `visibility`: visible/hidden (ocultar sin quitar del layout)
- `cursor`: cambiar el cursor al pasar sobre elementos
- `pointer-events`: controlar la interactividad (ver seccion 21)
- `transform` y `transform-origin`
- `transition` y `animation` (para animaciones CSS)
- `mix-blend-mode`: modos de mezcla
- `isolation`: contexto de mezcla
- Variables CSS (`--custom-property`)

### 19.5 Selectores CSS en SVG
- Todos los selectores CSS funcionan: clase, id, tipo, atributo, pseudo-clases
- Pseudo-clases interactivas: `:hover`, `:focus`, `:active`, `:focus-visible`
- Pseudo-clases estructurales: `:first-child`, `:nth-child()`, etc.
- Seleccion por atributo: `[fill="red"]`, `circle[r="50"]`
- No se puede seleccionar dentro de elementos `<use>` (shadow DOM)

### 19.6 Variables CSS (Custom Properties) en SVG
- Se pueden definir y usar variables CSS
- Muy util para tematizacion de SVGs
- Las variables se heredan del contexto HTML cuando el SVG es inline
- Ejemplo: `fill: var(--icon-color, currentColor)`

### 19.7 Bloque `<style>` dentro de SVG
- Se coloca tipicamente dentro de `<defs>` (pero puede ir en cualquier parte)
- Acepta la misma sintaxis CSS que HTML
- Se puede usar `<![CDATA[ ... ]]>` para escapar caracteres especiales en archivos SVG standalone
- Media queries dentro del SVG

---

## 20. Animaciones en SVG

### 20.1 Tipos de animacion disponibles
1. **SMIL (SVG Animation)**: nativa de SVG, declarativa
2. **CSS Animations/Transitions**: usando @keyframes y transition
3. **JavaScript**: usando requestAnimationFrame, Web Animations API o librerias
4. Cada tipo tiene sus ventajas y limitaciones

### 20.2 Animacion SMIL con `<animate>`
- Se coloca como hijo del elemento a animar
- Atributos:
  - `attributeName`: la propiedad a animar
  - `from`: valor inicial
  - `to`: valor final
  - `values`: lista de valores intermedios (alternativa a from/to)
  - `dur`: duracion (ej: "2s", "500ms")
  - `begin`: cuando comienza (ej: "0s", "click", "mouseover", "otroId.end")
  - `end`: cuando termina
  - `repeatCount`: veces que se repite ("indefinite" para infinito)
  - `repeatDur`: duracion total de repeticion
  - `fill`: que pasa al terminar ("freeze" = mantener, "remove" = volver al inicio)
  - `keyTimes`: tiempos clave para los valores
  - `keySplines`: curvas de bezier para interpolacion
  - `calcMode`: modo de calculo (linear, discrete, paced, spline)
  - `additive`: sum o replace
  - `accumulate`: sum o none

### 20.3 Animacion de movimiento `<animateMotion>`
- Mueve un elemento a lo largo de un trazado
- Atributos:
  - `path`: el trazado d a seguir
  - `rotate`: auto (el elemento rota siguiendo el path), auto-reverse, o un angulo fijo
  - `keyPoints`: puntos clave a lo largo del path
- Sub-elemento `<mpath>`: referencia un path existente en el documento
- Otros atributos heredados de `<animate>` (dur, begin, repeatCount, etc.)

### 20.4 Animacion de transformaciones `<animateTransform>`
- Anima transformaciones (translate, scale, rotate, skewX, skewY)
- Atributo `type`: el tipo de transformacion a animar
- Atributos from/to/values con la sintaxis de la transformacion correspondiente
- Solo una animateTransform por tipo se aplica a la vez (por defecto se reemplazan)
- `additive="sum"` para combinar multiples transformaciones animadas

### 20.5 `<set>`
- Establece un valor de atributo en un momento dado (sin interpolacion)
- Util para cambios discretos (mostrar/ocultar, cambiar color de golpe)
- Atributos: `attributeName`, `to`, `begin`, `dur`

### 20.6 Sincronizacion de animaciones SMIL
- `begin="otroId.begin"`: comienza cuando otra animacion comienza
- `begin="otroId.end"`: comienza cuando otra animacion termina
- `begin="otroId.end + 1s"`: comienza 1 segundo despues de que otra termine
- `begin="click"`: comienza al hacer click
- `begin="mouseover"`: comienza al pasar el mouse
- `begin="2s;click"`: comienza a los 2s Y al hacer click
- Eventos de teclado y otros eventos DOM

### 20.7 Animaciones CSS en SVG
- `transition` en propiedades SVG: fill, stroke, opacity, transform, etc.
- `@keyframes` con propiedades SVG
- Propiedades animables con CSS: fill, stroke, stroke-width, stroke-dasharray, stroke-dashoffset, opacity, transform, r (radio en SVG 2), cx, cy, x, y, width, height
- Ventajas sobre SMIL: sintaxis mas familiar, mejor rendimiento en algunos casos, mas herramientas de desarrollo

### 20.8 Tecnicas de animacion comunes
- **Line drawing (dibujo de linea)**:
  - Usa stroke-dasharray y stroke-dashoffset
  - Se calcula la longitud total del path con `getTotalLength()`
  - Se anima dashoffset desde la longitud total hasta 0
- **Morphing de formas**: animar el atributo `d` del path (los paths deben tener el mismo numero de puntos)
- **Pulsar/latir**: escalar un elemento repetidamente
- **Rotar**: spin continuo con rotate transform
- **Fade in/out**: animar opacity
- **Color transitions**: animar fill o stroke entre colores

### 20.9 Estado actual de SMIL
- Chrome depreco SMIL pero revirtio la decision: SMIL sigue soportado
- Soporte en todos los navegadores modernos
- SVG 2 esta refinando SMIL pero no lo elimina
- Para proyectos nuevos: CSS o JS suelen ser preferidos, pero SMIL sigue siendo valido

---

## 21. Interactividad con JavaScript

### 21.1 Acceso al DOM SVG
- Los elementos SVG son parte del DOM cuando son inline
- `document.querySelector('circle')`, `document.getElementById('miCirculo')`
- `document.querySelectorAll('.clase')` funciona igual que en HTML
- Los elementos SVG son instancias de `SVGElement` (hereda de `Element`)

### 21.2 Manipulacion de atributos
- `element.getAttribute('cx')` y `element.setAttribute('cx', '100')`
- `element.style.fill = 'red'` para propiedades CSS
- Propiedades SVG especificas: `element.cx.baseVal.value = 100` (interfaz SVGAnimatedLength)
- `element.classList.add()`, `.remove()`, `.toggle()` para clases CSS
- `element.getBBox()`: obtiene el bounding box del elemento (x, y, width, height)
- `element.getCTM()`: obtiene la Current Transformation Matrix
- `element.getScreenCTM()`: CTM relativa a la pantalla

### 21.3 Creacion dinamica de elementos SVG
- Se debe usar `document.createElementNS()` con el namespace SVG
- Namespace SVG: `"http://www.w3.org/2000/svg"`
- Ejemplo: `document.createElementNS("http://www.w3.org/2000/svg", "circle")`
- Usar `setAttribute()` para establecer atributos
- Agregar al DOM con `appendChild()` o `insertBefore()`

### 21.4 Eventos en SVG
- Todos los eventos DOM funcionan: click, mousedown, mouseup, mousemove, mouseover, mouseout, mouseenter, mouseleave
- Eventos tactiles: touchstart, touchmove, touchend
- Eventos de puntero (recomendados): pointerdown, pointermove, pointerup, pointerover, pointerout
- Registro de eventos: `element.addEventListener('click', handler)`
- El objeto `event` contiene coordenadas, target, etc.

### 21.5 Coordenadas del mouse en SVG
- `event.clientX/clientY`: coordenadas del viewport del navegador
- Conversion a coordenadas SVG:
  ```javascript
  const svg = document.querySelector('svg');
  const pt = svg.createSVGPoint();
  pt.x = event.clientX;
  pt.y = event.clientY;
  const svgPoint = pt.matrixTransform(svg.getScreenCTM().inverse());
  ```
- `svgPoint.x`, `svgPoint.y`: coordenadas en el espacio del SVG

### 21.6 pointer-events en SVG
- Controla que partes del elemento responden a eventos del mouse/puntero
- Valores:
  - `visiblePainted` (por defecto): responde en areas con fill/stroke visible
  - `visibleFill`: responde en el area de fill visible
  - `visibleStroke`: responde en el area de stroke visible
  - `visible`: responde en fill + stroke si visibility es visible
  - `painted`: responde en areas con fill/stroke (aunque no sea visible)
  - `fill`: responde en el area de fill
  - `stroke`: responde en el area de stroke
  - `all`: responde en toda el area de fill + stroke
  - `none`: no responde a eventos
- Uso comun: `pointer-events="none"` para que un elemento no intercepte clicks

### 21.7 Drag and drop en SVG
- Capturar mousedown/pointerdown en el elemento
- Calcular offset entre la posicion del mouse y la posicion del elemento
- En mousemove/pointermove: actualizar la posicion del elemento
- En mouseup/pointerup: dejar de mover
- Consideraciones con transformaciones y viewBox

### 21.8 Metodos SVG DOM especificos
- `SVGPathElement.getTotalLength()`: longitud total de un path
- `SVGPathElement.getPointAtLength(distance)`: coordenadas en un punto del path
- `SVGGeometryElement.isPointInFill(point)`: si un punto esta dentro del fill
- `SVGGeometryElement.isPointInStroke(point)`: si un punto esta en el stroke
- `SVGSVGElement.createSVGPoint()`: crea un objeto SVGPoint
- `SVGSVGElement.createSVGMatrix()`: crea un objeto SVGMatrix

### 21.9 Scripts dentro de archivos SVG
- Se puede incluir `<script>` dentro del SVG
- Usa `<![CDATA[ ... ]]>` para evitar conflictos con XML
- Solo se ejecuta cuando el SVG se carga como documento (inline, object, iframe)
- No se ejecuta cuando se usa como `<img>` o `background-image` (por seguridad)

---

## 22. Sprites SVG e Iconos

### 22.1 Sistema de iconos con SVG
- Ventajas sobre icon fonts: multicolor, precision en pixeles, accesibilidad, menos hacks
- Ventajas sobre imagenes raster: escalabilidad, estilizabilidad, menor tamanio

### 22.2 SVG Sprite con `<symbol>` y `<use>`
- Definir todos los iconos como `<symbol>` dentro de un unico SVG
- Cada `<symbol>` tiene su propio `id` y `viewBox`
- Referenciar con `<use href="#iconId">`
- El SVG sprite puede estar inline en el HTML (oculto con `display:none` o `hidden`)
- O puede ser un archivo externo referenciado con `<use href="sprites.svg#iconId">`

### 22.3 SVG Sprite externo
- Archivo `.svg` con multiples `<symbol>`
- Referencia: `<use href="sprites.svg#iconId">`
- Limitaciones cross-origin: no funciona entre dominios diferentes sin CORS
- No funciona en IE/Edge legacy con archivos externos (se necesita polyfill como svg4everybody)
- No se puede estilizar el contenido interno con CSS externo

### 22.4 SVG inline como componentes (React, Vue, etc.)
- Cada icono como un componente que retorna SVG inline
- Maximo control de estilizacion y animacion
- Patron comun en frameworks modernos
- Herramientas: SVGR (React), vue-svg-loader, etc.

### 22.5 Organizacion de un sistema de iconos
- Convenciones de naming para ids
- Tamanio base consistente (16x16, 24x24, etc.)
- Uso de `viewBox="0 0 24 24"` como estandar comun
- currentColor como fill por defecto para heredar color del contexto

---

## 23. Optimizacion de SVG

### 23.1 Por que optimizar SVG
- Los editores graficos generan SVG con metadatos innecesarios
- Reducir tamanio de archivo para mejor rendimiento web
- Eliminar informacion redundante o innecesaria
- Mejorar legibilidad del codigo si se va a editar manualmente

### 23.2 SVGO (SVG Optimizer)
- Herramienta principal de optimizacion
- Uso por CLI: `svgo archivo.svg`
- Plugins de SVGO y que hacen:
  - `removeDoctype`: elimina DOCTYPE
  - `removeXMLProcInst`: elimina declaracion XML
  - `removeComments`: elimina comentarios
  - `removeMetadata`: elimina `<metadata>`
  - `removeEditorsNSData`: elimina namespaces de editores (Inkscape, Illustrator)
  - `cleanupAttrs`: limpia atributos con espacios extra
  - `mergeStyles`: combina bloques `<style>`
  - `inlineStyles`: convierte estilos en atributos de presentacion
  - `removeUselessDefs`: elimina `<defs>` vacios
  - `cleanupNumericValues`: reduce precision de numeros
  - `convertColors`: convierte colores a formato mas corto
  - `removeUnknownsAndDefaults`: elimina atributos desconocidos y valores por defecto
  - `removeNonInheritableGroupAttrs`: elimina atributos no heredables de grupos
  - `removeUselessStrokeAndFill`: elimina fill/stroke innecesarios
  - `cleanupIds`: acorta o elimina ids no usados
  - `convertShapeToPath`: convierte formas basicas a paths (reduce tamanio pero pierde semantica)
  - `mergePaths`: combina paths adyacentes con los mismos atributos
  - `convertPathData`: optimiza los datos del atributo d
  - `removeEmptyAttrs`, `removeEmptyContainers`, `removeEmptyText`
  - `collapseGroups`: elimina grupos innecesarios
  - `sortAttrs`: ordena atributos (mejora compresion gzip)
- Configuracion de SVGO: `svgo.config.js`
- Precauciones: algunos plugins pueden romper animaciones o interactividad

### 23.3 Optimizacion manual
- Reducir decimales en coordenadas (2-3 decimales suele ser suficiente)
- Usar coordenadas relativas en paths (suelen producir numeros mas pequenios)
- Eliminar grupos innecesarios (`<g>` sin atributos)
- Combinar paths con los mismos estilos
- Usar `<use>` para elementos repetidos
- Simplificar trazados (menos puntos de control)
- Preferir formas basicas sobre paths equivalentes (mas legible)

### 23.4 Compresion
- SVG se comprime muy bien con gzip/brotli (es texto)
- Formato SVGZ: SVG comprimido con gzip (requiere header Content-Encoding: gzip)
- Configurar el servidor para servir SVG con compresion

### 23.5 Rendimiento de renderizado
- Filtros complejos son costosos (especialmente feGaussianBlur con stdDeviation grande)
- Muchos elementos SVG pueden degradar el rendimiento
- Clip-paths y masks tienen costo de renderizado
- `will-change` en CSS para optimizar animaciones
- Considerar Canvas o WebGL para graficos con miles de elementos

---

## 24. Herramientas y Flujo de Trabajo

### 24.1 Editores graficos
- **Inkscape**: gratuito, open source, exporta SVG limpio
- **Adobe Illustrator**: profesional, necesita optimizar la salida SVG
- **Figma**: diseno web/UI, exporta SVG, plugin de optimizacion
- **Sketch**: macOS, popular para diseno UI
- **Affinity Designer**: alternativa a Illustrator

### 24.2 Editores de codigo
- Cualquier editor de texto puede editar SVG
- VS Code con extensiones SVG (preview, snippets, linting)
- Emmet para generar SVG rapidamente

### 24.3 Herramientas online
- **SVGOMG**: interfaz web para SVGO (jakearchibald.github.io/svgomg/)
- **SVG Path Editor**: editores visuales de paths
- **SVG Viewer**: previsualizar SVGs en el navegador
- **Boxy SVG**: editor online completo
- **Method Draw**: editor SVG online simple
- **URL Encoder for SVG**: para data URIs

### 24.4 Herramientas de desarrollo
- DevTools del navegador: inspeccionar SVG inline como cualquier elemento HTML
- Visualizar bounding boxes, transformaciones, filtros
- Editar atributos en tiempo real
- Animaciones: panel de animaciones en Chrome DevTools

### 24.5 Librerias JavaScript para SVG
- **D3.js**: visualizacion de datos (bindea datos a SVG)
- **Snap.svg**: manipulacion de SVG (sucesor de Raphael)
- **SVG.js**: libreria ligera para crear y animar SVG
- **GreenSock (GSAP)**: animaciones de alto rendimiento (soporta SVG)
- **Anime.js**: libreria de animaciones ligera
- **Vivus.js**: animacion de line-drawing
- **Lottie**: reproduce animaciones After Effects exportadas como JSON (puede usar SVG como renderer)
- **Paper.js**: graficos vectoriales con canvas y SVG

### 24.6 Integracion con build tools
- Importar SVG en Webpack (svg-inline-loader, @svgr/webpack)
- Importar SVG en Vite (vite-plugin-svgr, vite-svg-loader)
- PostCSS plugins para SVG en CSS

---

## 25. Buenas Practicas y Patrones Comunes

### 25.1 Estructura y organizacion
- Usar `<defs>` para todo lo reutilizable
- Agrupar elementos relacionados con `<g>` y dar ids/clases descriptivos
- Mantener un orden logico en el codigo: defs primero, luego contenido
- Usar viewBox siempre para garantizar escalabilidad
- Preferir coordenadas relativas al viewBox sobre valores absolutos

### 25.2 Nombrado y semantica
- Ids descriptivos para elementos que se referencian
- Clases CSS para estilizado (igual que en HTML)
- `<title>` y `<desc>` para accesibilidad
- Comentarios XML para secciones complejas

### 25.3 Rendimiento
- Mantener el numero de nodos bajo (menos de ~1000 para rendimiento optimo)
- Evitar filtros complejos en elementos animados
- Usar CSS transitions en lugar de SMIL cuando sea posible
- Considerar alternativas (Canvas, WebGL) para graficos muy complejos
- Lazy loading de SVGs pesados

### 25.4 Compatibilidad
- Probar en multiples navegadores
- Usar prefijos de vendor cuando sea necesario para CSS
- Tener en cuenta IE11 si se necesita soporte legacy:
  - No soporta SVG 2
  - Necesita `xlink:href` en vez de `href`
  - Problemas con `<use>` externo
  - Necesita width/height explicitos
- Fallbacks: `<img>` con PNG como alternativa dentro de `<object>`

### 25.5 Seguridad
- SVG puede contener `<script>`: cuidado con SVG de fuentes no confiables
- SVG en `<img>` y CSS `background-image` son seguros (scripts no se ejecutan)
- Sanitizar SVG subido por usuarios (eliminar scripts, event handlers, foreignObject)
- Nunca confiar en SVG externo para mostrarlo inline sin sanitizar
- XSS a traves de SVG: los atributos de evento (onclick, onload) son vectores de ataque

### 25.6 Patrones de diseno comunes
- **Icono adaptativo**: SVG que cambia de detalle segun tamanio (media queries internas)
- **Logo responsive**: multiples versiones del logo que se muestran segun el espacio
- **Grafico de datos**: combinar formas basicas y texto para charts simples
- **Ilustracion interactiva**: hover effects, tooltips, animaciones al hacer click
- **Loader/spinner**: animacion de carga con rotate + stroke-dasharray
- **Progress bar**: barra de progreso con stroke-dashoffset animado
- **Mapa interactivo**: regiones como paths con eventos hover/click
- **Texto decorativo**: texto en paths curvos con gradientes

### 25.7 Checklist antes de publicar
- [ ] viewBox definido correctamente
- [ ] Tamanios width/height adecuados o responsivos
- [ ] Accesibilidad: title, desc, role, aria-labels
- [ ] Optimizado con SVGO o similar
- [ ] Colores usando currentColor donde sea apropiado
- [ ] Sin metadatos innecesarios del editor
- [ ] Funciona en los navegadores objetivo
- [ ] Sin scripts si se va a usar como `<img>` o background
- [ ] Clases/ids limpios y descriptivos
- [ ] Compresion gzip/brotli configurada en el servidor

---

## Orden de Estudio Recomendado

### Fase 1: Fundamentos (Secciones 1-4)
Entender que es SVG, su estructura, el sistema de coordenadas y las formas basicas.

### Fase 2: Dibujo (Secciones 5-8)
Dominar paths, texto, fill/stroke y colores. Con esto puedes crear cualquier grafico estatico.

### Fase 3: Apariencia Avanzada (Secciones 9-11)
Gradientes, patrones y transformaciones para graficos mas ricos visualmente.

### Fase 4: Reutilizacion y Efectos (Secciones 12-16)
Agrupacion, recorte, mascaras, filtros, marcadores e imagenes embebidas.

### Fase 5: Integracion Web (Secciones 17-19)
SVG responsivo, accesibilidad, metodos de integracion y estilado con CSS.

### Fase 6: Dinamismo (Secciones 20-21)
Animaciones (SMIL, CSS, JS) e interactividad con JavaScript.

### Fase 7: Produccion (Secciones 22-25)
Sprites, optimizacion, herramientas y buenas practicas.

---

> **Nota**: Este roadmap cubre SVG a nivel basico-intermedio. Temas avanzados como filtros de iluminacion complejos, SVG en Web Components, animaciones complejas con GSAP, graficos 3D con SVG, o la especificacion SVG 2 en profundidad quedan fuera de este alcance.
