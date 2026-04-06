# NOTES — Animaciones en SVG

Detalles de compatibilidad, trampas prácticas y trucos que no caben en la teoría principal.

---

## Compatibilidad por sistema (estado en 2026)

**SMIL**
- Soportado en Chrome, Firefox, Safari, Edge.
- IE nunca lo soportó (ya irrelevante).
- Funciona incluso cuando el SVG está en `<img>` (única ventaja única frente a CSS).
- La mala fama de "deprecated" sigue circulando, pero es falsa: Chrome revirtió la deprecación en 2016.

**CSS Animations/Transitions en SVG**
- Fill, stroke, opacity, transform, stroke-dasharray/offset: soporte universal.
- `r`, `cx`, `cy`, `x`, `y`, `width`, `height` como propiedades animables: SVG 2, Chrome 65+, Firefox parcial (revisa caniuse antes de apoyarte en ellos).
- Animar `d` vía CSS: ni siquiera Chrome interpola — cambia de golpe.

**Web Animations API**
- `element.animate()` disponible en todos los navegadores modernos. Para SVG funciona igual que en HTML siempre y cuando la propiedad sea animable.

---

## El atributo `d` — el caso difícil

Solo hay tres formas de morphear un path:
1. **SMIL** con `<animate attributeName="d" values="...;..."/>`. Los paths deben tener *exactamente* el mismo número de comandos y puntos.
2. **JavaScript con interpolación manual**: parsea el `d`, interpola punto a punto, reasigna. Tedioso.
3. **Flubber** (Mike Bostock) — resuelve automáticamente el problema del "número de puntos distinto". Uso: `flubber.interpolate(pathA, pathB)` devuelve una función `t → d`. Combina con GSAP/Motion para el timing.

Recomendación: si el proyecto ya usa una librería de animación, usa `Flubber` + esa librería. Si es un SVG autocontenido, `SMIL` sigue siendo la respuesta más directa.

---

## `transform-origin` en SVG — la trampa silenciosa

En HTML, `transform-origin` por defecto es `50% 50%` (el centro del elemento).
En SVG, el comportamiento histórico era usar el origen del **viewBox**, no el del elemento. Eso provoca que una rotación "orbite" en vez de "girar".

**Cómo arreglarlo**:
- **CSS**: `transform-origin: center;` o coordenadas absolutas `transform-origin: 100px 100px;`
- **SMIL `animateTransform`**: no hay `transform-origin`; el truco es usar `rotate(angle cx cy)` (tres números) para dar el centro directamente, o envolver el elemento en un `<g transform="translate(cx cy)">` y animar el hijo con coordenadas locales.

Los navegadores modernos han ido convergiendo a "center" como default en SVG cuando se usa `transform-box: fill-box`, pero para máxima compatibilidad declara siempre el origen explícito.

---

## Debugging de animaciones SMIL

Las DevTools de Chrome/Firefox muestran las animaciones **CSS** en el panel Animation, pero no las SMIL. Trucos para depurar:

- Dale `id` a cada `<animate>` para poder inspeccionarlas en la consola con `document.getElementById()`.
- `anim.getCurrentTime()` devuelve el tiempo interno de la animación en segundos.
- `anim.beginElement()` / `anim.endElement()` para arrancar/parar desde la consola y reproducir a mano.
- Si no ves nada moverse: revisa `begin` (¿es `indefinite`?), `attributeName` (¿está bien escrito?), `fill` (¿está en `remove` cuando querías `freeze`?).

---

## Performance

- **SMIL y CSS transform/opacity** se pueden acelerar por GPU en la mayoría de navegadores. `transform: translate3d(0,0,0)` es un truco clásico para forzar capa.
- **Animar `width`/`height`/`cx`/`cy` fuerza layout** — más caro que animar `transform`. Para movimiento prefiere `transform: translate()` antes que mover `cx`.
- **Animaciones fuera de pantalla**: los navegadores modernos las pausan automáticamente; el coste es cero hasta que vuelven a ser visibles.
- **JavaScript con `requestAnimationFrame`** es generalmente más barato que `setInterval` y sincroniza con el repintado.

---

## Bucles infinitos y accesibilidad

Todo `repeatCount="indefinite"` o `animation: ... infinite` debe tener un escape:
- **CSS**: envolver en `@media (prefers-reduced-motion: reduce) { animation: none; }`.
- **SMIL**: no tiene equivalente automático. Soluciones:
  1. Comprobar la media query con JS y cambiar `repeatCount` a `1` o parar con `endElement()`.
  2. Usar un `<style>` dentro del SVG con `@media (prefers-reduced-motion: reduce)` que oculte el elemento animado o reemplace sus atributos.

Esta es otra razón por la que, si el SVG es inline y el efecto es simple, CSS suele ser mejor: la media query es nativa.

---

## SMIL dentro de un SVG en `<img>`

Es el único contexto donde SMIL es insustituible. Ejemplo: un logo animado en un correo HTML. JavaScript está bloqueado, CSS externo no llega, pero las animaciones SMIL definidas dentro del SVG *se ejecutan*. Ideal para:
- Íconos animados en RSS/feeds.
- Logotipos en firmas de email.
- SVG embebido en Markdown/MDX sin acceso a CSS del sitio.

---

## Interpolación de colores

SMIL y CSS interpolan colores en el espacio RGB por defecto. Esto a veces produce grises "sucios" al pasar entre colores complementarios (ej. azul → amarillo pasa por un gris). Workarounds:
- **SMIL**: usa un `values="azul;verde;amarillo"` con un punto intermedio intencionado.
- **CSS**: considera `color-interpolation: linearRGB` en el elemento SVG (afecta a gradientes también).
- **JS avanzado**: usa d3-interpolate o color.js para interpolar en HSL, OKLCH o LCH.

---

## Orden de sincronización entre CSS y SMIL

Cuando coexisten en el mismo SVG: **SMIL gana**. La animación SMIL está definida encima del árbol de cascada CSS. Si tienes una regla `circle { fill: red; }` y un `<animate attributeName="fill" .../>`, el animate pisa al CSS mientras esté activo.

Excepción: `!important` + la propiedad *no* siendo una presentation attribute puede cambiar el orden. En la práctica: evita mezclar ambos sistemas sobre la misma propiedad en el mismo elemento.

---

## Consejo general: elige por contexto, no por nostalgia

- Si tu proyecto ya tiene un pipeline CSS/Tailwind y usas SVG inline → **CSS primero**.
- Si tu SVG es un archivo que alguien va a descargar o incrustar como `<img>` → **SMIL**.
- Si hay datos en tiempo real (dashboards, charts) → **JavaScript** con D3, GSAP o Motion One.
- Si necesitas morphear paths → **SMIL o una librería JS** (Flubber + GSAP).

Mezclar los tres en un mismo proyecto no es un olor; es lo normal en aplicaciones reales.
