# Seccion 11: Transformaciones

> Las transformaciones modifican el sistema de coordenadas local de un elemento.
> Entender que transforman el SISTEMA DE COORDENADAS (no el elemento en si)
> es la clave para no perderse con el orden y la composicion.

---

## 11.1 El atributo `transform`

### Naturaleza de las transformaciones SVG
- Las transformaciones no mueven el elemento: **modifican el sistema de coordenadas** en el que vive
- Es como mover y rotar la "hoja de papel" sobre la que esta dibujado el elemento
- El elemento sigue en las mismas coordenadas relativas a su sistema, pero ese sistema cambio
- Esta distincion es crucial para entender efectos como "rotar alrededor del centro"

### Donde se puede aplicar
- En cualquier **elemento grafico**: `<rect>`, `<circle>`, `<path>`, `<text>`, etc.
- En **grupos** `<g>`: la transformacion afecta a todos los hijos del grupo
- En `<use>`, `<image>`, `<pattern>`, `<linearGradient>` (con `gradientTransform`), etc.
- No en `<defs>` directamente (no tiene efecto visual, aunque es tecnicamente valido)

### Multiples transformaciones en un solo atributo
- Se escriben separadas por espacios: `transform="translate(50,50) rotate(45) scale(2)"`
- **Orden de aplicacion: de derecha a izquierda** (la ultima escrita se aplica primero al sistema de coordenadas)
- En el ejemplo anterior: primero se aplica `scale(2)`, luego `rotate(45)`, luego `translate(50,50)`
- Dicho de otra forma: si se lee de izquierda a derecha, se ven las operaciones en el orden en que afectan al elemento visualmente

### La herencia de transformaciones
- Los elementos hijos **heredan** el sistema de coordenadas transformado del padre
- Un `<g transform="translate(100,100)">` hace que todos sus hijos tengan (100,100) como nuevo origen
- Las transformaciones se **acumulan**: hijo dentro de grupo transformado dentro de otro grupo transformado = composicion de todas las matrices

---

## 11.2 translate(tx, ty)

### Que hace
- Desplaza el sistema de coordenadas `tx` unidades en X y `ty` unidades en Y
- Todo lo que este en ese sistema de coordenadas se "mueve" visualmente

### Sintaxis
- `translate(tx, ty)` — con los dos valores
- `translate(tx)` — si solo se da un valor, `ty = 0` (desplazamiento solo horizontal)

### Diferencia entre `translate` y cambiar `x`/`y` directamente

**Cambiar `x`/`y` del elemento**
- Mueve el elemento dentro de su sistema de coordenadas actual
- Solo aplica a elementos que tienen `x`/`y` (no todos los tienen: `<circle>` usa `cx`/`cy`)
- No afecta a los hijos del elemento

**`translate` en el `transform`**
- Desplaza el sistema de coordenadas completo
- Aplica a cualquier elemento (incluso los que no tienen x/y)
- Afecta a todos los hijos si se aplica a un grupo
- Se compone con otras transformaciones en el mismo atributo

### Uso principal de `translate`
- Posicionar grupos sin cambiar las coordenadas internas de sus elementos
- "Pivotear" transformaciones: mover al punto de rotacion, rotar, volver al lugar original (ver seccion 11.4)
- Crear animaciones de movimiento que se combinen con otras transformaciones

### Valores negativos
- `translate(-50, 0)`: desplazamiento hacia la izquierda
- `translate(0, -30)`: desplazamiento hacia arriba
- `translate(-cx, -cy)`: patron para mover el origen al centro de un elemento

---

## 11.3 scale(sx, sy)

### Que hace
- Escala el sistema de coordenadas: estira o comprime el espacio
- Todo lo que este en ese espacio se ve mas grande o mas pequenio

### Sintaxis
- `scale(sx, sy)` — escala horizontal y vertical independientes
- `scale(s)` — un solo valor: `sy = sx` (escala uniforme en ambas dimensiones)

