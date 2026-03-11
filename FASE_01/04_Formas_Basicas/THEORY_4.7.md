# 4.7 Atributos de presentación

Los atributos de presentación controlan la apariencia visual de todos los elementos SVG. Se pueden aplicar directamente en el elemento o heredarse desde un grupo padre.

---

## Los atributos fundamentales

`fill`, `stroke`, `stroke-width` y `opacity` son los cuatro pilares.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- fill -->
  <rect x="10" y="10" width="60" height="50" fill="#3b82f6"/>
  <text x="40" y="75" text-anchor="middle" font-size="7.5" fill="#64748b">fill</text>
  <text x="40" y="86" text-anchor="middle" font-size="7" fill="#94a3b8">color de relleno</text>

  <!-- stroke + stroke-width -->
  <rect x="90" y="10" width="60" height="50" fill="none"
        stroke="#ef4444" stroke-width="4"/>
  <text x="120" y="75" text-anchor="middle" font-size="7.5" fill="#64748b">stroke</text>
  <text x="120" y="86" text-anchor="middle" font-size="7" fill="#94a3b8">+ stroke-width</text>

  <!-- fill + stroke combinados -->
  <rect x="170" y="10" width="60" height="50"
        fill="#10b981" stroke="#065f46" stroke-width="3"/>
  <text x="200" y="75" text-anchor="middle" font-size="7.5" fill="#64748b">fill + stroke</text>
  <text x="200" y="86" text-anchor="middle" font-size="7" fill="#94a3b8">combinados</text>

  <!-- opacity -->
  <rect x="10" y="100" width="60" height="35" fill="#6366f1"/>
  <rect x="30" y="105" width="60" height="35" fill="#f59e0b" opacity="0.6"/>
  <text x="60" y="128" text-anchor="middle" font-size="7.5" fill="#64748b" dy="10">opacity=0.6</text>

  <!-- fill-opacity y stroke-opacity independientes -->
  <rect x="170" y="100" width="115" height="35"
        fill="#ef4444" fill-opacity="0.3"
        stroke="#ef4444" stroke-width="3" stroke-opacity="1"/>
  <text x="227" y="128" text-anchor="middle" font-size="7" fill="#64748b" dy="10">fill-opacity ≠ stroke-opacity</text>
