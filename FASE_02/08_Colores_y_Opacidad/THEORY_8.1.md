# 8.1 Formatos de color soportados

SVG acepta exactamente los mismos formatos de color que CSS. Todos los formatos son equivalentes — son distintas maneras de expresar el mismo valor.

---

## Nombres de color

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="10"  y="15" width="50" height="50" fill="tomato"/>
  <text x="35"  y="78" text-anchor="middle" font-size="7.5" fill="#64748b">tomato</text>

  <rect x="70"  y="15" width="50" height="50" fill="steelblue"/>
  <text x="95"  y="78" text-anchor="middle" font-size="7.5" fill="#64748b">steelblue</text>

  <rect x="130" y="15" width="50" height="50" fill="mediumseagreen"/>
  <text x="155" y="78" text-anchor="middle" font-size="7.5" fill="#64748b">mediumseagreen</text>

  <rect x="190" y="15" width="50" height="50" fill="goldenrod"/>
  <text x="215" y="78" text-anchor="middle" font-size="7.5" fill="#64748b">goldenrod</text>

  <rect x="250" y="15" width="50" height="50" fill="orchid"/>
  <text x="275" y="78" text-anchor="middle" font-size="7.5" fill="#64748b">orchid</text>

  <text x="150" y="95" text-anchor="middle" font-size="7" fill="#94a3b8">140+ nombres CSS disponibles — insensibles a mayúsculas</text>
</svg>
```

---

## Notación hexadecimal

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- #RRGGBB -->
  <rect x="10" y="15" width="60" height="40" fill="#e74c3c"/>
  <text x="40" y="65" text-anchor="middle" font-size="7" fill="#64748b">#e74c3c</text>
  <text x="40" y="74" text-anchor="middle" font-size="6.5" fill="#94a3b8">largo (6 dígitos)</text>

  <!-- #RGB corto -->
  <rect x="85" y="15" width="60" height="40" fill="#e43"/>
  <text x="115" y="65" text-anchor="middle" font-size="7" fill="#64748b">#e43</text>
  <text x="115" y="74" text-anchor="middle" font-size="6.5" fill="#94a3b8">corto → #ee4433</text>

  <!-- #RRGGBBAA con alfa -->
  <rect x="160" y="10" width="60" height="50" fill="#1e293b"/>
  <rect x="160" y="15" width="60" height="40" fill="#3b82f680"/>
  <text x="190" y="65" text-anchor="middle" font-size="7" fill="#64748b">#3b82f680</text>
  <text x="190" y="74" text-anchor="middle" font-size="6.5" fill="#94a3b8">80 hex ≈ 50% alfa</text>

  <!-- negro y blanco -->
  <rect x="240" y="15" width="25" height="40" fill="#000000"/>
  <rect x="270" y="15" width="25" height="40" fill="#ffffff" stroke="#e2e8f0" stroke-width="1"/>
  <text x="252" y="65" text-anchor="middle" font-size="7" fill="#64748b">#000</text>
  <text x="282" y="65" text-anchor="middle" font-size="7" fill="#64748b">#fff</text>

  <text x="150" y="95" text-anchor="middle" font-size="7.5" fill="#64748b">
    #RRGGBB — cada par controla un canal (00=0, FF=255)
  </text>
  <text x="150" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">
    #RGB (corto) solo funciona si cada par es idéntico
  </text>
</svg>
```

---

## rgb() y rgba()

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- rgb clásico -->
  <rect x="10" y="15" width="65" height="50" fill="rgb(59, 130, 246)"/>
  <text x="42" y="76" text-anchor="middle" font-size="7" fill="#64748b">rgb(59,130,246)</text>

  <!-- rgba con opacidad -->
  <rect x="5" y="10" width="65" height="55" fill="#1e293b"/>
  <rect x="90" y="15" width="65" height="50" fill="rgba(59, 130, 246, 0.4)"/>
  <text x="122" y="76" text-anchor="middle" font-size="7" fill="#64748b">rgba(59,130,246,.4)</text>

  <!-- rgb moderno sin comas -->
  <rect x="170" y="15" width="65" height="50" fill="rgb(59 130 246 / 70%)"/>
  <text x="202" y="76" text-anchor="middle" font-size="7" fill="#64748b">rgb(59 130 246 / 70%)</text>
  <text x="202" y="86" text-anchor="middle" font-size="6.5" fill="#94a3b8">CSS moderno</text>

  <text x="150" y="101" text-anchor="middle" font-size="7.5" fill="#64748b">
    rgb(R, G, B) — valores 0–255. rgba añade canal alfa 0–1
  </text>
