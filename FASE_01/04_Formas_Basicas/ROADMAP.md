# Seccion 4: Formas Basicas

> SVG tiene 6 elementos de forma basica. Juntos pueden crear la mayoria de interfaces,
> iconos y diagramas. Dominarlos bien es la base de todo lo que viene despues.

---

## 4.1 Rectangulo `<rect>`

### Atributos de posicion y tamano

**`x` y `y` — Posicion de la esquina superior izquierda**
- `x`: posicion horizontal del borde izquierdo del rectangulo
- `y`: posicion vertical del borde superior del rectangulo
- Valor por defecto de ambos: `0` (si se omiten, el rectangulo empieza en el origen)
- Un rectangulo con `x="0" y="0"` esta pegado a la esquina superior izquierda del viewport/viewBox

**`width` y `height` — Dimensiones**
- `width`: ancho del rectangulo en unidades de usuario
- `height`: alto del rectangulo en unidades de usuario
- Ambos son **obligatorios**: sin ellos el rectangulo no se renderiza
- Valores negativos causan error; valor `0` produce un rectangulo invisible (pero valido)

### Esquinas redondeadas: `rx` y `ry`

**`rx` — Radio horizontal de las esquinas**
- Define el radio horizontal de la curva de cada esquina
- Valor `0` (por defecto): esquinas rectas
- Valor positivo: esquinas redondeadas

**`ry` — Radio vertical de las esquinas**
- Define el radio vertical de la curva de cada esquina
- Si se especifica solo `rx`: `ry` toma automaticamente el mismo valor que `rx`
- Si se especifica solo `ry`: `rx` toma automaticamente el mismo valor que `ry`
- Si se especifican ambos con valores distintos: las esquinas son curvas elipticas (no semicirculares)

**Limites de rx y ry**
- `rx` no puede ser mayor que `width/2` (el navegador lo recorta al maximo posible)
- `ry` no puede ser mayor que `height/2`
- Si `rx = width/2` y `ry = height/2`: el resultado es una elipse perfecta (no un rectangulo)

### Variantes comunes del rectangulo

**Cuadrado**
- Cuando `width = height`
- Puede tener esquinas redondeadas tambien

**Capsula (pill shape)**
- `rx` y `ry` iguales a la mitad del lado mas corto
- Si `height < width`: `rx = ry = height/2`
- El rectangulo parece un estadio o pastilla

**Cuadrado redondeado (rounded square)**
- `width = height` con `rx` y `ry` moderados (ej: `rx="10" ry="10"`)
- Patron muy comun en UI (botones, tarjetas, iconos)

---

## 4.2 Circulo `<circle>`

### Atributos

**`cx` y `cy` — Centro del circulo**
- `cx`: coordenada X del centro
- `cy`: coordenada Y del centro
- Valor por defecto de ambos: `0`
- Un circulo con `cx="0" cy="0"` tiene su centro en la esquina superior izquierda: solo se vera el cuadrante inferior derecho del circulo

**`r` — Radio**
- Radio del circulo en unidades de usuario
- **Obligatorio**: sin `r` el circulo no se renderiza
- El diametro del circulo es `2*r`
- El circulo se extiende desde `cx-r` hasta `cx+r` horizontalmente, y desde `cy-r` hasta `cy+r` verticalmente

### Casos de uso del circulo
- Puntos en graficos de datos
- Iconos redondos y avatares (con `clipPath` para recortar imagenes)
- Indicadores de estado (verde/rojo)
- Elementos de UI (radio buttons, bullets)
- Base para otros graficos (como sectores de torta usando `<path>` o arcos)

### Diferencia conceptual con `<ellipse>`
- `<circle>` es un caso especial de `<ellipse>` donde `rx = ry`
- Usar `<circle>` cuando el radio es uniforme: mas semantico y mas corto de escribir
- Usar `<ellipse>` cuando los radios horizontal y vertical difieren

---

## 4.3 Elipse `<ellipse>`

### Atributos

**`cx` y `cy` — Centro de la elipse**
- Misma logica que en `<circle>`
- Valor por defecto: `0`

**`rx` — Radio horizontal**
- La mitad del ancho de la elipse
- El punto mas a la derecha del eje horizontal esta en `cx + rx`
- El punto mas a la izquierda esta en `cx - rx`

**`ry` — Radio vertical**
- La mitad del alto de la elipse
- El punto mas abajo del eje vertical esta en `cy + ry`
- El punto mas arriba esta en `cy - ry`

