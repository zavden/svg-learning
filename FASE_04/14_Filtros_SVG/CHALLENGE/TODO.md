# Ideas de desafíos — Filtros SVG

---

## 1. Botón "elevado" con sombras Material Design

Diseñar tres botones (plano, elevado, flotante) usando el mismo filtro base pero con distintos niveles de profundidad. Cada nivel debe apilar varios `<feDropShadow>`: una sombra cercana dura y una o dos lejanas suaves. Al pasar el cursor (estado `:hover` con CSS), la elevación aumenta. Practicar el apilado de `feDropShadow` y la estética Material.

---

## 2. Texto con efecto neón parpadeante

Crear un cartel de bar con la palabra "OPEN" en estilo neón: color saturado, halo con `feGaussianBlur` repetido varias veces en un `<feMerge>`, y una animación sutil de `stdDeviation` que simule el parpadeo. Bonus: añadir un segundo estado con la palabra "CLOSED" en rojo que alterna. Practicar el efecto glow y la animación de primitivas.

---

## 3. Tarjeta "de papel" con textura y sombra

Construir una tarjeta con fondo beige sobre un escritorio simulado (gradiente). Aplicar un filtro combinado: `feTurbulence` para generar textura de papel sutil, `feColorMatrix` para teñirla en tonos cálidos, y `feDropShadow` para darle elevación. El resultado debe parecer una hoja real, no un rectángulo plano. Practicar `feTurbulence` y la combinación de primitivas.

---

## 4. Logo "líquido" con `feDisplacementMap`

Partiendo de un logo simple (un texto o una forma geométrica), aplicar `feTurbulence` + `feDisplacementMap` para distorsionarlo como si estuviera bajo agua o derritiéndose. Probar diferentes valores de `scale` para ver el rango de efectos. Extra: animar `baseFrequency` o `seed` para que la distorsión cambie con el tiempo. Practicar la pareja turbulence/displacement.

---

## 5. Botón con borde "chromatic aberration"

Crear un botón con el mismo texto desplazado tres veces en rojo, verde y azul con `feOffset` + `feColorMatrix`, para imitar la aberración cromática típica del glitch art. Cada canal se desplaza una cantidad distinta, y el resultado final se combina con `feBlend` o `feMerge`. Practicar manipulación por canales y composición de capas.

---

## 6. Escena 3D falsa con `feDiffuseLighting`

Construir una "escena" plana (una pelota, un cilindro, un texto) y aplicarle `feDiffuseLighting` + `feSpecularLighting` con una `fePointLight` para que parezca un objeto tridimensional iluminado. Probar a mover la posición de la luz para ver cómo cambia el relieve. Bonus: usar `feGaussianBlur` sobre `SourceAlpha` como bump map para suavizar los bordes. Practicar iluminación y composición avanzada.
