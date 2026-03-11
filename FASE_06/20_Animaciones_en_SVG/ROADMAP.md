# Seccion 20: Animaciones en SVG

> SVG tiene tres sistemas de animacion: SMIL (declarativo, nativo de SVG),
> CSS (el mas familiar para desarrolladores web) y JavaScript (el mas potente).
> Cada uno tiene su lugar segun la complejidad y el contexto.

---

## 20.1 Tipos de animacion disponibles

### Los tres sistemas y su filosofia

**SMIL (Synchronized Multimedia Integration Language)**
- Animaciones declaradas directamente en el SVG como elementos XML
- No requiere CSS ni JavaScript
- El SVG se anima solo con abrirlo
- Funciona incluso en `<img>` (el unico sistema que si funciona en ese contexto)
- Sintaxis XML, a veces verbosa pero muy expresiva para animaciones de path

**CSS Animations y Transitions**
- La misma sintaxis `@keyframes` y `transition` que se usa en HTML
- La forma mas familiar para desarrolladores web modernos
- Mejor soporte en DevTools (inspector de animaciones de Chrome)
- No funciona para animar el atributo `d` de paths (en la mayoria de navegadores)
- No funciona en SVG como `<img>`

**JavaScript**
- Control total y dinamico de cualquier propiedad
- Puede reaccionar a datos externos, eventos del usuario, tiempo real
- Usa `requestAnimationFrame` para animaciones de alto rendimiento
- O la Web Animations API para animaciones declarativas desde JS
- O librerias como GSAP, Anime.js, Motion One

### Tabla de capacidades por sistema

| Capacidad | SMIL | CSS | JavaScript |
|-----------|------|-----|------------|
| Animar `d` (morphing de paths) | ✅ | ❌ (mayoria) | ✅ |
| Funciona en `<img>` | ✅ | ❌ | ❌ |
| Reaccionar a datos | ❌ | ❌ | ✅ |
| Sincronizacion con otras animaciones | ✅ (declarativo) | Limitado | ✅ |
| Inspector DevTools | Limitado | ✅ | ✅ |
| Reduccion de movimiento (`prefers-reduced-motion`) | Manual | ✅ (nativo) | Manual |
| Rendimiento GPU | Limitado | ✅ (transform, opacity) | ✅ (Web Animations API) |

---

## 20.2 Animacion SMIL con `<animate>`

### Como se declara

El elemento `<animate>` se coloca **como hijo del elemento a animar**:
```xml
<circle cx="50" cy="50" r="20" fill="steelblue">
  <animate
    attributeName="cx"
    from="10"
    to="90"
    dur="2s"
    repeatCount="indefinite"
  />
</circle>
```

El circulo se mueve horizontalmente de x=10 a x=90, en 2 segundos, indefinidamente.

### Atributos de `<animate>` en detalle

**`attributeName` — Que propiedad animar**
- El nombre del atributo SVG a animar: `cx`, `cy`, `r`, `fill`, `opacity`, `stroke-width`, `d`, etc.
- Case-sensitive: debe coincidir exactamente con el nombre del atributo
- Se puede animar cualquier atributo animable de SVG

**`from` y `to` — Valor inicial y final**
- `from`: valor al inicio de la animacion
- `to`: valor al final de la animacion
- Si se omite `from`: comienza desde el valor actual del atributo

**`values` — Secuencia de valores**
- Alternativa a `from`/`to` para animaciones con multiples pasos
- `values="10; 50; 90; 50; 10"`: anima a traves de estos cinco valores en orden
- Si se usa `values`, se ignoran `from` y `to`
- Los valores se distribuyen uniformemente en el tiempo, a menos que se use `keyTimes`

**`dur` — Duracion**
- Duracion de un ciclo completo de la animacion
- Formato: `"2s"` (segundos), `"500ms"` (milisegundos), `"00:02"` (mm:ss)
- Tambien acepta `"indefinite"` (duracion ilimitada, solo util con `repeatCount`)

**`begin` — Cuando comienza**
- `"0s"` o `"0"` (por defecto): comienza inmediatamente al cargar el SVG
- `"2s"`: comienza 2 segundos despues de cargar
- `"click"`: comienza cuando el elemento recibe un click
- `"mouseover"`: comienza al pasar el mouse
- `"otroId.begin"`: comienza cuando otra animacion comienza
- `"otroId.end"`: comienza cuando otra animacion termina
- `"otroId.end + 1s"`: comienza 1 segundo despues de que otra termine
- `"indefinite"`: solo comienza cuando se llama a `beginElement()` desde JavaScript

**`end` — Cuando termina**
- Similar a `begin` pero para el fin de la animacion
- `"5s"`: la animacion termina a los 5 segundos aunque `dur` sea mayor