</svg>
```

---

## Valores de `fill` y `stroke`

Ambos aceptan los mismos tipos de valor.

```svg
<svg width="300" height="150"
     viewBox="0 0 300 150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <text x="150" y="15" text-anchor="middle" font-size="9" fill="#64748b">Tipos de valor para fill / stroke</text>

  <!-- Nombre CSS -->
  <rect x="10" y="25" width="55" height="35" fill="tomato"/>
  <text x="37" y="72" text-anchor="middle" font-size="7" fill="#64748b">nombre</text>
  <text x="37" y="82" text-anchor="middle" font-size="7" fill="#94a3b8">"tomato"</text>

  <!-- Hex -->
  <rect x="75" y="25" width="55" height="35" fill="#3b82f6"/>
  <text x="102" y="72" text-anchor="middle" font-size="7" fill="#64748b">hex</text>
  <text x="102" y="82" text-anchor="middle" font-size="7" fill="#94a3b8">"#3b82f6"</text>

  <!-- rgb() -->
  <rect x="140" y="25" width="55" height="35" fill="rgb(16,185,129)"/>
  <text x="167" y="72" text-anchor="middle" font-size="7" fill="#64748b">rgb()</text>
  <text x="167" y="82" text-anchor="middle" font-size="7" fill="#94a3b8">rgb(16,185,129)</text>

  <!-- none -->
  <rect x="205" y="25" width="55" height="35" fill="none" stroke="#94a3b8" stroke-width="1.5" stroke-dasharray="3,2"/>
  <text x="232" y="72" text-anchor="middle" font-size="7" fill="#64748b">none</text>
  <text x="232" y="82" text-anchor="middle" font-size="7" fill="#94a3b8">transparent</text>

  <!-- currentColor -->
  <rect x="10" y="100" width="80" height="35" fill="currentColor" style="color:#a855f7"/>
  <text x="50" y="145" text-anchor="middle" font-size="7" fill="#64748b">currentColor</text>

  <!-- url(#id) para gradientes -->
  <defs>
    <linearGradient id="grad47" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" stop-color="#3b82f6"/>
      <stop offset="100%" stop-color="#a855f7"/>
    </linearGradient>
  </defs>
  <rect x="105" y="100" width="180" height="35" fill="url(#grad47)" rx="4"/>
  <text x="195" y="145" text-anchor="middle" font-size="7" fill="#64748b">url(#id) — gradiente</text>
</svg>
```

---

## Herencia desde el grupo padre

Los atributos de presentación se **heredan** a los elementos hijos. Definirlos en `<g>` evita repetición.

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Sin herencia: cada elemento repite atributos -->
  <rect x="10" y="15" width="40" height="40"
        fill="#ef4444" stroke="#991b1b" stroke-width="2"/>
  <rect x="60" y="15" width="40" height="40"
        fill="#ef4444" stroke="#991b1b" stroke-width="2"/>
  <rect x="110" y="15" width="40" height="40"
        fill="#ef4444" stroke="#991b1b" stroke-width="2"/>
  <text x="80" y="72" text-anchor="middle" font-size="7.5" fill="#ef4444">sin herencia → repetir</text>

  <!-- Con herencia: atributos en el grupo -->
  <g fill="#3b82f6" stroke="#1e40af" stroke-width="2">
    <rect x="10" y="90" width="40" height="40"/>
    <rect x="60" y="90" width="40" height="40"/>
    <rect x="110" y="90" width="40" height="40"/>
  </g>
  <text x="80" y="140" text-anchor="middle" font-size="7.5" fill="#3b82f6">con &lt;g&gt; → herencia ✓</text>

  <!-- Sobreescribir herencia en un hijo -->
  <g fill="#10b981" stroke="#065f46" stroke-width="2">
    <rect x="175" y="10" width="40" height="40"/>
    <rect x="225" y="10" width="40" height="40" fill="#f59e0b" stroke="#92400e"/>
    <!-- El segundo sobreescribe -->
  </g>
  <text x="218" y="62" text-anchor="middle" font-size="7" fill="#64748b">hijo sobreescribe</text>
  <text x="218" y="72" text-anchor="middle" font-size="7" fill="#94a3b8">al grupo padre</text>

  <!-- opacity hereda de forma multiplicada -->
  <g opacity="0.5">
    <rect x="175" y="85" width="40" height="40" fill="#3b82f6"/>
    <rect x="225" y="85" width="40" height="40" fill="#ef4444" opacity="0.5"/>
    <!-- 0.5 × 0.5 = 0.25 efectiva -->
  </g>
  <text x="200" y="137" text-anchor="middle" font-size="7" fill="#64748b">g:opacity=0.5 × hijo:0.5 = 0.25</text>
</svg>
```

---

## CSS vs atributos de presentación: prioridad

El orden de especificidad de mayor a menor: `style=""` > hoja CSS > atributos de presentación.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Tabla de prioridad -->
  <rect x="10" y="10" width="280" height="20" rx="3" fill="#0f172a"/>
  <text x="150" y="24" text-anchor="middle" font-size="8.5" fill="white" font-family="monospace">
    style="" &gt; CSS class &gt; atributo de presentación
  </text>

  <!-- Atributo solo -->
  <rect x="20" y="42" width="70" height="35" fill="#ef4444"/>
  <text x="55" y="92" text-anchor="middle" font-size="7.5" fill="#64748b">atributo</text>
  <text x="55" y="103" text-anchor="middle" font-size="7" fill="#94a3b8">fill="#ef4444"</text>

  <!-- Atributo + CSS class (CSS gana) -->
  <style>.override { fill: #10b981; }</style>
  <rect x="110" y="42" width="70" height="35" fill="#ef4444" class="override"/>
  <text x="145" y="92" text-anchor="middle" font-size="7.5" fill="#64748b">CSS gana</text>
  <text x="145" y="103" text-anchor="middle" font-size="7" fill="#94a3b8">class sobreescribe</text>

  <!-- Atributo + CSS + style="" (style gana) -->
  <rect x="200" y="42" width="70" height="35" fill="#ef4444" class="override" style="fill:#a855f7"/>
  <text x="235" y="92" text-anchor="middle" font-size="7.5" fill="#64748b">style gana</text>
  <text x="235" y="103" text-anchor="middle" font-size="7" fill="#94a3b8">inline &gt; todo</text>

  <!-- Flechas de prioridad -->
  <text x="97"  y="65" text-anchor="middle" font-size="10" fill="#64748b">→</text>
  <text x="187" y="65" text-anchor="middle" font-size="10" fill="#64748b">→</text>

  <text x="150" y="120" text-anchor="middle" font-size="8" fill="#94a3b8">
    Misma regla que CSS en HTML
  </text>
</svg>
```

---

## `opacity` vs `fill-opacity` + `stroke-opacity`

`opacity` afecta el elemento completo (fill + stroke juntos). `fill-opacity` y `stroke-opacity` son independientes.

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Referencia sin transparencia -->
  <rect x="10" y="15" width="60" height="60"
        fill="#3b82f6" stroke="#1e40af" stroke-width="6"/>
  <text x="40" y="90" text-anchor="middle" font-size="8" fill="#64748b">sin opacity</text>

  <!-- opacity afecta todo: fill y stroke al mismo tiempo -->
  <rect x="90" y="15" width="60" height="60"
        fill="#3b82f6" stroke="#1e40af" stroke-width="6" opacity="0.4"/>
  <text x="120" y="90" text-anchor="middle" font-size="8" fill="#64748b">opacity=0.4</text>
  <text x="120" y="101" text-anchor="middle" font-size="7" fill="#94a3b8">todo a 0.4</text>

  <!-- fill-opacity solo afecta el relleno -->
  <rect x="170" y="15" width="60" height="60"
        fill="#3b82f6" fill-opacity="0.2"
        stroke="#1e40af" stroke-width="6" stroke-opacity="1"/>
  <text x="200" y="90" text-anchor="middle" font-size="8" fill="#64748b">fill-opacity=0.2</text>
  <text x="200" y="101" text-anchor="middle" font-size="7" fill="#94a3b8">stroke opaco</text>
</svg>
```
