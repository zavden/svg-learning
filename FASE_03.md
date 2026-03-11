# FASE 3: Apariencia Avanzada
> Secciones 9-11 del Roadmap SVG Basico-Intermedio
> Gradientes, patrones y transformaciones para graficos mas ricos visualmente.

---

## 9. Gradientes

### 9.1 Gradiente lineal `<linearGradient>`
- Se define dentro de `<defs>`
- Requiere un `id` para ser referenciado
- Atributos de direccion:
  - `x1`, `y1`: punto de inicio (por defecto: 0%, 0%)
  - `x2`, `y2`: punto final (por defecto: 100%, 0% = horizontal izquierda a derecha)
- Ejemplos de direcciones comunes:
  - Horizontal: `x1="0%" y1="0%" x2="100%" y2="0%"`
  - Vertical: `x1="0%" y1="0%" x2="0%" y2="100%"`
  - Diagonal: `x1="0%" y1="0%" x2="100%" y2="100%"`

### 9.2 Gradiente radial `<radialGradient>`
- Atributos:
  - `cx`, `cy`: centro del gradiente (por defecto: 50%, 50%)
  - `r`: radio del gradiente (por defecto: 50%)
  - `fx`, `fy`: punto focal (por defecto: igual que cx, cy)
  - `fr`: radio focal (SVG 2)
- El punto focal desplazado crea gradientes asimetricos

### 9.3 Stops de color `<stop>`
- Elementos hijos de los gradientes
- Atributos:
  - `offset`: posicion en el gradiente (0% a 100%, o 0 a 1)
  - `stop-color`: el color en esa posicion
  - `stop-opacity`: opacidad del color en esa posicion
- Minimo dos stops para crear un gradiente visible
- Los stops deben ir en orden creciente de offset

### 9.4 Atributo `gradientUnits`
- `objectBoundingBox` (por defecto): coordenadas relativas al bounding box del elemento (0 a 1 o 0% a 100%)
- `userSpaceOnUse`: coordenadas absolutas en el espacio del usuario

### 9.5 Atributo `spreadMethod`
- `pad` (por defecto): los colores extremos se extienden mas alla del gradiente
- `reflect`: el gradiente se refleja (espejo) al llegar al final
- `repeat`: el gradiente se repite desde el inicio

### 9.6 Transformaciones de gradientes
- `gradientTransform`: aplica transformaciones al gradiente (rotate, skew, etc.)
- Permite rotar gradientes sin cambiar x1/y1/x2/y2

### 9.7 Referenciar gradientes
- Se usan con `fill="url(#idDelGradiente)"` o `stroke="url(#idDelGradiente)"`
- Se pueden heredar con `href` o `xlink:href` de otro gradiente

---

## 10. Patrones (Patterns)

### 10.1 El elemento `<pattern>`
- Se define dentro de `<defs>`
- Requiere un `id` para ser referenciado
- Atributos principales:
  - `x`, `y`: posicion de inicio del patron
  - `width`, `height`: tamanio de cada celda del patron
  - `viewBox`: sistema de coordenadas interno del patron

### 10.2 patternUnits
- `objectBoundingBox` (por defecto): las dimensiones del patron son relativas al elemento que lo usa
- `userSpaceOnUse`: las dimensiones son absolutas en el espacio del usuario
- Diferencias practicas entre ambos

### 10.3 patternContentUnits
- `userSpaceOnUse` (por defecto): el contenido del patron usa coordenadas del usuario
- `objectBoundingBox`: el contenido usa coordenadas relativas al bounding box
- Combinaciones con patternUnits y sus efectos

### 10.4 patternTransform
- Aplica transformaciones al patron completo
- Ejemplo: rotar un patron 45 grados para crear diagonales

### 10.5 Creacion de patrones comunes
- Rayas horizontales/verticales/diagonales
- Puntos (polka dots)
- Cuadriculas (grids)
- Patrones de ladrillos
- Patrones complejos anidados (patron dentro de patron)

### 10.6 Referenciar patrones
- `fill="url(#idDelPatron)"` o `stroke="url(#idDelPatron)"`

---

## 11. Transformaciones

### 11.1 El atributo `transform`
- Se puede aplicar a cualquier elemento grafico o grupo
- Multiples transformaciones se aplican de derecha a izquierda (la ultima escrita se aplica primero)
- Las transformaciones afectan al sistema de coordenadas del elemento

### 11.2 translate(tx, ty)
- Mueve el elemento tx unidades en X y ty unidades en Y
- Si solo se da un valor, ty = 0
- Equivalente a cambiar la posicion sin modificar x/y del elemento

### 11.3 scale(sx, sy)
- Escala el elemento sx veces en X y sy veces en Y
- Si solo se da un valor, sy = sx (escalado uniforme)
- Valores negativos reflejan el elemento
- La escala se aplica desde el origen del sistema de coordenadas (0,0), no desde el centro del elemento
- Para escalar desde el centro: translate al centro, scale, translate de vuelta

### 11.4 rotate(angle, cx, cy)
- Rota el elemento `angle` grados en sentido horario
- `cx`, `cy` (opcionales): centro de rotacion
- Sin cx/cy, rota alrededor del origen (0,0)
- Con cx/cy, equivale a: translate(cx,cy) rotate(angle) translate(-cx,-cy)

### 11.5 skewX(angle) y skewY(angle)
- `skewX`: inclina el elemento en el eje X
- `skewY`: inclina el elemento en el eje Y
- El angulo se especifica en grados

### 11.6 matrix(a, b, c, d, e, f)
- Transformacion generica usando una matriz 3x3
- Los 6 valores representan: `[a c e; b d f; 0 0 1]`
- Cualquier combinacion de translate/scale/rotate/skew se puede expresar como una sola matrix
- Correspondencias:
  - translate(tx,ty) = matrix(1, 0, 0, 1, tx, ty)
  - scale(sx,sy) = matrix(sx, 0, 0, sy, 0, 0)
  - rotate(a) = matrix(cos(a), sin(a), -sin(a), cos(a), 0, 0)

### 11.7 Encadenamiento de transformaciones
- Sintaxis: `transform="translate(50,50) rotate(45) scale(2)"`
- Orden de aplicacion: scale primero, luego rotate, luego translate (derecha a izquierda)
- El orden importa: rotate + translate != translate + rotate

### 11.8 transform-origin
- Define el punto de referencia para las transformaciones
- Sintaxis CSS: `transform-origin: 50% 50%` (centro del elemento)
- En SVG 1.1 no existia; se emulaba con translate/transform/translate
- Soportado en SVG 2 y en CSS aplicado a SVG

### 11.9 Transformaciones y sistema de coordenadas
- Cada transformacion modifica el sistema de coordenadas local del elemento
- Los hijos heredan el sistema de coordenadas transformado del padre
- Esto afecta a stroke-width, font-size y otros valores con unidades
