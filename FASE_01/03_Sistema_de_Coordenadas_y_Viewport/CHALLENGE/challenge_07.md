# Challenge 07 — SVG anidado con coordenadas independientes

**Tema:** SVG anidado, sistemas de coordenadas independientes, viewBox anidado
**Dificultad:** ⭐⭐⭐☆

---

## Tarea

Crea un **panel de instrumentos** compuesto por 4 "widgets" (medidores circulares). Cada widget debe ser un `<svg>` anidado dentro del SVG principal, con su **propio sistema de coordenadas** (su propio `viewBox`).

**Requisitos:**
1. El SVG principal tiene `width="480" height="280"` y representa el panel completo
2. Hay 4 SVGs anidados, cada uno en una esquina del panel
3. Cada SVG anidado tiene `viewBox="0 0 100 100"` (su propio espacio cuadrado)
4. Cada widget muestra: un círculo de fondo, un arco de progreso, y un label
5. Los 4 widgets son independientes: sus coordenadas internas no afectan a los demás

---

## Código base (panel con los 4 `<svg>` anidados sin contenido)

```svg
<svg width="480" height="280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block;">

  <!-- Título del panel -->
  <text x="240" y="28" text-anchor="middle"
        font-size="16" fill="#94a3b8"
        font-family="monospace" letter-spacing="4">
    DASHBOARD
  </text>

  <!-- Widget 1 — superior izquierda (CPU) -->
  <!-- x="20" y="40" width="110" height="110": posición en el panel principal -->
  <!-- viewBox="0 0 100 100": coordenadas internas del widget -->
  <svg x="20" y="40" width="110" height="110"
       viewBox="0 0 100 100"
       xmlns="http://www.w3.org/2000/svg">
    <!-- TAREA: Dibuja el widget CPU aquí usando coords 0-100 -->
    <!-- Ejemplo de arco de progreso al 70% -->
  </svg>

  <!-- Widget 2 — superior derecha (RAM) -->
  <svg x="350" y="40" width="110" height="110"
       viewBox="0 0 100 100"
       xmlns="http://www.w3.org/2000/svg">
    <!-- TAREA: Dibuja el widget RAM aquí -->
  </svg>

  <!-- Widget 3 — inferior izquierda (TEMP) -->
  <svg x="20" y="160" width="110" height="110"
       viewBox="0 0 100 100"
       xmlns="http://www.w3.org/2000/svg">
    <!-- TAREA: Dibuja el widget TEMP aquí -->
  </svg>

  <!-- Widget 4 — inferior derecha (DISK) -->
  <svg x="350" y="160" width="110" height="110"
       viewBox="0 0 100 100"
       xmlns="http://www.w3.org/2000/svg">
    <!-- TAREA: Dibuja el widget DISK aquí -->
  </svg>

  <!-- Panel central (info) -->
  <rect x="150" y="50" width="180" height="180" rx="8"
        fill="#1e293b" stroke="#334155" stroke-width="1"/>
  <text x="240" y="120" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="monospace">
    ESTADO DEL SISTEMA
  </text>
  <text x="240" y="150" text-anchor="middle"
        font-size="28" fill="#10b981" font-family="monospace">
    OK
  </text>
  <text x="240" y="175" text-anchor="middle"
        font-size="9" fill="#475569" font-family="monospace">
    Todos los sistemas
  </text>
  <text x="240" y="190" text-anchor="middle"
        font-size="9" fill="#475569" font-family="monospace">
    funcionando
  </text>

</svg>
```

---

## Pistas

<details>
<summary>Pista 1 — Arco de progreso con `<path>`</summary>

Para un arco circular de progreso, usa el comando `A` de SVG paths. Un arco del 70% (252° de 360°):

```svg
<!-- Círculo de fondo -->
<circle cx="50" cy="50" r="38" fill="none"
        stroke="#1e293b" stroke-width="8"/>

<!-- Arco de progreso: usa stroke-dasharray y stroke-dashoffset -->
<circle cx="50" cy="50" r="38" fill="none"
        stroke="#3b82f6" stroke-width="8"
        stroke-dasharray="239" stroke-dashoffset="72"
        stroke-linecap="round"
        transform="rotate(-90, 50, 50)"/>
<!-- stroke-dasharray = 2π×r = 2×3.14159×38 ≈ 239 -->
<!-- stroke-dashoffset = 239 × (1 - 0.70) = 72 (30% restante) -->
```

</details>

<details>
<summary>Pista 2 — Calcular stroke-dashoffset para otros porcentajes</summary>

