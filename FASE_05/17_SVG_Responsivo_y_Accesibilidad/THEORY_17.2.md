# 17.2 Técnicas de SVG fluido

Más allá de la receta mínima, hay **patrones y trucos CSS** que resuelven casos frecuentes: limitar el tamaño máximo, conservar proporciones concretas, eliminar espacio fantasma y decidir entre tamaño intrínseco vs. controlado.

---

## El patrón recomendado moderno

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    patrón recomendado
  </text>

  <!-- Bloque de CSS -->
  <rect x="15" y="40" width="270" height="135" fill="#0f172a" rx="4"/>
  <text x="25" y="58" font-size="8" font-family="monospace" fill="#94a3b8">/* contenedor con límite máximo */</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#e2e8f0">.contenedor-svg {</text>
  <text x="35" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  width: 100%;</text>
  <text x="35" y="96" font-size="8" font-family="monospace" fill="#60a5fa">  max-width: 600px;</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>

  <text x="25" y="126" font-size="8" font-family="monospace" fill="#94a3b8">/* SVG fluido dentro */</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#e2e8f0">.contenedor-svg svg {</text>
  <text x="35" y="152" font-size="8" font-family="monospace" fill="#34d399">  width: 100%;</text>
  <text x="35" y="164" font-size="8" font-family="monospace" fill="#34d399">  height: auto;</text>
  <text x="35" y="176" font-size="8" font-family="monospace" fill="#fbbf24">  display: block;</text>

  <text x="150" y="196" text-anchor="middle" font-size="7" fill="#64748b">
    el contenedor fija los límites, el SVG llena el ancho
  </text>
  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">
    y calcula la altura según el aspect ratio del viewBox
  </text>
</svg>
```

---

## El detalle de `display: block`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el "espacio fantasma" inline
  </text>

  <!-- Sin display: block -->
  <text x="80" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    inline (por defecto)
  </text>
  <rect x="20" y="55" width="120" height="95" fill="white" stroke="#ef4444" stroke-width="1.5"/>

  <!-- simulación del SVG "centrado" visualmente -->
  <rect x="35" y="70" width="90" height="55" fill="#dbeafe" stroke="#3b82f6"/>
  <circle cx="55" cy="98" r="12" fill="#3b82f6"/>
  <rect x="75" y="85" width="25" height="25" fill="#ec4899"/>

  <!-- línea base de texto simulada -->
  <line x1="35" y1="130" x2="125" y2="130" stroke="#ef4444" stroke-dasharray="2,2"/>
  <text x="80" y="143" text-anchor="middle" font-size="6" fill="#ef4444">← espacio para descendentes</text>

  <!-- Con display: block -->
  <text x="220" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    display: block
  </text>
  <rect x="160" y="55" width="120" height="75" fill="white" stroke="#10b981" stroke-width="1.5"/>
  <rect x="175" y="60" width="90" height="60" fill="#dcfce7" stroke="#10b981"/>
  <circle cx="195" cy="90" r="12" fill="#10b981"/>
  <rect x="215" y="78" width="25" height="25" fill="#ec4899"/>
  <text x="220" y="143" text-anchor="middle" font-size="6" fill="#10b981">sin espacio extra</text>

  <!-- Explicación -->
  <rect x="20" y="160" width="260" height="68" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="30" y="176" font-size="8" font-weight="bold" fill="#92400e">¿Por qué aparece ese espacio?</text>
  <text x="30" y="190" font-size="7" fill="#475569">
    Los SVG son inline por defecto; como un carácter de texto,
  </text>
  <text x="30" y="201" font-size="7" fill="#475569">
    reservan espacio vertical para letras con descendentes (j, g, p, y...).
  </text>
  <text x="30" y="215" font-size="7" fill="#475569">
    display: block convierte el SVG en bloque — sin línea base, sin espacio fantasma.
  </text>
</svg>
```

---