### Relacion entre circle y ellipse
- `<circle r="50">` equivale exactamente a `<ellipse rx="50" ry="50">`
- Cuando `rx = ry`: resultado es un circulo perfecto
- Cuando `rx > ry`: elipse horizontal (achatada)
- Cuando `rx < ry`: elipse vertical (alargada)

### Casos de uso de la elipse
- Planetas y esferas en ilustraciones
- Sombras proyectadas (elipse achatada debajo de un objeto)
- Representar orbitas o trayectorias curvas
- Partes de ilustraciones organicas (ojos, cuerpos)
- Secciones transversales de cilindros (perspectiva isometrica)

---

## 4.4 Linea `<line>`

### Atributos

**`x1` y `y1` — Punto de inicio**
- `x1`: coordenada X donde empieza la linea
- `y1`: coordenada Y donde empieza la linea

**`x2` y `y2` — Punto final**
- `x2`: coordenada X donde termina la linea
- `y2`: coordenada Y donde termina la linea

### Caracteristica importante: la linea no tiene fill
- A diferencia de `<rect>`, `<circle>`, etc., `<line>` **no tiene area de relleno**
- Si no se especifica `stroke`, la linea es **invisible**
- El atributo `fill` existe en `<line>` pero no tiene efecto visual
- Siempre hay que definir: `stroke="algún-color"` y normalmente `stroke-width`

### Tipos de lineas segun sus coordenadas

**Linea horizontal**
- `y1` y `y2` son iguales: `<line x1="0" y1="50" x2="100" y2="50">`

**Linea vertical**
- `x1` y `x2` son iguales: `<line x1="50" y1="0" x2="50" y2="100">`

**Linea diagonal**
- Todos los valores distintos: `<line x1="0" y1="0" x2="100" y2="100">`

**Linea de longitud cero (punto)**
- `x1=x2` y `y1=y2`: produce un punto (si `stroke-linecap="round"`)

### Cuand usar `<line>` vs `<path>`
- `<line>` es mas semantico y legible para lineas simples entre dos puntos
- `<path>` es necesario para lineas con curvas, multiples segmentos, o trazados complejos
- Rendimiento similar entre ambos para una sola linea
- Preferir `<line>` por claridad cuando el caso de uso es una linea recta simple

---

## 4.5 Polilinia `<polyline>`

### Que es una polilinia
- Una serie de **segmentos de linea recta conectados** definida por una lista de puntos
- Es como dibujar a mano alzada: punto 1 → punto 2 → punto 3 → ... sin levantar el lapiz
- La forma **no se cierra** automaticamente: el ultimo punto no se conecta con el primero
- Es la diferencia clave con `<polygon>` (que si se cierra)

### El atributo `points`

**Sintaxis**
- Lista de coordenadas X e Y de cada punto
- Los pares se pueden separar por espacios, comas, o una combinacion
- Formatos equivalentes:
  - `points="0,0 50,25 100,0"` — pares separados por espacios, x,y con coma
  - `points="0 0 50 25 100 0"` — todos separados por espacios
  - `points="0,0,50,25,100,0"` — todos separados por comas
- El formato `"x,y x,y x,y"` es el mas legible y recomendado

**Minimo de puntos**
- Con 1 punto: nada visible (no hay segmento)
- Con 2 puntos: equivale a `<line>` (un solo segmento)
- Con 3 o mas: la polilinia tiene forma propia

### El problema del fill negro por defecto
- Como todas las formas SVG, `<polyline>` tiene `fill="black"` por defecto
- Pero una polilinia abierta con fill negro produce un resultado visual confuso (el fill se cierra "imaginariamente" entre el primer y ultimo punto)
- La practica estandar es siempre especificar `fill="none"` en polylines para que solo se vea el trazo

### Casos de uso de polyline
- Graficos de lineas (line charts) con datos simples
- Contornos de formas irregulares sin necesidad de curvas
- Representacion de rutas en mapas simples
- Bordes decorativos en zigzag o dientes de sierra

---

## 4.6 Poligono `<polygon>`

### Diferencia con polyline
- Misma logica y atributo `points` que `<polyline>`
- **Diferencia clave**: `<polygon>` **cierra automaticamente** la forma conectando el ultimo punto con el primero
- Esto lo hace el navegador: no hay que repetir el primer punto al final

### El atributo `points`
- Identica sintaxis que en `<polyline>`
- Con `<polygon>`, al menos 3 puntos son necesarios para crear una forma con area real

