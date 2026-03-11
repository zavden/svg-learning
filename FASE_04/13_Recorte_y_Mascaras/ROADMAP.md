# Seccion 13: Recorte y Mascaras

> Tanto `<clipPath>` como `<mask>` ocultan partes de un elemento.
> La diferencia fundamental: clipPath es binario (dentro o fuera),
> mask es gradual (puede haber semitransparencia).

---

## 13.1 Recorte con `<clipPath>`

### Que hace un clipPath
- Define una **region de recorte**: solo la parte del elemento que cae dentro de esa region es visible
- Todo lo que esta fuera de la region de recorte se oculta completamente
- El recorte es **binario**: un pixel esta completamente visible o completamente oculto, sin gradaciones
- Es como usar unas tijeras para recortar una forma de un papel

### Como se define y aplica

**Definicion (en `<defs>`)**
```
<clipPath id="miRecorte">
  <circle cx="50" cy="50" r="40" />
</clipPath>
```

**Aplicacion (en el elemento a recortar)**
```
<image clip-path="url(#miRecorte)" ... />
<rect clip-path="url(#miRecorte)" ... />
<g clip-path="url(#miRecorte)"> ... </g>
```

### La geometria del clipPath, no el estilo

Una caracteristica clave: en el clipPath, **solo importa la geometria** de las formas, no su fill ni stroke:
- Una forma roja o azul dentro del clipPath produce exactamente el mismo resultado de recorte
- `fill="none"` en una forma dentro del clipPath: la forma **sigue recortando** (su area geometrica define la region visible)
- El stroke de las formas dentro del clipPath tambien contribuye a la region de recorte (tanto el area del fill como del stroke son "dentro")
- Los valores de `opacity` dentro del clipPath son ignorados; la region es binaria

### Que puede ir dentro de `<clipPath>`
- Formas basicas: `<rect>`, `<circle>`, `<ellipse>`, `<line>`, `<polyline>`, `<polygon>`
- Trazados: `<path>`
- Texto: `<text>` (el area de los caracteres define la region de recorte)
- Grupos: `<g>` con multiples formas (la union de todas las formas es la region)
- Elementos `<use>` que referencian formas

### Region de recorte como union de formas
- Si el clipPath contiene multiples formas, la region visible es la **union** de todas ellas
- No hay "resta" ni "interseccion" directa con multiples formas en un solo clipPath
- Para formas con agujeros: usar un `<path>` con sub-trazados y `fill-rule="evenodd"`

---

## 13.2 clipPathUnits

### Que controla
- El sistema de coordenadas en el que estan definidas las formas dentro del `<clipPath>`

### `userSpaceOnUse` (valor por defecto)

**Como funciona**
- Las formas del clipPath usan las **coordenadas absolutas del espacio del usuario** (el viewBox del SVG)
- Una forma en el clipPath con `cx="50" cy="50"` recorta en el punto (50, 50) del SVG

**Cuando usarlo**
- Cuando la region de recorte tiene una posicion fija en el SVG, independiente del elemento recortado
- Cuando se reutiliza el mismo clipPath para elementos en diferentes posiciones (el recorte se ve diferente en cada uno)
- Para efectos de "ventana" donde la region visible es fija en el espacio del SVG

**Limitacion**
- Si el elemento se mueve, el clipPath no se mueve con el
- Hay que actualizar las coordenadas del clipPath o usar transformaciones

### `objectBoundingBox`

**Como funciona**
- Las coordenadas del clipPath son **relativas al bounding box del elemento recortado**
- Rango: 0 a 1 (o 0% a 100%)
- `(0, 0)` = esquina superior izquierda del elemento; `(1, 1)` = esquina inferior derecha

**Cuando usarlo**
- Cuando el recorte debe **seguir al elemento** si este se mueve o redimensiona
- Para recortes proporcionales: "mostrar solo la mitad izquierda del elemento" sin importar su tamano

**Ejemplo tipico**
- `<clipPath clipPathUnits="objectBoundingBox"><rect width="0.5" height="1" /></clipPath>`
- Recorta cualquier elemento dejando visible solo la mitad izquierda

---

## 13.3 Mascaras con `<mask>`

### Que hace una mascara
- Define la **visibilidad de cada pixel** del elemento usando la luminancia o el canal alfa del contenido de la mascara
- A diferencia del clipPath, la mascara permite **transiciones suaves** (semitransparencia)
- Es como colocar un papel translucido sobre el elemento: la parte transparente del papel oculta, la opaca muestra

### La regla de luminancia
- **Blanco** en la mascara → el pixel del elemento es **completamente visible**
- **Negro** en la mascara → el pixel del elemento es **completamente oculto**
- **Gris** en la mascara → el pixel del elemento es **parcialmente visible** (proporcional a la luminancia)
- Un gradiente de blanco a negro en la mascara produce un desvanecimiento suave del elemento

### Como se define y aplica

**Definicion (en `<defs>`)**
```
<mask id="miMascara">
  <rect width="100%" height="100%" fill="white" />  <!-- todo visible por defecto -->
  <circle cx="50" cy="50" r="30" fill="black" />    <!-- agujero circular -->
</mask>
```

