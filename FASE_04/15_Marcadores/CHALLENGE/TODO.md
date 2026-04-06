# Ideas de desafíos — Marcadores

---

## 1. Diagrama de clases UML mínimo

Construir un pequeño diagrama UML con 3-4 clases (rectángulos con nombre y lista de métodos) conectadas con distintos tipos de flechas: **herencia** (triángulo hueco), **composición** (diamante lleno), **agregación** (diamante hueco) y **asociación** (flecha abierta). Cada tipo de relación debe ser un marker reutilizable. Practicar el diseño de markers complejos y la selección entre distintos estilos según la semántica.

---

## 2. Gráfico de líneas con series diferenciadas

Dibujar un gráfico de líneas con 2 o 3 series de datos. Cada serie debe tener un marker distinto en sus vértices: círculo, cuadrado, diamante. Usar `markerUnits="userSpaceOnUse"` para que los puntos mantengan tamaño constante aunque cambies el stroke. Extra: añadir una leyenda usando los mismos markers como muestra visual.

---

## 3. Mapa de red (grafo dirigido)

Dibujar un pequeño grafo con 5-6 nodos (círculos) conectados por aristas dirigidas. Las aristas deben terminar con flecha, y algunas pueden ser bidireccionales (usando `orient="auto-start-reverse"`). Jugar con el `refX` para que la flecha no se meta dentro del círculo del nodo destino. Extra: colorear cada arista distinto y hacer que la flecha herede el color con `context-stroke`.

---

## 4. Regla graduada con markers en los tick marks

Crear una regla horizontal usando una línea larga con markers como marcas de graduación: markers grandes cada 10 unidades, medianos cada 5, pequeños cada 1. La línea base se dibuja con `stroke-dasharray` para dar la ilusión de subdivisiones. Practicar la combinación de varios markers en distintos `marker-mid` sobre la misma línea.

---

## 5. Flowchart con flechas etiquetadas

Diseñar un flowchart (proceso con inicio, decisión, dos ramas y fin) donde todas las flechas usen el mismo marker. Añadir etiquetas de texto junto a las flechas ("sí", "no", "continuar"). Practicar paths con codos (combinación de `L` horizontales y verticales) y reutilización de un único marker en múltiples conexiones. Extra: aplicar `context-stroke` para que las flechas tomen colores distintos según el tipo de rama.

---

## 6. Firma animada "escribiendo"

Crear un path que simule una firma a mano. En lugar de animar el path con `stroke-dasharray`, colocar un marker especial en el `marker-end`: un círculo pequeño que represente la "punta del bolígrafo". Bonus: hacer que el marker tenga un efecto de animación interna (un pulso de color o un anillo expandiéndose con SMIL). Practicar markers animados e integración con paths complejos.
