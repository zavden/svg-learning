# 1.5 XML como base de SVG

Decir que SVG "esta basado en XML" no es un detalle decorativo. Significa que,
antes de pensar en color, geometria o animacion, el navegador necesita poder
construir un **arbol sintactico coherente** a partir del texto fuente.

En un archivo `.svg` independiente, ese proceso suele pasar por un parser XML.
Si el documento esta mal formado, el parser puede abortar y el grafico no se
renderiza. En SVG inline dentro de HTML, el navegador usa el parser HTML en
modo de "foreign content", que a veces recupera ciertos errores. Aun asi,
escribir SVG como si fuera XML estricto sigue siendo la unica estrategia seria:
mejora portabilidad, evita bugs al exportar el SVG a archivo propio y reduce
problemas con optimizadores, minificadores y generadores automaticos.

Piensa en XML como la capa que aporta estas garantias:

- estructura jerarquica sin ambiguedad,
- nombres exactos para elementos y atributos,
- reglas claras para escapar caracteres reservados,
- namespaces para distinguir vocabularios,
- serializacion estable para tooling y control de versiones.

---

## Bien formado: primero existe el documento, luego el dibujo

Un SVG "bien formado" no es necesariamente bonito ni semantico. Solo significa
que el parser puede construir un arbol valido sin adivinar intenciones del
autor. Las reglas minimas son:

- debe existir un unico elemento raiz `<svg>`,
- cada elemento debe cerrarse,
- el anidado debe ser correcto,
- el texto y los atributos no pueden romper la gramatica del documento.

Esto importa porque SVG no funciona como HTML clasico, donde el navegador suele
parchar errores authoring-time. En un `.svg` real, un cierre faltante o un
anidado cruzado puede invalidar el documento completo.

### Ejemplo 1: Arbol minimo que el parser si puede construir

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="24" y="24" width="72" height="72" rx="10"
        fill="#dbeafe" stroke="#93c5fd" stroke-width="2"/>
  <rect x="114" y="24" width="72" height="20" rx="6"
        fill="#dcfce7" stroke="#86efac" stroke-width="2"/>
  <rect x="114" y="50" width="72" height="20" rx="6"
        fill="#fef3c7" stroke="#fcd34d" stroke-width="2"/>
  <rect x="114" y="76" width="72" height="20" rx="6"
        fill="#fee2e2" stroke="#fca5a5" stroke-width="2"/>
  <text x="60" y="66" text-anchor="middle"
        font-size="12" fill="#1e3a8a" font-family="monospace">
    svg
  </text>
  <text x="150" y="38" text-anchor="middle"
        font-size="11" fill="#166534" font-family="monospace">
    title
  </text>
  <text x="150" y="64" text-anchor="middle"
        font-size="11" fill="#92400e" font-family="monospace">
    desc
  </text>
  <text x="150" y="90" text-anchor="middle"
        font-size="11" fill="#991b1b" font-family="monospace">
    circle
  </text>
  <line x1="96" y1="34" x2="114" y2="34"
        stroke="#64748b" stroke-width="2"/>
  <line x1="96" y1="60" x2="114" y2="60"
        stroke="#64748b" stroke-width="2"/>
  <line x1="96" y1="86" x2="114" y2="86"
        stroke="#64748b" stroke-width="2"/>
</svg>
```

### Ejemplo 2: Auto-cierre y cierre explicito no cumplen la misma funcion

Los elementos vacios pueden cerrarse con `/>`. Los contenedores o elementos con
hijos necesitan etiqueta de apertura y cierre.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    Elemento vacio:
  </text>
  <text x="16" y="42" font-size="10" fill="#86efac"
        font-family="monospace">
    &lt;circle cx="40" cy="40" r="18" /&gt;
  </text>
  <text x="16" y="72" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    Elemento con hijos:
  </text>
  <text x="16" y="92" font-size="10" fill="#93c5fd"
        font-family="monospace">
    &lt;g&gt; ... &lt;/g&gt;
  </text>
  <text x="16" y="110" font-size="9" fill="#fca5a5"
        font-family="monospace">
    &lt;g /&gt; no puede contener hijos despues
  </text>
</svg>
```

