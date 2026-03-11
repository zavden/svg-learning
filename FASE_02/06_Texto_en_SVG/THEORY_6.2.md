# 6.2 Atributos tipográficos

Los atributos tipográficos de SVG son los mismos que en CSS. Se aplican directamente en el elemento o se heredan desde grupos padre.

---

## `font-family` y `font-size`

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- font-family -->
  <text x="10" y="25"  font-size="14" font-family="sans-serif"     fill="#1e293b">Sans-serif</text>
  <text x="10" y="48"  font-size="14" font-family="serif"          fill="#1e293b">Serif</text>
  <text x="10" y="71"  font-size="14" font-family="monospace"      fill="#1e293b">Monospace</text>
  <text x="10" y="94"  font-size="14" font-family="cursive"        fill="#1e293b">Cursive</text>

  <!-- font-size variante -->
  <line x1="160" y1="0" x2="160" y2="160" stroke="#e2e8f0" stroke-width="1"/>

  <text x="170" y="20"  font-size="8"  fill="#3b82f6">font-size=8</text>
  <text x="170" y="38"  font-size="12" fill="#3b82f6">font-size=12</text>
  <text x="170" y="62"  font-size="16" fill="#3b82f6">size=16</text>
  <text x="170" y="92"  font-size="22" fill="#3b82f6">size=22</text>
  <text x="170" y="130" font-size="30" fill="#3b82f6">30</text>
</svg>
```

---

## `font-weight` y `font-style`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- font-weight -->
  <text x="10" y="25"  font-size="14" font-weight="100" fill="#1e293b">Weight 100 (thin)</text>
  <text x="10" y="45"  font-size="14" font-weight="400" fill="#1e293b">Weight 400 (normal)</text>
  <text x="10" y="65"  font-size="14" font-weight="700" fill="#1e293b">Weight 700 (bold)</text>
  <text x="10" y="85"  font-size="14" font-weight="900" fill="#1e293b">Weight 900 (black)</text>

  <!-- font-style -->
  <line x1="185" y1="0" x2="185" y2="130" stroke="#e2e8f0" stroke-width="1"/>
  <text x="195" y="30"  font-size="14" font-style="normal"  fill="#3b82f6">normal</text>
  <text x="195" y="55"  font-size="14" font-style="italic"  fill="#3b82f6">italic</text>
  <text x="195" y="80"  font-size="14" font-style="oblique" fill="#3b82f6">oblique</text>

  <!-- small-caps -->
  <text x="10" y="115" font-size="14" font-variant="small-caps" fill="#64748b">Small Caps Ejemplo</text>
</svg>
```

---

## `text-decoration`

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="20" y="28"  font-size="14" text-decoration="underline"    fill="#3b82f6">underline</text>
  <text x="20" y="56"  font-size="14" text-decoration="overline"     fill="#10b981">overline</text>
  <text x="20" y="84"  font-size="14" text-decoration="line-through" fill="#ef4444">line-through</text>
  <text x="20" y="112" font-size="14" text-decoration="underline overline" fill="#a855f7">underline + overline</text>
</svg>
```

---

## `letter-spacing` y `word-spacing`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- letter-spacing -->
  <text x="150" y="22" text-anchor="middle" font-size="8.5" fill="#64748b">letter-spacing</text>
  <text x="20" y="42"  font-size="13" letter-spacing="-2"  fill="#1e293b">COMPRIMIDO (-2)</text>
  <text x="20" y="62"  font-size="13" letter-spacing="0"   fill="#1e293b">NORMAL (0)</text>
  <text x="20" y="82"  font-size="13" letter-spacing="3"   fill="#1e293b">EXPANDIDO (3)</text>
  <text x="20" y="102" font-size="13" letter-spacing="8"   fill="#1e293b">MUY ANCHO (8)</text>

  <!-- word-spacing -->
  <line x1="0" y1="112" x2="300" y2="112" stroke="#e2e8f0" stroke-width="1"/>
  <text x="20"  y="126" font-size="11" word-spacing="15" fill="#3b82f6">hola mundo SVG</text>
  <text x="200" y="126" font-size="8.5" fill="#94a3b8">word-spacing=15</text>
</svg>
```

---

## Herencia tipográfica desde grupos

Los atributos tipográficos se heredan — define en `<g>` para no repetirlos.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin herencia: cada texto repite font-family y fill -->
  <text x="10" y="25"  font-family="monospace" font-size="11" fill="#64748b">const x = 1;</text>
  <text x="10" y="42"  font-family="monospace" font-size="11" fill="#64748b">let y = 2;</text>
  <text x="10" y="59"  font-family="monospace" font-size="11" fill="#ef4444">// repetitivo</text>

  <line x1="155" y1="0" x2="155" y2="110" stroke="#e2e8f0" stroke-width="1"/>

  <!-- Con herencia de <g> -->
  <g font-family="monospace" font-size="11" fill="#64748b">
    <text x="165" y="25">const x = 1;</text>
    <text x="165" y="42">let y = 2;</text>
    <text x="165" y="59" fill="#10b981">// herencia ✓</text>
    <!-- fill="#10b981" sobreescribe solo este -->
  </g>
</svg>
```
