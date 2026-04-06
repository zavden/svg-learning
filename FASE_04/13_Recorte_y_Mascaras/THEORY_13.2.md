# 13.2 `clipPathUnits`

El atributo `clipPathUnits` controla en qué **sistema de coordenadas** están definidas las formas dentro del `<clipPath>`. Hay dos opciones: `userSpaceOnUse` (el espacio del SVG, por defecto) o `objectBoundingBox` (relativo al elemento recortado).

---

## `userSpaceOnUse` — coordenadas absolutas (por defecto)

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">userSpaceOnUse: posición fija</text>

  <defs>
    <!-- Círculo centrado en 80,90 del espacio del SVG -->
    <clipPath id="user-clip" clipPathUnits="userSpaceOnUse">
      <circle cx="80" cy="90" r="30"/>
    </clipPath>
  </defs>

  <!-- Guía visual: donde está el círculo de recorte -->
  <circle cx="80" cy="90" r="30" fill="none" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>

  <!-- Primer rect: el recorte cae dentro -->
  <rect x="50" y="60" width="60" height="60" fill="#3b82f6" clip-path="url(#user-clip)"/>
  <text x="80" y="135" text-anchor="middle" font-size="7" fill="#64748b">rect en 50,60</text>

  <!-- Segundo rect: a otra posición, pero el recorte sigue en 80,90 → casi no se ve -->
  <rect x="160" y="40" width="60" height="60" fill="#10b981" clip-path="url(#user-clip)"/>
  <circle cx="80" cy="90" r="30" fill="none" stroke="#ef4444" stroke-width="1" stroke-dasharray="3,2"/>
  <text x="190" y="110" text-anchor="middle" font-size="7" fill="#64748b">rect en 160,40</text>
  <text x="190" y="122" text-anchor="middle" font-size="7" fill="#ef4444">el recorte no llega</text>

  <text x="150" y="160" text-anchor="middle" font-size="8" fill="#94a3b8">
    el recorte tiene posición absoluta: el rect se mueve, el círculo no
  </text>
</svg>
```

---

## `objectBoundingBox` — coordenadas relativas (0 a 1)

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">objectBoundingBox: relativo al elemento</text>

  <defs>
    <!-- Círculo en (0.5, 0.5) con radio 0.4: el centro del bounding box -->
    <clipPath id="bbox-clip" clipPathUnits="objectBoundingBox">
      <circle cx="0.5" cy="0.5" r="0.4"/>
    </clipPath>
  </defs>

  <!-- rect pequeño: el recorte se ajusta al tamaño del rect -->
  <rect x="30" y="40" width="50" height="50" fill="#3b82f6" clip-path="url(#bbox-clip)"/>
  <text x="55" y="105" text-anchor="middle" font-size="7" fill="#64748b">rect 50×50</text>

  <!-- rect mediano -->
  <rect x="100" y="40" width="80" height="80" fill="#10b981" clip-path="url(#bbox-clip)"/>
  <text x="140" y="135" text-anchor="middle" font-size="7" fill="#64748b">rect 80×80</text>

  <!-- rect grande con proporciones no cuadradas -->
  <rect x="200" y="40" width="90" height="60" fill="#ec4899" clip-path="url(#bbox-clip)"/>
  <text x="245" y="115" text-anchor="middle" font-size="7" fill="#64748b">rect 90×60 (elipsoidal)</text>

  <text x="150" y="155" text-anchor="middle" font-size="8" fill="#94a3b8">
    el mismo clipPath se adapta al tamaño de cada elemento
  </text>
</svg>
```

---

## El mismo clipPath en dos modos: comparación

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">mismo clip, dos modos</text>

  <defs>
    <!-- Recorte: la mitad izquierda -->
    <clipPath id="mitad-user" clipPathUnits="userSpaceOnUse">
      <rect x="0" y="0" width="50" height="200"/>
    </clipPath>
    <clipPath id="mitad-bbox" clipPathUnits="objectBoundingBox">
      <rect x="0" y="0" width="0.5" height="1"/>
    </clipPath>
  </defs>

  <!-- userSpaceOnUse: la "mitad izquierda" siempre está en x<50 absoluto -->
  <text x="75" y="38" text-anchor="middle" font-size="8" fill="#1e40af" font-weight="bold">userSpaceOnUse</text>
  <rect x="20" y="50" width="40" height="40" fill="#3b82f6" clip-path="url(#mitad-user)"/>
  <rect x="70" y="50" width="60" height="40" fill="#3b82f6" clip-path="url(#mitad-user)"/>
  <text x="40" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">20-60</text>
  <text x="100" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">70-130</text>
  <text x="75" y="120" text-anchor="middle" font-size="7" fill="#ef4444">el segundo casi no se ve</text>

  <!-- objectBoundingBox: la "mitad izquierda" siempre es la mitad del elemento -->
  <text x="215" y="38" text-anchor="middle" font-size="8" fill="#166534" font-weight="bold">objectBoundingBox</text>
  <rect x="160" y="50" width="40" height="40" fill="#10b981" clip-path="url(#mitad-bbox)"/>
  <rect x="210" y="50" width="60" height="40" fill="#10b981" clip-path="url(#mitad-bbox)"/>
  <text x="180" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">160-200</text>
  <text x="240" y="105" text-anchor="middle" font-size="6.5" fill="#64748b">210-270</text>
  <text x="215" y="120" text-anchor="middle" font-size="7" fill="#10b981">cada uno muestra su mitad</text>

  <text x="150" y="150" text-anchor="middle" font-size="8" fill="#94a3b8">
    userSpace = posición fija — bbox = porcentaje del elemento
  </text>
  <text x="150" y="165" text-anchor="middle" font-size="7" fill="#94a3b8">
    bbox es mejor cuando el recorte debe seguir al elemento
  </text>
