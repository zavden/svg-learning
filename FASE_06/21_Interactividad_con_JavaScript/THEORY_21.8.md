# 21.8 Métodos SVG DOM específicos

Además del DOM genérico, SVG expone métodos propios que no existen en HTML: `getTotalLength`, `getPointAtLength`, `isPointInFill`… Son pequeños pero resuelven problemas concretos que con DOM puro serían tediosos.

---

## `getTotalLength()` — longitud del path

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cuánto mide un path
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// devuelve un número en unidades del usuario</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">const path = document.querySelector('path');</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">const L = path.getTotalLength();</text>
  <text x="25" y="102" font-size="7" font-family="monospace" fill="#94a3b8">// ej: 453.27</text>

  <rect x="15" y="126" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="8" font-weight="bold" fill="#94a3b8">Usos frecuentes</text>
  <text x="25" y="158" font-size="7" fill="#e2e8f0">• line drawing: stroke-dasharray = L</text>
  <text x="25" y="172" font-size="7" fill="#e2e8f0">• distribuir elementos uniformemente a lo largo</text>
  <text x="25" y="186" font-size="7" fill="#e2e8f0">• calcular velocidades (L / tiempo)</text>
  <text x="25" y="200" font-size="7" fill="#e2e8f0">• medir recorridos en mapas o flujos</text>
  <text x="25" y="220" font-size="7" fill="#fbbf24">→ sirve para cualquier SVGGeometryElement:</text>
  <text x="25" y="232" font-size="7" fill="#fbbf24">   path, line, circle, rect, polyline…</text>
</svg>
```

---

## `getPointAtLength(d)` — coordenadas a distancia `d`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    punto exacto en cualquier posición del path
  </text>

  <!-- Demo -->
  <path id="demo" d="M 30 100 Q 150 30 270 100" fill="none"
        stroke="#3b82f6" stroke-width="2"/>
  <circle cx="30" cy="100" r="4" fill="#10b981"/>
  <text x="30" y="118" text-anchor="middle" font-size="6" fill="#166534">d=0</text>
  <circle cx="150" cy="55" r="4" fill="#f59e0b"/>
  <text x="150" y="48" text-anchor="middle" font-size="6" fill="#92400e">d=L/2</text>
  <circle cx="270" cy="100" r="4" fill="#ef4444"/>
  <text x="270" y="118" text-anchor="middle" font-size="6" fill="#991b1b">d=L</text>

  <rect x="15" y="136" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#94a3b8">// punto a 50 unidades del inicio</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#60a5fa">const p = path.getPointAtLength(50);</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#34d399">p.x, p.y    // coordenadas SVG</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#94a3b8">// animar un elemento "siguiendo" el path</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">const t = (Date.now() % 2000) / 2000;</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#fbbf24">const q = path.getPointAtLength(t * L);</text>
</svg>
```

---

## Calcular la tangente (rotación "looking ahead")

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    orientar un objeto según la curva
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">La idea</text>
  <text x="25" y="70" font-size="7" fill="#475569">pide dos puntos consecutivos (d y d+1),</text>
  <text x="25" y="84" font-size="7" fill="#475569">calcula el ángulo entre ellos → esa es la tangente</text>
  <text x="25" y="98" font-size="7" fill="#475569">aplicalo como rotate() al objeto que va por el path</text>
  <text x="25" y="114" font-size="7" fill="#166534">equivale a rotate="auto" de animateMotion, pero en JS</text>

  <rect x="15" y="134" width="270" height="102" fill="#0f172a" rx="3"/>
  <text x="25" y="150" font-size="8" font-family="monospace" fill="#60a5fa">function tangente(path, d) {</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#34d399">  const p1 = path.getPointAtLength(d);</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#34d399">  const p2 = path.getPointAtLength(d + 1);</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">  return Math.atan2(</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">    p2.y - p1.y, p2.x - p1.x</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">  ) * 180 / Math.PI;</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
</svg>
```

---

## `isPointInFill()` e `isPointInStroke()`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    hit testing geométrico
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Cuándo se usa</text>
  <text x="25" y="70" font-size="7" fill="#475569">• formas no rectangulares donde getBBox no basta</text>
  <text x="25" y="84" font-size="7" fill="#475569">• colisiones en juegos / visualizaciones</text>
  <text x="25" y="98" font-size="7" fill="#475569">• "está el mouse dentro de esta forma recortada?"</text>

  <rect x="15" y="126" width="270" height="122" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// crear un punto en coordenadas SVG</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">const pt = new DOMPoint(50, 50);</text>

  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">// consultar</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#34d399">shape.isPointInFill(pt)   // true/false</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#34d399">shape.isPointInStroke(pt) // true/false</text>
  <text x="25" y="228" font-size="7" fill="#94a3b8">funciona para path, circle, rect, polygon…</text>
  <text x="25" y="240" font-size="7" fill="#94a3b8">que hereden de SVGGeometryElement</text>
</svg>
```

---

## `createSVGPoint()` y `createSVGMatrix()`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    fábricas del &lt;svg&gt; raíz
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// punto SVG — alternativa a DOMPoint</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">const pt = svg.createSVGPoint();</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">pt.x = 100; pt.y = 50;</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#fbbf24">pt.matrixTransform(svg.getScreenCTM().inverse())</text>
  <text x="25" y="122" font-size="7" font-family="monospace" fill="#94a3b8">// equivalente moderno: new DOMPoint(100, 50)</text>

  <rect x="15" y="142" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">// matriz SVG para composiciones</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">const m = svg.createSVGMatrix();</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#94a3b8">// métodos encadenables</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">m.translate(10, 20).scale(2).rotate(45)</text>
  <text x="25" y="230" font-size="7" fill="#94a3b8">útil si construyes transformaciones complejas</text>
  <text x="25" y="242" font-size="7" fill="#94a3b8">paso a paso en código</text>
</svg>
```
