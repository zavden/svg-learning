# Seccion 21: Interactividad con JavaScript

> SVG inline en HTML es parte del mismo DOM. Todo lo que JavaScript puede hacer
> con HTML, puede hacerlo con SVG. Pero hay particularidades importantes:
> namespaces, coordenadas y APIs especificas de SVG.

---

## 21.1 Acceso al DOM SVG

### SVG inline = parte del DOM del documento

Cuando el SVG esta inline en HTML, sus elementos son ciudadanos de primera clase del DOM:
- Los mismos metodos de seleccion que se usan para HTML funcionan para SVG
- Los elementos SVG heredan de `Element` (igual que los elementos HTML)
- La herencia especifica: `SVGElement` → `Element` → `Node` → `EventTarget`

### Metodos de seleccion

**Por tipo de elemento**
```javascript
document.querySelector('circle')           // primer circulo del documento
document.querySelectorAll('path')          // todos los paths
document.querySelectorAll('svg rect')      // todos los rects dentro de SVG
```

**Por id**
```javascript
document.getElementById('mi-circulo')
document.querySelector('#mi-circulo')      // equivalente
```

**Por clase**
```javascript
document.querySelectorAll('.punto-datos')
document.querySelectorAll('g.barra')       // grupos con clase "barra"
```

**Desde un elemento SVG padre**
```javascript
const svg = document.querySelector('svg');
svg.querySelector('circle');               // circulo dentro de ese SVG especifico
svg.querySelectorAll('[data-id]');         // elementos con atributo data-id
```

### Tipos de objetos SVG en el DOM

Los elementos SVG son instancias de interfaces especificas:
- `<svg>` → `SVGSVGElement`
- `<g>` → `SVGGElement`
- `<path>` → `SVGPathElement`
- `<circle>` → `SVGCircleElement`
- `<rect>` → `SVGRectElement`
- `<text>` → `SVGTextElement`
- `<use>` → `SVGUseElement`

Estas interfaces extienden `SVGElement` y luego `Element`, heredando todos los metodos del DOM.

---

## 21.2 Manipulacion de atributos

### Metodos genericos del DOM

**Leer atributos**
```javascript
const circulo = document.querySelector('circle');
circulo.getAttribute('cx')          // devuelve el string "50" (siempre string)
circulo.getAttribute('fill')        // "steelblue"
circulo.getAttribute('class')       // "mi-clase"
circulo.hasAttribute('display')     // false si no existe
```

**Escribir atributos**
```javascript
circulo.setAttribute('cx', '100')   // mover el centro a x=100
circulo.setAttribute('r', '30')     // cambiar el radio
circulo.setAttribute('fill', 'red') // cambiar el color
circulo.removeAttribute('stroke')   // eliminar el trazo
```

**Importante: `setAttribute` siempre convierte a string**
- `setAttribute('cx', 100)` funciona (100 se convierte a "100")
- `getAttribute('cx')` siempre devuelve un string: parsear con `parseFloat()` o `parseInt()` si se necesita el numero

### Manipulacion de estilos CSS

```javascript
// Leer estilo computado (el valor final segun la cascada)
const estilos = getComputedStyle(circulo);
estilos.fill;           // el valor de fill aplicado

// Aplicar estilos inline
circulo.style.fill = 'red';
circulo.style.strokeWidth = '2px';   // camelCase en JS
circulo.style.opacity = '0.5';

// Limpiar un estilo inline
circulo.style.fill = '';
```

### Clases CSS en SVG

```javascript
// Agregar clase
circulo.classList.add('activo');
circulo.classList.add('destacado', 'grande');   // multiples clases

// Quitar clase
circulo.classList.remove('activo');

// Alternar clase (si existe la quita, si no existe la agrega)
circulo.classList.toggle('seleccionado');

// Verificar si tiene una clase
circulo.classList.contains('activo');   // true o false

// Reemplazar una clase por otra
circulo.classList.replace('activo', 'inactivo');
```

### La interfaz `SVGAnimatedLength` — propiedades SVG tipadas

Ademas de `getAttribute`/`setAttribute`, los elementos SVG exponen sus atributos como propiedades tipadas:

