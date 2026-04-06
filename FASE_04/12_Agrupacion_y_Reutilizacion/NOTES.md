# Notas — Agrupación y Reutilización

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| `<g>` | ✓ | ✓ | ✓ | ✓ |
| `<use href="#id">` (interno) | ✓ | ✓ | ✓ | ✓ |
| `<use xlink:href="#id">` (legacy) | ✓ | ✓ | ✓ | ✓ |
| `<use href="file.svg#id">` (externo) | ✓ | ✓ | ✓ | ✓ |
| `<symbol>` con `viewBox` | ✓ | ✓ | ✓ | ✓ |
| `<defs>` | ✓ | ✓ | ✓ | ✓ |
| `currentColor` en `<symbol>` | ✓ | ✓ | ✓ | ✓ |
| CSS custom properties en `<use>` | ✓ | ✓ | ✓ | ✓ |
| Estilizar hijos de `<use>` con selectores CSS externos | ✗ | ✗ | ✗ | ✗ |

IE11 y Edge Legacy no soportaban referencias externas con `<use href="file.svg#id">` — requerían el polyfill `svgxuse`. En navegadores actuales funciona sin problemas.

---

## `href` vs `xlink:href`

SVG 2 introdujo `href` como el atributo estándar, sin namespace. Todos los navegadores modernos lo soportan. `xlink:href` sigue funcionando por compatibilidad, pero requiere declarar el namespace en el `<svg>` raíz:

```svg
<svg xmlns="http://www.w3.org/2000/svg"
     xmlns:xlink="http://www.w3.org/1999/xlink">
  <use xlink:href="#icono"/>
</svg>
```

**Recomendación:** usar solo `href` en proyectos nuevos. Solo añadir `xlink:href` si hace falta soporte para navegadores anteriores a 2017.

---

## `<g>` vs `<symbol>` — comparación

| Aspecto | `<g>` | `<symbol>` |
|---------|-------|------------|
| Se renderiza directamente | Sí | No (solo vía `<use>`) |
| `viewBox` propio | No | Sí |
| `preserveAspectRatio` propio | No | Sí |
| `width`/`height` en `<use>` lo escala | No | Sí |
| Atributos de presentación heredables | Sí | Sí |
| Semántico para iconos | Regular | Ideal |
| Uso típico | Agrupación, transform común | Componentes reutilizables |

---

## Técnicas de estilizado de `<use>`

### `currentColor` (un color por instancia)

```svg
<symbol id="icon" viewBox="0 0 24 24">
  <path fill="currentColor" d="..."/>
</symbol>

<!-- En el HTML -->
<use href="#icon" style="color:#ef4444"/>
<use href="#icon" style="color:#3b82f6"/>
```

### Variables CSS (varios colores por instancia)

```svg
<symbol id="icon" viewBox="0 0 24 24">
  <path fill="var(--fg, black)" d="..."/>
  <path fill="var(--bg, white)" d="..."/>
</symbol>

<use href="#icon" style="--fg:#ef4444; --bg:#fef2f2"/>
```

### Hereditar `fill`/`stroke` del `<use>`

Funciona solo si los elementos del `<symbol>` no tienen `fill` propio:

```svg
<symbol id="icon" viewBox="0 0 24 24">
  <path d="..."/>  <!-- sin fill -->
</symbol>

<use href="#icon" fill="#10b981"/>
```

---

## Sprites externos: nota sobre CORS

Un sprite `sprite.svg` servido desde otro origen requiere cabeceras CORS:

```
Access-Control-Allow-Origin: *
```

Sin CORS, el navegador bloqueará la carga del sprite externo en algunos contextos (especialmente con `<use>` cross-origin). Para desarrollo local con `file://`, Firefox suele fallar más que Chrome.

---

## Patrones útiles

### SVG raíz como fuente de estilo

Definir `fill`, `stroke`, `stroke-width` y `font-family` directamente en el `<svg>`:

```svg
<svg fill="none" stroke="currentColor" stroke-width="2" font-family="sans-serif">
  <!-- todos los hijos heredan estos valores -->
</svg>
```

Ideal para líneas de iconos que toman el color del texto del contenedor HTML.

### Namespace para grupos en animaciones

Usar clases o IDs en `<g>` para que JavaScript pueda encontrar grupos específicos:

```html
<svg>
  <g class="constelacion" id="orion">
    <!-- estrellas -->
  </g>
</svg>

<script>
  document.querySelectorAll('.constelacion').forEach(g => {
    g.addEventListener('click', () => g.classList.toggle('activa'));
  });
</script>
```

### `<use>` con transformaciones encadenadas

Un `<use>` puede recibir su propio `transform`, que se aplica después del `x`/`y`:

```svg
<use href="#icono" x="100" y="50" transform="rotate(45)"/>
```

El `transform` se aplica al sistema de coordenadas del `<use>`, no al elemento original.

---

## Trucos y comportamientos sorpresivos

**Un `<symbol>` no hereda del `<svg>` padre en el sentido de coordenadas.** Tiene su propio `viewBox`; el `width`/`height` del `<use>` determina el tamaño final.

**`<g>` con `transform` crea un nuevo sistema de coordenadas.** Los hijos se posicionan relativos a ese sistema; si un hijo tiene su propio `transform`, se compone con el del padre.

**`opacity` en un grupo no es igual a `opacity` en cada hijo.** El grupo se renderiza primero a un buffer, luego se le aplica la opacidad como una única capa. Los hijos no se mezclan entre sí.

**Los atributos de presentación pierden frente al CSS.** Esto se puede aprovechar para tematizar un SVG externo sin modificarlo: una hoja de estilos puede sobrescribir los `fill` que trajo el archivo original.

**`<defs>` puede aparecer después de su uso.** El navegador resuelve las referencias en un segundo paso, no secuencialmente. Convencionalmente se pone al inicio por legibilidad, no por necesidad técnica.

**`<use>` sobre un `<svg>` externo no se puede estilizar con CSS desde la página principal.** El contenido viene de otro archivo y el shadow DOM aísla los estilos aún más. Usar `currentColor` es prácticamente la única salida.

---

## JS: manipular elementos dentro de `<g>` vs dentro de `<use>`

```js
// Dentro de <g>: accesible con querySelector
document.querySelector('#cabeza circle').setAttribute('fill', 'red');

// Dentro de <use>: NO accesible directamente
// document.querySelector('#mi-use circle')  ← null
// Hay que estilizar con currentColor o variables CSS
document.querySelector('#mi-use').style.color = 'red';
```

Si se necesita manipular cada elemento interno individualmente, usar `<g>` con IDs en vez de `<symbol>` + `<use>`.
