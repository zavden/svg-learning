# 21.7 Drag and drop en SVG

Arrastrar un elemento SVG es la combinación de todo lo anterior: pointer events, conversión de coordenadas y `setPointerCapture`. La clave que evita el 90% de los bugs es la captura del puntero — sin ella, si el cursor se sale rápido del elemento, el drag "se pierde".

---

## La receta básica

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    pointerdown → move → up
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">let arrastrando = false;</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#94a3b8">let offsetX, offsetY;</text>

  <text x="25" y="90" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointerdown', e =&gt; {</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#34d399">  arrastrando = true;</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#fbbf24">  el.setPointerCapture(e.pointerId);</text>
  <text x="25" y="132" font-size="8" font-family="monospace" fill="#34d399">  const p = svgCoords(e, svg);</text>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#34d399">  offsetX = p.x - parseFloat(el.getAttribute('x'));</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#34d399">  offsetY = p.y - parseFloat(el.getAttribute('y'));</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">});</text>

  <text x="25" y="196" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointermove', e =&gt; {</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">  if (!arrastrando) return;</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#34d399">  const p = svgCoords(e, svg);</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#34d399">  el.setAttribute('x', p.x - offsetX);</text>
  <text x="25" y="252" font-size="8" font-family="monospace" fill="#34d399">  el.setAttribute('y', p.y - offsetY);</text>
  <text x="25" y="266" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
</svg>
```

---

## Por qué `setPointerCapture` es imprescindible

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    capturar el puntero al elemento
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Sin setPointerCapture</text>
  <text x="25" y="70" font-size="7" fill="#475569">• si arrastras rápido el mouse fuera del elemento,</text>
  <text x="25" y="84" font-size="7" fill="#475569">  el pointermove deja de dispararse en el elemento</text>
  <text x="25" y="98" font-size="7" fill="#475569">• el drag "se congela" y el elemento queda atrás</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">→ síntoma clásico del "bug del arrastre que escapa"</text>

  <rect x="15" y="142" width="270" height="106" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="158" font-size="9" font-weight="bold" fill="#166534">Con setPointerCapture</text>
  <text x="25" y="174" font-size="7" fill="#475569">• todos los eventos de ese pointerId van al elemento,</text>
  <text x="25" y="188" font-size="7" fill="#475569">  aunque el cursor esté fuera</text>
  <text x="25" y="202" font-size="7" fill="#475569">• se libera automáticamente en pointerup o pointercancel</text>
  <text x="25" y="216" font-size="7" fill="#475569">• un solo pointerId por dedo/mouse → multitouch incluido</text>
  <text x="25" y="232" font-size="7" fill="#166534">→ siempre captúralo en pointerdown</text>
</svg>
```

---

## Drag de círculos: `cx`/`cy` en vez de `x`/`y`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada forma tiene su atributo de posición
  </text>

  <rect x="15" y="38" width="270" height="196" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// para &lt;circle&gt; y &lt;ellipse&gt; → cx/cy</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">circle.addEventListener('pointerdown', e =&gt; {</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  const p = svgCoords(e, svg);</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">  offsetX = p.x - parseFloat(</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#fbbf24">    circle.getAttribute('cx'));</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#34d399">  offsetY = p.y - parseFloat(</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#fbbf24">    circle.getAttribute('cy'));</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#60a5fa">});</text>

  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">// en pointermove</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#ec4899">circle.setAttribute('cx', p.x - offsetX);</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#ec4899">circle.setAttribute('cy', p.y - offsetY);</text>
  <text x="25" y="226" font-size="7" fill="#fbbf24">rect → x/y · circle → cx/cy · path → transform</text>
</svg>
```

---

## Drag vía `transform` — la opción más flexible

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    mover con translate en lugar de x/y
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Ventajas</text>
  <text x="25" y="70" font-size="7" fill="#475569">• funciona igual para circle, rect, path, g</text>
  <text x="25" y="84" font-size="7" fill="#475569">• más rápido (no fuerza reflow de layout)</text>
  <text x="25" y="98" font-size="7" fill="#475569">• combinable con rotate, scale, etc.</text>
  <text x="25" y="112" font-size="7" fill="#166534">→ estándar de facto en editores gráficos SVG</text>

  <rect x="15" y="126" width="270" height="140" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// posición inicial guardada aparte</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">let tx = 0, ty = 0;</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#94a3b8">let startX, startY;</text>

  <text x="25" y="192" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointerdown', e =&gt; {</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">  const p = svgCoords(e, svg);</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">  startX = p.x - tx; startY = p.y - ty;</text>
  <text x="25" y="234" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
  <text x="25" y="254" font-size="8" font-family="monospace" fill="#ec4899">el.setAttribute('transform',</text>
  <text x="25" y="268" font-size="8" font-family="monospace" fill="#ec4899">  `translate(${tx} ${ty})`);</text>
</svg>
```

---

## Snap a la rejilla

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    alinear a una cuadrícula
  </text>

  <rect x="15" y="38" width="270" height="166" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">const GRID = 20;  // tamaño de la celda</text>

  <text x="25" y="76" font-size="7" font-family="monospace" fill="#94a3b8">// redondeo al múltiplo más cercano</text>
  <text x="25" y="92" font-size="8" font-family="monospace" fill="#60a5fa">function snap(v) {</text>
  <text x="25" y="106" font-size="8" font-family="monospace" fill="#34d399">  return Math.round(v / GRID) * GRID;</text>
  <text x="25" y="120" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// dentro de pointermove</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#fbbf24">el.setAttribute('x', snap(p.x - offsetX));</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#fbbf24">el.setAttribute('y', snap(p.y - offsetY));</text>
  <text x="25" y="192" font-size="7" fill="#94a3b8">→ el elemento "salta" entre celdas mientras arrastras</text>
</svg>
```
