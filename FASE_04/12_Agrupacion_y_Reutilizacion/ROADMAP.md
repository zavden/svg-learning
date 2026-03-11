# Seccion 12: Agrupacion y Reutilizacion

> SVG ofrece tres mecanismos principales para organizar y reutilizar contenido:
> `<g>` para agrupar, `<symbol>` para definir componentes, y `<use>` para instanciarlos.
> Usarlos correctamente reduce la repeticion y hace el SVG mas mantenible.

---

## 12.1 El elemento `<g>` (grupo)

### Que es y para que sirve
- `<g>` agrupa elementos SVG como una **unidad logica y visual**
- Es el equivalente SVG de un `<div>` en HTML: un contenedor sin representacion visual propia
- No tiene atributos de posicion ni tamano propios (`x`, `y`, `width`, `height` no aplican)
- Su valor esta en lo que **hereda y propaga** a sus hijos

### Herencia de atributos de presentacion

Cualquier atributo de presentacion en el `<g>` se hereda por todos sus hijos:
- `fill`, `stroke`, `stroke-width`, `opacity`, `font-family`, `font-size`, `color`, etc.
- Un hijo puede **sobrescribir** el valor heredado con su propio atributo
- Si un hijo tiene su propio `fill`, ese valor tiene prioridad sobre el del grupo

**Atributos que SI se heredan**
- `fill`, `fill-opacity`, `fill-rule`
- `stroke`, `stroke-width`, `stroke-opacity`, `stroke-linecap`, `stroke-linejoin`, `stroke-dasharray`
- `font-family`, `font-size`, `font-weight`, `font-style`
- `color`, `visibility`, `cursor`, `pointer-events`

**Atributos que NO se heredan** (se aplican solo al grupo como unidad)
- `opacity` (se aplica al grupo completo renderizado, no a cada hijo individualmente)
- `clip-path`, `mask`, `filter` (afectan al resultado renderizado del grupo, no a los hijos por separado)
- `transform` (se hereda en el sentido de que el sistema de coordenadas se propaga, pero no es "herencia" CSS)

### Transformaciones en grupos
- `<g transform="translate(50, 50)">`: desplaza todo el grupo
- Todos los hijos se mueven junto con el grupo
- Permite posicionar conjuntos de elementos sin modificar sus coordenadas internas
- Composicion de transformaciones: si un hijo tiene su propio `transform`, se compone con el del grupo

### Filtros, clipPath y mask en grupos
- Aplicar `filter`, `clip-path` o `mask` a un grupo afecta a **todos los hijos como una unidad**
- El grupo se renderiza primero a un buffer, luego se le aplica el efecto
- Diferente a aplicar el efecto a cada hijo individualmente:
  - Un filtro de blur en el grupo: toda la imagen del grupo se desenfoca junta
  - Un filtro de blur en cada hijo: cada elemento se desenfoca individualmente

### `<g>` como herramienta de semantica
- Dar un `id` o clase al grupo para identificar secciones: `<g id="cabeza">`, `<g class="zona-interactiva">`
- Comentarios XML dentro o antes del grupo para documentar la estructura
- Grupos anidados para jerarquias logicas: `<g id="personaje"><g id="torso">...</g><g id="cabeza">...</g></g>`

---

## 12.2 El elemento `<use>`

### Que hace
- Crea una **instancia (copia visual)** de cualquier elemento definido en el documento
- El elemento original puede estar en cualquier parte del SVG (en `<defs>` o no)
- Multiples `<use>` pueden referenciar el mismo elemento: solo se define una vez, se usa muchas veces
- Los cambios en el original se reflejan automaticamente en todas las instancias

### Atributos

**`href` (SVG 2) / `xlink:href` (SVG 1.1)**
- Referencia al `id` del elemento a reutilizar
- Sintaxis: `href="#idDelElemento"` o `xlink:href="#idDelElemento"`
- Para maxima compatibilidad: incluir ambos
- El elemento referenciado puede ser cualquier elemento SVG con `id`

**`x` y `y`**
- Desplazan la instancia respecto a la posicion original del elemento referenciado
- Equivalente a aplicar `transform="translate(x, y)"` a la instancia
- `x="0" y="0"` (por defecto): la instancia aparece exactamente donde esta el original

