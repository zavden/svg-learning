# Seccion 6: Texto en SVG

> El texto en SVG no es como el texto en HTML. Tiene sus propias reglas de posicionamiento,
> sus propias limitaciones y sus propias ventajas. Entender la diferencia es clave.

---

## 6.1 El elemento `<text>`

### Naturaleza del texto SVG
- El texto en SVG se renderiza como **graficos vectoriales**: cada caracter es una forma dibujada
- Al mismo tiempo, sigue siendo **texto real**: seleccionable, copiable, buscable por el navegador y por motores de busqueda
- No es una imagen del texto: es texto con renderizado vectorial
- Se comporta como cualquier otro elemento SVG: puede tener `fill`, `stroke`, `transform`, `opacity`, etc.

### Posicionamiento del texto

**`x` e `y` — El punto de anclaje**
- `x`: coordenada horizontal del punto de inicio del texto
- `y`: coordenada vertical de la **linea base** del texto (no del borde superior)
- La linea base es la linea imaginaria sobre la que "descansan" los caracteres (como las rayas en un cuaderno)
- Las letras con descendentes (p, q, g, y) bajan por debajo de la linea base
- Las letras con ascendentes (b, d, h, l) suben por encima

**El problema del y=0**
- Un `<text y="0">` coloca la linea base en y=0
- Como el eje Y de SVG va hacia abajo, y=0 es el borde superior del viewport
- El texto quedara mayormente fuera del viewport (solo se ven los descendentes)
- Error muy comun de principiantes: poner texto con y pequenio y no verlo

**Valores tipicos para el primer texto**
- `y` deberia ser al menos igual al tamanio de la fuente (ej: si `font-size="16"`, usar `y="16"` o mas)

### Diferencias respecto al texto HTML
| Aspecto | HTML | SVG `<text>` |
|---------|------|-------------|
| Flujo automatico | Si (word wrap, saltos de linea) | No |
| Posicionamiento | CSS box model | Coordenadas absolutas |
| Referencia de y | Borde superior del elemento | Linea base del texto |
| Seleccion | Si | Si |
| Accesibilidad | Si | Si |
| Herencia de estilos CSS | Si | Parcial |

---

## 6.2 Atributos tipograficos

### La fuente

**`font-family`**
- Familia tipografica del texto
- Acepta los mismos valores que CSS: nombres de fuente, fuentes genéricas (`serif`, `sans-serif`, `monospace`, `cursive`, `fantasy`)
- Ejemplo: `font-family="Arial, sans-serif"`
- Si la fuente no esta disponible en el sistema del usuario, el navegador usa la fuente de fallback
- Se puede usar con `@font-face` dentro de un bloque `<style>` en el SVG para embeber fuentes web

**`font-size`**
- Tamanio de la fuente en unidades de usuario (o con unidades explicitas: px, em, pt, etc.)
- Tambien puede especificarse con palabras clave: `small`, `medium`, `large`, `x-large`, etc.
- Afecta al tamanio del texto y tambien a los valores en `em`

### Peso y estilo

**`font-weight`**
- Grosor de la fuente
- Valores numericos: `100`, `200`, ..., `900` (no todas las fuentes tienen todos los pesos)
- Valores nominales: `normal` (400), `bold` (700), `lighter`, `bolder`

**`font-style`**
- `normal`: texto recto
- `italic`: variante italica de la fuente (si existe)
- `oblique`: version inclinada sintetizada si no hay variante italica real

**`font-variant`**
- `normal`: texto normal
- `small-caps`: las letras minusculas se renderizan como mayusculas pequenias

### Decoracion y espaciado

**`text-decoration`**
- `none`: sin decoracion
- `underline`: subrayado
- `overline`: linea encima del texto
- `line-through`: tachado
- Se puede combinar: `underline line-through`

**`letter-spacing`**
- Espacio adicional entre caracteres (kerning global)
- Valor positivo: separa los caracteres
- Valor negativo: los acerca
- Unidades: valores del usuario o con unidades CSS

**`word-spacing`**
- Espacio adicional entre palabras (ademas del espacio natural)
- Mismas unidades que `letter-spacing`

---

## 6.3 Alineacion y posicionamiento

### Alineacion horizontal: `text-anchor`

Controla como se alinea el texto respecto al punto `x`:

**`start`** (por defecto en la mayoria de contextos)
- El punto `x` es el borde **izquierdo** del texto (en idiomas de izquierda a derecha)
- El texto se extiende hacia la derecha desde `x`

**`middle`**
- El punto `x` es el **centro horizontal** del texto
- El texto se extiende por igual a ambos lados de `x`
- Muy util para centrar etiquetas en graficos de datos

**`end`**
- El punto `x` es el borde **derecho** del texto
- El texto se extiende hacia la izquierda desde `x`

**Caso de uso tipico**
- Etiquetas de eje en graficos: `text-anchor="end"` para etiquetas a la izquierda de un eje vertical
- Titulos centrados: `text-anchor="middle"` con `x` igual a la mitad del SVG

