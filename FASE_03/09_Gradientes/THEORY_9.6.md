# 9.6 Transformaciones de gradientes

`gradientTransform` aplica una transformación SVG al gradiente completo: rotar, escalar, sesgar. Especialmente útil para obtener ángulos arbitrarios sin calcular coordenadas trigonométricas.

---

## `gradientTransform="rotate(...)"` — el caso más habitual

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Base horizontal, rotado 0° (referencia) -->
    <linearGradient id="rot-0" gradientTransform="rotate(0, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- 45° -->
    <linearGradient id="rot-45" gradientTransform="rotate(45, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- 90° (equivale a vertical) -->
    <linearGradient id="rot-90" gradientTransform="rotate(90, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- 135° -->
    <linearGradient id="rot-135" gradientTransform="rotate(135, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <rect x="10"  y="15" width="60" height="60" rx="4" fill="url(#rot-0)"/>
  <text x="40"  y="87" text-anchor="middle" font-size="7.5" fill="#64748b">0°</text>

  <rect x="85"  y="15" width="60" height="60" rx="4" fill="url(#rot-45)"/>
  <text x="115" y="87" text-anchor="middle" font-size="7.5" fill="#64748b">45°</text>

  <rect x="160" y="15" width="60" height="60" rx="4" fill="url(#rot-90)"/>
  <text x="190" y="87" text-anchor="middle" font-size="7.5" fill="#64748b">90°</text>

  <rect x="235" y="15" width="60" height="60" rx="4" fill="url(#rot-135)"/>
  <text x="265" y="87" text-anchor="middle" font-size="7.5" fill="#64748b">135°</text>

  <text x="150" y="103" text-anchor="middle" font-size="7.5" fill="#64748b">
    gradientTransform="rotate(ángulo, 0.5, 0.5)"
  </text>
  <text x="150" y="115" text-anchor="middle" font-size="7" fill="#94a3b8">
    (0.5, 0.5) = centro del elemento en objectBoundingBox
  </text>
  <text x="150" y="127" text-anchor="middle" font-size="7" fill="#94a3b8">
    sin (0.5, 0.5) la rotación sería desde (0,0) = esquina
  </text>
</svg>
```

---

## Centro de rotación — con y sin (0.5, 0.5)

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- rotate sin centro: rota desde (0,0) = esquina superior izquierda -->
    <linearGradient id="rot-corner" gradientTransform="rotate(45)">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>

    <!-- rotate con centro (0.5, 0.5): rota desde el centro -->
    <linearGradient id="rot-center" gradientTransform="rotate(45, 0.5, 0.5)">
      <stop offset="0%"   stop-color="#ef4444"/>
      <stop offset="100%" stop-color="#fbbf24"/>
    </linearGradient>
  </defs>

  <rect x="20"  y="10" width="100" height="70" rx="4" fill="url(#rot-corner)"/>
  <text x="70"  y="90" text-anchor="middle" font-size="7" fill="#ef4444">rotate(45)</text>
  <text x="70"  y="99" text-anchor="middle" font-size="6.5" fill="#94a3b8">rota desde esquina</text>

  <rect x="180" y="10" width="100" height="70" rx="4" fill="url(#rot-center)"/>
  <text x="230" y="90" text-anchor="middle" font-size="7" fill="#10b981">rotate(45, 0.5, 0.5) ✓</text>
  <text x="230" y="99" text-anchor="middle" font-size="6.5" fill="#94a3b8">rota desde el centro</text>
</svg>
```

---

## Otras transformaciones: scale y skewX

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <linearGradient id="base-grad">
      <stop offset="0%"   stop-color="#10b981"/>
      <stop offset="50%"  stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>

    <!-- scale: el gradiente ocupa solo la mitad del elemento -->
    <linearGradient id="scaled" gradientTransform="scale(0.5, 1) translate(0.5, 0)"
                    href="#base-grad"/>

    <!-- skewX: inclina el gradiente horizontalmente -->
    <linearGradient id="skewed" gradientTransform="skewX(20)"
                    href="#base-grad"/>
  </defs>

  <rect x="10"  y="15" width="85" height="65" rx="4" fill="url(#base-grad)"/>
  <text x="52"  y="92" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <rect x="110" y="15" width="85" height="65" rx="4" fill="url(#scaled)"/>
  <text x="152" y="92" text-anchor="middle" font-size="7" fill="#64748b">scale(0.5,1)</text>

  <rect x="210" y="15" width="85" height="65" rx="4" fill="url(#skewed)"/>
  <text x="252" y="92" text-anchor="middle" font-size="7" fill="#64748b">skewX(20)</text>
</svg>
```
