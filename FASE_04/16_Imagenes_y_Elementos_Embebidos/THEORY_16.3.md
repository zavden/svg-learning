# 16.3 SVGs anidados

Un `<svg>` puede contener otro `<svg>` como hijo. A diferencia de un `<g>`, el SVG anidado crea un **nuevo viewport** y, si se declara un `viewBox`, también un **nuevo sistema de coordenadas** aislado.

---

## SVG anidado vs grupo `<g>`

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">&lt;g&gt; vs &lt;svg&gt; anidado</text>

  <!-- Opción A: grupo <g> -->
  <rect x="15" y="35" width="130" height="170" fill="#f8fafc" stroke="#cbd5e1" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">&lt;g&gt;</text>
  <text x="80" y="64" text-anchor="middle" font-size="6" fill="#64748b">mismas coordenadas del padre</text>

  <g transform="translate(30, 80)">
    <circle cx="0" cy="0" r="18" fill="#3b82f6"/>
    <rect x="20" y="-15" width="30" height="30" fill="#ec4899"/>
  </g>
  <text x="80" y="145" text-anchor="middle" font-size="6" fill="#64748b">mismo sistema de coords</text>
  <text x="80" y="156" text-anchor="middle" font-size="6" fill="#64748b">sin viewport propio</text>
  <text x="80" y="175" text-anchor="middle" font-size="6" fill="#10b981">✓ ligero, transformable</text>
  <text x="80" y="186" text-anchor="middle" font-size="6" fill="#ef4444">✗ sin recorte automático</text>

  <!-- Opción B: <svg> anidado -->
  <rect x="155" y="35" width="130" height="170" fill="#f8fafc" stroke="#cbd5e1" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">&lt;svg&gt; anidado</text>
  <text x="220" y="64" text-anchor="middle" font-size="6" fill="#64748b">nuevo sistema de coords</text>

  <svg x="170" y="75" width="100" height="60" viewBox="0 0 100 60"
       style="overflow:visible">
    <rect width="100" height="60" fill="#f1f5f9" stroke="#94a3b8" stroke-dasharray="2,2"/>
    <circle cx="25" cy="30" r="18" fill="#3b82f6"/>
    <rect x="50" y="15" width="30" height="30" fill="#ec4899"/>
  </svg>
  <text x="220" y="148" text-anchor="middle" font-size="6" fill="#64748b">nuevo viewport en (170,75)</text>
  <text x="220" y="158" text-anchor="middle" font-size="6" fill="#64748b">coord 0,0 en su esquina</text>
  <text x="220" y="175" text-anchor="middle" font-size="6" fill="#10b981">✓ aislamiento de coordenadas</text>
  <text x="220" y="186" text-anchor="middle" font-size="6" fill="#10b981">✓ viewBox propio</text>
</svg>
```

---

## El SVG anidado tiene su propio `viewBox`

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">mismo contenido, distintos viewBox</text>

  <!-- Caja 1: viewBox normal -->
  <rect x="15" y="40" width="85" height="85" fill="white" stroke="#cbd5e1"/>
  <svg x="15" y="40" width="85" height="85" viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="40" fill="#3b82f6"/>
    <text x="50" y="58" text-anchor="middle" font-size="18" font-weight="bold" fill="white">A</text>
  </svg>
  <text x="57" y="140" text-anchor="middle" font-size="7" fill="#64748b">viewBox</text>
  <text x="57" y="150" text-anchor="middle" font-size="6" fill="#94a3b8">"0 0 100 100"</text>

  <!-- Caja 2: viewBox ampliado (zoom out) -->
  <rect x="110" y="40" width="85" height="85" fill="white" stroke="#cbd5e1"/>
  <svg x="110" y="40" width="85" height="85" viewBox="-50 -50 200 200">
    <circle cx="50" cy="50" r="40" fill="#10b981"/>
    <text x="50" y="58" text-anchor="middle" font-size="18" font-weight="bold" fill="white">A</text>
  </svg>
  <text x="152" y="140" text-anchor="middle" font-size="7" fill="#64748b">viewBox</text>
  <text x="152" y="150" text-anchor="middle" font-size="6" fill="#94a3b8">"-50 -50 200 200"</text>

  <!-- Caja 3: viewBox recortado (zoom in) -->
  <rect x="205" y="40" width="85" height="85" fill="white" stroke="#cbd5e1"/>
  <svg x="205" y="40" width="85" height="85" viewBox="25 25 50 50">
    <circle cx="50" cy="50" r="40" fill="#ec4899"/>
    <text x="50" y="58" text-anchor="middle" font-size="18" font-weight="bold" fill="white">A</text>
  </svg>
  <text x="247" y="140" text-anchor="middle" font-size="7" fill="#64748b">viewBox</text>
  <text x="247" y="150" text-anchor="middle" font-size="6" fill="#94a3b8">"25 25 50 50"</text>

  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#94a3b8">
    el mismo círculo, tres "cámaras" distintas
  </text>
</svg>
```

