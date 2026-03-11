# Estado del proyecto y guía para continuar

## Resumen del currículo

Currículo SVG de 25 secciones dividido en 7 fases. Cada sección tiene su propio directorio con estos archivos:

```
SECCION/
├── ROADMAP.md            ← ya existe, describe los subseciones
├── THEORY_X.1.md         ← una por cada subsección del ROADMAP
├── THEORY_X.2.md
├── ...
├── PITFALLS.md
├── REFERENCE_SHAPES.md
├── GLOSARIO.md
├── NOTES.md
└── CHALLENGE/
    └── TODO.md
```

---

## Estado actual

### Completadas (todos los archivos creados)

| Sección | Directorio |
|---------|------------|
| 01 | `FASE_01/01_Fundamentos_y_Conceptos_Previos/` |
| 02 | `FASE_01/02_Estructura_del_Documento_SVG/` |
| 03 | `FASE_01/03_Sistema_de_Coordenadas_y_Viewport/` |
| 04 | `FASE_01/04_Formas_Basicas/` |
| 05 | `FASE_02/05_Trazados_Paths/` |
| 06 | `FASE_02/06_Texto_en_SVG/` |
| 07 | `FASE_02/07_Relleno_y_Trazo/` |
| 08 | `FASE_02/08_Colores_y_Opacidad/` |
| 09 | `FASE_03/09_Gradientes/` |
| 10 | `FASE_03/10_Patrones/` |
| 11 | `FASE_03/11_Transformaciones/` |

### Pendientes — solo tienen ROADMAP.md

| Sección | Directorio | THEORY files a crear |
|---------|------------|----------------------|
| 12 | `FASE_04/12_Agrupacion_y_Reutilizacion/` | 12.1 – 12.5 (5 archivos) |
| 13 | `FASE_04/13_Recorte_y_Mascaras/` | 13.1 – 13.6 (6 archivos) |
| 14 | `FASE_04/14_Filtros_SVG/` | 14.1 – 14.6 (6 archivos) |
| 15 | `FASE_04/15_Marcadores/` | 15.1 – 15.3 (3 archivos) |
| 16 | `FASE_04/16_Imagenes_y_Elementos_Embebidos/` | 16.1 – 16.4 (4 archivos) |
| 17 | `FASE_05/17_SVG_Responsivo_y_Accesibilidad/` | 17.1 – 17.5 (5 archivos) |
| 18 | `FASE_05/18_Integracion_de_SVG_en_la_Web/` | 18.1 – 18.8 (8 archivos) |
| 19 | `FASE_05/19_Estilado_con_CSS/` | 19.1 – 19.7 (7 archivos) |
| 20 | `FASE_06/20_Animaciones_en_SVG/` | 20.1 – 20.9 (9 archivos) |
| 21 | `FASE_06/21_Interactividad_con_JavaScript/` | 21.1 – 21.9 (9 archivos) |
| 22 | `FASE_07/22_Sprites_SVG_e_Iconos/` | 22.1 – 22.5 (5 archivos) |
| 23 | `FASE_07/23_Optimizacion_de_SVG/` | 23.1 – 23.5 (5 archivos) |
| 24 | `FASE_07/24_Herramientas_y_Flujo_de_Trabajo/` | 24.1 – 24.6 (6 archivos) |
| 25 | `FASE_07/25_Buenas_Practicas_y_Patrones_Comunes/` | 25.1 – 25.8 (8 archivos) |

Para cada sección pendiente, además de los THEORY files, hay que crear:
`PITFALLS.md`, `REFERENCE_SHAPES.md`, `GLOSARIO.md`, `NOTES.md`, `CHALLENGE/TODO.md`

---

## Instrucciones para la IA que continúe este trabajo

### Regla más importante: UNA SECCIÓN POR RESPUESTA

Procesar **una sola sección** por turno de conversación. Terminar completamente los archivos de la sección 12 antes de mencionar o crear nada de la sección 13. El usuario dirá explícitamente cuándo continuar con la siguiente.

