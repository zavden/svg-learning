# 21.5 Coordenadas del mouse en SVG

`event.clientX` te da coordenadas de **pantalla**, pero los elementos SVG viven en el espacio del **viewBox**. Si el SVG mide `viewBox="0 0 100 100"` y ocupa 500×500 píxeles, un click en `(250, 250)` de pantalla equivale a `(50, 50)` en SVG. Hay que convertir.

---

## El problema fundamental

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    pantalla ≠ viewBox
  </text>

  <!-- Pantalla -->
  <rect x="20" y="42" width="120" height="90" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="80" y="58" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">Pantalla</text>
  <text x="80" y="74" text-anchor="middle" font-size="7" fill="#475569">clientX, clientY</text>
  <text x="80" y="88" text-anchor="middle" font-size="7" fill="#475569">en píxeles CSS</text>
  <text x="80" y="104" text-anchor="middle" font-size="7" fill="#475569">500 × 500 px</text>
  <circle cx="110" cy="118" r="2" fill="#ef4444"/>
  <text x="120" y="122" font-size="6" fill="#991b1b">(250,250)</text>

  <!-- Flecha -->
  <text x="150" y="92" text-anchor="middle" font-size="14" fill="#475569">→</text>

  <!-- ViewBox -->
  <rect x="160" y="42" width="120" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="220" y="58" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">viewBox SVG</text>
  <text x="220" y="74" text-anchor="middle" font-size="7" fill="#475569">coordenadas SVG</text>
  <text x="220" y="88" text-anchor="middle" font-size="7" fill="#475569">unidades del usuario</text>
  <text x="220" y="104" text-anchor="middle" font-size="7" fill="#475569">100 × 100</text>
  <circle cx="220" cy="118" r="2" fill="#10b981"/>
  <text x="230" y="122" font-size="6" fill="#166534">(50,50)</text>

  <rect x="15" y="144" width="270" height="104" fill="#0f172a" rx="3"/>
  <text x="25" y="160" font-size="7" fill="#e2e8f0">• el SVG puede estar escalado</text>
  <text x="25" y="176" font-size="7" fill="#e2e8f0">• puede tener márgenes, padding, transforms CSS</text>
  <text x="25" y="192" font-size="7" fill="#e2e8f0">• puede estar dentro de un div con scroll</text>
  <text x="25" y="208" font-size="7" fill="#fbbf24">clientX no sabe nada de todo eso</text>
  <text x="25" y="224" font-size="7" fill="#94a3b8">→ necesitamos una conversión que considere</text>
  <text x="25" y="238" font-size="7" fill="#94a3b8">   todas las transformaciones acumuladas</text>
</svg>
```

---

## Conversión moderna con `DOMPoint`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    DOMPoint + getScreenCTM().inverse()
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// helper estándar</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">function svgCoords(evt, svg) {</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  const pt = new DOMPoint(</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">    evt.clientX, evt.clientY</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">  );</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#34d399">  return pt.matrixTransform(</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#fbbf24">    svg.getScreenCTM().inverse()</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#34d399">  );</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="194" font-size="7" font-family="monospace" fill="#94a3b8">// uso</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#ec4899">svg.addEventListener('mousemove', e =&gt; {</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#60a5fa">  const {x, y} = svgCoords(e, svg);</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#60a5fa">  cursor.setAttribute('cx', x);</text>
  <text x="25" y="252" font-size="8" font-family="monospace" fill="#ec4899">});</text>
</svg>
```

---

## Qué hace `getScreenCTM().inverse()`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    matriz pantalla→SVG, en un solo paso
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">getScreenCTM()</text>
  <text x="25" y="70" font-size="7" fill="#475569">devuelve la matriz que lleva del espacio del SVG</text>
  <text x="25" y="84" font-size="7" fill="#475569">al espacio de la pantalla (incluye scale por viewBox,</text>
  <text x="25" y="98" font-size="7" fill="#475569">position CSS, transforms del contenedor, etc.)</text>
  <text x="25" y="116" font-size="7" fill="#166534">es la matriz "SVG → pantalla" ya cocinada</text>

  <rect x="15" y="136" width="270" height="112" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="152" font-size="9" font-weight="bold" fill="#1e40af">.inverse()</text>
  <text x="25" y="168" font-size="7" fill="#475569">la invertimos para ir en dirección contraria:</text>
  <text x="25" y="182" font-size="7" fill="#475569">de pantalla a SVG</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#475569">punto_en_svg = punto_pantalla ×</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#475569">               inverse(screenCTM)</text>
  <text x="25" y="234" font-size="7" fill="#1e40af">la matemática la pone matrixTransform()</text>
</svg>
```

---

## Coordenadas relativas a un elemento, no al SVG raíz

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    coords dentro de un &lt;g&gt; transformado
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Caso</text>
  <text x="25" y="70" font-size="7" fill="#475569">Tienes un &lt;g transform="translate(100,50) scale(2)"&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">y quieres coordenadas en el espacio local del grupo,</text>
  <text x="25" y="98" font-size="7" fill="#475569">no del SVG entero (útil para editores zoomables)</text>

  <rect x="15" y="126" width="270" height="122" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// cambiar svg por el elemento destino</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">function coordsEn(evt, el) {</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#34d399">  const pt = new DOMPoint(</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#34d399">    evt.clientX, evt.clientY);</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#34d399">  return pt.matrixTransform(</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">    el.getScreenCTM().inverse()</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#34d399">  );</text>
  <text x="25" y="244" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
</svg>
```

---

## Método legacy: `createSVGPoint()`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    compatibilidad con navegadores antiguos
  </text>

  <rect x="15" y="38" width="270" height="146" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// equivalente pre-DOMPoint</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">function svgCoordsLegacy(evt, svg) {</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  const pt = svg.createSVGPoint();</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">  pt.x = evt.clientX;</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">  pt.y = evt.clientY;</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#34d399">  return pt.matrixTransform(</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#fbbf24">    svg.getScreenCTM().inverse());</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="178" font-size="7" fill="#94a3b8">DOMPoint es más moderno y genérico,</text>

  <rect x="15" y="194" width="270" height="34" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="210" font-size="7" fill="#475569">ambos métodos funcionan en todos los navegadores</text>
  <text x="25" y="222" font-size="7" fill="#166534">modernos desde 2018 — elige DOMPoint para código nuevo</text>
</svg>
```
