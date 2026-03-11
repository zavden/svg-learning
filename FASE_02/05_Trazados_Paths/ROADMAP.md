# Seccion 5: Trazados (Paths)

> `<path>` es el elemento mas poderoso de SVG. Todo lo que las formas basicas pueden hacer,
> `<path>` tambien puede hacerlo — y mucho mas. Dominarlo desbloquea cualquier forma imaginable.

---

## 5.1 Introduccion a `<path>`

### Por que existe `<path>`
- Las formas basicas (`<rect>`, `<circle>`, etc.) son atajos convenientes para formas comunes
- Cualquier forma geometrica, por compleja que sea, puede describirse con `<path>`
- Es el elemento que usan los editores graficos (Illustrator, Inkscape, Figma) para exportar cualquier forma
- Internamente, el navegador puede convertir todas las formas basicas a paths para el renderizado

### El atributo `d` — Path Data
- El unico atributo obligatorio y especifico de `<path>`
- Contiene una serie de **comandos** que describen el trazado
- Cada comando es una letra seguida de sus parametros numericos
- La letra indica la operacion; los numeros indican coordenadas o distancias
- El atributo `d` con valor vacio o invalido produce un path invisible (sin error visible)

### El "lapiz" imaginario
- La mejor forma de entender paths es imaginar un lapiz sobre papel
- Los comandos de path controlan ese lapiz:
  - Levantar el lapiz y moverlo a otro punto sin dibujar (M)
  - Bajar el lapiz y trazar una linea recta (L, H, V)
  - Trazar una curva (C, S, Q, T)
  - Trazar un arco de elipse (A)
  - Cerrar el trazo actual regresando al inicio (Z)
- El lapiz tiene siempre una "posicion actual" que cambia con cada comando

### Resumen rapido de todos los comandos

| Comando | Nombre | Parametros |
|---------|--------|------------|
| `M` / `m` | Move To | x y |
| `L` / `l` | Line To | x y |
| `H` / `h` | Horizontal Line | x |
| `V` / `v` | Vertical Line | y |
| `Z` / `z` | Close Path | (ninguno) |
| `C` / `c` | Cubic Bezier | x1 y1, x2 y2, x y |
| `S` / `s` | Smooth Cubic Bezier | x2 y2, x y |
| `Q` / `q` | Quadratic Bezier | x1 y1, x y |
| `T` / `t` | Smooth Quadratic Bezier | x y |
| `A` / `a` | Arc | rx ry rot large-arc sweep x y |

---

## 5.2 Sintaxis del atributo `d`

### Mayusculas vs minusculas: absoluto vs relativo

**Comandos en MAYUSCULA — Coordenadas absolutas**
- Las coordenadas son relativas al **origen del sistema de coordenadas** (el punto 0,0)
- `L 100 50` significa "traza una linea hasta el punto (100, 50) del sistema de coordenadas"
- No importa donde este el "lapiz" actualmente: siempre va al punto indicado

**Comandos en minuscula — Coordenadas relativas**
- Las coordenadas son relativas a la **posicion actual del lapiz**
- `l 100 50` significa "traza una linea 100 unidades a la derecha y 50 abajo desde donde estoy ahora"
- Si el lapiz esta en (200, 200), `l 100 50` lo lleva a (300, 250)

**Cuando usar cada uno**
- Absoluto: mas predecible para posicionar elementos en lugares especificos
- Relativo: mas portable (el path se puede mover cambiando solo el M inicial); produce numeros mas pequenios en muchos casos; preferido por SVGO al optimizar

### Separadores: espacios y comas
- Los numeros del atributo `d` se pueden separar con espacios, comas, o una combinacion
- Estos tres son equivalentes:
  - `d="M 10 20 L 50 80"` — espacios
  - `d="M10,20L50,80"` — comas, sin espacios extra
  - `d="M10 20L50 80"` — mixto
