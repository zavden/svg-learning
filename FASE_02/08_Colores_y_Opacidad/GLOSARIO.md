# Glosario — Colores y Opacidad

---

**color CSS**
Todos los formatos de color de CSS son válidos en SVG: nombres (`red`, `steelblue`), hexadecimal (`#ff0000`, `#f00`), `rgb()`, `rgba()`, `hsl()`, `hsla()`, y los formatos modernos de CSS Color Level 4 (`oklch()`, hex con alfa `#rrggbbaa`).

---

**notación hexadecimal**
Formato `#RRGGBB` donde cada par de dígitos hexadecimales (00–FF) representa un canal de color (Rojo, Verde, Azul). Formato corto `#RGB`: cada dígito se duplica (`#f06` → `#ff0066`). Solo funciona cuando ambos dígitos de cada canal son iguales.

---

**rgb() / rgba()**
Función que define color por sus componentes Rojo, Verde, Azul (0–255 o 0%–100%). `rgba()` añade canal alfa (0–1 o 0%–100%). CSS moderno permite `rgb(R G B / alfa)` sin comas.

---

**hsl() / hsla()**
Función que define color por Hue (tono, 0–360°), Saturation (saturación, 0%–100%), Lightness (luminosidad, 0%–100%). Más intuitivo que RGB para crear variantes: modificar solo L da oscuro/claro del mismo color; H+180° da el complementario.

---

**Hue**
El "tono" en el modelo HSL: posición en la rueda de colores medida en grados. 0°/360° = rojo, 120° = verde, 240° = azul. Cambiar H cambia el color base; S y L controlan su viveza y brillo.

---

**canal alfa**
Componente de transparencia en un color, de 0 (completamente transparente) a 1 (completamente opaco). Disponible en `rgba()`, `hsla()`, y hex con 8 dígitos (`#RRGGBBAA`). A diferencia de `opacity`, el canal alfa solo afecta al color en sí, no a los hijos.

---

**opacity**
Propiedad que controla la opacidad de todo el elemento (fill + stroke + hijos). Se calcula después de renderizar el elemento completo. En jerarquías, los valores se multiplican: padre 0.5 × hijo 0.5 = 0.25 de opacidad total.

---

**fill-opacity**
Controla la opacidad únicamente del relleno, independientemente del stroke. Permite fill semitransparente con stroke opaco. Al contrario que `opacity`, no afecta a los hijos.

---

**stroke-opacity**
Controla la opacidad únicamente del trazo, independientemente del fill. Permite stroke semitransparente con fill opaco.

---

**currentColor**
Palabra clave que toma el valor de la propiedad CSS `color` del elemento o sus ancestros HTML. Permite que iconos SVG hereden el color del texto del contexto donde están embebidos. Solo representa un único color; para múltiples colores usar variables CSS.

---

**none** (fill/stroke)
Ausencia completa de pintura. `fill="none"` hace el interior de la forma completamente invisible y, con `pointer-events` por defecto, no interactivo. Diferente de `transparent`.

---

**transparent**
Color especial con alfa = 0. Visualmente idéntico a `none`, pero conceptualmente "hay pintura" (invisible). Con `pointer-events` por defecto, el área sí responde a hover y click. Útil para áreas de interacción invisibles.

---

**pointer-events**
Propiedad SVG/CSS que controla si un elemento responde a eventos del mouse. `pointer-events="all"` activa la respuesta incluso con `fill="none"`. `pointer-events="none"` desactiva incluso con fill visible. Modifica el comportamiento por defecto de `none` vs `transparent`.

---

**CSS Color Level 4**
Especificación que añade: sintaxis sin comas en `rgb()`/`hsl()`, canal alfa directo en `rgb(R G B / a)`, hex con alfa (`#RRGGBBAA`), y nuevos espacios de color como `oklch()`. Soporte amplio en navegadores modernos.

---

**oklch() / oklab()**
Espacios de color perceptualmente uniformes de CSS Color Level 4. A diferencia de HSL, la percepción del brillo es consistente entre tonos. Ventaja en gradientes: los colores intermedios se perciben como "naturales" sin pasar por gris.
