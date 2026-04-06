# 20.1 Tipos de animación disponibles en SVG

SVG ofrece **tres sistemas distintos** para animar: SMIL (declarativo, nativo del propio SVG), CSS (familiar para cualquier desarrollador web) y JavaScript (el más potente y dinámico). No son mutuamente excluyentes: cada uno tiene un sweet spot, y en proyectos reales se mezclan según el caso.

---

## Los tres sistemas

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SMIL · CSS · JavaScript
  </text>

  <!-- SMIL -->
  <rect x="15" y="38" width="270" height="76" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">SMIL — declarativo, nativo SVG</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;circle&gt;&lt;animate attributeName="cx" .../&gt;&lt;/circle&gt;</text>
  <text x="25" y="82" font-size="7" fill="#475569">• vive dentro del SVG como elementos XML</text>
  <text x="25" y="96" font-size="7" fill="#475569">• funciona incluso en &lt;img&gt; (único que lo hace)</text>
  <text x="25" y="110" font-size="7" fill="#475569">• ideal para morphing de paths y SVGs autocontenidos</text>

  <!-- CSS -->
  <rect x="15" y="122" width="270" height="76" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="138" font-size="9" font-weight="bold" fill="#166534">CSS — familiar para web devs</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#475569">@keyframes pulso { ... }  transition: fill 0.3s;</text>
  <text x="25" y="166" font-size="7" fill="#475569">• misma sintaxis que en HTML</text>
  <text x="25" y="180" font-size="7" fill="#475569">• respeta prefers-reduced-motion nativamente</text>
  <text x="25" y="194" font-size="7" fill="#475569">• no anima el atributo d (morphing)</text>

  <!-- JS -->
  <rect x="15" y="206" width="270" height="68" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="222" font-size="9" font-weight="bold" fill="#5b21b6">JavaScript — control total</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#475569">requestAnimationFrame, Web Animations API, GSAP</text>
  <text x="25" y="250" font-size="7" fill="#475569">• reacciona a datos, eventos, tiempo real</text>
  <text x="25" y="264" font-size="7" fill="#475569">• ideal para visualizaciones de datos y morphing avanzado</text>
