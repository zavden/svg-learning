# 2.4 El elemento `<defs>`

`<defs>` es el almacén de recursos reutilizables. Todo lo que se defina dentro de él **no se renderiza** hasta que otro elemento lo referencia por `id`.

---

## La regla fundamental: define aquí, usa allá

Los elementos dentro de `<defs>` son invisibles por sí solos. Solo aparecen cuando son referenciados desde fuera mediante `url(#id)` o `href="#id"`.

```svg
<svg width="300" height="160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Definido aquí — invisible por sí mismo -->
    <linearGradient id="azulVerde" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#10b981"/>
    </linearGradient>
  </defs>

  <!-- Sin referenciar: el gradiente no se ve en ningún lado -->
  <text x="150" y="24" text-anchor="middle"
        font-size="10" fill="#64748b">
    El gradiente vive en &lt;defs&gt; — invisible
  </text>

  <!-- Al referenciar: aparece -->
  <rect x="30" y="35" width="240" height="50" rx="8"
        fill="url(#azulVerde)"/>
  <text x="150" y="65" text-anchor="middle"
        font-size="11" fill="white" font-weight="600">
    fill="url(#azulVerde)" → aparece
  </text>

  <!-- El mismo gradiente, reutilizado -->
  <circle cx="80"  cy="120" r="25" fill="url(#azulVerde)"/>
  <circle cx="150" cy="120" r="25" fill="url(#azulVerde)"/>
  <circle cx="220" cy="120" r="25" fill="url(#azulVerde)"/>
  <text x="150" y="152" text-anchor="middle"
        font-size="8" fill="#64748b">
    Mismo gradiente, reutilizado 3 veces — definido una sola vez
  </text>
</svg>
```

---

## Qué elementos van dentro de `<defs>`

Los recursos más comunes que se definen en `<defs>`:

```svg
<svg width="340" height="220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="grad1" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
    <radialGradient id="grad2" cx="50%" cy="50%" r="50%">
      <stop offset="0%"   stop-color="#fde68a"/>
      <stop offset="100%" stop-color="#f59e0b"/>
    </radialGradient>
    <pattern id="pat1" x="0" y="0" width="10" height="10"
             patternUnits="userSpaceOnUse">
      <rect width="10" height="10" fill="#eff6ff"/>
      <circle cx="5" cy="5" r="2" fill="#93c5fd"/>
    </pattern>
    <filter id="blur1">
      <feGaussianBlur stdDeviation="2"/>
    </filter>
    <clipPath id="clip1">
      <circle cx="0" cy="0" r="25"/>
    </clipPath>
  </defs>

  <!-- linearGradient -->
  <g transform="translate(30, 30)">
    <rect width="60" height="40" rx="6" fill="url(#grad1)"/>
    <text y="56" text-anchor="middle" x="30" font-size="8" fill="#64748b">linearGradient</text>
  </g>

  <!-- radialGradient -->
  <g transform="translate(130, 30)">
    <circle cx="30" cy="20" r="20" fill="url(#grad2)"/>
    <text y="56" text-anchor="middle" x="30" font-size="8" fill="#64748b">radialGradient</text>
  </g>

  <!-- pattern -->
  <g transform="translate(220, 30)">
    <rect width="80" height="40" rx="6" fill="url(#pat1)"
          stroke="#93c5fd" stroke-width="1"/>
    <text y="56" text-anchor="middle" x="40" font-size="8" fill="#64748b">pattern</text>
  </g>

  <!-- filter -->
  <g transform="translate(30, 100)">
    <circle cx="30" cy="20" r="20" fill="#3b82f6" filter="url(#blur1)"/>
    <text y="56" text-anchor="middle" x="30" font-size="8" fill="#64748b">filter (blur)</text>
  </g>

  <!-- clipPath -->
  <g transform="translate(130, 100)">
    <g clip-path="url(#clip1)" transform="translate(30, 20)">
      <rect x="-35" y="-35" width="70" height="70" fill="#10b981"/>
      <rect x="0"   y="-35" width="35" height="70" fill="#3b82f6" opacity="0.7"/>
    </g>
    <text y="56" text-anchor="middle" x="30" font-size="8" fill="#64748b">clipPath</text>
  </g>

  <!-- symbol (reutilizable) -->
  <defs>
    <symbol id="star" viewBox="0 0 20 20">
      <polygon points="10,2 12.4,7.5 18.5,8 14,12.2 15.3,18.1 10,15 4.7,18.1 6,12.2 1.5,8 7.6,7.5"
               fill="#f59e0b"/>
    </symbol>
  </defs>
  <g transform="translate(230, 100)">
    <use href="#star" x="5"  y="5" width="20" height="20"/>
    <use href="#star" x="25" y="5" width="20" height="20"/>
    <use href="#star" x="45" y="5" width="20" height="20"/>
    <text y="56" text-anchor="middle" x="35" font-size="8" fill="#64748b">symbol + use</text>
  </g>

  <!-- Nota -->
  <text x="170" y="210" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    Todos definidos en &lt;defs&gt; — reutilizables por id
  </text>
</svg>
```