</svg>
```

---

## hsl() y hsla()

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">hsl — Hue · Saturation · Lightness</text>

  <!-- Variaciones de Hue (mismo S y L) -->
  <rect x="10"  y="28" width="35" height="30" fill="hsl(0,   80%, 55%)"/>
  <rect x="50"  y="28" width="35" height="30" fill="hsl(30,  80%, 55%)"/>
  <rect x="90"  y="28" width="35" height="30" fill="hsl(60,  80%, 55%)"/>
  <rect x="130" y="28" width="35" height="30" fill="hsl(120, 80%, 55%)"/>
  <rect x="170" y="28" width="35" height="30" fill="hsl(180, 80%, 55%)"/>
  <rect x="210" y="28" width="35" height="30" fill="hsl(240, 80%, 55%)"/>
  <rect x="250" y="28" width="35" height="30" fill="hsl(300, 80%, 55%)"/>
  <text x="150" y="70" text-anchor="middle" font-size="7" fill="#64748b">Hue: 0°→360° (giro en la rueda de color)</text>

  <!-- Variaciones de Lightness (mismo H y S) -->
  <rect x="10"  y="78" width="35" height="30" fill="hsl(220, 70%, 20%)"/>
  <rect x="50"  y="78" width="35" height="30" fill="hsl(220, 70%, 35%)"/>
  <rect x="90"  y="78" width="35" height="30" fill="hsl(220, 70%, 50%)"/>
  <rect x="130" y="78" width="35" height="30" fill="hsl(220, 70%, 65%)"/>
  <rect x="170" y="78" width="35" height="30" fill="hsl(220, 70%, 80%)"/>
  <rect x="210" y="78" width="35" height="30" fill="hsl(220, 70%, 92%)"/>
  <text x="150" y="120" text-anchor="middle" font-size="7" fill="#64748b">Lightness: 0%=negro → 50%=color → 100%=blanco</text>
  <text x="150" y="130" text-anchor="middle" font-size="6.5" fill="#94a3b8">HSL facilita crear paletas: variar solo L para oscuro/claro</text>
</svg>
```

---

## Colores especiales: `black`, `white`, `transparent`, `none`

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#94a3b8;display:block; font-family:sans-serif;">

  <!-- black — fill por defecto -->
  <rect x="10" y="15" width="55" height="45" fill="black"/>
  <text x="37" y="72" text-anchor="middle" font-size="7" fill="white">black</text>
  <text x="37" y="81" text-anchor="middle" font-size="6.5" fill="#e2e8f0">fill default</text>

  <!-- white -->
  <rect x="80" y="15" width="55" height="45" fill="white"/>
  <text x="107" y="72" text-anchor="middle" font-size="7" fill="white">white</text>

  <!-- transparent — invisible pero interactivo -->
  <rect x="150" y="15" width="55" height="45" fill="transparent"/>
  <text x="177" y="72" text-anchor="middle" font-size="7" fill="white">transparent</text>
  <text x="177" y="81" text-anchor="middle" font-size="6.5" fill="#e2e8f0">alfa=0, clickeable</text>

  <!-- none — sin pintura -->
  <rect x="220" y="15" width="55" height="45" fill="none" stroke="white" stroke-width="2"/>
  <text x="247" y="72" text-anchor="middle" font-size="7" fill="white">none</text>
  <text x="247" y="81" text-anchor="middle" font-size="6.5" fill="#e2e8f0">sin pintura</text>
</svg>
```
