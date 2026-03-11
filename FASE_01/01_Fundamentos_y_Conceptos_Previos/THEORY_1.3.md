# 1.3 Cuándo usar SVG y cuándo no

SVG es poderoso, pero no es la solución para todo. Elegir el formato correcto es tan importante como saber usarlo.

---

## SVG sí: iconos e interfaz de usuario

Los iconos de UI son el caso de uso ideal de SVG. Son gráficos simples, necesitan verse nítidos en cualquier tamaño (pantallas Retina, zoom del navegador), y a menudo necesitan cambiar de color según el tema o estado.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <text x="150" y="20" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="sans-serif">
    Iconos SVG — nítidos a cualquier tamaño, coloreables con CSS
  </text>

  <!-- Ícono home -->
  <g transform="translate(35, 65)">
    <polygon points="0,-25 25,0 20,0 20,22 -20,22 -20,0 -25,0" fill="#3b82f6"/>
    <rect x="-10" y="5" width="20" height="17" fill="white" opacity="0.9"/>
    <text y="42" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">home</text>
  </g>

  <!-- Ícono check -->
  <g transform="translate(95, 65)">
    <circle r="22" fill="#10b981"/>
    <polyline points="-10,2 -2,10 12,-8" fill="none"
              stroke="white" stroke-width="3.5" stroke-linecap="round" stroke-linejoin="round"/>
    <text y="42" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">check</text>
  </g>

  <!-- Ícono alerta -->
  <g transform="translate(155, 65)">
    <polygon points="0,-24 24,20 -24,20" fill="#f59e0b"/>
    <rect x="-2" y="-10" width="4" height="14" rx="2" fill="white"/>
    <circle cx="0" cy="12" r="2.5" fill="white"/>
    <text y="42" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">alerta</text>
  </g>

  <!-- Ícono user -->
  <g transform="translate(215, 65)">
    <circle r="22" fill="#6366f1"/>
    <circle cx="0" cy="-5" r="8" fill="white"/>
    <ellipse cx="0" cy="18" rx="13" ry="9" fill="white"/>
    <text y="42" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">usuario</text>
  </g>

  <!-- Ícono búsqueda -->
  <g transform="translate(268, 65)">
    <circle cx="-3" cy="-5" r="12" fill="none" stroke="#ef4444" stroke-width="4"/>
    <line x1="6" y1="4" x2="18" y2="16" stroke="#ef4444" stroke-width="4" stroke-linecap="round"/>
    <text y="42" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">buscar</text>
  </g>
</svg>
```

---

## SVG sí: gráficos de datos

Barras, líneas, tortas, áreas — SVG es el backbone de casi todas las librerías de visualización de datos (D3.js, Chart.js, Recharts, Highcharts).

```svg
<svg width="300" height="150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Gráfico de barras simple -->
  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="sans-serif">
    Ventas por trimestre — gráfico SVG
  </text>

  <!-- Eje Y -->
  <line x1="40" y1="25" x2="40" y2="120" stroke="#cbd5e1" stroke-width="1"/>
  <!-- Eje X -->
  <line x1="40" y1="120" x2="285" y2="120" stroke="#cbd5e1" stroke-width="1"/>

  <!-- Barras -->
  <rect x="55"  y="75"  width="40" height="45" rx="3" fill="#3b82f6" opacity="0.85"/>
  <rect x="110" y="50"  width="40" height="70" rx="3" fill="#3b82f6" opacity="0.85"/>
  <rect x="165" y="60"  width="40" height="60" rx="3" fill="#3b82f6" opacity="0.85"/>
  <rect x="220" y="35"  width="40" height="85" rx="3" fill="#10b981" opacity="0.85"/>

  <!-- Etiquetas X -->
  <text x="75"  y="133" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">Q1</text>
  <text x="130" y="133" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">Q2</text>
  <text x="185" y="133" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">Q3</text>
  <text x="240" y="133" text-anchor="middle" font-size="8" fill="#64748b" font-family="sans-serif">Q4</text>

  <!-- Valores -->
  <text x="75"  y="70" text-anchor="middle" font-size="7" fill="#1e293b" font-family="sans-serif">45K</text>
  <text x="130" y="45" text-anchor="middle" font-size="7" fill="#1e293b" font-family="sans-serif">70K</text>
  <text x="185" y="55" text-anchor="middle" font-size="7" fill="#1e293b" font-family="sans-serif">60K</text>
  <text x="240" y="30" text-anchor="middle" font-size="7" fill="#10b981" font-family="sans-serif">85K ↑</text>
</svg>
```

---

## SVG sí: logotipos y decoración

Logos, separadores con curvas orgánicas, fondos geométricos — SVG a cualquier tamaño en cualquier pantalla.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block;">

  <defs>
    <linearGradient id="waveGrad" x1="0" y1="0" x2="1" y2="0">
      <stop offset="0%"   stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#8b5cf6"/>
    </linearGradient>
  </defs>

  <!-- Ola decorativa tipo "section separator" -->
  <path d="M0,60 C50,20 100,100 150,60 S250,20 300,60 L300,120 L0,120 Z"
        fill="url(#waveGrad)" opacity="0.3"/>
  <path d="M0,80 C60,40 120,110 180,70 S260,40 300,70 L300,120 L0,120 Z"
        fill="url(#waveGrad)" opacity="0.5"/>

  <!-- Logo ficticio encima -->
  <text x="150" y="48" text-anchor="middle"
        font-size="24" fill="white" font-family="sans-serif"
        font-weight="900" letter-spacing="-1">
    STUDIO
  </text>
  <text x="150" y="64" text-anchor="middle"
        font-size="9" fill="#93c5fd" font-family="sans-serif"
        letter-spacing="6">
    DESIGN · CODE · CREATE
  </text>
</svg>
```

