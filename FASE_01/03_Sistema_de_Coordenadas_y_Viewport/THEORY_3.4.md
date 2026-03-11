# 3.4 Sistema de coordenadas del usuario

El sistema de coordenadas SVG tiene dos particularidades que sorprenden a quienes vienen de matemáticas o de otros sistemas gráficos:

1. El **origen (0,0) está en la esquina superior izquierda**, no en el centro ni en la esquina inferior.
2. **El eje Y crece hacia abajo**, al contrario que en matemáticas (donde Y crece hacia arriba). Esto es consistente con HTML/CSS y con la mayoría de APIs gráficas de pantalla.

Cada elemento SVG usa este sistema para definir su posición. Pero la semántica de `x, y` varía según el elemento: en `<rect>` es la esquina superior izquierda; en `<circle>` es el centro; en `<text>` es el inicio de la línea base.

---

## Ejemplo 1: El eje Y está invertido respecto a las matemáticas

En matemáticas, valores positivos de Y suben. En SVG, valores positivos de Y **bajan**. Este ejemplo compara ambos sistemas con los mismos valores numéricos.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Comparación visual: eje Y en SVG (izquierda) vs matemáticas (derecha) -->

  <!-- === SVG: Y crece hacia ABAJO === -->
  <text x="60" y="18" text-anchor="middle"
        font-size="11" fill="#3b82f6">Sistema SVG</text>

  <!-- Eje Y apuntando hacia abajo -->
  <line x1="60" y1="28" x2="60" y2="185"
        stroke="#3b82f6" stroke-width="2"/>
  <polygon points="57,183 60,191 63,183" fill="#3b82f6"/>
  <text x="64" y="194" font-size="9" fill="#3b82f6">+Y</text>

  <!-- Marcadores de Y -->
  <line x1="55" y1="28"  x2="65" y2="28"  stroke="#3b82f6" stroke-width="1"/>
  <line x1="55" y1="68"  x2="65" y2="68"  stroke="#3b82f6" stroke-width="1"/>
  <line x1="55" y1="108" x2="65" y2="108" stroke="#3b82f6" stroke-width="1"/>
  <line x1="55" y1="148" x2="65" y2="148" stroke="#3b82f6" stroke-width="1"/>
  <text x="32" y="32"  font-size="9" fill="#3b82f6">y=0</text>
  <text x="32" y="72"  font-size="9" fill="#3b82f6">y=40</text>
  <text x="32" y="112" font-size="9" fill="#3b82f6">y=80</text>
  <text x="32" y="152" font-size="9" fill="#3b82f6">y=120</text>

  <!-- Punto de ejemplo: (0, 40) está ABAJO del (0, 0) -->
  <circle cx="60" cy="68" r="5" fill="#3b82f6"/>
  <text x="70" y="72" font-size="9" fill="#3b82f6">(0, 40) — más abajo</text>

  <!-- Separador vertical -->
  <line x1="140" y1="15" x2="140" y2="195"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- === Matemáticas: Y crece hacia ARRIBA === -->
  <text x="210" y="18" text-anchor="middle"
        font-size="11" fill="#10b981">Matemáticas</text>

  <!-- Eje Y apuntando hacia arriba -->
  <line x1="210" y1="185" x2="210" y2="28"
        stroke="#10b981" stroke-width="2"/>
  <polygon points="207,30 210,22 213,30" fill="#10b981"/>
  <text x="214" y="22" font-size="9" fill="#10b981">+Y</text>

  <!-- Marcadores de Y (invertidos visualmente) -->
  <line x1="205" y1="185" x2="215" y2="185"
        stroke="#10b981" stroke-width="1"/>
  <line x1="205" y1="145" x2="215" y2="145"
        stroke="#10b981" stroke-width="1"/>
  <line x1="205" y1="105" x2="215" y2="105"
        stroke="#10b981" stroke-width="1"/>
  <line x1="205" y1="65"  x2="215" y2="65"
        stroke="#10b981" stroke-width="1"/>
  <text x="218" y="189" font-size="9" fill="#10b981">y=0</text>
  <text x="218" y="149" font-size="9" fill="#10b981">y=40</text>
  <text x="218" y="109" font-size="9" fill="#10b981">y=80</text>
  <text x="218" y="69"  font-size="9" fill="#10b981">y=120</text>

  <!-- Punto de ejemplo: (0, 40) está ARRIBA del (0, 0) -->
  <circle cx="210" cy="145" r="5" fill="#10b981"/>
  <text x="152" y="149" font-size="9" fill="#10b981">(0,40) — más arriba</text>
