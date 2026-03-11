# 2.5 Orden de renderizado — el modelo del pintor

SVG dibuja sus elementos como un pintor: de abajo hacia arriba. El **primer elemento** del código queda al fondo; el **último** queda al frente.

---

## El modelo del pintor

Cada elemento se pinta encima de los anteriores. El orden del código **es** el orden visual.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b">Orden en el código = orden visual</text>

  <!-- 1. Fondo (primero en el código → al fondo) -->
  <rect x="40" y="30" width="220" height="110" rx="10"
        fill="#1e40af"/>

  <!-- 2. Encima del fondo -->
  <rect x="70" y="50" width="160" height="80" rx="8"
        fill="#3b82f6"/>

  <!-- 3. Encima de los dos -->
  <circle cx="150" cy="90" r="30" fill="#60a5fa"/>

  <!-- 4. Al frente (último en el código) -->
  <text x="150" y="95" text-anchor="middle"
        font-size="13" fill="#0f172a" font-weight="700">
    4 — frente
  </text>

  <!-- Numeración de capas -->
  <text x="60"  y="82" font-size="8" fill="#93c5fd">1 — fondo</text>
  <text x="80"  y="64" font-size="8" fill="#bfdbfe">2 — encima</text>
  <text x="192" y="68" font-size="8" fill="#eff6ff">3 — más</text>
</svg>
```

---

## No existe z-index en SVG 1.1

En HTML/CSS, `z-index` permite reordenar elementos sin cambiar el HTML. En SVG 1.1, la única forma de controlar la superposición es el **orden del código**.

```svg
<svg width="300" height="170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b">
    Para cambiar quién está encima: reordenar en el código
  </text>

  <!-- Caso A: círculo encima del cuadrado -->
  <g transform="translate(20, 30)">
    <text x="60" y="14" text-anchor="middle"
          font-size="9" fill="#64748b">código:</text>
    <text x="60" y="26" text-anchor="middle"
          font-size="8" fill="#64748b" font-family="monospace">1.rect 2.circle</text>
    <!-- rect primero → detrás -->
    <rect x="15" y="35" width="60" height="60" rx="6" fill="#3b82f6"/>
    <!-- circle después → encima -->
    <circle cx="60" cy="65" r="25" fill="#f59e0b"/>
    <text x="60" y="115" text-anchor="middle"
          font-size="8" fill="#64748b">círculo encima</text>
  </g>

  <!-- Caso B: cuadrado encima del círculo -->
  <g transform="translate(160, 30)">
    <text x="60" y="14" text-anchor="middle"
          font-size="9" fill="#64748b">código:</text>
    <text x="60" y="26" text-anchor="middle"
          font-size="8" fill="#64748b" font-family="monospace">1.circle 2.rect</text>
    <!-- circle primero → detrás -->
    <circle cx="60" cy="65" r="25" fill="#f59e0b"/>
    <!-- rect después → encima -->
    <rect x="15" y="35" width="60" height="60" rx="6" fill="#3b82f6"/>
    <text x="60" y="115" text-anchor="middle"
          font-size="8" fill="#64748b">rect encima</text>
  </g>

  <!-- Flecha de cambio -->
  <line x1="130" y1="75" x2="160" y2="75"
        stroke="#94a3b8" stroke-width="1.5"
        marker-end="url(#a)"/>
  <text x="145" y="70" text-anchor="middle" font-size="8" fill="#94a3b8">swap</text>

  <text x="150" y="158" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    Mismo código — solo cambia el orden de los elementos
  </text>
</svg>
```

---

## Los grupos se renderizan como unidad

Un `<g>` (grupo) se dibuja completo antes de pasar al siguiente elemento hermano. El grupo ocupa una "capa" en el modelo del pintor.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b">
    El grupo se renderiza como una unidad
  </text>

  <!-- Grupo 1 (al fondo) — azul -->
  <g id="grupo-azul">
    <rect x="20"  y="30" width="80" height="60" rx="6" fill="#1d4ed8"/>
    <rect x="60"  y="50" width="80" height="60" rx="6" fill="#2563eb" opacity="0.9"/>
    <rect x="100" y="70" width="80" height="60" rx="6" fill="#3b82f6" opacity="0.8"/>
    <text x="90" y="72" text-anchor="middle"
          font-size="8" fill="white">grupo azul (1°)</text>
  </g>

  <!-- Grupo 2 (al frente) — naranja -->
  <g id="grupo-naranja">
    <rect x="80"  y="60" width="70" height="50" rx="6" fill="#c2410c"/>
    <rect x="120" y="80" width="70" height="50" rx="6" fill="#ea580c" opacity="0.9"/>
    <text x="155" y="75" text-anchor="middle"
          font-size="8" fill="white">grupo naranja (2°)</text>
  </g>

  <text x="150" y="148" text-anchor="middle"
        font-size="8" fill="#64748b">
    Todo el grupo azul se dibuja antes que cualquier elemento del naranja
  </text>
</svg>
```

---

## Truco: mover al frente con JavaScript

En SVG dinámico, reordenar elementos en el DOM es la forma estándar de traer algo al frente.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="150" y="22" text-anchor="middle"
        font-size="8" fill="#6a9955" font-style="italic">
    // Mover al frente (pasar al último hijo)
  </text>
  <text x="16" y="40" font-size="9" fill="#e06c75">el.</text>
  <text x="36" y="40" font-size="9" fill="#61afef">parentNode</text>
  <text x="102" y="40" font-size="9" fill="#abb2bf">.</text>
  <text x="108" y="40" font-size="9" fill="#61afef">appendChild</text>
  <text x="178" y="40" font-size="9" fill="#abb2bf">(</text>
  <text x="185" y="40" font-size="9" fill="#e06c75">el</text>
  <text x="197" y="40" font-size="9" fill="#abb2bf">);</text>

  <text x="150" y="62" text-anchor="middle"
        font-size="8" fill="#6a9955" font-style="italic">
    // Mover al fondo (antes del primer hijo)
  </text>
  <text x="16" y="80" font-size="9" fill="#e06c75">el.</text>
  <text x="36" y="80" font-size="9" fill="#61afef">parentNode</text>
  <text x="102" y="80" font-size="9" fill="#abb2bf">.</text>
  <text x="108" y="80" font-size="9" fill="#61afef">insertBefore</text>
  <text x="176" y="80" font-size="9" fill="#abb2bf">(</text>
  <text x="183" y="80" font-size="9" fill="#e06c75">el</text>
  <text x="195" y="80" font-size="9" fill="#abb2bf">,</text>
  <text x="203" y="80" font-size="9" fill="#e06c75">el.</text>
  <text x="222" y="80" font-size="9" fill="#61afef">parentNode</text>
  <text x="288" y="80" font-size="9" fill="#abb2bf">.</text>
  <text x="16" y="96" font-size="9" fill="#61afef">    firstChild</text>
  <text x="92" y="96" font-size="9" fill="#abb2bf">);</text>

  <text x="150" y="112" text-anchor="middle"
        font-size="8" fill="#475569">
    No hay z-index en SVG 1.1 → se reordena el DOM
  </text>
</svg>
```
