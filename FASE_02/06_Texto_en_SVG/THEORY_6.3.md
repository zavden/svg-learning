# 6.3 Alineación y posicionamiento

`text-anchor` controla la alineación horizontal, `dominant-baseline` controla la vertical. `dx`/`dy` añaden desplazamientos relativos.

---

## `text-anchor` — alineación horizontal

Determina si `x` es el borde izquierdo, el centro, o el borde derecho del texto.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Línea de referencia x=150 -->
  <line x1="150" y1="0" x2="150" y2="130" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="153" y="8" font-size="7" fill="#ef4444">x=150</text>

  <!-- start: x es el borde izquierdo -->
  <text x="150" y="42" font-size="16" text-anchor="start" fill="#3b82f6">Texto</text>
  <text x="150" y="54" font-size="7" text-anchor="start" fill="#94a3b8">start — empieza en x</text>

  <!-- middle: x es el centro -->
  <text x="150" y="80" font-size="16" text-anchor="middle" fill="#10b981">Texto</text>
  <text x="150" y="92" font-size="7" text-anchor="middle" fill="#94a3b8">middle — centrado en x</text>

  <!-- end: x es el borde derecho -->
  <text x="150" y="115" font-size="16" text-anchor="end" fill="#f59e0b">Texto</text>
  <text x="150" y="127" font-size="7" text-anchor="end" fill="#94a3b8">end — termina en x</text>
</svg>
```

---

## `dominant-baseline` — alineación vertical

Determina qué punto del texto coincide con la coordenada `y`.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Línea de referencia y=60 -->
  <line x1="0" y1="60" x2="300" y2="60" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="3" y="56" font-size="7" fill="#ef4444">y=60</text>

  <!-- auto (default): y es la línea base -->
  <text x="20" y="60" font-size="18" dominant-baseline="auto" fill="#3b82f6">auto</text>
  <text x="30" y="88" text-anchor="middle" font-size="7" fill="#94a3b8">línea base</text>

  <!-- middle: y es el centro vertical -->
  <text x="95" y="60" font-size="18" dominant-baseline="middle" fill="#10b981">mid</text>
  <text x="105" y="88" text-anchor="middle" font-size="7" fill="#94a3b8">centro</text>

  <!-- hanging: y es el borde superior -->
  <text x="160" y="60" font-size="18" dominant-baseline="hanging" fill="#f59e0b">hang</text>
  <text x="175" y="88" text-anchor="middle" font-size="7" fill="#94a3b8">borde sup.</text>

  <!-- text-before-edge: similar a hanging -->
  <text x="235" y="60" font-size="18" dominant-baseline="text-before-edge" fill="#a855f7">tbe</text>
  <text x="250" y="88" text-anchor="middle" font-size="7" fill="#94a3b8">text-before-edge</text>
</svg>
```

---

## `dominant-baseline="middle"` — texto centrado en un elemento

El patrón más útil: centrar texto tanto horizontal como verticalmente.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Centrar texto en un rectángulo -->
  <rect x="20" y="20" width="120" height="70" rx="8" fill="#dbeafe" stroke="#3b82f6" stroke-width="2"/>
  <text x="80" y="55"
        text-anchor="middle"
        dominant-baseline="middle"
        font-size="16" fill="#1e40af">Centrado</text>

  <!-- En un círculo -->
  <circle cx="220" cy="55" r="40" fill="#dcfce7" stroke="#10b981" stroke-width="2"/>
  <text x="220" y="55"
        text-anchor="middle"
        dominant-baseline="middle"
        font-size="16" fill="#166534">OK</text>

  <!-- Notas -->
  <text x="80"  y="102" text-anchor="middle" font-size="7.5" fill="#64748b">text-anchor="middle"</text>
  <text x="220" y="102" text-anchor="middle" font-size="7.5" fill="#64748b">dominant-baseline="middle"</text>
</svg>
```

---

## `dx` y `dy` — desplazamientos relativos

`dy` es la forma estándar de simular saltos de línea en SVG.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- dy para texto multilinea -->
  <text x="20" y="28" font-size="13" fill="#1e293b">
    <tspan>Primera línea</tspan>
    <tspan x="20" dy="1.4em">Segunda línea</tspan>
    <tspan x="20" dy="1.4em">Tercera línea</tspan>
  </text>
  <text x="20" y="95" font-size="7.5" fill="#64748b">dy="1.4em" → siguiente línea</text>

  <!-- dx individual por carácter -->
  <line x1="155" y1="0" x2="155" y2="130" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="65" font-size="16" fill="#3b82f6"
        dx="0 4 -2 6 -1 5">KERNING</text>
  <text x="165" y="80" font-size="7.5" fill="#64748b">dx="0 4 -2 6 -1 5"</text>
  <text x="165" y="92" font-size="7" fill="#94a3b8">cada caracter tiene su dx</text>
</svg>
```

---

## `rotate` — rotación de caracteres

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Todos los caracteres con la misma rotación -->
  <text x="20" y="50" font-size="22" rotate="15" fill="#3b82f6">TILTED</text>

  <!-- Cada carácter con rotación diferente -->
  <text x="160" y="60" font-size="22" fill="#10b981"
        rotate="0 20 40 60 40 20 0">WAVY</text>

  <text x="20"  y="90" font-size="7.5" fill="#64748b">rotate="15" — todos iguales</text>
  <text x="185" y="90" font-size="7.5" fill="#64748b">rotate="0 20 40..." — por caracter</text>
</svg>
```
