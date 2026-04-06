# Cheat sheet — Marcadores

---

## Anatomía de un `<marker>`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    estructura de &lt;marker&gt;
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#f1f5f9" stroke="#cbd5e1" stroke-width="1.5" rx="5"/>

  <text x="25" y="56" font-size="8" font-family="monospace" fill="#1e293b">
    &lt;marker id="flecha"
  </text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>viewBox="0 0 10 10"
  </text>
  <text x="115" y="70" font-size="7" fill="#3b82f6">← sistema de coords interno</text>

  <text x="25" y="84" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>refX="10" refY="5"
  </text>
  <text x="115" y="84" font-size="7" fill="#3b82f6">← punto de anclaje</text>

  <text x="25" y="98" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>markerWidth="6"
  </text>
  <text x="115" y="98" font-size="7" fill="#3b82f6">← ancho del viewport</text>

  <text x="25" y="112" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>markerHeight="6"
  </text>

  <text x="25" y="126" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>markerUnits="strokeWidth"
  </text>
  <text x="165" y="126" font-size="7" fill="#3b82f6">← o userSpaceOnUse</text>

  <text x="25" y="140" font-size="8" font-family="monospace" fill="#1e293b">
    <tspan fill="#64748b">        </tspan>orient="auto"&gt;
  </text>
  <text x="115" y="140" font-size="7" fill="#3b82f6">← o auto-start-reverse / 0 / Ndeg</text>

  <!-- contenido -->
  <rect x="30" y="150" width="255" height="26" fill="#dbeafe" stroke="#3b82f6" rx="2"/>
  <text x="38" y="167" font-size="7" font-family="monospace" fill="#1e40af">
    &lt;path d="M 0 0 L 10 5 L 0 10 z" fill="#3b82f6"/&gt;
  </text>

  <text x="25" y="192" font-size="8" font-family="monospace" fill="#1e293b">
    &lt;/marker&gt;
  </text>

  <!-- Aplicación -->
  <line x1="30" y1="218" x2="255" y2="218" stroke="#cbd5e1" stroke-dasharray="3,2"/>
  <text x="25" y="235" font-size="7" font-family="monospace" fill="#64748b">
    &lt;line ... marker-end="url(#flecha)"/&gt;
  </text>
</svg>
```

---

## Los tres atributos de aplicación

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    dónde aparece cada marker-*
  </text>

  <rect x="10" y="34" width="280" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="48" font-size="8" font-weight="bold" fill="#166534">marker-start</text>
  <text x="18" y="60" font-size="7" fill="#64748b">primer punto del path (tras el primer M)</text>

  <rect x="10" y="78" width="280" height="48" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="18" y="92" font-size="8" font-weight="bold" fill="#92400e">marker-mid</text>
  <text x="18" y="104" font-size="7" fill="#64748b">cada vértice intermedio (ni el primero ni el último)</text>
  <text x="18" y="115" font-size="7" fill="#64748b">solo en anclas explícitas del path</text>

  <rect x="10" y="132" width="280" height="38" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="18" y="146" font-size="8" font-weight="bold" fill="#991b1b">marker-end</text>
  <text x="18" y="158" font-size="7" fill="#64748b">último punto del path</text>

  <text x="150" y="188" text-anchor="middle" font-size="7" fill="#94a3b8">
    marker="url(#id)" = atajo que aplica a los tres
  </text>
</svg>
```

---