### Ejemplo 3: El anidado cruzado rompe el documento

XML exige jerarquia estricta. Si abres `g`, luego `text`, debes cerrar primero
`text` y despues `g`. No se pueden "entrelazar" cierres.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#86efac"
        font-family="monospace">
    Correcto:
  </text>
  <text x="16" y="42" font-size="10" fill="#93c5fd"
        font-family="monospace">
    &lt;g&gt;&lt;text&gt;Hola&lt;/text&gt;&lt;/g&gt;
  </text>
  <text x="16" y="74" font-size="10" fill="#fca5a5"
        font-family="monospace">
    Incorrecto:
  </text>
  <text x="16" y="94" font-size="10" fill="#f87171"
        font-family="monospace">
    &lt;g&gt;&lt;text&gt;Hola&lt;/g&gt;&lt;/text&gt;
  </text>
  <text x="16" y="114" font-size="9" fill="#cbd5e1"
        font-family="monospace">
    el parser pierde la jerarquia y aborta
  </text>
</svg>
```

### Ejemplo 4: Un archivo SVG solo puede tener una raiz

Puedes tener muchos hijos dentro de `<svg>`, pero no dos elementos raiz
separados al mismo nivel del documento.

```svg
<svg width="300" height="118"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    Archivo valido:
  </text>
  <text x="16" y="42" font-size="10" fill="#86efac"
        font-family="monospace">
    &lt;svg&gt; ... muchos hijos ... &lt;/svg&gt;
  </text>
  <text x="16" y="74" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    Archivo invalido:
  </text>
  <text x="16" y="94" font-size="10" fill="#f87171"
        font-family="monospace">
    &lt;svg&gt;...&lt;/svg&gt; &lt;svg&gt;...&lt;/svg&gt;
  </text>
</svg>
```

---

## Atributos y nombres: XML no completa lo que falta

En XML, los atributos forman parte dura de la sintaxis. No hay tolerancia para
atajos de HTML como `width=100` sin comillas o atributos booleanos sin valor.
Ademas, los nombres de elementos y atributos deben escribirse exactamente como
los espera SVG.

Hay un matiz sutil pero muy importante: algunos errores no siempre rompen el
documento completo; a veces lo dejan **bien formado pero semanticamente roto**.
Por ejemplo, `strokeWidth="6"` es un atributo XML valido como texto, pero no es
el nombre que el formato SVG espera en un archivo crudo. El parser no falla;
simplemente el renderer ignora ese atributo.

### Ejemplo 5: Las comillas no son opcionales

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#86efac"
        font-family="monospace">
    Correcto:
  </text>
  <text x="16" y="42" font-size="10" fill="#93c5fd"
        font-family="monospace">
    width="120" height="80"
  </text>
  <text x="16" y="74" font-size="10" fill="#86efac"
        font-family="monospace">
    Tambien valido:
  </text>
  <text x="16" y="94" font-size="10" fill="#93c5fd"
        font-family="monospace">
    width='120' height='80'
  </text>
  <text x="170" y="58" font-size="10" fill="#f87171"
        font-family="monospace">
    width=120
  </text>
  <text x="170" y="78" font-size="9" fill="#fca5a5"
        font-family="monospace">
    XML no admite esta forma
  </text>
</svg>
```

### Ejemplo 6: Los nombres son case-sensitive

Esto aplica a elementos y a atributos. `viewBox` existe; `viewbox` no. En
archivos SVG reales, `linearGradient` existe; `lineargradient` no.

