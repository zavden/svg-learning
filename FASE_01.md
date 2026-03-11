# FASE 1: Fundamentos
> Secciones 1-4 del Roadmap SVG Basico-Intermedio
> Entender que es SVG, su estructura, el sistema de coordenadas y las formas basicas.

---

## 1. Fundamentos y Conceptos Previos

### 1.1 Que es SVG
- Definicion: Scalable Vector Graphics, formato de imagen vectorial basado en XML
- Especificacion del W3C (version actual: SVG 1.1 Second Edition, SVG 2 en desarrollo)
- Diferencia entre graficos vectoriales y graficos rasterizados (bitmap)
- Historia breve: SVG 1.0 (2001), SVG 1.1 (2003), SVG Tiny 1.2 (2008), SVG 2 (en progreso)

### 1.2 Ventajas de SVG
- Escalabilidad infinita sin perdida de calidad
- Tamanio de archivo pequenio comparado con formatos raster para graficos simples
- Es texto plano (XML): se puede editar con cualquier editor de texto
- Se puede estilizar con CSS
- Se puede manipular con JavaScript y el DOM
- Es indexable y accesible (lectores de pantalla)
- Soporte nativo en todos los navegadores modernos
- Se puede animar de multiples formas (SMIL, CSS, JS)

### 1.3 Cuando usar SVG y cuando no
- **Ideal para**: iconos, logotipos, ilustraciones, graficos de datos, mapas, diagramas, animaciones UI, fondos decorativos
- **No ideal para**: fotografias, imagenes con millones de colores y detalles complejos, texturas fotograficas
- Comparacion con PNG, JPEG, GIF, WebP: casos de uso de cada uno

### 1.4 Graficos Vectoriales vs Rasterizados
- Como funcionan los vectores: puntos, lineas, curvas definidas matematicamente
- Como funcionan los raster: cuadricula de pixeles
- Impacto en el escalado: vectores mantienen nitidez, raster se pixelan
- Impacto en el tamanio de archivo segun complejidad del grafico

### 1.5 XML como base de SVG
- Que es XML: lenguaje de marcado extensible
- Reglas de sintaxis XML que aplican a SVG:
  - Todos los elementos deben cerrarse (`<rect />` o `<rect></rect>`)
  - Los atributos deben ir entre comillas
  - Es case-sensitive (distingue mayusculas de minusculas)
  - Debe estar bien formado (well-formed)
- Namespaces XML en SVG

---

## 2. Estructura del Documento SVG

### 2.1 El elemento raiz `<svg>`
- Atributos fundamentales:
  - `xmlns="http://www.w3.org/2000/svg"` (namespace obligatorio en archivos `.svg`)
  - `xmlns:xlink="http://www.w3.org/1999/xlink"` (namespace para enlaces, legacy)
  - `version` (valor tipico: `"1.1"`)
  - `width` y `height` (dimensiones del viewport)
  - `viewBox` (sistema de coordenadas interno)
- Diferencia entre usar SVG como archivo independiente vs inline en HTML

### 2.2 Prologo XML
- Declaracion XML: `<?xml version="1.0" encoding="UTF-8"?>`
- Declaracion DOCTYPE (opcional, rara vez usada hoy)
- Cuando se necesita y cuando se omite

### 2.3 Elementos de metadatos
- `<title>`: titulo del SVG (importante para accesibilidad)
- `<desc>`: descripcion del contenido SVG
- `<metadata>`: metadatos adicionales en otros formatos (RDF, Dublin Core)

### 2.4 El elemento `<defs>`
- Proposito: definir elementos reutilizables que no se renderizan directamente
- Que va dentro de `<defs>`: gradientes, patrones, filtros, clipPaths, simbolos, marcadores
- Diferencia entre definir y referenciar

### 2.5 Orden de renderizado (Painter's Model)
- SVG usa el modelo del pintor: los elementos se dibujan en orden del codigo fuente
- Los elementos posteriores se pintan encima de los anteriores
- No existe z-index nativo en SVG (se controla con el orden del DOM)

### 2.6 Estructura minima de un SVG valido
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <title>Descripcion breve</title>
  <desc>Descripcion detallada del contenido</desc>
  <!-- Contenido grafico aqui -->
