# RULES_CODEX

Guia operativa para futuras refinaciones de los archivos `THEORY_X.Y.md` del
curso de SVG. Este documento consolida lo aprendido del proyecto, adapta
`TODO_REFINE.md` al flujo de Codex y fija un criterio editorial estable para no
reinventar el estilo en cada iteracion.

---

## 1. Objetivo general

La tarea no es "reescribir bonito". La tarea es **elevar el valor didactico y
tecnico** de cada THEORY:

- mas claridad conceptual,
- mas profundidad real,
- mejores matices tecnicos,
- ejemplos SVG mas pequenos, precisos y reutilizables,
- mejor separacion entre sintaxis, semantica, render y uso practico.

Cada THEORY refinado debe sentirse como una leccion mas madura que el original
y, si existe una version `.gemini.md`, debe poder **superarla en claridad,
precision y estructura**.

---

## 2. Convencion de archivos

Regla principal: **no se sobreescribe el original**.

- Archivo fuente base: `THEORY_X.Y.md`
- Salida refinada por Codex: `THEORY_X.Y.codex.md`
- Misma carpeta que el original

Ejemplo:

```text
FASE_01/01_Fundamentos_y_Conceptos_Previos/THEORY_1.5.md
FASE_01/01_Fundamentos_y_Conceptos_Previos/THEORY_1.5.codex.md
```

Objetivo:

- comparar versiones,
- no perder el texto original,
- permitir que coexistan original, Gemini y Codex.

Nunca:

- reemplazar `THEORY_X.Y.md`,
- editar o borrar una version `.gemini.md` por defecto,
- mover el archivo refinado a otra carpeta.

---

## 3. Archivos que deben leerse antes de refinar

Antes de trabajar en un THEORY nuevo, leer como minimo:

1. `TODO_REFINE.md`
2. `FASE_01.md`
3. `ROADMAP_BASIC_INTERMEDIATE.md`
4. El `ROADMAP.md` local de la fase o subseccion correspondiente
5. El `THEORY_X.Y.md` original

Si ya existen refinamientos previos de la misma fase, conviene revisar al
menos uno o dos para mantener continuidad de nivel y estilo.

Orden recomendado:

1. contexto global del curso,
2. contexto de la fase,
3. reglas de refinamiento,
4. THEORY original,
5. ejemplos refinados anteriores si hacen falta.

---

## 4. Filosofia editorial

### 4.1 Claridad antes que compacidad

La explicacion no debe quedarse en frases cortas tipo resumen escolar.
Debe desarrollar la idea con estructura, contraste y consecuencias practicas.

Malo:

- definicion corta,
- una lista de bullets obvios,
- afirmaciones sin matiz.

Bueno:

- define,
- explica por que importa,
- aclara limites,
- distingue casos parecidos,
- conecta con comportamiento real del navegador o del formato.

### 4.2 Densidad tecnica alta

El standard objetivo es el de `TODO_REFINE.md`: nivel tecnico alto.

Eso implica:

- no simplificar de forma enganosa,
- introducir vocabulario tecnico correcto cuando aporte valor,
- explicar diferencias sutiles: parser vs renderer, XML valido vs SVG valido,
  inline vs archivo externo, atributo reconocido vs atributo ignorado, etc.

No caer en:

- humo,
- grandilocuencia vacia,
- frases dramaticas sin contenido,
- analogias que deformen la realidad tecnica.

### 4.3 Explicar el "por que", no solo el "que"

Cada `##` debe responder, en lo posible, a estas preguntas:

- que es esto,
- como funciona,
- por que importa,
- que se rompe si se usa mal,
- cuando conviene,
- cuando no conviene.

### 4.4 Mejorar a Gemini cuando exista

Si existe una version `.gemini.md`, tomarla como referencia de ambicion visual,
pero no como techo.

Codex debe mejorar:

- rigor,
- exactitud de matices,
- orden pedagogico,
- separacion de conceptos,
- utilidad de los ejemplos.

---

## 5. Estilo de escritura

### 5.1 Idioma

- Espanol claro y tecnico
- Preferencia por ASCII en el archivo final
- Evitar caracteres exoticos innecesarios

