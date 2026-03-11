# Seccion 2: Estructura del Documento SVG

> Todo SVG tiene una anatomia definida. Conocer cada parte y su proposito
> permite escribir SVG correcto, accesible y mantenible desde el principio.

---

## 2.1 El elemento raiz `<svg>`

### Que es y por que existe
- Es el **contenedor principal** de todo el contenido SVG
- Define el **viewport**: el area visible del grafico
- Establece el **espacio de coordenadas** en el que viven los elementos hijos
- Sin el elemento `<svg>`, no hay SVG

### Atributos fundamentales

**`xmlns` — Namespace XML**
- Valor: `"http://www.w3.org/2000/svg"`
- **Obligatorio** en archivos `.svg` independientes
- **Opcional** (pero recomendado) en SVG inline dentro de HTML5
- Sin este atributo en un archivo `.svg`, el navegador puede no reconocer el documento como SVG
- Es una URI (no una URL real), solo un identificador unico de vocabulario

**`xmlns:xlink` — Namespace XLink (legacy)**
- Valor: `"http://www.w3.org/1999/xlink"`
- Necesario en SVG 1.1 para el atributo `xlink:href` (referencias a elementos)
- En SVG 2, `href` reemplaza a `xlink:href`, haciendo este namespace opcional
- Mantenerlo no hace dano; omitirlo puede causar problemas en SVGs que usan `xlink:href`

**`width` y `height` — Dimensiones del viewport**
- Definen el **tamano fisico** del SVG en la pagina
- Aceptan: valores sin unidad (pixeles por defecto), px, em, %, cm, mm, etc.
- Si se omiten: el SVG ocupa el 100% del contenedor (comportamiento en HTML inline)
- Interaccion con `viewBox`: si ambos estan presentes, el viewBox se escala para caber en width/height

**`viewBox` — Sistema de coordenadas interno**
- Sintaxis: `viewBox="min-x min-y ancho alto"`
- Define el sistema de coordenadas que usan los elementos hijos
- Permite que el SVG sea escalable e independiente de su tamano fisico
- Es el atributo mas importante para SVG responsivo

**`version`**
- Valor tipico: `"1.1"`
- Historicamente indicaba la version de SVG
- En la practica moderna, los navegadores lo ignoran
- No es necesario incluirlo, pero tampoco hace dano

**`preserveAspectRatio`**
- Controla como se ajusta el viewBox dentro del viewport
- Solo relevante cuando ambos estan presentes y tienen proporciones distintas
- Ver seccion 3.3 para detalle completo

### SVG como archivo independiente vs inline en HTML

**Archivo `.svg` independiente**
- Requiere `xmlns`
- Puede tener prolog XML (`<?xml version="1.0"?>`)
- Se carga como documento XML completo
- Tiene su propio contexto: CSS del documento padre no aplica directamente
- Se puede abrir directamente en el navegador

**SVG inline en HTML5**
- Se inserta el codigo `<svg>...</svg>` directamente en el HTML
- El `xmlns` puede omitirse (el parser HTML lo infiere)
- El CSS del documento padre SI aplica
- JavaScript del documento padre puede manipular el SVG directamente
- El SVG es parte del mismo DOM que el HTML
- No se puede acceder directamente por URL

### Casos especiales del elemento `<svg>`

**SVG anidado**
- Un elemento `<svg>` puede contener otro `<svg>` hijo
- Cada `<svg>` hijo crea su propio viewport y sistema de coordenadas
- Tiene atributos `x`, `y` para posicionarlo dentro del padre

**SVG como icono (patron comun)**
- `viewBox="0 0 24 24"` sin `width`/`height` fijos
- Controlado 100% por CSS desde el HTML
- Hereda el color con `currentColor`

---

## 2.2 Prologo XML

### La declaracion XML
```
<?xml version="1.0" encoding="UTF-8"?>
```
- No es un elemento XML, es una **instruccion de procesamiento**
- Debe ser la **primera linea** del documento si se incluye (sin espacios antes)
- Indica la version de XML (siempre `"1.0"`) y la codificacion del archivo

### Cuando incluirla y cuando no
| Contexto | Recomendacion |
|----------|---------------|
| Archivo `.svg` independiente | Opcional pero recomendado |
| SVG inline en HTML | No incluir (el parser HTML lo rechaza) |
| SVG procesado por servidores XML | Puede ser obligatorio |
| SVG generado programaticamente | Depende del destino final |

