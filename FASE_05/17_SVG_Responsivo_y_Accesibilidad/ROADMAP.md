# Seccion 17: SVG Responsivo y Accesibilidad

> Un SVG responsivo se adapta a cualquier contenedor sin perder calidad.
> Un SVG accesible puede ser usado por personas con tecnologias de apoyo.
> Ambos objetivos deben considerarse desde el principio, no como adicion posterior.

---

## 17.1 SVG responsivo basico

### El principio fundamental
- Un SVG es responsivo cuando se adapta al tamano de su contenedor **sin perder calidad**
- La clave es que SVG ya es escalable por naturaleza (es vectorial)
- El reto es configurarlo correctamente para que el navegador le permita escalar

### La receta minima para un SVG responsivo

**1. Usar `viewBox`**
- `viewBox` define las proporciones del contenido interno
- Sin `viewBox`, el SVG no sabe como escalar su contenido al redimensionarse
- Siempre incluir `viewBox` en cualquier SVG que deba ser responsivo

**2. No fijar `width` y `height` en unidades absolutas**
- Si el SVG tiene `width="400" height="300"` fijos, siempre ocupa ese espacio exacto
- Eliminar los atributos `width`/`height` del elemento `<svg>`, o ponerlos en porcentajes
- Controlarlo desde CSS del contenedor padre

**3. Controlar el tamano desde CSS**
```css
svg {
  width: 100%;
  height: auto;
}
```
- `width: 100%`: el SVG ocupa todo el ancho del contenedor
- `height: auto`: la altura se calcula automaticamente manteniendo la proporcion del viewBox

### Por que `height: auto` funciona
- El viewBox define una proporcion (ej: `viewBox="0 0 400 300"` → proporcion 4:3)
- Con `width: 100%` y `height: auto`, el navegador calcula la altura para mantener esa proporcion
- El contenido vectorial se escala perfectamente al nuevo tamano

### El comportamiento sin width/height en el SVG

Cuando se omiten `width` y `height` del elemento `<svg>` y se controla desde CSS:
- **SVG inline en HTML**: se comporta como un elemento de bloque; toma el ancho de su contenedor
- **SVG en `<img>`**: se comporta como una imagen; se puede controlar con CSS en el `<img>`
- **SVG en `<object>`**: necesita width/height en el propio elemento `<object>` o en CSS

---

## 17.2 Tecnicas de SVG fluido

### Control total desde CSS

**Patrón recomendado moderno**
```css
.contenedor-svg {
  width: 100%;
  max-width: 600px;     /* tamanio maximo */
}

.contenedor-svg svg {
  width: 100%;
  height: auto;
  display: block;       /* evita espacio extra debajo (inline whitespace) */
}
```

**Por que `display: block`**
- Los elementos SVG son `inline` por defecto
- Como elementos inline, tienen un pequenio espacio debajo (el espacio de descendentes tipograficos)
- `display: block` elimina ese espacio extra

### La propiedad CSS `aspect-ratio`

CSS moderno ofrece `aspect-ratio` para mantener proporciones sin trucos:
```css
svg {
  width: 100%;
  aspect-ratio: 4 / 3;   /* define la proporcion directamente */
}
```
- Reemplaza el "padding-hack" que se usaba antes
- Mas legible e intuitivo
- Soporte amplio en navegadores modernos

### El padding-hack (tecnica legacy, para referencia)

Antes de `aspect-ratio`, se usaba este truco CSS:
```css
.wrapper {
  position: relative;
  padding-bottom: 75%;  /* 75% = 300/400 * 100% (proporcion 4:3) */
  height: 0;
}
.wrapper svg {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
}
```
- Funciona porque `padding-bottom` en porcentaje es relativo al **ancho** del elemento
- Al poner `height: 0` y `padding-bottom: X%`, el contenedor toma una altura proporcional al ancho
- El SVG dentro se estira para llenar ese espacio con `position: absolute`
- Todavia se encuentra en codigo legacy; entenderlo ayuda a mantener proyectos viejos

### SVG con tamano intrinseco vs controlado

**Con `width`/`height` en el SVG (tamano intrinseco)**
- El SVG "sabe" su tamano preferido
- Util cuando el SVG debe tener un tamano especifico por defecto
- CSS puede sobreescribir estos valores

**Sin `width`/`height` (controlado por CSS)**
- El SVG no tiene tamano preferido propio
- Depende completamente del CSS para su tamano
- Mas flexible para sistemas de diseno responsivo

---

## 17.3 SVG adaptativo (responsive con media queries)

### Media queries dentro del SVG

