# Seccion 15: Marcadores (Markers)

> Los marcadores son graficos pequenios que se colocan automaticamente en los extremos
> o vertices de lineas y paths. Son la forma nativa de SVG para crear flechas,
> puntos en graficos y decoraciones de linea.

---

## 15.1 El elemento `<marker>`

### Que es un marcador
- Un `<marker>` es un **elemento grafico pequenio** que SVG posiciona automaticamente en puntos especificos de lineas y paths
- El navegador calcula la posicion y, opcionalmente, la rotacion del marcador
- Evita tener que calcular manualmente la posicion y el angulo de cada cabeza de flecha o punto

### Donde se define
- Siempre dentro de `<defs>` con un `id` unico
- No se renderiza directamente; se instancia automaticamente por el navegador en los puntos indicados

### Contenido del marcador
- Puede contener cualquier elemento SVG: formas, paths, texto, grupos
- El contenido se dibuja dentro del viewport del marcador (definido por `markerWidth`, `markerHeight` y `viewBox`)
- El contenido hereda algunos atributos del elemento padre (segun el atributo `markerUnits`)

### Atributos principales

**`markerWidth` y `markerHeight`**
- Dimensiones del **viewport** del marcador (el area donde se dibuja su contenido)
- Valores por defecto: `markerWidth="3"` `markerHeight="3"`
- Si `markerUnits="strokeWidth"` (por defecto): estas dimensiones se multiplican por el `stroke-width` del elemento para obtener el tamano real en pixels
- Si `markerUnits="userSpaceOnUse"`: son dimensiones absolutas en el espacio del usuario

**`refX` y `refY`**
- El **punto de anclaje** del marcador: el punto del marcador que se coloca exactamente en el vertice de la linea
- Por defecto: `refX="0"` `refY="0"` (la esquina superior izquierda del viewport del marcador)
- Para una punta de flecha que apunta hacia la derecha: `refX` deberia ser el extremo derecho del triangulo
- Para un punto circular centrado en el vertice: `refX = markerWidth/2`, `refY = markerHeight/2`

**Calcular refX/refY correctamente**
- Si el viewBox del marcador es `"0 0 10 10"` y el marcador es un circulo en el centro:
  - `refX="5"` `refY="5"` — el centro del circulo se coloca en el vertice
- Si es una punta de flecha que apunta a la derecha con el pico en x=10:
  - `refX="10"` `refY="5"` — el pico se coloca en el vertice, el marcador "viene desde atras"

**`viewBox`**
- Sistema de coordenadas interno del marcador
- Misma logica que el `viewBox` del elemento `<svg>`
- Permite definir el contenido del marcador en cualquier sistema de coordenadas y escalarlo al viewport del marcador

**`orient` — Orientacion del marcador**
- Controla como se rota el marcador al colocarse en el path:
  - `auto`: el marcador rota para alinearse con la **tangente** de la linea en ese punto (sigue la direccion de la linea)
  - `auto-start-reverse`: igual que `auto` pero el marcador de inicio (`marker-start`) se rota 180° adicionales
  - Un angulo fijo en grados (ej: `orient="90"`): el marcador siempre tiene esa orientacion, independiente de la direccion de la linea
- Para flechas: siempre usar `orient="auto"` para que la punta siga la direccion de la linea

**`markerUnits` — Sistema de unidades del viewport del marcador**
- `strokeWidth` (por defecto): `markerWidth` y `markerHeight` se multiplican por el `stroke-width` del elemento
  - Si `stroke-width="2"` y `markerWidth="5"`: el marcador ocupa `2 * 5 = 10px` de ancho real
  - El marcador escala automaticamente con el grosor de la linea
- `userSpaceOnUse`: `markerWidth` y `markerHeight` son en unidades absolutas del espacio del usuario
  - El marcador tiene tamano fijo independientemente del `stroke-width`

---

## 15.2 Aplicacion de marcadores

### Los tres atributos de aplicacion

**`marker-start`**
- Coloca el marcador en el **primer punto** del path (el punto inicial, tras el primer `M`)
- `marker-start="url(#idDelMarcador)"`

**`marker-mid`**
- Coloca el marcador en **cada vertice intermedio** del path
- Para una `<polyline>` con 5 puntos: `marker-mid` aparece en los puntos 2, 3 y 4 (no el primero ni el ultimo)
- Para un path con curvas: aparece en cada punto de control explícito (no en puntos a lo largo de la curva)