</svg>
```

---

## Ejemplo 2: x, y en `<rect>` — esquina superior izquierda

En `<rect>`, los atributos `x` e `y` definen la **esquina superior izquierda** del rectángulo. Aumentar `x` mueve el rect a la derecha; aumentar `y` lo mueve hacia abajo.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- x, y en rect = coordenada de la ESQUINA SUPERIOR IZQUIERDA -->
  <!-- El punto marcado con un círculo rojo es (x, y) exactamente -->

  <!-- Cuadrícula de referencia -->
  <line x1="0" y1="50"  x2="280" y2="50"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0" y1="100" x2="280" y2="100"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="0" y1="150" x2="280" y2="150"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="50"  y1="0" x2="50"  y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="100" y1="0" x2="100" y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="150" y1="0" x2="150" y2="200"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="200" y1="0" x2="200" y2="200"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- Rect 1: x=20, y=20 -->
  <rect x="20" y="20" width="60" height="40"
        fill="#3b82f6" opacity="0.8" rx="2"/>
  <circle cx="20" cy="20" r="5" fill="tomato"/>
  <text x="22" y="16" font-size="9" fill="tomato">x=20, y=20</text>

  <!-- Rect 2: x=110, y=60 -->
  <rect x="110" y="60" width="80" height="50"
        fill="#10b981" opacity="0.8" rx="2"/>
  <circle cx="110" cy="60" r="5" fill="tomato"/>
  <text x="112" y="56" font-size="9" fill="tomato">x=110, y=60</text>

  <!-- Rect 3: x=50, y=130 -->
  <rect x="50" y="130" width="70" height="45"
        fill="#f59e0b" opacity="0.8" rx="2"/>
  <circle cx="50" cy="130" r="5" fill="tomato"/>
  <text x="52" y="126" font-size="9" fill="tomato">x=50, y=130</text>

  <!-- Rect 4: x=200, y=100 -->
  <rect x="200" y="100" width="65" height="65"
        fill="#a855f7" opacity="0.8" rx="2"/>
  <circle cx="200" cy="100" r="5" fill="tomato"/>
  <text x="202" y="96" font-size="9" fill="tomato">x=200, y=100</text>

  <!-- Leyenda -->
  <circle cx="10" cy="190" r="4" fill="tomato"/>
  <text x="18" y="194" font-size="9" fill="#64748b">
    = punto (x, y) — siempre la esquina superior izquierda del rect
  </text>
</svg>
```

---

## Ejemplo 3: cx, cy en `<circle>` — el centro del círculo

