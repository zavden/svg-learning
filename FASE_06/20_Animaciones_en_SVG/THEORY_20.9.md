# 20.9 Estado actual de SMIL y elección entre los tres sistemas

SMIL pasó por un susto en 2015 cuando Chrome anunció su deprecación. Un año después la decisión se revirtió y hoy SMIL está soportado en todos los navegadores modernos. Pero ese episodio dejó una mala fama que aún confunde: la pregunta correcta no es "¿se puede usar SMIL?" sino "¿cuándo conviene cada uno?".

---

## La historia (resumida)

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SMIL en 2015 → 2016 → hoy
  </text>

  <!-- Línea de tiempo -->
  <line x1="30" y1="80" x2="270" y2="80" stroke="#1e293b" stroke-width="1.5"/>
  <circle cx="60" cy="80" r="4" fill="#ef4444"/>
  <circle cx="150" cy="80" r="4" fill="#10b981"/>
  <circle cx="240" cy="80" r="4" fill="#3b82f6"/>

  <text x="60" y="100" text-anchor="middle" font-size="7" fill="#475569">2015</text>
  <text x="60" y="112" text-anchor="middle" font-size="6" fill="#94a3b8">"deprecation"</text>
  <text x="60" y="62" text-anchor="middle" font-size="6" fill="#991b1b">Chrome avisa</text>

  <text x="150" y="100" text-anchor="middle" font-size="7" fill="#475569">2016</text>
  <text x="150" y="112" text-anchor="middle" font-size="6" fill="#94a3b8">"unintention"</text>
  <text x="150" y="62" text-anchor="middle" font-size="6" fill="#166534">Chrome revierte</text>

  <text x="240" y="100" text-anchor="middle" font-size="7" fill="#475569">hoy</text>
  <text x="240" y="112" text-anchor="middle" font-size="6" fill="#94a3b8">"soportado"</text>
  <text x="240" y="62" text-anchor="middle" font-size="6" fill="#1e40af">universal</text>

  <rect x="15" y="138" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="8" font-weight="bold" fill="#94a3b8">en 2026</text>
  <text x="25" y="170" font-size="7" fill="#e2e8f0">• SMIL funciona en Chrome, Firefox, Safari, Edge</text>
  <text x="25" y="184" font-size="7" fill="#e2e8f0">• su mayor virtud sigue intacta: funciona en &lt;img&gt;</text>
  <text x="25" y="198" font-size="7" fill="#e2e8f0">• librerías de íconos animados (Lord Icon, etc.) lo usan</text>
  <text x="25" y="212" font-size="7" fill="#fbbf24">la fama de "deprecated" sigue circulando, pero es falsa</text>
  <text x="25" y="222" font-size="7" fill="#94a3b8">caniuse.com lo confirma como "stable"</text>
</svg>
```

---

## Decisión: SMIL vs CSS vs JavaScript

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el sistema correcto según contexto
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Necesidad</text>
  <text x="200" y="46" font-size="7" font-weight="bold" fill="#1e293b">Sistema</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">SVG dentro de &lt;img&gt; y debe animarse</text>
  <text x="200" y="66" font-size="7" font-family="monospace" fill="#3b82f6">SMIL</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Morphing del atributo d</text>
  <text x="200" y="88" font-size="7" font-family="monospace" fill="#3b82f6">SMIL o JS</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">SVG inline + hover sencillo</text>
  <text x="200" y="110" font-size="7" font-family="monospace" fill="#10b981">CSS transition</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Loop infinito (spinner, pulso)</text>
  <text x="200" y="132" font-size="7" font-family="monospace" fill="#10b981">CSS @keyframes</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">prefers-reduced-motion nativo</text>
  <text x="200" y="154" font-size="7" font-family="monospace" fill="#10b981">CSS</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Datos en tiempo real</text>
  <text x="200" y="176" font-size="7" font-family="monospace" fill="#8b5cf6">JavaScript</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">Sincronización compleja</text>
  <text x="200" y="198" font-size="7" font-family="monospace" fill="#8b5cf6">JS / GSAP</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" fill="#475569">SVG standalone autocontenido</text>
  <text x="200" y="220" font-size="7" font-family="monospace" fill="#3b82f6">SMIL</text>

  <rect x="10" y="228" width="280" height="22" fill="white"/>
  <text x="20" y="242" font-size="7" fill="#475569">Coreografía declarativa</text>
  <text x="200" y="242" font-size="7" font-family="monospace" fill="#3b82f6">SMIL</text>

  <text x="150" y="270" text-anchor="middle" font-size="7" fill="#94a3b8">en proyectos reales se mezclan los tres según el caso</text>
</svg>
```

---

## La regla práctica

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    decisión por defecto
  </text>

  <rect x="15" y="38" width="270" height="64" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Por defecto: CSS</text>
  <text x="25" y="70" font-size="7" fill="#475569">la mayoría de UIs web tienen SVG inline en HTML</text>
  <text x="25" y="84" font-size="7" fill="#475569">CSS gana en familiaridad, DevTools y accesibilidad</text>
  <text x="25" y="96" font-size="7" fill="#475569">cubre 90% de los casos: hover, spinners, fades</text>

  <rect x="15" y="110" width="270" height="58" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="126" font-size="9" font-weight="bold" fill="#1e40af">Excepción 1: SMIL</text>
  <text x="25" y="142" font-size="7" fill="#475569">cuando el SVG vive como archivo standalone o &lt;img&gt;,</text>
  <text x="25" y="156" font-size="7" fill="#475569">o cuando hay morphing del atributo d</text>

  <rect x="15" y="176" width="270" height="58" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="192" font-size="9" font-weight="bold" fill="#5b21b6">Excepción 2: JavaScript</text>
  <text x="25" y="208" font-size="7" fill="#475569">cuando los datos cambian, hay timing complejo,</text>
  <text x="25" y="222" font-size="7" fill="#475569">o usas una librería como GSAP / Anime.js / Motion One</text>
</svg>
```

---

## Mezclar los tres en un mismo proyecto

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    no son mutuamente excluyentes
  </text>

  <rect x="15" y="38" width="270" height="194" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#94a3b8">ejemplo: dashboard de un producto</text>

  <text x="25" y="74" font-size="7" fill="#60a5fa">• íconos hover de la navegación</text>
  <text x="25" y="86" font-size="7" fill="#94a3b8">  → CSS transition</text>

  <text x="25" y="104" font-size="7" fill="#34d399">• spinner mientras carga</text>
  <text x="25" y="116" font-size="7" fill="#94a3b8">  → CSS @keyframes con prefers-reduced-motion</text>

  <text x="25" y="134" font-size="7" fill="#fbbf24">• logo animado en email HTML</text>
  <text x="25" y="146" font-size="7" fill="#94a3b8">  → SMIL (solo así funciona embebido en mail)</text>

  <text x="25" y="164" font-size="7" fill="#ec4899">• gráfico de barras que actualiza con datos en vivo</text>
  <text x="25" y="176" font-size="7" fill="#94a3b8">  → JavaScript (D3.js, Web Animations API…)</text>

  <text x="25" y="196" font-size="7" fill="#a78bfa">• morph del ícono de menú a "X" al abrir</text>
  <text x="25" y="208" font-size="7" fill="#94a3b8">  → SMIL o JS (CSS no anima d)</text>

  <text x="25" y="226" font-size="7" fill="#fbbf24">elige por caso, no por dogma</text>
</svg>
```
