# 21.6 `pointer-events` en SVG

`pointer-events` controla *qué partes* de un elemento responden al puntero. En SVG es mucho más granular que en HTML: puedes pedir "solo el fill", "solo el stroke" o "nada en absoluto". Es la herramienta para arreglar problemas de capas que se roban los clicks.

---

## Qué controla

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué zonas reciben eventos
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">es una propiedad CSS / atributo SVG</text>
  <text x="25" y="70" font-size="7" fill="#475569">• decide qué áreas "capturan" clicks y hovers</text>
  <text x="25" y="84" font-size="7" fill="#475569">• se aplica al elemento entero, no al área del viewBox</text>
  <text x="25" y="98" font-size="7" fill="#475569">• hereda de padre a hijo (como CSS normal)</text>

  <rect x="15" y="126" width="270" height="102" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">/* como atributo */</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">&lt;circle pointer-events="none"/&gt;</text>

  <text x="25" y="180" font-size="7" font-family="monospace" fill="#94a3b8">/* como CSS */</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">.capa-decor {</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">  pointer-events: none;</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#34d399">}</text>
</svg>
```

---

## Los valores, de más común a más raro

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    tabla de valores
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">valor</text>
  <text x="130" y="46" font-size="7" font-weight="bold" fill="#1e293b">responde donde</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" font-family="monospace" fill="#3b82f6">visiblePainted</text>
  <text x="130" y="66" font-size="7" fill="#475569">fill o stroke visibles ← default</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" font-family="monospace" fill="#3b82f6">none</text>
  <text x="130" y="88" font-size="7" fill="#475569">nunca — el elemento es "invisible" al mouse</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" font-family="monospace" fill="#3b82f6">all</text>
  <text x="130" y="110" font-size="7" fill="#475569">siempre, sea visible o no</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" font-family="monospace" fill="#3b82f6">visibleFill</text>
  <text x="130" y="132" font-size="7" fill="#475569">solo el fill, si es visible</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" font-family="monospace" fill="#3b82f6">visibleStroke</text>
  <text x="130" y="154" font-size="7" fill="#475569">solo el stroke, si es visible</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" font-family="monospace" fill="#3b82f6">visible</text>
  <text x="130" y="176" font-size="7" fill="#475569">fill+stroke si visibility es visible</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" font-family="monospace" fill="#3b82f6">painted</text>
  <text x="130" y="198" font-size="7" fill="#475569">fill o stroke, aunque sean invisibles</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" font-family="monospace" fill="#3b82f6">fill</text>
  <text x="130" y="220" font-size="7" fill="#475569">solo el fill, sin importar visibilidad</text>

  <rect x="10" y="228" width="280" height="22" fill="white"/>
  <text x="20" y="242" font-size="7" font-family="monospace" fill="#3b82f6">stroke</text>
  <text x="130" y="242" font-size="7" fill="#475569">solo el stroke, sin importar visibilidad</text>

  <text x="150" y="268" text-anchor="middle" font-size="7" fill="#94a3b8">en la práctica usarás none, all y el default</text>
</svg>
```

---

## Caso 1: desactivar eventos en capas decorativas

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    overlay que roba clicks
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">Tienes un gráfico interactivo y encima</text>
  <text x="25" y="84" font-size="7" fill="#475569">una "guía visual" (grid, cursores…). Los clicks</text>
  <text x="25" y="98" font-size="7" fill="#475569">van a la guía en vez de a los datos.</text>

  <rect x="15" y="126" width="270" height="102" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">/* la solución clásica */</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">&lt;g class="grid"&gt; ... &lt;/g&gt;</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">&lt;g class="data"&gt; ... &lt;/g&gt;</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">.grid {</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#fbbf24">  pointer-events: none;</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#34d399">}</text>
</svg>
```

---

## Caso 2: área de click mayor que el elemento visible

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    hit target más grande que el punto
  </text>

  <!-- Demo visual -->
  <g transform="translate(150 80)">
    <circle r="18" fill="transparent" stroke="#94a3b8" stroke-dasharray="3 3"/>
    <circle r="4" fill="#3b82f6"/>
    <text x="30" y="0" font-size="7" fill="#475569">punto visible (4px)</text>
    <text x="30" y="12" font-size="7" fill="#94a3b8">área de click (18px)</text>
  </g>

  <rect x="15" y="120" width="270" height="122" fill="#0f172a" rx="3"/>
  <text x="25" y="136" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- punto pequeño visible --&gt;</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#60a5fa">&lt;circle r="4" fill="blue"</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#fbbf24">  pointer-events="none"/&gt;</text>

  <text x="25" y="186" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- hit target grande, invisible --&gt;</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#34d399">&lt;circle r="18" fill="transparent"</text>
  <text x="25" y="216" font-size="8" font-family="monospace" fill="#ec4899">  pointer-events="all"/&gt;</text>
  <text x="25" y="234" font-size="7" fill="#94a3b8">→ UX accesible: se pueden tocar con el dedo</text>
</svg>
```

---

## Caso 3: path con `fill="none"` que necesita responder

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    área interior sin fill
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Problema</text>
  <text x="25" y="70" font-size="7" fill="#475569">Un path cerrado con fill="none" — visualmente</text>
  <text x="25" y="84" font-size="7" fill="#475569">parece una región, pero el "interior" no existe</text>
  <text x="25" y="98" font-size="7" fill="#475569">para el mouse porque no hay fill visible.</text>

  <rect x="15" y="126" width="270" height="102" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- path cerrado pero sin fill --&gt;</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">&lt;path d="M 10 10 L 90 10 L 50 90 Z"</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">      fill="none" stroke="black"</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#fbbf24">      pointer-events="fill"/&gt;</text>
  <text x="25" y="214" font-size="7" fill="#94a3b8">→ el interior responde al click aunque no esté pintado</text>
</svg>
```
