# 4.1 Rectángulo `<rect>`

El rectángulo es la forma más usada en SVG. Se posiciona por su **esquina superior izquierda** (`x`, `y`) y se dimensiona con `width` y `height`.

---

## Atributos de posición y tamaño

`x` e `y` definen dónde empieza el rectángulo. Si se omiten, valen `0` (esquina superior izquierda del viewport).

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- En el origen (x=0, y=0) -->
  <rect x="0" y="0" width="80" height="50" fill="#3b82f6" opacity="0.8"/>
  <text x="40" y="65" text-anchor="middle" font-size="8" fill="#64748b">x=0, y=0</text>

  <!-- Desplazado -->
  <rect x="110" y="20" width="80" height="50" fill="#10b981" opacity="0.8"/>
  <text x="150" y="82" text-anchor="middle" font-size="8" fill="#64748b">x=110, y=20</text>

  <!-- Más abajo y a la derecha -->
  <rect x="210" y="50" width="80" height="50" fill="#f59e0b" opacity="0.8"/>
  <text x="250" y="112" text-anchor="middle" font-size="8" fill="#64748b">x=210, y=50</text>

  <!-- Guías de origen -->
  <circle cx="0"   cy="0"  r="4" fill="tomato"/>
  <circle cx="110" cy="20" r="4" fill="tomato"/>
  <circle cx="210" cy="50" r="4" fill="tomato"/>
  <text x="6"   y="14" font-size="7" fill="tomato">(0,0)</text>
  <text x="116" y="34" font-size="7" fill="tomato">(110,20)</text>
  <text x="216" y="64" font-size="7" fill="tomato">(210,50)</text>

  <text x="150" y="132" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    El punto rojo es la esquina superior izquierda (x, y)
  </text>
</svg>
```

---

## Esquinas redondeadas: `rx` y `ry`

`rx` define el radio horizontal de la curva de esquina. Si solo se especifica `rx`, `ry` toma el mismo valor automáticamente.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- rx=0 (por defecto) -->
  <rect x="10" y="20" width="60" height="60" fill="#3b82f6" rx="0"/>
  <text x="40" y="96" text-anchor="middle" font-size="8" fill="#64748b">rx=0</text>

  <!-- rx=8 -->
  <rect x="85" y="20" width="60" height="60" fill="#3b82f6" rx="8"/>
  <text x="115" y="96" text-anchor="middle" font-size="8" fill="#64748b">rx=8</text>

  <!-- rx=20 -->
  <rect x="160" y="20" width="60" height="60" fill="#3b82f6" rx="20"/>
  <text x="190" y="96" text-anchor="middle" font-size="8" fill="#64748b">rx=20</text>

  <!-- rx=30 = width/2 = height/2 → elipse/círculo -->
  <rect x="235" y="20" width="60" height="60" fill="#3b82f6" rx="30"/>
  <text x="265" y="96" text-anchor="middle" font-size="8" fill="#64748b">rx=30</text>
  <text x="265" y="108" text-anchor="middle" font-size="7" fill="#94a3b8">(=w/2 → círculo)</text>

  <text x="150" y="124" text-anchor="middle"
        font-size="8" fill="#94a3b8">
    Al aumentar rx, las esquinas se redondean progresivamente
  </text>
</svg>
```

---

## `rx` y `ry` diferentes — esquinas elípticas

Cuando `rx ≠ ry`, las esquinas son arcos elípticos en lugar de circulares.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- rx=rx=ry (esquina semicircular) -->
  <rect x="10" y="20" width="80" height="60" fill="#6366f1" rx="15" ry="15"/>
  <text x="50" y="94" text-anchor="middle" font-size="8" fill="#64748b">rx=ry=15</text>
  <text x="50" y="104" text-anchor="middle" font-size="7" fill="#94a3b8">esquina circular</text>

  <!-- rx mayor -->
  <rect x="110" y="20" width="80" height="60" fill="#8b5cf6" rx="30" ry="8"/>
  <text x="150" y="94" text-anchor="middle" font-size="8" fill="#64748b">rx=30, ry=8</text>
  <text x="150" y="104" text-anchor="middle" font-size="7" fill="#94a3b8">ancho, achatada</text>

  <!-- ry mayor -->
  <rect x="210" y="20" width="80" height="60" fill="#a855f7" rx="8" ry="25"/>
  <text x="250" y="94" text-anchor="middle" font-size="8" fill="#64748b">rx=8, ry=25</text>
  <text x="250" y="104" text-anchor="middle" font-size="7" fill="#94a3b8">alta, alargada</text>
