# Ideas de desafíos — SVG responsivo y accesibilidad

---

## 1. Card responsive con SVG que se adapta

Construir una tarjeta de producto HTML con una imagen SVG como encabezado. La imagen debe: (a) llenar todo el ancho del contenedor sin importar el tamaño, (b) mantener una proporción 16:9 en pantallas anchas y 1:1 en móviles (usando `aspect-ratio` con una media query HTML), (c) conservar su contenido visible al redimensionar. Practicar la integración de SVG + CSS moderno + `aspect-ratio`.

---

## 2. Logo adaptativo de tres niveles

Diseñar un logo SVG que tenga **tres variantes en un solo archivo** controladas por media queries internas: versión completa (símbolo + nombre + tagline), versión intermedia (símbolo + nombre) y versión mínima (solo símbolo). Los elementos se ocultan con `display: none` en los breakpoints correspondientes. Probar el archivo como `<img src>` a distintos tamaños y verificar que cada variante aparece cuando corresponde.

---

## 3. Gráfico de barras accesible

Implementar un gráfico de barras SVG con 5-6 valores, incluyendo: `role="img"`, `<title>` descriptivo, `<desc>` con los datos numéricos exactos, y una `<table>` HTML oculta visualmente (`.sr-only`) con los mismos datos en formato accesible. Probar con un lector de pantalla (NVDA, VoiceOver o DevTools accessibility tree) para verificar que la información se anuncia correctamente.

---

## 4. Set de íconos monocromáticos con `currentColor`

Crear una colección de 6-8 íconos SVG (guardar, eliminar, editar, compartir, descargar, etc.) que usen `fill="currentColor"`. Integrarlos en una página HTML con botones de distintos colores (azul para primario, verde para éxito, rojo para peligro) y verificar que cada ícono hereda automáticamente el color del botón contenedor, sin tener que editar el SVG.

---

## 5. Ilustración dual claro/oscuro con `prefers-color-scheme`

Diseñar una ilustración SVG relativamente compleja (paisaje simple, escena con varios elementos, diagrama) que tenga **dos paletas completas**: modo claro y modo oscuro. Implementar usando variables CSS definidas en `:root` dentro del SVG y un bloque `@media (prefers-color-scheme: dark)` que redefina esas variables. Verificar el cambio automático al alternar el tema del sistema operativo.

---

## 6. Dashboard con gráficos accesibles y `prefers-reduced-motion`

Construir un mini-dashboard con 2-3 gráficos SVG animados (barras que crecen al cargar, líneas que se dibujan progresivamente, círculos que rotan). Cada gráfico debe: (a) tener su patrón de accesibilidad completo (`role="img"`, `<title>`, `<desc>`), (b) respetar `prefers-reduced-motion` desactivando las animaciones cuando el usuario lo prefiera, (c) mantenerse completamente funcional y accesible en modo reducido. Integrar una tabla oculta con los datos reales para lectores de pantalla.
