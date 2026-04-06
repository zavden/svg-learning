# 19.5 Selectores CSS en SVG

**Todos** los selectores CSS funcionan dentro de SVG: tipo, clase, id, atributo, relación, pseudo-clases interactivas y estructurales. La única frontera real es el shadow DOM de `<use>`, donde los selectores **no penetran**.

---

## Selectores básicos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    selector de tipo, clase, id
  </text>

  <rect x="15" y="38" width="270" height="76" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* tipo */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">circle { fill: steelblue; }</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#94a3b8">/* clase */</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">.activo { fill: #e74c3c; }</text>

  <rect x="15" y="122" width="270" height="60" fill="#0f172a" rx="3"/>
  <text x="25" y="138" font-size="7" font-family="monospace" fill="#94a3b8">/* id */</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#fbbf24">#fondo { fill: #f5f5f5; }</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#94a3b8">/* combinados */</text>

  <rect x="15" y="190" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="206" font-size="7" font-family="monospace" fill="#94a3b8">/* atributo */</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#60a5fa">[fill="red"] { stroke: darkred; }</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#34d399">[data-estado="on"] { opacity: 1; }</text>
  <text x="25" y="252" font-size="8" font-family="monospace" fill="#fbbf24">circle[r="50"] { fill: gold; }</text>
</svg>
```

---

## Selectores de relación

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    relación entre elementos
  </text>

  <rect x="15" y="38" width="270" height="86" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* hijo directo */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">g &gt; circle { fill: blue; }</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#94a3b8">/* descendiente */</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">.grupo circle { fill: blue; }</text>
  <text x="25" y="114" font-size="7" font-family="monospace" fill="#94a3b8">    cualquier nivel de anidamiento</text>

  <rect x="15" y="132" width="270" height="76" fill="#0f172a" rx="3"/>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#94a3b8">/* hermano inmediato */</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#fbbf24">circle + text { font-size: 10px; }</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#94a3b8">/* hermano general */</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#ec4899">circle ~ text { fill: red; }</text>

  <text x="150" y="226" text-anchor="middle" font-size="7" fill="#94a3b8">funcionan exactamente igual que en HTML</text>
</svg>
```

---

## Pseudo-clases interactivas

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    :hover, :focus, :active funcionan
  </text>

  <rect x="15" y="38" width="270" height="118" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* hover sobre el círculo, no sobre todo el SVG */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">circle:hover {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  fill: #e74c3c;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  cursor: pointer;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#fbbf24">circle:active { fill: #c0392b; }</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#ec4899">.btn:focus { outline: 2px solid blue; }</text>

  <rect x="15" y="164" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="180" font-size="9" font-weight="bold" fill="#166534">Importante</text>
  <text x="25" y="196" font-size="7" fill="#475569">• :hover funciona en cada path/circle/rect, no solo en el &lt;svg&gt;</text>
  <text x="25" y="210" font-size="7" fill="#475569">• cada elemento es independientemente "interactivo"</text>
  <text x="25" y="224" font-size="7" fill="#475569">• :focus solo aplica si el elemento es enfocable (tabindex="0")</text>
  <text x="25" y="238" font-size="7" fill="#475569">• :focus-visible respeta navegación por teclado vs ratón</text>
  <text x="25" y="252" font-size="7" fill="#166534">→ permite UIs interactivas sin JS para casos simples</text>
</svg>
```

---

## Pseudo-clases estructurales

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    nth-child, first-of-type, :not()
  </text>

  <!-- Demo visual: nth-child(even) -->
  <rect x="20" y="40" width="40" height="20" fill="white" stroke="#1e293b"/>
  <rect x="60" y="40" width="40" height="20" fill="#f0f0f0" stroke="#1e293b"/>
  <rect x="100" y="40" width="40" height="20" fill="white" stroke="#1e293b"/>
  <rect x="140" y="40" width="40" height="20" fill="#f0f0f0" stroke="#1e293b"/>
  <rect x="180" y="40" width="40" height="20" fill="white" stroke="#1e293b"/>
  <rect x="220" y="40" width="40" height="20" fill="#f0f0f0" stroke="#1e293b"/>
  <text x="150" y="78" text-anchor="middle" font-size="7" font-family="monospace" fill="#475569">rect:nth-child(even) { fill: #f0f0f0; }</text>

  <rect x="15" y="92" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="108" font-size="7" font-family="monospace" fill="#94a3b8">/* primer hijo */</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#60a5fa">g:first-child { opacity: 0.5; }</text>
  <text x="25" y="138" font-size="7" font-family="monospace" fill="#94a3b8">/* impares */</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#34d399">circle:nth-child(odd) { fill: gold; }</text>
  <text x="25" y="168" font-size="7" font-family="monospace" fill="#94a3b8">/* último */</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#fbbf24">circle:last-child { stroke: red; }</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#ec4899">path:not(.excluido) { fill: steelblue; }</text>
</svg>
```

---

## La frontera de `<use>` (shadow DOM)

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los selectores NO entran en &lt;use&gt;
  </text>

  <!-- Mal -->
  <rect x="15" y="38" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ no funciona</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;use href="#icono"/&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">/* CSS */</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">use circle { fill: red; }  /* IGNORADO */</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">el contenido clonado vive en shadow DOM</text>

  <!-- Bien -->
  <rect x="15" y="126" width="270" height="142" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#166534">✓ alternativas</text>
  <text x="25" y="158" font-size="7" fill="#475569">1) currentColor (heredado)</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">   &lt;circle fill="currentColor"/&gt;</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#475569">   .icono { color: red; }</text>

  <text x="25" y="196" font-size="7" fill="#475569">2) variables CSS heredadas</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#475569">   &lt;circle fill="var(--c, blue)"/&gt;</text>
  <text x="25" y="218" font-size="7" font-family="monospace" fill="#475569">   .uno { --c: red; }</text>

  <text x="25" y="234" font-size="7" fill="#475569">3) atributos en el propio &lt;use&gt;</text>
  <text x="25" y="246" font-size="7" font-family="monospace" fill="#475569">   &lt;use href="#icono" fill="red"/&gt;</text>
  <text x="25" y="260" font-size="7" fill="#166534">→ ver sección 12 sobre &lt;symbol&gt; y &lt;use&gt;</text>
</svg>
```
