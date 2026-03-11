# 2.1 El elemento raíz `<svg>`

El elemento `<svg>` es el contenedor que define el viewport, el sistema de coordenadas y el contexto de renderizado. Sin él, no hay SVG.

---

## Atributos fundamentales

El elemento `<svg>` concentra los atributos más importantes de todo el documento.

```svg
<svg width="340" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Línea 1: apertura + namespace -->
  <text x="16" y="28" font-size="9" fill="#e06c75">&lt;svg</text>

  <!-- xmlns -->
  <text x="16" y="46" font-size="9" fill="#d19a66">  xmlns</text>
  <text x="68" y="46" font-size="9" fill="#56b6c2">=</text>
  <text x="76" y="46" font-size="9" fill="#98c379">"http://www.w3.org/2000/svg"</text>
  <text x="270" y="46" font-size="8" fill="#5c6370" font-style="italic">  ← namespace</text>

  <!-- width / height -->
  <text x="16" y="63" font-size="9" fill="#d19a66">  width</text>
  <text x="56" y="63" font-size="9" fill="#56b6c2">=</text>
  <text x="64" y="63" font-size="9" fill="#98c379">"400"</text>
  <text x="16" y="79" font-size="9" fill="#d19a66">  height</text>
  <text x="62" y="79" font-size="9" fill="#56b6c2">=</text>
  <text x="70" y="79" font-size="9" fill="#98c379">"200"</text>
  <text x="270" y="71" font-size="8" fill="#5c6370" font-style="italic">← viewport</text>

  <!-- viewBox -->
  <text x="16" y="96" font-size="9" fill="#d19a66">  viewBox</text>
  <text x="70" y="96" font-size="9" fill="#56b6c2">=</text>
  <text x="78" y="96" font-size="9" fill="#98c379">"0 0 100 50"</text>
  <text x="270" y="96" font-size="8" fill="#5c6370" font-style="italic">← coordenadas</text>

  <!-- preserveAspectRatio -->
  <text x="16" y="112" font-size="9" fill="#d19a66">  preserveAspectRatio</text>
  <text x="156" y="112" font-size="9" fill="#56b6c2">=</text>
  <text x="164" y="112" font-size="9" fill="#98c379">"xMidYMid meet"</text>
  <text x="270" y="112" font-size="8" fill="#5c6370" font-style="italic">← ajuste</text>

  <!-- cierre -->
  <text x="16" y="128" font-size="9" fill="#56b6c2">&gt;</text>

  <!-- contenido -->
  <text x="32" y="144" font-size="9" fill="#5c6370" font-style="italic">  &lt;!-- elementos hijos --&gt;</text>

  <text x="16" y="158" font-size="9" fill="#e06c75">&lt;/svg&gt;</text>
</svg>
```

---

## width y height definen el viewport

El viewport es el área física que ocupa el SVG en pantalla — independientemente de las coordenadas internas.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- SVG pequeño: 80×60 -->
  <g transform="translate(20, 10)">
    <rect width="80" height="60" fill="none" stroke="#3b82f6" stroke-width="2"/>
    <rect width="80" height="60" fill="#eff6ff" opacity="0.5"/>
    <circle cx="40" cy="28" r="18" fill="#3b82f6"/>
    <text x="40" y="78" text-anchor="middle" font-size="9" fill="#64748b">
      width="80" height="60"
    </text>
    <text x="40" y="90" text-anchor="middle" font-size="8" fill="#94a3b8">
      viewport pequeño
    </text>
  </g>

  <!-- SVG grande: 160×80 -->
  <g transform="translate(120, 10)">
    <rect width="160" height="80" fill="none" stroke="#10b981" stroke-width="2"/>
    <rect width="160" height="80" fill="#f0fdf4" opacity="0.5"/>
    <circle cx="80" cy="38" r="30" fill="#10b981"/>
    <text x="80" y="100" text-anchor="middle" font-size="9" fill="#64748b">
      width="160" height="80"
    </text>
    <text x="80" y="112" text-anchor="middle" font-size="8" fill="#94a3b8">
      viewport grande
    </text>
  </g>
