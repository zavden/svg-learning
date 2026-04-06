# REFERENCE — Interactividad con JavaScript

Cheat sheet para copiar/pegar: los helpers fundamentales (crear elemento, convertir coordenadas, drag) y las APIs más usadas.

---

## Helper: crear elementos SVG

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    svgEl — la utilidad clave
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">const NS = 'http://www.w3.org/2000/svg';</text>

  <text x="25" y="78" font-size="8" font-family="monospace" fill="#60a5fa">function svgEl(tag, attrs = {}) {</text>
  <text x="25" y="92" font-size="8" font-family="monospace" fill="#34d399">  const el = document.createElementNS(NS, tag);</text>
  <text x="25" y="106" font-size="8" font-family="monospace" fill="#34d399">  for (const [k, v] of Object.entries(attrs))</text>
  <text x="25" y="120" font-size="8" font-family="monospace" fill="#34d399">    el.setAttribute(k, v);</text>
  <text x="25" y="134" font-size="8" font-family="monospace" fill="#34d399">  return el;</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="172" font-size="7" font-family="monospace" fill="#94a3b8">// uso</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#fbbf24">svg.appendChild(svgEl('circle',</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#fbbf24">  { cx: 50, cy: 50, r: 20, fill: 'red' }));</text>
</svg>
```

---

## Helper: coordenadas del mouse → SVG

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    svgCoords — para todo evento pointer
  </text>

  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">function svgCoords(evt, svg) {</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#34d399">  const pt = new DOMPoint(</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">    evt.clientX, evt.clientY);</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#34d399">  return pt.matrixTransform(</text>
  <text x="25" y="116" font-size="8" font-family="monospace" fill="#fbbf24">    svg.getScreenCTM().inverse());</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="152" font-size="7" font-family="monospace" fill="#94a3b8">// uso</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#ec4899">const { x, y } = svgCoords(e, svg);</text>
</svg>
```

---

## Helper: drag and drop reusable

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    makeDraggable — funciona para cualquier forma
  </text>

  <rect x="15" y="38" width="270" height="250" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">function makeDraggable(el, svg, {</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#34d399">  xAttr = 'x', yAttr = 'y'</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#60a5fa">} = {}) {</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  let ox, oy;</text>

  <text x="25" y="116" font-size="8" font-family="monospace" fill="#60a5fa">  el.addEventListener('pointerdown', e =&gt; {</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#fbbf24">    el.setPointerCapture(e.pointerId);</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#34d399">    const p = svgCoords(e, svg);</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#34d399">    ox = p.x - parseFloat(el.getAttribute(xAttr));</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#34d399">    oy = p.y - parseFloat(el.getAttribute(yAttr));</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#60a5fa">  });</text>

  <text x="25" y="206" font-size="8" font-family="monospace" fill="#60a5fa">  el.addEventListener('pointermove', e =&gt; {</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">    if (!el.hasPointerCapture(e.pointerId)) return;</text>
  <text x="25" y="234" font-size="8" font-family="monospace" fill="#34d399">    const p = svgCoords(e, svg);</text>
  <text x="25" y="248" font-size="8" font-family="monospace" fill="#34d399">    el.setAttribute(xAttr, p.x - ox);</text>
  <text x="25" y="262" font-size="8" font-family="monospace" fill="#34d399">    el.setAttribute(yAttr, p.y - oy);</text>
  <text x="25" y="276" font-size="8" font-family="monospace" fill="#60a5fa">  });</text>
  <text x="25" y="282" font-size="7" font-family="monospace" fill="#94a3b8">}</text>
</svg>
```

---

## Métodos SVG específicos de un vistazo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    las APIs más usadas
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">método</text>
  <text x="160" y="46" font-size="7" font-weight="bold" fill="#1e293b">para</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" font-family="monospace" fill="#3b82f6">getBBox()</text>
  <text x="160" y="66" font-size="7" fill="#475569">caja del elemento</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" font-family="monospace" fill="#3b82f6">getScreenCTM()</text>
  <text x="160" y="88" font-size="7" fill="#475569">matriz SVG → pantalla</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" font-family="monospace" fill="#3b82f6">getTotalLength()</text>
  <text x="160" y="110" font-size="7" fill="#475569">longitud de un path</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" font-family="monospace" fill="#3b82f6">getPointAtLength(d)</text>
  <text x="160" y="132" font-size="7" fill="#475569">punto a distancia d</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" font-family="monospace" fill="#3b82f6">isPointInFill(pt)</text>
  <text x="160" y="154" font-size="7" fill="#475569">hit test dentro</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" font-family="monospace" fill="#3b82f6">isPointInStroke(pt)</text>
  <text x="160" y="176" font-size="7" fill="#475569">hit test en el trazo</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" font-family="monospace" fill="#3b82f6">setPointerCapture</text>
  <text x="160" y="198" font-size="7" fill="#475569">fijar pointer al elemento</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" font-family="monospace" fill="#3b82f6">classList.add/remove</text>
  <text x="160" y="220" font-size="7" fill="#475569">cambiar clases</text>
</svg>
```

---

## Patrón: event delegation

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un listener para muchos elementos
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">svg.addEventListener('click', e =&gt; {</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#34d399">  const barra = e.target.closest('.barra');</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  if (!barra) return;</text>
  <text x="25" y="106" font-size="8" font-family="monospace" fill="#34d399">  const id = barra.dataset.id;</text>
  <text x="25" y="120" font-size="8" font-family="monospace" fill="#34d399">  const valor = barra.dataset.valor;</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#fbbf24">  seleccionar(id, valor);</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
  <text x="25" y="178" font-size="7" fill="#94a3b8">• un solo listener para 1000 elementos</text>
  <text x="25" y="194" font-size="7" fill="#94a3b8">• funciona con elementos añadidos dinámicamente</text>
</svg>
```
