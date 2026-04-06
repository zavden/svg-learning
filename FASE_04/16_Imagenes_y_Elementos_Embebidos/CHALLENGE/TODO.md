# Ideas de desafíos — Imágenes y elementos embebidos

---

## 1. Tarjeta de perfil con avatar circular

Diseñar una tarjeta de usuario estilo red social: avatar circular (recortado con `clipPath`), nombre, bio multilínea, y dos botones "Seguir" / "Mensaje". El avatar debe ser una `<image>` con `preserveAspectRatio="xMidYMid slice"` para que se centre correctamente. Bonus: convertir la foto a data URI base64 para que el SVG sea totalmente autosuficiente. Practicar recorte, posicionamiento y composición de imagen dentro de SVG.

---

## 2. Comparador antes/después con `<image>`

Crear dos imágenes superpuestas (la "antes" y la "después") donde una está recortada a la mitad con `clipPath` rectangular. Añadir una línea vertical divisoria. Opcional: el filtro `feColorMatrix` aplicado a una de las imágenes para simular el "después" sin necesitar dos archivos distintos. Practicar combinación de imágenes, clipping y filtros.

---

## 3. Diagrama con tabla HTML embebida

Construir un diagrama de arquitectura con 3 cajas SVG conectadas por flechas (markers de la sección anterior), donde una de las cajas contiene una `<foreignObject>` con una pequeña tabla HTML de "estadísticas" (3 filas, 2 columnas). Practicar la integración de HTML dentro del flujo SVG y decidir cuándo la tabla vale la pena frente a `<text>`.

---

## 4. Mapa con detalles ampliados

Dibujar un "mapa" simple (rectángulo con 3-4 puntos de interés marcados) como un `<svg>` anidado con su propio `viewBox`. Junto al mapa, colocar 2 recuadros que son también `<svg>` anidados mostrando versiones ampliadas (viewBox más pequeño) de zonas específicas. Demostrar que el contenido es el mismo pero las "cámaras" distintas. Practicar composición con viewBox múltiples.

---

## 5. Ícono generado como data URI dinámicamente

Crear un SVG que contenga 4-5 instancias de la misma ícono pequeña (por ejemplo una estrella) pero cada una embebida como data URI `image/svg+xml;utf8` con un color distinto. Mostrar que el mismo SVG se puede "parametrizar" cambiando solo el `fill` en la cadena del data URI. Practicar construcción manual de data URIs y URL-encoding.

---

## 6. Formulario de contacto dentro de una composición SVG

Diseñar una "tarjeta de contacto" que sea 50% decoración SVG (fondo con gradiente, ornamentos, icono) y 50% formulario HTML real dentro de un `<foreignObject>`: campos email, mensaje y botón. El formulario debe ser estilizado con CSS inline para que luzca como parte natural del diseño. Practicar la integración real SVG + HTML y reflexionar sobre cuándo vale la pena `<foreignObject>` frente a posicionar HTML externo con `position: absolute`.
