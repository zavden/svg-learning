# 11.9 Transformaciones y sistema de coordenadas

Cada transformación crea un sistema de coordenadas local. Los hijos heredan el sistema del padre y las transformaciones se acumulan.

---

## Herencia en profundidad

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Herencia de coordenadas</text>

  <!-- Nivel 1: translate -->
  <g transform="translate(40, 35)">
    <rect x="0" y="0" width="220" height="80" rx="4"
          fill="#dbeafe" stroke="#3b82f6" stroke-width="1" fill-opacity="0.3"/>
    <text x="5" y="12" font-size="7" fill="#3b82f6">translate(40, 35)</text>

    <!-- Nivel 2: rotate dentro del translate -->
    <g transform="rotate(10, 110, 40)">
      <rect x="30" y="20" width="160" height="50" rx="3"
            fill="#dcfce7" stroke="#10b981" stroke-width="1" fill-opacity="0.5"/>
      <text x="35" y="32" font-size="7" fill="#10b981">rotate(10, 110, 40)</text>

      <!-- Nivel 3: scale dentro del rotate -->
      <rect x="60" y="30" width="80" height="30" rx="3"
            fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5"/>
      <text x="100" y="49" text-anchor="middle" font-size="6.5" fill="#92400e">scale(?) — hereda todo</text>
    </g>
  </g>

  <text x="150" y="120" text-anchor="middle" font-size="7" fill="#64748b">
    el rect más interior acumula translate + rotate del padre
  </text>
</svg>
```

---

## Efecto en stroke-width y font-size

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">scale afecta stroke-width y font-size</text>

  <!-- Sin scale: stroke=1, font=10 -->
  <g transform="translate(20, 28)">
    <rect x="0" y="0" width="50" height="40" fill="#f1f5f9" stroke="#3b82f6" stroke-width="1"/>
    <text x="25" y="55" text-anchor="middle" font-size="10" fill="#3b82f6">Aa</text>
    <text x="25" y="68" text-anchor="middle" font-size="6.5" fill="#64748b">sin scale</text>
    <text x="25" y="76" text-anchor="middle" font-size="6" fill="#94a3b8">sw=1 / fs=10</text>
  </g>

  <!-- scale(2): stroke y font también ×2 -->
  <g transform="translate(90, 28) scale(2)">
    <rect x="0" y="0" width="50" height="40" fill="#fef2f2" stroke="#ef4444" stroke-width="1"/>
    <text x="25" y="55" text-anchor="middle" font-size="10" fill="#ef4444">Aa</text>
  </g>
  <text x="140" y="112" text-anchor="middle" font-size="6.5" fill="#ef4444">scale(2): sw=2, fs=20 visual</text>

  <!-- Solución: non-scaling-stroke -->
  <g transform="translate(200, 28) scale(2)">
    <rect x="0" y="0" width="50" height="40" fill="#dcfce7" stroke="#10b981" stroke-width="1"
          vector-effect="non-scaling-stroke"/>
  </g>
  <text x="245" y="98" text-anchor="middle" font-size="6.5" fill="#10b981">non-scaling-stroke</text>
  <text x="245" y="107" text-anchor="middle" font-size="6" fill="#94a3b8">stroke siempre 1px visual</text>
</svg>
```

---

## Current Transformation Matrix (CTM) con JavaScript

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">CTM — Current Transformation Matrix</text>

  <text x="10" y="36" font-size="7.5" fill="#64748b" font-family="monospace">const ctm = element.getCTM();</text>
  <text x="10" y="49" font-size="7" fill="#94a3b8">→ DOMMatrix con la transformación acumulada hasta ese elemento</text>

  <text x="10" y="64" font-size="7.5" fill="#64748b" font-family="monospace">const pt = svg.createSVGPoint();</text>
  <text x="10" y="77" font-size="7.5" fill="#64748b" font-family="monospace">pt.x = e.clientX; pt.y = e.clientY;</text>
  <text x="10" y="88" font-size="7" fill="#94a3b8">→ convertir coords del mouse a coords del elemento</text>
</svg>
```