```svg
<svg width="300" height="126"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#86efac"
        font-family="monospace">
    Nombres correctos:
  </text>
  <text x="16" y="42" font-size="10" fill="#93c5fd"
        font-family="monospace">
    &lt;linearGradient&gt;
  </text>
  <text x="16" y="60" font-size="10" fill="#93c5fd"
        font-family="monospace">
    viewBox="0 0 100 100"
  </text>
  <text x="16" y="90" font-size="10" fill="#fca5a5"
        font-family="monospace">
    Nombres incorrectos:
  </text>
  <text x="16" y="108" font-size="10" fill="#f87171"
        font-family="monospace">
    &lt;lineargradient&gt;  viewbox="..."
  </text>
</svg>
```

### Ejemplo 7: "strokeWidth" es un error comun al venir de JSX

En un archivo `.svg` crudo debes usar los nombres reales del formato. Las
versiones camelCase pertenecen al adaptador del framework, no al XML fuente.

```svg
<svg width="300" height="128"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="28" y="34" width="70" height="46"
        fill="none" stroke="#2563eb" stroke-width="8"/>
  <rect x="168" y="34" width="70" height="46"
        fill="none" stroke="#2563eb" strokeWidth="8"/>
  <text x="63" y="98" text-anchor="middle"
        font-size="9" fill="#1e3a8a" font-family="monospace">
    stroke-width
  </text>
  <text x="203" y="98" text-anchor="middle"
        font-size="9" fill="#991b1b" font-family="monospace">
    strokeWidth
  </text>
  <text x="150" y="116" text-anchor="middle"
        font-size="8" fill="#475569">
    el segundo atributo existe como XML, pero SVG lo ignora
  </text>
</svg>
```

### Ejemplo 8: Duplicar un atributo vuelve ambiguo el nodo

XML no permite dos atributos con el mismo nombre en el mismo elemento.

```svg
<svg width="300" height="116"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#86efac"
        font-family="monospace">
    Valido:
  </text>
  <text x="16" y="42" font-size="10" fill="#93c5fd"
        font-family="monospace">
    &lt;rect fill="red" /&gt;
  </text>
  <text x="16" y="76" font-size="10" fill="#fca5a5"
        font-family="monospace">
    Invalido:
  </text>
  <text x="16" y="96" font-size="10" fill="#f87171"
        font-family="monospace">
    &lt;rect fill="red" fill="blue" /&gt;
  </text>
</svg>
```

---

## Texto y entidades: escribir codigo dentro de otro codigo

SVG no solo contiene geometria; tambien puede contener texto, snippets visibles,
URLs, estilos inline y metadatos. En todos esos casos aparece la misma pregunta:
como representar caracteres que XML reserva para su propia gramatica.

Las reglas operativas mas utiles son estas:

- `&` debe escaparse si forma parte del contenido literal,
- `<` debe escaparse dentro de texto,
- las comillas del valor de un atributo no pueden colisionar con el delimitador
  externo,
- cuando dudas, usa la entidad XML o cambia el tipo de comilla.

### Ejemplo 9: Entidades que conviene memorizar

```svg
<svg width="300" height="132"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &amp;amp;   -&gt;   &amp;
  </text>
  <text x="16" y="44" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &amp;lt;    -&gt;   &lt;
  </text>
  <text x="16" y="66" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &amp;gt;    -&gt;   &gt;
  </text>
  <text x="16" y="88" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &amp;quot;  -&gt;   "
  </text>
  <text x="16" y="110" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &amp;apos;  -&gt;   '
  </text>
</svg>
```

### Ejemplo 10: Mostrar codigo dentro de un nodo `<text>`

Si quieres ensenar una etiqueta SVG como texto visible, debes escapar los
delimitadores para que no se interpreten como markup real.

```svg
<svg width="300" height="112"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="24" font-size="10" fill="#86efac"
        font-family="monospace">
    En el fuente escribes:
  </text>
  <text x="16" y="48" font-size="10" fill="#93c5fd"
        font-family="monospace">
    &amp;lt;rect x="10" y="10" width="80" /&amp;gt;
  </text>
  <text x="16" y="82" font-size="10" fill="#fca5a5"
        font-family="monospace">
    Incorrecto:
  </text>
  <text x="16" y="100" font-size="9" fill="#f87171"
        font-family="monospace">
    &lt;rect ... /&gt; sin escape dejaria de ser texto
  </text>
</svg>
```