### El origen de la escala: punto critico
- La escala se aplica **desde el origen del sistema de coordenadas** (el punto 0,0)
- Un `scale(2)` hace que todo sea el doble de grande, pero tambien **el doble de lejos del origen**
- Un rectangulo en `x=100, y=100` con `scale(2)`: el rectangulo ahora esta en `x=200, y=200` visualmente
- Esto sorprende a los principiantes que esperan que el elemento escale desde su propio centro

### Escalar desde el centro de un elemento
Para escalar un elemento desde su propio centro (cx, cy):
1. `translate(cx, cy)` — mover el origen al centro del elemento
2. `scale(s)` — escalar desde ese nuevo origen
3. `translate(-cx, -cy)` — devolver el origen a su posicion original

Resultado: `transform="translate(cx,cy) scale(s) translate(-cx,-cy)"`

En SVG 2 y con CSS `transform-origin`, esto se simplifica (ver seccion 11.8).

### Valores negativos: reflexion (espejo)
- `scale(-1, 1)`: espejo horizontal (reflejo en eje Y)
- `scale(1, -1)`: espejo vertical (reflejo en eje X)
- `scale(-1, -1)`: rotacion de 180° (equivalente a rotate(180))
- Muy util para reutilizar formas asimetricas (una flecha apuntando derecha e izquierda desde el mismo path)

### Escala no uniforme: distorsion
- `scale(2, 0.5)`: el doble de ancho, la mitad de alto
- Los circulos se convierten en elipses
- Los cuadrados se convierten en rectangulos
- Afecta tambien al `stroke-width`: el trazo se distorsiona proportionalmente

---

## 11.4 rotate(angle, cx, cy)

### Que hace
- Rota el sistema de coordenadas `angle` grados **en sentido horario**
- En SVG, los angulos positivos van en sentido horario (porque el eje Y esta invertido)
- Angulos negativos: sentido antihorario

### Sintaxis
- `rotate(angle)` — rota alrededor del origen (0,0)
- `rotate(angle, cx, cy)` — rota alrededor del punto (cx, cy)

### Rotar alrededor del origen (0,0)
- Con `rotate(angle)` solo: el elemento rota pero puede moverse lejos de su posicion original
- Un rectangulo que no esta en (0,0) "orbita" alrededor del origen al rotarlo
- Rara vez es el comportamiento deseado para UI e ilustraciones

### Rotar alrededor de un punto especifico
- `rotate(angle, cx, cy)` es equivalente a:
  `translate(cx, cy) rotate(angle) translate(-cx, -cy)`
- El punto (cx, cy) actua como "pivote" de la rotacion
- El elemento gira sobre si mismo sin desplazarse (si (cx,cy) es el centro del elemento)

### Calcular el centro para rotar "en su lugar"
- Para un `<rect x="x" y="y" width="w" height="h">`:
  - Centro: `cx = x + w/2`, `cy = y + h/2`
  - `rotate(45, x+w/2, y+h/2)`
- Para un `<circle cx="50" cy="50" r="30">`:
  - Centro: ya dado por `cx` y `cy`
  - `rotate(45, 50, 50)`
- Para grupos: calcular el bounding box del grupo y usar su centro

### Angulos comunes y sus efectos
| Angulo | Efecto |
|--------|--------|
| `0` | Sin rotacion |
| `45` | Diagonal (arriba-derecha a abajo-izquierda) |
| `90` | 90° horario (abajo se convierte en derecha) |
| `180` | Cabeza abajo |
| `270` o `-90` | 90° antihorario |
| `360` | Vuelta completa (igual que 0) |

---

## 11.5 skewX(angle) y skewY(angle)

### Que hace `skewX`
- Inclina el sistema de coordenadas en el **eje X**
- Los puntos se desplazan horizontalmente en proporcion a su coordenada Y
- `skewX(20)`: por cada unidad de Y, el punto se desplaza `tan(20°) ≈ 0.36` unidades en X
- Efecto visual: los elementos "se inclinan" como si estuvieran en perspectiva lateral

### Que hace `skewY`
- Inclina el sistema de coordenadas en el **eje Y**
- Los puntos se desplazan verticalmente en proporcion a su coordenada X
- `skewY(20)`: por cada unidad de X, el punto se desplaza `tan(20°) ≈ 0.36` unidades en Y