</svg>
```

---

## Variantes comunes: cuadrado, píldora, tarjeta

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cuadrado -->
  <rect x="10" y="20" width="60" height="60" fill="#3b82f6"/>
  <text x="40" y="95" text-anchor="middle" font-size="8" fill="#64748b">cuadrado</text>
  <text x="40" y="107" text-anchor="middle" font-size="7" fill="#94a3b8">w=h=60</text>

  <!-- Píldora (pill) -->
  <rect x="85" y="35" width="100" height="30" fill="#10b981" rx="15"/>
  <text x="135" y="80" text-anchor="middle" font-size="8" fill="#64748b">píldora (pill)</text>
  <text x="135" y="92" text-anchor="middle" font-size="7" fill="#94a3b8">rx = height/2</text>

  <!-- Tarjeta UI -->
  <rect x="200" y="15" width="90" height="70" fill="#1e293b" rx="10"/>
  <rect x="208" y="23" width="74" height="40" fill="#334155" rx="6"/>
  <rect x="208" y="68" width="34" height="10" fill="#3b82f6" rx="3"/>
  <rect x="248" y="68" width="26" height="10" fill="#475569" rx="3"/>
  <text x="245" y="102" text-anchor="middle" font-size="8" fill="#64748b">tarjeta UI</text>
  <text x="245" y="114" text-anchor="middle" font-size="7" fill="#94a3b8">rx=10</text>

  <!-- Botón -->
  <rect x="10" y="120" width="120" height="30" fill="#3b82f6" rx="6"/>
  <text x="70" y="140" text-anchor="middle" font-size="11" fill="white"
        font-weight="600">Guardar</text>
  <text x="70" y="160" text-anchor="middle" font-size="8" fill="#64748b">botón (rx=6)</text>

  <!-- Badge redondeado -->
  <rect x="160" y="125" width="40" height="20" fill="#ef4444" rx="10"/>
  <text x="180" y="139" text-anchor="middle" font-size="9" fill="white"
        font-weight="700">99</text>
  <text x="220" y="132" font-size="8" fill="#64748b">badge</text>
  <text x="220" y="144" font-size="8" fill="#94a3b8">rx=10</text>
</svg>
```

---

## El stroke se dibuja centrado sobre el borde

Un `stroke-width="10"` se reparte 5px hacia afuera y 5px hacia adentro del borde geométrico.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Rect base sin stroke (borde real) -->
  <rect x="60" y="20" width="180" height="80"
        fill="#eff6ff" stroke="none"/>
  <!-- Borde geométrico real -->
  <rect x="60" y="20" width="180" height="80"
        fill="none" stroke="#cbd5e1" stroke-width="1" stroke-dasharray="4,2"/>

  <!-- Stroke grueso centrado -->
  <rect x="60" y="20" width="180" height="80"
        fill="#3b82f6" opacity="0.15"
        stroke="#3b82f6" stroke-width="14"/>

  <!-- Anotaciones -->
  <line x1="53" y1="40" x2="60" y2="40" stroke="#ef4444" stroke-width="1"/>
  <text x="50" y="38" text-anchor="end" font-size="7" fill="#ef4444">7px</text>
  <text x="50" y="48" text-anchor="end" font-size="7" fill="#ef4444">afuera</text>

  <line x1="60" y1="55" x2="67" y2="55" stroke="#10b981" stroke-width="1"/>
  <text x="72" y="53" font-size="7" fill="#10b981">7px</text>
  <text x="72" y="63" font-size="7" fill="#10b981">adentro</text>

  <text x="150" y="122" text-anchor="middle"
        font-size="8" fill="#64748b">
    stroke-width=14 → 7px hacia afuera, 7px hacia adentro
  </text>
</svg>
```
