# 4.3 Elipse `<ellipse>`

La elipse tiene **dos radios independientes**: `rx` (horizontal) y `ry` (vertical). Cuando `rx = ry`, el resultado es un círculo perfecto.

---

## Los cuatro atributos

Centro en `cx`, `cy` y dimensiones con `rx`, `ry`.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <ellipse cx="150" cy="70" rx="100" ry="45" fill="#6366f1" opacity="0.85"/>

  <!-- Ejes -->
  <line x1="50" y1="70" x2="250" y2="70"
        stroke="white" stroke-width="1" stroke-dasharray="4,2"/>
  <line x1="150" y1="25" x2="150" y2="115"
        stroke="white" stroke-width="1" stroke-dasharray="4,2"/>

  <!-- rx anotación -->
  <line x1="150" y1="62" x2="250" y2="62"
        stroke="#fde68a" stroke-width="1.5"/>
  <text x="200" y="56" text-anchor="middle" font-size="9" fill="#fde68a">rx = 100</text>

  <!-- ry anotación -->
  <line x1="158" y1="70" x2="158" y2="25"
        stroke="#86efac" stroke-width="1.5"/>
  <text x="185" y="48" text-anchor="start" font-size="9" fill="#86efac">ry = 45</text>

  <!-- Centro -->
  <circle cx="150" cy="70" r="4" fill="white"/>
  <text x="158" y="75" font-size="8" fill="white">cx=150, cy=70</text>

  <!-- Código -->
  <text x="150" y="130" text-anchor="middle"
        font-size="8" fill="#64748b" font-family="monospace">
    &lt;ellipse cx="150" cy="70" rx="100" ry="45"/&gt;
  </text>
</svg>
```

---

## Elipse horizontal, vertical y circular

La forma de la elipse depende de la relación entre `rx` y `ry`.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- rx > ry → achatada horizontal -->
  <ellipse cx="55" cy="55" rx="45" ry="20" fill="#3b82f6" opacity="0.85"/>
  <text x="55" y="90"  text-anchor="middle" font-size="8" fill="#64748b">rx=45, ry=20</text>
  <text x="55" y="102" text-anchor="middle" font-size="7" fill="#94a3b8">horizontal</text>

  <!-- rx = ry → círculo -->
  <ellipse cx="150" cy="55" rx="35" ry="35" fill="#10b981" opacity="0.85"/>
  <text x="150" y="104" text-anchor="middle" font-size="8" fill="#64748b">rx=ry=35</text>
  <text x="150" y="116" text-anchor="middle" font-size="7" fill="#94a3b8">= círculo</text>

  <!-- rx < ry → alta vertical -->
  <ellipse cx="245" cy="65" rx="22" ry="50" fill="#a855f7" opacity="0.85"/>
  <text x="245" y="128" text-anchor="middle" font-size="8" fill="#64748b">rx=22, ry=50</text>
</svg>
```

---

## Casos de uso: sombras, planetas y más

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Planeta con sombra proyectada -->
  <g transform="translate(60, 60)">
    <!-- Sombra (elipse achatada debajo del objeto) -->
    <ellipse cx="0" cy="40" rx="30" ry="8" fill="#000000" opacity="0.5"/>
    <!-- Planeta -->
    <circle r="28" fill="#3b82f6"/>
    <ellipse cx="-5" cy="-5" rx="12" ry="10" fill="#60a5fa" opacity="0.4"/>
    <text y="65" text-anchor="middle" font-size="7" fill="#64748b">sombra proyectada</text>
  </g>

  <!-- Sistema solar simplificado con órbitas -->
  <g transform="translate(190, 75)">
    <!-- Órbitas (elipses) -->
    <ellipse rx="70" ry="25" fill="none" stroke="#1e293b" stroke-width="1"/>
    <ellipse rx="48" ry="18" fill="none" stroke="#1e293b" stroke-width="1"/>
    <!-- Sol -->
    <circle r="14" fill="#f59e0b"/>
    <!-- Planetas en las órbitas -->
    <circle cx="70" cy="0" r="6" fill="#10b981"/>
    <circle cx="-48" cy="0" r="4" fill="#ef4444"/>
    <text y="42" text-anchor="middle" font-size="7" fill="#64748b">órbitas elípticas</text>
  </g>

  <!-- Ojo (ilustración) -->
  <g transform="translate(60, 120)">
    <ellipse rx="28" ry="15" fill="white"/>
    <ellipse rx="28" ry="15" fill="none" stroke="#64748b" stroke-width="0.5"/>
    <circle cx="5" r="11" fill="#3b82f6"/>
    <circle cx="5" r="5" fill="#1e293b"/>
    <circle cx="9" cy="-4" r="3" fill="white"/>
    <text y="28" text-anchor="middle" font-size="7" fill="#64748b">ojo</text>
  </g>
</svg>
```

---

## Relación con `<circle>`

`<circle r="50">` es exactamente igual a `<ellipse rx="50" ry="50">`. Usa `<circle>` cuando los radios son iguales — es más semántico.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- circle -->
  <circle cx="75" cy="50" r="35" fill="#10b981" opacity="0.8"/>
  <text x="75" y="92" text-anchor="middle" font-size="8" fill="#166534">
    &lt;circle r="35"&gt;
  </text>

  <!-- Símbolo = -->
  <text x="150" y="55" text-anchor="middle" font-size="20" fill="#64748b">=</text>

  <!-- ellipse rx=ry -->
  <ellipse cx="225" cy="50" rx="35" ry="35" fill="#3b82f6" opacity="0.8"/>
  <text x="225" y="92" text-anchor="middle" font-size="8" fill="#1e40af">
    &lt;ellipse rx="35" ry="35"&gt;
  </text>
</svg>
```