**Aplicacion**
```
<image mask="url(#miMascara)" ... />
<g mask="url(#miMascara)"> ... </g>
```

### El fondo de la mascara es negro por defecto
- Lo que esta **fuera** del contenido de la mascara es negro (invisible)
- Para que el elemento sea visible por defecto y solo se oculte en zonas especificas:
  - Agregar un rectangulo blanco que cubra toda el area: `<rect width="100%" height="100%" fill="white" />`
  - Luego agregar formas negras para las zonas ocultas

### Mascaras con gradientes

**Caso muy comun: desvanecimiento gradual**
- Definir un `<linearGradient>` de blanco a negro
- Usar ese gradiente como `fill` de un rectangulo dentro de la mascara
- El elemento se desvanece suavemente de visible a invisible

**Aplicaciones**
- Texto que se desvanece al final (efecto de fade-out)
- Imagenes con bordes difuminados
- Transiciones entre secciones de una ilustracion

---

## 13.4 Atributos de `<mask>`

### `x`, `y`, `width`, `height` — Area de la mascara

**Que definen**
- Las dimensiones del area donde la mascara tiene efecto
- Fuera de este area: el elemento es invisible (equivale a negro en la mascara)

**Valores por defecto**
- `x="-10%"` `y="-10%"` `width="120%"` `height="120%"`
- El area por defecto es 10% mas grande que el bounding box del elemento en cada direccion
- Esto previene que los bordes del elemento queden recortados por la mascara

**Cuando ajustar estos valores**
- Si la mascara tiene efectos que se extienden mas alla del elemento (como un blur en la mascara)
- Para mascaras que aplican a toda la pantalla o a areas muy grandes

### `maskUnits`

- Controla el sistema de coordenadas de los atributos `x`, `y`, `width`, `height` de la **mascara en si**
- `objectBoundingBox` (por defecto): relativo al bounding box del elemento enmascarado
- `userSpaceOnUse`: coordenadas absolutas del espacio del usuario

### `maskContentUnits`

- Controla el sistema de coordenadas del **contenido** de la mascara (las formas dentro)
- `userSpaceOnUse` (por defecto): las formas usan coordenadas absolutas
- `objectBoundingBox`: las formas usan coordenadas relativas al bounding box del elemento

---

## 13.5 Diferencias entre clipPath y mask

### Tabla comparativa

| Aspecto | `<clipPath>` | `<mask>` |
|---------|-------------|---------|
| Tipo de recorte | Binario (todo o nada) | Gradual (semitransparencia posible) |
| Basado en | Geometria de las formas | Luminancia o alfa del contenido |
| Permite gradientes | No (solo formas) | Si (los gradientes afectan la visibilidad) |
| Rendimiento | Mejor (mas simple) | Ligeramente mas costoso |
| Nitidez de bordes | Siempre nítido | Puede ser suave (con blur en la mascara) |
| Casos tipicos | Recortar imagen en forma, iconos | Desvanecimientos, efectos de luz, overlays |

### Cuando usar clipPath

- El borde de la region visible debe ser nítido y definido
- Se quiere recortar una imagen en una forma geometrica (circulo, estrella, letra)
- El rendimiento importa (muchos elementos con recorte)
- La region de recorte no necesita gradaciones

### Cuando usar mask

- Se necesita un desvanecimiento o transicion suave en los bordes
- La region visible depende de gradientes (fade-out, viñeta)
- Se quiere crear efectos de iluminacion o brillo con transparencia
- El contenido dentro de la mascara tiene semitransparencia que debe respetarse

---

## 13.6 Recorte con CSS

### La propiedad CSS `clip-path`

Permite aplicar recorte desde CSS sin necesidad de definir un `<clipPath>` en el SVG:

**Funciones de forma basica**
- `clip-path: circle(50%)` — recorta en circulo del 50% del tamano del elemento
- `clip-path: ellipse(50% 30% at 50% 50%)` — elipse centrada
- `clip-path: inset(10px)` — rectangulo con margen de 10px en cada lado
- `clip-path: inset(10px round 5px)` — rectangulo con esquinas redondeadas
- `clip-path: polygon(0 0, 100% 0, 50% 100%)` — triangulo

**`path()` — Forma arbitraria**
- `clip-path: path('M 0 0 L 100 0 L 50 100 Z')` — cualquier forma con sintaxis de path SVG
- Soporte creciente en navegadores modernos

**`url()` — Referencia a `<clipPath>` SVG**
- `clip-path: url(#miRecorte)` — usa un `<clipPath>` definido en el SVG o en el HTML
- Permite usar la potencia completa de `<clipPath>` desde CSS

### Ventajas del recorte CSS
- No requiere modificar el SVG para agregar el recorte
- Se puede animar con CSS transitions: `transition: clip-path 0.3s ease`
- Funciona en elementos HTML tambien, no solo SVG
- Mas facil de cambiar dinamicamente con JavaScript

### Animacion de clip-path con CSS
- `clip-path` es animable con CSS transitions y `@keyframes`
- Para animar entre formas: ambas deben tener el mismo numero de puntos (para polygon)
- Permite efectos de "revelar" contenido, transiciones entre pantallas, etc.