En `<circle>` y `<ellipse>`, `cx` y `cy` definen el **centro** del elemento, no una esquina. Esto es diferente de `<rect>`. El radio `r` se extiende en todas direcciones desde ese centro.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- cx, cy en circle = coordenada del CENTRO del círculo -->
  <!-- El punto marcado es (cx, cy): el centro exacto -->

  <!-- Cuadrícula de referencia -->
  <line x1="0"   y1="100" x2="280" y2="100"
        stroke="#e2e8f0" stroke-width="1"/>
  <line x1="140" y1="0"   x2="140" y2="200"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- Círculo 1: cx=60, cy=60, r=40 -->
  <circle cx="60" cy="60" r="40" fill="#3b82f6" opacity="0.7"/>
  <!-- Líneas que muestran cx y cy -->
  <line x1="20" y1="60" x2="100" y2="60"
        stroke="#1d4ed8" stroke-width="1" stroke-dasharray="3,2"/>
  <line x1="60" y1="20" x2="60" y2="100"
        stroke="#1d4ed8" stroke-width="1" stroke-dasharray="3,2"/>
  <circle cx="60" cy="60" r="4" fill="tomato"/>
  <text x="65" y="57" font-size="9" fill="tomato">cx=60, cy=60</text>
  <text x="65" y="68" font-size="9" fill="white">r=40</text>

  <!-- Círculo 2: cx=200, cy=80, r=50 -->
  <circle cx="200" cy="80" r="50" fill="#10b981" opacity="0.7"/>
  <line x1="150" y1="80" x2="250" y2="80"
        stroke="#065f46" stroke-width="1" stroke-dasharray="3,2"/>
  <line x1="200" y1="30" x2="200" y2="130"
        stroke="#065f46" stroke-width="1" stroke-dasharray="3,2"/>
  <circle cx="200" cy="80" r="4" fill="tomato"/>
  <text x="205" y="77" font-size="9" fill="tomato">cx=200, cy=80</text>
  <text x="205" y="88" font-size="9" fill="white">r=50</text>

  <!-- Círculo 3: cx=100, cy=160, r=30 -->
  <circle cx="100" cy="160" r="30" fill="#f59e0b" opacity="0.7"/>
  <line x1="70" y1="160" x2="130" y2="160"
        stroke="#92400e" stroke-width="1" stroke-dasharray="3,2"/>
  <line x1="100" y1="130" x2="100" y2="190"
        stroke="#92400e" stroke-width="1" stroke-dasharray="3,2"/>
  <circle cx="100" cy="160" r="4" fill="tomato"/>
  <text x="105" y="157" font-size="9" fill="tomato">cx=100, cy=160</text>
  <text x="105" y="168" font-size="9" fill="white">r=30</text>

  <!-- Leyenda -->
  <circle cx="10" cy="192" r="4" fill="tomato"/>
  <text x="18" y="196" font-size="9" fill="#64748b">
    = punto (cx, cy) — siempre el CENTRO del círculo
  </text>
</svg>
```

---

## Ejemplo 4: x, y en `<text>` — la línea base del texto

En `<text>`, los atributos `x` e `y` no definen la esquina superior izquierda de la caja de texto. Definen el **inicio de la línea base** (baseline) del texto. La línea base es donde "apoyan" las letras; los descendentes (g, p, y...) quedan por debajo de ella.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- x, y en text = inicio de la LÍNEA BASE del texto -->
  <!-- La línea base es donde "apoyan" las letras mayúsculas y minúsculas -->
  <!-- Los descendentes (g, p, q, y) quedan POR DEBAJO de la baseline -->

  <!-- Ejemplo 1: texto simple con baseline marcada -->
  <line x1="10" y1="60" x2="270" y2="60"
        stroke="#fbbf24" stroke-width="1.5"/>
  <text x="20" y="60" font-size="28" fill="#3b82f6">
    Hg Tipografía
  </text>
  <!-- El punto (x, y) = (20, 60) -->
  <circle cx="20" cy="60" r="5" fill="tomato"/>
  <text x="22" y="50" font-size="9" fill="tomato">x=20, y=60</text>
  <text x="195" y="56" font-size="8" fill="#f59e0b">← baseline</text>

  <!-- Las "g" descienden por debajo de la baseline -->
  <line x1="10" y1="78" x2="270" y2="78"
        stroke="#e2e8f0" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="195" y="86" font-size="8" fill="#94a3b8">← descendentes</text>

  <!-- Ejemplo 2: text-anchor cambia el punto de referencia horizontal -->
  <line x1="10" y1="140" x2="270" y2="140"
        stroke="#fbbf24" stroke-width="1.5"/>

  <!-- text-anchor="start" (por defecto): x es el inicio del texto -->
  <text x="20" y="140" font-size="14"
        text-anchor="start" fill="#10b981">start</text>
  <circle cx="20" cy="140" r="4" fill="tomato"/>

  <!-- text-anchor="middle": x es el centro del texto -->
  <text x="140" y="140" font-size="14"
        text-anchor="middle" fill="#3b82f6">middle</text>
  <circle cx="140" cy="140" r="4" fill="tomato"/>

  <!-- text-anchor="end": x es el final del texto -->
  <text x="260" y="140" font-size="14"
        text-anchor="end" fill="#f59e0b">end</text>
  <circle cx="260" cy="140" r="4" fill="tomato"/>

  <text x="140" y="165" text-anchor="middle"
        font-size="9" fill="#64748b">
    text-anchor controla qué parte del texto queda en (x, y)
  </text>
  <text x="140" y="180" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    start (default) | middle | end
  </text>
</svg>
```