**`width` y `height`**
- Solo tienen efecto cuando el elemento referenciado es un `<svg>` o un `<symbol>`
- Para otros elementos, estos atributos no afectan al renderizado

### El shadow DOM de `<use>`

Cuando se usa `<use>`, el navegador crea un **shadow DOM**: una copia oculta del elemento referenciado.

**Implicaciones importantes:**
- Los elementos dentro de la instancia `<use>` **no son accesibles directamente** desde CSS externo
- Los selectores como `.mi-clase circle` no funcionan dentro de una instancia `<use>`
- Los estilos heredados del `<use>` si se propagan: `fill`, `stroke`, etc. del `<use>` llegan a los hijos

### Estilizar instancias `<use>`: el problema y las soluciones

**El problema**
- No se puede apuntar con CSS a elementos dentro de `<use>`: `use circle { fill: red; }` no funciona

**Solucion 1: `currentColor`**
- Los elementos internos del `<symbol>` o `<g>` usan `fill="currentColor"`
- El color se establece via la propiedad CSS `color` en el elemento `<use>`: `use { color: red; }`
- Limitado a un color por instancia

**Solucion 2: Variables CSS (Custom Properties)**
- Los elementos internos usan `fill: var(--icon-color, black);`
- Cada instancia `<use>` puede tener diferentes valores de la variable: `style="--icon-color: red;"`
- Permite multiples colores independientes por instancia

**Solucion 3: Atributos directos en `<use>`**
- `fill` y `stroke` en el `<use>` se heredan a los hijos que no tienen valor propio
- Si los elementos del `<symbol>` no tienen `fill` explicito, heredan el del `<use>`
- Simple pero solo funciona si los elementos internos no tienen fill fijado

---

## 12.3 El elemento `<symbol>`

### Diferencia con `<g>`
- `<symbol>` no se renderiza directamente, nunca aparece en el SVG sin ser referenciado
- Puede tener su propio `viewBox` y `preserveAspectRatio`, al igual que un `<svg>`
- Esta diferencia de `viewBox` es la razon principal para preferir `<symbol>` sobre `<g>` para iconos

### Ventaja del `viewBox` propio

**Con `<g>` sin viewBox**
- El icono se posiciona en el espacio de coordenadas del SVG principal
- El tamano del icono depende de las coordenadas del SVG principal
- Para un icono de 24x24 en un SVG de 500x500: el icono ocupa poco espacio

**Con `<symbol>` con viewBox**
- El icono tiene su propio sistema de coordenadas interno
- Al hacer `<use width="50" height="50" href="#miIcono">`: el icono ocupa exactamente 50x50 unidades
- El `viewBox` del `<symbol>` mapea el contenido al tamano `width`/`height` del `<use>`
- Esto permite definir el icono en cualquier sistema de coordenadas (ej: 0 0 24 24) y usarlo en cualquier tamano

### Donde definir `<symbol>`
- Convencionalmente dentro de `<defs>`, aunque puede estar fuera
- Si esta fuera de `<defs>`: sigue sin renderizarse directamente (es la naturaleza de `<symbol>`)
- En la practica: siempre dentro de `<defs>` por claridad

### `<symbol>` como sistema de iconos
- Patron estandar: un SVG con multiples `<symbol>`, uno por icono
- Cada `<symbol>` tiene: `id` unico, `viewBox="0 0 24 24"` (o el tamano estandar), `aria-hidden="true"` o `aria-label`
- En el HTML: `<use href="#nombre-icono">` con el tamano controlado por CSS

### `preserveAspectRatio` en `<symbol>`
- Funciona igual que en `<svg>`
- Controla como el contenido del `<symbol>` se ajusta dentro del `width`/`height` del `<use>`
- Valor por defecto: `xMidYMid meet`
- Para iconos que deben llenar exactamente el espacio sin preservar aspecto: `none`

---

## 12.4 El elemento `<defs>`

### Proposito y naturaleza
- **Contenedor** para cualquier elemento que se define pero no se renderiza directamente
- Conviene pensar en `<defs>` como la "biblioteca" del SVG
- Todo lo que este dentro es invisible hasta que algo lo referencia
- Puede contener cualquier tipo de elemento SVG

### Que elementos van tipicamente en `<defs>`

