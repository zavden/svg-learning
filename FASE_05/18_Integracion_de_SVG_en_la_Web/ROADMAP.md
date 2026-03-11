# Seccion 18: Integracion de SVG en la Web

> Hay seis formas de incluir un SVG en una pagina web. Cada una tiene capacidades
> distintas en CSS, JavaScript, caching y accesibilidad. Elegir la correcta
> depende del caso de uso concreto.

---

## 18.1 SVG inline en HTML

### Como funciona
- El codigo SVG se escribe directamente dentro del HTML:
  ```html
  <body>
    <p>Texto anterior</p>
    <svg viewBox="0 0 100 100" width="50" height="50">
      <circle cx="50" cy="50" r="40" fill="steelblue" />
    </svg>
    <p>Texto posterior</p>
  </body>
  ```
- El SVG se convierte en parte del mismo arbol DOM que el HTML
- El navegador lo parsea como HTML5 (no como XML)

### Ventajas

**Acceso completo al DOM**
- JavaScript del documento puede seleccionar elementos SVG: `document.querySelector('circle')`
- Los elementos SVG se pueden modificar, agregar y eliminar como cualquier elemento HTML
- Los eventos del DOM (click, hover, focus) funcionan directamente

**Estilizacion con CSS externo**
- El CSS del documento aplica completamente al SVG inline
- `circle { fill: red; }` en el CSS externo cambia el circulo del SVG
- Las clases, ids, pseudo-clases y variables CSS funcionan sin restriccion

**Sin peticion HTTP adicional**
- El SVG ya esta en el HTML: no se necesita cargar un archivo separado
- Reduce el numero de peticiones de red (importante para performance)

**Mas facil accesibilidad**
- El contenido del SVG (texto, `<title>`, `<desc>`) es parte del documento
- Los lectores de pantalla lo procesan de forma mas predecible

### Desventajas

**No se cachea por separado**
- El SVG forma parte del HTML: se descarga y cachea junto con el HTML
- Si el mismo SVG se usa en multiples paginas, se descarga en cada una
- Los archivos externos SVG pueden cachearse independientemente

**Aumenta el peso del HTML**
- Un SVG complejo puede agregar kilobytes al HTML
- Impacta el Time to First Byte (TTFB) y el parseo inicial

**Dificultad de mantenimiento**
- Un SVG inline complejo dentro del HTML hace el codigo menos legible
- Para SVGs que se reutilizan en muchas paginas: mejor como archivo externo

**Namespace `xmlns` no es obligatorio**
- El parser HTML5 no requiere el atributo `xmlns="http://www.w3.org/2000/svg"`
- Pero si el SVG se extrae a un archivo `.svg`, si sera necesario

---

## 18.2 SVG como imagen con `<img>`

### Como funciona
```html
<img src="icono.svg" alt="Descripcion del icono" width="50" height="50">
```
- El navegador carga el archivo SVG como una imagen externa
- El SVG se trata como un bitmap opaco, igual que un PNG o JPEG

### Ventajas

**Simplicidad**
- La forma mas familiar y sencilla de incluir imagenes
- Compatible con todo el ecosistema de imagenes web (lazy loading, `srcset`, etc.)
- Fallback natural: si el SVG no carga, se muestra el `alt`

**Se cachea independientemente**
- El archivo `icono.svg` se cachea separadamente del HTML
- Si se usa en multiples paginas, se descarga una sola vez
- Los headers de cache controlan su duracion

**Animaciones SMIL funcionan**
- Las animaciones SMIL internas del SVG se ejecutan normalmente
- El SVG se mueve y anima dentro de la etiqueta `<img>`

**Seguridad**
- Los scripts dentro del SVG **no se ejecutan**
- Los estilos CSS externos no aplican al SVG
- El SVG esta "sandboxed": no puede acceder al DOM del documento padre

### Desventajas y limitaciones

**Sin CSS externo**
- Las reglas CSS del documento padre no afectan al interior del SVG
- Los selectores como `img.mi-clase path { fill: red; }` no funcionan
- El color del SVG solo puede cambiarse con CSS en el nivel del `<img>` (opacidad, filtros CSS)

**Sin JavaScript del padre**
- No se puede manipular los elementos internos del SVG con JS del documento
- `document.querySelector('img circle')` devuelve `null`

**Sin interaccion interna**
- Los elementos del SVG no pueden recibir eventos de forma individual
- El click se registra en el `<img>`, no en el elemento SVG especifico donde se hizo click