- Los espacios entre el comando (letra) y sus numeros son opcionales
- Los numeros negativos actuan como separadores implicitos: `l10-5` es `l 10 -5`
- Los puntos decimales tambien: `l10.5.3` es `l 10.5 0.3`

### Comandos repetidos (parametros multiples)
- Si despues de un comando hay mas numeros de los necesarios, se repiten con el mismo comando
- `L 10 20 30 40 50 60` equivale a `L 10 20 L 30 40 L 50 60`
- **Excepcion importante**: despues de `M`, los puntos adicionales se tratan como `L`, no como `M`
  - `M 10 20 30 40` equivale a `M 10 20 L 30 40`

### Legibilidad del atributo `d`
- Un path complejo puede ser una cadena de cientos de numeros — completamente ilegible a simple vista
- Los editores graficos generan paths ilegibles pero correctos
- Para paths escritos a mano, usar saltos de linea dentro del atributo es valido:
  ```
  d="M 10 20
     L 50 80
     Z"
  ```
- Herramientas como SVG Path Editor permiten editar paths visualmente

---

## 5.3 Comandos de movimiento

### `M x y` / `m dx dy` — Move To

**Que hace**
- Mueve el "lapiz" a la posicion indicada **sin dibujar nada**
- Establece el punto de inicio de un nuevo sub-trazado
- No produce linea visible, solo cambia la posicion actual

**`M` (absoluto)**
- Mueve el lapiz al punto (x, y) del sistema de coordenadas
- Ejemplo: `M 50 50` — el lapiz va al punto (50, 50)

**`m` (relativo)**
- Mueve el lapiz dx unidades en X y dy en Y desde la posicion actual
- Si el lapiz esta en (20, 20): `m 30 30` lo mueve a (50, 50)

**El primer comando de todo path**
- Todo atributo `d` debe comenzar con `M` o `m`
- Un path sin `M` inicial no se renderiza correctamente
- El punto establecido por el primer `M` es el "punto de inicio" para el cierre con `Z`

**Parametros multiples en `M`**
- `M 10 20 30 40` — se mueve a (10,20) y luego traza una linea a (30,40)
- Esto es porque los puntos extra se tratan como `L` (Line To)
- En `m`, los puntos extra se tratan como `l` (linea relativa)

**Multiples `M` en un path**
- Cada `M` adicional inicia un nuevo sub-trazado (ver seccion 5.9)
- Permite crear formas con multiples partes desconectadas en un solo atributo `d`

---

## 5.4 Comandos de linea

### `L x y` / `l dx dy` — Line To

**Que hace**
- Traza una linea recta desde la posicion actual hasta el punto indicado
- La posicion actual pasa a ser el punto final de la linea

**`L` (absoluto)**
- Traza una linea hasta el punto (x, y) del sistema de coordenadas
- `L 100 50` — linea hasta (100, 50), sin importar la posicion actual

**`l` (relativo)**
- Traza una linea dx unidades en X y dy en Y desde la posicion actual
- `l 50 0` — linea horizontal de 50 unidades a la derecha
- `l 0 50` — linea vertical de 50 unidades hacia abajo

### `H x` / `h dx` — Horizontal Line To

**Que hace**
- Traza una linea horizontal hasta la coordenada X indicada
- Solo un parametro (la coordenada Y no cambia)

**`H` (absoluto)**
- `H 100` — traza hasta x=100, manteniendo la Y actual

**`h` (relativo)**
- `h 50` — traza 50 unidades a la derecha desde la posicion actual
- `h -30` — traza 30 unidades a la izquierda

**Ventaja sobre `L`**
- Un solo numero en vez de dos: mas conciso para lineas horizontales
- Produce paths mas cortos en bytes

### `V y` / `v dy` — Vertical Line To

**Que hace**
- Traza una linea vertical hasta la coordenada Y indicada
- Solo un parametro (la coordenada X no cambia)

**`V` (absoluto)**
- `V 100` — traza hasta y=100, manteniendo la X actual

