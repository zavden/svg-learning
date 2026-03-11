# Referencia — Colores y Opacidad

---

## Formatos de color — conversión visual

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">El mismo color — distintos formatos</text>

  <!-- El mismo azul en todos los formatos -->
  <rect x="10" y="30" width="40" height="40" fill="steelblue"/>
  <text x="30" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">steelblue</text>

  <rect x="58" y="30" width="40" height="40" fill="#4682b4"/>
  <text x="78" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">#4682b4</text>

  <rect x="106" y="30" width="40" height="40" fill="rgb(70,130,180)"/>
  <text x="126" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">rgb(70,130,180)</text>

  <rect x="154" y="30" width="40" height="40" fill="rgba(70,130,180,1)"/>
  <text x="174" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">rgba(...,1)</text>

  <rect x="202" y="30" width="40" height="40" fill="hsl(207,44%,49%)"/>
  <text x="222" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">hsl(207,44%,49%)</text>

  <text x="150" y="100" text-anchor="middle" font-size="7.5" fill="#64748b">
    Todos producen exactamente el mismo color
  </text>
  <text x="150" y="114" text-anchor="middle" font-size="7" fill="#94a3b8">
    nombre → fácil | hex → herramientas | hsl → variaciones manuales
  </text>
  <text x="150" y="126" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    rgba/hsla → con canal alfa para semitransparencia
  </text>
</svg>
```

---

## Variantes HSL — crear paleta de un color

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">hsl(220, 70%, L%) — variando solo Lightness</text>

  <rect x="5"   y="27" width="42" height="45" fill="hsl(220, 70%, 15%)"/>
  <rect x="50"  y="27" width="42" height="45" fill="hsl(220, 70%, 25%)"/>
  <rect x="95"  y="27" width="42" height="45" fill="hsl(220, 70%, 40%)"/>
  <rect x="140" y="27" width="42" height="45" fill="hsl(220, 70%, 55%)"/>
  <rect x="185" y="27" width="42" height="45" fill="hsl(220, 70%, 70%)"/>
  <rect x="230" y="27" width="42" height="45" fill="hsl(220, 70%, 85%)"/>
  <rect x="275" y="27" width="22" height="45" fill="hsl(220, 70%, 95%)"/>

  <text x="26"  y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=15</text>
  <text x="71"  y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=25</text>
  <text x="116" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=40</text>
  <text x="161" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=55</text>
  <text x="206" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=70</text>
  <text x="251" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=85</text>
  <text x="286" y="82" text-anchor="middle" font-size="6.5" fill="#64748b">L=95</text>

  <text x="150" y="96" text-anchor="middle" font-size="7" fill="#94a3b8">
    Misma H y S → paleta cohesiva de oscuro a claro
  </text>
</svg>
```

---

## Los tres niveles de opacidad — cheat sheet

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Opacidad — tres propiedades, tres alcances</text>

  <!-- opacity -->
  <rect x="10" y="32" width="85" height="65" fill="#3b82f6" stroke="#ef4444" stroke-width="5" opacity="0.5"/>
  <text x="52" y="108" text-anchor="middle" font-size="7" fill="#64748b">opacity="0.5"</text>
  <text x="52" y="118" text-anchor="middle" font-size="6.5" fill="#94a3b8">fill + stroke + hijos</text>

  <!-- fill-opacity -->
  <rect x="110" y="32" width="85" height="65" fill="#3b82f6" stroke="#ef4444" stroke-width="5" fill-opacity="0.5"/>
  <text x="152" y="108" text-anchor="middle" font-size="7" fill="#64748b">fill-opacity="0.5"</text>
  <text x="152" y="118" text-anchor="middle" font-size="6.5" fill="#94a3b8">solo el fill</text>

  <!-- stroke-opacity -->
  <rect x="210" y="32" width="85" height="65" fill="#3b82f6" stroke="#ef4444" stroke-width="5" stroke-opacity="0.3"/>
  <text x="252" y="108" text-anchor="middle" font-size="7" fill="#64748b">stroke-opacity="0.3"</text>
  <text x="252" y="118" text-anchor="middle" font-size="6.5" fill="#94a3b8">solo el stroke</text>

  <text x="150" y="130" text-anchor="middle" font-size="6.5" fill="#94a3b8">rango 0 (invisible) → 1 (opaco)</text>
</svg>
```

---

## none vs transparent — decisión rápida

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Cuándo usar none vs transparent</text>

  <text x="10" y="36" font-size="7.5" fill="#3b82f6" font-weight="700">fill="none"</text>
  <text x="10" y="49" font-size="7" fill="#64748b">• Solo quiero ver el stroke (polyline, contornos)</text>
  <text x="10" y="61" font-size="7" fill="#64748b">• El interior no debe capturar eventos del mouse</text>
  <text x="10" y="73" font-size="7" fill="#64748b">• Elemento puramente decorativo sin interacción</text>

  <line x1="155" y1="22" x2="155" y2="90" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="36" font-size="7.5" fill="#10b981" font-weight="700">fill="transparent"</text>
  <text x="165" y="49" font-size="7" fill="#64748b">• Área de click invisible (botón, tooltip trigger)</text>
  <text x="165" y="61" font-size="7" fill="#64748b">• Hover sobre zona vacía debe activarse</text>
  <text x="165" y="73" font-size="7" fill="#64748b">• Zona de captura más grande que el área visual</text>
</svg>
```

---

## currentColor — patrón de uso

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">currentColor — flujo de herencia</text>

  <!-- Flujo visual -->
  <rect x="15" y="30" width="70" height="30" rx="4" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="50" y="49" text-anchor="middle" font-size="7" fill="#1e40af">CSS: color: blue</text>

  <line x1="85" y1="45" x2="110" y2="45" stroke="#64748b" stroke-width="1"/>
  <polygon points="108,41 115,45 108,49" fill="#64748b"/>

  <rect x="115" y="30" width="80" height="30" rx="4" fill="#dcfce7" stroke="#10b981" stroke-width="1"/>
  <text x="155" y="46" text-anchor="middle" font-size="7" fill="#166534">SVG elemento</text>
  <text x="155" y="56" text-anchor="middle" font-size="6.5" fill="#166534">fill="currentColor"</text>

  <line x1="195" y1="45" x2="220" y2="45" stroke="#64748b" stroke-width="1"/>
  <polygon points="218,41 225,45 218,49" fill="#64748b"/>

  <rect x="225" y="30" width="60" height="30" rx="4" fill="#3b82f6"/>
  <text x="255" y="50" text-anchor="middle" font-size="7" fill="white">azul ✓</text>

  <text x="150" y="82" text-anchor="middle" font-size="7" fill="#64748b">
    HTML color → se hereda → SVG fill lo recoge
  </text>
</svg>
```

---

## Tabla de formatos de color

| Formato | Ejemplo | Con alfa | Notas |
|---|---|---|---|
| Nombre | `tomato`, `steelblue` | No | 140+ nombres, fácil de leer |
| Hex corto | `#f06` | `#f068` (CSS4) | Solo si cada par es idéntico |
| Hex largo | `#ff0066` | `#ff006680` (CSS4) | Más preciso |
| `rgb()` | `rgb(255, 0, 0)` | `rgba(255,0,0,.5)` | Valores 0–255 |
| `hsl()` | `hsl(0, 100%, 50%)` | `hsla(0,100%,50%,.5)` | Ideal para paletas |
| `currentColor` | `currentColor` | — | Hereda `color` CSS |
| `none` | `fill="none"` | — | Sin pintura |
| `transparent` | `fill="transparent"` | — | Invisible, clickeable |
