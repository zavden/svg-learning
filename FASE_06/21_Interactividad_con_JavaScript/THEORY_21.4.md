# 21.4 Eventos en SVG

Los elementos SVG inline reciben todos los eventos del DOM: `click`, `mouseover`, `keydown`, `pointerdown`… No hay una API paralela. La única diferencia relevante es que SVG no es focusable por defecto: para teclado necesitas añadir `tabindex`.

---

## Los eventos clásicos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los mismos eventos que en HTML
  </text>

  <rect x="15" y="38" width="270" height="228" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// click</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">circle.addEventListener('click', e =&gt; {</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  console.log(e.target); // el &lt;circle&gt;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#60a5fa">});</text>

  <text x="25" y="116" font-size="7" font-family="monospace" fill="#94a3b8">// mouse</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#fbbf24">el.addEventListener('mouseover', ...)</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#fbbf24">el.addEventListener('mouseout',  ...)</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#fbbf24">el.addEventListener('mousemove', ...)</text>

  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">// teclado (requiere tabindex)</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#ec4899">circle.setAttribute('tabindex','0');</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#ec4899">circle.addEventListener('keydown', e =&gt; {</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">  if (e.key === 'Enter') activar();</text>
  <text x="25" y="234" font-size="8" font-family="monospace" fill="#ec4899">});</text>
  <text x="25" y="254" font-size="7" fill="#fbbf24">sin tabindex, SVG no recibe foco ni keydown</text>
</svg>
```

---

## Pointer events — la API moderna

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    pointerdown/move/up unifican mouse+touch+pen
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Por qué Pointer Events &gt; Mouse Events</text>
  <text x="25" y="70" font-size="7" fill="#475569">• una sola API para mouse, touch y stylus</text>
  <text x="25" y="84" font-size="7" fill="#475569">• evento dispara la primera vez, aunque sea touch</text>
  <text x="25" y="98" font-size="7" fill="#475569">• información extra: presión, tamaño, tilt, pointerId</text>
  <text x="25" y="112" font-size="7" fill="#475569">• soporte universal desde 2018</text>

  <rect x="15" y="142" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointerdown', e =&gt; { });</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointermove', e =&gt; { });</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointerup',   e =&gt; { });</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#60a5fa">el.addEventListener('pointercancel', ...);</text>
  <text x="25" y="226" font-size="7" fill="#fbbf24">úsalos para todo menos el click puro</text>
  <text x="25" y="240" font-size="7" fill="#94a3b8">(el click sigue funcionando bien para botones)</text>
</svg>
```

---

## Propagación y delegación

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un listener para muchos elementos
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">El problema</text>
  <text x="25" y="70" font-size="7" fill="#475569">Un scatter plot con 500 puntos. Si pones un</text>
  <text x="25" y="84" font-size="7" fill="#475569">listener por punto, son 500 listeners y más</text>
  <text x="25" y="98" font-size="7" fill="#475569">memoria. Además, si añades puntos después,</text>
  <text x="25" y="112" font-size="7" fill="#92400e">los nuevos no tendrán listener automáticamente.</text>

  <rect x="15" y="130" width="270" height="138" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">// delegación: 1 listener en el padre</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#60a5fa">svg.addEventListener('click', e =&gt; {</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#34d399">  if (e.target.matches('circle')) {</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#34d399">    const id = e.target.id;</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#34d399">    const val = e.target.dataset.valor;</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#34d399">  }</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#fbbf24">  const g = e.target.closest('.barra');</text>
  <text x="25" y="246" font-size="8" font-family="monospace" fill="#fbbf24">  if (g) { /* click en grupo */ }</text>
  <text x="25" y="260" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
</svg>
```

---

## `e.target` vs `e.currentTarget`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    quién recibió el click, quién lo escucha
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;g&gt;  &lt;!-- escucha aquí --&gt;</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;circle/&gt;  ← el usuario hace click aquí</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;text/&gt;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/g&gt;</text>
  <text x="25" y="118" font-size="7" fill="#fbbf24">el click "burbujea" del círculo al grupo</text>

  <rect x="15" y="136" width="270" height="98" fill="#0f172a" rx="3"/>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#60a5fa">g.addEventListener('click', e =&gt; {</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">  e.target        // &lt;circle&gt; ← el que recibió</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#34d399">  e.currentTarget // &lt;g&gt; ← el que escucha</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
  <text x="25" y="220" font-size="7" fill="#94a3b8">target cambia en la burbuja; currentTarget no</text>
</svg>
```

---

## `e.preventDefault()` y `e.stopPropagation()`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    detener acciones por defecto
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// evitar acción por defecto</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">svg.addEventListener('contextmenu', e =&gt; {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  e.preventDefault(); // sin menú del navegador</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
  <text x="25" y="118" font-size="7" fill="#94a3b8">útil para gestos personalizados con click derecho</text>

  <rect x="15" y="136" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#94a3b8">// detener la propagación</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#60a5fa">circle.addEventListener('click', e =&gt; {</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#34d399">  e.stopPropagation();</text>
  <text x="25" y="198" font-size="7" font-family="monospace" fill="#94a3b8">  // el click NO llegará al padre &lt;g&gt; ni al &lt;svg&gt;</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#60a5fa">});</text>
  <text x="25" y="228" font-size="7" fill="#fbbf24">ojo: rompe la delegación — úsalo con cuidado</text>
</svg>
```
