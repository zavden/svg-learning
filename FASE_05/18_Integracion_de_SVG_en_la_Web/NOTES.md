# Notas — Integración de SVG en la web

---

## Compatibilidad

- **SVG inline en HTML5**: soportado desde IE9. El parser de HTML5 reconoce `<svg>` sin necesidad de `xmlns`. En XHTML serializado el namespace sí es obligatorio.
- **`<img>` con SVG**: universal desde IE9. En IE9-11 el SVG debe declarar `width` y `height` o no se renderiza.
- **`background-image: url(*.svg)`**: universal desde IE9. Compatible con `background-size`, `repeat`, `position`.
- **`<object type="image/svg+xml">`**: universal. `getSVGDocument()` requiere mismo origen.
- **`<embed>`**: universal pero comportamiento variable. Su API no está estandarizada para SVG.
- **`<iframe>`**: universal. El atributo `sandbox` añade granularidad de permisos.
- **Data URI con SVG**: universal. Hay diferencias en qué caracteres aceptan los navegadores sin codificar.
- **`loading="lazy"`** en `<img>`: Chrome 76+, Firefox 75+, Safari 15.4+. No es nativo en `<object>` ni `<iframe>` (estos también lo soportan, pero con diferencias).

---

## El detalle del `xmlns`

En **HTML5** el parser ya sabe que `<svg>` pertenece al namespace SVG, así que técnicamente puedes omitir `xmlns="http://www.w3.org/2000/svg"`. Pero **en cuanto el SVG sale del HTML** (archivo externo, data URI, copiado a un email, abierto fuera del navegador), el namespace se vuelve obligatorio. **Regla práctica**: ponlo siempre.

```html
<!-- Funciona en HTML5 inline, pero frágil -->
<svg viewBox="0 0 24 24"><path d="..."/></svg>

<!-- Robusto en cualquier contexto -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path d="..."/></svg>
```

---

## El truco de pintar `<img>` SVG con `filter`

