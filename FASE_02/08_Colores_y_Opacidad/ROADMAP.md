# Seccion 8: Colores y Opacidad

> Los colores en SVG son identicos a CSS con una excepcion: `currentColor`.
> La opacidad tiene tres niveles distintos y confundirlos produce errores sutiles.

---

## 8.1 Formatos de color soportados

### Nombres de color
- SVG acepta todos los **nombres de color CSS**: `red`, `blue`, `green`, `steelblue`, `tomato`, etc.
- Hay 140+ nombres de color definidos en CSS Color Level 4
- Son insensibles a mayusculas/minusculas: `Red`, `RED`, `red` son equivalentes
- Util para prototipos rapidos y colores comunes; para produccion se prefiere hex o hsl

**Nombres de color especiales importantes**
- `black`: negro puro `#000000` — el **fill por defecto** de los elementos SVG
- `white`: blanco puro `#FFFFFF`
- `transparent`: totalmente transparente (alfa = 0)
- `none`: ausencia de pintura (diferente de `transparent`, ver seccion 8.4)
- `currentColor`: color heredado del contexto CSS (ver seccion 8.3)

### Notacion hexadecimal

**Formato largo: `#RRGGBB`**
- Seis digitos hexadecimales (0-9, A-F)
- Dos digitos por canal: Rojo, Verde, Azul
- Rango de cada canal: `00` (0) a `FF` (255)
- `#FF0000` = rojo puro, `#00FF00` = verde puro, `#0000FF` = azul puro
- `#000000` = negro, `#FFFFFF` = blanco

**Formato corto: `#RGB`**
- Tres digitos; cada digito se duplica para obtener el valor real
- `#F00` equivale a `#FF0000` (rojo)
- `#ABC` equivale a `#AABBCC`
- Solo funciona cuando ambos digitos de cada canal son iguales

**Con canal alfa: `#RRGGBBAA` y `#RGBA`**
- El cuarto par de digitos define la opacidad
- `#FF000080` = rojo al 50% de opacidad (80 hex = 128 dec ≈ 50%)
- `#F008` = rojo al ~53% de opacidad
- Soporte: CSS Color Level 4, navegadores modernos

### Funcion `rgb()`

**Sintaxis clasica**
```
rgb(255, 0, 0)         /* rojo puro */
rgb(0, 128, 0)         /* verde medio */
rgb(100%, 0%, 0%)      /* rojo en porcentajes */
```
- Tres parametros: rojo, verde, azul
- Rango: 0–255 o 0%–100%
- Sin informacion de opacidad

**Sintaxis moderna (CSS Color Level 4)**
```
rgb(255 0 0)           /* sin comas */
rgb(255 0 0 / 0.5)     /* con opacidad */
rgb(255 0 0 / 50%)     /* opacidad como porcentaje */
```

### Funcion `rgba()`

```
rgba(255, 0, 0, 0.5)   /* rojo al 50% de opacidad */
rgba(0, 0, 255, 0.8)   /* azul al 80% de opacidad */
```
- Cuarto parametro: canal alfa (0 = invisible, 1 = opaco)
- Acepta fracciones (0.5) o porcentajes (50%)
- En CSS moderno, `rgb()` acepta alfa directamente: `rgb(255 0 0 / 50%)`

### Funcion `hsl()`

**Que es HSL**
- HSL = Hue (tono), Saturation (saturacion), Lightness (luminosidad)
- Mas intuitivo que RGB para elegir y ajustar colores a mano
- **H** (tono): angulo en la rueda de color (0° = rojo, 120° = verde, 240° = azul, 360° = rojo de nuevo)
- **S** (saturacion): intensidad del color (0% = gris, 100% = color puro)
- **L** (luminosidad): brillo (0% = negro, 50% = color normal, 100% = blanco)

**Ejemplos**
```
hsl(0, 100%, 50%)       /* rojo puro */
hsl(120, 100%, 50%)     /* verde puro */
hsl(240, 100%, 50%)     /* azul puro */
hsl(0, 0%, 50%)         /* gris medio */
hsl(210, 80%, 60%)      /* azul cielo */
```

**Ventaja de HSL para diseño**
- Crear variantes de un color cambiando solo L: versiones mas claras/oscuras
- Crear colores complementarios: H + 180° (rojo → cyan)
- Ajustar saturacion para versiones "apagadas" del mismo tono

