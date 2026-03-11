# 7.6 Atributos vs CSS — prioridad de estilos

Fill, stroke y todos los atributos de presentación se pueden especificar de tres formas. La **prioridad** determina cuál gana cuando hay conflicto.

---

## Las tres formas de aplicar estilos

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .css-clase { fill: #10b981; }
  </style>

  <!-- 1. Atributo de presentación -->
  <rect x="10" y="20" width="70" height="50" fill="#3b82f6"/>
  <text x="45" y="82" text-anchor="middle" font-size="7.5" fill="#3b82f6">atributo</text>
  <text x="45" y="93" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill="..."</text>

  <!-- 2. Hoja de estilos / clase CSS -->
  <rect x="115" y="20" width="70" height="50" class="css-clase"/>
  <text x="150" y="82" text-anchor="middle" font-size="7.5" fill="#10b981">CSS clase</text>
  <text x="150" y="93" text-anchor="middle" font-size="6.5" fill="#94a3b8">class="..."</text>

  <!-- 3. Style inline -->
  <rect x="220" y="20" width="70" height="50" style="fill: #a855f7;"/>
  <text x="255" y="82" text-anchor="middle" font-size="7.5" fill="#a855f7">style inline</text>
  <text x="255" y="93" text-anchor="middle" font-size="6.5" fill="#94a3b8">style="fill:..."</text>

  <text x="150" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">
    las tres son válidas — la prioridad decide cuál gana
  </text>
</svg>
```

---

## La cascada: de menor a mayor prioridad

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Cascada SVG (de menor a mayor)</text>

  <!-- Escalera de prioridad -->
  <rect x="10"  y="30" width="280" height="16" rx="3" fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>
  <text x="20"  y="41" font-size="7.5" fill="#94a3b8">1. Herencia del padre</text>

  <rect x="10"  y="50" width="260" height="16" rx="3" fill="#dbeafe" stroke="#93c5fd" stroke-width="1"/>
  <text x="20"  y="61" font-size="7.5" fill="#3b82f6">2. Atributos de presentación (fill="red")</text>
  <text x="275" y="61" text-anchor="end" font-size="7" fill="#64748b">más bajo</text>

  <rect x="10"  y="70" width="240" height="16" rx="3" fill="#dcfce7" stroke="#86efac" stroke-width="1"/>
  <text x="20"  y="81" font-size="7.5" fill="#15803d">3. CSS en hoja de estilos (por especificidad)</text>

  <rect x="10"  y="90" width="220" height="16" rx="3" fill="#fef9c3" stroke="#fde68a" stroke-width="1"/>
  <text x="20"  y="101" font-size="7.5" fill="#92400e">4. Style inline (style="fill: red;")</text>

  <rect x="10"  y="110" width="200" height="16" rx="3" fill="#fecaca" stroke="#fca5a5" stroke-width="1"/>
  <text x="20"  y="121" font-size="7.5" fill="#dc2626">5. CSS !important</text>
  <text x="215" y="121" text-anchor="end" font-size="7" fill="#64748b">más alto</text>
</svg>
```

---

## El punto crítico: CSS supera a los atributos

Un selector CSS normal sobreescribe un atributo de presentación.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .sobreescrito { fill: #10b981; }
  </style>

  <!-- fill="red" + clase CSS → CSS gana (verde) -->
  <rect x="20" y="20" width="80" height="55"
        fill="#ef4444" class="sobreescrito"/>
  <text x="60" y="88" text-anchor="middle" font-size="7.5" fill="#10b981">CSS gana ✓</text>
  <text x="60" y="99" text-anchor="middle" font-size="7" fill="#94a3b8">fill="red" + clase verde</text>
  <text x="60" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">→ resultado: verde</text>

  <!-- style inline + clase CSS → style inline gana -->
  <rect x="150" y="20" width="130" height="55"
        class="sobreescrito" style="fill: #a855f7;"/>
  <text x="215" y="88" text-anchor="middle" font-size="7.5" fill="#a855f7">style inline gana ✓</text>
  <text x="215" y="99" text-anchor="middle" font-size="7" fill="#94a3b8">clase verde + style morado</text>
  <text x="215" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">→ resultado: morado</text>
</svg>
```

---

## Cuándo usar cada forma

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Cuándo usar cada forma</text>

  <text x="10" y="36" font-size="8" fill="#3b82f6" font-weight="700">Atributos</text>
  <text x="10" y="49" font-size="7" fill="#64748b">• SVG generado por Figma/Inkscape</text>
  <text x="10" y="61" font-size="7" fill="#64748b">• Elementos únicos sin reutilización</text>
  <text x="10" y="73" font-size="7" fill="#64748b">• SVG embebido como icon simple</text>

  <line x1="155" y1="22" x2="155" y2="140" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="36" font-size="8" fill="#10b981" font-weight="700">CSS + clases</text>
  <text x="165" y="49" font-size="7" fill="#64748b">• Muchos elementos del mismo tipo</text>
  <text x="165" y="61" font-size="7" fill="#64748b">• Temas cambiables (dark mode)</text>
  <text x="165" y="73" font-size="7" fill="#64748b">• Estados con JS (hover, active)</text>
  <text x="165" y="85" font-size="7" fill="#64748b">• Variables CSS (var(--color))</text>
  <text x="165" y="97" font-size="7" fill="#64748b">• @media queries responsivo</text>

  <text x="150" y="120" text-anchor="middle" font-size="7.5" fill="#64748b">
    Para SVGs interactivos o temáticos → CSS
  </text>
  <text x="150" y="133" text-anchor="middle" font-size="7" fill="#94a3b8">
    Para SVGs estáticos simples → atributos
  </text>
</svg>
```
