# Seccion 1: Fundamentos y Conceptos Previos

> Base conceptual antes de escribir una sola linea de SVG.
> Sin entender estos fundamentos, muchas decisiones posteriores no tendran sentido.

---

## 1.1 Que es SVG

### Definicion formal
- **SVG** = Scalable Vector Graphics
- Formato de imagen basado en **XML** (texto plano, no binario)
- Estandar del **W3C** (World Wide Web Consortium), el mismo organismo que define HTML y CSS
- Un archivo `.svg` es, en esencia, un documento de texto que describe geometria

### Versiones de la especificacion
| Version | Ano | Estado | Notas |
|---------|-----|--------|-------|
| SVG 1.0 | 2001 | Obsoleta | Primera version oficial |
| SVG 1.1 | 2003 | Vigente | La mas soportada, base de este roadmap |
| SVG 1.1 Second Edition | 2011 | Vigente | Correcciones de SVG 1.1 |
| SVG Tiny 1.2 | 2008 | Nicho | Para dispositivos moviles de la epoca |
| SVG 2 | En progreso | Parcial | Algunos navegadores ya implementan partes |

### Que significa "basado en XML"
- El contenido es texto legible por humanos y maquinas
- Se puede abrir con cualquier editor de texto
- Sigue las reglas de sintaxis de XML (no de HTML)
- Los navegadores lo parsean igual que un documento HTML: construyen un DOM
- Hereda las capacidades de XML: namespaces, validacion, transformaciones XSLT

### Conceptos previos recomendados (no obligatorios pero utiles)
- Conocimiento basico de HTML (estructura de etiquetas y atributos)
- Nociones de CSS (selectores, propiedades, valores)
- Nocion de sistema de coordenadas cartesiano (ejes X e Y)

---

## 1.2 Ventajas de SVG

### Escalabilidad
- Un SVG se ve igual de nítido en un icono de 16px que en una valla de 10 metros
- No hay "pixelacion": la descripcion matematica se recalcula en cada tamano
- Critico para pantallas de alta densidad (Retina, 4K): un PNG necesita versiones @2x, @3x; SVG funciona con uno solo
- Comparacion directa: zoom al 400% en un SVG vs un PNG del mismo grafico

### Naturaleza de texto
- Se puede escribir a mano en un editor de texto
- Se puede generar programaticamente (con JavaScript, Python, cualquier lenguaje)
- Se puede leer y entender sin herramientas especiales
- Se puede versionar con git y ver los diffs de cambios
- Se puede buscar y reemplazar contenido con herramientas de texto

### Integracion con tecnologias web
- CSS puede estilizar SVG: los mismos selectores, propiedades de color, fuentes, etc.
- JavaScript puede manipular el DOM del SVG: crear, modificar, eliminar elementos
- El SVG es parte del mismo documento HTML (cuando es inline): comparte estilos y scripts
- Los elementos SVG pueden recibir eventos del mouse/teclado/tactil

### Accesibilidad nativa
- El texto dentro de SVG es texto real (seleccionable, copiable, buscable)
- Los lectores de pantalla pueden leer el contenido semantico
- Se puede agregar texto alternativo con `<title>` y `<desc>`
- Los motores de busqueda pueden indexar el texto dentro de SVG

### Rendimiento en casos apropiados
- Un icono SVG puede pesar menos de 1KB vs varios KB para una imagen PNG equivalente
- Un sprite SVG reemplaza decenas de peticiones HTTP de imagenes individuales
- La compresion gzip/brotli funciona muy bien con SVG (texto con patrones repetitivos)

---

## 1.3 Cuando usar SVG y cuando no

### Casos donde SVG es la eleccion correcta

**Iconos y UI**
- Iconos de interfaz de usuario (navegacion, botones, indicadores)
- Logotipos y marcas que deben verse nitidos en cualquier tamano
- Ilustraciones simples o de mediana complejidad
- Diagramas, organigramas, flujos de proceso

**Graficos de datos**
- Graficos de barras, lineas, torta, area
- Mapas geograficos (paises, regiones, rutas)
- Visualizaciones interactivas donde los elementos responden al usuario

**Decoracion y efectos**
- Fondos geometricos y patrones
- Separadores decorativos entre secciones
- Formas organicas de fondo (blobs, ondas)
- Animaciones de carga (spinners, progress bars)

### Casos donde SVG NO es la eleccion correcta

**Fotografias y imagenes realistas**
- Una foto tiene millones de colores y detalles organicos que SVG no puede representar eficientemente
- Un SVG intentando representar una fotografia seria enormemente mas pesado que un JPEG
- Usar JPEG para fotografias, WebP como alternativa moderna

**Imagenes con muy alta complejidad**
- Ilustraciones con miles de elementos puede degradar el rendimiento de renderizado
- Si el SVG pesa mas que una imagen PNG equivalente, probablemente no es el formato correcto

**Cuando la interactividad no es necesaria y el peso importa**
- Para un icono estatico en una app movil, un PNG bien optimizado puede ser mas practico
- El contexto importa: web vs movil nativo vs impresion

### Comparacion de formatos

| Formato | Mejor para | Escalable | Editable | Peso tipico |
|---------|-----------|-----------|----------|-------------|
| SVG | Iconos, ilustraciones, graficos | Si (infinito) | Si (texto) | Bajo-medio |
| PNG | Graficos con transparencia, capturas | No | No | Medio |
| JPEG | Fotografias | No | No | Bajo-medio |
| GIF | Animaciones simples (legacy) | No | No | Variable |
| WebP | Alternativa moderna a JPEG/PNG | No | No | Bajo |
| AVIF | Alternativa moderna de alta compresion | No | No | Muy bajo |

