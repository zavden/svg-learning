# 9.5 Atributo `spreadMethod`

Controla qué ocurre más allá de los límites del gradiente cuando este no cubre todo el área del elemento.

---

## Los tres valores

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Gradiente pequeño: solo del 25% al 75% del ancho -->
    <!-- pad: extiende el color del borde -->
    <linearGradient id="sm-pad" x1="25%" y1="0%" x2="75%" y2="0%" spreadMethod="pad">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- reflect: espejo en las repeticiones -->
    <linearGradient id="sm-reflect" x1="25%" y1="0%" x2="75%" y2="0%" spreadMethod="reflect">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- repeat: repite desde el inicio -->
    <linearGradient id="sm-repeat" x1="25%" y1="0%" x2="75%" y2="0%" spreadMethod="repeat">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">spreadMethod — gradiente del 25% al 75%</text>

  <rect x="20" y="30" width="260" height="26" rx="4" fill="url(#sm-pad)"/>
  <text x="150" y="67" text-anchor="middle" font-size="7.5" fill="#64748b">pad (default) — extiende el color del borde</text>

  <rect x="20" y="74" width="260" height="26" rx="4" fill="url(#sm-reflect)"/>
  <text x="150" y="111" text-anchor="middle" font-size="7.5" fill="#64748b">reflect — espejo en cada repetición (sin saltos)</text>

  <rect x="20" y="118" width="260" height="26" rx="4" fill="url(#sm-repeat)"/>
  <text x="150" y="140" text-anchor="middle" font-size="7.5" fill="#64748b">repeat — repite desde el inicio (salto en la unión)</text>
</svg>
```

---

## spreadMethod en gradiente radial

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Radio pequeño: solo r=20% del bounding box -->
    <radialGradient id="rg-pad" cx="50%" cy="50%" r="20%" spreadMethod="pad">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#3b82f6"/>
    </radialGradient>

    <radialGradient id="rg-reflect" cx="50%" cy="50%" r="20%" spreadMethod="reflect">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#3b82f6"/>
    </radialGradient>

    <radialGradient id="rg-repeat" cx="50%" cy="50%" r="20%" spreadMethod="repeat">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#3b82f6"/>
    </radialGradient>
  </defs>

  <rect x="10"  y="10" width="80" height="80" rx="4" fill="url(#rg-pad)"/>
  <text x="50"  y="100" text-anchor="middle" font-size="7" fill="#64748b">pad</text>

  <rect x="110" y="10" width="80" height="80" rx="4" fill="url(#rg-reflect)"/>
  <text x="150" y="100" text-anchor="middle" font-size="7" fill="#64748b">reflect</text>

  <rect x="210" y="10" width="80" height="80" rx="4" fill="url(#rg-repeat)"/>
  <text x="250" y="100" text-anchor="middle" font-size="7" fill="#64748b">repeat</text>

  <text x="150" y="112" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    radialGradient con r=20% — los tres spreadMethod
  </text>
</svg>
```

---

## Efecto visual de `reflect` vs `repeat`

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- reflect: A→B B→A A→B (sin saltos) -->
    <linearGradient id="reflect-demo" x1="0%" y1="0%" x2="20%" y2="0%" spreadMethod="reflect">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>

    <!-- repeat: A→B A→B A→B (salto en unión) -->
    <linearGradient id="repeat-demo" x1="0%" y1="0%" x2="20%" y2="0%" spreadMethod="repeat">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>
  </defs>

  <rect x="10"  y="12" width="280" height="30" rx="4" fill="url(#reflect-demo)"/>
  <text x="150" y="52" text-anchor="middle" font-size="7.5" fill="#64748b">
    reflect: A→B → B→A → A→B (sin saltos, como olas)
  </text>

  <rect x="10"  y="58" width="280" height="30" rx="4" fill="url(#repeat-demo)"/>
  <text x="150" y="98" text-anchor="middle" font-size="7.5" fill="#64748b">
    repeat: A→B | A→B | A→B (salto abrupto en unión)
  </text>
</svg>
```