### Casos de uso
- Crear efectos de perspectiva isometrica simplificada
- Sombras de texto en perspectiva (combinando `skewX` con `scale`)
- Formas de paralelogramo desde un rectangulo normal
- Efectos de "italica" manual en elementos que no tienen variante italic

### Angulos y efectos visuales
- Angulos pequenios (5°-15°): inclinacion sutil, efecto de perspectiva leve
- Angulos medios (15°-45°): deformacion visible y notable
- Angulos mayores de 45°: deformacion extrema, el elemento "se tumba"
- `skewX(90)`: el elemento desaparece (queda completamente plano en horizontal)

---

## 11.6 matrix(a, b, c, d, e, f)

### Que es una matriz de transformacion
- Todas las transformaciones SVG son internamente **matrices de transformacion 3x3**
- `matrix(a, b, c, d, e, f)` permite especificar cualquier transformacion directamente en formato de matriz
- La matriz completa es:
  ```
  | a  c  e |
  | b  d  f |
  | 0  0  1 |
  ```

### Los seis parametros y su significado
- `a`: escala en X y componente de rotacion
- `b`: componente de rotacion y skewY
- `c`: componente de rotacion y skewX
- `d`: escala en Y y componente de rotacion
- `e`: traslacion en X
- `f`: traslacion en Y

### Equivalencias con las transformaciones individuales

| Transformacion | Equivalente en matrix |
|---------------|----------------------|
| `translate(tx, ty)` | `matrix(1, 0, 0, 1, tx, ty)` |
| `scale(sx, sy)` | `matrix(sx, 0, 0, sy, 0, 0)` |
| `rotate(a)` | `matrix(cos(a), sin(a), -sin(a), cos(a), 0, 0)` |
| `skewX(a)` | `matrix(1, 0, tan(a), 1, 0, 0)` |
| `skewY(a)` | `matrix(1, tan(a), 0, 1, 0, 0)` |

### Cuando usar `matrix` directamente
- Cuando se calcula la transformacion programaticamente (JavaScript devuelve matrices)
- Cuando se quiere aplicar una transformacion compuesta exacta en un solo paso (mejor rendimiento)
- Al leer el `CTM` (Current Transformation Matrix) del DOM SVG y querer aplicarlo
- Al interpolar entre dos transformaciones complejas

### Limitacion de `matrix`
- Ilegible para un humano: no es obvio que hace `matrix(0.866, 0.5, -0.5, 0.866, 0, 0)` (es una rotacion de 30°)
- Para SVGs escritos a mano, preferir las funciones individuales por claridad

---

## 11.7 Encadenamiento de transformaciones

### Como se encadenan
- Multiples transformaciones en el mismo atributo separadas por espacios:
  `transform="translate(50,50) rotate(45) scale(2)"`
- El orden de escritura importa profundamente

### Orden de aplicacion: la regla fundamental

**Las transformaciones se aplican de derecha a izquierda** (desde el punto de vista del sistema de coordenadas):
- Primero se aplica `scale(2)` al sistema de coordenadas
- Luego `rotate(45)` sobre el sistema ya escalado
- Luego `translate(50,50)` sobre el sistema ya escalado y rotado

**Interpretacion alternativa equivalente (de izquierda a derecha)**
- Se puede pensar que las transformaciones se aplican al elemento en el orden de lectura
- Primero el elemento se traslada, luego rota, luego escala
- Ambas interpretaciones producen el mismo resultado final

### Por que el orden importa: casos concretos

**`translate(100, 0) rotate(45)`** — diferente a — **`rotate(45) translate(100, 0)`**

- Caso 1: el elemento se mueve 100 unidades a la derecha y LUEGO rota desde ese punto
- Caso 2: el elemento rota primero y LUEGO se mueve 100 unidades en la direccion del eje X rotado (que ya no apunta a la derecha sino en diagonal)

