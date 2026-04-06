# 21.2 Manipulación de atributos

Hay varias formas de cambiar un atributo SVG desde JavaScript: `setAttribute`, la propiedad `.style`, `classList` o la interfaz tipada (`cx.baseVal.value`). Cada una tiene su momento — pero en la práctica moderna, `setAttribute` resuelve el 90% de los casos.

---

## Leer y escribir con `getAttribute` / `setAttribute`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la API universal del DOM
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// leer</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">circle.getAttribute('cx')</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#94a3b8">  → "50" (siempre string)</text>

  <text x="25" y="100" font-size="8" font-family="monospace" fill="#60a5fa">circle.hasAttribute('display')</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#94a3b8">  → true o false</text>

  <text x="25" y="128" font-size="7" fill="#fbbf24">* parseFloat() si necesitas el número</text>

  <rect x="15" y="146" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">// escribir</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#34d399">circle.setAttribute('cx', 100)</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">circle.setAttribute('fill', 'red')</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#ec4899">circle.removeAttribute('stroke')</text>
  <text x="25" y="228" font-size="7" fill="#94a3b8">setAttribute convierte todo a string automáticamente</text>
</svg>
```

---

## Estilos inline con `.style`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    propiedad .style (camelCase)
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// aplicar estilos inline</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">circle.style.fill = 'red';</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#60a5fa">circle.style.strokeWidth = '2px';</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#60a5fa">circle.style.opacity = '0.5';</text>
  <text x="25" y="122" font-size="7" fill="#fbbf24">stroke-width → strokeWidth (camelCase)</text>

  <rect x="15" y="146" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">// leer estilo computado (valor final tras cascada)</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#34d399">getComputedStyle(circle).fill</text>
  <text x="25" y="198" font-size="7" font-family="monospace" fill="#94a3b8">// limpiar un estilo inline</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#ec4899">circle.style.fill = '';</text>
</svg>
```

---

## Clases con `classList`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la forma idiomática
  </text>

  <rect x="15" y="38" width="270" height="200" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// añadir / quitar</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">circle.classList.add('activo')</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">circle.classList.add('grande','rojo')</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#ec4899">circle.classList.remove('activo')</text>

  <text x="25" y="118" font-size="7" font-family="monospace" fill="#94a3b8">// alternar</text>
  <text x="25" y="134" font-size="8" font-family="monospace" fill="#fbbf24">circle.classList.toggle('seleccionado')</text>

  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">// comprobar</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#60a5fa">circle.classList.contains('activo')</text>

  <text x="25" y="190" font-size="7" font-family="monospace" fill="#94a3b8">// reemplazar</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#60a5fa">circle.classList.replace('a','b')</text>

  <text x="25" y="228" font-size="7" fill="#fbbf24">→ patrón recomendado: clases CSS para estados,</text>
  <text x="25" y="240" font-size="7" fill="#fbbf24">   JS solo cambia la clase</text>
</svg>
```

---

## Interfaz tipada `SVGAnimatedLength`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    acceso tipado: baseVal y animVal
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// números, no strings</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">circle.cx.baseVal.value = 100;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">circle.r.baseVal.value  = 30;</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#60a5fa">const x = circle.cx.baseVal.value;</text>
  <text x="25" y="116" font-size="7" font-family="monospace" fill="#94a3b8">// x es un number, no "100"</text>

  <rect x="15" y="142" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="8" font-weight="bold" fill="#94a3b8">baseVal vs animVal</text>
  <text x="25" y="174" font-size="7" fill="#e2e8f0">baseVal → el valor definido en el atributo</text>
  <text x="25" y="188" font-size="7" fill="#e2e8f0">animVal → el valor actual incluyendo animaciones</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#60a5fa">circle.cx.baseVal.value // 50</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#60a5fa">circle.cx.animVal.value // 73.4 (mid-anim)</text>
  <text x="25" y="240" font-size="7" fill="#fbbf24">útil si inspeccionas animaciones SMIL</text>
</svg>
```

---

## `getBBox()` — caja envolvente

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la bounding box del elemento
  </text>

  <!-- Demo visual -->
  <g transform="translate(40 50)">
    <path d="M 0 20 Q 30 0 60 20 T 120 20" fill="none" stroke="#3b82f6" stroke-width="3"/>
    <rect x="0" y="0" width="120" height="40" fill="none" stroke="#ef4444"
          stroke-width="1" stroke-dasharray="3 3"/>
    <text x="60" y="56" text-anchor="middle" font-size="7" fill="#991b1b">getBBox() → ese rectángulo rojo</text>
  </g>

  <rect x="15" y="118" width="270" height="114" fill="#0f172a" rx="3"/>
  <text x="25" y="134" font-size="8" font-family="monospace" fill="#60a5fa">const bbox = path.getBBox();</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#94a3b8">// SVGRect con x, y, width, height</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">bbox.x, bbox.y</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#34d399">bbox.width, bbox.height</text>
  <text x="25" y="202" font-size="7" fill="#fbbf24">* coordenadas locales del elemento</text>
  <text x="25" y="214" font-size="7" fill="#fbbf24">  (sin transformaciones del padre)</text>
  <text x="25" y="226" font-size="7" fill="#94a3b8">útil para centrar, colisiones, tooltips</text>
</svg>
```

---

## Matrices: `getCTM()` y `getScreenCTM()`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    matrices de transformación acumuladas
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// transformaciones del elemento</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">const ctm = elem.getCTM();</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#94a3b8">// SVGMatrix con a, b, c, d, e, f</text>
  <text x="25" y="104" font-size="7" fill="#fbbf24">→ para operaciones matemáticas avanzadas</text>

  <rect x="15" y="126" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// matriz relativa a la pantalla</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#34d399">const screen = svg.getScreenCTM();</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">// el gran uso práctico:</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#94a3b8">// convertir mouse → coordenadas SVG</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#fbbf24">point.matrixTransform(screen.inverse())</text>
  <text x="25" y="226" font-size="7" fill="#94a3b8">(se verá en detalle en 21.5)</text>
</svg>
```
