# Glosario — Texto en SVG

---

## Línea base (baseline)

La línea imaginaria sobre la que "descansan" los caracteres. El atributo `y` de `<text>` apunta a esta línea, **no** al borde superior del texto. Los caracteres con descendentes (g, p, q, y) cuelgan por debajo.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <line x1="10" y1="55" x2="290" y2="55" stroke="#ef4444" stroke-width="1.5"/>
  <text x="10" y="55" font-size="30" font-family="Georgia, serif" fill="#1e293b">SVGpyqg</text>
  <text x="150" y="72" text-anchor="middle" font-size="7.5" fill="#ef4444">línea base (y=55)</text>
</svg>
```

---

## `text-anchor`

Controla qué parte del texto se alinea con la coordenada `x`. Valores: `start` (borde izquierdo), `middle` (centro), `end` (borde derecho).

---

## `dominant-baseline`

Controla qué punto del texto coincide con la coordenada `y`. `auto` = línea base, `middle` = centro vertical, `hanging` = borde superior.

---

## `<tspan>`

Sub-elemento de `<text>` para aplicar estilos diferentes a fragmentos de texto o reposicionar partes del texto. Hereda los estilos del `<text>` padre.

```svg
<svg width="300" height="55"
     viewBox="0 0 300 55"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="20" y="30" font-size="15" fill="#1e293b">
    Normal
    <tspan fill="#3b82f6" font-weight="700">azul bold</tspan>
    <tspan font-style="italic" fill="#10b981">italic verde</tspan>
    normal
  </text>
  <text x="20" y="50" font-size="7" fill="#94a3b8">tspan hereda de text y puede sobreescribir</text>
</svg>
```

---

## `<textPath>`

Elemento hijo de `<text>` que hace que el texto siga la geometría de un `<path>` referenciado por su `id`.

---

## `startOffset`

Atributo de `<textPath>`. Define dónde empieza el texto a lo largo del path. `0%` = inicio del path, `50%` = mitad, `100%` = final. Combinado con `text-anchor="middle"`, centra el texto en el path.

---

## Word wrap

Capacidad de romper automáticamente el texto al final de la línea. El texto SVG **no tiene word wrap** — el texto se sale del área sin aviso. Para texto multilinea, se usan múltiples `<tspan dy>` o `<foreignObject>`.

---

## `dy`

Desplazamiento vertical **relativo** en `<text>` o `<tspan>`. `dy="1.4em"` mueve el texto 1.4× el tamaño de fuente hacia abajo — el mecanismo estándar para simular saltos de línea. Acepta una lista de valores para desplazar carácter a carácter.

---

## Texto como path

Cuando el texto se convierte a `<path>` (ej. en Inkscape: Object → Path), el texto deja de ser texto real: ya no es seleccionable, accesible ni indexable, pero es independiente de la fuente instalada.