## La propiedad CSS `aspect-ratio`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    aspect-ratio: la solución moderna
  </text>

  <!-- Ejemplo 1: 16:9 -->
  <text x="60" y="50" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">16:9</text>
  <rect x="25" y="58" width="70" height="39" fill="white" stroke="#3b82f6" stroke-width="1.5"/>
  <circle cx="60" cy="77" r="10" fill="#3b82f6"/>
  <text x="60" y="110" text-anchor="middle" font-size="6" fill="#475569">video, hero</text>

  <!-- Ejemplo 2: 4:3 -->
  <text x="150" y="50" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">4:3</text>
  <rect x="115" y="58" width="70" height="52" fill="white" stroke="#10b981" stroke-width="1.5"/>
  <circle cx="150" cy="84" r="12" fill="#10b981"/>
  <text x="150" y="125" text-anchor="middle" font-size="6" fill="#475569">clásico, fotos</text>

  <!-- Ejemplo 3: 1:1 -->
  <text x="240" y="50" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">1:1</text>
  <rect x="215" y="58" width="50" height="50" fill="white" stroke="#ec4899" stroke-width="1.5"/>
  <circle cx="240" cy="83" r="12" fill="#ec4899"/>
  <text x="240" y="125" text-anchor="middle" font-size="6" fill="#475569">avatar, ícono</text>

  <!-- Bloque de CSS -->
  <rect x="20" y="140" width="260" height="90" fill="#0f172a" rx="3"/>
  <text x="30" y="158" font-size="8" font-family="monospace" fill="#94a3b8">/* sin trucos: solo CSS moderno */</text>
  <text x="30" y="172" font-size="8" font-family="monospace" fill="#e2e8f0">svg {</text>
  <text x="40" y="184" font-size="8" font-family="monospace" fill="#60a5fa">  width: 100%;</text>
  <text x="40" y="196" font-size="8" font-family="monospace" fill="#34d399">  aspect-ratio: 16 / 9;</text>
  <text x="30" y="208" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
  <text x="30" y="224" font-size="7" font-family="monospace" fill="#64748b">/* soporte en todos los navegadores desde 2021 */</text>
</svg>
```

---

## El padding-hack legado

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    padding-hack (solo para código antiguo)
  </text>

  <!-- Bloque de CSS -->
  <rect x="15" y="38" width="270" height="140" fill="#0f172a" rx="3"/>
  <text x="25" y="55" font-size="8" font-family="monospace" fill="#94a3b8">/* antes de aspect-ratio, se usaba esto: */</text>
  <text x="25" y="69" font-size="8" font-family="monospace" fill="#e2e8f0">.wrapper {</text>
  <text x="35" y="81" font-size="8" font-family="monospace" fill="#60a5fa">  position: relative;</text>
  <text x="35" y="93" font-size="8" font-family="monospace" fill="#fbbf24">  padding-bottom: 75%;</text>
  <text x="35" y="104" font-size="7" font-family="monospace" fill="#64748b">    /* 300/400 × 100 = 75 (ratio 4:3) */</text>
  <text x="35" y="116" font-size="8" font-family="monospace" fill="#60a5fa">  height: 0;</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#e2e8f0">.wrapper svg {</text>
  <text x="35" y="154" font-size="8" font-family="monospace" fill="#60a5fa">  position: absolute;</text>
  <text x="35" y="166" font-size="8" font-family="monospace" fill="#60a5fa">  top: 0; left: 0;</text>
  <text x="35" y="178" font-size="8" font-family="monospace" fill="#34d399">  width: 100%; height: 100%;</text>

  <!-- Nota explicativa -->
  <rect x="15" y="188" width="270" height="52" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="204" font-size="7" font-weight="bold" fill="#92400e">Por qué funciona:</text>
  <text x="25" y="216" font-size="7" fill="#475569">padding-bottom en % es relativo al ancho del elemento padre.</text>
  <text x="25" y="227" font-size="7" fill="#475569">Con height: 0 + padding-bottom: X%, la altura queda proporcional al ancho.</text>
  <text x="25" y="237" font-size="7" fill="#92400e">Hoy: usa aspect-ratio. Solo mantén este truco en código legacy.</text>
</svg>
```

---

## Tamaño intrínseco vs. controlado

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿tamaño preferido o totalmente flexible?
  </text>

  <!-- Intrínseco -->
  <rect x="15" y="38" width="130" height="180" fill="white" stroke="#cbd5e1" rx="4"/>
  <text x="80" y="55" text-anchor="middle" font-size="9" font-weight="bold" fill="#1e293b">con width/height</text>
  <text x="80" y="67" text-anchor="middle" font-size="7" fill="#64748b">(tamaño intrínseco)</text>

  <rect x="25" y="78" width="110" height="26" fill="#0f172a" rx="2"/>
  <text x="30" y="93" font-size="7" font-family="monospace" fill="#e2e8f0">&lt;svg width="400"</text>
  <text x="30" y="102" font-size="7" font-family="monospace" fill="#e2e8f0">     height="300"&gt;</text>

  <text x="25" y="120" font-size="7" fill="#475569">• el SVG "conoce" su</text>
  <text x="25" y="131" font-size="7" fill="#475569">  tamaño por defecto</text>
  <text x="25" y="148" font-size="7" fill="#10b981">✓ útil si el SVG viaja</text>
  <text x="25" y="159" font-size="7" fill="#10b981">  fuera de tu CSS</text>
  <text x="25" y="176" font-size="7" fill="#10b981">✓ fallback si el CSS</text>
  <text x="25" y="187" font-size="7" fill="#10b981">  no se aplica</text>
  <text x="25" y="205" font-size="7" fill="#94a3b8">CSS puede sobreescribir</text>

  <!-- Controlado -->
  <rect x="155" y="38" width="130" height="180" fill="white" stroke="#cbd5e1" rx="4"/>
  <text x="220" y="55" text-anchor="middle" font-size="9" font-weight="bold" fill="#1e293b">sin width/height</text>
  <text x="220" y="67" text-anchor="middle" font-size="7" fill="#64748b">(controlado por CSS)</text>

  <rect x="165" y="78" width="110" height="26" fill="#0f172a" rx="2"/>
  <text x="170" y="93" font-size="7" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox=</text>
  <text x="170" y="102" font-size="7" font-family="monospace" fill="#e2e8f0">  "0 0 400 300"&gt;</text>

  <text x="165" y="120" font-size="7" fill="#475569">• el CSS decide todo</text>
  <text x="165" y="131" font-size="7" fill="#475569">  el tamaño</text>
  <text x="165" y="148" font-size="7" fill="#10b981">✓ flexibilidad máxima</text>
  <text x="165" y="159" font-size="7" fill="#10b981">✓ sistemas de diseño</text>
  <text x="165" y="176" font-size="7" fill="#ef4444">✗ sin CSS, el tamaño</text>
  <text x="165" y="187" font-size="7" fill="#ef4444">  es 300×150 px (default</text>
  <text x="165" y="198" font-size="7" fill="#ef4444">  del reemplazado)</text>

  <text x="150" y="233" text-anchor="middle" font-size="6" fill="#94a3b8">
    regla práctica: intrínseco para assets sueltos, controlado para componentes
  </text>
</svg>
```
