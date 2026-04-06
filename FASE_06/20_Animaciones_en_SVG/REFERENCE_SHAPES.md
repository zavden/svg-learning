# REFERENCE — Animaciones en SVG

Cheat sheet para copiar/pegar: los tres sistemas de animación, recetas clásicas (spinner, progreso, line drawing, morph) y el mini-árbol de decisión.

---

## Los tres sistemas de un vistazo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SMIL vs CSS vs JS
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Capacidad</text>
  <text x="120" y="46" font-size="7" font-weight="bold" fill="#1e293b">SMIL</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">CSS</text>
  <text x="220" y="46" font-size="7" font-weight="bold" fill="#1e293b">JS</text>

  <rect x="10" y="52" width="280" height="18" fill="white"/>
  <text x="20" y="64" font-size="7" fill="#475569">animar atributo d</text>
  <text x="120" y="64" font-size="7" fill="#10b981">✓</text>
  <text x="170" y="64" font-size="7" fill="#ef4444">✗</text>
  <text x="220" y="64" font-size="7" fill="#10b981">✓</text>

  <rect x="10" y="70" width="280" height="18" fill="#f8fafc"/>
  <text x="20" y="82" font-size="7" fill="#475569">funciona en SVG &lt;img&gt;</text>
  <text x="120" y="82" font-size="7" fill="#10b981">✓</text>
  <text x="170" y="82" font-size="7" fill="#f59e0b">parcial</text>
  <text x="220" y="82" font-size="7" fill="#ef4444">✗</text>

  <rect x="10" y="88" width="280" height="18" fill="white"/>
  <text x="20" y="100" font-size="7" fill="#475569">prefers-reduced-motion</text>
  <text x="120" y="100" font-size="7" fill="#ef4444">✗</text>
  <text x="170" y="100" font-size="7" fill="#10b981">nativo</text>
  <text x="220" y="100" font-size="7" fill="#f59e0b">manual</text>

  <rect x="10" y="106" width="280" height="18" fill="#f8fafc"/>
  <text x="20" y="118" font-size="7" fill="#475569">DevTools timeline</text>
  <text x="120" y="118" font-size="7" fill="#ef4444">✗</text>
  <text x="170" y="118" font-size="7" fill="#10b981">✓</text>
  <text x="220" y="118" font-size="7" fill="#10b981">✓</text>

  <rect x="10" y="124" width="280" height="18" fill="white"/>
  <text x="20" y="136" font-size="7" fill="#475569">datos dinámicos</text>
  <text x="120" y="136" font-size="7" fill="#ef4444">✗</text>
  <text x="170" y="136" font-size="7" fill="#ef4444">✗</text>
  <text x="220" y="136" font-size="7" fill="#10b981">✓</text>

  <rect x="10" y="142" width="280" height="18" fill="#f8fafc"/>
  <text x="20" y="154" font-size="7" fill="#475569">sincronización declarativa</text>
  <text x="120" y="154" font-size="7" fill="#10b981">✓</text>
  <text x="170" y="154" font-size="7" fill="#f59e0b">limitada</text>
  <text x="220" y="154" font-size="7" fill="#10b981">✓</text>

  <rect x="10" y="160" width="280" height="18" fill="white"/>
  <text x="20" y="172" font-size="7" fill="#475569">familiaridad web</text>
  <text x="120" y="172" font-size="7" fill="#f59e0b">baja</text>
  <text x="170" y="172" font-size="7" fill="#10b981">alta</text>
  <text x="220" y="172" font-size="7" fill="#10b981">alta</text>

  <rect x="15" y="188" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="204" font-size="8" font-weight="bold" fill="#94a3b8">regla por defecto</text>
  <text x="25" y="220" font-size="7" fill="#60a5fa">• CSS para el 90% de UIs web</text>
  <text x="25" y="234" font-size="7" fill="#34d399">• SMIL si el SVG vive en &lt;img&gt; o necesitas morphing de d</text>
  <text x="25" y="248" font-size="7" fill="#f472b6">• JS cuando hay datos o sincronía compleja</text>
  <text x="25" y="262" font-size="7" fill="#fbbf24">mezclar los tres es lo normal en proyectos reales</text>
</svg>
```

---

## Snippet — spinner CSS

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    spinner 1-line
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">/* CSS */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">.spinner {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  animation: rotar 1s linear infinite;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  transform-origin: center;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#fbbf24">@keyframes rotar {</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#ec4899">  to { transform: rotate(360deg); }</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#fbbf24">}</text>

  <text x="25" y="174" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- HTML --&gt;</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg class="spinner" viewBox="0 0 40 40"&gt;</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;circle cx="20" cy="20" r="16" .../&gt;&lt;/svg&gt;</text>
</svg>
```

---