```javascript
const circulo = document.querySelector('circle');

// Acceso tipado (SVGAnimatedLength)
circulo.cx.baseVal.value = 100;     // valor base (sin animacion)
circulo.cy.baseVal.value = 50;
circulo.r.baseVal.value = 30;

// Para leer
const cx = circulo.cx.baseVal.value;   // numero, no string

// animVal vs baseVal
circulo.cx.baseVal.value;    // valor definido en el atributo
circulo.cx.animVal.value;    // valor actual incluyendo animaciones SMIL activas
```

**Cuando usar la interfaz tipada vs `getAttribute`/`setAttribute`**
- `getAttribute`/`setAttribute`: mas simple, siempre funciona, pero trabaja con strings
- Interfaz tipada: devuelve numeros directamente, refleja animaciones en `animVal`, mas especifico
- En la practica moderna: `getAttribute`/`setAttribute` es suficiente para la mayoria de casos

### Metodos de geometria

**`getBBox()` — Bounding box del elemento**
```javascript
const bbox = elemento.getBBox();
// Devuelve un SVGRect con:
bbox.x;       // coordenada X de la esquina superior izquierda
bbox.y;       // coordenada Y de la esquina superior izquierda
bbox.width;   // ancho del bounding box
bbox.height;  // alto del bounding box
```
- Las coordenadas son en el **espacio de coordenadas del elemento** (sin considerar transformaciones del padre)
- Util para centrar elementos, calcular colisiones, posicionar tooltips

**`getCTM()` — Current Transformation Matrix**
```javascript
const ctm = elemento.getCTM();
// Devuelve un SVGMatrix (a, b, c, d, e, f)
// Representa todas las transformaciones acumuladas del elemento
```

**`getScreenCTM()` — Matriz relativa a la pantalla**
```javascript
const screenCtm = svgElement.getScreenCTM();
// La misma pero en coordenadas de pantalla (pixels CSS)
// Se usa para convertir coordenadas del mouse a coordenadas SVG
```

---

## 21.3 Creacion dinamica de elementos SVG

### El namespace es obligatorio

En HTML, `document.createElement('div')` funciona porque el parser conoce el namespace de HTML.
Para SVG, el namespace debe especificarse explicitamente:

```javascript
const NS = 'http://www.w3.org/2000/svg';

// Crear un circulo SVG
const circulo = document.createElementNS(NS, 'circle');
circulo.setAttribute('cx', '50');
circulo.setAttribute('cy', '50');
circulo.setAttribute('r', '30');
circulo.setAttribute('fill', 'steelblue');

// Agregar al SVG
const svg = document.querySelector('svg');
svg.appendChild(circulo);
```

**Error comun**: usar `document.createElement('circle')` sin namespace
- Crea un elemento HTML desconocido, no un elemento SVG
- No se renderiza como circulo SVG

### Crear distintos tipos de elementos

```javascript
const NS = 'http://www.w3.org/2000/svg';

// Path
const path = document.createElementNS(NS, 'path');
path.setAttribute('d', 'M 10 10 L 90 90 Z');

// Texto
const texto = document.createElementNS(NS, 'text');
texto.setAttribute('x', '50');
texto.setAttribute('y', '50');
texto.textContent = 'Hola SVG';

// Grupo
const grupo = document.createElementNS(NS, 'g');
grupo.setAttribute('class', 'mi-grupo');
grupo.appendChild(circulo);
grupo.appendChild(texto);
svg.appendChild(grupo);
```

### Crear elementos SVG en el `<defs>`

```javascript
const NS = 'http://www.w3.org/2000/svg';
const defs = svg.querySelector('defs') || document.createElementNS(NS, 'defs');
if (!svg.querySelector('defs')) svg.insertBefore(defs, svg.firstChild);

// Crear un gradiente dinamicamente
const gradiente = document.createElementNS(NS, 'linearGradient');
gradiente.id = 'gradiente-dinamico';
// ... agregar stops ...
defs.appendChild(gradiente);
```

### Clonar elementos existentes