### Paso 0 — Leer el ROADMAP antes de escribir cualquier archivo

Antes de crear cualquier archivo de la sección X, leer `FASE_XX/XX_.../ROADMAP.md`. El ROADMAP tiene los subsecciones exactas y su contenido. Los archivos THEORY deben seguir fielmente lo que describe el ROADMAP para esa sección.

Para orientarse también en el estilo visual, leer uno o dos THEORY de una sección ya completada, por ejemplo:
- `FASE_03/11_Transformaciones/THEORY_11.1.md`
- `FASE_03/09_Gradientes/THEORY_9.3.md`

### Orden de creación dentro de una sección

1. Todos los `THEORY_X.Y.md` (en orden: 12.1, 12.2, … 12.5)
2. `PITFALLS.md`
3. `REFERENCE_SHAPES.md`
4. `GLOSARIO.md`
5. `NOTES.md`
6. `CHALLENGE/TODO.md` (crear el directorio si no existe)

---

## Especificaciones por tipo de archivo

### THEORY_X.Y.md

**Propósito:** explicar un concepto concreto del ROADMAP con ejemplos SVG visuales.

**Estructura tipo:**
```
# X.Y Título del subtema

Frase introductoria corta (1-2 líneas).

---

## Subtítulo 1

Explicación en prosa. Puntos clave si corresponde.

\`\`\`svg
<svg width="300" height="NNN"
     viewBox="0 0 300 NNN"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">
  ...
</svg>
\`\`\`

---

## Subtítulo 2
...
```

**Convenciones de los bloques SVG:**
- Ancho siempre `width="300"`. El alto varía según el contenido (90, 110, 130, 150, 170, 190…).
- `viewBox="0 0 300 H"` coincide con `width` y `height`.
- Estilo claro (fondo claro): `style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;"`
- Estilo oscuro (cuando el ejemplo lo requiere): `style="border:2px solid #334155;background:#0f172a;display:block; font-family:sans-serif;"`
- Siempre `display:block` para evitar espacio extra inline.
- El bloque de código usa ` ```svg ` (no ` ```xml `).
- Cada ejemplo debe ser autónomo y renderizable tal cual.
- Usar headers oscuros (`fill="#1e293b"` con texto blanco) para separar secciones dentro del SVG cuando hay varias demos en un mismo ejemplo.
- El código SVG debe ser correcto: atributos bien cerrados, elementos bien anidados.
- No usar JavaScript dentro de los bloques SVG de THEORY (los ejemplos son estáticos).

**Paleta de colores habitual en los ejemplos:**
- Azul: `#3b82f6`, `#1e40af`, `#dbeafe`
- Verde: `#10b981`, `#166534`, `#dcfce7`
- Amarillo: `#f59e0b`, `#92400e`, `#fef9c3`
- Rosa: `#ec4899`, `#9d174d`, `#fce7f3`
- Violeta: `#8b5cf6`, `#4c1d95`, `#ede9fe`
- Gris claro: `#f8fafc`, `#e2e8f0`, `#94a3b8`, `#64748b`
- Negro encabezado: `#1e293b`
- Rojo error: `#ef4444`, `#fef2f2`

**Extensión:** entre 3 y 7 ejemplos SVG por archivo. Un archivo con un solo ejemplo corto es insuficiente. Cubrir los casos más importantes del concepto.

---

### PITFALLS.md

**Propósito:** documentar 5 errores comunes de la sección, cada uno con:
1. Título descriptivo del error
2. Explicación concisa de por qué ocurre y qué sucede
3. Bloque SVG que muestra el problema (etiquetado como "MAL" o con color rojo) y/o la solución (etiquetado "BIEN" o con color verde)

**Formato:**
```
# Errores comunes — Título sección

---

## 1. Título del error

Descripción del error y sus consecuencias.

\`\`\`svg
...
\`\`\`

---

## 2. Título del error
...
```