**`v` (relativo)**
- `v 50` — traza 50 unidades hacia abajo
- `v -30` — traza 30 unidades hacia arriba

**Ejemplo practico: dibujar un rectangulo con path**
```
M 10 10   (ir a la esquina superior izquierda)
H 90      (linea horizontal hasta x=90)
V 90      (linea vertical hasta y=90)
H 10      (linea horizontal hasta x=10)
Z         (cerrar: linea de vuelta a 10,10)
```

---

## 5.5 Comando de cierre

### `Z` / `z` — Close Path

**Que hace**
- Traza una linea recta desde la posicion actual hasta el **punto de inicio del sub-trazado actual**
- El punto de inicio es el punto establecido por el ultimo `M`
- Si la posicion actual ya es el punto de inicio, `Z` no dibuja nada visible (pero sigue siendo importante para el estilo de union de lineas)

**Mayusculas o minusculas: no importa**
- `Z` y `z` hacen exactamente lo mismo
- Es el unico comando donde la diferencia absoluto/relativo no aplica

**Por que usar `Z` en vez de repetir el primer punto**
- `Z` cierra el path con un `stroke-linejoin` correcto (la union visual de la esquina)
- Si se repite el primer punto con `L`, la union puede verse diferente (como `stroke-linecap` en vez de `stroke-linejoin`)
- Para paths rellenos, `Z` asegura el cierre correcto del area

**`Z` en sub-trazados**
- `Z` cierra el sub-trazado actual, no todo el path
- Si hay multiples sub-trazados (multiples `M`), cada uno tiene su propio "punto de inicio"
- Despues de `Z`, la posicion actual vuelve al punto de inicio del sub-trazado

---

## 5.6 Curvas de Bezier cubicas

### Que es una curva de Bezier
- Una curva matematica controlada por **puntos de control** que "atraen" la curva sin que esta los toque
- Las curvas de Bezier cubicas usan **dos puntos de control** por segmento
- Son el tipo de curva mas usado en diseño grafico por su flexibilidad y suavidad

### `C x1 y1, x2 y2, x y` / `c dx1 dy1, dx2 dy2, dx dy` — Cubic Bezier

**Los tres grupos de parametros**
- `(x1, y1)`: primer punto de control — "jala" el inicio de la curva
- `(x2, y2)`: segundo punto de control — "jala" el final de la curva
- `(x, y)`: el punto final de la curva (el "lapiz" llega aqui)

**Como interpretar los puntos de control**
- El punto de control 1 esta cerca del punto de inicio de la curva
- La tangente de la curva en el inicio apunta hacia el punto de control 1
- El punto de control 2 esta cerca del punto final de la curva
- La tangente de la curva en el final apunta desde el punto de control 2
- Si ambos puntos de control coinciden con los extremos: la curva es una linea recta

**Visualizacion mental**
- Imaginar dos "tirantes" o "mangos" que salen del inicio y del final de la curva
- El angulo y la longitud de cada tirante controla la direccion y la curvatura en ese extremo
- Puntos de control lejanos = curva mas pronunciada; puntos cercanos = curva mas suave

### `S x2 y2, x y` / `s dx2 dy2, dx dy` — Smooth Cubic Bezier

**Que hace**
- Es un `C` simplificado: solo necesita el segundo punto de control y el punto final
- El primer punto de control se calcula **automaticamente** como el reflejo del segundo punto de control del comando `C` o `S` anterior
- Si no hay comando previo `C` o `S`: el primer punto de control coincide con la posicion actual

**Por que es util**
- Permite encadenar curvas que se unen suavemente (continuidad tangencial)
- Ideal para trazados organicos como letras, bordes decorativos, formas naturales
- Reduce la cantidad de parametros a escribir a la mitad

**Patron comun: encadenar C y S**
```
C (primer segmento curvo)
S (segundo segmento, suave con el anterior)
S (tercer segmento, suave con el anterior)
```

---

## 5.7 Curvas de Bezier cuadraticas

