# 4.5 Polilínea `<polyline>`

La polilínea conecta una serie de puntos en orden. A diferencia del polígono, **no cierra automáticamente** el contorno.

---

## El atributo `points`

Lista de coordenadas `x,y` separadas por espacios (o comas). Cada par define un vértice.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <polyline points="20,100 80,30 140,80 200,20 280,90"
            stroke="#3b82f6" stroke-width="2.5" fill="none"/>

  <!-- Vértices marcados -->
  <circle cx="20"  cy="100" r="5" fill="#ef4444"/>
  <circle cx="80"  cy="30"  r="5" fill="#ef4444"/>
  <circle cx="140" cy="80"  r="5" fill="#ef4444"/>
  <circle cx="200" cy="20"  r="5" fill="#ef4444"/>
  <circle cx="280" cy="90"  r="5" fill="#ef4444"/>

  <!-- Etiquetas -->
  <text x="20"  y="115" text-anchor="middle" font-size="8" fill="#64748b">P1</text>
  <text x="80"  y="22"  text-anchor="middle" font-size="8" fill="#64748b">P2</text>
  <text x="140" y="95"  text-anchor="middle" font-size="8" fill="#64748b">P3</text>
  <text x="200" y="12"  text-anchor="middle" font-size="8" fill="#64748b">P4</text>
  <text x="280" y="105" text-anchor="middle" font-size="8" fill="#64748b">P5</text>

  <!-- Código -->
  <rect x="0" y="113" width="300" height="17" fill="#0f172a"/>
  <text x="150" y="124" text-anchor="middle"
        font-size="7" fill="#abb2bf" font-family="monospace">
    points="20,100 80,30 140,80 200,20 280,90"
  </text>
</svg>
```

---

## `fill="none"` — trampa crítica

Sin `fill="none"`, el área entre los puntos y el eje se rellena con negro. La polilínea abierta no tiene un "interior" visual intuitivo.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin fill="none" (default fill=black) -->
  <rect x="5" y="5" width="135" height="115" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <polyline points="20,95 60,35 100,70 130,25"
            stroke="#ef4444" stroke-width="2"/>
  <text x="72" y="118" text-anchor="middle" font-size="8" fill="#ef4444">fill por defecto (negro)</text>

  <!-- Con fill="none" -->
  <rect x="155" y="5" width="140" height="115" rx="4" fill="#f0fdf4" stroke="#bbf7d0" stroke-width="1"/>
  <polyline points="170,95 210,35 250,70 280,25"
            stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="225" y="118" text-anchor="middle" font-size="8" fill="#10b981">fill="none" ✓</text>
</svg>
```

---

## Sintaxis de `points`: variantes válidas

Los separadores pueden ser espacios, comas o una mezcla. Los tres formatos son equivalentes.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Formato 1: "x,y x,y" -->
  <polyline points="20,30 80,80 140,30"
            stroke="#3b82f6" stroke-width="2" fill="none"/>
  <text x="80" y="100" text-anchor="middle" font-size="7" fill="#64748b" font-family="monospace">
    "20,30 80,80 140,30"
  </text>

  <!-- Formato 2: "x y x y" (sin comas) -->
  <polyline points="160 30 220 80 280 30"
            stroke="#10b981" stroke-width="2" fill="none"/>
  <text x="220" y="100" text-anchor="middle" font-size="7" fill="#64748b" font-family="monospace">
    "160 30 220 80 280 30"
  </text>

  <!-- Etiquetas de formato -->
  <text x="80"  y="112" text-anchor="middle" font-size="7" fill="#94a3b8">comas entre x,y</text>
  <text x="220" y="112" text-anchor="middle" font-size="7" fill="#94a3b8">solo espacios</text>
</svg>
```

---

## Caso de uso: gráfico de línea

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Ejes -->
  <line x1="30" y1="120" x2="285" y2="120" stroke="#334155" stroke-width="1.5"/>
  <line x1="30" y1="15"  x2="30"  y2="120" stroke="#334155" stroke-width="1.5"/>

  <!-- Líneas de cuadrícula -->
  <line x1="30" y1="85"  x2="285" y2="85"  stroke="#1e293b" stroke-width="1"/>
  <line x1="30" y1="50"  x2="285" y2="50"  stroke="#1e293b" stroke-width="1"/>

  <!-- Área bajo la curva (relleno suave) -->
  <polyline points="30,120 75,95 120,60 165,75 210,35 255,55 285,40 285,120"
            fill="#3b82f6" opacity="0.15" stroke="none"/>

  <!-- Línea de datos -->
  <polyline points="30,120 75,95 120,60 165,75 210,35 255,55 285,40"
            stroke="#3b82f6" stroke-width="2" fill="none" stroke-linejoin="round"/>

  <!-- Puntos de datos -->
  <circle cx="30"  cy="120" r="3" fill="#3b82f6"/>
  <circle cx="75"  cy="95"  r="3" fill="#3b82f6"/>
  <circle cx="120" cy="60"  r="3" fill="#3b82f6"/>
  <circle cx="165" cy="75"  r="3" fill="#3b82f6"/>
  <circle cx="210" cy="35"  r="4" fill="#60a5fa"/>
  <circle cx="255" cy="55"  r="3" fill="#3b82f6"/>
  <circle cx="285" cy="40"  r="3" fill="#3b82f6"/>

  <!-- Etiquetas del eje X -->
  <text x="30"  y="135" text-anchor="middle" font-size="7" fill="#64748b">Ene</text>
  <text x="75"  y="135" text-anchor="middle" font-size="7" fill="#64748b">Feb</text>
  <text x="120" y="135" text-anchor="middle" font-size="7" fill="#64748b">Mar</text>
  <text x="165" y="135" text-anchor="middle" font-size="7" fill="#64748b">Abr</text>
  <text x="210" y="135" text-anchor="middle" font-size="7" fill="#64748b">May</text>
  <text x="255" y="135" text-anchor="middle" font-size="7" fill="#64748b">Jun</text>
  <text x="285" y="135" text-anchor="middle" font-size="7" fill="#64748b">Jul</text>

  <!-- Título -->
  <text x="155" y="148" text-anchor="middle" font-size="7" fill="#475569">gráfico de línea con &lt;polyline&gt;</text>
</svg>
```

---

## Zigzag y formas angulares

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Zigzag regular -->
  <polyline points="10,80 35,30 60,80 85,30 110,80 135,30 160,80"
            stroke="#f59e0b" stroke-width="2.5" fill="none" stroke-linejoin="miter"/>
  <text x="85" y="100" text-anchor="middle" font-size="8" fill="#64748b">zigzag (miter)</text>

  <!-- Escalones -->
  <polyline points="175,75 195,75 195,45 225,45 225,75 255,75 255,45 285,45"
            stroke="#a855f7" stroke-width="2.5" fill="none" stroke-linejoin="round"/>
  <text x="230" y="100" text-anchor="middle" font-size="8" fill="#64748b">escalones (round)</text>

  <!-- Indicador sin cierre -->
  <text x="85"  y="113" text-anchor="middle" font-size="7" fill="#94a3b8">abierta — no cierra</text>
  <text x="230" y="113" text-anchor="middle" font-size="7" fill="#94a3b8">abierta — no cierra</text>
</svg>
```
