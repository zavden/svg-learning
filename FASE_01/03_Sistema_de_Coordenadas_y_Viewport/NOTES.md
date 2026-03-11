# Notas de compatibilidad y referencias extra — Sección 03

Observaciones que no encajan en la teoría principal: quirks de navegadores, patrones avanzados, y cosas a revisar cuando algo no funciona como se espera.

---

## Compatibilidad de navegadores

### IE11 y viewBox sin width/height explícito

Internet Explorer 11 no puede calcular las proporciones de un SVG inline cuando solo tiene `viewBox` pero no `width` y `height`. El SVG colapsa a altura 0.

**Solución para IE11:**
```html
<!-- Workaround para IE11: añadir width/height aunque uses CSS para el tamaño -->
<svg viewBox="0 0 100 100" width="100%" height="auto">
  ...
</svg>
```
O usar el truco del `padding-top` en el contenedor:
```html
<div style="position:relative; padding-top:100%; /* ratio 1:1 */">
  <svg viewBox="0 0 100 100"
       style="position:absolute; top:0; left:0; width:100%; height:100%;">
  </svg>
</div>
```
> **Nota:** IE11 tiene soporte en < 1% del mercado global (2025). Solo relevante si tu proyecto tiene requisitos legacy explícitos.

---

### Safari y `height: auto` en SVG

Safari (especialmente versiones anteriores a 15) puede ignorar `height: auto` en SVGs inline. El SVG puede colapsar o no mantener las proporciones.

**Workaround:**
```css
/* En vez de height:auto, usa aspect-ratio (Chrome 88+, Firefox 89+, Safari 15+) */
svg {
  width: 100%;
  aspect-ratio: attr(viewBox) / 1; /* experimental */
}

/* O simplemente dale una altura explícita al contenedor */
.svg-container {
  width: 100%;
  aspect-ratio: 4 / 3; /* si tu viewBox es 400×300 = ratio 4:3 */
}
```

---

### `preserveAspectRatio` en SVG como `<img>`

Cuando un SVG se incrusta como `<img src="archivo.svg">`, el `preserveAspectRatio` funciona correctamente en todos los navegadores modernos. Sin embargo, **JavaScript no puede interactuar** con el contenido del SVG.

Para animaciones o interactividad, usa SVG inline (directamente en el HTML) o via `<object>` o `<embed>`.

---

### `%` en `height` de SVG inline (Firefox)

Firefox puede tener comportamientos inesperados con `height="100%"` en SVG inline cuando el elemento padre no tiene altura explícita. Chrome suele manejar esto mejor.

**Regla práctica:** si necesitas que un SVG inline llene la altura de su contenedor, da altura explícita al contenedor con CSS, y luego usa `height="100%` en el SVG. O usa `viewBox` + CSS `aspect-ratio`.

---

## Patrones recomendados

### Patrón 1: SVG responsivo estándar (el más recomendado para web)

```html
<svg viewBox="0 0 200 100" xmlns="http://www.w3.org/2000/svg"
     style="width:100%; height:auto; display:block;">
  <!-- Contenido con coordenadas en user units -->
</svg>
```

- No tiene `width`/`height` fijos en el atributo → el CSS controla el tamaño
- `viewBox` establece el sistema de coordenadas y las proporciones
- `display:block` evita espacio extra debajo del SVG (comportamiento inline)
- Funciona en todos los navegadores modernos

---

### Patrón 2: SVG de dimensión fija (iconos, logos no-responsivos)

```html
<svg width="24" height="24" viewBox="0 0 24 24"
     xmlns="http://www.w3.org/2000/svg">
  <!-- Contenido diseñado en un espacio 24×24 -->
</svg>
```

- El `viewBox="0 0 24 24"` permite diseñar en coordenadas convenientes
- `width="24" height="24"` fija el tamaño en pantalla
- Estándar para icon sets (Material Icons, Heroicons, etc.)
- Si necesitas escalarlo con CSS: `width: 48px; height: 48px;` sobreescribe los atributos (CSS tiene mayor especificidad que atributos de presentación)

---

### Patrón 3: SVG como fondo CSS (para fondos decorativos)

```css
.hero {
  background-image: url("fondo.svg");
  background-size: cover;      /* equivalente a slice */
  background-position: center; /* equivalente a xMidYMid */
}
```

El SVG externo debe tener `viewBox` y opcionalmente `preserveAspectRatio="xMidYMid slice"` para que el comportamiento sea predecible.

---

## Cosas a revisar cuando algo no funciona

| Síntoma | Causa probable | Qué revisar |
|---|---|---|
| El SVG no se ve | viewBox con valores 0 o negativos | Verificar los 4 valores del viewBox |
| El SVG está en (0,0) y no donde esperaba | `x`/`y` en `<circle>` en vez de `cx`/`cy` | Verificar el tipo de elemento |
| El SVG se ve plano/estirado | `preserveAspectRatio="none"` activo | Revisar el atributo o quitar el default |
| El SVG no escala responsivo | Falta `viewBox` | Añadir viewBox con las proporciones correctas |
| El contenido aparece desplazado | min-x o min-y del viewBox no son 0 | Verificar los dos primeros valores del viewBox |
| Elemento no visible pero en el DOM | Está fuera del viewport y `overflow:hidden` | Añadir `overflow="visible"` al SVG o ajustar coords |
| Elemento rotado en posición incorrecta | `rotate()` sin punto de pivote | Usar `rotate(deg, cx, cy)` |
| SVG inline con altura 0 en IE11 | viewBox sin width/height explícitos | Añadir `width="100%" height="auto"` |

---

## Recursos adicionales para profundizar

- **SVG Coordinate Systems** — Sara Soueidan: artículo exhaustivo sobre viewBox y transformaciones (buscar en su blog)
- **MDN Web Docs: viewBox** — referencia oficial con todos los edge cases documentados
- **SVG 2 spec** — W3C: la especificación completa (árida pero definitiva para dudas técnicas)
- **SVGOMG** — herramienta online para ver cómo los optimizadores modifican el viewBox y preserveAspectRatio
- **CSS `aspect-ratio`** — alternativa moderna para mantener proporciones sin viewBox en algunos contextos

---

## Temas avanzados para revisitar (Fase 7)

Estos conceptos de la sección 03 tienen profundidad adicional que se cubre en fases posteriores:

- **CTM (Current Transformation Matrix)** — la matriz de transformación compuesta que el navegador calcula para cada elemento. Relevante para animaciones precisas y para convertir coordenadas de mouse a coordenadas SVG (sección 21).
- **`getScreenCTM()` y `getClientBoundingRect()`** — APIs del DOM para convertir entre sistemas de coordenadas. Esenciales para interactividad avanzada.
- **`getBBox()`** — devuelve el bounding box de un elemento en coordenadas de usuario, sin tener en cuenta transformaciones del ancestro.
- **Animación del viewBox** — hacer zoom/pan animado modificando el viewBox. Técnica usada en visualizaciones de datos. Ver sección 20.
- **`viewBox` en elementos que no son `<svg>`** — `<marker>`, `<pattern>`, `<symbol>` también tienen `viewBox`. Se cubre en secciones 10, 12 y 15.
