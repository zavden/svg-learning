# 9.2 Gradiente radial `<radialGradient>`

Irradia desde un punto central hacia el exterior. Produce efectos de esfera, luz, resplandor y highlights.

---

## Estructura básica

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <radialGradient id="rg-basic" cx="50%" cy="50%" r="50%">
      <stop offset="0%"   stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#1d4ed8"/>
    </radialGradient>
  </defs>

  <circle cx="75"  cy="50" r="40" fill="url(#rg-basic)"/>
  <rect   x="130" y="10"  width="80" height="80" rx="6" fill="url(#rg-basic)"/>
  <text x="150" y="97" text-anchor="middle" font-size="7.5" fill="#64748b">
    mismo gradiente en círculo y rect — se adapta al bounding box
  </text>
</svg>
```

---

## cx, cy, r — Centro y radio

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- Centro por defecto (50%, 50%), r=50% -->
    <radialGradient id="rg-default">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#92400e"/>
    </radialGradient>

    <!-- Centro desplazado: cx=30%, cy=30% -->
    <radialGradient id="rg-offset" cx="30%" cy="30%" r="60%">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#92400e"/>
    </radialGradient>

    <!-- Radio pequeño: r=30% (gradiente ocupa menos) -->
    <radialGradient id="rg-small-r" cx="50%" cy="50%" r="30%">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#92400e"/>
    </radialGradient>
  </defs>

  <rect x="10"  y="10" width="80" height="80" rx="4" fill="url(#rg-default)"/>
  <text x="50"  y="102" text-anchor="middle" font-size="7" fill="#64748b">cx=50% cy=50%</text>
  <text x="50"  y="111" text-anchor="middle" font-size="6.5" fill="#94a3b8">r=50% (default)</text>

  <rect x="110" y="10" width="80" height="80" rx="4" fill="url(#rg-offset)"/>
  <text x="150" y="102" text-anchor="middle" font-size="7" fill="#64748b">cx=30% cy=30%</text>
  <text x="150" y="111" text-anchor="middle" font-size="6.5" fill="#94a3b8">centro desplazado</text>

  <rect x="210" y="10" width="80" height="80" rx="4" fill="url(#rg-small-r)"/>
  <text x="250" y="102" text-anchor="middle" font-size="7" fill="#64748b">r=30%</text>
  <text x="250" y="111" text-anchor="middle" font-size="6.5" fill="#94a3b8">gradiente más pequeño</text>
</svg>
```

---

## fx, fy — Punto focal: efecto de luz lateral

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <defs>
    <!-- Sin punto focal: luz central -->
    <radialGradient id="rg-center" cx="50%" cy="50%" r="50%">
      <stop offset="0%"   stop-color="white" stop-opacity="0.9"/>
      <stop offset="40%"  stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#1e3a5f"/>
    </radialGradient>

    <!-- Punto focal desplazado: luz lateral (arriba-izquierda) -->
    <radialGradient id="rg-focal" cx="50%" cy="50%" r="50%" fx="30%" fy="30%">
      <stop offset="0%"   stop-color="white" stop-opacity="0.9"/>
      <stop offset="40%"  stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#1e3a5f"/>
    </radialGradient>
  </defs>

  <!-- Esfera con luz centrada -->
  <circle cx="75" cy="60" r="50" fill="url(#rg-center)"/>
  <text x="75"  y="118" text-anchor="middle" font-size="7" fill="#94a3b8">fx=50% fy=50% (=cx,cy)</text>
  <text x="75"  y="108" text-anchor="middle" font-size="7" fill="#64748b">luz centrada</text>

  <!-- Esfera con luz lateral -->
  <circle cx="225" cy="60" r="50" fill="url(#rg-focal)"/>
  <!-- Marcar el punto focal -->
  <circle cx="210" cy="45" r="3" fill="white" opacity="0.7"/>
  <text x="225" y="118" text-anchor="middle" font-size="7" fill="#94a3b8">fx=30% fy=30%</text>
  <text x="225" y="108" text-anchor="middle" font-size="7" fill="#64748b">luz lateral (esfera 3D)</text>
</svg>
```

---

## Gradiente radial en texto

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <defs>
    <radialGradient id="text-rg" cx="50%" cy="40%" r="60%">
      <stop offset="0%"   stop-color="#fbbf24"/>
      <stop offset="60%"  stop-color="#f97316"/>
      <stop offset="100%" stop-color="#7c2d12"/>
    </radialGradient>
  </defs>

  <text x="150" y="55" text-anchor="middle" font-size="40" font-weight="900"
        fill="url(#text-rg)">FUEGO</text>
  <text x="150" y="73" text-anchor="middle" font-size="7.5" fill="#475569">
    gradiente radial en texto — fill="url(#id)"
  </text>
</svg>
```
