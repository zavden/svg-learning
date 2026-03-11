# FASE 2: Dibujo
> Secciones 5-8 del Roadmap SVG Basico-Intermedio
> Dominar paths, texto, fill/stroke y colores. Con esto puedes crear cualquier grafico estatico.

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