### Fill y el cierre automatico
- Como `<polygon>` es una forma cerrada, el fill negro por defecto SI tiene sentido visual
- Se puede usar `fill`, `stroke`, o ambos segun el diseño

### Formas geometricas regulares con polygon
La clave es calcular los puntos usando trigonometria (seno y coseno):

**Triangulo equilatero** (3 puntos a 120° entre si)
- Los puntos estan en un circulo, separados 120°

**Cuadrado** (4 puntos a 90° entre si)
- Los puntos estan en un circulo, separados 90°
- Mas sencillo usar `<rect>` para cuadrados alineados a los ejes

**Pentagono** (5 puntos a 72° entre si)
**Hexagono** (6 puntos a 60° entre si) — muy usado en patrones de mapa
**Estrella de 5 puntas** — requiere alternar entre radio exterior e interior

### Cuando usar polygon vs path
- `<polygon>` es mas semantico y legible para formas poligonales con vertices rectos
- `<path>` es necesario cuando los lados del poligono tienen curvas, o cuando la forma es muy compleja
- Para formas con muchos puntos (>10), `<path>` puede ser mas conveniente programaticamente

---

## 4.7 Atributos de presentacion comunes a todas las formas

### Los cuatro atributos basicos de presentacion

**`fill` — Color de relleno**
- Acepta: nombre de color, hex, rgb, hsl, `none`, `currentColor`, `url(#id)` (para gradientes/patrones)
- Valor por defecto: `black` (negro)
- `fill="none"`: sin relleno (el interior de la forma es transparente)
- Sin `fill`, las formas son negras; esto sorprende a los principiantes

**`stroke` — Color del contorno**
- Acepta los mismos valores que `fill`
- Valor por defecto: `none` (sin contorno)
- Sin `stroke`, las formas no tienen borde visible
- `<line>` y `<polyline>` solo son visibles si tienen `stroke`

**`stroke-width` — Grosor del contorno**
- Valor en unidades de usuario
- Valor por defecto: `1`
- El trazo se dibuja **centrado** sobre el borde geometrico de la forma: la mitad hacia afuera y la mitad hacia adentro
- Un `stroke-width="10"` en un rectangulo de 100x100: el rectangulo "visible" parece ser 105x105 (5px de trazo hacia afuera en cada lado)

**`opacity` — Opacidad general del elemento**
- Rango: `0` (invisible) a `1` (completamente opaco)
- Afecta al elemento completo: fill, stroke, texto, y elementos hijos (en grupos)
- Es diferente a `fill-opacity` y `stroke-opacity` que controlan fill y stroke por separado

### Atributos de presentacion vs propiedades CSS
- Estos atributos pueden escribirse directamente en el elemento SVG (`fill="red"`)
- O pueden especificarse via CSS (`rect { fill: red; }`)
- O con style inline (`style="fill: red;"`)
- La especificidad/prioridad: CSS > style inline > atributos de presentacion
- Detalles completos en la Fase 2, Seccion 7

### Herencia de atributos de presentacion
- Los atributos como `fill`, `stroke`, `font-size` se **heredan** de padres a hijos
- Un grupo `<g fill="red">` hace que todos sus hijos sean rojos por defecto
- Un hijo puede sobrescribir el valor heredado con su propio atributo
- Esto permite definir estilos en el nivel mas alto y sobreescribirlos selectivamente

### Tabla resumen de formas y sus atributos obligatorios

| Forma | Atributos obligatorios | Atributos opcionales importantes |
|-------|------------------------|----------------------------------|
| `<rect>` | `width`, `height` | `x`, `y`, `rx`, `ry` |
| `<circle>` | `r` | `cx`, `cy` |
| `<ellipse>` | `rx`, `ry` | `cx`, `cy` |
| `<line>` | `x1`, `y1`, `x2`, `y2` | `stroke` (necesario para verla) |
| `<polyline>` | `points` | `fill="none"` recomendado |
| `<polygon>` | `points` | — |

### Visibilidad y el fill por defecto: el error mas comun

El error mas frecuente de los principiantes:
1. Crear una `<line>` y no verla → falta `stroke`
2. Crear una `<polyline>` y ver un area negra extraña → falta `fill="none"`
3. Crear un `<rect>` y no ver su borde → falta `stroke`
4. Todos los elementos son negros y parecen pegados → estan en (0,0) sin x/y explicitos

Regla practica para empezar:
- Siempre especificar `fill` y `stroke` explicitamente
- Siempre especificar `x`, `y` (o `cx`, `cy`) explicitamente
- No asumir valores por defecto hasta conocerlos bien
