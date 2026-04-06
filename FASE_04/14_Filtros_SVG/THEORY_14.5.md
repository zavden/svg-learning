# 14.5 Creación de sombras proyectadas

La sombra proyectada (*drop shadow*) es el efecto más icónico de los filtros SVG. Hay **tres formas** de crearla: manual (cadena de primitivas), atajo con `<feDropShadow>`, y la propiedad CSS `filter: drop-shadow()`.

---

## Método manual: los 4 pasos clásicos

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">construcción paso a paso</text>

  <defs>
    <!-- Paso 1: solo la silueta desenfocada -->
    <filter id="p1" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4"/>
    </filter>
    <!-- Paso 2: silueta desenfocada + desplazada -->
    <filter id="p2" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4"/>
      <feOffset dx="5" dy="6"/>
    </filter>
    <!-- Paso 3: coloreada en azul translúcido -->
    <filter id="p3" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4"/>
      <feOffset dx="5" dy="6"/>
      <feFlood flood-color="#1e40af" flood-opacity="0.5"/>
      <feComposite in2="SourceAlpha" operator="in"/>
    </filter>
    <!-- Paso 4: apilar la sombra bajo el elemento original -->
    <filter id="p4" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="4" result="b"/>
      <feOffset in="b" dx="5" dy="6" result="o"/>
      <feFlood flood-color="#1e40af" flood-opacity="0.5" result="c"/>
      <feComposite in="c" in2="o" operator="in" result="s"/>
      <feMerge>
        <feMergeNode in="s"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
  </defs>

  <!-- Paso 1 -->
  <rect x="15" y="40" width="60" height="75" fill="#f8fafc" stroke="#cbd5e1" stroke-dasharray="2,2"/>
  <circle cx="45" cy="75" r="18" fill="#3b82f6" filter="url(#p1)"/>
  <text x="45" y="130" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">1. blur</text>
  <text x="45" y="140" text-anchor="middle" font-size="6" fill="#64748b">SourceAlpha</text>

  <!-- Paso 2 -->
  <rect x="85" y="40" width="60" height="75" fill="#f8fafc" stroke="#cbd5e1" stroke-dasharray="2,2"/>
  <circle cx="115" cy="75" r="18" fill="#3b82f6" filter="url(#p2)"/>
  <text x="115" y="130" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">2. offset</text>
  <text x="115" y="140" text-anchor="middle" font-size="6" fill="#64748b">dx dy</text>

  <!-- Paso 3 -->
  <rect x="155" y="40" width="60" height="75" fill="#f8fafc" stroke="#cbd5e1" stroke-dasharray="2,2"/>
  <circle cx="185" cy="75" r="18" fill="#3b82f6" filter="url(#p3)"/>
  <text x="185" y="130" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">3. flood+comp</text>
  <text x="185" y="140" text-anchor="middle" font-size="6" fill="#64748b">colorear</text>

  <!-- Paso 4 -->
  <rect x="225" y="40" width="60" height="75" fill="#f8fafc" stroke="#cbd5e1" stroke-dasharray="2,2"/>
  <circle cx="255" cy="75" r="18" fill="#3b82f6" filter="url(#p4)"/>
  <text x="255" y="130" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e293b">4. merge</text>
  <text x="255" y="140" text-anchor="middle" font-size="6" fill="#64748b">apilar original</text>

  <!-- Flechas entre pasos -->
  <text x="80" y="80" font-size="12" fill="#94a3b8">→</text>
  <text x="150" y="80" font-size="12" fill="#94a3b8">→</text>
  <text x="220" y="80" font-size="12" fill="#94a3b8">→</text>

  <!-- Código resumen -->
  <rect x="15" y="155" width="270" height="85" fill="#f1f5f9" stroke="#cbd5e1" rx="4"/>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#1e293b">&lt;filter id="sombra"&gt;</text>
  <text x="25" y="181" font-size="7" font-family="monospace" fill="#0369a1">  &lt;feGaussianBlur in="SourceAlpha" stdDeviation="4" result="b"/&gt;</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#0369a1">  &lt;feOffset in="b" dx="5" dy="6" result="o"/&gt;</text>
  <text x="25" y="203" font-size="7" font-family="monospace" fill="#0369a1">  &lt;feFlood flood-color="#1e40af" flood-opacity="0.5" result="c"/&gt;</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#0369a1">  &lt;feComposite in="c" in2="o" operator="in" result="s"/&gt;</text>
  <text x="25" y="225" font-size="7" font-family="monospace" fill="#0369a1">  &lt;feMerge&gt;&lt;feMergeNode in="s"/&gt;&lt;feMergeNode in="SourceGraphic"/&gt;&lt;/feMerge&gt;</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#1e293b">&lt;/filter&gt;</text>