**`repeatCount` — Veces que se repite**
- Un numero entero: `"3"` (se repite 3 veces)
- `"indefinite"`: se repite para siempre

**`repeatDur` — Duracion total de repeticion**
- Alternativa a `repeatCount`: define el tiempo total de repeticion
- `"10s"`: la animacion se repite durante 10 segundos en total

**`fill` — Estado al terminar (no confundir con el fill de color)**
- `"remove"` (por defecto): al terminar, el atributo vuelve a su valor original
- `"freeze"`: al terminar, el atributo se queda en el valor final

**`keyTimes` — Tiempos de los valores clave**
- Lista de tiempos normalizados (0 a 1) que corresponden a cada valor en `values`
- `values="0; 50; 100"` con `keyTimes="0; 0.8; 1"`: el movimiento de 0 a 50 ocupa el 80% del tiempo, el de 50 a 100 solo el 20%
- Debe tener el mismo numero de elementos que `values`
- El primer valor debe ser `0`, el ultimo `1`

**`keySplines` — Curvas de aceleracion por tramo**
- Define la curva de bezier para cada tramo entre valores clave
- Cada curva se define con 4 valores: `x1 y1 x2 y2` (puntos de control de la curva)
- Tiene un elemento menos que `values` (define las curvas entre valores, no los valores en si)
- Solo se usa cuando `calcMode="spline"`

**`calcMode` — Modo de calculo de la interpolacion**
- `"linear"` (por defecto para la mayoria): interpolacion lineal entre valores
- `"discrete"`: saltos instantaneos entre valores (sin interpolacion suave)
- `"paced"`: velocidad constante independientemente de la distancia entre valores
- `"spline"`: usa `keySplines` para la curva de aceleracion entre cada par de valores

**`additive` — Como se combina con el valor actual**
- `"replace"` (por defecto): la animacion reemplaza el valor del atributo
- `"sum"`: la animacion se suma al valor del atributo (permite componer animaciones)

**`accumulate` — Acumulacion entre repeticiones**
- `"none"` (por defecto): cada repeticion empieza desde el valor inicial
- `"sum"`: cada repeticion comienza donde termino la anterior (el valor se acumula)

---

## 20.3 Animacion de movimiento `<animateMotion>`

### Que hace
- Mueve un elemento a lo largo de un **trazado** (path)
- El elemento se desplaza siguiendo la curva del path automaticamente
- Mas potente que animar `cx`/`cy` individualmente porque sigue cualquier curva

### Atributos especificos

**`path`**
- El trazado que seguira el elemento, en la misma sintaxis que el atributo `d` de `<path>`
- El elemento se mueve desde el inicio hasta el final del path durante la animacion

**`rotate`**
- `"auto"`: el elemento se rota para alinearse con la tangente del path (sigue la direccion de la curva)
- `"auto-reverse"`: igual que `auto` pero rotado 180° (para cuando el elemento "mira hacia atras")
- Un angulo fijo: `rotate="45"` — el elemento siempre tiene esa inclinacion

**`keyPoints`**
- Lista de valores (0-1) que indican la posicion en el path para cada `keyTime`
- Permite controlar la velocidad del movimiento a lo largo del path
- Si se usa, debe tener el mismo numero de elementos que `keyTimes`

**Sub-elemento `<mpath>`**
- Alternativa a definir `path` directamente en `<animateMotion>`
- Referencia a un `<path>` existente en el documento:
  ```xml
  <animateMotion dur="3s" repeatCount="indefinite">
    <mpath href="#mi-trayectoria" />
  </animateMotion>
  ```
- Ventaja: el mismo path puede usarse visualmente y como trayectoria de animacion

---

## 20.4 Animacion de transformaciones `<animateTransform>`

### Por que existe ademas de `<animate>`

- `<animate>` puede animar el atributo `transform` pero producira resultados incorrectos para transformaciones compuestas
- `<animateTransform>` esta especificamente diseñado para animar transformaciones SVG
- Entiende la sintaxis de `translate`, `rotate`, `scale`, `skewX`, `skewY`

### Atributo `type`

Define el tipo de transformacion a animar:
- `"translate"`: anima la traslacion
- `"rotate"`: anima la rotacion
- `"scale"`: anima la escala
- `"skewX"`: anima el sesgo horizontal
- `"skewY"`: anima el sesgo vertical

### Sintaxis de `from`/`to`/`values`

Los valores tienen la sintaxis de la transformacion correspondiente:
```xml
<!-- Rotacion de 0 a 360 grados, alrededor del centro (50,50) -->
<animateTransform
  attributeName="transform"
  type="rotate"
  from="0 50 50"
  to="360 50 50"
  dur="3s"
  repeatCount="indefinite"
/>

<!-- Traslacion de izquierda a derecha -->
<animateTransform
  attributeName="transform"
  type="translate"
  from="0 0"
  to="100 0"
  dur="2s"
/>
```