**Truco para cambiar el color de un SVG en `<img>`**
- CSS `filter` puede aproximar cambios de color: `filter: hue-rotate(90deg) saturate(2)`
- Para cambios precisos de color: usar SVG inline o `<object>`

---

## 18.3 SVG como fondo CSS

### Como funciona
```css
.icono {
  background-image: url('patron.svg');
  background-size: 50px 50px;
  background-repeat: repeat;
}
```

### Ventajas

**Control de tamano y repeticion con CSS**
- `background-size`: controla el tamano del SVG como fondo
- `background-repeat`: permite crear patrones repetidos facilmente
- `background-position`: controla la alineacion

**Ideal para patrones decorativos**
- Fondos con texturas o patrones SVG
- Separadores y decoraciones que no necesitan interactividad

### Desventajas y limitaciones

**Mismas limitaciones que `<img>`**
- Sin acceso al DOM interno del SVG
- Sin CSS externo aplicado al SVG
- Sin scripts

**Accesibilidad nula**
- Un SVG como background CSS es completamente invisible para lectores de pantalla
- Solo usar para decoracion pura, nunca para contenido informativo

**Cambiar colores: la tecnica de SVG inline en CSS**
- Es posible incrustar el SVG directamente en el CSS como data URI:
  ```css
  background-image: url("data:image/svg+xml,%3Csvg...%3E");
  ```
- El SVG se puede parametrizar (cambiar colores) generando el CSS dinamicamente
- Util para iconos de diferente color en distintos estados (normal, hover, activo)

---

## 18.4 SVG con `<object>`

### Como funciona
```html
<object type="image/svg+xml" data="diagrama.svg" width="400" height="300">
  <!-- Contenido fallback si el SVG no carga -->
  <img src="diagrama.png" alt="Diagrama de flujo del proceso" />
</object>
```

### Ventajas

**Acceso al DOM del SVG desde JavaScript externo**
- El metodo `getSVGDocument()` permite acceder al documento del SVG:
  ```javascript
  const obj = document.querySelector('object');
  const svgDoc = obj.getSVGDocument();
  svgDoc.querySelector('circle').setAttribute('fill', 'red');
  ```
- Permite manipulacion limitada del SVG desde el exterior

**Contenido fallback integrado**
- Todo lo que hay entre `<object>` y `</object>` se muestra si el SVG no puede cargarse
- El fallback puede ser una imagen PNG, texto descriptivo, o cualquier HTML

**El SVG tiene su propio contexto de documento**
- El CSS interno del SVG aplica normalmente
- Los scripts internos del SVG se ejecutan (diferente de `<img>`)

### Desventajas

**CSS externo no aplica directamente**
- Las reglas CSS del documento padre no llegan al interior del SVG
- El SVG tiene su propio documento aislado

**Mas complejo que `<img>`**
- Requiere especificar `type` y `data`
- El fallback hay que mantenerlo actualizado
- Menos familiar para muchos desarrolladores

**Menos usado hoy en dia**
- El metodo `getSVGDocument()` es de uso limitado y poco conocido
- Para casos donde se necesita manipulacion JS, SVG inline es mas practico

---

## 18.5 SVG con `<embed>`

### Como funciona
```html
<embed type="image/svg+xml" src="grafico.svg" width="400" height="300" />
```

### Diferencias con `<object>`
- Elemento de autocierre: no acepta contenido fallback
- Sin estandarizacion clara de comportamiento entre navegadores
- Comportamiento similar a `<object>` pero con menos caracteristicas

### Recomendacion
- **Evitar `<embed>` para SVG** en proyectos nuevos
- Usar `<object>` si se necesita ese nivel de embedding con fallback
- Usar SVG inline si se necesita manipulacion completa

---

## 18.6 SVG con `<iframe>`

### Como funciona
```html
<iframe src="grafico.svg" width="400" height="300" title="Grafico de ventas"></iframe>
```

### Ventajas

**SVG completamente aislado**
- El SVG tiene su propio documento, historial de navegacion y contexto JavaScript
- Los scripts internos del SVG se ejecutan en su propio contexto
- El CSS interno aplica normalmente

**Comunicacion via `postMessage`**
- El documento padre puede enviar mensajes al SVG en el iframe: `iframe.contentWindow.postMessage()`
- El SVG puede responder con `window.parent.postMessage()`
- Permite comunicacion bidireccional controlada entre el documento y el SVG

### Desventajas

**Iframe es pesado**
- Los iframes crean un contexto de navegacion completo: memoria y recursos adicionales
- Para un simple SVG, es excesivo

