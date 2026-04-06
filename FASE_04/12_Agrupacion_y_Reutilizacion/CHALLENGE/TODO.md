# Ideas de desafíos — Agrupación y Reutilización

---

## 1. Sistema de iconos con `<symbol>`

Crear un sprite con al menos 6 iconos distintos (home, user, mail, settings, heart, star) como `<symbol>` en un único `<defs>`, todos con `viewBox="0 0 24 24"`. Los paths deben usar `fill="currentColor"` para ser tematizables. Luego renderizar los iconos en tres tamaños distintos (16, 24, 48) y tres colores distintos, demostrando que la misma definición produce todas las variantes.

---

## 2. Bosque con `<use>`

Definir un único `<symbol>` de árbol (tronco + copa frondosa) y renderizar un bosque completo instanciándolo 15-20 veces con `<use>`. Variar la posición con `x`/`y`, el tamaño con `width`/`height` y la rotación con `transform`, para que no se vea repetitivo. Añadir suelo y algún detalle de fondo.

---

## 3. Cartel con paleta heredada

Construir un cartel decorativo (tipo anuncio retro) donde todos los textos, bordes y formas hereden `fill`, `stroke`, `font-family` y `font-size` del `<svg>` raíz o de un `<g>` padre. Demostrar que cambiar los valores en el padre cambia toda la apariencia sin tocar los elementos individuales.

---

## 4. Personaje articulado con grupos anidados

Diseñar un personaje humano (cabeza, torso, brazos, piernas) usando grupos anidados con IDs semánticos. Cada extremidad debe ser un `<g>` hija del grupo del cuerpo. Aplicar un `transform` al grupo raíz para mover al personaje completo, y probar a aplicar `rotate` a los grupos de los brazos desde sus puntos de unión para ver articulación.

---

## 5. Tarjetas de juego compartiendo diseño

Crear 4 "cartas" (tipo naipes) que compartan un mismo `<symbol>` como plantilla del marco y fondo, y un `<symbol>` por cada palo (corazón, trébol, pica, diamante). Cada carta se compone con `<use>` al marco y al palo correspondiente. Practicar cómo un pequeño conjunto de símbolos produce muchas combinaciones.

---

## 6. Galería tematizable con variables CSS

Crear 3 iconos compuestos (botones de media: play, pause, stop) como `<symbol>` con dos partes: fondo y contorno. Usar `fill="var(--btn-bg)"` y `stroke="var(--btn-border)"` en las partes. Renderizar cada icono cuatro veces con diferentes `style="--btn-bg:…; --btn-border:…"` para demostrar que un mismo símbolo puede verse distinto en cada instancia sin necesidad de duplicar código.