Para un radio `r` y un porcentaje `p` (0–1):
- `circumference = 2 × π × r ≈ 6.283 × r`
- `stroke-dasharray = circumference`
- `stroke-dashoffset = circumference × (1 - p)`

Para r=38: circumference ≈ 239
- 80%: offset = 239 × 0.20 = 48
- 60%: offset = 239 × 0.40 = 96
- 45%: offset = 239 × 0.55 = 131

</details>

<details>
<summary>Pista 3 — Coordenadas de los SVGs anidados</summary>

Dentro de cada `<svg>` anidado con `viewBox="0 0 100 100"`, el centro siempre es (50, 50). Esto es independiente de dónde esté posicionado el SVG anidado dentro del padre. Puedes usar las mismas coordenadas para los 4 widgets.

</details>

---

## Resultado esperado

```svg-only
<svg width="480" height="280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block;">

  <text x="240" y="28" text-anchor="middle"
        font-size="16" fill="#94a3b8"
        font-family="monospace" letter-spacing="4">DASHBOARD</text>

  <!-- Widget 1: CPU 70% -->
  <svg x="20" y="40" width="110" height="110"
       viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="44" fill="#0f172a"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#1e293b" stroke-width="8"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#3b82f6" stroke-width="8"
            stroke-dasharray="239" stroke-dashoffset="72"
            stroke-linecap="round"
            transform="rotate(-90, 50, 50)"/>
    <text x="50" y="46" text-anchor="middle"
          font-size="20" fill="white" font-family="monospace"
          font-weight="bold">70%</text>
    <text x="50" y="62" text-anchor="middle"
          font-size="9" fill="#64748b" font-family="monospace">CPU</text>
  </svg>

  <!-- Widget 2: RAM 80% -->
  <svg x="350" y="40" width="110" height="110"
       viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="44" fill="#0f172a"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#1e293b" stroke-width="8"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#10b981" stroke-width="8"
            stroke-dasharray="239" stroke-dashoffset="48"
            stroke-linecap="round"
            transform="rotate(-90, 50, 50)"/>
    <text x="50" y="46" text-anchor="middle"
          font-size="20" fill="white" font-family="monospace"
          font-weight="bold">80%</text>
    <text x="50" y="62" text-anchor="middle"
          font-size="9" fill="#64748b" font-family="monospace">RAM</text>
  </svg>

  <!-- Widget 3: TEMP 60% -->
  <svg x="20" y="160" width="110" height="110"
       viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="44" fill="#0f172a"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#1e293b" stroke-width="8"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#f59e0b" stroke-width="8"
            stroke-dasharray="239" stroke-dashoffset="96"
            stroke-linecap="round"
            transform="rotate(-90, 50, 50)"/>
    <text x="50" y="46" text-anchor="middle"
          font-size="20" fill="white" font-family="monospace"
          font-weight="bold">60°</text>
    <text x="50" y="62" text-anchor="middle"
          font-size="9" fill="#64748b" font-family="monospace">TEMP</text>
  </svg>

  <!-- Widget 4: DISK 45% -->
  <svg x="350" y="160" width="110" height="110"
       viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="44" fill="#0f172a"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#1e293b" stroke-width="8"/>
    <circle cx="50" cy="50" r="38" fill="none"
            stroke="#a855f7" stroke-width="8"
            stroke-dasharray="239" stroke-dashoffset="131"
            stroke-linecap="round"
            transform="rotate(-90, 50, 50)"/>
    <text x="50" y="46" text-anchor="middle"
          font-size="20" fill="white" font-family="monospace"
          font-weight="bold">45%</text>
    <text x="50" y="62" text-anchor="middle"
          font-size="9" fill="#64748b" font-family="monospace">DISK</text>
  </svg>

  <!-- Panel central -->
  <rect x="150" y="50" width="180" height="180" rx="8"
        fill="#1e293b" stroke="#334155" stroke-width="1"/>
  <text x="240" y="120" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="monospace">
    ESTADO DEL SISTEMA
  </text>
  <text x="240" y="155" text-anchor="middle"
        font-size="28" fill="#10b981" font-family="monospace">OK</text>
  <text x="240" y="178" text-anchor="middle"
        font-size="9" fill="#475569" font-family="monospace">
    Todos los sistemas
  </text>
  <text x="240" y="193" text-anchor="middle"
        font-size="9" fill="#475569" font-family="monospace">
    funcionando
  </text>
</svg>
```