---

## SVG no: fotografías y contenido realista

Una fotografía tiene millones de píxeles con transiciones de color orgánicas e impredecibles. Describir eso en SVG sería enormemente más pesado que JPEG.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <text x="150" y="18" text-anchor="middle"
        font-size="10" fill="#64748b" font-family="sans-serif">
    Para fotografías: JPEG/WebP gana siempre
  </text>

  <!-- Caso correcto -->
  <rect x="20" y="28" width="120" height="90" rx="6"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="80" y="50" text-anchor="middle"
        font-size="9" fill="#1e40af" font-family="sans-serif" font-weight="600">
    Icono / Logo
  </text>
  <circle cx="80" cy="78" r="20" fill="#3b82f6"/>
  <polyline points="70,78 77,85 92,68" fill="none"
            stroke="white" stroke-width="3" stroke-linecap="round"/>
  <text x="80" y="114" text-anchor="middle"
        font-size="8" fill="#10b981" font-family="sans-serif">
    ✓ SVG — ~500 bytes
  </text>

  <!-- Caso incorrecto -->
  <rect x="160" y="28" width="120" height="90" rx="6"
        fill="#fff1f2" stroke="#ef4444" stroke-width="1.5"/>
  <text x="220" y="50" text-anchor="middle"
        font-size="9" fill="#991b1b" font-family="sans-serif" font-weight="600">
    Fotografía
  </text>
  <!-- simulación de foto con rectángulos de colores -->
  <rect x="168" y="56" width="16" height="12" fill="#92400e"/>
  <rect x="184" y="56" width="16" height="12" fill="#78716c"/>
  <rect x="200" y="56" width="16" height="12" fill="#a3e635"/>
  <rect x="216" y="56" width="16" height="12" fill="#38bdf8"/>
  <rect x="232" y="56" width="16" height="12" fill="#f472b6"/>
  <rect x="248" y="56" width="24" height="12" fill="#fbbf24"/>
  <rect x="168" y="68" width="22" height="12" fill="#60a5fa"/>
  <rect x="190" y="68" width="14" height="12" fill="#34d399"/>
  <rect x="204" y="68" width="18" height="12" fill="#fb923c"/>
  <rect x="222" y="68" width="12" height="12" fill="#818cf8"/>
  <rect x="234" y="68" width="14" height="12" fill="#e879f9"/>
  <rect x="248" y="68" width="24" height="12" fill="#f87171"/>
  <rect x="168" y="80" width="104" height="14" fill="#cbd5e1"/>
  <text x="220" y="91" text-anchor="middle" font-size="7" fill="#64748b" font-family="sans-serif">millones de píxeles…</text>
  <text x="220" y="114" text-anchor="middle"
        font-size="8" fill="#ef4444" font-family="sans-serif">
    ✗ SVG — 500 KB+ (usar JPEG)
  </text>
</svg>
```

---

## Tabla de decisión rápida

```svg
<svg width="300" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cabecera -->
  <rect x="0" y="0" width="300" height="30" fill="#1e293b"/>
  <text x="75"  y="20" text-anchor="middle" font-size="10" fill="#f1f5f9" font-weight="600">Caso de uso</text>
  <text x="200" y="20" text-anchor="middle" font-size="10" fill="#f1f5f9" font-weight="600">Formato ideal</text>

  <!-- Filas -->
  <rect x="0" y="30" width="300" height="28" fill="#eff6ff"/>
  <text x="75"  y="49" text-anchor="middle" font-size="9" fill="#1e293b">Iconos / UI</text>
  <text x="200" y="49" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">SVG ✓</text>

  <rect x="0" y="58" width="300" height="28" fill="#f8fafc"/>
  <text x="75"  y="77" text-anchor="middle" font-size="9" fill="#1e293b">Logotipos</text>
  <text x="200" y="77" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">SVG ✓</text>

  <rect x="0" y="86" width="300" height="28" fill="#eff6ff"/>
  <text x="75"  y="105" text-anchor="middle" font-size="9" fill="#1e293b">Gráficos de datos</text>
  <text x="200" y="105" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">SVG ✓</text>

  <rect x="0" y="114" width="300" height="28" fill="#f8fafc"/>
  <text x="75"  y="133" text-anchor="middle" font-size="9" fill="#1e293b">Fotografías</text>
  <text x="200" y="133" text-anchor="middle" font-size="9" fill="#ef4444" font-weight="600">JPEG / WebP ✓</text>

  <rect x="0" y="142" width="300" height="28" fill="#eff6ff"/>
  <text x="75"  y="161" text-anchor="middle" font-size="9" fill="#1e293b">Capturas de pantalla</text>
  <text x="200" y="161" text-anchor="middle" font-size="9" fill="#ef4444" font-weight="600">PNG ✓</text>

  <rect x="0" y="170" width="300" height="28" fill="#f8fafc"/>
  <text x="75"  y="189" text-anchor="middle" font-size="9" fill="#1e293b">Ilustr. muy compleja</text>
  <text x="200" y="189" text-anchor="middle" font-size="9" fill="#f59e0b" font-weight="600">Depende del peso</text>

  <!-- Línea separadora columnas -->
  <line x1="150" y1="0" x2="150" y2="200" stroke="#e2e8f0" stroke-width="1"/>
</svg>
```