### El atributo `encoding`
- `UTF-8`: el estandar moderno, soporta todos los caracteres Unicode
- Si el archivo tiene caracteres no-ASCII (acentos, simbolos) y no se declara encoding, puede haber problemas en algunos parsers
- Los editores de texto modernos guardan UTF-8 por defecto

### DOCTYPE en SVG (por que no usarlo)
- En SVG 1.1 habia una declaracion DOCTYPE oficial:
  ```
  <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
    "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
  ```
- Los navegadores modernos la **ignoran** completamente
- Puede causar peticiones HTTP innecesarias a la URL del DTD
- SVGO la elimina por defecto
- **Recomendacion**: no incluirla en SVGs nuevos

---

## 2.3 Elementos de metadatos

### `<title>` — El titulo del SVG

**Proposito**
- Texto alternativo al SVG, equivalente al atributo `alt` de `<img>`
- El lector de pantalla anuncia este texto al encontrar el SVG
- Los navegadores lo muestran como tooltip al pasar el mouse (en algunos contextos)

**Posicion y reglas**
- Debe ser el **primer hijo** del elemento al que describe
- Puede estar dentro de `<svg>` (describe el SVG completo) o dentro de `<g>` (describe ese grupo)
- Solo contiene texto plano (no elementos graficos)

**Relacion con accesibilidad**
- Sin `<title>`, un SVG complejo es inaccesible para usuarios de lectores de pantalla
- Se referencia con `aria-labelledby="id-del-title"` en el elemento `<svg>`
- Para SVGs decorativos sin significado, se usa `aria-hidden="true"` en vez de `<title>`

### `<desc>` — Descripcion larga

**Proposito**
- Descripcion detallada del SVG para usuarios que no pueden verlo
- Complementa a `<title>`: si el titulo es "Grafico de ventas 2024", la descripcion explica los datos
- No se muestra visualmente
- Los lectores de pantalla pueden anunciarlo (dependiendo de la configuracion)

**Cuando incluirlo**
- SVGs complejos con informacion importante (graficos de datos, mapas, diagramas)
- SVGs interactivos donde el usuario necesita contexto
- No es necesario para iconos puramente decorativos

### `<metadata>` — Metadatos estructurados

**Proposito**
- Contenedor para metadatos en formatos externos (no texto plano)
- Tipicamente RDF (Resource Description Framework) o Dublin Core
- Usado por editores graficos como Inkscape para almacenar informacion del autor, licencia, etc.

**Uso practico**
- Los editores graficos lo generan automaticamente
- En SVGs para web, usualmente se elimina con SVGO (es innecesario para el navegador)
- No confundir con `<title>` y `<desc>` que SI son relevantes para accesibilidad

---

## 2.4 El elemento `<defs>`

### Proposito y naturaleza
- **`<defs>`** = definitions (definiciones)
- Contenedor para elementos que se **definen** pero **no se renderizan directamente**
- Todo lo que este dentro de `<defs>` es invisible hasta que se **referencia** desde fuera
- Permite definir recursos reutilizables una sola vez y usarlos multiples veces

### Que elementos van dentro de `<defs>`

| Elemento | Para que se usa |
|----------|-----------------|
| `<linearGradient>` | Gradientes lineales para fill/stroke |
| `<radialGradient>` | Gradientes radiales para fill/stroke |
| `<pattern>` | Patrones repetibles para fill/stroke |
| `<filter>` | Filtros de efectos visuales (blur, shadow, etc.) |
| `<clipPath>` | Mascaras de recorte |
| `<mask>` | Mascaras con transparencia gradual |
| `<symbol>` | Componentes graficos reutilizables |
| `<marker>` | Cabezas de flecha y decoraciones de linea |
| `<g>` | Grupos que seran referenciados por `<use>` |

### Como funciona la relacion definir-referenciar

**Definir**: se asigna un `id` unico al elemento dentro de `<defs>`
```
Dentro de <defs>: <linearGradient id="miGradiente"> ... </linearGradient>
```

**Referenciar**: se usa `url(#id)` o `href="#id"` desde otro elemento
```
Fuera de <defs>: <rect fill="url(#miGradiente)" />
```

### Posicion de `<defs>` en el documento
- Convencionalmente al inicio del SVG, antes del contenido visual
- El navegador no requiere un orden especifico (puede referenciar hacia adelante)
- Por claridad y mantenibilidad, siempre primero