### Ejemplo 11: Las comillas del atributo y las comillas internas

Dentro de un atributo puedes evitar choques usando comillas simples internas o
entidades XML.

```svg
<svg width="300" height="124"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#86efac"
        font-family="monospace">
    Opcion 1:
  </text>
  <text x="16" y="42" font-size="10" fill="#93c5fd"
        font-family="monospace">
    style="font-family:'Fira Code';"
  </text>
  <text x="16" y="74" font-size="10" fill="#86efac"
        font-family="monospace">
    Opcion 2:
  </text>
  <text x="16" y="94" font-size="10" fill="#93c5fd"
        font-family="monospace">
    style="font-family:&amp;quot;Fira Code&amp;quot;;"
  </text>
</svg>
```

### Ejemplo 12: El ampersand en valores literales debe escaparse

Esto aparece mucho en URLs con query strings, texto descriptivo o datos
serializados.

```svg
<svg width="300" height="118"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="22" font-size="10" fill="#86efac"
        font-family="monospace">
    Correcto:
  </text>
  <text x="16" y="42" font-size="10" fill="#93c5fd"
        font-family="monospace">
    data-note="x &amp;amp; y"
  </text>
  <text x="16" y="74" font-size="10" fill="#fca5a5"
        font-family="monospace">
    Incorrecto:
  </text>
  <text x="16" y="94" font-size="10" fill="#f87171"
        font-family="monospace">
    data-note="x &amp; y"
  </text>
</svg>
```

---

## Namespaces: como sabe el parser que este nodo es SVG

Un namespace XML es un identificador que le dice al parser a que vocabulario
pertenece cada nombre. En SVG, el namespace principal es:

`http://www.w3.org/2000/svg`

No se trata de una URL que el navegador "visita". Es un identificador estable.
Su funcion es evitar colisiones y permitir que distintos lenguajes XML convivan
sin ambiguedad.

En un archivo `.svg` independiente, declarar `xmlns` en la raiz es la opcion
correcta y portable. En un `<svg>` inline dentro de HTML5, el navegador ya sabe
entrar al namespace SVG al encontrar esa etiqueta. Aun asi, si luego copias ese
fragmento a un archivo propio, necesitaras volver a declarar `xmlns`.

### Ejemplo 13: Cabecera minima de un SVG externo

```svg
<svg width="300" height="128"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="24" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &lt;svg
  </text>
  <text x="32" y="44" font-size="10" fill="#93c5fd"
        font-family="monospace">
    xmlns="http://www.w3.org/2000/svg"
  </text>
  <text x="32" y="64" font-size="10" fill="#93c5fd"
        font-family="monospace">
    viewBox="0 0 100 100"
  </text>
  <text x="16" y="84" font-size="10" fill="#e2e8f0"
        font-family="monospace">
    &gt; ... &lt;/svg&gt;
  </text>
  <text x="16" y="108" font-size="9" fill="#86efac"
        font-family="monospace">
    esta forma funciona bien al guardar como archivo propio
  </text>
</svg>
```

### Ejemplo 14: Inline en HTML y externo no son el mismo contexto

La sintaxis autoral deberia ser consistente, pero el contexto de parseo cambia.

```svg
<svg width="300" height="122"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="18" y="22" width="118" height="74" rx="10"
        fill="#eff6ff" stroke="#93c5fd" stroke-width="2"/>
  <rect x="164" y="22" width="118" height="74" rx="10"
        fill="#f0fdf4" stroke="#86efac" stroke-width="2"/>
  <text x="77" y="44" text-anchor="middle"
        font-size="10" fill="#1e3a8a" font-family="monospace">
    inline HTML
  </text>
  <text x="77" y="66" text-anchor="middle"
        font-size="8" fill="#475569">
    parser HTML + namespace SVG
  </text>
  <text x="223" y="44" text-anchor="middle"
        font-size="10" fill="#166534" font-family="monospace">
    archivo .svg
  </text>
  <text x="223" y="66" text-anchor="middle"
        font-size="8" fill="#475569">
    parser XML, mas estricto
  </text>
  <text x="150" y="108" text-anchor="middle"
        font-size="8" fill="#475569">
    escribe como XML estricto y te sirven ambos contextos
  </text>
</svg>
```