### El atributo `additive="sum"` para componer transformaciones

Por defecto, multiples `<animateTransform>` se reemplazan entre si (solo el ultimo tiene efecto).
Con `additive="sum"`, se combinan:
```xml
<!-- Estas dos animaciones se combinan: el elemento rota Y se traslada -->
<animateTransform type="rotate" from="0" to="360" ... additive="sum" />
<animateTransform type="translate" from="0 0" to="50 0" ... additive="sum" />
```

---

## 20.5 `<set>`

### Que hace
- Cambia el valor de un atributo en un momento especifico **sin interpolacion** (salto directo)
- Util para cambios discretos: mostrar/ocultar, cambiar clase, cambiar color de golpe

### Diferencia con `<animate>`
- `<animate>` interpola: genera todos los valores intermedios entre `from` y `to`
- `<set>` no interpola: el atributo salta directamente al valor de `to` en el momento `begin`

### Casos de uso

**Mostrar/ocultar elementos**
```xml
<rect visibility="hidden">
  <set attributeName="visibility" to="visible" begin="2s" dur="3s" fill="remove" />
</rect>
```
El rectangulo es invisible, aparece a los 2s, y desaparece a los 5s.

**Cambiar color en un momento especifico**
```xml
<circle fill="blue">
  <set attributeName="fill" to="red" begin="click" />
</circle>
```
Al hacer click, el circulo cambia de azul a rojo instantaneamente.

---

## 20.6 Sincronizacion de animaciones SMIL

### Sistema de tiempo SMIL

Cada elemento con `begin` participa en el sistema de tiempo de SMIL:
- Las animaciones pueden referenciarse entre si por su `id`
- Se pueden crear cadenas y solapamientos de animaciones

### Referencias de tiempo entre animaciones

```xml
<rect id="caja">
  <animate id="anim1" attributeName="x" from="0" to="100" dur="1s" begin="0s" />
  <animate id="anim2" attributeName="y" from="0" to="50" dur="0.5s" begin="anim1.end" />
  <animate id="anim3" attributeName="width" from="100" to="200" dur="1s" begin="anim1.begin + 0.3s" />
</rect>
```

- `anim2` comienza cuando termina `anim1`
- `anim3` comienza 0.3s despues de que empieza `anim1` (se superponen)

### Eventos como disparadores de `begin`

```xml
<circle>
  <!-- Comienza al hacer click en el circulo -->
  <animate begin="click" ... />

  <!-- Comienza al pasar el mouse por encima -->
  <animate begin="mouseover" ... />

  <!-- Comienza al pasar el mouse AND a los 5s -->
  <animate begin="mouseover; 5s" ... />
</circle>
```

### Control desde JavaScript

Las animaciones SMIL pueden controlarse desde JavaScript:
- `animElement.beginElement()` — inicia la animacion (util cuando `begin="indefinite"`)
- `animElement.endElement()` — termina la animacion
- `animElement.getCurrentTime()` — tiempo actual de la animacion
- Estas APIs permiten integrar SMIL con logica JavaScript

---

## 20.7 Animaciones CSS en SVG

### Que propiedades SVG son animables con CSS

**Animables con buen soporte**
- `fill`, `stroke`, `stroke-width`, `stroke-opacity`
- `opacity`
- `transform` (con `transform-origin`)
- `stroke-dasharray`, `stroke-dashoffset` (clave para line-drawing)
- `clip-path` (formas basicas)
- `filter`

**Animables en SVG 2 (soporte creciente)**
- `r` (radio de circulos)
- `cx`, `cy` (centro de circulos y elipses)
- `x`, `y`, `width`, `height` (de rectangulos)

**No animables con CSS (en la mayoria de navegadores)**
- El atributo `d` de `<path>` (morphing) — para esto se necesita JavaScript o SMIL

### CSS Transitions en SVG

```css
circle {
  fill: steelblue;
  transition: fill 0.3s ease, r 0.3s ease;
}

circle:hover {
  fill: #e74c3c;
  r: 45;  /* Solo SVG 2 */
}
```

**La transicion mas usada: hover de color**
```css
.boton-svg path {
  fill: #005ea5;
  transition: fill 0.2s ease;
}
.boton-svg:hover path {
  fill: #003d7a;
}
```

### CSS @keyframes en SVG

```css
@keyframes pulso {
  0%, 100% { r: 30; opacity: 1; }
  50%       { r: 40; opacity: 0.7; }
}

.indicador {
  animation: pulso 1.5s ease-in-out infinite;
}
```

### `prefers-reduced-motion` — Accesibilidad en animaciones

**Por que es importante**
- Algunas personas tienen vestibulares sensibles o epilepsia: las animaciones pueden causarles malestar fisico
- El sistema operativo ofrece una opcion "reducir movimiento" (`prefers-reduced-motion: reduce`)
- CSS puede y debe respetar esta preferencia