</svg>
```

---

## Tabla de capacidades

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">capacidades por sistema</text>

  <!-- Header -->
  <rect x="10" y="28" width="280" height="22" fill="#e2e8f0"/>
  <text x="20" y="42" font-size="7" font-weight="bold" fill="#1e293b">Capacidad</text>
  <text x="180" y="42" font-size="7" font-weight="bold" fill="#1e293b">SMIL</text>
  <text x="215" y="42" font-size="7" font-weight="bold" fill="#1e293b">CSS</text>
  <text x="245" y="42" font-size="7" font-weight="bold" fill="#1e293b">JS</text>

  <rect x="10" y="50" width="280" height="20" fill="white"/>
  <text x="20" y="63" font-size="7" fill="#475569">Animar d (morphing)</text>
  <text x="180" y="63" font-size="7" fill="#10b981">sí</text>
  <text x="215" y="63" font-size="7" fill="#ef4444">no</text>
  <text x="245" y="63" font-size="7" fill="#10b981">sí</text>

  <rect x="10" y="70" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="83" font-size="7" fill="#475569">Funciona en &lt;img&gt;</text>
  <text x="180" y="83" font-size="7" fill="#10b981">sí</text>
  <text x="215" y="83" font-size="7" fill="#ef4444">no</text>
  <text x="245" y="83" font-size="7" fill="#ef4444">no</text>

  <rect x="10" y="90" width="280" height="20" fill="white"/>
  <text x="20" y="103" font-size="7" fill="#475569">Reaccionar a datos</text>
  <text x="180" y="103" font-size="7" fill="#ef4444">no</text>
  <text x="215" y="103" font-size="7" fill="#ef4444">no</text>
  <text x="245" y="103" font-size="7" fill="#10b981">sí</text>

  <rect x="10" y="110" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="123" font-size="7" fill="#475569">Sincronización entre animaciones</text>
  <text x="180" y="123" font-size="7" fill="#10b981">sí</text>
  <text x="215" y="123" font-size="7" fill="#f59e0b">limitada</text>
  <text x="245" y="123" font-size="7" fill="#10b981">sí</text>

  <rect x="10" y="130" width="280" height="20" fill="white"/>
  <text x="20" y="143" font-size="7" fill="#475569">Inspector DevTools</text>
  <text x="180" y="143" font-size="7" fill="#f59e0b">parcial</text>
  <text x="215" y="143" font-size="7" fill="#10b981">sí</text>
  <text x="245" y="143" font-size="7" fill="#10b981">sí</text>

  <rect x="10" y="150" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="163" font-size="7" fill="#475569">prefers-reduced-motion nativo</text>
  <text x="180" y="163" font-size="7" fill="#f59e0b">manual</text>
  <text x="215" y="163" font-size="7" fill="#10b981">sí</text>
  <text x="245" y="163" font-size="7" fill="#f59e0b">manual</text>

  <rect x="10" y="170" width="280" height="20" fill="white"/>
  <text x="20" y="183" font-size="7" fill="#475569">GPU (transform, opacity)</text>
  <text x="180" y="183" font-size="7" fill="#f59e0b">limitado</text>
  <text x="215" y="183" font-size="7" fill="#10b981">sí</text>
  <text x="245" y="183" font-size="7" fill="#10b981">sí (WAAPI)</text>

  <rect x="10" y="190" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="203" font-size="7" fill="#475569">Sin dependencias externas</text>
  <text x="180" y="203" font-size="7" fill="#10b981">sí</text>
  <text x="215" y="203" font-size="7" fill="#10b981">sí</text>
  <text x="245" y="203" font-size="7" fill="#f59e0b">depende</text>

  <text x="150" y="232" text-anchor="middle" font-size="7" fill="#94a3b8">la decisión real depende del contexto:</text>
  <text x="150" y="244" text-anchor="middle" font-size="7" fill="#94a3b8">¿inline o &lt;img&gt;? ¿hay JS disponible? ¿qué se anima?</text>
  <text x="150" y="262" text-anchor="middle" font-size="7" fill="#475569">en proyectos reales se combinan los tres</text>
</svg>
```

---

## Cuándo elegir cada uno

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    decisión rápida
  </text>

  <!-- SMIL -->
  <rect x="15" y="38" width="270" height="70" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">elige SMIL si…</text>
  <text x="25" y="70" font-size="7" fill="#475569">• el SVG va dentro de &lt;img&gt; o como standalone</text>
  <text x="25" y="84" font-size="7" fill="#475569">• necesitas morphing del atributo d</text>
  <text x="25" y="98" font-size="7" fill="#475569">• quieres animaciones autosuficientes sin CSS/JS</text>

  <!-- CSS -->
  <rect x="15" y="116" width="270" height="70" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="132" font-size="9" font-weight="bold" fill="#166534">elige CSS si…</text>
  <text x="25" y="148" font-size="7" fill="#475569">• el SVG es inline en HTML</text>
  <text x="25" y="162" font-size="7" fill="#475569">• quieres :hover y transitions sencillas</text>
  <text x="25" y="176" font-size="7" fill="#475569">• necesitas respetar prefers-reduced-motion sin código extra</text>

  <!-- JS -->
  <rect x="15" y="194" width="270" height="76" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="210" font-size="9" font-weight="bold" fill="#5b21b6">elige JavaScript si…</text>
  <text x="25" y="226" font-size="7" fill="#475569">• las animaciones dependen de datos en tiempo real</text>
  <text x="25" y="240" font-size="7" fill="#475569">• hay sincronización compleja con eventos del usuario</text>
  <text x="25" y="254" font-size="7" fill="#475569">• necesitas GSAP/Anime.js para casos avanzados (morphing)</text>
  <text x="25" y="266" font-size="7" fill="#475569">• construyes una visualización de datos interactiva</text>
</svg>
```
