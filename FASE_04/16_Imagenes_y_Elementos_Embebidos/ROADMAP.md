# Seccion 16: Imagenes y Elementos Embebidos

> SVG no es solo vectores. Puede incrustar imagenes rasterizadas, HTML,
> e incluso otros SVGs. Cada mecanismo tiene sus reglas y sus casos de uso.

---

## 16.1 El elemento `<image>`

### Que hace
- Incrusta una **imagen externa** dentro del SVG
- La imagen puede ser rasterizada (PNG, JPEG, GIF, WebP) o otro SVG
- A diferencia de los graficos vectoriales del SVG contenedor, el contenido de `<image>` es bitmap (si es rasterizado)

### Atributos principales

**`href` (SVG 2) / `xlink:href` (SVG 1.1)**
- La **fuente de la imagen**
- Puede ser:
  - URL relativa: `href="foto.jpg"`
  - URL absoluta: `href="https://ejemplo.com/foto.png"`
  - Data URI: `href="data:image/png;base64,iVBOR..."` (imagen embebida directamente)
- Para compatibilidad: incluir ambos atributos `href` y `xlink:href`

**`x` y `y`**
- Posicion de la esquina superior izquierda de la imagen
- Por defecto: `x="0"` `y="0"`

**`width` y `height`**
- Dimensiones de visualizacion de la imagen en el SVG
- **Obligatorios**: sin ellos la imagen no se renderiza
- No necesariamente iguales a las dimensiones reales de la imagen: se puede redimensionar

**`preserveAspectRatio`**
- Controla como se ajusta la imagen dentro del area `width x height`
- Misma logica que el `preserveAspectRatio` del elemento `<svg>`
- `xMidYMid meet` (por defecto): la imagen se ajusta para caber completa, centrada
- `xMidYMid slice`: la imagen cubre toda el area, puede recortarse
- `none`: la imagen se estira para llenar exactamente el area (puede distorsionarse)

### SVG incrustado con `<image>`

- Se puede usar `<image>` para incrustar otro SVG: `href="icono.svg"`
- El SVG incrustado se trata como una imagen opaca: no se puede acceder a su DOM
- Los estilos del SVG contenedor no afectan al SVG incrustado
- Las animaciones SMIL del SVG incrustado SI funcionan
- Los scripts del SVG incrustado NO se ejecutan

### Interaccion con clipPath y mask

- `<image>` puede tener `clip-path` y `mask` aplicados
- Permite recortar la imagen en cualquier forma SVG: estrella, circulo, letra, etc.
- Patron comun para avatares circulares:
  - `<clipPath>` con un `<circle>`
  - `<image>` con `clip-path="url(#miCirculo)"` aplicado

---

## 16.2 El elemento `<foreignObject>`

### Que es y para que sirve
- Permite incrustar **contenido que no es SVG** dentro de un SVG
- El caso de uso mas comun: HTML dentro del SVG
- El namespace del contenido interno debe declararse explicitamente (xmlns de XHTML)

### Atributos

**`x`, `y`, `width`, `height`**
- Posicion y tamano del area donde se muestra el contenido externo
- El contenido se renderiza dentro de este rectangulo
- El contenido puede hacer scroll si es mas grande que el area (dependiendo del CSS)

### Contenido HTML dentro de SVG

**Estructura tipica**
```xml
<foreignObject x="10" y="10" width="200" height="150">
  <div xmlns="http://www.w3.org/1999/xhtml">
    <p>Parrafo de texto con <strong>HTML</strong> real.</p>
    <ul>
      <li>Lista</li>
    </ul>
  </div>
</foreignObject>
```

**Por que es util**
- El HTML dentro del foreignObject tiene word-wrap nativo
- Se pueden usar todos los elementos HTML: listas, tablas, formularios, inputs
- El CSS del documento padre aplica al contenido HTML dentro del foreignObject
- Permite crear diagramas con etiquetas de texto complejas y bien formateadas

### Limitaciones de `<foreignObject>`

**Soporte inconsistente**
- El soporte varia entre navegadores, especialmente para contenido complejo
- En SVG como archivo externo (`<img src="...svg">`): el foreignObject no se renderiza
- En SVG inline en HTML: funciona en navegadores modernos

**No funciona en todos los contextos**
- SVG como imagen (`<img>`, CSS `background-image`): el foreignObject es ignorado
- SVG generado para impresion o PDF: soporte variable
- SVG en algunos entornos de servidor o herramientas de conversion

**Seguridad**
- El HTML dentro puede ejecutar scripts si el SVG esta inline
- Si el SVG viene de una fuente externa, el foreignObject es un vector de XSS potencial
- Los sanitizadores de SVG suelen eliminar `<foreignObject>`

### Casos de uso validos de `<foreignObject>`

- **Etiquetas de texto multilinea** en diagramas que necesitan word-wrap
- **Formularios dentro de SVG** (aunque es raro y hay mejores alternativas)
- **Contenido mixto** en visualizaciones complejas donde parte del contenido es HTML nativo
- **Herramientas de diagramacion** como mermaid.js o D3 que usan foreignObject para etiquetas

