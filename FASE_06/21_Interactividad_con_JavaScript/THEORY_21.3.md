# 21.3 Creación dinámica de elementos SVG

Crear un elemento SVG desde JavaScript no se hace con `document.createElement`. Hay que usar `createElementNS` y darle el namespace correcto. Olvidar el namespace es probablemente el bug más común al empezar con SVG programático.

---

## El namespace es obligatorio

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    createElement vs createElementNS
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">MAL — createElement</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#991b1b">const c = document.createElement('circle');</text>
  <text x="25" y="84" font-size="7" fill="#475569">→ crea un elemento HTML &lt;circle&gt; desconocido</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ NO se renderiza como círculo SVG</text>
  <text x="25" y="114" font-size="7" fill="#475569">→ aparece en el DOM pero está "muerto" visualmente</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">es el error #1 al crear SVG programático</text>

  <rect x="15" y="142" width="270" height="128" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="158" font-size="9" font-weight="bold" fill="#166534">BIEN — createElementNS</text>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#475569">const NS = 'http://www.w3.org/2000/svg';</text>
  <text x="25" y="188" font-size="7" font-family="monospace" fill="#475569">const c = document.createElementNS(NS, 'circle');</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#475569">c.setAttribute('cx', 50);</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#475569">c.setAttribute('cy', 50);</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#475569">c.setAttribute('r', 30);</text>
  <text x="25" y="244" font-size="7" font-family="monospace" fill="#475569">svg.appendChild(c);</text>
  <text x="25" y="260" font-size="7" fill="#166534">→ crea un SVGCircleElement real, renderizable</text>
</svg>
```

---

## Crear cualquier tipo de elemento

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    path, text, g — el mismo patrón
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">const NS = 'http://www.w3.org/2000/svg';</text>

  <text x="25" y="76" font-size="7" font-family="monospace" fill="#94a3b8">// path</text>
  <text x="25" y="90" font-size="8" font-family="monospace" fill="#60a5fa">const p = document.createElementNS(NS,'path');</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#60a5fa">p.setAttribute('d', 'M 10 10 L 90 90');</text>

  <text x="25" y="124" font-size="7" font-family="monospace" fill="#94a3b8">// text</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#34d399">const t = document.createElementNS(NS,'text');</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#34d399">t.setAttribute('x', 50);</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#34d399">t.textContent = 'Hola SVG';</text>

  <text x="25" y="186" font-size="7" font-family="monospace" fill="#94a3b8">// grupo</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#fbbf24">const g = document.createElementNS(NS,'g');</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#fbbf24">g.classList.add('barra');</text>
  <text x="25" y="228" font-size="8" font-family="monospace" fill="#fbbf24">g.appendChild(p);</text>
  <text x="25" y="242" font-size="8" font-family="monospace" fill="#fbbf24">g.appendChild(t);</text>
  <text x="25" y="256" font-size="8" font-family="monospace" fill="#ec4899">svg.appendChild(g);</text>
</svg>
```

---

## Insertar en `<defs>` dinámicamente

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    gradientes y patrones en &lt;defs&gt;
  </text>

  <rect x="15" y="38" width="270" height="212" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// asegurar que existe &lt;defs&gt;</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#e2e8f0">let defs = svg.querySelector('defs');</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#e2e8f0">if (!defs) {</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#60a5fa">  defs = document.createElementNS(NS,'defs');</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#60a5fa">  svg.insertBefore(defs, svg.firstChild);</text>
  <text x="25" y="126" font-size="7" font-family="monospace" fill="#e2e8f0">}</text>

  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">// crear un gradiente</text>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#34d399">const grad = document.createElementNS(NS,</text>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#34d399">  'linearGradient');</text>
  <text x="25" y="188" font-size="7" font-family="monospace" fill="#34d399">grad.id = 'miGrad';</text>

  <text x="25" y="206" font-size="7" font-family="monospace" fill="#fbbf24">const stop = document.createElementNS(NS,'stop');</text>
  <text x="25" y="218" font-size="7" font-family="monospace" fill="#fbbf24">stop.setAttribute('offset','0%');</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#fbbf24">stop.setAttribute('stop-color','red');</text>
  <text x="25" y="242" font-size="7" font-family="monospace" fill="#ec4899">grad.appendChild(stop); defs.appendChild(grad);</text>
</svg>
```

---

## Clonar elementos existentes

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cloneNode para replicar plantillas
  </text>

  <rect x="15" y="38" width="270" height="192" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// clonar todo (hijos incluidos)</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">const original = svg.querySelector('#icono');</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#60a5fa">const clon = original.cloneNode(true);</text>

  <text x="25" y="106" font-size="7" font-family="monospace" fill="#94a3b8">// siempre cambiar el id</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#fbbf24">clon.id = 'icono-2';</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#fbbf24">clon.setAttribute('x', 120);</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#ec4899">svg.appendChild(clon);</text>

  <text x="25" y="172" font-size="7" fill="#94a3b8">cloneNode(true) → copia profunda (hijos)</text>
  <text x="25" y="186" font-size="7" fill="#94a3b8">cloneNode(false) → solo el nodo raíz</text>
  <text x="25" y="206" font-size="7" fill="#fbbf24">útil para replicar "plantillas" de UI</text>
  <text x="25" y="220" font-size="7" fill="#fbbf24">(ej: puntos de un scatter plot)</text>
</svg>
```

---

## Helper recomendado

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    reduce la verbosidad con un helper
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// función de conveniencia</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">const NS = 'http://www.w3.org/2000/svg';</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#60a5fa">function svgEl(tag, attrs = {}) {</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">  const el = document.createElementNS(NS, tag);</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">  for (const [k, v] of Object.entries(attrs))</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#34d399">    el.setAttribute(k, v);</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#34d399">  return el;</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="176" font-size="7" font-family="monospace" fill="#94a3b8">// uso</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#fbbf24">svg.appendChild(svgEl('circle', {</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#fbbf24">  cx: 50, cy: 50, r: 20, fill: 'red'</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#fbbf24">}));</text>
  <text x="25" y="238" font-size="7" fill="#94a3b8">→ mucho más legible que createElementNS a pelo</text>
</svg>
```