**Patron recomendado**
```css
/* Animacion por defecto */
.spinner {
  animation: rotar 1s linear infinite;
}

/* Desactivar o suavizar si el usuario lo prefiere */
@media (prefers-reduced-motion: reduce) {
  .spinner {
    animation: none;
  }
  /* O una alternativa mas sutil */
  .spinner {
    animation-duration: 3s;
  }
}
```

---

## 20.8 Tecnicas de animacion comunes

### Line drawing (dibujo de linea)

La animacion que hace que un path "se dibuje" progresivamente:

**El principio**
1. `stroke-dasharray`: define el patron de guiones. Si se pone un valor igual a la longitud total del path, el path aparece como un solo "trazo"
2. `stroke-dashoffset`: desplaza ese trazo. Si el offset = longitud total, el trazo esta completamente desplazado (invisible). Si offset = 0, el trazo es completamente visible
3. Animar `stroke-dashoffset` de la longitud total a 0: el path "aparece" de un extremo al otro

**Como obtener la longitud del path**
- JavaScript: `pathElement.getTotalLength()` devuelve la longitud en unidades de usuario
- Se usa para establecer el valor inicial de `stroke-dasharray` y `stroke-dashoffset`

**Con CSS**
```css
.mi-path {
  stroke-dasharray: 500;   /* longitud aproximada o exacta del path */
  stroke-dashoffset: 500;
  animation: dibujar 2s ease forwards;
}

@keyframes dibujar {
  to { stroke-dashoffset: 0; }
}
```

**Consideraciones**
- La longitud del path cambia si el viewBox o las transformaciones cambian
- Para un valor exacto: calcular con `getTotalLength()` en JavaScript y aplicarlo como estilo inline

### Morphing de formas

Animar el atributo `d` para transformar un path en otro:

**Requisito critico**: los dos paths deben tener **exactamente el mismo numero de comandos y puntos**
- Un triangulo (3 puntos) puede morphear a un cuadrado (4 puntos si se repite uno)
- Si los numeros no coinciden: la animacion no funcionara o producira resultados erraticos

**Con SMIL**
```xml
<path d="M 10 80 Q 50 10 90 80">
  <animate
    attributeName="d"
    values="M 10 80 Q 50 10 90 80; M 10 80 Q 50 150 90 80"
    dur="1s"
    repeatCount="indefinite"
  />
</path>
```

**Con JavaScript / GSAP**
- Librerias como GSAP con el plugin MorphSVG manejan automaticamente la compatibilidad entre paths
- Pueden interpolar paths con diferente numero de puntos (con algoritmos de igualacion)

### Spinner / loader rotativo

```css
@keyframes rotar {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

.spinner {
  transform-origin: center;
  animation: rotar 1s linear infinite;
}
```

**Truco para spinner con `stroke-dashoffset`**
- Un circulo con `stroke-dasharray` parcial que rota: efecto de "arco giratorio"
- Mas elegante que un circulo completo rotando

### Progress bar circular

```
stroke-dasharray = circunferencia_total
stroke-dashoffset = circunferencia_total * (1 - porcentaje/100)
```

- La circunferencia de un circulo = `2 * PI * r`
- Con `stroke-dashoffset` al 100%: barra vacia
- Con `stroke-dashoffset` al 0%: barra llena
- Animar el `stroke-dashoffset` para mostrar el progreso

---

## 20.9 Estado actual de SMIL

### Historia del soporte

- **2015**: Chrome anuncio la deprecacion de SMIL, mostrando advertencias en consola
- **2016**: Chrome revirtio la decision: SMIL seguira siendo soportado indefinidamente
- **Hoy**: SMIL esta soportado en todos los navegadores modernos (Chrome, Firefox, Safari, Edge)

### SMIL vs CSS vs JS: la eleccion en 2024

**Elegir SMIL cuando**
- El SVG se usa en `<img>` y necesita animarse (SMIL es el unico que funciona)
- La animacion debe funcionar sin JavaScript y sin CSS externo
- Animacion de `d` (morphing de paths): SMIL es la forma mas sencilla
- SVG que se distribuye como archivo standalone autocontenido

**Elegir CSS cuando**
- El SVG es inline en HTML y el equipo conoce bien CSS
- Se necesita `transition` para efectos hover (la forma mas sencilla)
- Se quiere usar `prefers-reduced-motion` de forma nativa
- Las animaciones son de propiedades que CSS puede animar (fill, opacity, transform)

**Elegir JavaScript cuando**
- Las animaciones responden a datos externos o eventos complejos
- Se necesita sincronizacion precisa entre multiples animaciones
- Se usa una libreria como GSAP para mayor control
- El morphing de paths necesita compatibilidad de puntos automatica