---

## 16.3 SVGs anidados

### Cuando se usa un `<svg>` dentro de otro `<svg>`

- Un elemento `<svg>` puede aparecer como hijo de otro elemento `<svg>`
- Crea un **nuevo viewport** y, si tiene `viewBox`, un nuevo sistema de coordenadas
- Es diferente a un grupo `<g>`: el SVG anidado tiene su propio sistema de coordenadas aislado

### Atributos del SVG anidado

**`x` y `y`**
- Posicion del SVG anidado dentro del padre (en el sistema de coordenadas del padre)

**`width` y `height`**
- Tamano del viewport del SVG anidado (en unidades del padre)

**`viewBox`**
- Si se especifica: crea un sistema de coordenadas interno independiente
- Si se omite: el SVG anidado usa las mismas coordenadas que el padre (escaladas al width/height)

**`overflow`**
- Por defecto en SVG anidados: `overflow="hidden"` — el contenido se recorta al viewport del hijo
- Con `overflow="visible"`: el contenido puede salir del viewport del hijo

### Casos de uso de SVGs anidados

**Composicion con diferentes escalas**
- Una ilustracion principal con viewBox grande
- Un detalle ampliado incrustado como SVG anidado con su propio viewBox pequenio

**Reutilizacion de un SVG en diferentes posiciones**
- Incrustar el mismo SVG varias veces en diferentes posiciones del padre
- Cada instancia puede tener su propio viewBox para un ajuste distinto

**Aislamiento de sistemas de coordenadas**
- Cuando parte de una ilustracion compleja tiene su propio sistema de coordenadas
- Evita que las transformaciones de una parte afecten a otras

---

## 16.4 Imagenes base64 embebidas

### Que son las data URIs
- Una forma de incrustar datos binarios (como imagenes) directamente en el texto
- El contenido de la imagen se convierte a texto **codificado en base64**
- Se incluye directamente en el atributo `href` de `<image>` o en CSS

### Sintaxis
```
href="data:[tipo-mime];base64,[datos-en-base64]"
```

**Ejemplos**
- PNG: `href="data:image/png;base64,iVBORw0KGgo..."`
- JPEG: `href="data:image/jpeg;base64,/9j/4AAQ..."`
- SVG: `href="data:image/svg+xml;base64,PHN2Zy..."`
- SVG sin base64 (URL-encoded): `href="data:image/svg+xml,%3Csvg..."`

### Ventajas de embeber imagenes en base64

**Archivo autocontenido**
- El SVG no depende de archivos externos
- Se puede mover, copiar o enviar el SVG como un solo archivo y funciona igual
- No hay peticiones HTTP adicionales al cargar la imagen

**Sin problemas de CORS**
- Las restricciones de Cross-Origin no aplican a data URIs
- Una imagen de otro dominio embebida en base64 no causa errores CORS

### Desventajas de embeber imagenes en base64

**Tamano de archivo significativamente mayor**
- Base64 incrementa el tamano del dato original aproximadamente un **33%**
- Una imagen PNG de 100KB se convierte en ~133KB de texto base64
- El SVG total crece sustancialmente

**Sin cache independiente**
- El archivo SVG completo se cachea como unidad
- La imagen embebida no puede cachearse por separado ni compartirse entre SVGs
- Si multiples SVGs usan la misma imagen, cada uno tiene su propia copia

**Legibilidad nula**
- El codigo SVG se vuelve ilegible con grandes bloques de base64
- Dificil de mantener manualmente

### Cuando usar imagenes embebidas en base64

**Si** usar base64:
- SVG enviado como archivo adjunto (email, descarga)
- SVG generado programaticamente que debe ser autocontenido
- Iconos pequenios con imagenes de referencia diminutas
- Cuando el entorno no permite peticiones HTTP externas

**No** usar base64:
- SVGs en produccion web con imagenes de tamano normal
- Cuando multiples elementos usan la misma imagen (mejor una URL externa)
- Cuando el rendimiento y el tamano de archivo son criticos

### Conversion a base64
- Herramientas online: multiple disponibles para convertir archivos a data URI
- JavaScript: `FileReader.readAsDataURL()`, `btoa()`, `Buffer.from().toString('base64')`
- CLI: `base64 imagen.png` (Linux/macOS)
- El resultado debe prefijarse con `data:image/[tipo];base64,` antes de usarse

### SVG dentro de SVG via data URI

Un caso especial: incrustar un SVG dentro de otro SVG como data URI:
- `href="data:image/svg+xml;utf8,<svg>...</svg>"` (URL-encoded)
- O en base64: `href="data:image/svg+xml;base64,[base64 del SVG]"`
- El SVG embebido se comporta como una imagen opaca (igual que con `href="icono.svg"`)
- Util para generar SVGs complejos completamente autocontenidos