---

## Reutilización: mismo SVG anidado en varias posiciones

```svg
<svg width="300" height="230"
     viewBox="0 0 300 230"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">composición de diferentes escalas</text>

  <!-- "Mapa" grande -->
  <svg x="15" y="35" width="170" height="170" viewBox="0 0 200 200">
    <rect width="200" height="200" fill="#e0f2fe" stroke="#0ea5e9"/>
    <text x="100" y="20" text-anchor="middle" font-size="14" font-weight="bold" fill="#0369a1">mapa</text>

    <!-- "Ciudades" -->
    <circle cx="50" cy="80" r="5" fill="#ef4444"/>
    <text x="50" y="100" text-anchor="middle" font-size="10" fill="#1e293b">A</text>

    <circle cx="140" cy="60" r="5" fill="#ef4444"/>
    <text x="140" y="80" text-anchor="middle" font-size="10" fill="#1e293b">B</text>

    <circle cx="100" cy="160" r="5" fill="#ef4444"/>
    <text x="100" y="180" text-anchor="middle" font-size="10" fill="#1e293b">C</text>

    <line x1="50" y1="80" x2="140" y2="60" stroke="#0369a1" stroke-width="1" stroke-dasharray="3,2"/>
    <line x1="140" y1="60" x2="100" y2="160" stroke="#0369a1" stroke-width="1" stroke-dasharray="3,2"/>
  </svg>

  <!-- Detalle ampliado del mismo mapa: solo la ciudad A -->
  <svg x="195" y="35" width="90" height="90" viewBox="30 60 40 40">
    <rect x="30" y="60" width="40" height="40" fill="#e0f2fe" stroke="#0ea5e9"/>
    <circle cx="50" cy="80" r="5" fill="#ef4444"/>
    <text x="50" y="95" text-anchor="middle" font-size="5" fill="#1e293b">A</text>
    <!-- Detalles visibles solo en el zoom -->
    <line x1="42" y1="80" x2="58" y2="80" stroke="#0369a1" stroke-width="0.5"/>
    <line x1="50" y1="72" x2="50" y2="88" stroke="#0369a1" stroke-width="0.5"/>
    <circle cx="50" cy="80" r="2" fill="white" stroke="#ef4444" stroke-width="0.5"/>
  </svg>
  <text x="240" y="135" text-anchor="middle" font-size="6" fill="#64748b">detalle de A</text>

  <!-- Detalle ampliado de la ciudad B -->
  <svg x="195" y="140" width="90" height="80" viewBox="120 40 40 40">
    <rect x="120" y="40" width="40" height="40" fill="#e0f2fe" stroke="#0ea5e9"/>
    <circle cx="140" cy="60" r="5" fill="#ef4444"/>
    <text x="140" y="75" text-anchor="middle" font-size="5" fill="#1e293b">B</text>
    <line x1="132" y1="60" x2="148" y2="60" stroke="#0369a1" stroke-width="0.5"/>
    <line x1="140" y1="52" x2="140" y2="68" stroke="#0369a1" stroke-width="0.5"/>
  </svg>
  <text x="240" y="225" text-anchor="middle" font-size="6" fill="#64748b">detalle de B</text>
</svg>
```

