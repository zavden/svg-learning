# Seccion 14: Filtros SVG

> Los filtros SVG son el sistema de efectos visuales mas potente del formato.
> Operan sobre bitmaps (pixels), no sobre vectores. Cada primitiva es un paso
> en una cadena de procesamiento de imagen.

---

## 14.1 El elemento `<filter>`

### Naturaleza de los filtros SVG
- Los filtros SVG procesan el elemento como una **imagen rasterizada** (bitmap)
- El elemento vectorial se renderiza primero a pixels, luego los filtros manipulan esos pixels
- El resultado puede tener aspecto muy diferente al elemento original
- Los filtros pueden ser costosos computacionalmente, especialmente en animacion

### Definicion y aplicacion

**Definicion (siempre en `<defs>`)**
```
<filter id="miFiltro">
  <!-- primitivas de filtro aqui -->
</filter>
```

**Aplicacion**
- Como atributo: `filter="url(#miFiltro)"` en el elemento
- Como propiedad CSS: `filter: url(#miFiltro);`
- Aplica a cualquier elemento o grupo

### Atributos de region del filtro

**`x`, `y`, `width`, `height`**
- Definen el **area de procesamiento** del filtro
- Por defecto: `x="-10%"` `y="-10%"` `width="120%"` `height="120%"`
- El area es 10% mas grande que el bounding box del elemento en cada direccion
- Por que el 10% extra: efectos como blur o sombra se extienden fuera del elemento; sin el extra, quedan cortados
- Para efectos que se extienden mucho (blur grande, sombra larga): aumentar este area

**`filterUnits`**
- `objectBoundingBox` (por defecto): x/y/width/height son relativos al bounding box del elemento
- `userSpaceOnUse`: coordenadas absolutas del espacio del usuario

**`primitiveUnits`**
- Sistema de coordenadas para las primitivas dentro del filtro
- `userSpaceOnUse` (por defecto): coordenadas del espacio del usuario
- `objectBoundingBox`: relativas al bounding box

### El modelo de procesamiento por capas
- Cada primitiva de filtro **recibe una imagen de entrada** y produce **una imagen de salida**
- Las primitivas se encadenan: la salida de una es la entrada de la siguiente
- Al final, la ultima imagen producida reemplaza el elemento original en el renderizado

---

## 14.2 Primitivas de filtro basicas

### 14.2.1 `<feGaussianBlur>` — Desenfoque gaussiano

**Que hace**
- Aplica un desenfoque suave (blur) al contenido
- El algoritmo gaussiano produce el desenfoque mas natural (identico al de los editores graficos)

**Atributos clave**
- `in`: la fuente de entrada (por defecto: `SourceGraphic`)
- `stdDeviation`: cantidad de desenfoque
  - Un solo valor: mismo blur en X e Y
  - Dos valores separados por espacio: `stdDeviation="5 2"` = blur horizontal 5, vertical 2
  - Valor mas alto = mas borroso; `stdDeviation="0"` = sin blur
- `result`: nombre que identifica la salida para usarla en otra primitiva

**Impacto en rendimiento**
- Es la primitiva mas costosa computacionalmente
- Un `stdDeviation` grande en un elemento grande = muy costoso
- Nunca animar `stdDeviation` directamente en elementos grandes (mejor animar `opacity`)

### 14.2.2 `<feOffset>` — Desplazamiento

**Que hace**
- Desplaza la imagen de entrada sin modificarla
- No cambia los pixels, solo los mueve

**Atributos clave**
- `in`: la fuente de entrada
- `dx`: desplazamiento horizontal (positivo = derecha)
- `dy`: desplazamiento vertical (positivo = abajo)
- `result`: nombre de la salida

**Uso tipico**
- Es el paso que da la "direccion" a una sombra proyectada
- `feGaussianBlur` crea el blur; `feOffset` lo desplaza para simular que la sombra esta debajo y desplazada

### 14.2.3 `<feFlood>` — Relleno de color