---

## Ejemplo 5: Coordenadas absolutas vs relativas en `<path>`

En `<path>`, los comandos en **mayúscula** (M, L, C...) usan coordenadas absolutas respecto al origen del SVG. Los comandos en **minúscula** (m, l, c...) usan coordenadas relativas al punto actual del trazado. Ambos producen el mismo resultado visual si los valores son correctos.

```svg
<svg width="280" height="200"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">
  <!-- Comparación: coordenadas ABSOLUTAS (mayúsculas) vs RELATIVAS (minúsculas) -->
  <!-- Ambos paths dibujan la misma figura en distintas posiciones -->

  <!-- Etiquetas de sección -->
  <text x="70" y="16" text-anchor="middle"
        font-size="11" fill="#3b82f6">Absolutas (M, L)</text>
  <text x="210" y="16" text-anchor="middle"
        font-size="11" fill="#10b981">Relativas (m, l)</text>
  <line x1="140" y1="12" x2="140" y2="188"
        stroke="#e2e8f0" stroke-width="1"/>

  <!-- Path ABSOLUTO: todas las coords son respecto al origen (0,0) del SVG -->
  <!-- M=moveTo absoluto, L=lineTo absoluto -->
  <path d="M 20 40 L 120 40 L 120 140 L 20 140 Z"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="2"/>
  <path d="M 45 65 L 95 65 L 95 115 L 45 115 Z"
        fill="#3b82f6" opacity="0.8"/>
  <circle cx="20" cy="40" r="4" fill="tomato"/>
  <text x="22" y="36" font-size="8" fill="tomato">M 20,40</text>
  <text x="70" y="170" text-anchor="middle"
        font-size="9" fill="#3b82f6">M 20 40 L 120 40 L 120 140...</text>

  <!-- Path RELATIVO: las coords son relativas al punto anterior -->
  <!-- m=moveTo relativo al punto actual, l=lineTo relativo al punto actual -->
  <!-- Empieza en (160,40), luego se mueve +100 en X, +100 en Y, etc. -->
  <path d="M 160 40 l 100 0 l 0 100 l -100 0 Z"
        fill="#d1fae5" stroke="#10b981" stroke-width="2"/>
  <path d="M 185 65 l 50 0 l 0 50 l -50 0 Z"
        fill="#10b981" opacity="0.8"/>
  <circle cx="160" cy="40" r="4" fill="tomato"/>
  <text x="162" y="36" font-size="8" fill="tomato">M 160,40</text>
  <text x="210" y="170" text-anchor="middle"
        font-size="9" fill="#10b981">m 160 40 l 100 0 l 0 100...</text>

  <text x="140" y="190" text-anchor="middle"
        font-size="9" fill="#94a3b8">
    Mayúscula = absoluto desde (0,0) | minúscula = relativo al punto actual
  </text>
</svg>
```
