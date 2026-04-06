# 18.4 SVG con `<object>`

`<object>` es un elemento pensado para incrustar recursos externos con **fallback integrado**. Aplicado a SVG, ofrece algo que `<img>` no: los scripts internos se ejecutan, y hay una puerta limitada al DOM mediante `getSVGDocument()`.

---

## Sintaxis con fallback

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;object&gt; con fallback integrado
  </text>

  <rect x="15" y="38" width="270" height="146" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;object</text>
  <text x="35" y="70" font-size="8" font-family="monospace" fill="#fbbf24">  type="image/svg+xml"</text>
  <text x="35" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  data="diagrama.svg"</text>
  <text x="35" y="98" font-size="8" font-family="monospace" fill="#34d399">  width="400" height="300"&gt;</text>

  <text x="35" y="116" font-size="8" font-family="monospace" fill="#94a3b8">  &lt;!-- fallback si el SVG no carga --&gt;</text>
  <text x="35" y="130" font-size="8" font-family="monospace" fill="#c084fc">  &lt;img src="diagrama.png"</text>
  <text x="35" y="142" font-size="8" font-family="monospace" fill="#c084fc">       alt="Diagrama del proceso"/&gt;</text>

  <text x="25" y="160" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/object&gt;</text>
  <text x="25" y="176" font-size="7" font-family="monospace" fill="#94a3b8">/* todo el contenido interno es fallback */</text>
</svg>
```

---

## `getSVGDocument()` — la puerta al interior

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    acceder al DOM interno desde JS externo
  </text>

  <!-- Código -->
  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">// 1. esperar a que el &lt;object&gt; haya cargado</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">const obj = document.querySelector('object');</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#e2e8f0">obj.addEventListener('load', () =&gt; {</text>

  <text x="25" y="102" font-size="8" font-family="monospace" fill="#94a3b8">  // 2. obtener el documento interno del SVG</text>
  <text x="25" y="116" font-size="8" font-family="monospace" fill="#60a5fa">  const svgDoc = obj.getSVGDocument();</text>

  <text x="25" y="134" font-size="8" font-family="monospace" fill="#94a3b8">  // 3. manipular como si fuera HTML</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#34d399">  const c = svgDoc.querySelector('circle');</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#34d399">  c.setAttribute('fill', 'red');</text>

  <text x="25" y="180" font-size="8" font-family="monospace" fill="#e2e8f0">});</text>

  <text x="150" y="210" text-anchor="middle" font-size="7" fill="#64748b">
    también disponible: obj.contentDocument
  </text>
  <text x="150" y="224" text-anchor="middle" font-size="6" fill="#94a3b8">
    (requiere same-origin; no funciona con SVGs de otros dominios)
  </text>
</svg>
```

---

## SVG con contexto propio

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada &lt;object&gt; tiene su propio documento SVG
  </text>

  <!-- Característica 1 -->
  <rect x="15" y="38" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ Scripts internos se ejecutan</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;&lt;script&gt;console.log('hola')&lt;/script&gt;&lt;/svg&gt;</text>
  <text x="25" y="86" font-size="7" fill="#475569">a diferencia de &lt;img&gt;, el &lt;object&gt; ejecuta los &lt;script&gt; internos</text>

  <!-- Característica 2 -->
  <rect x="15" y="108" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="124" font-size="9" font-weight="bold" fill="#1e40af">✓ CSS interno funciona normalmente</text>
  <text x="25" y="140" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;&lt;style&gt;circle { fill: blue; }&lt;/style&gt;...&lt;/svg&gt;</text>
  <text x="25" y="156" font-size="7" fill="#475569">el SVG es un documento separado con sus propios estilos</text>

  <!-- Limitación -->
  <rect x="15" y="178" width="270" height="72" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="194" font-size="9" font-weight="bold" fill="#991b1b">✗ CSS externo del padre NO aplica</text>
  <text x="25" y="210" font-size="7" font-family="monospace" fill="#475569">/* esto no afecta al SVG del &lt;object&gt; */</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#475569">object circle { fill: red; }</text>
  <text x="25" y="238" font-size="7" fill="#991b1b">el documento del &lt;object&gt; es independiente</text>
</svg>
```

---

## Fallback en acción

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el fallback es cualquier HTML
  </text>

  <!-- Caso 1 -->
  <rect x="15" y="38" width="270" height="50" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#1e40af">Imagen PNG como plan B</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;object data="chart.svg"&gt;</text>
  <text x="25" y="80" font-size="7" font-family="monospace" fill="#475569">  &lt;img src="chart.png" alt="..."/&gt;&lt;/object&gt;</text>

  <!-- Caso 2 -->
  <rect x="15" y="96" width="270" height="50" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="112" font-size="8" font-weight="bold" fill="#166534">Texto descriptivo como plan B</text>
  <text x="25" y="126" font-size="7" font-family="monospace" fill="#475569">&lt;object data="diagrama.svg"&gt;</text>
  <text x="25" y="138" font-size="7" font-family="monospace" fill="#475569">  &lt;p&gt;Flujo: inicio → validar → fin&lt;/p&gt;&lt;/object&gt;</text>

  <!-- Caso 3 -->
  <rect x="15" y="154" width="270" height="50" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="170" font-size="8" font-weight="bold" fill="#5b21b6">Otro &lt;object&gt; anidado</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#475569">&lt;object data="a.svg"&gt;&lt;object data="b.svg"&gt;</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#475569">  &lt;img.../&gt;&lt;/object&gt;&lt;/object&gt;</text>
</svg>
```

---

## ¿Por qué se usa poco?

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;object&gt; hoy: nicho específico
  </text>

  <!-- Razones -->
  <rect x="15" y="38" width="270" height="130" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Por qué ha caído en desuso</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• inline es mucho más simple para lo mismo que necesitas</text>
  <text x="25" y="84" font-size="7" fill="#1e293b">• getSVGDocument() tiene comportamiento inconsistente</text>
  <text x="25" y="98" font-size="7" fill="#1e293b">• el evento 'load' puede dispararse antes de tiempo</text>
  <text x="25" y="112" font-size="7" fill="#1e293b">• CSS externo sigue sin aplicar: las build tools modernas resuelven</text>
  <text x="25" y="124" font-size="7" fill="#1e293b">  el caso de cache inline → extracción automática</text>
  <text x="25" y="138" font-size="7" fill="#1e293b">• la sintaxis es verbosa frente a &lt;img&gt; / inline</text>
  <text x="25" y="152" font-size="7" fill="#1e293b">• los lectores de pantalla lo tratan igual que una imagen</text>

  <!-- Cuándo sigue siendo válido -->
  <rect x="15" y="178" width="270" height="50" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="194" font-size="8" font-weight="bold" fill="#166534">Sigue teniendo sentido cuando...</text>
  <text x="25" y="208" font-size="7" fill="#475569">necesitas un SVG con scripts internos propios + fallback PNG</text>
  <text x="25" y="220" font-size="7" fill="#475569">(legacy, conversores, herramientas antiguas)</text>
</svg>
```