### Funcion `hsla()`

```
hsla(0, 100%, 50%, 0.5)    /* rojo al 50% de opacidad */
hsla(120, 60%, 40%, 0.8)   /* verde oscuro al 80% */
```
- Cuarto parametro: canal alfa (igual que `rgba`)

### Formatos modernos (CSS Color Level 4 y 5)

**`oklch()` y `oklab()`**
- Espacios de color perceptualmente uniformes (mejor para interpolacion)
- Soporte creciente en navegadores modernos
- Util para gradientes donde los colores intermedios deben ser perceptualmente correctos

**Paletas del sistema**
- `Canvas`, `ButtonText`, `Highlight`, etc.: colores del sistema operativo
- Utiles para adaptar SVGs al modo claro/oscuro del SO sin media queries

---

## 8.2 Opacidad

### Los tres niveles de opacidad en SVG

SVG tiene tres propiedades distintas para controlar la transparencia, cada una con alcance diferente:

#### Nivel 1: `opacity` — Todo el elemento

**Que afecta**
- El elemento completo: su fill, su stroke, sus hijos (si es un grupo)
- Es una "opacidad de composicion": el elemento se renderiza completamente y luego se aplica la transparencia al resultado final

**Caracteristica clave: la opacidad es multiplicativa para los hijos**
- Un grupo `<g opacity="0.5">` que contiene un elemento con `opacity="0.5"`
- El elemento hijo se ve al `0.5 * 0.5 = 0.25` de opacidad total
- Los valores se multiplican a lo largo de la jerarquia

**Cuando usarlo**
- Para hacer un elemento o grupo completo semitransparente
- Para animar la aparicion/desaparicion de elementos (fade in/out)

#### Nivel 2: `fill-opacity` — Solo el relleno

**Que afecta**
- Unicamente el color de relleno (`fill`) del elemento
- El stroke no se ve afectado

**Ejemplo practico**
- Una forma con `fill-opacity="0.3"` y `stroke="black"`: el interior es semitransparente pero el borde es completamente opaco

**Cuando usarlo**
- Formas con fondo semitransparente pero contorno solido
- Superposicion de formas donde se quiere ver el fondo a traves del fill
- Efectos de "highlight" sobre contenido existente

#### Nivel 3: `stroke-opacity` — Solo el trazo

**Que afecta**
- Unicamente el color del trazo (`stroke`)
- El fill no se ve afectado

**Ejemplo practico**
- `fill="white" stroke-opacity="0.5"`: forma blanca opaca con borde semitransparente
- `fill="none" stroke="blue" stroke-opacity="0.4"`: solo el contorno, semitransparente

### Diferencias criticas entre `opacity` y `fill-opacity`/`stroke-opacity`

| Aspecto | `opacity` | `fill-opacity` / `stroke-opacity` |
|---------|-----------|----------------------------------|
| Alcance | Todo el elemento (y sus hijos) | Solo fill o stroke del elemento |
| Composicion | Renderiza primero, opaca despues | Mezcla los colores directamente |
| Efecto en grupos | Afecta a todos los hijos | No aplica a los hijos |
| Herencia | Si (afecta hijos) | Solo el elemento propio |

### Ejemplo ilustrativo de la diferencia

**Caso: dos circulos solapados en un grupo**
- Con `<g opacity="0.5">`: los dos circulos se renderizan a opacidad plena, luego todo el grupo se hace 50% transparente. Los circulos no se "ven" entre si dentro del grupo.
- Con `fill-opacity="0.5"` en cada circulo: cada circulo mezcla su color directamente con el fondo. Donde se solapan, hay doble mezcla → se ve mas opaco que en las zonas sin solapamiento.

---

## 8.3 currentColor

### Que es `currentColor`
- Una palabra clave CSS especial que representa el **valor actual de la propiedad `color`** del elemento o sus ancestros
- `color` en CSS controla el color del texto; `currentColor` lo hereda para otros usos
- Cuando un elemento SVG usa `fill="currentColor"`, su fill es el mismo color que el texto del entorno

### Por que existe y para que sirve

**El problema que resuelve**
- Los iconos SVG inline necesitan adaptarse al color del contexto donde se usan
- Sin `currentColor`: hay que especificar el color del icono explicitamente o usar clases CSS
- Con `currentColor`: el icono hereda automaticamente el color del texto del elemento padre HTML

