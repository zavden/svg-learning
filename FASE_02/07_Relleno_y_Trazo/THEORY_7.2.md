# 7.2 Propiedades de trazo (`stroke`)

El **stroke** es el trazo dibujado sobre el borde geométrico. Por defecto es `none`. Se pinta **centrado** sobre el borde: mitad hacia dentro, mitad hacia fuera.

---

## `stroke` y `stroke-width`

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin stroke: solo el fill visible -->
  <rect x="10" y="20" width="60" height="60" fill="#dbeafe"/>
  <text x="40" y="95" text-anchor="middle" font-size="7.5" fill="#64748b">sin stroke</text>

  <!-- Con stroke fino -->
  <rect x="90" y="20" width="60" height="60" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="120" y="95" text-anchor="middle" font-size="7.5" fill="#3b82f6">sw=1</text>

  <!-- Stroke medio -->
  <rect x="170" y="20" width="60" height="60" fill="#dbeafe" stroke="#3b82f6" stroke-width="5"/>
  <text x="200" y="95" text-anchor="middle" font-size="7.5" fill="#3b82f6">sw=5</text>

  <!-- Stroke grueso -->
  <rect x="245" y="30" width="50" height="50" fill="#dbeafe" stroke="#3b82f6" stroke-width="14"/>
  <text x="270" y="95" text-anchor="middle" font-size="7.5" fill="#3b82f6">sw=14</text>

  <text x="150" y="115" text-anchor="middle" font-size="7.5" fill="#64748b">
    stroke-width controla el grosor del trazo
  </text>
  <text x="150" y="127" text-anchor="middle" font-size="7" fill="#94a3b8">
    el stroke se pinta encima del fill
  </text>
</svg>
```

---

## El stroke se centra sobre el borde

La mitad del stroke está **dentro** de la forma y la mitad **fuera**. Esto afecta el área visual y puede causar recorte en los bordes del `viewBox`.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Rectángulo base (borde geométrico) -->
  <rect x="60" y="20" width="180" height="80" fill="#eff6ff" stroke="none"/>
  <!-- Borde geométrico real (punteado) -->
  <rect x="60" y="20" width="180" height="80" fill="none" stroke="#94a3b8" stroke-width="1" stroke-dasharray="3,2"/>

  <!-- Stroke grueso centrado (sw=16 → 8 dentro + 8 fuera) -->
  <rect x="60" y="20" width="180" height="80"
        fill="#3b82f6" fill-opacity="0.15"
        stroke="#3b82f6" stroke-width="16"/>

  <!-- Anotaciones: afuera -->
  <line x1="44" y1="45" x2="60" y2="45" stroke="#ef4444" stroke-width="1.5"/>
  <text x="40" y="43" text-anchor="end" font-size="7" fill="#ef4444">8px</text>
  <text x="40" y="53" text-anchor="end" font-size="7" fill="#ef4444">afuera</text>

  <!-- Anotaciones: adentro -->
  <line x1="60" y1="65" x2="68" y2="65" stroke="#10b981" stroke-width="1.5"/>
  <text x="72" y="63" font-size="7" fill="#10b981">8px</text>
  <text x="72" y="73" font-size="7" fill="#10b981">adentro</text>

  <text x="150" y="118" text-anchor="middle" font-size="8" fill="#1e293b">stroke-width=16</text>
  <text x="150" y="130" text-anchor="middle" font-size="7.5" fill="#64748b">
    8px hacia afuera + 8px hacia adentro del borde
  </text>
</svg>
```

---

## `stroke-opacity`

Controla solo la opacidad del trazo, sin afectar el fill.

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="10"  y="15" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="8" stroke-opacity="1.0"/>
  <rect x="80"  y="15" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="8" stroke-opacity="0.7"/>
  <rect x="150" y="15" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="8" stroke-opacity="0.4"/>
  <rect x="220" y="15" width="55" height="55" fill="#dbeafe" stroke="#3b82f6" stroke-width="8" stroke-opacity="0.1"/>

  <text x="37"  y="83" text-anchor="middle" font-size="7.5" fill="#64748b">s-op=1</text>
  <text x="107" y="83" text-anchor="middle" font-size="7.5" fill="#64748b">s-op=0.7</text>
  <text x="177" y="83" text-anchor="middle" font-size="7.5" fill="#64748b">s-op=0.4</text>
  <text x="247" y="83" text-anchor="middle" font-size="7.5" fill="#64748b">s-op=0.1</text>

  <text x="150" y="97" text-anchor="middle" font-size="7" fill="#94a3b8">el fill permanece opaco</text>
</svg>
```

---

## `stroke-width="0"` — trazo invisible sin borrar el fill

A diferencia de `stroke="none"`, `stroke-width="0"` mantiene el stroke definido pero invisible.

```svg
<svg width="300" height="80"
     viewBox="0 0 300 80"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="20" y="15" width="60" height="50" fill="#3b82f6" stroke="none"/>
  <text x="50" y="73" text-anchor="middle" font-size="7.5" fill="#64748b">stroke="none"</text>

  <rect x="120" y="15" width="60" height="50" fill="#3b82f6" stroke="#ef4444" stroke-width="0"/>
  <text x="150" y="73" text-anchor="middle" font-size="7.5" fill="#64748b">stroke-width=0</text>

  <rect x="220" y="15" width="60" height="50" fill="#3b82f6" stroke="#ef4444" stroke-width="4"/>
  <text x="250" y="73" text-anchor="middle" font-size="7.5" fill="#64748b">stroke-width=4</text>
</svg>
```