**Que hace**
- Crea una imagen solida de un unico color que llena el area del filtro
- No depende del elemento original; genera una nueva imagen de color plano

**Atributos clave**
- `flood-color`: el color de relleno (acepta cualquier valor CSS de color)
- `flood-opacity`: opacidad del relleno (0 a 1)
- `result`: nombre de la salida

**Uso tipico**
- Proporcionar el "color" de una sombra en combinacion con otras primitivas
- Como base para mezclas de color

### 14.2.4 `<feComposite>` — Composicion de imagenes

**Que hace**
- Combina dos imagenes segun una operacion de composicion
- Las operaciones de Porter-Duff definen como se mezclan los pixels

**Atributos clave**
- `in`: primera imagen de entrada
- `in2`: segunda imagen de entrada
- `operator`: la operacion a aplicar:
  - `over` (por defecto): la imagen `in` encima de `in2`
  - `in`: solo la parte de `in` que se superpone con `in2`
  - `out`: solo la parte de `in` que NO se superpone con `in2`
  - `atop`: `in` encima de `in2`, solo donde `in2` es opaco
  - `xor`: areas no superpuestas de ambas imagenes
  - `arithmetic`: formula matematica con parametros k1, k2, k3, k4
- `k1`, `k2`, `k3`, `k4`: parametros para el operador `arithmetic`

**Uso tipico**
- Limitar el color de la sombra al area de la silueta del elemento
- Combinar multiples efectos en la composicion final

### 14.2.5 `<feMerge>` y `<feMergeNode>` — Apilado de capas

**Que hace**
- Apila multiples imagenes en orden, una sobre la otra
- Equivalente a poner varias capas en un editor grafico

**Estructura**
```
<feMerge>
  <feMergeNode in="sombra" />      <!-- capa inferior -->
  <feMergeNode in="SourceGraphic" /> <!-- capa superior -->
</feMerge>
```

**Uso tipico**
- Combinar la sombra desenfocada con el elemento original encima
- Apilar multiples efectos que deben verse en orden especifico

### 14.2.6 `<feColorMatrix>` — Transformacion de colores

**Que hace**
- Transforma los colores del elemento aplicando una matriz de transformacion de color
- Puede cambiar saturacion, rotar el tono, convertir a escala de grises, etc.

**Atributos clave**
- `in`: fuente de entrada
- `type`: el tipo de transformacion:
  - `matrix`: matriz 4x5 completa (acceso a todos los canales RGBA)
  - `saturate`: ajuste de saturacion con un solo valor (0 = grises, 1 = normal, >1 = hipersaturado)
  - `hueRotate`: rotacion del tono en grados
  - `luminanceToAlpha`: convierte la luminancia del pixel en su canal alfa (base para efectos de luz)
- `values`: la matriz (20 valores para `matrix`) o el valor unico para los otros tipos

**Casos de uso comunes**
- `type="saturate" values="0"`: convertir a escala de grises
- `type="hueRotate" values="180"`: invertir el tono
- `type="luminanceToAlpha"`: crear una silueta basada en el brillo (para usar en mascaras)

### 14.2.7 `<feDropShadow>` — Sombra proyectada (atajo)

**Que hace**
- Genera una sombra proyectada en un solo paso
- Equivalente a: `feGaussianBlur` + `feOffset` + `feFlood` + `feComposite` + `feMerge`, pero en una sola primitiva

**Atributos clave**
- `dx`, `dy`: desplazamiento de la sombra
- `stdDeviation`: cantidad de blur de la sombra
- `flood-color`: color de la sombra
- `flood-opacity`: opacidad de la sombra

**Disponibilidad**
- Definido en SVG 2
- Bien soportado en navegadores modernos
- Para compatibilidad con entornos mas antiguos: usar la combinacion manual de primitivas

---

## 14.3 Entradas de filtro predefinidas

Las primitivas reciben datos de entrada a traves del atributo `in`. Hay valores predefinidos que no requieren definicion:

| Valor | Descripcion |
|-------|-------------|
| `SourceGraphic` | El elemento original renderizado (con fill, stroke, etc.) |
| `SourceAlpha` | Solo el canal alfa del elemento (silueta, sin color) |
| `BackgroundImage` | La imagen del fondo detras del elemento (soporte limitado) |
| `BackgroundAlpha` | El canal alfa del fondo |
| `FillPaint` | El color de fill del elemento (imagen solida del color de relleno) |
| `StrokePaint` | El color de stroke del elemento |

**El mas comun: `SourceGraphic`**
- Es la entrada por defecto cuando no se especifica `in`
- Representa el elemento completo tal como se veria sin filtro

**`SourceAlpha` para efectos de silueta**
- Produce una imagen negra con la forma del elemento
- Ideal para crear sombras que no revelan los colores internos del elemento
- La sombra resultante es siempre negra (o del color especificado en `feFlood`)

---

## 14.4 Encadenamiento de filtros

### El sistema de nombres y referencias

**`result` — nombrar la salida de una primitiva**
- `result="miBlur"`: la salida de esta primitiva se puede referenciar con el nombre "miBlur"
- El nombre es local al `<filter>`, no necesita ser un id global

**`in` — referenciar la salida de otra primitiva**
- `in="miBlur"`: esta primitiva recibe la salida de la primitiva que definio `result="miBlur"`
- Si no se especifica `in`: la primitiva toma la salida de la primitiva anterior en el codigo fuente

### Orden de ejecucion
- Las primitivas se ejecutan en el **orden en que aparecen en el codigo fuente**
- Si una primitiva referencia la salida de una posterior (hacia adelante), el comportamiento es indefinido
- Siempre encadenar de arriba hacia abajo

### Patron de cadena lineal
```
<feGaussianBlur result="blur" />         <!-- paso 1: blur a SourceGraphic -->
<feOffset in="blur" result="desplazado" dx="5" dy="5" />  <!-- paso 2: desplazar -->
<feMerge>                                 <!-- paso 3: combinar -->
  <feMergeNode in="desplazado" />
  <feMergeNode in="SourceGraphic" />
</feMerge>
```

### Patron de cadena con bifurcacion
- Una primitiva puede ser referenciada por multiples primitivas posteriores
- `result="silueta"` puede ser usado en dos ramas distintas que luego se combinan con `feMerge`
- Esto permite efectos compuestos sin repetir calculos

---

## 14.5 Creacion de sombras proyectadas (drop shadow)

### Metodo rapido: `<feDropShadow>`
- Un solo elemento, parametros directos
- `<feDropShadow dx="3" dy="3" stdDeviation="2" flood-color="#000" flood-opacity="0.5" />`
- Disponible en SVG 2 y en navegadores modernos

### Metodo manual: cadena de primitivas
Util para entender el mecanismo y para mayor control:

1. **`feGaussianBlur in="SourceAlpha"`** — desenfocar la silueta del elemento
2. **`feOffset dx="..." dy="..."`** — desplazar la sombra
3. **`feFlood flood-color="..."` + `feComposite operator="in"`** — colorear la sombra
4. **`feMerge`** — apilar la sombra debajo del elemento original

### Alternativa CSS: `filter: drop-shadow()`
- `filter: drop-shadow(3px 3px 4px rgba(0,0,0,0.5));`
- No requiere ninguna definicion en el SVG
- Sintaxis identica a `box-shadow` pero sigue el contorno real del elemento (no el bounding box)
- Menos control que la version SVG, pero mucho mas sencillo

### Diferencia entre `drop-shadow` CSS y `box-shadow` CSS
- `box-shadow`: sombra del rectangulo del bounding box (ignora transparencias y formas)
- `drop-shadow()` CSS: sombra del contorno real del elemento (ideal para SVG y PNGs con transparencia)

---

## 14.6 Otras primitivas de filtro (nivel intermedio)