```javascript
// Clonar un elemento existente (true = clonar hijos tambien)
const original = document.querySelector('#mi-icono');
const clon = original.cloneNode(true);
clon.id = 'mi-icono-2';         // cambiar el id para evitar duplicados
clon.setAttribute('x', '100'); // mover el clon
svg.appendChild(clon);
```

---

## 21.4 Eventos en SVG

### Todos los eventos DOM funcionan

Los elementos SVG inline pueden recibir cualquier evento del DOM:

```javascript
const circulo = document.querySelector('circle');

// Click
circulo.addEventListener('click', (e) => {
  console.log('Click en el circulo');
  console.log(e.target);   // el elemento que recibio el click
});

// Mouse
circulo.addEventListener('mouseover', (e) => { /* ... */ });
circulo.addEventListener('mouseout', (e) => { /* ... */ });
circulo.addEventListener('mousemove', (e) => { /* ... */ });

// Teclado (el elemento debe ser focusable: tabindex="0")
circulo.setAttribute('tabindex', '0');
circulo.addEventListener('keydown', (e) => {
  if (e.key === 'Enter') { /* activar */ }
});
```

### Eventos de puntero (recomendados sobre mouse)

Los eventos de puntero (`pointer*`) unifican mouse, touch y pen en una sola API:
```javascript
elemento.addEventListener('pointerdown', (e) => { /* inicio de interaccion */ });
elemento.addEventListener('pointermove', (e) => { /* movimiento */ });
elemento.addEventListener('pointerup', (e) => { /* fin de interaccion */ });
elemento.addEventListener('pointercancel', (e) => { /* cancelacion */ });
```

### Propagacion y delegacion de eventos

Los eventos SVG se propagan (bubble) igual que en HTML:
```javascript
// Delegacion: un solo listener en el SVG para todos sus hijos
const svg = document.querySelector('svg');
svg.addEventListener('click', (e) => {
  const objetivo = e.target;
  if (objetivo.matches('circle')) {
    // click en un circulo
    const id = objetivo.id;
    const datos = objetivo.dataset.valor;
  }
  if (objetivo.closest('.barra')) {
    // click dentro de un grupo con clase "barra"
  }
});
```

**Delegacion de eventos en SVG**: patron especialmente util para graficos con muchos elementos del mismo tipo

---

## 21.5 Coordenadas del mouse en SVG

### El problema fundamental

- `event.clientX` y `event.clientY` son coordenadas del **viewport del navegador** (pixels CSS de pantalla)
- Los elementos SVG viven en el **espacio de coordenadas del viewBox**
- Si el SVG tiene `viewBox="0 0 100 100"` pero ocupa 500x500px en pantalla, las coordenadas son diferentes

### Conversion de coordenadas del mouse al espacio SVG

**Metodo moderno (usando DOMPoint y matrices)**
```javascript
function obtenerCoordenadas(evento, elemento) {
  const svg = elemento.ownerSVGElement || elemento;
  const punto = new DOMPoint(evento.clientX, evento.clientY);
  return punto.matrixTransform(svg.getScreenCTM().inverse());
}

svg.addEventListener('mousemove', (e) => {
  const coords = obtenerCoordenadas(e, svg);
  console.log(`SVG x: ${coords.x}, y: ${coords.y}`);
});
```

**Metodo legacy (compatible con navegadores antiguos)**
```javascript
function obtenerCoordenadas(evento, svg) {
  const punto = svg.createSVGPoint();
  punto.x = evento.clientX;
  punto.y = evento.clientY;
  return punto.matrixTransform(svg.getScreenCTM().inverse());
}
```

### Por que es necesaria la conversion

- El SVG puede estar escalado (el viewBox es de 100x100 pero ocupa 500x500px)
- El SVG puede estar posicionado con CSS (margin, padding, position)
- El SVG puede tener transformaciones aplicadas
- `getScreenCTM().inverse()` tiene en cuenta todo esto automaticamente

### Coordenadas relativas a un elemento especifico

```javascript
// Coordenadas relativas a un grupo transformado, no al SVG raiz
function coordsRelativasAlElemento(evento, elemento) {
  const punto = new DOMPoint(evento.clientX, evento.clientY);
  return punto.matrixTransform(elemento.getScreenCTM().inverse());
}
```