El SVG puede contener su propio bloque `<style>` con media queries:
```xml
<svg viewBox="0 0 200 100">
  <style>
    .detalle { display: block; }
    @media (max-width: 200px) {
      .detalle { display: none; }
    }
  </style>
  <circle cx="50" cy="50" r="40" />
  <text class="detalle" x="100" y="50">Detalle visible</text>
</svg>
```

### Comportamiento de las media queries en SVG: el detalle critico

**SVG inline en HTML**
- Las media queries responden al **viewport del navegador** (el ancho de la ventana)
- Mismo comportamiento que las media queries en CSS externo

**SVG como archivo externo (`<img>`, `<object>`, `background-image`)**
- Las media queries responden al **tamano del propio SVG** (su viewport interno)
- No al ancho de la ventana del navegador
- Un SVG de 100px de ancho mostrara el "estado movil" aunque la pantalla sea de 1920px
- Esto es en realidad muy util: el SVG adapta su contenido a su propio tamano disponible

**Consecuencia practica**
- Para SVG responsivos con media queries como archivos externos: pensar en el tamano del SVG, no de la pantalla
- Para SVG inline: las media queries se comportan como en CSS normal

### Casos de uso del SVG adaptativo

**Simplificar en tamanos pequenios**
- Ocultar etiquetas de texto cuando el SVG es pequenio
- Reducir el numero de elementos decorativos
- Mostrar una version simplificada del grafico

**Logo responsivo**
- Un logo detallado para tamanos grandes
- El mismo logo simplificado (solo el simbolo, sin texto) para tamanos pequenios
- Todo en un solo archivo SVG con media queries internas

**Graficos de datos adaptativos**
- Mostrar todos los puntos de datos en pantallas grandes
- Mostrar solo los datos principales en pantallas pequenias
- Cambiar la orientacion de etiquetas segun el espacio disponible

---

## 17.4 Accesibilidad en SVG

### Por que la accesibilidad importa en SVG
- Los graficos SVG transmiten informacion visual que puede ser inaccesible para:
  - Usuarios ciegos (usan lectores de pantalla como NVDA, JAWS, VoiceOver)
  - Usuarios con baja vision
  - Usuarios que navegan solo con teclado
- Sin accesibilidad, la informacion se pierde completamente para estos usuarios

### Patron de accesibilidad completo

```xml
<svg role="img"
     aria-labelledby="titulo-svg desc-svg"
     viewBox="0 0 200 100">
  <title id="titulo-svg">Ventas por trimestre 2024</title>
  <desc id="desc-svg">
    Grafico de barras. T1: 45, T2: 62, T3: 78, T4: 91.
    Tendencia creciente durante el anio.
  </desc>
  <!-- contenido grafico -->
</svg>
```

### Atributo `role="img"`

**Por que es necesario**
- Los navegadores y lectores de pantalla no siempre reconocen un `<svg>` como una imagen
- `role="img"` indica explicitamente que el SVG es una imagen con significado
- Sin este role, algunos lectores de pantalla pueden ignorar el SVG o leer su contenido interno de forma confusa

**Cuando usarlo**
- Siempre que el SVG transmita informacion (graficos, iconos significativos, ilustraciones)
- No en SVGs puramente decorativos (usar `aria-hidden="true"` en su lugar)

### `<title>` — El texto alternativo del SVG

**Analogia con HTML**
- `<title>` en SVG es equivalente al atributo `alt` en `<img>`
- El lector de pantalla anuncia el contenido del `<title>` cuando el usuario llega al SVG
- Debe ser conciso pero descriptivo

**Reglas de posicion**
- Debe ser el **primer hijo** del elemento al que describe
- Puede estar dentro de `<svg>` (describe todo el SVG) o dentro de `<g>` (describe ese grupo)

**Contenido del `<title>`**
- Texto breve y descriptivo: "Logotipo de Empresa X", "Flecha apuntando a la derecha"
- Para graficos de datos: el titulo del grafico, no los datos en si (eso va en `<desc>`)

### `<desc>` — Descripcion larga

**Cuando es necesario**
- Cuando el `<title>` no es suficiente para transmitir la informacion completa
- Graficos de datos: describir los datos mas importantes o la tendencia
- Diagramas complejos: describir la estructura y las relaciones

**Ejemplo de contenido util**
- No util: "Grafico de barras"
- Util: "Grafico de barras con cuatro barras. La primera (enero) llega al 45%, la segunda (febrero) al 62%, la tercera (marzo) al 78% y la cuarta (abril) al 91%. La tendencia es claramente ascendente."

### `aria-labelledby` y `aria-describedby`