</svg>
```

---

## SVG como archivo externo vs inline en HTML

La diferencia más práctica: si el SVG está inline en el HTML, comparte CSS y JavaScript con la página.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Columna: archivo .svg externo -->
  <rect x="10" y="10" width="130" height="140" rx="8"
        fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>
  <text x="75" y="30" text-anchor="middle"
        font-size="10" fill="#1e293b" font-weight="600">archivo.svg</text>

  <rect x="20" y="40" width="110" height="20" rx="4" fill="#1e293b"/>
  <text x="75" y="55" text-anchor="middle" font-size="8" fill="#98c379" font-family="monospace">
    xmlns obligatorio
  </text>

  <text x="75" y="80" text-anchor="middle" font-size="8" fill="#64748b">✓ URL directa</text>
  <text x="75" y="94" text-anchor="middle" font-size="8" fill="#64748b">✓ Cacheable</text>
  <text x="75" y="108" text-anchor="middle" font-size="8" fill="#ef4444">✗ Sin acceso JS</text>
  <text x="75" y="122" text-anchor="middle" font-size="8" fill="#ef4444">✗ CSS externo</text>
  <text x="75" y="136" text-anchor="middle" font-size="8" fill="#64748b">≈ HTTP request</text>

  <!-- Columna: inline -->
  <rect x="160" y="10" width="130" height="140" rx="8"
        fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>
  <text x="225" y="30" text-anchor="middle"
        font-size="10" fill="#1e293b" font-weight="600">inline en HTML</text>

  <rect x="170" y="40" width="110" height="20" rx="4" fill="#1e293b"/>
  <text x="225" y="55" text-anchor="middle" font-size="8" fill="#5c6370" font-family="monospace">
    xmlns opcional
  </text>

  <text x="225" y="80" text-anchor="middle" font-size="8" fill="#10b981">✓ CSS compartido</text>
  <text x="225" y="94" text-anchor="middle" font-size="8" fill="#10b981">✓ JS directo</text>
  <text x="225" y="108" text-anchor="middle" font-size="8" fill="#10b981">✓ Sin request HTTP</text>
  <text x="225" y="122" text-anchor="middle" font-size="8" fill="#ef4444">✗ No cacheable</text>
  <text x="225" y="136" text-anchor="middle" font-size="8" fill="#64748b">≈ Parte del DOM</text>
</svg>
```

---

## Patrón SVG como ícono con currentColor

El patrón más común en sistemas de diseño: el SVG hereda el color del texto del elemento padre via `currentColor`.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="20" text-anchor="middle"
        font-size="10" fill="#64748b">
    fill="currentColor" hereda el color CSS del padre
  </text>

  <!-- Ícono que usa currentColor — el color cambia según el contexto CSS -->
  <!-- Simulamos 3 contextos de color distinto -->

  <!-- Color azul (como si el padre tuviese color: #3b82f6) -->
  <g transform="translate(50, 65)">
    <circle r="26" fill="#eff6ff" stroke="#dbeafe" stroke-width="1"/>
    <!-- ícono check con fill=currentColor simulado en azul -->
    <circle r="18" fill="#3b82f6"/>
    <polyline points="-8,2 -2,8 10,-6" fill="none"
              stroke="white" stroke-width="3" stroke-linecap="round"/>
    <text y="40" text-anchor="middle" font-size="8" fill="#3b82f6">color: blue</text>
  </g>

  <!-- Color verde -->
  <g transform="translate(150, 65)">
    <circle r="26" fill="#f0fdf4" stroke="#dcfce7" stroke-width="1"/>
    <circle r="18" fill="#10b981"/>
    <polyline points="-8,2 -2,8 10,-6" fill="none"
              stroke="white" stroke-width="3" stroke-linecap="round"/>
    <text y="40" text-anchor="middle" font-size="8" fill="#10b981">color: green</text>
  </g>

  <!-- Color rojo -->
  <g transform="translate(250, 65)">
    <circle r="26" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
    <circle r="18" fill="#ef4444"/>
    <polyline points="-8,2 -2,8 10,-6" fill="none"
              stroke="white" stroke-width="3" stroke-linecap="round"/>
    <text y="40" text-anchor="middle" font-size="8" fill="#ef4444">color: red</text>
  </g>

  <text x="150" y="118" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    El mismo SVG — 3 colores via CSS sin tocar el SVG
  </text>
</svg>
```

---

## SVG anidado

Un `<svg>` puede contener otro `<svg>`. Cada uno tiene su propio viewport y sistema de coordenadas, posicionado con `x` e `y`.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- SVG padre: 300×160 -->
  <rect width="300" height="160" fill="#f1f5f9" rx="8"/>
  <text x="150" y="18" text-anchor="middle"
        font-size="9" fill="#64748b">SVG padre — 300×160</text>

  <!-- SVG hijo 1: en la esquina superior izquierda -->
  <svg x="20" y="30" width="100" height="60"
       viewBox="0 0 100 60">
    <rect width="100" height="60" rx="6"
          fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <circle cx="50" cy="28" r="18" fill="#3b82f6" opacity="0.8"/>
    <text x="50" y="54" text-anchor="middle"
          font-size="8" fill="#1e40af">&lt;svg x="20" y="30"&gt;</text>
  </svg>

  <!-- SVG hijo 2: a la derecha -->
  <svg x="170" y="30" width="110" height="80"
       viewBox="0 0 110 80">
    <rect width="110" height="80" rx="6"
          fill="#f0fdf4" stroke="#10b981" stroke-width="1.5"/>
    <rect x="15" y="15" width="80" height="50" rx="4" fill="#10b981" opacity="0.8"/>
    <text x="55" y="72" text-anchor="middle"
          font-size="8" fill="#166534">&lt;svg x="170" y="30"&gt;</text>
  </svg>

  <text x="150" y="148" text-anchor="middle"
        font-size="8" fill="#64748b">
    Cada &lt;svg&gt; hijo tiene su propio espacio de coordenadas
  </text>
</svg>
```