**Problemas de accesibilidad**
- Los iframes requieren `title` para ser accesibles
- El contenido del iframe puede no integrarse bien con la accesibilidad del documento padre

**Uso tipico**
- SVGs interactivos complejos que necesitan aislamiento completo (ej: un editor SVG embebido)
- Situaciones donde el SVG viene de un origen diferente y se necesita aislamiento de seguridad

---

## 18.7 SVG como data URI

### Como funciona

**En `<img>`**
```html
<img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 10 10'%3E%3Ccircle cx='5' cy='5' r='4'/%3E%3C/svg%3E" alt="Circulo" />
```

**En CSS**
```css
background-image: url("data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 10 10'><circle cx='5' cy='5' r='4'/></svg>");
```

### Dos tipos de codificacion

**URL-encoded (mas legible)**
- Los caracteres especiales se codifican: `<` → `%3C`, `>` → `%3E`, `#` → `%23`, etc.
- El SVG puede contener espacios y algunas comillas sin codificar (segun el contexto)
- Mas legible que base64 pero potencialmente mas largo

**Base64**
- `data:image/svg+xml;base64,[base64 del SVG]`
- El contenido es ilegible pero la longitud es fija (~33% mayor que el original)
- Util cuando el SVG contiene caracteres que son dificiles de URL-encodear

### Cuando usar data URIs para SVG

**Si usar**
- SVGs muy pequenios (< 1-2KB): el overhead de la peticion HTTP no justifica un archivo externo
- CSS generado dinamicamente donde el color del SVG cambia segun el contexto
- Emails HTML donde las referencias a archivos externos no funcionan
- Contextos sin servidor donde no se pueden servir archivos externos

**No usar**
- SVGs de tamano medio o grande: base64 los hace 33% mas grandes sin beneficio de cache
- SVGs reutilizados en multiples lugares: mejor un archivo externo cacheado

---

## 18.8 Comparativa de metodos de integracion

### Tabla completa de capacidades

| Metodo | CSS externo | JS externo | Animacion SMIL | Scripts internos | Cache | Accesible | Fallback |
|--------|-------------|------------|----------------|------------------|-------|-----------|---------|
| **Inline** | ✅ Completo | ✅ Completo | ✅ | ✅ | ❌ | ✅ Bueno | ❌ |
| **`<img>`** | ❌ | ❌ | ✅ | ❌ | ✅ | ✅ (alt) | ✅ (alt) |
| **CSS bg** | ❌ | ❌ | ✅ | ❌ | ✅ | ❌ | ❌ |
| **`<object>`** | ❌ (interno si) | getSVGDocument | ✅ | ✅ | ✅ | ✅ (fallback) | ✅ |
| **`<embed>`** | ❌ (interno si) | Limitado | ✅ | ✅ | ✅ | ❌ | ❌ |
| **`<iframe>`** | ❌ (interno si) | postMessage | ✅ | ✅ | ✅ | ⚠️ (title) | HTML |
| **Data URI** | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ (alt) | ✅ (alt) |

### Guia de decision rapida

**Necesito manipular el SVG con JS o CSS externo**
→ SVG **inline**

**Es un icono o imagen simple que no necesita interaccion**
→ `<img>` con `alt` descriptivo

**Es un patron decorativo de fondo**
→ CSS `background-image`

**Es un SVG con scripts internos y necesito fallback**
→ `<object>` con imagen PNG de fallback

**Es un SVG muy pequenio y quiero evitar una peticion HTTP**
→ Data URI (inline en CSS o `<img>`)

**El SVG viene de un origen externo y necesito aislamiento**
→ `<iframe>`

### El metodo mas versatil: inline

Para la mayoria de casos en aplicaciones web modernas, SVG inline es el metodo preferido porque:
- Maximo control con CSS y JavaScript
- Sin peticiones HTTP adicionales
- Mejor integracion con el flujo del documento
- Las desventajas (no cacheable, mas HTML) se mitigan con build tools que extraen SVGs

### Build tools y el "mejor de los dos mundos"

Herramientas modernas permiten escribir SVG en archivos separados (cacheables, editables) y tenerlos inline en el HTML final:
- **Vite + vite-plugin-svgr**: importar SVG como componentes React con contenido inline
- **webpack + svg-inline-loader**: inlining automatico de SVGs en el bundle
- **Eleventy, Hugo, Jekyll**: plugins para SVG inline en templates
- El resultado: archivos SVG separados en desarrollo, inline en produccion
