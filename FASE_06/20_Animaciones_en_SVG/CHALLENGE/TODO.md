# CHALLENGE — Animaciones en SVG

Seis retos progresivos para poner en práctica los tres sistemas de animación. Cada uno te obliga a decidir *qué sistema* encaja mejor.

---

## Reto 1 — Spinner accesible (CSS)

Crea un spinner circular que:
- Rote a 1 giro/segundo con CSS `@keyframes`.
- Use `stroke-dasharray` para que solo la mitad del círculo esté pintada (arco giratorio).
- Tenga `transform-origin: center;` correcto.
- Se detenga completamente cuando `prefers-reduced-motion: reduce` esté activo — en su lugar muestra un mensaje estático ("Cargando…").

**Criterio de éxito**: gira suavemente, y desactivando animaciones en el SO, se queda inmóvil sin parpadear.

---

## Reto 2 — Line drawing de una firma (CSS + JS mínimo)

Dibuja tu firma (o cualquier path curvo) con el efecto de "se está escribiendo":
1. Crea el path en Figma o a mano.
2. En JS, usa `getTotalLength()` para calcular la longitud exacta.
3. Aplica `stroke-dasharray` y `stroke-dashoffset` iguales a esa longitud.
4. Anima con CSS `@keyframes` de `dashoffset` → `0` en 3 segundos.
5. Al terminar, cambia `fill` del path de `none` a un color sólido con una transición de 0.5s (efecto "se rellena después de dibujarse").

**Criterio de éxito**: el trazo se dibuja de principio a fin sin saltos y luego se "rellena".

---

## Reto 3 — Icono menú → X con SMIL

Implementa un botón de hamburguesa (tres líneas horizontales) que se transforme en una X al pulsarlo, usando SMIL:
- Las tres líneas son `<line>` o `<path>`.
- Al click, la línea de arriba rota 45° y baja al centro, la de abajo rota −45° y sube al centro, la del medio desaparece.
- Usa `<animate begin="click"/>` y `<set>` para coordinar el morphing.
- Un segundo click debe devolverlo al estado original.

**Criterio de éxito**: funciona sin JavaScript (demuéstralo abriendo el SVG directamente en el navegador, no inline en HTML).

---

## Reto 4 — Barra de progreso circular animada (CSS + var)

Haz un componente circular de progreso donde:
- El porcentaje llega como CSS custom property: `--pct: 75;`.
- El `stroke-dashoffset` se calcula con `calc()` usando `--pct`.
- Al cambiar `--pct` (desde JS), la transición CSS anima el arco.
- Incluye un `<text>` central con el número (que puedes sincronizar vía JS `element.textContent`).
- Arranca desde arriba con `transform="rotate(-90)"`.

**Criterio de éxito**: cambiando `--pct` desde la consola (`svg.style.setProperty('--pct', 50)`), el arco anima suavemente al nuevo valor.

---

## Reto 5 — Planeta orbitando con `animateMotion`

Construye una mini escena del sistema solar:
- Un sol amarillo en el centro.
- Tres planetas de tamaños diferentes, cada uno orbitando a distinta velocidad.
- Usa `<animateMotion>` con `<mpath>` que referencia círculos definidos como paths ocultos (las órbitas visibles).
- Añade `rotate="auto"` para que un planeta con forma alargada (un cometa) se oriente siguiendo la dirección del movimiento.
- Las órbitas son visibles como líneas punteadas.

**Criterio de éxito**: los planetas siguen trayectorias circulares a velocidades distintas y el cometa apunta siempre "hacia adelante".

---

## Reto 6 — Dashboard híbrido (los tres sistemas)

Combina los tres sistemas en una misma mini-UI:
- **Un chart de barras** que se actualiza con datos generados cada 2 segundos (JavaScript).
- **Un indicador de "live"** pulsando con CSS `@keyframes` y `prefers-reduced-motion` respetado.
- **Un ícono de recarga** que usa `<animateTransform>` (SMIL) para rotar 360° cada vez que los datos se refrescan — dispara la animación desde JS con `beginElement()`.
- **Hover sobre cada barra** debe cambiar el `fill` con CSS `transition`.

**Criterio de éxito**: cada sistema hace lo que mejor sabe hacer:
- CSS → hover, pulso con accesibilidad.
- SMIL → giro puntual coreografiado desde fuera.
- JS → datos cambiantes.

---

## Extras opcionales

- **Morph SVG complejo con Flubber**: instala `flubber` y anima un ícono de sol → luna (formas completamente distintas). Verás por qué SMIL solo no puede.
- **Reduce tu bundle**: comprueba cuántos KB pesa tu dashboard del Reto 6 y optimízalo (SVG inline vs sprites, minificación con SVGO).
- **Publica**: convierte uno de los retos en un Codepen o Glitch público. Útil como pieza de portfolio.