### Diferencia con las cubicas
- Las curvas cuadraticas tienen **un solo punto de control** por segmento (en vez de dos)
- Menor control sobre la forma, pero mas simples de definir
- Producen curvas mas "simples" y menos flexibles

### `Q x1 y1, x y` / `q dx1 dy1, dx dy` — Quadratic Bezier

**Los dos grupos de parametros**
- `(x1, y1)`: el unico punto de control — controla la forma de toda la curva
- `(x, y)`: el punto final de la curva

**Cuando usar Q vs C**
- `Q` es suficiente para curvas simples (arcos suaves, esquinas redondeadas)
- `C` da mas control sobre la curvatura en cada extremo
- Para texto curvo y trazados decorativos simples, `Q` suele ser suficiente

### `T x y` / `t dx dy` — Smooth Quadratic Bezier

**Que hace**
- Similar a `S` pero para curvas cuadraticas
- El punto de control se calcula automaticamente como reflejo del punto de control del `Q` o `T` anterior
- Solo necesita el punto final

**Uso tipico**
- Encadenar multiples curvas cuadraticas suaves en una sola serie
- Trazo de letras cursivas y formas organicas simples

---

## 5.8 Arcos

### El comando de arco: el mas complejo
- `A` es el comando con mas parametros de todos
- Traza un fragmento (arco) de una elipse entre dos puntos
- Siempre existen **cuatro arcos posibles** entre dos puntos: los flags seleccionan cual

### `A rx ry x-rotation large-arc-flag sweep-flag x y`

**`rx` y `ry` — Radios de la elipse**
- `rx`: radio horizontal de la elipse imaginaria
- `ry`: radio vertical de la elipse imaginaria
- Si `rx = ry`: la elipse es un circulo y el arco es un arco de circulo
- Si la distancia entre los puntos es mayor que `2*rx` o `2*ry`: el navegador escala los radios automaticamente

**`x-rotation` — Rotacion de la elipse**
- Grados de rotacion de la elipse respecto al eje X
- Valor `0`: la elipse no esta rotada (ejes alineados con X e Y)
- Solo tiene efecto visible cuando `rx != ry`
- Permite crear arcos en elipses inclinadas

**`large-arc-flag` — Arco grande o pequeño**
- `0`: tomar el arco **menor** de los dos posibles (el que recorre el angulo menor)
- `1`: tomar el arco **mayor** de los dos posibles (el que recorre el angulo mayor)

**`sweep-flag` — Sentido de la curva**
- `0`: trazar en sentido **antihorario** (counter-clockwise)
- `1`: trazar en sentido **horario** (clockwise)

**`x y` — Punto final del arco**
- Coordenadas del punto donde termina el arco
- El arco comienza en la posicion actual del lapiz

### Los cuatro arcos posibles

Entre dos puntos cualesquiera, una elipse de radios `rx` y `ry` puede generar cuatro arcos distintos:

| large-arc-flag | sweep-flag | Resultado |
|----------------|------------|-----------|
| `0` | `0` | Arco pequeño, antihorario |
| `0` | `1` | Arco pequeño, horario |
| `1` | `0` | Arco grande, antihorario |
| `1` | `1` | Arco grande, horario |

**Truco para memorizar**
- Primero decidir: ¿quiero el arco corto o el largo? → `large-arc-flag`
- Luego decidir: ¿por donde "dobla" la curva? → `sweep-flag`

### Casos de uso del arco

**Sector de torta (pie chart)**
- Cada sector es un `M` al centro, `L` al borde exterior, arco `A` a lo largo del borde, `Z` para cerrar

**Borde superior curvo**
- Un rectangulo donde el borde superior es un arco en vez de una linea recta

**Indicador circular (progress ring)**
- Un solo arco cuya longitud representa el porcentaje (alternativa a `stroke-dashoffset`)

**Circulos y semicirculos**
- Para un circulo completo con `A`: necesita dos arcos (un circulo completo no se puede hacer con un solo comando A porque inicio y fin coinciden)

---