### 5.2 Tono

- directo,
- sobrio,
- didactico,
- sin relleno motivacional,
- sin "marketing voice".

### 5.3 Nivel de detalle

Por defecto, **explicar mas de lo que explicaba el original**.

Si un punto necesita expansion, se expande. No recortar longitud por miedo a
"hacerlo largo". La prioridad es que el lector entienda bien.

### 5.4 Vocabulario

Usar terminologia tecnica real cuando ayude:

- parser,
- DOM,
- rasterizacion,
- remuestreo,
- semantica,
- namespace,
- well-formed,
- render pipeline,
- atributo de presentacion,
- escena,
- geometria parametrica.

Pero siempre explicando o contextualizando cuando el termino pueda ser opaco.

### 5.5 Matices obligatorios

Evitar afirmaciones demasiado absolutas cuando la realidad es mas fina.

Ejemplos:

- no decir solo "SVG nunca se pixela"; mejor explicar que el vector se
  rasteriza al final y por eso mantiene nitidez al escalar.
- no decir solo "si el XML falla no se renderiza"; aclarar el contexto de
  archivo `.svg` versus inline en HTML cuando sea relevante.
- no decir solo "un atributo mal escrito rompe todo"; distinguir entre error de
  sintaxis XML y atributo semanticamente ignorado.

---

## 6. Estructura recomendada de cada THEORY refinado

### 6.1 Encabezado

Mantener el titulo del tema:

```md
# 1.5 XML como base de SVG
```

### 6.2 Apertura

Abrir con 1 a 3 parrafos que:

- enmarquen el tema,
- expliquen por que importa,
- corrijan una simplificacion comun si aplica.

### 6.3 Cuerpo por secciones `##`

Cada bloque principal debe organizar una idea completa, no una mezcla difusa.

Patron recomendado:

1. definicion o tesis,
2. desarrollo tecnico,
3. consecuencias practicas,
4. ejemplos atomicos,
5. regla operativa o cierre local si hace falta.

### 6.4 Cierre

Es recomendable terminar con una de estas dos opciones:

- una `### Regla operativa final`
- un checklist corto

Objetivo:

- dejar criterio accionable,
- no cerrar solo con teoria abstracta.

---

## 7. Reglas para los ejemplos SVG

Esta es la parte mas importante a nivel de formato.

### 7.1 Atomicidad

No usar un unico `<svg>` gigante para explicar muchos conceptos mezclados.

Preferir:

- varios ejemplos pequenos,
- cada ejemplo con una sola idea central,
- un ejemplo por contraste si hace falta comparar correcto vs incorrecto.

Meta recomendada:

- minimo 2 a 3 ejemplos por cada `##`,
- 5 a 6 si el tema realmente lo necesita y mejora la comprension.

### 7.2 Frame visual del previewer

Usar el marco de previsualizacion consistente:

```xml
<svg width="300" height="100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
```

Se puede cambiar el fondo a oscuro si el ejemplo tipo "codigo en pantalla" lo
justifica, pero debe seguir viendose bien en el previewer.

### 7.3 Lineas cortas

Dentro de bloques `<svg>`, mantener lineas cercanas a 80 columnas.

Objetivo:

- evitar que el previewer rompa visualmente,
- hacer el codigo mas legible,
- facilitar diffs y mantenimiento.

### 7.4 Ejemplos autocontenidos

Cada ejemplo debe funcionar y entenderse por si mismo.

Evitar ejemplos que dependan de una explicacion externa para no parecer rotos.

### 7.5 Utilidad didactica

Cada ejemplo debe responder a una razon concreta:

- demostrar un concepto,
- contrastar correcto vs incorrecto,
- ilustrar un caso borde,
- mostrar una buena practica.

Si un ejemplo es decorativo pero no ensena nada nuevo, sobra.

### 7.6 Complejidad visual controlada

Aunque el documento sea tecnico, el SVG del ejemplo no debe ser innecesariamente
barroco.

Preferir:

- geometria simple,
- composiciones legibles,
- textos cortos,
- contraste claro.

### 7.7 Comentarios dentro del SVG

Permitidos solo si aclaran algo relevante.

