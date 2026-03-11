# Glosario — Trazados (Paths)

---

## Path Data (`d`)

El atributo `d` contiene la secuencia completa de comandos que describen la forma del path. Una cadena de letras (comandos) y números (parámetros).

```
d="M 20 80 L 150 20 C 180 10, 250 50, 270 80 Z"
    ↑          ↑         ↑                    ↑
   Move      Line    Cubic Bézier           Close
```

---

## Posición actual (current point)

El punto donde se encuentra el "lápiz" tras ejecutar el último comando. Cada comando lo actualiza. Es el punto de partida implícito para el siguiente comando.

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <path d="M 20 70 L 100 20 L 180 70 L 260 20"
        stroke="#3b82f6" stroke-width="2" fill="none" stroke-linecap="round"/>

  <!-- Posición actual avanza con cada comando -->
  <circle cx="20"  cy="70" r="5" fill="#ef4444"/>
  <circle cx="100" cy="20" r="4" fill="#3b82f6"/>
  <circle cx="180" cy="70" r="4" fill="#3b82f6"/>
  <circle cx="260" cy="20" r="4" fill="#10b981"/>

  <text x="20"  y="85" text-anchor="middle" font-size="7" fill="#ef4444">M</text>
  <text x="100" y="12" text-anchor="middle" font-size="7" fill="#3b82f6">L</text>
  <text x="180" y="85" text-anchor="middle" font-size="7" fill="#3b82f6">L</text>
  <text x="260" y="12" text-anchor="middle" font-size="7" fill="#10b981">posición actual</text>
</svg>
```

---

## Sub-trazado (sub-path)

Porción independiente de un path, iniciada por un `M`. Un path puede tener múltiples sub-trazados. `Z` cierra el sub-trazado actual.

---

## Punto de control (Control Point)

Punto que "jala" la curva sin que esta lo toque. Determina la dirección y la curvatura de la curva en sus extremos.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Bézier cúbica con CPs visibles -->
  <path d="M 20 85 C 50 10, 130 10, 160 85"
        stroke="#3b82f6" stroke-width="2.5" fill="none"/>

  <line x1="20"  y1="85" x2="50"  y2="10" stroke="#ef4444" stroke-width="1.5" stroke-dasharray="3,2"/>
  <line x1="160" y1="85" x2="130" y2="10" stroke="#10b981" stroke-width="1.5" stroke-dasharray="3,2"/>

  <circle cx="20"  cy="85" r="4" fill="white"/>
  <circle cx="160" cy="85" r="4" fill="white"/>
  <circle cx="50"  cy="10" r="5" fill="#ef4444"/>
  <circle cx="130" cy="10" r="5" fill="#10b981"/>

  <text x="50"  y="7"  text-anchor="middle" font-size="7" fill="#ef4444">CP1</text>
  <text x="130" y="7"  text-anchor="middle" font-size="7" fill="#10b981">CP2</text>

  <!-- Bézier cuadrática con CP único -->
  <path d="M 195 85 Q 245 10, 290 85"
        stroke="#f59e0b" stroke-width="2.5" fill="none"/>
  <line x1="195" y1="85" x2="245" y2="10" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="290" y1="85" x2="245" y2="10" stroke="#fef9c3" stroke-width="1" stroke-dasharray="2,2"/>
  <circle cx="195" cy="85" r="4" fill="white"/>
  <circle cx="290" cy="85" r="4" fill="white"/>
  <circle cx="245" cy="10" r="5" fill="#f59e0b"/>
  <text x="245" y="7" text-anchor="middle" font-size="7" fill="#f59e0b">CP único (Q)</text>
</svg>
```

---

## Curva de Bézier

Curva matemática definida por un punto de inicio, un punto final y uno o más puntos de control. Las **cuadráticas** (Q) tienen 1 CP; las **cúbicas** (C) tienen 2 CP. Más puntos de control = más control sobre la forma.

---

## `large-arc-flag`

Parámetro del comando `A`. `0` = tomar el arco más corto (ángulo < 180°). `1` = tomar el arco más largo (ángulo ≥ 180°).

---

## `sweep-flag`

Parámetro del comando `A`. `0` = sentido antihorario (counter-clockwise). `1` = sentido horario (clockwise).

---

## `fill-rule`

Define qué regiones de un path complejo se consideran interiores. `nonzero` (defecto): regla de no-cero. `evenodd`: regla par-impar — crea agujeros en áreas de doble superposición.

---

## Path relativo vs absoluto

```svg
<svg width="300" height="75"
     viewBox="0 0 300 75"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8" fill="white">Absoluto vs Relativo</text>

  <text x="10" y="36" font-size="8" fill="#3b82f6" font-weight="700">Absoluto (MAYÚSCULA)</text>
  <text x="10" y="48" font-size="7" fill="#64748b">L 100 50 → siempre va al punto (100, 50)</text>
  <text x="10" y="60" font-size="7" fill="#64748b">no importa la posición actual</text>

  <line x1="155" y1="22" x2="155" y2="75" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="36" font-size="8" fill="#10b981" font-weight="700">Relativo (minúscula)</text>
  <text x="165" y="48" font-size="7" fill="#64748b">l 100 50 → desplaza +100, +50 desde aquí</text>
  <text x="165" y="60" font-size="7" fill="#64748b">depende de la posición actual</text>
</svg>
```
