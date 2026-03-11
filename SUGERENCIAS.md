# Sugerencias para mejorar el aprendizaje de SVG

Ideas ordenadas por impacto estimado. Las más accionables primero.

---

## 1. Ejercicios por sección (EXERCISE_X.X.md)

Cada sección de teoría debería tener un archivo `EXERCISE_X.X.md` complementario con ejercicios prácticos graduales:

- **Nivel 1 — Copia guiada**: reproducir un SVG dado a partir de instrucciones paso a paso
- **Nivel 2 — Modificación**: tomar un SVG existente y cambiar propiedades concretas
- **Nivel 3 — Desde cero**: construir un SVG describiendo solo el resultado esperado
- **Nivel 4 — Reto abierto**: un problema creativo sin solución única (ej. "dibuja un paisaje usando solo formas básicas")

El previewer ya funciona perfectamente para los niveles 1–3: escribes el código, ves el resultado en tiempo real.

---

## 2. Sección de errores comunes (PITFALLS_X.X.md)

Para cada sección, un archivo que documente los errores más frecuentes con ejemplos SVG que muestren el error y su corrección lado a lado:

```
Error:    viewBox="0 0 100" (faltan valores)
Correcto: viewBox="0 0 100 100"
```

Especialmente útil para: paths (`d` attribute), preserveAspectRatio, coordenadas anidadas, y unidades mixtas. Muchos conceptos de SVG son silenciosamente incorrectos (el navegador no lanza errores, simplemente no renderiza lo esperado).

---

## 3. Galería de referencia visual (REFERENCE_SHAPES.md / REFERENCE_PATTERNS.md)

Un archivo por tema con una galería densa de ejemplos pequeños, tipo "cheat sheet visual":

- Todos los comandos de `<path>` (M, L, H, V, C, Q, A, Z) en un solo lugar con ejemplos
- Todos los valores de `preserveAspectRatio` en una cuadrícula 3×3
- Combinaciones de `fill-rule` (nonzero vs evenodd) con figuras complejas
- Tipos de `stroke-linecap` y `stroke-linejoin` comparados

Perfecto para consultar sin tener que releer teoría completa.

---

## 4. Archivos CHALLENGE (desafíos con solución oculta)

Un archivo `CHALLENGE_X.md` por fase con SVGs "incompletos" o con bugs intencionales:

```svg
<!-- ¿Por qué este círculo no aparece centrado? -->
<svg width="200" height="200">
  <circle x="100" y="100" r="50" fill="steelblue"/>
</svg>
```

El alumno diagnostica y corrige en el previewer. Al final del archivo, la solución comentada. Refuerza el pensamiento de depuración, clave en SVG real.

---

## 5. Comparativas CSS ↔ SVG (COMPARE_CSS_SVG.md)

SVG tiene conceptos que existen también en CSS pero funcionan diferente. Un archivo de comparativas explícitas evita confusiones:

| Concepto | CSS | SVG |
|---|---|---|
| Posición | `top`, `left` (relativo al padre) | `x`, `y` (esquina sup-izq) / `cx`, `cy` (centro) |
| Transformaciones | `transform: translate(x, y)` | `transform="translate(x, y)"` (acumula distinto) |
| Colores | `color`, `background-color` | `fill`, `stroke` |
| Opacidad | `opacity`, `rgba()` | `opacity`, `fill-opacity`, `stroke-opacity` |
| Unidades | `px`, `rem`, `em`, `vw`... | user units (sin sufijo), `px`, `%` del viewport |
| Overflow | `overflow: hidden/visible` | `overflow="hidden/visible"` (atributo) |

---

## 6. Mejoras al previewer

### 6.1 Modo comparación A/B
Dividir la vista en dos previews: el código original (izquierda) y una versión modificada (derecha). Ideal para entender el impacto de cambiar un solo atributo.

### 6.2 Resaltar elemento al hacer hover en el código
Al pasar el cursor sobre un tag en el editor (ej. `<circle`), resaltar ese elemento en el preview. Requiere parsear el SVG y añadir event listeners dinámicamente.

### 6.3 Panel de propiedades en vivo
Al hacer clic sobre un elemento en el preview, mostrar un panel con sus atributos editables como sliders/inputs (ej. `r`, `cx`, `cy`, `fill`). Cambiar un valor actualiza el código y el preview.

### 6.4 Exportar SVG corregido
Botón "Descargar" en cada card que exporta el SVG actual (posiblemente modificado en el editor) como archivo `.svg`.

### 6.5 Modo presentación
Ocultar el editor y mostrar solo los previews en grande, para repasar visualmente todos los ejemplos de una sección.

---

## 7. Índice maestro (INDEX.md)

Un archivo en la raíz que liste todas las secciones con:
- Estado: ✅ con teoría / 🔲 solo roadmap
- Link directo al THEORY y al EXERCISE
- Tiempo estimado de lectura
- Conceptos clave de cada sección (2–3 palabras)

Sirve como mapa de progreso del aprendizaje.

---

## 8. Mini-proyectos por fase (PROJECT_FASE_X.md)

Al terminar cada fase, un proyecto integrador que use todos los conceptos aprendidos:

- **Fase 1** (fundamentos): dibujar un plano de habitación con formas básicas
- **Fase 2** (paths y texto): crear un logotipo tipográfico simple
- **Fase 3** (gradientes/patrones): ilustración de un paisaje con cielo degradado y suelo con patrón
- **Fase 4** (efectos avanzados): tarjeta con máscara, sombra y clip-path
- **Fase 5** (web): icon set de 8 iconos, integrado como sprite en una página HTML
- **Fase 6** (animación): loader animado + menú con transiciones SVG
- **Fase 7** (producción): tomar el proyecto de Fase 5 y optimizarlo, documentarlo y publicarlo

---

## 9. Glosario visual (GLOSARIO.md)

Cada término con una definición de una línea y un SVG mínimo que lo ilustra:

- `viewport`: la ventana física del SVG → SVG de 200×100 con borde visible
- `viewBox`: el mapa interno → mismo SVG con viewBox distinto
- `user unit`: coordenada sin unidad → ejemplo antes/después de viewBox
- `baseline`: línea de referencia del texto → texto con punto en (x,y)
- etc.

Útil para consulta rápida sin tener que releer secciones completas.

---

## 10. Notas de compatibilidad por sección

Al final de cada THEORY, una nota corta sobre comportamientos distintos entre navegadores para ese tema específico (recordatorio: ver `WARNING_FIREFOX.md` para el contexto general). Por ejemplo:

- `filter` SVG: diferente comportamiento en Safari pre-15
- `foreignObject`: soporte limitado en algunos contextos
- `text` con `textLength`: interpretación variable

Esto evita sorpresas cuando el alumno lleve el conocimiento a producción.
