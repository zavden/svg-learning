# Notas — Imágenes y elementos embebidos

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `<image>` con `href` | ✓ | ✓ | ✓ | ✓ |
| `<image>` con `xlink:href` (legacy) | ✓ | ✓ | ✓ | ✓ |
| `preserveAspectRatio` en `<image>` | ✓ | ✓ | ✓ | ✓ |
| Filtros CSS en `<image>` | ✓ | ✓ | ✓ | ✓ |
| `<foreignObject>` en SVG inline | ✓ | ✓ | ✓ | ✓ |
| `<foreignObject>` en `<img src>` | ✗ | ✗ | ✗ | ✗ |
| SVG anidado con `viewBox` propio | ✓ | ✓ | ✓ | ✓ |
| Data URI `base64` en `href` | ✓ | ✓ | ✓ | ✓ |
| Data URI `utf8` (URL-encoded) en `href` | ✓ | ✓ | ✓ | ✓ |
| `image-rendering: pixelated` | ✓ | ✓ | ✓ | ✓ |

El patrón `href` sin prefijo `xlink:` es seguro desde ~2018 en todos los navegadores modernos. Si necesitas compatibilidad con IE11 o motores muy antiguos, declara ambos: `href="..."` y `xlink:href="..."`.

---

## `href` vs `xlink:href`

La razón histórica:

1. **SVG 1.1 (hasta ~2018)**: `xlink:href` era el estándar, heredado del lenguaje XLink. Requería declarar `xmlns:xlink="http://www.w3.org/1999/xlink"` en la raíz del SVG.
2. **SVG 2**: `href` nativo (sin prefijo) se convirtió en el atributo canónico. Más simple, sin namespace extra.

**Recomendación actual**: usar `href`. Si detectas problemas en motores muy antiguos, agrega `xlink:href` como fallback:

```svg
<image href="logo.png" xlink:href="logo.png" width="100" height="100"/>
```

El navegador prefiere `href` cuando ambos están presentes.

---

## `<foreignObject>`: cuándo es la opción correcta

**Sí, úsalo cuando**:
- el SVG siempre vivirá inline dentro de una página HTML renderizada por un navegador moderno
- necesitas texto multilínea con word-wrap automático
- quieres insertar un formulario, botón, input, textarea, checkbox, etc.
- necesitas tablas complejas con layouts de HTML
- quieres aprovechar CSS completo (flexbox, grid, animaciones)

**No, busca alternativas cuando**:
- el SVG se exportará como archivo independiente
- el SVG se procesará con herramientas de conversión (Inkscape, rsvg-convert, librsvg)
- el SVG se embeberá con `<img src="...">` o como `background-image`
- el SVG debe ser compatible con sanitizadores que eliminan `<foreignObject>` por seguridad
- necesitas compatibilidad amplia con renderizadores no-navegador

Para texto multilínea portable, usa `<text>` con múltiples `<tspan>` y `dy="1.2em"`. Para elementos interactivos, posiciona el HTML **fuera** del SVG con `position: absolute` alineado a las coordenadas del canvas.

---

## SVG anidado: tres casos de uso reales

### 1. Widgets reutilizables con coordenadas cómodas

```svg
<!-- Define un gráfico en coordenadas 0–100 -->
<svg x="20" y="20" width="120" height="120" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>

<!-- Repítelo a otro tamaño sin recalcular -->
<svg x="160" y="20" width="60" height="60" viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>
```

Alternativa: `<symbol>` + `<use>` — más conciso para reutilización pura. Usa SVG anidado cuando necesitas también ajustar `preserveAspectRatio` o modificar contenido en cada instancia.

### 2. Vista + detalle (zoom-in) en el mismo canvas

Un mapa a escala normal más un "recuadro" que muestra una zona ampliada. El recuadro es un `<svg>` anidado con un `viewBox` más pequeño que solo enmarca la zona de interés.

### 3. Recorte automático rectangular

`<svg>` anidado recorta por defecto. Si quieres una ventana donde el contenido interno no pueda "escaparse", es más barato que definir un `clipPath` rectangular.

---

## Patrones no obvios

### Imagen que llena un rectángulo cualquiera sin deformarse

```svg
<svg x="20" y="20" width="200" height="80" viewBox="0 0 200 80"
     preserveAspectRatio="xMidYMid slice">
  <image href="foto.jpg" x="0" y="0" width="200" height="80"
         preserveAspectRatio="xMidYMid slice"/>
</svg>
```