---

## La relación definir → referenciar

El mecanismo completo: asignar `id` dentro de `<defs>` y usar `url(#id)` o `href="#id"` fuera.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Paso 1: Definir -->
  <text x="150" y="22" text-anchor="middle" font-size="8" fill="#6a9955" font-style="italic">
    Paso 1 — Definir (invisible)
  </text>
  <rect x="10" y="28" width="280" height="32" rx="4" fill="#0d2137"/>
  <text x="16" y="44" font-size="9" fill="#e06c75">&lt;defs&gt;</text>
  <text x="16" y="56" font-size="9" fill="#e06c75">  &lt;linearGradient</text>
  <text x="130" y="56" font-size="9" fill="#d19a66">id</text>
  <text x="143" y="56" font-size="9" fill="#56b6c2">=</text>
  <text x="151" y="56" font-size="9" fill="#98c379">"gradAzul"</text>
  <text x="210" y="56" font-size="9" fill="#56b6c2">&gt;</text>
  <text x="220" y="56" font-size="9" fill="#5c6370" font-style="italic">...&lt;/defs&gt;</text>

  <!-- Flecha -->
  <line x1="150" y1="62" x2="150" y2="76" stroke="#475569" stroke-width="1.5" marker-end="url(#arr)"/>
  <text x="150" y="74" text-anchor="middle" font-size="8" fill="#475569">↓</text>

  <!-- Paso 2: Referenciar -->
  <text x="150" y="88" text-anchor="middle" font-size="8" fill="#6a9955" font-style="italic">
    Paso 2 — Referenciar (visible)
  </text>
  <rect x="10" y="94" width="280" height="24" rx="4" fill="#0d2137"/>
  <text x="16" y="110" font-size="9" fill="#e06c75">&lt;rect</text>
  <text x="52" y="110" font-size="9" fill="#d19a66">fill</text>
  <text x="72" y="110" font-size="9" fill="#56b6c2">=</text>
  <text x="80" y="110" font-size="9" fill="#98c379">"url(#gradAzul)"</text>
  <text x="176" y="110" font-size="9" fill="#5c6370" font-style="italic">
    ...
  </text>
  <text x="186" y="110" font-size="9" fill="#56b6c2">/&gt;</text>

  <!-- Gradiente aplicado -->
  <defs>
    <linearGradient id="gradDemo" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>
  <rect x="10" y="118" width="280" height="8" rx="2" fill="url(#gradDemo)"/>
</svg>
```

---

## ¿Pueden ir fuera de `<defs>`?

Técnicamente sí, pero la convención es siempre usar `<defs>`.

```svg
<svg width="300" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Fuera de defs: funciona pero no es convención -->
  <linearGradient id="gradFuera" x1="0" y1="0" x2="1" y2="0">
    <stop offset="0%" stop-color="#f59e0b"/>
    <stop offset="100%" stop-color="#ef4444"/>
  </linearGradient>

  <text x="150" y="22" text-anchor="middle"
        font-size="9" fill="#64748b">
    Ambos funcionan — la convención prefiere &lt;defs&gt;
  </text>

  <!-- Dentro de defs (recomendado) -->
  <defs>
    <linearGradient id="gradDentro" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#10b981"/>
    </linearGradient>
  </defs>

  <rect x="20"  y="35" width="120" height="40" rx="6" fill="url(#gradDentro)"/>
  <text x="80"  y="83" text-anchor="middle" font-size="8" fill="#10b981">dentro de &lt;defs&gt; ✓</text>
  <text x="80"  y="95" text-anchor="middle" font-size="8" fill="#94a3b8">más legible</text>

  <rect x="160" y="35" width="120" height="40" rx="6" fill="url(#gradFuera)"/>
  <text x="220" y="83" text-anchor="middle" font-size="8" fill="#f59e0b">fuera de &lt;defs&gt;</text>
  <text x="220" y="95" text-anchor="middle" font-size="8" fill="#94a3b8">funciona igual</text>
</svg>
```