---

## 21.6 pointer-events en SVG

### Que controla

La propiedad `pointer-events` (como atributo SVG o propiedad CSS) determina **que partes del elemento responden a eventos del mouse/puntero**.

### Valores especificos de SVG

```xml
<circle pointer-events="visiblePainted" />  <!-- por defecto -->
<circle pointer-events="none" />            <!-- no responde a eventos -->
<circle pointer-events="all" />             <!-- responde en toda el area -->
```

| Valor | Responde donde |
|-------|---------------|
| `visiblePainted` | Zonas con fill o stroke visibles |
| `visibleFill` | Solo el area de fill, si es visible |
| `visibleStroke` | Solo el area de stroke, si es visible |
| `visible` | Fill + stroke, si `visibility` es visible |
| `painted` | Zonas con fill o stroke (aunque no sean visibles) |
| `fill` | El area de fill (sin importar visibilidad) |
| `stroke` | El area de stroke (sin importar visibilidad) |
| `all` | Fill + stroke siempre |
| `none` | Nunca |

### Casos de uso comunes

**Desactivar eventos en elementos superpuestos**
```css
.capa-decorativa { pointer-events: none; }
```
Un elemento encima de otro que no debe interceptar clicks.

**Area de click mas grande que el elemento visible**
```xml
<!-- Circulo visible pequenio con area de click mayor -->
<circle r="5" fill="red" />
<circle r="15" fill="transparent" pointer-events="all" />
```

**Activar eventos en zonas sin fill**
```xml
<!-- Un path con fill="none" que debe responder a clicks en su interior -->
<path fill="none" stroke="black" pointer-events="fill" />
```
Con `pointer-events="fill"`, el area interior del path responde aunque no tenga fill.

---

## 21.7 Drag and drop en SVG

### Implementacion basica

```javascript
let arrastrando = false;
let offsetX, offsetY;

elemento.addEventListener('pointerdown', (e) => {
  arrastrando = true;
  elemento.setPointerCapture(e.pointerId);  // capturar el puntero

  // Calcular el offset entre el puntero y el elemento
  const coords = obtenerCoordenadas(e, svg);
  const bbox = elemento.getBBox();
  offsetX = coords.x - bbox.x;
  offsetY = coords.y - bbox.y;
});

svg.addEventListener('pointermove', (e) => {
  if (!arrastrando) return;

  const coords = obtenerCoordenadas(e, svg);
  elemento.setAttribute('x', coords.x - offsetX);
  elemento.setAttribute('y', coords.y - offsetY);
});

svg.addEventListener('pointerup', () => {
  arrastrando = false;
});
```

### El metodo `setPointerCapture`

- Captura el puntero en el elemento: todos los eventos del puntero se envian a ese elemento aunque el cursor salga de el
- Critico para drag and drop: sin captura, si el usuario mueve el mouse rapido fuera del elemento, se pierden los eventos `pointermove`
- Se libera automaticamente al hacer `pointerup` o `pointercancel`

### Drag de circulos (con cx/cy en vez de x/y)

```javascript
// Para circulos, mover cx y cy en vez de x e y
elemento.addEventListener('pointerdown', (e) => {
  const coords = obtenerCoordenadas(e, svg);
  offsetX = coords.x - parseFloat(elemento.getAttribute('cx'));
  offsetY = coords.y - parseFloat(elemento.getAttribute('cy'));
  // ...
});

// En pointermove:
elemento.setAttribute('cx', coords.x - offsetX);
elemento.setAttribute('cy', coords.y - offsetY);
```

---

## 21.8 Metodos SVG DOM especificos

### `SVGPathElement.getTotalLength()`

```javascript
const path = document.querySelector('path');
const longitud = path.getTotalLength();
// Devuelve la longitud total del path en unidades de usuario (numero)
```

**Usos:**
- Calcular `stroke-dasharray` para line-drawing
- Distribuir elementos uniformemente a lo largo de un path
- Saber la "longitud" de un recorrido para calcular velocidades