Los 5 errores deben ser reales y frecuentes para la sección, no inventados. Basarse en el contenido del ROADMAP para identificarlos. Mostrar en el SVG tanto el caso incorrecto (en rojo, `#fef2f2` de fondo, `#ef4444` de texto/borde) como el correcto (en verde, `#dcfce7`, `#10b981`) cuando sea posible en un mismo SVG.

---

### REFERENCE_SHAPES.md

**Propósito:** cheat sheet visual de los conceptos clave de la sección. 2-4 SVGs grandes (height entre 110 y 220) que funcionen como referencia rápida.

**Tipos de contenido habitual:**
- Tabla comparativa (como lista de filas coloreadas)
- Galería de variantes del concepto principal
- Reglas o patrones resumidos visualmente

**Formato:** sección de cabecera en negro (`#1e293b`) con el título del cheat sheet, luego filas o columnas con los elementos. Mismo estilo visual que los THEORY.

---

### GLOSARIO.md

**Propósito:** definir los términos técnicos clave de la sección.

**Formato:**
```
# Glosario — Título sección

---

**`nombre-del-término`**
Definición en 1-4 líneas. Precisa y práctica, no solo la definición estándar sino su implicación real en SVG.

---

**`otro-término`**
...
```

Incluir entre 8 y 16 términos. Cubrir los atributos principales, conceptos y patrones de la sección. Los términos más importantes primero.

---

### NOTES.md

**Propósito:** notas de referencia rápida. No repite lo de THEORY sino que complementa con:
- Tabla de compatibilidad de navegadores (siempre primera sección)
- Diferencias entre enfoques alternativos
- Trucos y patrones no obvios
- Advertencias sobre comportamientos sorpresivos
- Snippets de JavaScript si la sección lo requiere (secciones 20-25)

**Formato:**
```
# Notas — Título sección

---

## Compatibilidad

| Característica | Chrome | Firefox | Safari | Edge |
|---|---|---|---|---|
| ... | ✓ | ✓ | ✓ | ✓ |

---

## Tema 2

Nota libre con texto y opcionalmente código en bloques \`\`\`svg o \`\`\`js o \`\`\`css.

---
```

Los bloques de código en NOTES no tienen que ser SVGs renderizables completos — pueden ser fragmentos, snippets de JS, CSS, etc.

---

### CHALLENGE/TODO.md

**Propósito:** lista de 6 ideas de ejercicios para practicar la sección. Son solo ideas, no implementaciones.

**Formato:**
```
# Ideas de desafíos — Título sección

---

## 1. Nombre del desafío

Descripción del ejercicio: qué construir, qué conceptos practica, variaciones opcionales.

---

## 2. Nombre del desafío
...
```

Las ideas deben ser concretas pero no incluir código. Cubrir los conceptos más importantes de la sección, distribuidos entre los 6 ejercicios.

---

## Verificación: cómo saber si una sección está completa

Una sección está **completa** cuando su directorio contiene exactamente:
```
ROADMAP.md                     ← preexistente, no modificar
THEORY_X.1.md
THEORY_X.2.md
...
THEORY_X.N.md                  ← N = número de subsecciones en el ROADMAP
PITFALLS.md
REFERENCE_SHAPES.md
GLOSARIO.md
NOTES.md
CHALLENGE/
    TODO.md
```

No crear ningún otro archivo. No modificar el ROADMAP.md existente.

---

## Ejemplo de verificación antes de empezar

Antes de crear los archivos de la sección 12, ejecutar mentalmente:
1. ¿Leí `FASE_04/12_Agrupacion_y_Reutilizacion/ROADMAP.md`? → Sí
2. ¿Cuántas subsecciones tiene? → 5 (12.1 a 12.5)
3. ¿Ya existe algún THEORY en esa carpeta? → No
4. Crear en orden: THEORY_12.1.md, THEORY_12.2.md, THEORY_12.3.md, THEORY_12.4.md, THEORY_12.5.md, PITFALLS.md, REFERENCE_SHAPES.md, GLOSARIO.md, NOTES.md, CHALLENGE/TODO.md

Si algún archivo ya existe, leerlo antes de crearlo o modificarlo.