Cuando tienes un SVG cargado con `<img>` y necesitas cambiarle el color (no puedes editar el archivo, no puedes cambiar a inline), `filter` es la única salida. Existen generadores online como [codepen.io/sosuke/pen/Pjoqqp](https://codepen.io/sosuke/pen/Pjoqqp) que calculan la combinación de `invert + sepia + saturate + hue-rotate` para llegar a un color objetivo.

Limitación: solo funciona si el SVG es **monocromático**. Para SVG multicolor el resultado es impredecible.

---

## SVG sprite con `<use>` cross-document

Patrón clásico para reutilizar muchos íconos sin inflar el HTML:

```html
<!-- icons.svg -->
<svg xmlns="http://www.w3.org/2000/svg" style="display:none">
  <symbol id="check" viewBox="0 0 24 24"><path d="..."/></symbol>
  <symbol id="close" viewBox="0 0 24 24"><path d="..."/></symbol>
</svg>

<!-- en cualquier página -->
<svg width="24" height="24"><use href="/icons.svg#check"/></svg>
```

Ventajas: un solo archivo cacheado, cada uso pesa unos bytes. Pega: el `currentColor` funciona, pero estilizar partes individuales del símbolo desde fuera no (CSS shadow boundary).

---

## El problema del CSS heredado en `<img>`

Es tentador pensar que si pones `<img src="icon.svg" class="rojo">` y luego `img.rojo { fill: red; }` el SVG se va a poner rojo. **No funciona**: el `fill` no se aplica a `<img>` y, aunque lo hiciera, no atravesaría la frontera del SVG.

**Soluciones por orden de preferencia**:
1. Usa SVG inline.
2. En el SVG fuente pon `fill="currentColor"`. Luego `<img style="color: red">` no funciona, pero si pasas a inline o `<object>`, sí.
3. Genera el SVG en build time con el color correcto.
4. Truco `filter` (último recurso).

---

## Lazy loading de SVG inline

`<img loading="lazy">` solo aplica a `<img>`. Para SVG inline, **el SVG se descarga siempre** con el HTML, porque forma parte de él. Si tienes muchos íconos inline, considera:

- Migrar a sprite + `<use>`.
- Migrar a `<img>` con `loading="lazy"` para los que están bajo el fold.
- Splittear el SVG inline en componentes que solo se rendericen cuando se necesitan (frameworks).

---

## CSP y SVG

Las Content Security Policies pueden afectar a SVG de formas no obvias:

- **`script-src`** afecta a scripts dentro de SVG inline y de `<object>`/`<iframe>`. Si tu CSP es estricta (`script-src 'self'`), las animaciones JS dentro del SVG necesitarán nonce o hash.
- **`img-src`** controla las fuentes de `<img>` y de `background-image`. Un `img-src 'self'` bloquea data URIs salvo que añadas `data:`.
- **`frame-src`** controla `<iframe>`.
- **`object-src`** controla `<object>` y `<embed>`. Las CSPs modernas suelen ponerlo a `'none'`.

Si el navegador "no muestra" un SVG sin razón aparente, mira la consola: el bloqueo CSP es silencioso fuera de DevTools.

---

## Eventos `load` y `getSVGDocument()`

```js
const obj = document.querySelector('object');
obj.addEventListener('load', () => {
  const doc = obj.getSVGDocument();
  doc.querySelector('circle').setAttribute('fill', 'red');
});
```

Tres errores frecuentes:
1. **Llamar `getSVGDocument()` antes del evento `load`**: devuelve `null`.
2. **Cargar el SVG desde otro origen**: la política del mismo origen lo bloquea silenciosamente.
3. **Olvidar que cada `<object>` tiene su propio `document`**: las consultas con `document.querySelector` desde el padre no atraviesan la frontera.

---

## Por qué `<embed>` sigue existiendo

Es legado de la era de los plugins (Flash, RealPlayer, QuickTime). Sobrevive porque eliminarlo rompería páginas viejas, pero la web moderna no tiene razón para usarlo. Cualquier guía actual que recomiende `<embed>` para SVG está desactualizada.

---

## Build tools: lo mejor de los dos mundos

Frameworks modernos resuelven la tensión "inline vs externo" automáticamente:

- **Vite + `?react`** (vite-plugin-svgr): convierte cada SVG en un componente React inline en build time, conservando un único archivo fuente.
- **webpack + SVGR**: igual, para proyectos más antiguos.
- **Astro**: soporta `<Image>` con SVG y permite `import as React`.
- **Next.js**: nativamente trata `import logo from './logo.svg'` como ruta; con `@svgr/webpack` lo convierte a componente.
- **SVGO**: paso de optimización que debería estar en el pipeline de cualquier proyecto que use SVG, da igual cómo se incrusten.

Resultado: escribes SVGs como archivos sueltos (DX cómoda), el bundler los inlinea con árbol-shaking (rendimiento óptimo).

---

## Patrones útiles

### Ícono que cambia con el tema

```css
.icono { color: var(--text); }
@media (prefers-color-scheme: dark) {
  .icono { color: white; }
}
```

```html
<svg class="icono" aria-hidden="true"><path fill="currentColor" d="..."/></svg>
```

### Patrón decorativo de fondo (data URI)

```css
.fondo {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='40' height='40'%3E%3Ccircle cx='20' cy='20' r='2' fill='%23e5e7eb'/%3E%3C/svg%3E");
}
```

### Logo con fallback PNG

```html
<object type="image/svg+xml" data="logo.svg" aria-label="Marca">
  <img src="logo.png" alt="Marca">
</object>
```

---

## Debugging

- **El SVG no aparece**: comprueba `xmlns` en el archivo, verifica el `viewBox` y que `width`/`height` no sean 0.
- **El CSS externo no aplica**: probablemente está cargado con `<img>`. Pasa a inline o `<object>`.
- **`getSVGDocument()` devuelve `null`**: faltó esperar al `load` o el SVG es de otro origen.
- **El data URI no funciona en Firefox pero sí en Chrome**: revisa los caracteres especiales sin codificar; Firefox es más estricto con `<` `>` y comillas.
- **`background-image` no se ve en producción pero sí en local**: ruta relativa rota tras un build con hashing; usa rutas absolutas o el sistema de assets del bundler.
- **El sprite con `<use href>` no carga**: el atributo `href` (no `xlink:href`) requiere navegadores modernos y mismo origen.