### Alineacion vertical: `dominant-baseline`

Controla la referencia vertical del punto `y`:

**`auto`** (por defecto)
- Comportamiento por defecto del idioma actual (generalmente la linea base del texto)

**`middle`**
- El punto `y` es el **centro vertical** del texto (aproximadamente)
- Util para centrar texto verticalmente en un area

**`hanging`**
- El punto `y` es el **borde superior** del texto (la linea de colgado)
- Util para posicionar texto desde arriba en vez de desde la linea base

**`text-before-edge`** / **`text-after-edge`**
- Bordes superior e inferior del em box

**`central`**
- Centro del em box (similar a `middle` pero mas preciso tipograficamente)

**`ideographic`**, **`alphabetic`**, **`mathematical`**
- Referencias para distintos sistemas de escritura y contextos matematicos

### Desplazamiento relativo: `dx` y `dy`

**`dx` — Desplazamiento horizontal adicional**
- Se suma a la posicion actual del texto
- Acepta un unico valor (desplaza todo el texto) o una lista de valores (desplaza caracter a caracter)
- `dx="5"`: todo el texto se desplaza 5 unidades a la derecha
- `dx="5 0 10 0 -3"`: cada caracter tiene su propio desplazamiento horizontal

**`dy` — Desplazamiento vertical adicional**
- Misma logica que `dx` pero en vertical
- Muy util para crear texto multilinea: `<tspan dy="1.2em">` mueve el tspan a la siguiente linea
- Acepta valores negativos para mover hacia arriba

**Lista de valores para dx/dy**
- Si hay menos valores que caracteres, el ultimo valor se aplica a los caracteres restantes
- Esto permite ajustes de kerning manual para caracteres especificos

### Rotacion de caracteres: `rotate`

- Acepta un angulo o una lista de angulos (uno por caracter)
- `rotate="45"`: todos los caracteres rotan 45 grados
- `rotate="0 45 90 135"`: cada caracter rota un angulo diferente
- La rotacion es relativa al punto base de cada caracter (no al centro del texto completo)

---

## 6.4 El elemento `<tspan>`

### Que es `<tspan>`
- Sub-elemento de `<text>` que permite aplicar estilos diferentes a **partes del texto**
- Similar a `<span>` en HTML pero con capacidades de posicionamiento propias de SVG
- El texto dentro de un `<tspan>` sigue el flujo del elemento `<text>` padre

### Casos de uso principales

**Cambiar el estilo de una parte del texto**
- Poner una palabra en negrita dentro de una frase
- Cambiar el color de un numero dentro de una etiqueta
- Subrayar una parte del texto

**Reposicionar partes del texto**
- Crear texto multilinea (con `dy` en cada tspan)
- Crear sub/superindices (con `dy` y `font-size` reducido)
- Ajustar el kerning de caracteres especificos

### Atributos propios de `<tspan>`
- `x`, `y`: posicion absoluta (reinicia el punto de referencia del texto)
- `dx`, `dy`: desplazamiento relativo respecto a la posicion actual
- `rotate`: rotacion de los caracteres del span
- Todos los atributos tipograficos: `font-size`, `font-weight`, `fill`, `stroke`, etc.

### La diferencia entre `x/y` y `dx/dy` en `<tspan>`

**`x` / `y` (absolutos)**
- Mueven el tspan a una posicion exacta del sistema de coordenadas
- Interrumpen el flujo natural del texto: el siguiente texto del padre continua desde esa posicion
- Uso: crear texto en columnas o en posiciones especificas

**`dx` / `dy` (relativos)**
- Mueven el tspan relativo a donde estaria normalmente en el flujo del texto
- Mas "natural" para ajustes menores
- Uso tipico: `<tspan dy="1.2em">` para nueva linea

### Anidamiento de `<tspan>`
- Un `<tspan>` puede contener otro `<tspan>`
- Los estilos se heredan del exterior al interior
- El anidamiento profundo puede complicar el calculo de posiciones

### Limitacion: `<tspan>` no puede contener elementos graficos
- Solo puede contener texto y otros `<tspan>`
- No se puede poner un `<rect>` o `<circle>` dentro de un `<tspan>`

---

## 6.5 Texto en trazados: `<textPath>`

### Que hace `<textPath>`
- Hace que el texto siga la curva de un `<path>` en lugar de una linea horizontal recta
- El texto se "dobla" siguiendo la geometria del trazado
- Permite crear efectos tipograficos imposibles con texto plano

### Requisitos basicos
- Debe ser hijo de un elemento `<text>`
- El path que sigue debe tener un `id`
- Se referencia con `href="#idDelPath"` (o `xlink:href` en SVG 1.1)
- El path referenciado puede estar visible en el SVG o invisible dentro de `<defs>`

### Atributos de `<textPath>`

**`href` (o `xlink:href`)**
- Referencia al `id` del `<path>` que el texto va a seguir
- El path puede ser cualquier forma: una curva, un circulo, una linea

