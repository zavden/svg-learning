# 11.5 `skewX(angle)` y `skewY(angle)`

Inclina el sistema de coordenadas en un eje. Produce efectos de perspectiva, paralelogramos y sombras en perspectiva.

---

## skewX: inclinación horizontal

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">skewX — ángulos crecientes</text>

  <!-- 0° (referencia) -->
  <rect x="10" y="28" width="40" height="50" rx="2" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="30" y="89" text-anchor="middle" font-size="7" fill="#64748b">0°</text>

  <!-- skewX(15) -->
  <rect x="70" y="28" width="40" height="50" rx="2" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"
        transform="skewX(15)"/>
  <text x="85" y="89" text-anchor="middle" font-size="7" fill="#3b82f6">15°</text>

  <!-- skewX(30) -->
  <rect x="145" y="28" width="40" height="50" rx="2" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"
        transform="skewX(30)"/>
  <text x="155" y="89" text-anchor="middle" font-size="7" fill="#3b82f6">30°</text>

  <!-- skewX(-20) — dirección contraria -->
  <rect x="220" y="28" width="40" height="50" rx="2" fill="#dcfce7" stroke="#10b981" stroke-width="1"
        transform="skewX(-20)"/>
  <text x="230" y="89" text-anchor="middle" font-size="7" fill="#10b981">-20°</text>

  <text x="150" y="103" text-anchor="middle" font-size="7" fill="#94a3b8">
    los puntos superiores se desplazan en X según su coordenada Y
  </text>
</svg>
```

---

## skewY: inclinación vertical

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">skewY — ángulos crecientes</text>

  <!-- 0° (referencia) -->
  <rect x="20" y="30" width="50" height="40" rx="2" fill="#fef9c3" stroke="#f59e0b" stroke-width="1"/>
  <text x="45" y="81" text-anchor="middle" font-size="7" fill="#64748b">0°</text>

  <!-- skewY(15) -->
  <rect x="90" y="30" width="50" height="40" rx="2" fill="#fef9c3" stroke="#f59e0b" stroke-width="1"
        transform="skewY(15)"/>
  <text x="115" y="81" text-anchor="middle" font-size="7" fill="#f59e0b">15°</text>

  <!-- skewY(-15) -->
  <rect x="180" y="30" width="50" height="40" rx="2" fill="#fef9c3" stroke="#f59e0b" stroke-width="1"
        transform="skewY(-15)"/>
  <text x="205" y="81" text-anchor="middle" font-size="7" fill="#f59e0b">-15°</text>

  <text x="150" y="101" text-anchor="middle" font-size="7" fill="#94a3b8">
    los puntos derechos se desplazan en Y según su coordenada X
  </text>
</svg>
```

---

## Casos de uso: sombra en perspectiva y texto inclinado

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sombra en perspectiva: texto + su sombra con skewX -->
  <g transform="translate(10, 20)">
    <!-- Sombra: skewX para perspectiva + aplastada -->
    <text x="0" y="50" font-size="28" font-weight="900"
          fill="#94a3b8" fill-opacity="0.4"
          transform="skewX(-30) scale(1, 0.3) translate(0, 80)">TEXTO</text>
    <!-- Texto real encima -->
    <text x="0" y="50" font-size="28" font-weight="900" fill="#1e293b">TEXTO</text>
  </g>
  <text x="85" y="112" text-anchor="middle" font-size="7" fill="#64748b">sombra en perspectiva</text>
  <text x="85" y="121" text-anchor="middle" font-size="6.5" fill="#94a3b8">skewX(-30) + scale(1, 0.3)</text>

  <!-- Paralelogramo -->
  <rect x="185" y="35" width="90" height="50" rx="2"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"
        transform="skewX(-15)"/>
  <text x="225" y="100" text-anchor="middle" font-size="7" fill="#3b82f6">paralelogramo</text>
  <text x="225" y="109" text-anchor="middle" font-size="6.5" fill="#94a3b8">rect + skewX</text>
</svg>
```