**Flujo tipico**
```
HTML: <button class="btn-danger"><svg>...</svg> Eliminar</button>
CSS:  .btn-danger { color: #dc3545; }
SVG:  <path fill="currentColor" d="..." />
```
El icono se vuelve rojo `#dc3545` porque hereda el `color` del boton.

### Casos de uso

**Iconos adaptables**
- Sistema de iconos donde todos tienen `fill="currentColor"`
- Al cambiar `color` en CSS, todos los iconos del elemento cambian
- Ideal para iconos en botones, enlaces, badges, estados (error/success/warning)

**Modo oscuro automatico**
- Con `currentColor`, los iconos que tienen `fill="currentColor"` cambian de color cuando se cambia `color` en el CSS del modo oscuro
- Sin necesidad de sobreescribir el SVG directamente

**Herencia en componentes**
- Componentes de interfaz que definen su color con `color`
- Los iconos SVG internos siguen ese color sin configuracion adicional

### Limitaciones de `currentColor`

**Solo un valor de color**
- `currentColor` solo puede representar un unico color (el de `color`)
- Para iconos de multiples colores, no es suficiente → usar variables CSS en su lugar

**Requiere que `color` este definido en el contexto**
- Si `color` no esta definido en el elemento o sus ancestros, `currentColor` usa el valor por defecto del navegador (generalmente negro)
- En SVG standalone (archivo `.svg` sin contexto HTML), `currentColor` puede ser impredecible

**Variables CSS para multiples colores**
- Para iconos bicolor o tricolor: definir variables CSS (`--icon-color-1`, `--icon-color-2`)
- Usar las variables como valores de fill: `fill: var(--icon-color-1, currentColor)`
- El valor de fallback `currentColor` garantiza un comportamiento razonable si la variable no esta definida

---

## 8.4 Diferencia entre `none` y `transparent`

### `none` — Sin pintura

**Que hace `fill="none"` o `stroke="none"`**
- El elemento **no tiene pintura** en esa propiedad
- No hay ninguna operacion de dibujo para esa propiedad
- La zona sin fill/stroke es **completamente ignorada para eventos del mouse** (segun el valor de `pointer-events`)

**Comportamiento con eventos**
- Con `fill="none"` y `pointer-events` por defecto (`visiblePainted`): el area interior de la forma **no responde** a eventos de click/hover
- Solo el stroke (si existe) responde a eventos en su area
- Util para formas que son solo decorativas y no deben interceptar clics del usuario

**Forma "clickeable" invisible**
- Para crear una zona de click transparente pero funcional: usar `fill="transparent"` (no `none`)

### `transparent` — Pintura invisible

**Que hace `fill="transparent"` o `stroke="transparent"`**
- El elemento **tiene pintura** pero con alfa = 0 (completamente invisible)
- A efectos visuales, es identico a `none`
- Pero la zona SI responde a eventos del mouse (como si la forma fuera visible)

**Comportamiento con eventos**
- Con `fill="transparent"` y `pointer-events` por defecto: el area interior de la forma **SI responde** a eventos de click/hover
- La forma es invisible pero "cliqueable"

### Tabla de diferencias

| | `fill="none"` | `fill="transparent"` |
|-|---------------|---------------------|
| Apariencia visual | Invisible | Invisible |
| Responde a hover/click (default) | No | Si |
| Tiene "pintura" (conceptual) | No | Si (alfa=0) |
| Es interpretado por filtros | No | Si |
| En gradientes | No aplica | Se puede interpolar |

### Cuando usar cada uno

**Usar `none`**
- Formas que solo tienen stroke y no deben interceptar eventos en su interior
- `<polyline fill="none">` para que solo el trazo sea visible e interactivo
- Elementos puramente decorativos sin necesidad de eventos

**Usar `transparent`**
- Crear areas de click invisibles (botones grandes, areas de interaccion expandidas)
- Garantizar que el hover se active sobre toda el area de una forma aunque no tenga color visible
- `<rect fill="transparent" width="100%" height="100%">` como capa de captura de eventos

### La propiedad `pointer-events` modifica este comportamiento

- `pointer-events="all"`: incluso con `fill="none"`, el area interior responde a eventos
- `pointer-events="none"`: incluso con `fill="transparent"`, no responde a eventos
- La combinacion de `fill` y `pointer-events` da control total sobre la interactividad