</svg>
```

---

## Pegas de `objectBoundingBox`: formas no circulares

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">bbox deforma en elementos no cuadrados</text>

  <defs>
    <!-- Círculo "centrado" en bbox units -->
    <clipPath id="circ-bbox" clipPathUnits="objectBoundingBox">
      <circle cx="0.5" cy="0.5" r="0.45"/>
    </clipPath>
  </defs>

  <!-- rect cuadrado: el "círculo" se ve como círculo -->
  <rect x="25" y="45" width="80" height="80" fill="#8b5cf6" clip-path="url(#circ-bbox)"/>
  <text x="65" y="140" text-anchor="middle" font-size="7" fill="#64748b">rect 80×80</text>
  <text x="65" y="152" text-anchor="middle" font-size="7" fill="#166534">se ve redondo</text>

  <!-- rect ancho: el "círculo" se ve como elipse aplastada -->
  <rect x="130" y="60" width="150" height="50" fill="#8b5cf6" clip-path="url(#circ-bbox)"/>
  <text x="205" y="140" text-anchor="middle" font-size="7" fill="#64748b">rect 150×50</text>
  <text x="205" y="152" text-anchor="middle" font-size="7" fill="#ef4444">se ve como elipse</text>
</svg>
```

---

## Cuándo usar cada uno

```svg
<svg width="300" height="190"
     viewBox="0 0 300 190"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    clipPathUnits — guía de elección
  </text>

  <!-- userSpaceOnUse -->
  <rect x="10" y="34" width="135" height="145" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5" rx="4"/>
  <text x="77" y="52" text-anchor="middle" font-size="9" font-weight="bold" fill="#1e40af">userSpaceOnUse</text>
  <text x="77" y="64" text-anchor="middle" font-size="7" fill="#64748b">(por defecto)</text>

  <text x="20" y="82"  font-size="7" fill="#1e293b">✓ región fija en el SVG</text>
  <text x="20" y="94"  font-size="7" fill="#1e293b">✓ efecto tipo "ventana"</text>
  <text x="20" y="106" font-size="7" fill="#1e293b">✓ coordenadas intuitivas</text>
  <text x="20" y="118" font-size="7" fill="#1e293b">✓ recorte complejo, exacto</text>
  <text x="20" y="130" font-size="7" fill="#1e293b">✓ uso con &lt;path d="..."&gt;</text>

  <text x="20" y="150" font-size="7" fill="#ef4444">× no sigue al elemento</text>
  <text x="20" y="162" font-size="7" fill="#ef4444">× si mueves el elemento,</text>
  <text x="20" y="172" font-size="7" fill="#ef4444">  el recorte queda atrás</text>

  <!-- objectBoundingBox -->
  <rect x="155" y="34" width="135" height="145" fill="#dcfce7" stroke="#10b981" stroke-width="1.5" rx="4"/>
  <text x="222" y="52" text-anchor="middle" font-size="9" font-weight="bold" fill="#166534">objectBoundingBox</text>
  <text x="222" y="64" text-anchor="middle" font-size="7" fill="#64748b">(rango 0-1)</text>

  <text x="165" y="82"  font-size="7" fill="#1e293b">✓ recorte proporcional</text>
  <text x="165" y="94"  font-size="7" fill="#1e293b">✓ sigue al elemento</text>
  <text x="165" y="106" font-size="7" fill="#1e293b">✓ mismo clip en N elementos</text>
  <text x="165" y="118" font-size="7" fill="#1e293b">✓ "mitad", "tercio", etc.</text>
  <text x="165" y="130" font-size="7" fill="#1e293b">✓ fácil de reutilizar</text>

  <text x="165" y="150" font-size="7" fill="#ef4444">× deforma en elementos</text>
  <text x="165" y="162" font-size="7" fill="#ef4444">  no cuadrados</text>
  <text x="165" y="172" font-size="7" fill="#ef4444">× coordenadas poco intuitivas</text>
</svg>
```