| Elemento | Funcion |
|----------|---------|
| `<linearGradient>` | Gradientes lineales |
| `<radialGradient>` | Gradientes radiales |
| `<pattern>` | Patrones de relleno |
| `<filter>` | Filtros de efectos visuales |
| `<clipPath>` | Mascaras de recorte |
| `<mask>` | Mascaras de transparencia |
| `<symbol>` | Componentes graficos reutilizables |
| `<marker>` | Decoraciones de inicio/fin de lineas |
| `<g>` | Grupos referenciados por `<use>` |
| `<path>` | Paths usados por `<textPath>` |

### Posicion en el documento
- Por convencion y legibilidad: lo primero dentro del `<svg>`, antes del contenido grafico
- El navegador no requiere este orden: puede referenciar hacia adelante (un elemento definido despues del que lo usa)
- Las herramientas y linters suelen recomendar mantener `<defs>` al inicio

### Un SVG puede tener multiples `<defs>`
- Es valido tener mas de un bloque `<defs>` en el documento
- No es recomendado: dificulta encontrar las definiciones
- Algunos editores graficos generan multiples `<defs>` al combinar SVGs

---

## 12.5 Herencia de atributos de presentacion

### Las reglas de herencia en SVG

SVG sigue un modelo de herencia similar al de CSS:
- Los elementos hijos **heredan** ciertos atributos de sus padres
- La herencia forma una cadena: nieto hereda del hijo que hereda del padre
- Un elemento puede sobrescribir el valor heredado con su propio valor

### Atributos heredables

Estos atributos se propagan de padres a hijos:

**Pintura**
- `color`, `fill`, `fill-opacity`, `fill-rule`
- `stroke`, `stroke-dasharray`, `stroke-dashoffset`, `stroke-linecap`, `stroke-linejoin`
- `stroke-miterlimit`, `stroke-opacity`, `stroke-width`
- `paint-order`

**Texto y fuentes**
- `font`, `font-family`, `font-size`, `font-size-adjust`, `font-stretch`
- `font-style`, `font-variant`, `font-weight`
- `direction`, `letter-spacing`, `text-anchor`, `text-decoration`
- `word-spacing`, `writing-mode`

**Otros**
- `visibility`, `cursor`, `pointer-events`
- `color-interpolation`, `color-rendering`, `image-rendering`, `shape-rendering`

### Atributos NO heredables

Estos atributos aplican solo al elemento especifico, no se propagan:

**Geometria y posicion**
- `x`, `y`, `width`, `height` (de `<rect>`, `<svg>`, etc.)
- `cx`, `cy`, `r`, `rx`, `ry` (de circulos y elipses)
- `x1`, `y1`, `x2`, `y2` (de `<line>`)

**Efectos**
- `opacity` (se aplica al elemento renderizado como unidad)
- `clip-path`, `mask`, `filter`
- `display`, `overflow`

**Geometria de transformacion**
- `transform` (el sistema de coordenadas se propaga, pero no es "herencia" en el sentido CSS)

### La cascada completa de estilos en SVG

De menor a mayor prioridad (el valor final es el de mayor prioridad que aplique):

1. **Valor inicial** (el valor por defecto del navegador para esa propiedad)
2. **Herencia del padre** (si la propiedad es heredable)
3. **Atributo de presentacion** (`fill="red"` en el elemento)
4. **Regla CSS en hoja de estilos** (por especificidad CSS: tipo < clase < id)
5. **Estilo inline** (`style="fill: red;"`)
6. **`!important`** en cualquier regla CSS

**El punto critico**: los **atributos de presentacion tienen menor prioridad que el CSS**. Esto permite sobreescribir estilos de un SVG generado por un editor grafico simplemente con una regla CSS, sin modificar el SVG.

### Patrones comunes de herencia en la practica

**Paleta de colores en el SVG raiz**
- `<svg fill="none" stroke="currentColor">`: todos los hijos sin fill y con stroke heredado

**Grupo como "tema"**
- `<g fill="#005ea5" font-family="Arial" font-size="14">`: todos los elementos del grupo comparten el estilo base

**Sobrescritura selectiva**
- Definir `fill` en el grupo, sobrescribir en elementos especificos
- Mas eficiente que repetir el mismo fill en cada elemento
