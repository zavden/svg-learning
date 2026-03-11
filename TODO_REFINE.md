# TODO REFINE: Directrices para Mejorar los Archivos THEORY de SVG

Este documento establece las reglas y el estilo acordados para la refactorización profunda de los archivos teóricos de SVG (ej. `THEORY_1.N.md`). Cualquier IA o colaborador que asista en este proyecto deberá adherirse estrictamente a los siguientes puntos:

## 1. Estrategia de Archivos ("Non-Destructive")
- **No sobreescribir el original**: A partir de `THEORY_1.3.md`, los nuevos archivos mejorados deben guardarse con el sufijo `.gemini.md` en la misma ruta exacta que el original (ejemplo: `THEORY_1.3.gemini.md`).
- El objetivo es comparar versiones y no perder el trabajo previo de manera abrupta.

## 2. Profundidad Técnica ("Nivel 1000%")
El texto explicativo en Markdown debe ser **técnicamente denso y avanzado**.
Debe profundizar en los puntos cruciales, puntos críticos, buenas prácticas y técnicas de código limpio de ser necesario.

## 3. Formato de los Ejemplos SVG ("Bloques Atómicos")
- **Extensión visual (80 columnas):** El código fuente dentro de los bloques `<svg>` no debe exceder las ~80 columnas por línea. Esto asegura que se vean perfectos al ser inyectados en el `previewer.html`.
- **Atomicidad:** En lugar de tener un solo bloque `<svg>` gigante que explique 5 cosas, deben dividirse en **múltiples ejemplos atómicos** (Ejemplo 1, Ejemplo 2, Ejemplo 3) por cada tema/sección `##` del documento. 
- **Volumen:** Tratar de incluir al menos 2 a 3 ejemplos SVG atómicos por cada subsección de H2 (`##`).
- **Atributos de Previsualización:** Los ejemplos deben mantener el frame del previewer (borde y fondo para contraste). Ejemplo estándar:
  ```xml
  <svg width="300" height="100" xmlns="http://www.w3.org/2000/svg"
       style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  ```

## 4. Instrucción de Continuidad
Para continuar el trabajo:
1. Lee `FASE_01.md` y `ROADMAP.md` para ganar contexto general de métricas del curso.
2. Lee este `TODO_REFINE.md` para entender el estilo de la refactorización.
3. Lee el archivo `THEORY_X.Y.md` original designado.
4. Genera el texto técnico profundo.
5. Crea los ejemplos SVG atómicos (< 80 columnas).
6. Escribe el resultado en `THEORY_X.Y.gemini.md`.