**Regla practica**
- Si se quiere rotar un elemento y luego desplazarlo en la direccion original (horizontal/vertical): `rotate primero, translate despues` (en la escritura)
- Si se quiere desplazar el elemento y rotarlo alrededor de su nueva posicion: `translate primero, rotate despues`

### Patron comun: rotar alrededor del centro propio
```
transform="translate(cx, cy) rotate(angulo) translate(-cx, -cy)"
```
- Mover el origen al centro del elemento
- Rotar
- Mover el origen de vuelta
- El elemento rota sobre su propio centro sin desplazarse

---

## 11.8 transform-origin

### El problema en SVG 1.1
- En SVG 1.1, no existia `transform-origin`
- La unica forma de cambiar el punto de origen de las transformaciones era encadenar translaciones: `translate(cx,cy) rotate(a) translate(-cx,-cy)`
- Verbose y propenso a errores

### `transform-origin` en SVG 2 y CSS

**SVG 2**
- La propiedad CSS `transform-origin` se puede aplicar a elementos SVG
- Acepta los mismos valores que para elementos HTML: `50% 50%`, `center`, `top left`, valores en px, etc.
- Cambia el punto de referencia de `rotate`, `scale`, y `skew`

**Como usarlo**
- `style="transform-origin: 50% 50%;"` — rotar/escalar desde el centro del elemento
- `style="transform-origin: 0 0;"` — desde la esquina superior izquierda (comportamiento por defecto de SVG)
- `style="transform-origin: center;"` — equivalente al 50% 50%

**Importante: coordenadas relativas vs absolutas**
- En HTML: `transform-origin: 50% 50%` es el centro del elemento
- En SVG: los porcentajes se calculan respecto al viewport del SVG, no del elemento
- Para el centro del elemento en SVG: mejor usar `transform-origin: centro_x centro_y` con valores en px o unidades absolutas
- O usar el patron `translate/rotate/translate` de SVG 1.1 que siempre funciona

### Soporte actual
- Bien soportado en todos los navegadores modernos para SVG inline
- Para SVG como archivo externo: soporte variable
- El patron `translate/rotate/translate` es mas compatible entre entornos

---

## 11.9 Transformaciones y sistema de coordenadas

### Las transformaciones crean sistemas de coordenadas locales

Cada elemento transformado "vive" en su propio sistema de coordenadas local:
- Los atributos de posicion del elemento (`x`, `y`, `cx`, `cy`) son relativos a su sistema local
- Las transformaciones del padre afectan al sistema local del hijo
- Los hijos ven su sistema local como si fuera absoluto

### Herencia en profundidad

**Cadena de transformaciones**
```
<g transform="translate(100, 100)">        Sistema desplazado 100,100
  <g transform="rotate(45)">              Sistema rotado 45° dentro del anterior
    <rect x="10" y="10" />               Rectangulo en (10,10) del sistema rotado
  </g>
</g>
```
El rectangulo se posiciona en (10,10) del sistema rotado, que a su vez esta dentro del sistema desplazado. La posicion final en pantalla es el resultado de componer todas las transformaciones.

### Efecto en stroke-width

- El `stroke-width` se ve afectado por las transformaciones de escala
- `scale(2)` hace que un `stroke-width="1"` se vea como `2px` en pantalla
- Con `scale(2, 1)` (solo en X): el stroke se ve mas grueso en vertical que en horizontal
- Solucion: usar `vector-effect="non-scaling-stroke"` en el elemento (ver seccion 7.8)

### Efecto en font-size

- El `font-size` tambien se escala con las transformaciones
- `scale(2)` duplica el tamanio visual del texto
- Hay que tenerlo en cuenta cuando se escalan grupos que contienen texto

### La Current Transformation Matrix (CTM)

- En cualquier punto del SVG, hay una "matriz acumulada" que combina todas las transformaciones de los elementos padre
- Se puede obtener con JavaScript: `element.getCTM()`
- Util para:
  - Convertir coordenadas del mouse (en pixels de pantalla) a coordenadas del elemento
  - Saber la posicion absoluta de un elemento en pantalla
  - Componer transformaciones programaticamente
