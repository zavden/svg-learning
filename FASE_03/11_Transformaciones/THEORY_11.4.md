# 11.4 `rotate(angle, cx, cy)`

Rota el sistema de coordenadas. Con un solo argumento rota desde el origen `(0,0)`. Con tres argumentos rota desde un punto específico — el uso más habitual.

---

## rotate(angle) — desde el origen

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">rotate — 4 ángulos distintos</text>

  <!-- 0° -->
  <rect x="25" y="32" width="50" height="35" rx="3" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="50" y="78" text-anchor="middle" font-size="7" fill="#3b82f6">0°</text>

  <!-- 15° desde su propio centro -->
  <rect x="105" y="32" width="50" height="35" rx="3" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="rotate(15, 130, 49)"/>
  <text x="130" y="85" text-anchor="middle" font-size="7" fill="#10b981">15°</text>

  <!-- 45° desde su propio centro -->
  <rect x="185" y="32" width="50" height="35" rx="3" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5"
        transform="rotate(45, 210, 49)"/>
  <text x="210" y="90" text-anchor="middle" font-size="7" fill="#f59e0b">45°</text>

  <!-- 90° desde su propio centro -->
  <rect x="255" y="32" width="35" height="25" rx="3" fill="#fce7f3" stroke="#ec4899" stroke-width="1.5"
        transform="rotate(90, 272, 44)"/>
  <text x="272" y="82" text-anchor="middle" font-size="7" fill="#ec4899">90°</text>

  <text x="150" y="102" text-anchor="middle" font-size="7" fill="#94a3b8">
    rotate(ángulo, cx, cy) — horario en SVG (eje Y invertido)
  </text>
</svg>
```

---

## rotate(a) vs rotate(a, cx, cy) — la diferencia clave

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">rotate(45) vs rotate(45, cx, cy)</text>

  <!-- Posición original (referencia) -->
  <rect x="80" y="35" width="50" height="35" fill="none" stroke="#94a3b8" stroke-width="0.5" stroke-dasharray="3,2"/>

  <!-- rotate(45) solo: rota desde (0,0) — el elemento se aleja -->
  <rect x="80" y="35" width="50" height="35" rx="3"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="rotate(45)"/>
  <!-- Marcar el origen (0,0) -->
  <circle cx="0" cy="0" r="3" fill="#ef4444"/>
  <text x="20" y="45" font-size="7" fill="#ef4444">origin (0,0)</text>
  <text x="35" y="107" text-anchor="middle" font-size="7" fill="#ef4444">rotate(45) desde (0,0)</text>
  <text x="35" y="116" text-anchor="middle" font-size="6.5" fill="#94a3b8">se aleja del lugar original</text>

  <!-- rotate(45, 105, 52): rota desde el centro del rect — se queda en su lugar -->
  <rect x="80" y="35" width="50" height="35" rx="3"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="translate(100,0) rotate(45, 105, 52)"/>
  <circle cx="205" cy="52" r="3" fill="#10b981"/>
  <text x="205" y="107" text-anchor="middle" font-size="7" fill="#10b981">rotate(45, cx, cy)</text>
  <text x="205" y="116" text-anchor="middle" font-size="6.5" fill="#94a3b8">gira en su lugar ✓</text>
</svg>
```

---

## Calcular el centro para rotar "en su lugar"

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Calcular cx, cy para el centro</text>

  <!-- rect: cx = x + w/2, cy = y + h/2 -->
  <rect x="20" y="32" width="80" height="50" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"
        transform="rotate(20, 60, 57)"/>
  <!-- Marcar el centro -->
  <circle cx="60" cy="57" r="3" fill="#ef4444"/>
  <text x="60" y="96" text-anchor="middle" font-size="7" fill="#3b82f6">rect: cx=x+w/2, cy=y+h/2</text>
  <text x="60" y="105" text-anchor="middle" font-size="6.5" fill="#94a3b8">20+40=60, 32+25=57</text>

  <!-- circle: cx y cy ya son el centro -->
  <circle cx="200" cy="60" r="35"
          fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
          transform="rotate(20, 200, 60)"/>
  <circle cx="200" cy="60" r="3" fill="#ef4444"/>
  <text x="200" y="108" text-anchor="middle" font-size="7" fill="#10b981">circle: cx y cy ya son el centro</text>
  <text x="200" y="117" text-anchor="middle" font-size="6.5" fill="#94a3b8">rotate(20, cx, cy)</text>
</svg>
```

---

## Reloj: rotate para posicionar elementos radialmente

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Marco del reloj -->
  <circle cx="150" cy="65" r="60" fill="white" stroke="#e2e8f0" stroke-width="2"/>

  <!-- 12 marcas de hora, rotando el mismo elemento con distintos ángulos -->
  <!-- Cada marca a 30° de separación (360/12=30) -->
  <g fill="#334155">
    <rect x="148" y="12" width="4" height="10" rx="1" transform="rotate(0,   150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(30,  150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(60,  150, 65)"/>
    <rect x="148" y="12" width="4" height="10" rx="1" transform="rotate(90,  150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(120, 150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(150, 150, 65)"/>
    <rect x="148" y="12" width="4" height="10" rx="1" transform="rotate(180, 150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(210, 150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(240, 150, 65)"/>
    <rect x="148" y="12" width="4" height="10" rx="1" transform="rotate(270, 150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(300, 150, 65)"/>
    <rect x="148" y="12" width="4" height="8"  rx="1" transform="rotate(330, 150, 65)"/>
  </g>

  <!-- Manecillas -->
  <line x1="150" y1="65" x2="150" y2="30" stroke="#1e293b" stroke-width="3" stroke-linecap="round"
        transform="rotate(120, 150, 65)"/>
  <line x1="150" y1="65" x2="150" y2="22" stroke="#334155" stroke-width="2" stroke-linecap="round"
        transform="rotate(210, 150, 65)"/>

  <text x="150" y="118" text-anchor="middle" font-size="7.5" fill="#64748b">
    12 marcas idénticas, cada una con rotate(n×30, 150, 65)
  </text>
</svg>
```