---

## `overflow`: recortar o no al viewport del hijo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">overflow en SVG anidado</text>

  <!-- overflow hidden (por defecto en SVG anidado) -->
  <text x="80" y="45" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    overflow="hidden"
  </text>
  <text x="80" y="56" text-anchor="middle" font-size="6" fill="#64748b">(por defecto)</text>
  <rect x="30" y="65" width="100" height="80" fill="white" stroke="#cbd5e1"/>
  <svg x="30" y="65" width="100" height="80" viewBox="0 0 100 80">
    <circle cx="90" cy="70" r="30" fill="#3b82f6"/>
    <circle cx="10" cy="15" r="20" fill="#ec4899"/>
  </svg>
  <text x="80" y="165" text-anchor="middle" font-size="6" fill="#64748b">contenido recortado</text>

  <!-- overflow visible -->
  <text x="220" y="45" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    overflow="visible"
  </text>
  <text x="220" y="56" text-anchor="middle" font-size="6" fill="#64748b">se muestra todo</text>
  <rect x="170" y="65" width="100" height="80" fill="white" stroke="#cbd5e1"/>
  <svg x="170" y="65" width="100" height="80" viewBox="0 0 100 80" overflow="visible">
    <circle cx="90" cy="70" r="30" fill="#10b981"/>
    <circle cx="10" cy="15" r="20" fill="#f59e0b"/>
  </svg>
  <text x="220" y="165" text-anchor="middle" font-size="6" fill="#64748b">rebasa el viewport</text>

  <text x="150" y="195" text-anchor="middle" font-size="7" fill="#94a3b8">
    un SVG anidado recorta al viewport automáticamente
  </text>
  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">
    útil cuando quieres ventanas estancas dentro del canvas
  </text>
  <text x="150" y="228" text-anchor="middle" font-size="6" fill="#94a3b8">
    (el SVG raíz por defecto también recorta, pero puede anularse con CSS)
  </text>
</svg>
```

---

## Aislamiento de coordenadas y transformaciones

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">viewBox aísla las coordenadas internas</text>

  <!-- Panel izquierdo: coordenadas del SVG padre (0-300) -->
  <text x="75" y="40" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    padre: 300×220
  </text>
  <rect x="15" y="50" width="120" height="150" fill="#f1f5f9" stroke="#94a3b8"/>
  <text x="25" y="65" font-size="6" fill="#64748b">(0,0) padre</text>
  <circle cx="25" cy="65" r="2" fill="#ef4444"/>
  <!-- el círculo azul está en (75, 125) del sistema padre -->
  <circle cx="75" cy="125" r="15" fill="#3b82f6"/>
  <text x="75" y="180" text-anchor="middle" font-size="6" fill="#64748b">elemento en</text>
  <text x="75" y="190" text-anchor="middle" font-size="6" fill="#64748b">padre(75, 125)</text>

  <!-- Panel derecho: SVG anidado con sus propias coordenadas -->
  <text x="225" y="40" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    anidado: 10×10
  </text>
  <svg x="165" y="50" width="120" height="150" viewBox="0 0 10 10">
    <rect width="10" height="10" fill="#f1f5f9" stroke="#94a3b8" stroke-width="0.1"/>
    <text x="0.5" y="1" font-size="0.6" fill="#64748b">(0,0)</text>
    <circle cx="0.5" cy="1" r="0.15" fill="#ef4444"/>
    <!-- El círculo azul está en (5, 5) del sistema interno -->
    <circle cx="5" cy="5" r="1.5" fill="#10b981"/>
    <text x="5" y="8" text-anchor="middle" font-size="0.7" fill="#64748b">(5, 5)</text>
  </svg>
  <text x="225" y="215" text-anchor="middle" font-size="6" fill="#64748b">mismo círculo lógico</text>

  <text x="150" y="232" text-anchor="middle" font-size="6" fill="#94a3b8">
    las coordenadas del hijo no "saben" que el padre es grande
  </text>
</svg>
```