## 5.9 Sub-trazados (Sub-paths)

### Que es un sub-trazado
- Un path puede contener **multiples porciones independientes**
- Cada nueva instruccion `M` inicia un sub-trazado nuevo
- Los sub-trazados no estan conectados entre si
- El atributo `d` completo (con todos sus sub-trazados) es un unico elemento `<path>`

### Por que usar sub-trazados

**Formas compuestas en un solo elemento**
- La letra "i": el palo vertical y el punto son dos sub-trazados
- Un icono de "casa" con una puerta: el contorno y el rectangulo de la puerta
- El numero "8": dos ovales como sub-trazados

**Formas con agujeros**
- Un donuts: un circulo exterior y uno interior como sub-trazados
- Con `fill-rule="evenodd"`, el interior se "vacia" (ver seccion 5.10)

**Separar visualmente con un solo elemento**
- Puntos suspensivos: tres circulos o cuadraditos como sub-trazados
- Barras de un menu hamburguesa: tres rectangulos

### `Z` y los sub-trazados
- `Z` cierra el sub-trazado **actual** (regresa al ultimo `M`)
- Despues de `Z`, el siguiente `M` inicia un nuevo sub-trazado completamente independiente

### Implicaciones de rendimiento
- Un path con multiples sub-trazados es generalmente mas eficiente que multiples elementos `<path>` separados
- Un elemento = un nodo del DOM = menos sobrecarga de memoria y eventos
- Importante para SVGs con muchos elementos repetidos (como patrones o datos densos)

---

## 5.10 fill-rule en paths

### El problema: que se rellena cuando los trazados se superponen

Cuando un path tiene sub-trazados o cuando los trazados se cruzan entre si, hay ambiguedad sobre que areas estan "dentro" y cuales "fuera". El atributo `fill-rule` resuelve esa ambiguedad.

### `fill-rule="nonzero"` — Regla de no-cero (valor por defecto)

**Como funciona**
- Para cada punto del plano, se traza un rayo imaginario hacia el infinito
- Se cuenta cuantas veces los trazados del path cruzan ese rayo:
  - Cruce en sentido horario: +1
  - Cruce en sentido antihorario: -1
- Si el resultado es distinto de cero: el punto esta **dentro** (se rellena)
- Si el resultado es cero: el punto esta **fuera** (no se rellena)

**Efecto practico**
- Para paths simples y no-superpuestos: rellena todo lo que visualmente parece "interior"
- Para sub-trazados donde ambos van en el mismo sentido: ambos se rellenan (no hay agujero)

### `fill-rule="evenodd"` — Regla par-impar

**Como funciona**
- Para cada punto, se traza un rayo imaginario hacia el infinito
- Se cuenta cuantas veces los trazados del path cruzan ese rayo (sin importar la direccion)
- Si el conteo es **impar**: el punto esta dentro (se rellena)
- Si el conteo es **par**: el punto esta fuera (no se rellena)

**Efecto practico**
- Las areas "dentro-dentro" (donde el rayo cruza dos veces) quedan vacias: crean agujeros
- Es la forma mas sencilla de crear formas con agujeros
- La letra "O": el exterior es un cruce, el interior es dos cruces → el interior queda vacio

### Como crear agujeros con evenodd

**Patron: forma exterior + forma interior**
```
M (inicio del exterior)
... trazado del exterior ...
Z
M (inicio del interior/agujero)
... trazado del interior ...
Z
fill-rule="evenodd"
```

El exterior es el primer sub-trazado; el agujero es el segundo. Con `evenodd`, el area interior queda vacia.

### Cuando usar cada uno

| Situacion | Regla recomendada |
|-----------|------------------|
| Formas simples sin cruces | `nonzero` (defecto, no hay diferencia) |
| Crear agujeros sencillos | `evenodd` (mas intuitivo) |
| Formas complejas que se cruzan | Depende del efecto deseado |
| Paths generados por editores graficos | El editor decide; mejor no cambiar |