### Ejemplo 15: Reutilizacion con `defs` y `use` usando `href`

En SVG moderno, `href` es la forma preferida para referenciar recursos
definidos dentro del mismo documento.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <defs>
    <circle id="dot" cx="0" cy="0" r="10" fill="#0ea5e9"/>
  </defs>
  <text x="16" y="20" font-size="10" fill="#0f172a"
        font-family="monospace">
    &lt;use href="#dot" /&gt;
  </text>
  <use href="#dot" x="78" y="68"/>
  <use href="#dot" x="128" y="68" opacity="0.7"/>
  <use href="#dot" x="178" y="68" opacity="0.4"/>
  <text x="230" y="72" font-size="8" fill="#475569">
    tres instancias
  </text>
</svg>
```

### Ejemplo 16: `xlink:href` es legado, no punto de partida

Mucho material viejo usa `xmlns:xlink` y `xlink:href`. Debes reconocerlo, pero
para ejemplos nuevos conviene priorizar `href`.

```svg
<svg width="300" height="118"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#0f172a; display:block;">
  <text x="16" y="24" font-size="10" fill="#86efac"
        font-family="monospace">
    Recomendado hoy:
  </text>
  <text x="16" y="44" font-size="10" fill="#93c5fd"
        font-family="monospace">
    &lt;use href="#icon" /&gt;
  </text>
  <text x="16" y="78" font-size="10" fill="#facc15"
        font-family="monospace">
    Legado que aun veras:
  </text>
  <text x="16" y="98" font-size="10" fill="#fde68a"
        font-family="monospace">
    &lt;use xlink:href="#icon" /&gt;
  </text>
</svg>
```

### Ejemplo 17: Si quieres HTML dentro de SVG, usa `foreignObject`

Un `<div>` no pertenece al namespace SVG. Si realmente necesitas mezclar HTML y
SVG, debes hacerlo de forma explicita.

```svg
<svg width="300" height="128"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="18" y="22" width="118" height="82" rx="10"
        fill="#fef2f2" stroke="#fca5a5" stroke-width="2"/>
  <rect x="164" y="22" width="118" height="82" rx="10"
        fill="#f0fdf4" stroke="#86efac" stroke-width="2"/>
  <text x="77" y="44" text-anchor="middle"
        font-size="9" fill="#991b1b" font-family="monospace">
    &lt;div&gt; dentro de svg
  </text>
  <text x="77" y="66" text-anchor="middle"
        font-size="8" fill="#7f1d1d">
    no pertenece al vocabulario SVG
  </text>
  <text x="223" y="44" text-anchor="middle"
        font-size="9" fill="#166534" font-family="monospace">
    &lt;foreignObject&gt;
  </text>
  <text x="223" y="66" text-anchor="middle"
        font-size="8" fill="#14532d">
    mezcla explicita de namespaces
  </text>
