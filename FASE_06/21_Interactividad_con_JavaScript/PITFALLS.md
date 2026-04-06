# PITFALLS — Interactividad con JavaScript

Los errores más frecuentes al mezclar JavaScript con SVG. Casi todos vienen de olvidar que SVG tiene su propio namespace, su propio sistema de coordenadas, y un comportamiento un poco distinto al HTML clásico.

---

## 1. `createElement` en vez de `createElementNS`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 1 — el elemento no se ve
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">creas un círculo desde JS, aparece en el DOM,</text>
  <text x="25" y="84" font-size="7" fill="#475569">pero no se renderiza por ningún lado</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">motivo: usaste createElement (HTML) en vez</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">de createElementNS (el namespace SVG)</text>

  <rect x="15" y="134" width="270" height="114" fill="#0f172a" rx="3"/>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL */</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#f87171">const c = document.createElement('circle');</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">→ crea un HTMLUnknownElement, no un círculo</text>

  <text x="25" y="200" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN */</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#34d399">const NS = 'http://www.w3.org/2000/svg';</text>
  <text x="25" y="228" font-size="8" font-family="monospace" fill="#34d399">const c = document.createElementNS(NS,'circle');</text>
  <text x="25" y="242" font-size="7" fill="#fbbf24">→ SVGCircleElement real, renderiza correctamente</text>
</svg>
```

---

## 2. Olvidar convertir coordenadas del mouse

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 2 — los clicks caen en el sitio incorrecto
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">al hacer click, el elemento que dibujas aparece</text>
  <text x="25" y="84" font-size="7" fill="#475569">desplazado, escalado, o lejos del cursor</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">usaste clientX/clientY directamente como coord SVG</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">pero el viewBox no coincide con los pixels de pantalla</text>

  <rect x="15" y="142" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL */</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#f87171">circle.setAttribute('cx', e.clientX);</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#f87171">circle.setAttribute('cy', e.clientY);</text>

  <text x="25" y="206" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN */</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">const pt = new DOMPoint(e.clientX, e.clientY);</text>
  <text x="25" y="234" font-size="8" font-family="monospace" fill="#34d399">const p = pt.matrixTransform(</text>
  <text x="25" y="246" font-size="8" font-family="monospace" fill="#fbbf24">  svg.getScreenCTM().inverse());</text>
</svg>
```

---

## 3. `getAttribute` devuelve string, no número

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 3 — concatenación en vez de suma
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">incrementas `cx` y el círculo se vuelve loco:</text>
  <text x="25" y="84" font-size="7" fill="#475569">50 → 501 → 5011 → NaN</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">getAttribute siempre devuelve string</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">"50" + 1 === "501" (concatenación, no suma)</text>

  <rect x="15" y="138" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL */</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#f87171">const cx = circle.getAttribute('cx');</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#f87171">circle.setAttribute('cx', cx + 1); // "501"</text>

  <text x="25" y="204" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN */</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#34d399">const cx = parseFloat(circle.getAttribute('cx'));</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#34d399">circle.setAttribute('cx', cx + 1); // 51</text>
  <text x="25" y="246" font-size="7" fill="#fbbf24">alternativa: circle.cx.baseVal.value (ya es number)</text>
</svg>
```

---

## 4. Drag sin `setPointerCapture`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 4 — el drag "se escapa"
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">si mueves el mouse rápido mientras arrastras,</text>
  <text x="25" y="84" font-size="7" fill="#475569">el elemento "se queda atrás" y deja de seguirte</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">el cursor salió del elemento → pointermove ya no</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">dispara en él → el drag se congela</text>

  <rect x="15" y="142" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL — pointermove en el propio elemento */</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#f87171">el.addEventListener('pointerdown', e =&gt; {</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#f87171">  /* ... no capture ... */</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#f87171">});</text>

  <text x="25" y="216" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN */</text>
  <text x="25" y="230" font-size="8" font-family="monospace" fill="#34d399">el.addEventListener('pointerdown', e =&gt; {</text>
  <text x="25" y="244" font-size="8" font-family="monospace" fill="#fbbf24">  el.setPointerCapture(e.pointerId);</text>
</svg>
```

---

## 5. `<script>` en SVG sin `<![CDATA[ ]]>`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 5 — "parsing error" al abrir el SVG
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">guardas un .svg autónomo con un script y al abrirlo</text>
  <text x="25" y="84" font-size="7" fill="#475569">en el navegador aparece "XML parsing error"</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">el JS usa caracteres XML reservados (`&lt;`, `&amp;`, `&gt;`)</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">y el parser XML los interpreta mal</text>

  <rect x="15" y="138" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL */</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#f87171">&lt;script&gt;if (a &lt; b) { ... }&lt;/script&gt;</text>
  <text x="25" y="182" font-size="7" fill="#94a3b8">→ el parser cree que `&lt; b)` abre una etiqueta</text>

  <text x="25" y="204" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN */</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#34d399">&lt;script&gt;&lt;![CDATA[</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#34d399">  if (a &lt; b) { ... }</text>
  <text x="25" y="246" font-size="8" font-family="monospace" fill="#34d399">]]&gt;&lt;/script&gt;</text>
</svg>
```

---

## Resumen

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    checklist mental
  </text>

  <rect x="15" y="38" width="270" height="148" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" fill="#34d399">✓ crear elementos siempre con createElementNS</text>
  <text x="25" y="78" font-size="8" fill="#34d399">✓ convertir mouse → SVG con screenCTM().inverse()</text>
  <text x="25" y="98" font-size="8" fill="#34d399">✓ parseFloat() al leer atributos numéricos</text>
  <text x="25" y="118" font-size="8" fill="#34d399">✓ setPointerCapture en todo pointerdown de drag</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#34d399">✓ CDATA en scripts de .svg autónomos</text>
  <text x="25" y="158" font-size="8" fill="#fbbf24">y recuerda: los SVG inline NO son HTMLElement</text>
  <text x="25" y="174" font-size="7" fill="#94a3b8">(algunas libs que asumen HTML fallan aquí)</text>
</svg>
```