---

## 1.4 Graficos Vectoriales vs Rasterizados

### Como funciona un grafico vectorial
- Se almacena como una **descripcion matematica**: "un circulo centrado en (50,50) con radio 30"
- El navegador/renderer **recalcula** la geometria cada vez que necesita dibujarlo
- El resultado visual depende del tamano de salida, no del tamano de almacenamiento
- Los graficos vectoriales son **resolución-independientes** por definicion

### Como funciona un grafico rasterizado (bitmap)
- Se almacena como una **cuadricula de pixeles**: cada pixel tiene un color especifico
- La resolucion esta fijada en el momento de creacion (ej: 100x100 pixeles)
- Escalar hacia arriba requiere **interpolar** pixeles: el resultado pierde nitidez
- Escalar hacia abajo **descarta** informacion de pixeles

### El problema del escalado en detalle
- Un PNG de 100x100px mostrado a 200x200px: el algoritmo de escalado "adivina" los pixeles intermedios → resultado borroso
- Un SVG mostrado a cualquier tamano: el renderer recalcula con precision matematica → siempre nítido
- Con pantallas Retina (2x), un PNG necesita ser el doble de grande en pixeles para verse nítido; SVG no tiene este problema

### Complejidad y tamano de archivo
- **Graficos simples**: SVG gana claramente (un circulo SVG pesa ~100 bytes; un PNG de 100x100 del mismo circulo pesa ~2KB)
- **Graficos medianamente complejos**: SVG sigue siendo competitivo
- **Graficos muy complejos** (miles de elementos, muchos colores): PNG/JPEG pueden ser mas eficientes
- Regla practica: si el SVG pesa mas de 50KB sin optimizar, cuestionar si SVG es el formato correcto

### Implicaciones para el desarrollo
- SVG permite **manipulacion programatica**: cambiar el color de un icono con CSS/JS es trivial
- Un PNG no tiene "partes" manipulables: es solo una grilla de pixeles
- SVG permite **animacion de sus partes**: un engranaje puede girar, un grafico puede dibujarse
- Los graficos raster requieren sprites pre-generados para animaciones

---

## 1.5 XML como base de SVG

### Que es XML
- **XML** = eXtensible Markup Language
- Lenguaje de marcado diseñado para **almacenar y transportar datos**
- A diferencia de HTML (diseñado para mostrar datos), XML es para estructurarlos
- SVG usa XML como sintaxis porque necesita describir estructuras jerarquicas de graficos

### Reglas de sintaxis XML que SVG hereda (criticas de memorizar)

**1. Todo elemento debe estar cerrado**
```
Correcto:  <rect width="100" height="50" />
Correcto:  <g> ... </g>
Incorrecto: <rect width="100" height="50">   (sin cerrar)
```

**2. Los atributos siempre van entre comillas**
```
Correcto:  width="100"
Correcto:  width='100'    (comillas simples tambien validas)
Incorrecto: width=100      (sin comillas, valido en HTML5 pero no en XML)
```

**3. Case-sensitive: mayusculas y minusculas importan**
```
Correcto:  <linearGradient>    (camelCase exacto)
Incorrecto: <lineargradient>   (todo minusculas, no existe)
Incorrecto: <LinearGradient>   (mayuscula inicial, no existe)
```

**4. Un unico elemento raiz**
- El documento XML debe tener exactamente un elemento raiz
- En SVG, ese elemento es `<svg>`
- Todo lo demas va dentro de `<svg>`

**5. Los atributos no se repiten**
```
Incorrecto: <rect fill="red" fill="blue" />   (fill duplicado, comportamiento indefinido)
```

**6. El documento debe estar "bien formado" (well-formed)**
- Todos los elementos abiertos deben cerrarse
- El anidamiento debe ser correcto (no se pueden entrelazar elementos)
- Los caracteres especiales deben escaparse: `&amp;` para `&`, `&lt;` para `<`, `&gt;` para `>`

### Diferencias importantes SVG vs HTML
| Aspecto | HTML | SVG/XML |
|---------|------|---------|
| Cierre de etiquetas | Opcional en muchos casos (`<li>`, `<p>`) | Obligatorio siempre |
| Atributos sin valor | Validos (`<input disabled>`) | No valido |
| Case sensitivity | Insensible (`<DIV>` = `<div>`) | Sensible (`<circle>` != `<Circle>`) |
| Caracteres especiales | Permisivo | Estricto (requiere escape) |
| Elemento raiz | `<html>` | `<svg>` |

### Namespaces XML en SVG
- Un **namespace** es un identificador unico que evita conflictos de nombres entre vocabularios XML
- SVG usa el namespace: `http://www.w3.org/2000/svg`
- XLink (para referencias) usa: `http://www.w3.org/1999/xlink`
- Los namespaces se declaran en el elemento raiz con `xmlns`
- En SVG inline dentro de HTML5, el namespace se infiere automaticamente y puede omitirse
- En archivos `.svg` independientes, el namespace es **obligatorio**

### Por que es importante entender XML
- Los errores de sintaxis XML hacen que el SVG no se renderice en absoluto
- Los mensajes de error del navegador para SVG mal formado pueden ser crípticos
- Al generar SVG programaticamente, hay que asegurarse de producir XML valido
- Al optimizar SVG manualmente, hay que respetar las reglas XML