**`marker-end`**
- Coloca el marcador en el **ultimo punto** del path
- `marker-end="url(#idDelMarcador)"`

**`marker` — Atajo para los tres**
- `marker="url(#id)"` aplica el mismo marcador en start, mid y end
- Es un atributo abreviado; si se necesitan marcadores diferentes en cada posicion, usar los tres atributos individuales

### Diferente marcador en cada posicion

Es comun necesitar marcadores distintos:
- **Flechas bidireccionales**: `marker-start` con punta apuntando hacia atras, `marker-end` con punta hacia adelante
- **Lineas de inicio-fin**: un circulo en el inicio, una flecha en el final
- **Graficos de red**: distintos simbolos en los extremos segun el tipo de relacion

### `orient="auto-start-reverse"` para flechas bidireccionales
- Permite usar **un solo marcador** para ambos extremos de una linea bidireccional
- El marcador de fin se coloca con la orientacion normal (alineado con la linea)
- El marcador de inicio se rota automaticamente 180° (apunta en la direccion opuesta)
- Elimina la necesidad de definir dos marcadores separados para el mismo diseno de flecha

---

## 15.3 Usos comunes

### Flechas en lineas y paths

**Punta de flecha basica**
- El marcador contiene un triangulo o path en forma de flecha
- El pico del triangulo es el punto de anclaje (`refX`)
- Con `orient="auto"`: la punta siempre sigue la direccion de la linea

**Consideraciones para flechas**
- El path de la linea llega hasta el punto final; el marcador se superpone
- Si la linea llega hasta el pico de la flecha, el trazo puede asomar por debajo del marcador
- Solucion: acortar el path de la linea para que termine antes de donde empieza el cuerpo de la flecha, o ajustar el `refX` para que la flecha "retroceda" ligeramente

**Flecha de doble sentido**
- Dos opciones:
  1. Dos marcadores distintos: uno para cada extremo
  2. Un marcador con `orient="auto-start-reverse"` aplicado en `marker-start` y `marker-end`

### Puntos en vertices de graficos

**Grafico de lineas con puntos**
- Definir un marcador con un circulo pequenio
- Aplicarlo en `marker-mid` de una `<polyline>` o path
- Los puntos aparecen automaticamente en cada vertice del grafico

**Puntos diferenciados por tipo de serie**
- Cada serie de datos usa un marcador diferente: circulo, cuadrado, triangulo, rombo
- Todos con el mismo `markerWidth`/`markerHeight` para consistencia visual

### Decoraciones de linea personalizadas

**Marcadores en puntos intermedios**
- `marker-mid` en un path complejo para colocar simbolos a lo largo de la linea
- Pero: los marcadores solo aparecen en los vertices explícitos del path, no a distancias uniformes
- Para marcadores a distancias uniformes: necesario usar JavaScript y `getPointAtLength()`

**Simbolos en diagramas**
- Marcadores con flechas de distintos tipos (llena, hueca, abierta) para diagramas UML, de flujo, de red
- Terminaciones de linea en diagramas de entidad-relacion: crow's foot notation

### Compatibilidad con `stroke-width`

Cuando se usa `markerUnits="strokeWidth"` (el valor por defecto), el tamano del marcador escala con el trazo:
- Util: el marcador siempre luce proporcional a la linea
- Precaucion: si el `stroke-width` cambia (por transformaciones de escala), el marcador cambia tambien
- Para marcadores de tamano fijo: usar `markerUnits="userSpaceOnUse"`

### Herencia de color en marcadores

**El problema del color**
- Los elementos dentro del marcador tienen sus propios atributos de fill y stroke
- Por defecto, no heredan el color de la linea a la que estan adjuntos

**Solucion: `context-stroke` y `context-fill` (SVG 2)**
- `fill="context-stroke"`: el relleno del marcador toma el color del stroke del elemento padre
- Permite que el marcador (ej: punta de flecha) tenga el mismo color que la linea automaticamente
- `fill="context-fill"`: toma el fill del elemento padre

**Alternativa para SVG 1.1: `currentColor`**
- Si los elementos internos del marcador usan `fill="currentColor"`, heredan la propiedad CSS `color` del elemento padre
- Requiere que el elemento padre tenga `color` definido en CSS