### Elementos fuera de `<defs>`
- Tecnicamente, elementos como `<linearGradient>` o `<symbol>` pueden definirse fuera de `<defs>`
- El comportamiento es el mismo: no se renderizan directamente si no son referenciados
- La convencion es usar `<defs>` para dejar claro que son definiciones
- Los validadores y linters suelen recomendar el uso correcto de `<defs>`

---

## 2.5 Orden de renderizado (Painter's Model)

### El modelo del pintor
- SVG usa el **"Painter's Algorithm"** o modelo del pintor
- Los elementos se dibujan **en orden de aparicion en el codigo fuente**
- Cada elemento nuevo se pinta **encima** de lo ya dibujado
- Es identico a como un pintor aplica capas sobre un lienzo: las capas posteriores tapan las anteriores

### Implicaciones practicas

**El orden del codigo ES el orden visual**
- El primer elemento del SVG queda "al fondo"
- El ultimo elemento del SVG queda "al frente"
- Para poner algo detras de otra cosa: moverlo antes en el codigo

**No existe `z-index` nativo en SVG**
- En HTML/CSS, `z-index` controla la superposicion independientemente del orden en el DOM
- En SVG 1.1, la unica forma de controlar superposicion es reordenar elementos en el codigo
- SVG 2 propone una propiedad `z-index` pero el soporte es limitado
- En la practica: reordenar el DOM es la solucion estandar

**Herramienta de JavaScript para reordenar**
- `element.parentNode.appendChild(element)` — mueve al final (frente)
- `element.parentNode.insertBefore(element, primerHijo)` — mueve al inicio (fondo)

### Grupos y el modelo del pintor
- Un `<g>` (grupo) se renderiza como una unidad
- Todos los hijos del grupo se dibujan antes de pasar al siguiente elemento hermano
- El grupo en su conjunto ocupa una "capa" en el orden de renderizado

### Ejemplo conceptual del modelo

Dado este orden en el codigo:
1. Fondo azul (rectangulo grande)
2. Circulo rojo
3. Texto "Hola"

El resultado visual es: fondo azul visible completamente → circulo rojo encima del fondo → texto encima de ambos.

Si el texto estuviera antes del circulo en el codigo, el circulo taparia el texto.

---

## 2.6 Estructura minima de un SVG valido

### Los componentes esenciales

**Version minima absoluta** (para experimentar rapidamente):
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <!-- contenido -->
</svg>
```

**Version recomendada para produccion**:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg"
     viewBox="0 0 100 100"
     width="100"
     height="100"
     role="img"
     aria-labelledby="titulo descripcion">
  <title id="titulo">Nombre descriptivo del grafico</title>
  <desc id="descripcion">Descripcion detallada del contenido para lectores de pantalla</desc>
  <defs>
    <!-- Recursos reutilizables aqui -->
  </defs>
  <!-- Contenido grafico aqui -->
</svg>
```

### Por que cada parte importa

| Parte | Obligatoria | Por que incluirla |
|-------|-------------|-------------------|
| `xmlns` | En archivos .svg | Sin esto, el parser puede no reconocer el SVG |
| `viewBox` | Muy recomendada | Sin esto, el SVG no es escalable |
| `width`/`height` | Depende del contexto | Para SVG independiente, define el tamano |
| `role="img"` | Recomendada | Indica a tecnologias asistivas que es una imagen |
| `aria-labelledby` | Recomendada | Conecta el SVG con su titulo/descripcion |
| `<title>` | Muy recomendada | Accesibilidad: texto alternativo |
| `<desc>` | Para SVGs complejos | Accesibilidad: descripcion detallada |
| `<defs>` | Solo si hay recursos | Organizar recursos reutilizables |

### Diferencias segun el contexto de uso

**SVG como icono decorativo** (sin significado propio)
```xml
<svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
  <!-- icono -->
</svg>
```

**SVG como icono con significado** (boton, enlace)
```xml
<svg role="img" aria-label="Cerrar ventana" viewBox="0 0 24 24">
  <!-- X de cierre -->
</svg>
```

**SVG como ilustracion o grafico complejo**
```xml
<svg role="img" aria-labelledby="t1 d1" viewBox="0 0 400 300">
  <title id="t1">Ventas por trimestre 2024</title>
  <desc id="d1">Grafico de barras que muestra...</desc>
  <!-- grafico -->
</svg>
```