## Galería de diseños listos

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    markers prediseñados
  </text>

  <defs>
    <marker id="g-solid" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="#1e293b"/>
    </marker>
    <marker id="g-hollow" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 0 L 10 5 L 0 10 z" fill="white" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <marker id="g-vee" viewBox="0 0 10 10" refX="9" refY="5"
            markerWidth="7" markerHeight="7" orient="auto">
      <path d="M 0 1 L 9 5 L 0 9" fill="none" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <marker id="g-bar" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="4" markerHeight="8" orient="auto">
      <rect x="4" y="0" width="2" height="10" fill="#1e293b"/>
    </marker>
    <marker id="g-dot" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <circle cx="5" cy="5" r="4" fill="#1e293b"/>
    </marker>
    <marker id="g-ring" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <circle cx="5" cy="5" r="3.5" fill="white" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
    <marker id="g-square" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="5" markerHeight="5">
      <rect x="1" y="1" width="8" height="8" fill="#1e293b"/>
    </marker>
    <marker id="g-cross" viewBox="0 0 10 10" refX="5" refY="5"
            markerWidth="6" markerHeight="6">
      <line x1="0" y1="0" x2="10" y2="10" stroke="#1e293b" stroke-width="1.5"/>
      <line x1="10" y1="0" x2="0" y2="10" stroke="#1e293b" stroke-width="1.5"/>
    </marker>
  </defs>

  <!-- Fila flechas -->
  <line x1="25" y1="55"  x2="80" y2="55"  stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-solid)"/>
  <text x="95" y="58" font-size="7" fill="#64748b">solid</text>

  <line x1="155" y1="55" x2="210" y2="55" stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-hollow)"/>
  <text x="225" y="58" font-size="7" fill="#64748b">hollow</text>

  <line x1="25" y1="85"  x2="80" y2="85"  stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-vee)"/>
  <text x="95" y="88" font-size="7" fill="#64748b">vee</text>

  <line x1="155" y1="85" x2="210" y2="85" stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-bar)"/>
  <text x="225" y="88" font-size="7" fill="#64748b">bar</text>

  <!-- Fila puntos -->
  <line x1="25" y1="120" x2="80" y2="120" stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-dot)"/>
  <text x="95" y="123" font-size="7" fill="#64748b">dot</text>

  <line x1="155" y1="120" x2="210" y2="120" stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-ring)"/>
  <text x="225" y="123" font-size="7" fill="#64748b">ring</text>

  <line x1="25" y1="150" x2="80" y2="150" stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-square)"/>
  <text x="95" y="153" font-size="7" fill="#64748b">square</text>

  <line x1="155" y1="150" x2="210" y2="150" stroke="#1e293b" stroke-width="1.5" marker-end="url(#g-cross)"/>
  <text x="225" y="153" font-size="7" fill="#64748b">cross</text>

  <rect x="15" y="180" width="270" height="70" fill="#f1f5f9" stroke="#cbd5e1" rx="3"/>
  <text x="25" y="194" font-size="7" font-weight="bold" fill="#1e293b">atributos clave por tipo:</text>
  <text x="25" y="207" font-size="6.5" fill="#64748b">• flecha con pico: refX = markerWidth del viewBox (extremo derecho)</text>
  <text x="25" y="219" font-size="6.5" fill="#64748b">• símbolo centrado: refX = markerWidth/2, refY = markerHeight/2</text>
  <text x="25" y="231" font-size="6.5" fill="#64748b">• con dirección (vee, bar, arrow): orient="auto"</text>
  <text x="25" y="243" font-size="6.5" fill="#64748b">• sin dirección (dot, square): omitir orient o usar orient="0"</text>
</svg>
```

---

## Esqueletos copy-paste

```svg
<svg width="300" height="290"
     viewBox="0 0 300 290"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    snippets base
  </text>

  <!-- Flecha simple -->
  <rect x="10" y="34" width="280" height="58" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="18" y="48" font-size="8" font-weight="bold" fill="#1e40af">flecha simple</text>
  <text x="18" y="60" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;marker id="arrow" viewBox="0 0 10 10"</text>
  <text x="18" y="70" font-size="6.5" fill="#1e293b" font-family="monospace">        refX="10" refY="5" markerWidth="6"</text>
  <text x="18" y="80" font-size="6.5" fill="#1e293b" font-family="monospace">        markerHeight="6" orient="auto"&gt;</text>
  <text x="18" y="90" font-size="6.5" fill="#1e293b" font-family="monospace">  &lt;path d="M 0 0 L 10 5 L 0 10 z"/&gt;&lt;/marker&gt;</text>

  <!-- Bidireccional -->
  <rect x="10" y="98" width="280" height="58" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="112" font-size="8" font-weight="bold" fill="#166534">flecha bidireccional</text>
  <text x="18" y="124" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;marker id="arr" ...</text>
  <text x="18" y="134" font-size="6.5" fill="#1e293b" font-family="monospace">        orient="auto-start-reverse"&gt;...&lt;/marker&gt;</text>
  <text x="18" y="144" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;line ... marker-start="url(#arr)"</text>
  <text x="18" y="154" font-size="6.5" fill="#1e293b" font-family="monospace">          marker-end="url(#arr)"/&gt;</text>

  <!-- Dot chart -->
  <rect x="10" y="162" width="280" height="48" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="18" y="176" font-size="8" font-weight="bold" fill="#92400e">dots en un gráfico</text>
  <text x="18" y="188" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;marker id="dot" refX="5" refY="5"</text>
  <text x="18" y="198" font-size="6.5" fill="#1e293b" font-family="monospace">        markerUnits="userSpaceOnUse"&gt;</text>
  <text x="18" y="208" font-size="6.5" fill="#1e293b" font-family="monospace">  &lt;circle cx="5" cy="5" r="4"/&gt;&lt;/marker&gt;</text>

  <!-- Color heredado -->
  <rect x="10" y="216" width="280" height="38" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="18" y="230" font-size="8" font-weight="bold" fill="#9d174d">hereda el color del trazo</text>
  <text x="18" y="242" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;path d="..." fill="context-stroke"/&gt;</text>
  <text x="18" y="252" font-size="6.5" fill="#64748b">dentro del marker, para cualquier fill/stroke</text>

  <!-- Aplicar -->
  <rect x="10" y="260" width="280" height="22" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="18" y="274" font-size="6.5" fill="#1e293b" font-family="monospace">&lt;line ... marker-start · marker-mid · marker-end · marker/&gt;</text>
</svg>
```
