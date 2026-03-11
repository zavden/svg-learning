# FASE 6: Dinamismo
> Secciones 20-21 del Roadmap SVG Basico-Intermedio
> Animaciones (SMIL, CSS, JS) e interactividad con JavaScript.

---

## 20. Animaciones en SVG

### 20.1 Tipos de animacion disponibles
1. **SMIL (SVG Animation)**: nativa de SVG, declarativa
2. **CSS Animations/Transitions**: usando @keyframes y transition
3. **JavaScript**: usando requestAnimationFrame, Web Animations API o librerias
4. Cada tipo tiene sus ventajas y limitaciones

### 20.2 Animacion SMIL con `<animate>`
- Se coloca como hijo del elemento a animar
- Atributos:
  - `attributeName`: la propiedad a animar
  - `from`: valor inicial
  - `to`: valor final
  - `values`: lista de valores intermedios (alternativa a from/to)
  - `dur`: duracion (ej: "2s", "500ms")
  - `begin`: cuando comienza (ej: "0s", "click", "mouseover", "otroId.end")
  - `end`: cuando termina
  - `repeatCount`: veces que se repite ("indefinite" para infinito)
  - `repeatDur`: duracion total de repeticion
  - `fill`: que pasa al terminar ("freeze" = mantener, "remove" = volver al inicio)
  - `keyTimes`: tiempos clave para los valores
  - `keySplines`: curvas de bezier para interpolacion
  - `calcMode`: modo de calculo (linear, discrete, paced, spline)
  - `additive`: sum o replace
  - `accumulate`: sum o none

### 20.3 Animacion de movimiento `<animateMotion>`
- Mueve un elemento a lo largo de un trazado
- Atributos:
  - `path`: el trazado d a seguir
  - `rotate`: auto (el elemento rota siguiendo el path), auto-reverse, o un angulo fijo
  - `keyPoints`: puntos clave a lo largo del path
- Sub-elemento `<mpath>`: referencia un path existente en el documento
- Otros atributos heredados de `<animate>` (dur, begin, repeatCount, etc.)

### 20.4 Animacion de transformaciones `<animateTransform>`
- Anima transformaciones (translate, scale, rotate, skewX, skewY)
- Atributo `type`: el tipo de transformacion a animar
- Atributos from/to/values con la sintaxis de la transformacion correspondiente
- Solo una animateTransform por tipo se aplica a la vez (por defecto se reemplazan)
- `additive="sum"` para combinar multiples transformaciones animadas

### 20.5 `<set>`
- Establece un valor de atributo en un momento dado (sin interpolacion)
- Util para cambios discretos (mostrar/ocultar, cambiar color de golpe)
- Atributos: `attributeName`, `to`, `begin`, `dur`

### 20.6 Sincronizacion de animaciones SMIL
- `begin="otroId.begin"`: comienza cuando otra animacion comienza
- `begin="otroId.end"`: comienza cuando otra animacion termina
- `begin="otroId.end + 1s"`: comienza 1 segundo despues de que otra termine
- `begin="click"`: comienza al hacer click
- `begin="mouseover"`: comienza al pasar el mouse
- `begin="2s;click"`: comienza a los 2s Y al hacer click
- Eventos de teclado y otros eventos DOM

### 20.7 Animaciones CSS en SVG
- `transition` en propiedades SVG: fill, stroke, opacity, transform, etc.
- `@keyframes` con propiedades SVG
- Propiedades animables con CSS: fill, stroke, stroke-width, stroke-dasharray, stroke-dashoffset, opacity, transform, r (radio en SVG 2), cx, cy, x, y, width, height
- Ventajas sobre SMIL: sintaxis mas familiar, mejor rendimiento en algunos casos, mas herramientas de desarrollo

### 20.8 Tecnicas de animacion comunes
- **Line drawing (dibujo de linea)**:
  - Usa stroke-dasharray y stroke-dashoffset
  - Se calcula la longitud total del path con `getTotalLength()`
  - Se anima dashoffset desde la longitud total hasta 0
- **Morphing de formas**: animar el atributo `d` del path (los paths deben tener el mismo numero de puntos)
- **Pulsar/latir**: escalar un elemento repetidamente
- **Rotar**: spin continuo con rotate transform
- **Fade in/out**: animar opacity
- **Color transitions**: animar fill o stroke entre colores

