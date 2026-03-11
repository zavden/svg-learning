# 5.8 Arcos: `A`

`A` traza un fragmento de una elipse entre dos puntos. Es el comando más complejo — 7 parámetros — pero desbloquea sectores, arcos de progreso y bordes curvos.

---

## Los 7 parámetros

`A rx ry x-rotation large-arc-flag sweep-flag x y`

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#1e293b;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="80" fill="#0f172a"/>

  <!-- Tabla de parámetros -->
  <text x="10" y="18" font-size="9" fill="#60a5fa" font-family="monospace">A  rx  ry  rot  large  sweep  x  y</text>
  <text x="10" y="35" font-size="8.5" fill="#fbbf24" font-family="monospace">   50  30   0     0      1    200 60</text>

  <!-- Flechas de cada parámetro -->
  <text x="10"  y="55" font-size="6.5" fill="#94a3b8">radio H</text>
  <text x="50"  y="55" font-size="6.5" fill="#94a3b8">radio V</text>
  <text x="88"  y="55" font-size="6.5" fill="#94a3b8">rotación</text>
  <text x="130" y="55" font-size="6.5" fill="#94a3b8">arco</text>
  <text x="163" y="55" font-size="6.5" fill="#94a3b8">sentido</text>
  <text x="205" y="55" font-size="6.5" fill="#94a3b8">destino X</text>
  <text x="250" y="55" font-size="6.5" fill="#94a3b8">destino Y</text>

  <text x="150" y="72" text-anchor="middle" font-size="7" fill="#475569">
    comienza en la posición actual del lápiz
  </text>
</svg>
```

---

## Los 4 arcos posibles

Entre dos puntos, una elipse genera 4 arcos distintos. `large-arc-flag` y `sweep-flag` seleccionan cuál.

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Punto inicio y fin (compartidos por los 4 arcos) -->
  <circle cx="100" cy="80" r="4" fill="#ef4444"/>
  <circle cx="200" cy="80" r="4" fill="#ef4444"/>

  <!-- large=0, sweep=0: arco pequeño antihorario -->
  <path d="M 100 80 A 60 40 0 0 0 200 80" stroke="#3b82f6" stroke-width="2" fill="none"/>
  <text x="150" y="38" text-anchor="middle" font-size="7" fill="#3b82f6">large=0, sweep=0</text>

  <!-- large=0, sweep=1: arco pequeño horario -->
  <path d="M 100 80 A 60 40 0 0 1 200 80" stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="150" y="128" text-anchor="middle" font-size="7" fill="#10b981">large=0, sweep=1</text>

  <!-- large=1, sweep=0: arco grande antihorario -->
  <path d="M 100 80 A 60 40 0 1 0 200 80" stroke="#f59e0b" stroke-width="1.5" fill="none" stroke-dasharray="4,2"/>
  <text x="38" y="80" text-anchor="middle" font-size="6.5" fill="#f59e0b">large=1 sweep=0</text>

  <!-- large=1, sweep=1: arco grande horario -->
  <path d="M 100 80 A 60 40 0 1 1 200 80" stroke="#a855f7" stroke-width="1.5" fill="none" stroke-dasharray="4,2"/>
  <text x="262" y="80" text-anchor="middle" font-size="6.5" fill="#a855f7">large=1 sweep=1</text>

  <text x="150" y="150" text-anchor="middle" font-size="7.5" fill="#64748b">
    Mismo inicio, mismo fin, mismo rx/ry — 4 arcos distintos
  </text>
</svg>
```

---