**`aria-labelledby`**
- Conecta el `<svg>` con su(s) elemento(s) de etiquetado por `id`
- `aria-labelledby="titulo-svg"`: el `<title>` es la etiqueta del SVG
- `aria-labelledby="titulo-svg desc-svg"`: tanto el `<title>` como el `<desc>` son leidos como etiqueta

**`aria-describedby`**
- Alternativa para conectar con una descripcion adicional
- El lector de pantalla lo anuncia como descripcion, no como nombre
- `aria-describedby="desc-svg"` junto con `aria-labelledby="titulo-svg"`

### SVGs decorativos: ocultarlos de tecnologias asistivas

Cuando el SVG no transmite informacion (puramente decorativo):

```xml
<svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
  <!-- icono decorativo -->
</svg>
```

- `aria-hidden="true"`: el lector de pantalla ignora completamente el SVG
- `focusable="false"`: en IE/Edge legacy, los SVGs pueden recibir foco con teclado — este atributo lo previene
- Sin `aria-hidden`, el lector puede intentar leer el SVG y anunciar algo confuso

### Iconos con significado en botones y enlaces

Cuando un icono esta dentro de un boton o enlace y tiene significado funcional:

**Opcion 1: Texto visible acompanando al icono**
```html
<button>
  <svg aria-hidden="true" focusable="false">...</svg>
  Enviar
</button>
```
- El texto "Enviar" es suficiente para el lector de pantalla
- El SVG se oculta con `aria-hidden` para evitar redundancia

**Opcion 2: Icono solo, con aria-label en el boton**
```html
<button aria-label="Enviar formulario">
  <svg aria-hidden="true" focusable="false">...</svg>
</button>
```
- `aria-label` en el boton proporciona el nombre accesible
- El SVG se oculta con `aria-hidden` para evitar que se lean dos veces

**Opcion 3: `<title>` dentro del SVG con `role="img"`**
```html
<button>
  <svg role="img" aria-labelledby="icono-enviar">
    <title id="icono-enviar">Enviar formulario</title>
    ...
  </svg>
</button>
```
- Menos comun, puede causar anuncios redundantes en algunos lectores de pantalla

### Accesibilidad de graficos de datos complejos

Para graficos de datos donde la informacion es critica:

**Tabla de datos oculta visualmente**
- Proporcionar los datos del grafico en una `<table>` oculta visualmente pero accesible
- `class="sr-only"` (screen-reader only): `position: absolute; width: 1px; height: 1px; overflow: hidden;`
- El grafico SVG con `aria-hidden="true"`: puramente visual
- La tabla proporciona los datos reales al lector de pantalla

**Descripcion completa en `<desc>`**
- Si la tabla no es viable, describir los datos mas importantes en `<desc>`
- Incluir valores exactos, tendencias y conclusiones principales

---

## 17.5 SVG y alto contraste / modo oscuro

### El modo de alto contraste del sistema operativo

- Windows tiene un modo de "Alto Contraste" que sobreescribe los colores de la pagina
- Los colores SVG definidos como atributos (`fill="blue"`) pueden ser sobreescritos
- Los colores CSS con `prefers-contrast: more` pueden adaptarse

**Implicacion**
- Los SVGs pueden verse afectados por el modo de alto contraste
- Usar `currentColor` y propiedades CSS hace que el SVG responda mejor al alto contraste
- Evitar informacion transmitida solo por color (principio de accesibilidad general)

### Media query `prefers-color-scheme`

Para SVG que deben adaptarse al modo oscuro del sistema:

```xml
<svg viewBox="0 0 100 100">
  <style>
    circle { fill: #1a1a2e; stroke: #e0e0e0; }
    @media (prefers-color-scheme: dark) {
      circle { fill: #e0e0e0; stroke: #1a1a2e; }
    }
  </style>
  <circle cx="50" cy="50" r="40" />
</svg>
```

- Los colores se invierten segun la preferencia del sistema
- Funciona en SVG inline y en SVG como archivo externo (en este ultimo, las media queries responden al viewport del SVG, lo cual puede ser inesperado)

### Variables CSS para tematizacion

El patron mas flexible para SVG tematizable:

```xml
<svg viewBox="0 0 100 100">
  <style>
    :root {
      --color-principal: #005ea5;
      --color-fondo: white;
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --color-principal: #7ab3d9;
        --color-fondo: #1a1a2e;
      }
    }
    circle { fill: var(--color-fondo); stroke: var(--color-principal); }
  </style>
  <circle cx="50" cy="50" r="40" />
</svg>
```

- Las variables CSS se definen en `:root` del SVG
- Las media queries solo cambian los valores de las variables
- El resto del CSS no necesita duplicarse
- Si el SVG es inline en HTML, hereda las variables CSS definidas en el documento HTML