El SVG anidado recorta y la imagen interna cubre todo el espacio, incluso si la foto tiene otro aspect ratio.

### Aplicar filtros a imágenes

Los filtros SVG funcionan sobre `<image>`, lo que convierte el SVG en un editor mini:

```svg
<filter id="gray">
  <feColorMatrix type="saturate" values="0"/>
</filter>
<image href="foto.jpg" width="200" height="200" filter="url(#gray)"/>
```

Combinable con `<feGaussianBlur>`, `<feColorMatrix>`, `<feComponentTransfer>`, etc. Todo lo que aprendiste en filtros aplica aquí.

### Obtener los píxeles de una imagen

Si cargas una imagen externa en `<image>` y luego procesas el SVG con canvas (`drawImage`), la imagen debe ser same-origin o venir con headers CORS adecuados — de lo contrario el canvas queda "tainted". **Los data URI nunca tienen este problema** porque no son recursos externos.

### Base64 inline dentro de CSS

Los data URIs también funcionan en CSS `background-image`:

```css
.icon {
  background-image: url('data:image/svg+xml;utf8,<svg ...>...</svg>');
}
```

Para SVG en CSS, la forma URL-encoded es preferible — más legible y funcional sin base64.

---

## Rendimiento

- **Imágenes grandes en base64**: desaconsejado. El SVG debe parsearse completo antes de renderizar, y el browser no puede cachear la imagen independientemente.
- **Muchos `<foreignObject>`**: cada uno abre un nuevo motor de layout HTML dentro del SVG. Coste medible si tienes docenas. Preferible posicionar HTML externo con `position: absolute`.
- **SVG anidado profundo**: cada nivel anidado añade un cambio de contexto. Mantén la anidación a 2–3 niveles máximo en SVGs interactivos.
- **`preserveAspectRatio="none"`**: la opción más barata porque no requiere cálculos de alineación. Útil en texturas y fondos.
- **Filtros sobre `<image>`**: la imagen se rasteriza una vez al cargar y el filtro se aplica sobre el bitmap. Coste inicial alto pero buen cache posterior.

---

## Debugging

- **La imagen no aparece**: comprueba (1) que `href` está bien escrito, (2) que declaraste `width` y `height`, (3) que el servidor sirve la imagen con el CORS correcto si vas a procesar el canvas, (4) que no hay un `clip-path` ocultándola sin querer.
- **`<foreignObject>` vacío o invisible**: falta `xmlns="http://www.w3.org/1999/xhtml"` en el primer elemento HTML interno.
- **El SVG anidado no respeta su viewBox**: asegúrate de que `viewBox` está en el `<svg>` anidado, no en el padre. Si el padre tiene su propio viewBox, el hijo hereda solo si no declara el suyo.
- **Contenido desbordando del SVG anidado**: es el comportamiento por defecto (recorte). Agrega `overflow="visible"` para verlo.
- **Data URI base64 corrupto**: el comando `base64` en Linux añade saltos de línea cada 76 caracteres por defecto. Usa `base64 -w 0` para una sola línea.
- **Data URI URL-encoded con SVG**: asegúrate de codificar `<`, `>`, `"`, `#`, `%` (`#` se convierte en `%23`, `%` en `%25`). Una cadena sin codificar el `#` de colores (`fill="#ff0000"`) fallará en algunos navegadores.

---

## Fragmentos útiles

### Avatar circular desde foto

```svg
<defs>
  <clipPath id="avatar">
    <circle cx="40" cy="40" r="40"/>
  </clipPath>
</defs>
<image href="user.jpg" x="0" y="0" width="80" height="80"
       preserveAspectRatio="xMidYMid slice"
       clip-path="url(#avatar)"/>
```

### Logo que siempre cabe entero

```svg
<image href="logo.svg" x="0" y="0" width="200" height="60"
       preserveAspectRatio="xMidYMid meet"/>
```

### Fondo de patrón que llena cualquier caja

```svg
<image href="patron.png" x="0" y="0" width="300" height="200"
       preserveAspectRatio="none"/>
```

### Convertir SVG a data URI desde Node

```js
import fs from 'fs';
const svg = fs.readFileSync('icon.svg', 'utf8');
const uri = 'data:image/svg+xml;utf8,' + encodeURIComponent(svg);
```

### Convertir PNG a data URI en terminal

```bash
printf "data:image/png;base64,"
base64 -w 0 icon.png
```