### 20.9 Estado actual de SMIL
- Chrome depreco SMIL pero revirtio la decision: SMIL sigue soportado
- Soporte en todos los navegadores modernos
- SVG 2 esta refinando SMIL pero no lo elimina
- Para proyectos nuevos: CSS o JS suelen ser preferidos, pero SMIL sigue siendo valido

---

## 21. Interactividad con JavaScript

### 21.1 Acceso al DOM SVG
- Los elementos SVG son parte del DOM cuando son inline
- `document.querySelector('circle')`, `document.getElementById('miCirculo')`
- `document.querySelectorAll('.clase')` funciona igual que en HTML
- Los elementos SVG son instancias de `SVGElement` (hereda de `Element`)

### 21.2 Manipulacion de atributos
- `element.getAttribute('cx')` y `element.setAttribute('cx', '100')`
- `element.style.fill = 'red'` para propiedades CSS
- Propiedades SVG especificas: `element.cx.baseVal.value = 100` (interfaz SVGAnimatedLength)
- `element.classList.add()`, `.remove()`, `.toggle()` para clases CSS
- `element.getBBox()`: obtiene el bounding box del elemento (x, y, width, height)
- `element.getCTM()`: obtiene la Current Transformation Matrix
- `element.getScreenCTM()`: CTM relativa a la pantalla

### 21.3 Creacion dinamica de elementos SVG
- Se debe usar `document.createElementNS()` con el namespace SVG
- Namespace SVG: `"http://www.w3.org/2000/svg"`
- Ejemplo: `document.createElementNS("http://www.w3.org/2000/svg", "circle")`
- Usar `setAttribute()` para establecer atributos
- Agregar al DOM con `appendChild()` o `insertBefore()`

### 21.4 Eventos en SVG
- Todos los eventos DOM funcionan: click, mousedown, mouseup, mousemove, mouseover, mouseout, mouseenter, mouseleave
- Eventos tactiles: touchstart, touchmove, touchend
- Eventos de puntero (recomendados): pointerdown, pointermove, pointerup, pointerover, pointerout
- Registro de eventos: `element.addEventListener('click', handler)`
- El objeto `event` contiene coordenadas, target, etc.

### 21.5 Coordenadas del mouse en SVG
- `event.clientX/clientY`: coordenadas del viewport del navegador
- Conversion a coordenadas SVG:
  ```javascript
  const svg = document.querySelector('svg');
  const pt = svg.createSVGPoint();
  pt.x = event.clientX;
  pt.y = event.clientY;
  const svgPoint = pt.matrixTransform(svg.getScreenCTM().inverse());
  ```
- `svgPoint.x`, `svgPoint.y`: coordenadas en el espacio del SVG

### 21.6 pointer-events en SVG
- Controla que partes del elemento responden a eventos del mouse/puntero
- Valores:
  - `visiblePainted` (por defecto): responde en areas con fill/stroke visible
  - `visibleFill`: responde en el area de fill visible
  - `visibleStroke`: responde en el area de stroke visible
  - `visible`: responde en fill + stroke si visibility es visible
  - `painted`: responde en areas con fill/stroke (aunque no sea visible)
  - `fill`: responde en el area de fill
  - `stroke`: responde en el area de stroke
  - `all`: responde en toda el area de fill + stroke
  - `none`: no responde a eventos
- Uso comun: `pointer-events="none"` para que un elemento no intercepte clicks

### 21.7 Drag and drop en SVG
- Capturar mousedown/pointerdown en el elemento
- Calcular offset entre la posicion del mouse y la posicion del elemento
- En mousemove/pointermove: actualizar la posicion del elemento
- En mouseup/pointerup: dejar de mover
- Consideraciones con transformaciones y viewBox

### 21.8 Metodos SVG DOM especificos
- `SVGPathElement.getTotalLength()`: longitud total de un path
- `SVGPathElement.getPointAtLength(distance)`: coordenadas en un punto del path
- `SVGGeometryElement.isPointInFill(point)`: si un punto esta dentro del fill
- `SVGGeometryElement.isPointInStroke(point)`: si un punto esta en el stroke
- `SVGSVGElement.createSVGPoint()`: crea un objeto SVGPoint
- `SVGSVGElement.createSVGMatrix()`: crea un objeto SVGMatrix

### 21.9 Scripts dentro de archivos SVG
- Se puede incluir `<script>` dentro del SVG
- Usa `<![CDATA[ ... ]]>` para evitar conflictos con XML
- Solo se ejecuta cuando el SVG se carga como documento (inline, object, iframe)
- No se ejecuta cuando se usa como `<img>` o `background-image` (por seguridad)