</svg>
```

---

## Método rápido: `<feDropShadow>` equivalente

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">5 primitivas vs 1: mismo resultado</text>

  <defs>
    <!-- Versión manual (5 primitivas) -->
    <filter id="manual" x="-50%" y="-50%" width="200%" height="200%">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" result="b"/>
      <feOffset in="b" dx="4" dy="5" result="o"/>
      <feFlood flood-color="#000" flood-opacity="0.4" result="c"/>
      <feComposite in="c" in2="o" operator="in" result="s"/>
      <feMerge>
        <feMergeNode in="s"/>
        <feMergeNode in="SourceGraphic"/>
      </feMerge>
    </filter>
    <!-- Versión corta (1 primitiva) -->
    <filter id="corta" x="-50%" y="-50%" width="200%" height="200%">
      <feDropShadow dx="4" dy="5" stdDeviation="3" flood-color="#000" flood-opacity="0.4"/>
    </filter>
  </defs>

  <!-- Lado manual -->
  <rect x="15" y="35" width="130" height="130" fill="#fef2f2" stroke="#fca5a5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">método manual</text>
  <rect x="45" y="70" width="60" height="60" rx="6" fill="#10b981" filter="url(#manual)"/>
  <text x="80" y="148" text-anchor="middle" font-size="6" fill="#64748b">5 primitivas</text>
  <text x="80" y="158" text-anchor="middle" font-size="6" fill="#64748b">más control</text>

  <!-- Lado feDropShadow -->
  <rect x="155" y="35" width="130" height="130" fill="#dcfce7" stroke="#86efac" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">feDropShadow</text>
  <rect x="185" y="70" width="60" height="60" rx="6" fill="#10b981" filter="url(#corta)"/>
  <text x="220" y="148" text-anchor="middle" font-size="6" fill="#64748b">1 primitiva</text>
  <text x="220" y="158" text-anchor="middle" font-size="6" fill="#64748b">rápido y legible</text>

  <text x="150" y="185" text-anchor="middle" font-size="7" fill="#94a3b8">
    feDropShadow = blur(SourceAlpha) + offset + flood + composite + merge
  </text>
</svg>
```

---

## CSS `filter: drop-shadow()` — sin `<defs>`

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .ds-css-1 { filter: drop-shadow(2px 3px 2px rgba(0,0,0,0.3)); }
    .ds-css-2 { filter: drop-shadow(4px 5px 4px rgba(0,0,0,0.4)); }
    .ds-css-3 { filter: drop-shadow(6px 8px 6px rgba(236,72,153,0.6)); }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">filter: drop-shadow() en CSS</text>

  <circle cx="60" cy="75" r="25" fill="#f59e0b" class="ds-css-1"/>
  <text x="60" y="120" text-anchor="middle" font-size="7" fill="#64748b">drop-shadow</text>
  <text x="60" y="130" text-anchor="middle" font-size="6" fill="#94a3b8">2px 3px 2px</text>

  <circle cx="150" cy="75" r="25" fill="#f59e0b" class="ds-css-2"/>
  <text x="150" y="120" text-anchor="middle" font-size="7" fill="#64748b">drop-shadow</text>
  <text x="150" y="130" text-anchor="middle" font-size="6" fill="#94a3b8">4px 5px 4px</text>

  <circle cx="240" cy="75" r="25" fill="#f59e0b" class="ds-css-3"/>
  <text x="240" y="120" text-anchor="middle" font-size="7" fill="#64748b">rosa</text>
  <text x="240" y="130" text-anchor="middle" font-size="6" fill="#94a3b8">6px 8px 6px</text>

  <!-- Sintaxis -->
  <rect x="30" y="140" width="240" height="28" fill="#f1f5f9" stroke="#cbd5e1" rx="3"/>
  <text x="150" y="155" text-anchor="middle" font-size="7" font-family="monospace" fill="#1e293b">
    filter: drop-shadow(offsetX offsetY blur color);
  </text>
  <text x="150" y="164" text-anchor="middle" font-size="6" fill="#94a3b8">
    misma sintaxis que box-shadow pero sigue el contorno real
  </text>
</svg>
```

---

## `drop-shadow` vs `box-shadow` — la diferencia clave

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <style>
    .con-drop { filter: drop-shadow(3px 4px 3px rgba(0,0,0,0.4)); }
  </style>

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">drop-shadow sigue el contorno real</text>

  <!-- Lado BOX-SHADOW simulado: sombra rectangular -->
  <rect x="15" y="35" width="130" height="150" fill="#fef2f2" stroke="#fca5a5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">box-shadow</text>
  <text x="80" y="63" text-anchor="middle" font-size="6" fill="#991b1b">rectángulo del bbox</text>

  <!-- Sombra rectangular simulada detrás de la estrella -->
  <rect x="40" y="85" width="80" height="60" fill="#000" opacity="0.3" transform="translate(3, 4)"/>
  <polygon points="80,80 93,115 128,115 98,135 108,170 80,150 52,170 62,135 32,115 67,115"
           fill="#3b82f6"/>
  <text x="80" y="195" text-anchor="middle" font-size="7" fill="#ef4444">sombra del bounding box</text>

  <!-- Lado DROP-SHADOW: sombra que sigue la forma -->
  <rect x="155" y="35" width="130" height="150" fill="#dcfce7" stroke="#86efac" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">drop-shadow</text>
  <text x="220" y="63" text-anchor="middle" font-size="6" fill="#166534">contorno del elemento</text>

  <polygon points="220,80 233,115 268,115 238,135 248,170 220,150 192,170 202,135 172,115 207,115"
           fill="#3b82f6" class="con-drop"/>
  <text x="220" y="195" text-anchor="middle" font-size="7" fill="#166534">sigue los picos de la estrella</text>

  <text x="150" y="212" text-anchor="middle" font-size="7" fill="#94a3b8">
    SVG siempre quiere drop-shadow: respeta las transparencias
  </text>
</svg>
```