</svg>
```

---

## 3. Sistema de Coordenadas y Viewport

### 3.1 El viewport
- Definicion: la ventana visible a traves de la cual se ve el contenido SVG
- Se define con `width` y `height` en el elemento `<svg>`
- Unidades del viewport: px, em, rem, %, cm, mm, in, pt, pc
- Viewport por defecto cuando no se especifican dimensiones

### 3.2 El atributo `viewBox`
- Sintaxis: `viewBox="min-x min-y width height"`
- Los cuatro valores y que significan:
  - `min-x`: coordenada X del punto superior izquierdo
  - `min-y`: coordenada Y del punto superior izquierdo
  - `width`: ancho del sistema de coordenadas interno
  - `height`: alto del sistema de coordenadas interno
- Relacion entre viewBox y viewport: como se mapea el contenido
- Efecto de un viewBox mas pequenio que el viewport (zoom in)
- Efecto de un viewBox mas grande que el viewport (zoom out)
- Efecto de desplazar min-x y min-y (panning)

### 3.3 El atributo `preserveAspectRatio`
- Sintaxis: `preserveAspectRatio="<align> [<meetOrSlice>]"`
- Valores de alineacion:
  - `none`: estira el contenido para llenar el viewport (distorsiona)
  - `xMinYMin`, `xMidYMin`, `xMaxYMin`
  - `xMinYMid`, `xMidYMid` (valor por defecto), `xMaxYMid`
  - `xMinYMax`, `xMidYMax`, `xMaxYMax`
- Valores de meet/slice:
  - `meet` (por defecto): todo el viewBox es visible, puede haber espacio vacio (como `background-size: contain`)
  - `slice`: el viewBox cubre todo el viewport, puede recortarse contenido (como `background-size: cover`)
- Combinaciones comunes y sus efectos visuales

### 3.4 Sistema de coordenadas del usuario
- Origen (0,0) en la esquina superior izquierda
- Eje X crece hacia la derecha
- Eje Y crece hacia abajo (diferente a las matematicas convencionales)
- Unidades del usuario: sin unidad especifica, mapeadas al viewBox

### 3.5 Sistemas de coordenadas anidados
- Cada elemento `<svg>` anidado crea un nuevo viewport y sistema de coordenadas
- Atributos `x`, `y`, `width`, `height` del SVG anidado
- Como interactuan los viewBox anidados

### 3.6 Unidades en SVG
- Unidades absolutas: px, cm, mm, in, pt, pc
- Unidades relativas: em, ex, %
- Unidades del usuario (sin sufijo): equivalentes a px por defecto
- Como se interpretan los porcentajes en distintos contextos

---

## 4. Formas Basicas

### 4.1 Rectangulo `<rect>`
- Atributos:
  - `x`, `y`: posicion de la esquina superior izquierda
  - `width`, `height`: dimensiones
  - `rx`, `ry`: radios de las esquinas redondeadas
- Comportamiento cuando solo se define `rx` o solo `ry`
- Creacion de cuadrados (width = height)
- Creacion de rectangulos completamente redondeados (rx = width/2)

### 4.2 Circulo `<circle>`
- Atributos:
  - `cx`, `cy`: coordenadas del centro
  - `r`: radio
- Valores por defecto de cx y cy (0 si no se especifican)

### 4.3 Elipse `<ellipse>`
- Atributos:
  - `cx`, `cy`: coordenadas del centro
  - `rx`: radio horizontal
  - `ry`: radio vertical
- Relacion con `<circle>` (un circulo es una elipse con rx = ry)

### 4.4 Linea `<line>`
- Atributos:
  - `x1`, `y1`: punto de inicio
  - `x2`, `y2`: punto final
- La linea solo es visible si tiene un `stroke` definido (no tiene fill)

### 4.5 Polilinia `<polyline>`
- Atributo `points`: lista de coordenadas x,y separadas por espacios o comas
- Formatos validos: `"0,0 50,25 100,0"` o `"0 0 50 25 100 0"`
- La polilinia no se cierra automaticamente
- El fill por defecto es negro: hay que poner `fill="none"` si solo se quiere el trazo

### 4.6 Poligono `<polygon>`
- Atributo `points`: misma sintaxis que polyline
- Diferencia con polyline: el poligono cierra automaticamente la forma (conecta el ultimo punto con el primero)
- Creacion de triangulos, pentagonos, estrellas y formas regulares

### 4.7 Atributos de presentacion comunes a todas las formas
- `fill`: color de relleno
- `stroke`: color del borde
- `stroke-width`: grosor del borde
- `opacity`: opacidad general del elemento
- Estos atributos se detallan en las secciones 7 y 8 (Fase 2)
