# 17.1 SVG responsivo básico

Un SVG es **responsivo** cuando se adapta al tamaño de su contenedor sin perder calidad. Dado que el SVG ya es vectorial por naturaleza, el reto no es técnico: es **configurarlo correctamente** para que el navegador le permita escalar.

---

## Sin `viewBox`, no hay responsividad

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">con y sin viewBox al escalar</text>

  <!-- Columna izquierda: SVG sin viewBox -->
  <rect x="15" y="35" width="130" height="170" fill="#fef2f2" stroke="#fca5a5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">SIN viewBox</text>

  <!-- Simulación: SVG "fijo" de 80×50 escalado -->
  <rect x="25" y="65" width="80" height="50" fill="white" stroke="#64748b"/>
  <circle cx="45" cy="90" r="12" fill="#3b82f6"/>
  <rect x="60" y="78" width="24" height="24" fill="#ec4899"/>
  <text x="25" y="128" font-size="6" fill="#64748b">tamaño original (100%)</text>

  <!-- Mismo SVG "estirado" sin viewBox: los elementos NO escalan, quedan truncados -->
  <rect x="25" y="140" width="110" height="50" fill="white" stroke="#64748b"/>
  <circle cx="45" cy="165" r="12" fill="#3b82f6"/>
  <rect x="60" y="153" width="24" height="24" fill="#ec4899"/>
  <text x="25" y="202" font-size="6" fill="#ef4444">estirado: contenido no crece</text>

  <!-- Columna derecha: SVG con viewBox -->
  <rect x="155" y="35" width="130" height="170" fill="#dcfce7" stroke="#86efac" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">CON viewBox</text>

  <rect x="165" y="65" width="80" height="50" fill="white" stroke="#64748b"/>
  <circle cx="185" cy="90" r="12" fill="#3b82f6"/>
  <rect x="200" y="78" width="24" height="24" fill="#ec4899"/>
  <text x="165" y="128" font-size="6" fill="#64748b">tamaño original (100%)</text>

  <!-- Mismo SVG con viewBox: los elementos SÍ escalan proporcionalmente -->
  <rect x="165" y="140" width="110" height="60" fill="white" stroke="#64748b"/>
  <circle cx="192" cy="170" r="16" fill="#3b82f6"/>
  <rect x="212" y="152" width="33" height="33" fill="#ec4899"/>
  <text x="165" y="212" font-size="6" fill="#10b981">escalado: todo crece en proporción</text>
</svg>
```

---

## La receta mínima

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    receta de SVG responsivo
  </text>

  <!-- Paso 1 -->
  <rect x="15" y="38" width="270" height="50" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <circle cx="30" cy="55" r="8" fill="#3b82f6"/>
  <text x="30" y="59" text-anchor="middle" font-size="9" font-weight="bold" fill="white">1</text>
  <text x="45" y="53" font-size="9" font-weight="bold" fill="#1e40af">Declarar viewBox</text>
  <text x="45" y="66" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 400 300"&gt;</text>
  <text x="45" y="78" font-size="7" fill="#64748b">define las proporciones internas del contenido</text>

  <!-- Paso 2 -->
  <rect x="15" y="96" width="270" height="58" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <circle cx="30" cy="113" r="8" fill="#10b981"/>
  <text x="30" y="117" text-anchor="middle" font-size="9" font-weight="bold" fill="white">2</text>
  <text x="45" y="111" font-size="9" font-weight="bold" fill="#166534">Omitir width/height absolutos en el &lt;svg&gt;</text>
  <text x="45" y="124" font-size="7" font-family="monospace" fill="#ef4444">&lt;svg width="400" height="300"&gt;     ← fijo</text>
  <text x="45" y="136" font-size="7" font-family="monospace" fill="#10b981">&lt;svg viewBox="0 0 400 300"&gt;        ← flexible</text>
  <text x="45" y="148" font-size="7" fill="#64748b">sin dimensiones, el CSS del padre decide el tamaño</text>

  <!-- Paso 3 -->
  <rect x="15" y="162" width="270" height="60" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <circle cx="30" cy="179" r="8" fill="#8b5cf6"/>
  <text x="30" y="183" text-anchor="middle" font-size="9" font-weight="bold" fill="white">3</text>
  <text x="45" y="177" font-size="9" font-weight="bold" fill="#5b21b6">Controlar desde CSS</text>
  <text x="45" y="190" font-size="7" font-family="monospace" fill="#475569">svg {</text>
  <text x="55" y="200" font-size="7" font-family="monospace" fill="#475569">width: 100%;</text>
  <text x="55" y="210" font-size="7" font-family="monospace" fill="#475569">height: auto;</text>
  <text x="45" y="220" font-size="7" font-family="monospace" fill="#475569">}</text>

  <text x="150" y="240" text-anchor="middle" font-size="7" fill="#94a3b8">
    con esto, el SVG llena su contenedor manteniendo el aspect ratio del viewBox
  </text>
</svg>
```

---

