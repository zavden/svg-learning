# 5.6 Curvas de Bézier cúbicas: `C` y `S`

Las curvas cúbicas de Bézier tienen **dos puntos de control** por segmento. Son el tipo de curva más usado en diseño gráfico.

---

## El concepto: puntos de control

Los puntos de control "jalan" la curva sin que esta los toque. La tangente de la curva en cada extremo apunta hacia su punto de control.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- La curva -->
  <path d="M 30 120 C 60 20, 240 20, 270 120"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>

  <!-- Líneas de control (handles) -->
  <line x1="30"  y1="120" x2="60"  y2="20" stroke="#ef4444" stroke-width="1.5" stroke-dasharray="3,2"/>
  <line x1="270" y1="120" x2="240" y2="20" stroke="#10b981" stroke-width="1.5" stroke-dasharray="3,2"/>

  <!-- Puntos de inicio y fin -->
  <circle cx="30"  cy="120" r="5" fill="white"/>
  <circle cx="270" cy="120" r="5" fill="white"/>

  <!-- Puntos de control -->
  <circle cx="60"  cy="20" r="5" fill="#ef4444"/>
  <circle cx="240" cy="20" r="5" fill="#10b981"/>

  <!-- Etiquetas -->
  <text x="30"  y="138" text-anchor="middle" font-size="8" fill="white">inicio</text>
  <text x="270" y="138" text-anchor="middle" font-size="8" fill="white">fin</text>
  <text x="60"  y="13"  text-anchor="middle" font-size="8" fill="#ef4444">CP1 (x1,y1)</text>
  <text x="240" y="13"  text-anchor="middle" font-size="8" fill="#10b981">CP2 (x2,y2)</text>

  <text x="150" y="148" text-anchor="middle" font-size="7.5" fill="#475569">
    C 60 20, 240 20, 270 120
  </text>
</svg>
```

---

## Sintaxis de `C`

`C x1 y1, x2 y2, x y` — tres pares de coordenadas:

1. **(x1, y1)** — primer punto de control (cerca del inicio)
2. **(x2, y2)** — segundo punto de control (cerca del final)
3. **(x, y)** — punto final de la curva

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Variantes de C con distintas posiciones de control -->

  <!-- Curva suave simétrica -->
  <path d="M 15 100 C 35 20, 95 20, 115 100"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>
  <line x1="15" y1="100" x2="35" y2="20" stroke="#dbeafe" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="115" y1="100" x2="95" y2="20" stroke="#dbeafe" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="35" cy="20" r="3" fill="#3b82f6"/>
  <circle cx="95" cy="20" r="3" fill="#3b82f6"/>
  <text x="65" y="128" text-anchor="middle" font-size="7.5" fill="#64748b">simétrica</text>

  <!-- Curva asimétrica: CP1 lejos, CP2 cerca -->
  <path d="M 140 100 C 145 10, 195 90, 210 100"
        stroke="#10b981" stroke-width="2.5" fill="none"/>
  <line x1="140" y1="100" x2="145" y2="10" stroke="#dcfce7" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="210" y1="100" x2="195" y2="90" stroke="#dcfce7" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="145" cy="10" r="3" fill="#10b981"/>
  <circle cx="195" cy="90" r="3" fill="#10b981"/>
  <text x="175" y="128" text-anchor="middle" font-size="7.5" fill="#64748b">asimétrica</text>

  <!-- Curva con "S": control points al mismo lado -->
  <path d="M 230 100 C 225 20, 285 80, 285 30"
        stroke="#f59e0b" stroke-width="2.5" fill="none"/>
  <line x1="230" y1="100" x2="225" y2="20" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="285" y1="30" x2="285" y2="80" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="225" cy="20" r="3" fill="#f59e0b"/>
  <circle cx="285" cy="80" r="3" fill="#f59e0b"/>
  <text x="257" y="128" text-anchor="middle" font-size="7.5" fill="#64748b">en S</text>
</svg>
```

---

## `S` — Smooth Cubic Bézier

`S x2 y2, x y` usa solo el segundo punto de control. El primero se calcula automáticamente como **reflejo** del CP2 del comando `C` o `S` anterior.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Primera curva con C -->
  <path d="M 20 80 C 40 20, 100 20, 120 80"
        stroke="#3b82f6" stroke-width="2" fill="none"/>

  <!-- Segunda curva con S: CP1 se refleja automáticamente -->
  <path d="M 20 80 C 40 20, 100 20, 120 80 S 200 140, 220 80"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>

  <!-- CP2 del primer C -->
  <line x1="120" y1="80" x2="100" y2="20" stroke="#ef4444" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="100" cy="20" r="4" fill="#ef4444"/>
  <text x="100" y="13" text-anchor="middle" font-size="7" fill="#ef4444">CP2 del C</text>

  <!-- Reflejo automático (CP1 implícito del S) -->
  <line x1="120" y1="80" x2="140" y2="140" stroke="#10b981" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="140" cy="140" r="4" fill="#10b981"/>
  <text x="140" y="135" text-anchor="middle" font-size="7" fill="#10b981">reflejo auto</text>

  <!-- CP2 explícito del S -->
  <line x1="220" y1="80" x2="200" y2="140" stroke="#f59e0b" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="200" cy="140" r="4" fill="#f59e0b"/>
  <text x="215" y="133" font-size="7" fill="#f59e0b">CP2 del S</text>

  <!-- Puntos finales -->
  <circle cx="20"  cy="80" r="4" fill="white"/>
  <circle cx="120" cy="80" r="4" fill="white"/>
  <circle cx="220" cy="80" r="4" fill="white"/>

  <text x="150" y="12" text-anchor="middle" font-size="7.5" fill="#475569">C → S: unión suave automática</text>
</svg>
```

---

## Caso de uso: formas orgánicas

Las curvas cúbicas son la base de formas naturales, logotipos y bordes decorativos.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Onda -->
  <path d="M 10 65 C 40 20, 80 110, 110 65 S 180 20, 210 65 S 280 110, 290 65"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>
  <text x="150" y="90" text-anchor="middle" font-size="7.5" fill="#64748b">C + S + S: onda</text>

  <!-- Blob / forma orgánica cerrada -->
  <path d="M 150 20 C 200 10, 230 50, 220 80 C 215 110, 170 120, 140 110 C 100 105, 75 85, 80 55 C 85 25, 110 28, 150 20 Z"
        fill="#a855f7" opacity="0.7"/>
  <text x="150" y="126" text-anchor="middle" font-size="7.5" fill="#64748b">blob orgánico cerrado con Z</text>
</svg>
```
