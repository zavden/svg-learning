# 11.7 Encadenamiento de transformaciones

Múltiples transformaciones en el mismo atributo se aplican en orden. El orden importa: `translate rotate` ≠ `rotate translate`.

---

## La regla: el orden cambia el resultado

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">El orden de las transformaciones importa</text>

  <!-- Posición original -->
  <rect x="50" y="30" width="40" height="30" fill="none" stroke="#94a3b8" stroke-width="0.5" stroke-dasharray="3,2"/>

  <!-- translate(80,0) rotate(30): primero mueve, luego rota en la nueva posición -->
  <rect x="50" y="30" width="40" height="30" rx="3"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="translate(80,0) rotate(30)"/>
  <text x="150" y="100" text-anchor="middle" font-size="7" fill="#ef4444">translate(80,0) rotate(30)</text>
  <text x="150" y="110" text-anchor="middle" font-size="6.5" fill="#94a3b8">mueve 80px, luego rota desde (0,0)</text>

  <!-- rotate(30) translate(80,0): primero rota el eje, luego mueve en el eje rotado -->
  <rect x="50" y="30" width="40" height="30" rx="3"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="rotate(30) translate(80,0)"/>
  <text x="150" y="121" text-anchor="middle" font-size="7" fill="#10b981">rotate(30) translate(80,0)</text>
  <text x="150" y="130" text-anchor="middle" font-size="6.5" fill="#94a3b8">rota el eje, luego mueve 80px en eje rotado</text>
</svg>
```

---

## Patrón clásico: rotar sobre el propio centro

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">translate(cx,cy) rotate(a) translate(-cx,-cy)</text>

  <!-- El elemento: rect de 80×50 centrado en cx=150, cy=70 -->
  <!-- Centro: cx = 110+40 = 150, cy = 45+25 = 70 -->
  <rect x="110" y="45" width="80" height="50" fill="none" stroke="#94a3b8" stroke-width="0.5" stroke-dasharray="3,2"/>

  <!-- 3 versiones rotadas del mismo rect sobre su centro (150, 70) -->
  <rect x="110" y="45" width="80" height="50" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" fill-opacity="0.5"
        transform="translate(150,70) rotate(15) translate(-150,-70)"/>
  <rect x="110" y="45" width="80" height="50" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" fill-opacity="0.5"
        transform="translate(150,70) rotate(30) translate(-150,-70)"/>
  <rect x="110" y="45" width="80" height="50" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" fill-opacity="0.5"
        transform="translate(150,70) rotate(45) translate(-150,-70)"/>

  <!-- Marcar el centro -->
  <circle cx="150" cy="70" r="3" fill="#ef4444"/>

  <text x="150" y="108" text-anchor="middle" font-size="7.5" fill="#64748b">
    3 copias del mismo rect, rotadas 15°, 30°, 45° — todas en su sitio
  </text>
</svg>
```

---

## Composición: translate + scale + rotate

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <pattern id="ref-g" width="20" height="20" patternUnits="userSpaceOnUse">
      <line x1="20" y1="0" x2="20" y2="20" stroke="#f0f0f0" stroke-width="0.5"/>
      <line x1="0" y1="20" x2="20" y2="20" stroke="#f0f0f0" stroke-width="0.5"/>
    </pattern>
  </defs>
  <rect fill="url(#ref-g)" width="300" height="110"/>

  <!-- Flecha original -->
  <path d="M 0,-8 L 12,0 L 0,8 L 3,0 Z" fill="#3b82f6"
        transform="translate(40, 55)"/>
  <text x="40" y="90" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- Translate + rotate: apunta en dirección específica -->
  <path d="M 0,-8 L 12,0 L 0,8 L 3,0 Z" fill="#ef4444"
        transform="translate(120, 55) rotate(45)"/>
  <text x="120" y="90" text-anchor="middle" font-size="7" fill="#ef4444">translate + rotate(45)</text>

  <!-- Translate + scale + rotate: más grande y rotada -->
  <path d="M 0,-8 L 12,0 L 0,8 L 3,0 Z" fill="#10b981"
        transform="translate(220, 55) scale(1.8) rotate(-30)"/>
  <text x="220" y="90" text-anchor="middle" font-size="7" fill="#10b981">translate + scale(1.8) + rotate(-30)</text>
</svg>
```
