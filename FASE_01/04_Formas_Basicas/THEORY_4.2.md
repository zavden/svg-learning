# 4.2 Círculo `<circle>`

El círculo se define por su **centro** (`cx`, `cy`) y su **radio** (`r`). Todos los puntos del círculo están a distancia `r` del centro.

---

## Los tres atributos

`cx` y `cy` son opcionales (por defecto `0`). `r` es obligatorio.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Círculo centrado -->
  <circle cx="80" cy="65" r="45" fill="#3b82f6" opacity="0.85"/>

  <!-- Líneas de referencia: radio -->
  <line x1="80" y1="65" x2="125" y2="65"
        stroke="white" stroke-width="1.5" stroke-dasharray="3,2"/>
  <text x="102" y="60" text-anchor="middle" font-size="8" fill="white">r=45</text>

  <!-- Centro -->
  <circle cx="80" cy="65" r="3" fill="white"/>
  <text x="86" y="72" font-size="7" fill="white">cx=80, cy=65</text>

  <!-- Anotaciones externas -->
  <line x1="35" y1="65" x2="80" y2="65"
        stroke="#cbd5e1" stroke-width="1" stroke-dasharray="2,2"/>
  <line x1="80" y1="20" x2="80" y2="65"
        stroke="#cbd5e1" stroke-width="1" stroke-dasharray="2,2"/>
  <text x="30" y="68" text-anchor="end" font-size="7" fill="#94a3b8">cx</text>
  <text x="83" y="18" font-size="7" fill="#94a3b8">cy</text>

  <!-- Información -->
  <text x="210" y="45" text-anchor="middle"
        font-size="9" fill="#64748b" font-family="monospace">&lt;circle</text>
  <text x="210" y="60" text-anchor="middle"
        font-size="9" fill="#d19a66" font-family="monospace">cx="80"</text>
  <text x="210" y="75" text-anchor="middle"
        font-size="9" fill="#d19a66" font-family="monospace">cy="65"</text>
  <text x="210" y="90" text-anchor="middle"
        font-size="9" fill="#d19a66" font-family="monospace">r="45"</text>
  <text x="210" y="105" text-anchor="middle"
        font-size="9" fill="#56b6c2" font-family="monospace">/&gt;</text>
</svg>
```

---

## Cuidado: cx=0, cy=0 no centra el círculo

El valor por defecto de `cx` y `cy` es `0` — la **esquina superior izquierda** del viewport. Con r=30, solo el cuadrante inferior derecho es visible.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Incorrecto: círculo en (0,0) solo parcialmente visible -->
  <rect x="0" y="0" width="130" height="130" fill="#fef2f2"/>
  <circle cx="0" cy="0" r="55" fill="#ef4444" opacity="0.7"/>
  <circle cx="0" cy="0" r="3" fill="#991b1b"/>
  <text x="65" y="120" text-anchor="middle" font-size="8" fill="#991b1b">
    cx=0, cy=0 — solo 1/4 visible
  </text>

  <!-- Correcto: círculo centrado explícitamente -->
  <rect x="150" y="0" width="150" height="130" fill="#f0fdf4"/>
  <circle cx="225" cy="65" r="55" fill="#10b981" opacity="0.7"/>
  <circle cx="225" cy="65" r="3" fill="#166534"/>
  <text x="225" y="120" text-anchor="middle" font-size="8" fill="#166534">
    cx=225, cy=65 — centrado ✓
  </text>

  <line x1="150" y1="0" x2="150" y2="130" stroke="#e2e8f0" stroke-width="2"/>
</svg>
```

---

## El radio determina el tamaño

El diámetro del círculo es `2*r`. El círculo se extiende `r` unidades en cada dirección desde su centro.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="9" fill="#64748b">Misma posición de centro — distintos radios</text>

  <!-- Círculos concéntricos en y=70 pero distintas posiciones para legibilidad -->
  <circle cx="30"  cy="75" r="15"  fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="30"  y="103" text-anchor="middle" font-size="8" fill="#64748b">r=15</text>

  <circle cx="90"  cy="75" r="30"  fill="#bfdbfe" stroke="#3b82f6" stroke-width="1"/>
  <text x="90"  y="118" text-anchor="middle" font-size="8" fill="#64748b">r=30</text>

  <circle cx="175" cy="75" r="45"  fill="#93c5fd" stroke="#3b82f6" stroke-width="1"/>
  <text x="175" y="118" text-anchor="middle" font-size="8" fill="#3b82f6">r=45</text>

  <circle cx="268" cy="75" r="28"  fill="#3b82f6" stroke="#1e40af" stroke-width="1"/>
  <text x="268" y="117" text-anchor="middle" font-size="8" fill="#1e40af">r=28</text>
</svg>
```

---

## Casos de uso típicos

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="18" text-anchor="middle"
        font-size="9" fill="#64748b">Usos comunes del &lt;circle&gt;</text>

  <!-- Punto en gráfico -->
  <g transform="translate(30, 65)">
    <line x1="-20" y1="0" x2="20" y2="0" stroke="#cbd5e1" stroke-width="1"/>
    <line x1="0" y1="-20" x2="0" y2="20" stroke="#cbd5e1" stroke-width="1"/>
    <circle cx="8" cy="-10" r="5" fill="#3b82f6"/>
    <circle cx="-5" cy="5" r="5" fill="#3b82f6"/>
    <circle cx="15" cy="-5" r="5" fill="#3b82f6"/>
    <text y="38" text-anchor="middle" font-size="7" fill="#64748b">puntos en gráfico</text>
  </g>

  <!-- Avatar -->
  <g transform="translate(110, 65)">
    <circle r="28" fill="#e0e7ff"/>
    <circle cy="-6" r="11" fill="#6366f1"/>
    <ellipse cy="20" rx="18" ry="12" fill="#6366f1"/>
    <text y="38" text-anchor="middle" font-size="7" fill="#64748b">avatar / foto</text>
  </g>

  <!-- Indicador de estado -->
  <g transform="translate(185, 65)">
    <circle r="12" fill="#10b981"/>
    <text y="4" text-anchor="middle" font-size="10" fill="white">✓</text>
    <text y="26" text-anchor="middle" font-size="7" fill="#10b981">éxito</text>
    <circle cx="32" r="12" fill="#ef4444"/>
    <text cx="32" x="32" y="4" text-anchor="middle" font-size="10" fill="white">✗</text>
    <text x="32" y="26" text-anchor="middle" font-size="7" fill="#ef4444">error</text>
    <text x="16" y="38" text-anchor="middle" font-size="7" fill="#64748b">indicadores</text>
  </g>

  <!-- Radio button -->
  <g transform="translate(260, 65)">
    <circle r="12" fill="none" stroke="#3b82f6" stroke-width="2"/>
    <circle r="6" fill="#3b82f6"/>
    <text y="26" text-anchor="middle" font-size="7" fill="#64748b">radio</text>
    <text y="38" text-anchor="middle" font-size="7" fill="#64748b">button</text>
  </g>
</svg>
```