**`startOffset`**
- Donde empieza el texto a lo largo del path
- Valor `0%` o `0`: el texto empieza al inicio del path
- Valor `50%`: el texto empieza en la mitad del path
- Valor `100%`: el texto empieza al final del path (tipicamente no visible)
- Combinado con `text-anchor="middle"` y `startOffset="50%"`: texto centrado en el path

**`method`**
- `align` (por defecto): los caracteres se rotan para seguir la tangente del path
- `stretch`: los caracteres se distorsionan para alinearse con el path (poco soportado)

**`spacing`**
- `auto` (por defecto): el espaciado entre caracteres puede ajustarse para seguir el path
- `exact`: el espaciado exacto se mantiene aunque el texto no siga perfectamente el path

### Texto en trazado circular
- El path que "da la vuelta" permite crear texto en circulo
- Texto en la parte superior: el path va en sentido antihorario para que el texto quede en el exterior
- Texto en la parte inferior: el path va en sentido horario con `startOffset` adecuado
- Alternativa: usar dos paths semicirculares para texto superior e inferior

### Limitaciones de `<textPath>`
- El texto no se ajusta automaticamente si es mas largo que el path (se recorta)
- No hay control linea a linea dentro del path
- El interlineado no existe en paths (no hay concepto de "segunda linea" en un path curvo)
- La alineacion vertical del texto sobre el path es fija (linea base sobre el path)

---

## 6.6 Limitaciones del texto en SVG

### Sin flujo automatico
- SVG no tiene concepto de "caja de texto" que hace word-wrap
- El texto **no se rompe automaticamente** en multiples lineas
- Si el texto es mas largo que el espacio disponible: se sale del viewport o se recorta (dependiendo del overflow)
- Para texto largo hay que partir manualmente usando multiples `<tspan>` o `<text>` con posiciones calculadas

### Texto multilinea: las opciones disponibles

**Opcion 1: Multiples elementos `<text>`**
- Un `<text>` por linea, cada uno con su propio `y` calculado
- Simple pero verbose; hay que calcular el espaciado manualmente

**Opcion 2: `<tspan>` con `dy`**
- Un `<text>` contenedor con `<tspan dy="1.2em">` para cada linea
- El `dy="1.2em"` mueve cada linea 1.2 veces el tamanio de fuente hacia abajo
- Mas flexible que multiples `<text>` para texto relacionado

**Opcion 3: `<foreignObject>` con HTML**
- Incrustar un elemento HTML (`<div>`, `<p>`) con texto usando `<foreignObject>`
- Tiene word-wrap nativo y toda la potencia CSS
- Limitaciones: soporte variable en algunos contextos; no funciona en SVG como imagen
- Ver seccion 16.2 para detalles

### Dependencia de fuentes
- Si el usuario no tiene la fuente especificada instalada, el navegador usa una fuente de fallback
- La fuente de fallback puede tener metricas diferentes: el texto puede quedar mal posicionado o fuera de lugar
- Solucion: usar `@font-face` para embeber la fuente en el SVG (aumenta el tamanio del archivo)
- Alternativa: convertir el texto a paths (`Object to Path` en Inkscape) para independencia tipografica

### Texto convertido a paths: ventajas y desventajas
| Aspecto | Texto SVG | Texto como path |
|---------|-----------|-----------------|
| Dependencia de fuente | Si | No |
| Seleccionable | Si | No |
| Accesible | Si | Solo con `<title>` |
| Indexable | Si | No |
| Editabilidad | Simple | Compleja |
| Tamanio de archivo | Pequenio | Puede ser grande |

**Regla practica**: texto informativo o de contenido → mantener como texto SVG real. Texto de marca o logos → convertir a paths si la fuente es especial.

---

## 6.7 Seleccion y busqueda de texto

### El texto SVG es texto real
- El texto dentro de `<text>` y `<tspan>` es **seleccionable con el cursor** en el navegador
- Se puede copiar y pegar igual que cualquier otro texto de la pagina
- Esto ocurre independientemente de si el SVG esta inline o como archivo externo en `<img>` (aunque en `<img>` no es interactivo)

### Indexabilidad por motores de busqueda
- El texto en SVG inline es indexado por Google y otros motores de busqueda
- Un mapa SVG con nombres de ciudades como texto SVG sera indexado correctamente
- El texto dentro de SVG como imagen (`<img src="...svg">`) puede o no ser indexado dependiendo del motor

### Accesibilidad del texto
- Los lectores de pantalla pueden leer el texto SVG directamente
- No se necesita texto alternativo adicional para el contenido de texto
- El texto sigue el orden del DOM del SVG (orden en el codigo fuente)
- Para texto con significado especifico (como un numero en un medidor), considerar agregar `aria-label` contextual en el elemento `<svg>` contenedor

### Busqueda en la pagina (Ctrl+F)
- Los navegadores modernos incluyen el texto SVG inline en la busqueda de la pagina
- Importante para mapas, diagramas y graficos que contienen etiquetas de texto
- El texto dentro de SVG como `<img>` no es buscable