Evitar comentar obviedades.

---

## 8. Reglas de Markdown

### 8.1 Jerarquia

Usar una jerarquia consistente:

- `#` titulo del THEORY
- `##` bloques conceptuales mayores
- `### Ejemplo N: ...` para ejemplos o mini cierres

### 8.2 Parrafos primero, bullets despues

No convertir todo en listas.

Regla general:

- usar parrafos para explicar,
- usar bullets para checklist, contrastes o reglas resumidas.

### 8.3 Tablas solo cuando simplifican

Usar tablas para comparativas reales:

- raster vs vector,
- error XML vs error semantico,
- inline vs externo.

No usar tablas por decorar.

### 8.4 Bloques de codigo

Los ejemplos SVG deben ir en fences:

```md
```svg
...
```
```

Si se muestra pseudoestructura o rutas, usar el info string adecuado:

- `svg`
- `xml`
- `md`
- `text`

### 8.5 Separadores

Usar `---` entre bloques grandes cuando ayude a respirar el documento.

---

## 9. Patrones pedagogicos que ya funcionaron

En las refinaciones hechas hasta ahora, estos patrones dieron buen resultado:

### 9.1 Contraste binario

Comparar dos cosas lado a lado:

- raster vs vector,
- correcto vs incorrecto,
- inline vs archivo,
- XML valido vs SVG semanticamente correcto.

### 9.2 Desacoplar capas del problema

Separar explicaciones por capas:

- sintaxis,
- semantica,
- render,
- tooling,
- accesibilidad,
- performance.

### 9.3 Cerrar con regla operativa

Al final de una seccion compleja, convertir la teoria en criterio accionable.

### 9.4 Corregir simplificaciones comunes

Muchos temas del curso empiezan con una version "popular" de la idea que es
parcialmente cierta. Mejorar eso aporta mucho valor.

Ejemplos:

- "SVG no son pixeles" -> matizar que el resultado final en pantalla si termina
  siendo rasterizado.
- "XML roto no renderiza" -> matizar segun contexto de parseo.

---

## 10. Validaciones obligatorias antes de cerrar

Antes de dar por terminado un THEORY refinado, comprobar:

1. el archivo nuevo termina en `.codex.md`
2. el original sigue intacto
3. todos los bloques `<svg>` tienen su `</svg>`
4. no hay caracteres no ASCII accidentales
5. no hay lineas largas dentro de los SVG
6. los ejemplos son realmente atomicos
7. el texto corrige o mejora el nivel del original
8. si habia version Gemini, la nueva la supera en claridad o rigor

Validaciones practicas utiles:

- contar aperturas y cierres de `<svg>`
- buscar caracteres no ASCII con `grep`
- revisar longitud de lineas con `awk`
- releer rapido el archivo entero para detectar ambiguedades

---

## 11. Errores que hay que evitar

- limitarse a parafrasear el original
- inflar longitud sin aportar claridad
- usar un tono rimbombante o "epico"
- crear ejemplos gigantes que explican demasiadas cosas a la vez
- confundir sintaxis XML con API JSX
- meter lineas largas que rompen el previewer
- dejar ejemplos con cierre faltante o atributos dudosos
- afirmar cosas absolutas cuando el comportamiento depende del contexto

---

## 12. Flujo de trabajo recomendado

Para cada THEORY nuevo:

1. leer contexto global y local
2. detectar que ideas del original son insuficientes o simplistas
3. decidir una nueva estructura `##`
4. escribir primero la explicacion tecnica
5. insertar ejemplos SVG atomicos
6. revisar consistencia pedagogica
7. validar formato y sintaxis
8. guardar como `.codex.md`

Si un THEORY ya tiene base razonable, el objetivo no es rehacer por deporte,
sino **concentrar la mejora donde de verdad sube el nivel**.

---

## 13. Regla final de calidad

Cada archivo `.codex.md` debe poder responder afirmativamente a esto:

> Si un estudiante solo leyera esta version refinada, entenderia mejor el tema,
> cometeria menos errores reales y tendria ejemplos mas utiles que con el texto
> original.

Si la respuesta no es claramente "si", el refinamiento aun no esta al nivel
correcto.