## Snippet — line drawing

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    stroke-dasharray + dashoffset
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">/* CSS */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">.draw {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  stroke-dasharray: var(--L);</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  stroke-dashoffset: var(--L);</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">  animation: draw 2s ease forwards;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <rect x="15" y="146" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#94a3b8">// JS — longitud exacta del path</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#e2e8f0">const p = document.querySelector('.draw');</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#e2e8f0">const L = p.getTotalLength();</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#e2e8f0">p.style.setProperty('--L', L);</text>
  <text x="25" y="222" font-size="7" fill="#fbbf24">→ así no tienes que adivinar el número</text>
</svg>
```

---

## Snippet — progreso circular

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    progreso circular — fórmula
  </text>

  <rect x="15" y="38" width="270" height="180" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">// fórmula</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#34d399">circ   = 2 × π × r</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">offset = circ × (1 − pct/100)</text>

  <text x="25" y="108" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- markup --&gt;</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle r="40" fill="none"</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#e2e8f0">        stroke="#10b981" stroke-width="6"</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#fbbf24">        stroke-dasharray="251"</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#fbbf24">        stroke-dashoffset="63"</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#e2e8f0">        transform="rotate(-90)"/&gt;</text>

  <text x="25" y="200" font-size="7" fill="#94a3b8">r=40 → circ ≈ 251</text>
  <text x="25" y="212" font-size="7" fill="#94a3b8">75% → offset = 251×0.25 ≈ 63</text>
</svg>
```

---

## Snippet — SMIL `<animate>` clásico

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    animate / animateTransform
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- color --&gt;</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;rect fill="blue"&gt;</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  &lt;animate attributeName="fill"</text>
  <text x="25" y="94" font-size="8" font-family="monospace" fill="#34d399">    values="blue;red;blue" dur="2s"</text>
  <text x="25" y="106" font-size="8" font-family="monospace" fill="#34d399">    repeatCount="indefinite"/&gt;</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/rect&gt;</text>

  <rect x="15" y="128" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="144" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- rotar sobre su centro --&gt;</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;g transform="translate(100 100)"&gt;</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;rect x="-20" y="-20" width="40" height="40"&gt;</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#34d399">    &lt;animateTransform attributeName="transform"</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#34d399">      type="rotate" from="0" to="360" dur="2s"</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#34d399">      repeatCount="indefinite"/&gt;</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#94a3b8">→ el g con translate sitúa el centro</text>
</svg>
```

---

## Snippet — accesibilidad

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    prefers-reduced-motion
  </text>

  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">/* desactivar globalmente */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">@media (prefers-reduced-motion: reduce) {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  *, *::before, *::after {</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#fbbf24">    animation-duration: 0.01ms !important;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#fbbf24">    animation-iteration-count: 1 !important;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#fbbf24">    transition-duration: 0.01ms !important;</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#34d399">  }</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="174" font-size="7" fill="#94a3b8">versión nuclear: todas las animaciones se anulan</text>
</svg>
```

---

## Árbol de decisión rápido

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿qué sistema elijo?
  </text>

  <rect x="15" y="38" width="270" height="20" fill="#e2e8f0"/>
  <text x="150" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    ¿el SVG está inline o en archivo suelto?
  </text>

  <rect x="15" y="64" width="130" height="40" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="80" y="80" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">inline en HTML</text>
  <text x="80" y="94" text-anchor="middle" font-size="7" fill="#475569">→ empieza por CSS</text>

  <rect x="155" y="64" width="130" height="40" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="220" y="80" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">en &lt;img&gt; o solo</text>
  <text x="220" y="94" text-anchor="middle" font-size="7" fill="#475569">→ SMIL</text>

  <rect x="15" y="114" width="270" height="20" fill="#e2e8f0"/>
  <text x="150" y="128" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    ¿necesitas morphear el atributo d?
  </text>

  <rect x="15" y="140" width="270" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="150" y="154" text-anchor="middle" font-size="7" font-weight="bold" fill="#92400e">sí → SMIL o librería JS (Flubber, GSAP)</text>
  <text x="150" y="166" text-anchor="middle" font-size="7" fill="#475569">CSS no interpola paths</text>

  <rect x="15" y="182" width="270" height="20" fill="#e2e8f0"/>
  <text x="150" y="196" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    ¿los datos cambian o hay timing complejo?
  </text>

  <rect x="15" y="208" width="270" height="40" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="150" y="224" text-anchor="middle" font-size="7" font-weight="bold" fill="#5b21b6">sí → JavaScript</text>
  <text x="150" y="238" text-anchor="middle" font-size="7" fill="#475569">D3, GSAP, Motion, Web Animations API</text>
</svg>
```

---

## Checklist

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    antes de darlo por terminado
  </text>

  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" fill="#34d399">☐ elegí el sistema por contexto, no por costumbre</text>
  <text x="25" y="76" font-size="8" fill="#34d399">☐ las rotaciones tienen su centro bien definido</text>
  <text x="25" y="94" font-size="8" fill="#34d399">☐ fill="freeze" en SMIL si el estado final debe quedar</text>
  <text x="25" y="112" font-size="8" fill="#34d399">☐ @media prefers-reduced-motion cubre los loops</text>
  <text x="25" y="130" font-size="8" fill="#34d399">☐ probé el SVG en el contexto final (inline / &lt;img&gt;)</text>
  <text x="25" y="148" font-size="8" fill="#34d399">☐ verifiqué en Chrome + Firefox + Safari</text>
  <text x="25" y="166" font-size="8" fill="#34d399">☐ no hay animación sin propósito comunicativo</text>
</svg>
```