### `SVGPathElement.getPointAtLength(distancia)`

```javascript
const path = document.querySelector('path');
const punto = path.getPointAtLength(50);  // punto a 50 unidades del inicio
punto.x;   // coordenada X en ese punto
punto.y;   // coordenada Y en ese punto
```

**Usos:**
- Posicionar elementos a lo largo de un path
- Calcular la tangente en un punto para rotar elementos
- Animar elementos a lo largo de un path con JavaScript

### `SVGGeometryElement.isPointInFill(punto)` y `isPointInStroke(punto)`

```javascript
const punto = svg.createSVGPoint();
punto.x = 50;
punto.y = 50;

const dentroDelFill = forma.isPointInFill(punto);     // true o false
const enElStroke = forma.isPointInStroke(punto);      // true o false
```

**Usos:**
- Hit testing: saber si un punto especifico esta dentro de una forma
- Util para formas no rectangulares donde el bounding box no es suficiente
- Deteccion de colisiones en juegos o visualizaciones interactivas

### `SVGSVGElement.createSVGPoint()` y `createSVGMatrix()`

```javascript
const svg = document.querySelector('svg');

// Crear un punto SVG
const punto = svg.createSVGPoint();
punto.x = 100;
punto.y = 50;

// Crear una matriz SVG
const matriz = svg.createSVGMatrix();
// SVGMatrix tiene propiedades a, b, c, d, e, f
// y metodos: translate(), scale(), rotate(), flipX(), flipY(), inverse()
```

**`createSVGPoint()` para conversion de coordenadas (metodo legacy):**
```javascript
const p = svg.createSVGPoint();
p.x = evento.clientX;
p.y = evento.clientY;
const svgCoords = p.matrixTransform(svg.getScreenCTM().inverse());
```

---

## 21.9 Scripts dentro de archivos SVG

### Como se incluyen

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200">
  <script>
    <![CDATA[
      // JavaScript dentro del SVG
      document.querySelector('circle').addEventListener('click', function() {
        this.setAttribute('fill', 'red');
      });
    ]]>
  </script>
  <circle cx="100" cy="100" r="50" fill="blue" />
</svg>
```

### Por que el `<![CDATA[]]>`

- El archivo `.svg` es XML: los caracteres `<`, `>`, `&` en el script deben escaparse
- `<![CDATA[...]]>` indica que el contenido es datos literales, no XML
- Dentro del CDATA se puede escribir JavaScript normalmente sin preocuparse por el escapado
- En SVG inline en HTML5, el CDATA no es necesario (el parser HTML lo maneja diferente)

### Cuando se ejecutan los scripts

**Se ejecutan cuando el SVG se carga como documento:**
- SVG inline en HTML → ✅ Se ejecutan
- SVG en `<object>` → ✅ Se ejecutan (en el contexto del SVG)
- SVG en `<iframe>` → ✅ Se ejecutan (en el contexto del iframe)

**NO se ejecutan:**
- SVG en `<img>` → ❌ Bloqueados por seguridad
- SVG como `background-image` CSS → ❌ Bloqueados por seguridad
- SVG en data URI en `<img>` → ❌ Bloqueados por seguridad

### Implicaciones de seguridad

- Los scripts en SVG pueden manipular el documento: son tan potentes como scripts en HTML
- Un SVG con scripts de origen desconocido incrustado inline puede ejecutar codigo malicioso (XSS)
- Los sanitizadores de SVG eliminan los elementos `<script>` y los atributos de evento (`onclick`, `onload`, etc.)
- Regla: nunca incrustar inline un SVG de origen no confiable sin sanitizarlo

### Atributos de evento en SVG

Ademas de `<script>`, los elementos SVG aceptan atributos de evento como `onclick`, `onmouseover`:
```xml
<circle onclick="cambiarColor(this)" fill="blue" />
```
- Funcionan igual que en HTML
- Son otro vector de XSS en SVGs de origen desconocido
- Los sanitizadores los eliminan
- En codigo propio: preferir `addEventListener` desde JavaScript externo (mejor separacion de responsabilidades)
