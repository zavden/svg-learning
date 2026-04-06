# Ideas de desafíos — Recorte y Máscaras

---

## 1. Avatar circular con borde decorativo

Crear un avatar partiendo de una imagen rectangular (o un patrón gradiente que la simule). Recortar con `<clipPath>` circular para conseguir el avatar. Añadir un anillo decorativo alrededor (segundo círculo con stroke) y un pequeño badge de "online" en la esquina inferior derecha. Practicar `clipPath`, posicionamiento y superposición.

---

## 2. Título "cinemático" con texto como recorte

Usar un texto muy grande (~100px, font-weight 900) dentro de un `<clipPath>` para recortar un fondo colorido (gradiente, imagen o patrón) a la forma de las letras. Añadir un sutil sombreado detrás del texto para dar profundidad. Probar con diferentes fuentes para ver cómo cambia el resultado.

---

## 3. Foto con viñeta y fade horizontal

Construir una "foto" (puede ser un paisaje simple hecho con formas SVG o un patrón de colores). Aplicarle una máscara radial para conseguir una viñeta clásica de fotografía. En una segunda versión, añadir también un fade horizontal en el borde derecho para simular un efecto de "overflow" de menú lateral.

---

## 4. Menú horizontal con fade en los bordes

Diseñar una barra de navegación horizontal con varios items de texto que se extienden más allá del contenedor visible. Usar una máscara con gradientes blanco → negro en ambos extremos para que los items entren y salgan gradualmente. Practicar combinar varios gradientes en una misma máscara.

---

## 5. Revelado animado con `clip-path` CSS

Crear una tarjeta o panel que aparece con una animación de "revelado": empieza con `clip-path: inset(0 100% 0 0)` (invisible) y termina con `clip-path: inset(0 0 0 0)` (visible completo). Usar `@keyframes` para animar. Extra: hacer variantes con revelado circular (`circle(0%)` → `circle(70%)`) y con un polígono que morfea.

---

## 6. Efecto "ojo de cerradura" combinando clipPath anidado

Crear una escena (paisaje, ilustración, foto) vista a través de la forma de un ojo de cerradura. La silueta del ojo de cerradura se consigue con un `<clipPath>` que contiene un círculo arriba y un trapecio abajo (unión automática). Extra: animar el tamaño del clip para que el "ojo" se abra y cierre.