## Por qué `height: auto` funciona

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">cálculo automático de altura por aspect ratio</text>

  <!-- viewBox declara proporción 4:3 -->
  <text x="150" y="40" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">
    viewBox="0 0 400 300"
  </text>
  <text x="150" y="52" text-anchor="middle" font-size="7" fill="#64748b">
    aspect ratio 4:3 (1.333...)
  </text>

  <!-- Tamaño 1: contenedor de 120px de ancho -->
  <rect x="30" y="65" width="120" height="90" fill="white" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="90" y="58" text-anchor="middle" font-size="6" fill="#64748b">contenedor: 120px</text>
  <circle cx="55" cy="95" r="12" fill="#3b82f6"/>
  <rect x="80" y="90" width="25" height="25" fill="#10b981"/>
  <text x="90" y="138" text-anchor="middle" font-size="6" fill="#475569">ancho: 120px</text>
  <text x="90" y="148" text-anchor="middle" font-size="6" fill="#475569">alto calculado: 90px</text>
  <text x="90" y="158" text-anchor="middle" font-size="6" fill="#10b981">(120 × 3/4 = 90)</text>

  <!-- Tamaño 2: contenedor de 160px -->
  <rect x="170" y="65" width="110" height="82.5" fill="white" stroke="#3b82f6" stroke-width="1.5"/>
  <text x="225" y="58" text-anchor="middle" font-size="6" fill="#64748b">contenedor: 110px</text>
  <circle cx="192" cy="93" r="11" fill="#3b82f6"/>
  <rect x="215" y="88" width="23" height="23" fill="#10b981"/>
  <text x="225" y="130" text-anchor="middle" font-size="6" fill="#475569">ancho: 110px</text>
  <text x="225" y="140" text-anchor="middle" font-size="6" fill="#475569">alto calculado: 82.5px</text>
  <text x="225" y="150" text-anchor="middle" font-size="6" fill="#10b981">(110 × 3/4 = 82.5)</text>

  <text x="150" y="178" text-anchor="middle" font-size="7" fill="#64748b">
    height: auto consulta el viewBox para mantener proporción
  </text>
  <text x="150" y="190" text-anchor="middle" font-size="7" fill="#64748b">
    el contenido se re-escala sin deformaciones
  </text>
  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">
    (si no hay viewBox, height: auto no sabe qué calcular)
  </text>
</svg>
```

---

## Comportamiento según contexto de uso

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cómo se comporta sin width/height
  </text>

  <!-- Caso 1: SVG inline -->
  <rect x="15" y="38" width="270" height="58" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">SVG inline en HTML</text>
  <text x="25" y="67" font-size="7" font-family="monospace" fill="#475569">&lt;div&gt;&lt;svg viewBox="..."&gt;...&lt;/svg&gt;&lt;/div&gt;</text>
  <text x="25" y="80" font-size="7" fill="#475569">• se comporta como elemento de bloque</text>
  <text x="25" y="91" font-size="7" fill="#475569">• toma el ancho del contenedor, alto según aspect ratio</text>

  <!-- Caso 2: SVG en <img> -->
  <rect x="15" y="104" width="270" height="58" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="120" font-size="9" font-weight="bold" fill="#1e40af">SVG en &lt;img src="..."&gt;</text>
  <text x="25" y="133" font-size="7" font-family="monospace" fill="#475569">&lt;img src="diagrama.svg" alt=""&gt;</text>
  <text x="25" y="146" font-size="7" fill="#475569">• se comporta como imagen raster normal</text>
  <text x="25" y="157" font-size="7" fill="#475569">• CSS se aplica al &lt;img&gt;, no al SVG interno</text>

  <!-- Caso 3: SVG en <object> -->
  <rect x="15" y="170" width="270" height="58" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="186" font-size="9" font-weight="bold" fill="#5b21b6">SVG en &lt;object&gt;</text>
  <text x="25" y="199" font-size="7" font-family="monospace" fill="#475569">&lt;object data="diagrama.svg" type="image/svg+xml"&gt;</text>
  <text x="25" y="212" font-size="7" fill="#475569">• requiere width/height en el propio &lt;object&gt;</text>
  <text x="25" y="223" font-size="7" fill="#475569">• más control pero más ceremonia</text>
</svg>
```

---

## Ejemplo antes/después

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    mismo SVG, dos declaraciones
  </text>

  <!-- Versión no responsiva -->
  <rect x="10" y="38" width="280" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="20" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ no responsiva (tamaño fijo)</text>
  <text x="20" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;svg width="400" height="300"&gt;</text>
  <text x="20" y="82" font-size="7" font-family="monospace" fill="#475569">  &lt;circle cx="200" cy="150" r="100"/&gt;</text>
  <text x="20" y="94" font-size="7" font-family="monospace" fill="#475569">&lt;/svg&gt;</text>
  <text x="20" y="112" font-size="7" fill="#991b1b">• siempre ocupa 400×300 px exactos</text>
  <text x="20" y="124" font-size="7" fill="#991b1b">• desborda en móvil, queda pequeño en desktop</text>

  <!-- Versión responsiva -->
  <rect x="10" y="146" width="280" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="20" y="162" font-size="9" font-weight="bold" fill="#166534">✓ responsiva (viewBox + CSS)</text>
  <text x="20" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 400 300"&gt;</text>
  <text x="20" y="190" font-size="7" font-family="monospace" fill="#475569">  &lt;circle cx="200" cy="150" r="100"/&gt;</text>
  <text x="20" y="202" font-size="7" font-family="monospace" fill="#475569">&lt;/svg&gt;</text>
  <text x="20" y="214" font-size="7" font-family="monospace" fill="#8b5cf6">svg { width: 100%; height: auto; }</text>
  <text x="20" y="228" font-size="7" fill="#166534">• se adapta al contenedor manteniendo 4:3</text>
  <text x="20" y="240" font-size="7" fill="#166534">• nunca desborda, siempre ocupa el ancho disponible</text>
</svg>
```
