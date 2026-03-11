# 11.1 El atributo `transform`

Las transformaciones no mueven el elemento: **modifican el sistema de coordenadas** en el que vive. Es como mover y rotar la hoja de papel sobre la que está dibujado.

---

## La metáfora del sistema de coordenadas

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Grid de referencia -->
  <defs>
    <pattern id="ref-grid" width="20" height="20" patternUnits="userSpaceOnUse">
      <line x1="20" y1="0"  x2="20" y2="20" stroke="#e2e8f0" stroke-width="0.5"/>
      <line x1="0"  y1="20" x2="20" y2="20" stroke="#e2e8f0" stroke-width="0.5"/>
    </pattern>
  </defs>
  <rect x="0" y="0" width="300" height="110" fill="url(#ref-grid)"/>

  <!-- Sin transform: el rect vive en el sistema original -->
  <rect x="20" y="20" width="50" height="40" fill="#dbeafe" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="45"  y="74" text-anchor="middle" font-size="7" fill="#3b82f6">sin transform</text>
  <text x="45"  y="83" text-anchor="middle" font-size="6.5" fill="#94a3b8">x=20, y=20</text>

  <!-- Con translate: el sistema se desplaza, el rect sigue en x=20,y=20 de su sistema -->
  <g transform="translate(130, 0)">
    <!-- Mostrar el nuevo origen con una marca -->
    <circle cx="0" cy="0" r="3" fill="#ef4444" opacity="0.5"/>
    <rect x="20" y="20" width="50" height="40" fill="#dcfce7" stroke="#10b981" stroke-width="1.5"/>
    <text x="45"  y="74" text-anchor="middle" font-size="7" fill="#10b981">translate(130,0)</text>
    <text x="45"  y="83" text-anchor="middle" font-size="6.5" fill="#94a3b8">mismo x=20,y=20 — otro sistema</text>
  </g>
</svg>
```

---

## Aplicar en elementos, grupos y herencia

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Transform en un solo elemento -->
  <rect x="10" y="20" width="60" height="45" rx="3"
        fill="#dbeafe" stroke="#3b82f6" stroke-width="1"
        transform="rotate(15, 40, 42)"/>
  <text x="40" y="78" text-anchor="middle" font-size="7" fill="#3b82f6">en elemento</text>

  <!-- Transform en un grupo: afecta a TODOS los hijos -->
  <g transform="translate(110, 10) rotate(10, 50, 30)">
    <rect x="10" y="10" width="80" height="50" rx="3" fill="#dcfce7" stroke="#10b981" stroke-width="1"/>
    <circle cx="50" cy="35" r="12" fill="#10b981" fill-opacity="0.4"/>
    <text x="50" y="38" text-anchor="middle" font-size="7.5" fill="#166534">grupo</text>
  </g>
  <text x="160" y="78" text-anchor="middle" font-size="7" fill="#10b981">en &lt;g&gt; — afecta a hijos</text>

  <!-- Herencia: hijo dentro de grupo transformado -->
  <g transform="translate(225, 15)">
    <rect x="0" y="0" width="65" height="50" rx="3" fill="#fef9c3" stroke="#f59e0b" stroke-width="1"/>
    <!-- El círculo de translate(10,10) es relativo al sistema del grupo -->
    <circle cx="32" cy="25" r="15" fill="#f59e0b" fill-opacity="0.5"/>
    <text x="32" y="63" text-anchor="middle" font-size="7" fill="#92400e">hereda sistema</text>
  </g>
</svg>
```

---

## Múltiples transformaciones: el orden importa

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Referencia: sin transformar -->
  <rect x="10" y="30" width="40" height="30" fill="#dbeafe" stroke="#3b82f6" stroke-width="1"/>
  <text x="30" y="75" text-anchor="middle" font-size="7" fill="#64748b">original</text>

  <!-- translate luego rotate (orden en escritura) -->
  <rect x="10" y="30" width="40" height="30" rx="2"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"
        transform="translate(80,0) rotate(25, 30, 45)"/>
  <text x="140" y="90" text-anchor="middle" font-size="7" fill="#ef4444">translate → rotate</text>

  <!-- rotate luego translate (mismo código en sentido contrario) -->
  <rect x="10" y="30" width="40" height="30" rx="2"
        fill="#dcfce7" stroke="#10b981" stroke-width="1.5"
        transform="translate(195,0) rotate(25)"/>
  <text x="230" y="90" text-anchor="middle" font-size="7" fill="#10b981">translate → rotate(origen)</text>

  <text x="150" y="100" text-anchor="middle" font-size="6.5" fill="#94a3b8">
    el orden de las transformaciones cambia el resultado
  </text>
</svg>
```