## `sweep-flag`: dirección de la curva

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- sweep=0: antihorario (curva hacia arriba si los puntos están en la misma Y) -->
  <path d="M 20 80 A 50 40 0 0 0 120 80" stroke="#3b82f6" stroke-width="2.5" fill="none"/>
  <circle cx="20"  cy="80" r="4" fill="#ef4444"/>
  <circle cx="120" cy="80" r="4" fill="#3b82f6"/>
  <text x="70" y="20" text-anchor="middle" font-size="7.5" fill="#3b82f6">sweep=0 — antihorario</text>
  <text x="70" y="30" text-anchor="middle" font-size="7" fill="#94a3b8">(curva hacia arriba)</text>
  <text x="70" y="98" text-anchor="middle" font-size="7" fill="#3b82f6" font-family="monospace">A 50 40 0 0 0 120 80</text>

  <!-- Separador -->
  <line x1="155" y1="0" x2="155" y2="110" stroke="#e2e8f0" stroke-width="1"/>

  <!-- sweep=1: horario (curva hacia abajo) -->
  <path d="M 180 30 A 50 40 0 0 1 280 30" stroke="#10b981" stroke-width="2.5" fill="none"/>
  <circle cx="180" cy="30" r="4" fill="#ef4444"/>
  <circle cx="280" cy="30" r="4" fill="#10b981"/>
  <text x="230" y="95" text-anchor="middle" font-size="7.5" fill="#10b981">sweep=1 — horario</text>
  <text x="230" y="105" text-anchor="middle" font-size="7" fill="#94a3b8">(curva hacia abajo)</text>
  <text x="230" y="18" text-anchor="middle" font-size="7" fill="#10b981" font-family="monospace">A 50 40 0 0 1 280 30</text>
</svg>
```

---

## Caso de uso: sector circular (pie chart)

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Sector 1: 0° → 120° (1/3) — azul -->
  <!-- M al centro, L al borde, A a lo largo del arco, Z al centro -->
  <path d="M 90 75 L 90 15 A 60 60 0 0 1 141.96 105 Z"
        fill="#3b82f6" opacity="0.9"/>

  <!-- Sector 2: 120° → 240° (1/3) — verde -->
  <path d="M 90 75 L 141.96 105 A 60 60 0 0 1 38.04 105 Z"
        fill="#10b981" opacity="0.9"/>

  <!-- Sector 3: 240° → 360° (1/3) — amarillo -->
  <path d="M 90 75 L 38.04 105 A 60 60 0 0 1 90 15 Z"
        fill="#f59e0b" opacity="0.9"/>

  <text x="90" y="142" text-anchor="middle" font-size="7.5" fill="#64748b">3 sectores iguales</text>
  <text x="90" y="10" text-anchor="middle" font-size="7.5" fill="#475569">M centro → L borde → A arco → Z</text>

  <!-- Progress ring: arco parcial -->
  <circle cx="220" cy="75" r="50" fill="none" stroke="#1e293b" stroke-width="12"/>
  <!-- 75% → ángulo = 270° → large-arc=1 porque >180° -->
  <path d="M 220 25 A 50 50 0 1 1 185 121.6"
        stroke="#6366f1" stroke-width="12" fill="none" stroke-linecap="round"/>
  <text x="220" y="79" text-anchor="middle" font-size="14" fill="white" font-weight="700">75%</text>
  <text x="220" y="92" text-anchor="middle" font-size="7" fill="#64748b">progress ring</text>
  <text x="220" y="142" text-anchor="middle" font-size="7.5" fill="#64748b">arco con large-arc=1</text>
</svg>
```

---

## `x-rotation`: elipse rotada

Solo tiene efecto visible cuando `rx ≠ ry`.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin rotación -->
  <path d="M 30 60 A 60 25 0 1 1 130 60"
        stroke="#3b82f6" stroke-width="2.5" fill="#dbeafe" fill-opacity="0.5"/>
  <circle cx="30"  cy="60" r="4" fill="#ef4444"/>
  <circle cx="130" cy="60" r="4" fill="#3b82f6"/>
  <text x="80" y="110" text-anchor="middle" font-size="7.5" fill="#64748b">rotation=0</text>

  <!-- Con rotación 45° -->
  <path d="M 170 60 A 60 25 45 1 1 270 60"
        stroke="#f59e0b" stroke-width="2.5" fill="#fef9c3" fill-opacity="0.5"/>
  <circle cx="170" cy="60" r="4" fill="#ef4444"/>
  <circle cx="270" cy="60" r="4" fill="#f59e0b"/>
  <text x="220" y="110" text-anchor="middle" font-size="7.5" fill="#64748b">rotation=45</text>
</svg>
```
