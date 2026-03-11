# 11.2 `translate(tx, ty)`

Desplaza el sistema de coordenadas. Ideal para posicionar grupos, crear "pivotes" para otras transformaciones y mover elementos sin cambiar sus coordenadas internas.

---

## Efecto básico

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Original -->
  <rect x="10" y="25" width="50" height="40" fill="none" stroke="#e2e8f0" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="35"  y="18" text-anchor="middle" font-size="7" fill="#94a3b8">posición original</text>

  <!-- Tras translate(80, 0) -->
  <rect x="10" y="25" width="50" height="40" rx="3" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"
        transform="translate(80, 0)"/>
  <line x1="60" y1="45" x2="88" y2="45" stroke="#3b82f6" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="115" y="18" text-anchor="middle" font-size="7" fill="#3b82f6">translate(80, 0)</text>
  <text x="115" y="83" text-anchor="middle" font-size="6.5" fill="#94a3b8">+80 en X, sin cambio en Y</text>

  <!-- Tras translate(200, 15) -->
  <rect x="10" y="25" width="50" height="40" rx="3" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="translate(200, 15)"/>
  <text x="235" y="83" text-anchor="middle" font-size="7" fill="#10b981">translate(200, 15)</text>
</svg>
```

---

## `translate` vs cambiar `x`/`y` directamente

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">translate vs cambiar x/y</text>

  <!-- Cambiando x/y: solo mueve este elemento -->
  <rect x="50" y="35" width="40" height="35" rx="3" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="70" y="82" text-anchor="middle" font-size="7" fill="#3b82f6">x="50" y="35"</text>
  <text x="70" y="92" text-anchor="middle" font-size="6.5" fill="#94a3b8">mueve este elemento</text>
  <text x="70" y="101" text-anchor="middle" font-size="6.5" fill="#94a3b8">solo funciona con x/y</text>

  <!-- translate en grupo: mueve TODO el grupo -->
  <g transform="translate(160, 35)">
    <rect x="0" y="0" width="40" height="35" rx="3" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
    <circle cx="20" cy="17" r="8" fill="#10b981" fill-opacity="0.5"/>
  </g>
  <text x="180" y="82" text-anchor="middle" font-size="7" fill="#10b981">translate en &lt;g&gt;</text>
  <text x="180" y="92" text-anchor="middle" font-size="6.5" fill="#94a3b8">mueve TODO el grupo</text>
  <text x="180" y="101" text-anchor="middle" font-size="6.5" fill="#94a3b8">sin cambiar coords internas</text>
</svg>
```

---

## Patrón: posicionar grupos con coordenadas internas limpias

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cada icono diseñado en torno a (0,0) y luego posicionado con translate -->
  <!-- Icono 1: diseñado en x=0,y=0, posicionado con translate -->
  <g transform="translate(30, 20)">
    <circle cx="0" cy="0" r="20" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="0" y="5" text-anchor="middle" font-size="14">★</text>
  </g>
  <text x="30" y="52" text-anchor="middle" font-size="7" fill="#3b82f6">translate(30,20)</text>

  <g transform="translate(120, 20)">
    <circle cx="0" cy="0" r="20" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
    <text x="0" y="5" text-anchor="middle" font-size="14">♥</text>
  </g>
  <text x="120" y="52" text-anchor="middle" font-size="7" fill="#10b981">translate(120,20)</text>

  <g transform="translate(210, 20)">
    <circle cx="0" cy="0" r="20" fill="#fef9c3" stroke="#f59e0b" stroke-width="1.5"/>
    <text x="0" y="5" text-anchor="middle" font-size="14">⚡</text>
  </g>
  <text x="210" y="52" text-anchor="middle" font-size="7" fill="#f59e0b">translate(210,20)</text>

  <text x="150" y="72" text-anchor="middle" font-size="7.5" fill="#64748b">
    cada icono diseñado centrado en (0,0) — posicionado con translate
  </text>
  <text x="150" y="84" text-anchor="middle" font-size="7" fill="#94a3b8">
    ventaja: las coords internas del icono no cambian nunca
  </text>
</svg>
```