---

## Sombra múltiple (apilada)

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">capas de sombra para profundidad</text>

  <defs>
    <!-- Sombra simple -->
    <filter id="s1" x="-50%" y="-50%" width="200%" height="200%">
      <feDropShadow dx="2" dy="3" stdDeviation="2" flood-opacity="0.35"/>
    </filter>
    <!-- Dos sombras apiladas: una cercana dura + otra lejana suave -->
    <filter id="s2" x="-80%" y="-80%" width="260%" height="260%">
      <feDropShadow dx="1" dy="1" stdDeviation="1" flood-opacity="0.4"/>
      <feDropShadow dx="4" dy="8" stdDeviation="6" flood-opacity="0.25"/>
    </filter>
    <!-- Tres capas: elevación pronunciada -->
    <filter id="s3" x="-80%" y="-80%" width="260%" height="260%">
      <feDropShadow dx="0" dy="1" stdDeviation="1" flood-opacity="0.3"/>
      <feDropShadow dx="0" dy="3" stdDeviation="3" flood-opacity="0.25"/>
      <feDropShadow dx="0" dy="8" stdDeviation="10" flood-opacity="0.2"/>
    </filter>
  </defs>

  <rect x="20"  y="45" width="65" height="65" rx="8" fill="#f8fafc" stroke="#e2e8f0" filter="url(#s1)"/>
  <text x="52"  y="130" text-anchor="middle" font-size="7" fill="#64748b">1 capa</text>
  <text x="52"  y="140" text-anchor="middle" font-size="6" fill="#94a3b8">plano</text>

  <rect x="115" y="45" width="65" height="65" rx="8" fill="#f8fafc" stroke="#e2e8f0" filter="url(#s2)"/>
  <text x="148" y="130" text-anchor="middle" font-size="7" fill="#64748b">2 capas</text>
  <text x="148" y="140" text-anchor="middle" font-size="6" fill="#94a3b8">elevado</text>

  <rect x="210" y="45" width="65" height="65" rx="8" fill="#f8fafc" stroke="#e2e8f0" filter="url(#s3)"/>
  <text x="243" y="130" text-anchor="middle" font-size="7" fill="#64748b">3 capas</text>
  <text x="243" y="140" text-anchor="middle" font-size="6" fill="#94a3b8">flotando</text>

  <text x="150" y="160" text-anchor="middle" font-size="7" fill="#94a3b8">
    varios feDropShadow seguidos se acumulan automáticamente
  </text>
</svg>
```

---

## Tabla comparativa: tres métodos

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    cuándo usar cada método
  </text>

  <!-- Método manual -->
  <rect x="10" y="34" width="280" height="60" fill="#fef2f2" stroke="#fca5a5" rx="4"/>
  <text x="20" y="50" font-size="9" font-weight="bold" fill="#991b1b">1. Manual (5 primitivas)</text>
  <text x="20" y="63" font-size="7" fill="#1e293b">+ máximo control: puedes inyectar pasos intermedios</text>
  <text x="20" y="74" font-size="7" fill="#1e293b">+ compatible con navegadores muy antiguos</text>
  <text x="20" y="85" font-size="7" fill="#64748b">- verboso, más fácil de equivocarse</text>

  <!-- feDropShadow -->
  <rect x="10" y="100" width="280" height="60" fill="#dcfce7" stroke="#86efac" rx="4"/>
  <text x="20" y="116" font-size="9" font-weight="bold" fill="#166534">2. feDropShadow (SVG)</text>
  <text x="20" y="129" font-size="7" fill="#1e293b">+ sintaxis simple, parámetros directos</text>
  <text x="20" y="140" font-size="7" fill="#1e293b">+ permite varias sombras apiladas en un solo filtro</text>
  <text x="20" y="151" font-size="7" fill="#64748b">- requiere SVG 2 (navegadores modernos)</text>

  <!-- CSS drop-shadow -->
  <rect x="10" y="166" width="280" height="60" fill="#dbeafe" stroke="#93c5fd" rx="4"/>
  <text x="20" y="182" font-size="9" font-weight="bold" fill="#1e40af">3. CSS filter: drop-shadow()</text>
  <text x="20" y="195" font-size="7" fill="#1e293b">+ no requiere &lt;defs&gt;, funciona con cualquier elemento</text>
  <text x="20" y="206" font-size="7" fill="#1e293b">+ animable con transiciones CSS</text>
  <text x="20" y="217" font-size="7" fill="#64748b">- menos control sobre el color y composición</text>
</svg>
```