</svg>
```

---

## Errores silenciosos, tooling y disciplina de trabajo

Una parte frustrante de SVG es que muchos errores no producen mensajes bonitos
en pantalla. El resultado habitual es uno de estos:

- el archivo no muestra nada,
- solo una parte del contenido desaparece,
- un atributo queda ignorado y el dibujo se ve "casi bien",
- el SVG inline funciona, pero al extraerlo a archivo propio se rompe.

Por eso conviene separar dos tipos de fallos:

- **error de XML**: el documento deja de estar bien formado,
- **error semantico de SVG**: el XML es valido, pero el vocabulario esta mal.

Esa distincion ahorra mucho tiempo de depuracion.

### Ejemplo 18: Bien formado no es lo mismo que semanticamente correcto

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="18" y="18" width="118" height="90" rx="10"
        fill="#f0fdf4" stroke="#86efac" stroke-width="2"/>
  <rect x="164" y="18" width="118" height="90" rx="10"
        fill="#eff6ff" stroke="#93c5fd" stroke-width="2"/>
  <text x="77" y="42" text-anchor="middle"
        font-size="9" fill="#166534">
    Error XML
  </text>
  <text x="77" y="62" text-anchor="middle"
        font-size="8" fill="#14532d">
    fill duplicado
  </text>
  <text x="77" y="78" text-anchor="middle"
        font-size="8" fill="#14532d">
    cierre faltante
  </text>
  <text x="223" y="42" text-anchor="middle"
        font-size="9" fill="#1e3a8a">
    Error semantico
  </text>
  <text x="223" y="62" text-anchor="middle"
        font-size="8" fill="#1d4ed8">
    strokeWidth en bruto
  </text>
  <text x="223" y="78" text-anchor="middle"
        font-size="8" fill="#1d4ed8">
    viewbox mal escrito
  </text>
</svg>
```

### Ejemplo 19: Pipeline mental para depurar

```svg
<svg width="300" height="118"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="18" y="42" width="64" height="34" rx="8"
        fill="#dbeafe" stroke="#93c5fd" stroke-width="2"/>
  <rect x="118" y="42" width="64" height="34" rx="8"
        fill="#dcfce7" stroke="#86efac" stroke-width="2"/>
  <rect x="218" y="42" width="64" height="34" rx="8"
        fill="#fef3c7" stroke="#fcd34d" stroke-width="2"/>
  <text x="50" y="63" text-anchor="middle"
        font-size="9" fill="#1e3a8a" font-family="monospace">
    parse
  </text>
  <text x="150" y="63" text-anchor="middle"
        font-size="9" fill="#166534" font-family="monospace">
    DOM
  </text>
  <text x="250" y="63" text-anchor="middle"
        font-size="9" fill="#92400e" font-family="monospace">
    render
  </text>
  <line x1="82" y1="59" x2="118" y2="59"
        stroke="#64748b" stroke-width="2"/>
  <line x1="182" y1="59" x2="218" y2="59"
        stroke="#64748b" stroke-width="2"/>
  <text x="150" y="98" text-anchor="middle"
        font-size="8" fill="#475569">
    pregunta siempre en que etapa se rompio
  </text>
</svg>
```

### Ejemplo 20: Checklist previo antes de dar un SVG por bueno

```svg
<svg width="300" height="136"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0; background:#f8fafc; display:block;">
  <rect x="22" y="18" width="256" height="100" rx="12"
        fill="white" stroke="#cbd5e1" stroke-width="2"/>
  <text x="38" y="40" font-size="9" fill="#0f172a">
    1. Unica raiz svg y cierres correctos
  </text>
  <text x="38" y="58" font-size="9" fill="#0f172a">
    2. Atributos con nombres SVG reales
  </text>
  <text x="38" y="76" font-size="9" fill="#0f172a">
    3. Caracteres reservados escapados
  </text>
  <text x="38" y="94" font-size="9" fill="#0f172a">
    4. xmlns presente si va a ser archivo externo
  </text>
  <text x="38" y="112" font-size="9" fill="#0f172a">
    5. Revisar inline y archivo por separado si aplica
  </text>
</svg>
```

### Regla operativa final

Si tuvieras que reducir toda esta seccion a una disciplina de trabajo, seria
esta:

- escribe SVG fuente como XML estricto aunque hoy vaya inline,
- no mezcles sintaxis de frameworks con sintaxis de archivo `.svg`,
- trata los nombres de elementos y atributos como API exacta,
- escapa caracteres reservados antes de que el parser los confunda con markup,
- anade `xmlns` cuando el SVG pueda vivir fuera del HTML actual.

Cuando entiendes esto, SVG deja de parecer "HTML dibujable" y pasa a verse como
lo que realmente es: un lenguaje XML especializado en describir escenas
graficas.