### `<feBlend>` — Mezcla de modos
- Mezcla dos imagenes usando modos de mezcla similares a los de editores graficos
- `mode`: `normal`, `multiply`, `screen`, `overlay`, `darken`, `lighten`, `color-dodge`, `color-burn`, `hard-light`, `soft-light`, `difference`, `exclusion`
- Identicos a los modos de `mix-blend-mode` en CSS
- Util para efectos de superposicion con modo de mezcla personalizado

### `<feMorphology>` — Erosion y dilatacion
- Modifica la forma del elemento expandiendola o contrayendola
- `operator="erode"`: contrae la forma (hace el elemento mas delgado)
- `operator="dilate"`: expande la forma (hace el elemento mas grueso)
- `radius`: cuanto expandir o contraer (en pixels del filtro)
- **Uso tipico**: crear un contorno exterior de texto sin modificar el path del texto; "engrosar" lineas

### `<feTurbulence>` — Ruido y turbulencia
- Genera texturas de ruido procesal (ruido de Perlin o turbulencia fractal)
- No depende de ninguna imagen de entrada: genera sus propios datos
- Atributos clave:
  - `type`: `fractalNoise` (ruido suave) o `turbulence` (ruido mas brusco)
  - `baseFrequency`: frecuencia base del ruido (valores tipicos: 0.01 - 0.5)
  - `numOctaves`: capas de detalle (mas octavas = mas detalle pero mas costoso)
  - `seed`: semilla aleatoria (mismo seed = mismo patron)
- **Usos tipicos**: texturas de agua, fuego, nube, papel, madera; efecto de distorsion

### `<feDisplacementMap>` — Mapa de desplazamiento
- Desplaza los pixels de una imagen usando como "mapa" otra imagen
- El color de cada pixel del mapa determina cuanto y en que direccion se desplaza el pixel correspondiente
- Atributos: `in`, `in2` (el mapa), `scale` (intensidad del desplazamiento), `xChannelSelector`, `yChannelSelector`
- **Uso tipico**: combinar con `feTurbulence` para crear distorsiones de agua, calor, cristal esmerilado

### `<feConvolveMatrix>` — Convolucion
- Aplica un kernel de convolucion (matriz de pesos) sobre la imagen
- Permite implementar: nitidez (sharpen), deteccion de bordes, emboss, etc.
- Requiere definir la matriz de kernel: complejo pero muy potente
- **Uso tipico**: efectos de nitidez, deteccion de bordes para efectos artisticos

### `<feComponentTransfer>` — Ajuste de canales individuales
- Ajusta cada canal de color (R, G, B, A) de forma independiente
- Hijos: `<feFuncR>`, `<feFuncG>`, `<feFuncB>`, `<feFuncA>`
- Tipos de funcion para cada canal: `identity`, `linear`, `gamma`, `table`, `discrete`
- **Usos tipicos**: curvas de brillo/contraste personalizadas, efectos de posterizacion, ajuste de gamma

### `<feDiffuseLighting>` y `<feSpecularLighting>` — Iluminacion 3D
- Simulan iluminacion 3D usando el canal alfa del elemento como mapa de relieve (bump map)
- `feDiffuseLighting`: luz difusa (iluminacion uniforme)
- `feSpecularLighting`: luz especular (brillos y reflejos)
- Fuentes de luz: `<fePointLight>`, `<feDistantLight>`, `<feSpotLight>`
- **Uso tipico**: dar aspecto tridimensional a formas planas

### `<feImage>` — Imagen externa como fuente
- Usa una imagen externa (URL o data URI) como fuente de entrada para el filtro
- Permite usar fotografias o texturas externas dentro del pipeline de filtros
- `href`: la URL de la imagen

### `<feTile>` — Repeticion como mosaico
- Repite su imagen de entrada para cubrir toda el area del filtro
- Util para crear texturas repetidas dentro de un filtro
- Funciona bien combinado con `<feImage>` o `<feTurbulence>`
