# Referencia rápida — Texto en SVG

---

## Cheat sheet: atributos de `<text>`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e3a5f"/>
  <text x="150" y="15" text-anchor="middle" font-size="9" fill="#93c5fd">Atributos de &lt;text&gt;</text>

  <!-- Posición -->
  <text x="10" y="38" font-size="7.5" fill="#60a5fa" font-family="monospace">x, y</text>
  <text x="90" y="38" font-size="7" fill="#94a3b8">punto de anclaje (y = línea base)</text>

  <!-- font -->
  <text x="10" y="55" font-size="7.5" fill="#34d399" font-family="monospace">font-family</text>
  <text x="90" y="55" font-size="7" fill="#94a3b8">"Arial, sans-serif"</text>

  <text x="10" y="70" font-size="7.5" fill="#34d399" font-family="monospace">font-size</text>
  <text x="90" y="70" font-size="7" fill="#94a3b8">número (px/em/etc.)</text>

  <text x="10" y="85" font-size="7.5" fill="#34d399" font-family="monospace">font-weight</text>
  <text x="90" y="85" font-size="7" fill="#94a3b8">100–900 | normal | bold</text>

  <text x="10" y="100" font-size="7.5" fill="#34d399" font-family="monospace">font-style</text>
  <text x="90" y="100" font-size="7" fill="#94a3b8">normal | italic | oblique</text>

  <text x="10" y="115" font-size="7.5" fill="#34d399" font-family="monospace">font-variant</text>
  <text x="90" y="115" font-size="7" fill="#94a3b8">normal | small-caps</text>

  <!-- Decoración -->
  <text x="10" y="132" font-size="7.5" fill="#fbbf24" font-family="monospace">text-decoration</text>
  <text x="90" y="132" font-size="7" fill="#94a3b8">underline | overline | line-through</text>

  <text x="10" y="147" font-size="7.5" fill="#fbbf24" font-family="monospace">letter-spacing</text>
  <text x="90" y="147" font-size="7" fill="#94a3b8">número (+ separa, – acerca)</text>

  <text x="10" y="162" font-size="7.5" fill="#fbbf24" font-family="monospace">word-spacing</text>
  <text x="90" y="162" font-size="7" fill="#94a3b8">número</text>

  <!-- Alineación -->
  <text x="10" y="179" font-size="7.5" fill="#fb923c" font-family="monospace">text-anchor</text>
  <text x="90" y="179" font-size="7" fill="#94a3b8">start | middle | end</text>

  <text x="10" y="194" font-size="7.5" fill="#fb923c" font-family="monospace">dominant-baseline</text>
  <text x="90" y="194" font-size="7" fill="#94a3b8">auto | middle | hanging</text>

  <!-- Desplazamiento -->
  <text x="10" y="211" font-size="7.5" fill="#f87171" font-family="monospace">dx, dy</text>
  <text x="90" y="211" font-size="7" fill="#94a3b8">desplazamiento relativo (lista por caracter)</text>

  <text x="10" y="226" font-size="7.5" fill="#f87171" font-family="monospace">rotate</text>
  <text x="90" y="226" font-size="7" fill="#94a3b8">ángulo o lista de ángulos</text>
</svg>
```

---

## `text-anchor` en tres variantes

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <line x1="150" y1="0" x2="150" y2="80" stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>

  <text x="150" y="22" text-anchor="start"  font-size="13" fill="#3b82f6">start</text>
  <text x="150" y="48" text-anchor="middle" font-size="13" fill="#10b981">middle</text>
  <text x="150" y="73" text-anchor="end"    font-size="13" fill="#f59e0b">end</text>
</svg>
```

---

## Patrón: texto centrado en un elemento

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Botón -->
  <rect x="20" y="20" width="120" height="50" rx="8" fill="#3b82f6"/>
  <text x="80" y="45"
        text-anchor="middle"
        dominant-baseline="middle"
        font-size="14" fill="white" font-weight="600">Guardar</text>

  <!-- Badge -->
  <circle cx="230" cy="45" r="30" fill="#ef4444"/>
  <text x="230" y="45"
        text-anchor="middle"
        dominant-baseline="middle"
        font-size="18" fill="white" font-weight="700">42</text>

  <text x="80"  y="82" text-anchor="middle" font-size="7" fill="#64748b">x=cx, y=cy, text-anchor="middle", dominant-baseline="middle"</text>
</svg>
```

---

## Patrón: texto multilinea con `<tspan>`

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Estructura recomendada -->
  <text x="20" y="25" font-size="13" fill="#1e293b">
    <tspan font-weight="700">Título</tspan>
    <tspan x="20" dy="1.5em" font-size="11" fill="#64748b">Primera línea de descripción</tspan>
    <tspan x="20" dy="1.4em" font-size="11" fill="#64748b">Segunda línea de descripción</tspan>
  </text>

  <!-- Código esquemático -->
  <rect x="0" y="78" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="92" text-anchor="middle" font-size="7" fill="#94a3b8" font-family="monospace">
    &lt;tspan x="mismo-x" dy="1.4em"&gt; por cada línea &lt;/tspan&gt;
  </text>
</svg>
```
