# 18.1 SVG inline en HTML

Un SVG **inline** es el que se escribe directamente dentro del HTML, sin archivo externo ni etiqueta contenedora. El parser HTML5 lo reconoce y lo integra en el mismo árbol DOM que el resto de la página.

---

## Sintaxis básica

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG dentro del HTML
  </text>

  <!-- Bloque código -->
  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;body&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;h1&gt;Título&lt;/h1&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;p&gt;Texto&lt;/p&gt;</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">  &lt;svg viewBox="0 0 100 100"&gt;</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">    &lt;circle cx="50" cy="50" r="40"</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#34d399">            fill="steelblue"/&gt;</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#34d399">  &lt;/svg&gt;</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;p&gt;Más texto&lt;/p&gt;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/body&gt;</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#94a3b8">/* el SVG es parte del mismo DOM */</text>
</svg>
```

---

## Acceso completo al DOM

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    JavaScript ve el SVG como HTML más
  </text>

  <!-- JS -->
  <rect x="15" y="38" width="270" height="88" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">// seleccionar elementos internos del SVG</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">const c = document.querySelector('svg circle');</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#e2e8f0">c.setAttribute('fill', 'red');</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#94a3b8">// eventos directos en cualquier forma SVG</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#e2e8f0">c.addEventListener('click', handleClick);</text>

  <!-- Lo que se puede hacer -->
  <rect x="15" y="134" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#166534">✓ Operaciones disponibles</text>
  <text x="25" y="164" font-size="7" fill="#475569">• querySelector / querySelectorAll sobre elementos SVG</text>
  <text x="25" y="176" font-size="7" fill="#475569">• addEventListener (click, hover, focus, drag...)</text>
  <text x="25" y="188" font-size="7" fill="#475569">• createElement / appendChild para insertar formas dinámicamente</text>
  <text x="25" y="200" font-size="7" fill="#475569">• modificación de atributos (fill, stroke, d, transform...)</text>
  <text x="25" y="212" font-size="7" fill="#475569">• getBoundingClientRect para posiciones en pantalla</text>
</svg>
```

---

## Estilado con CSS externo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    CSS externo aplica al SVG inline
  </text>

  <!-- CSS -->
  <rect x="15" y="38" width="270" height="90" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">/* styles.css */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#fbbf24">.boton-icono path {</text>
  <text x="25" y="80" font-size="8" font-family="monospace" fill="#fbbf24">  fill: #64748b;</text>
  <text x="25" y="92" font-size="8" font-family="monospace" fill="#fbbf24">  transition: fill 0.2s;</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#fbbf24">}</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#34d399">.boton-icono:hover path { fill: #3b82f6; }</text>

  <!-- Beneficios -->
  <rect x="15" y="136" width="270" height="92" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="152" font-size="9" font-weight="bold" fill="#1e40af">Características que funcionan</text>
  <text x="25" y="166" font-size="7" fill="#475569">• clases, ids, combinadores descendente/hijo</text>
  <text x="25" y="178" font-size="7" fill="#475569">• pseudo-clases: :hover, :focus, :active</text>
  <text x="25" y="190" font-size="7" fill="#475569">• variables CSS heredadas del documento</text>
  <text x="25" y="202" font-size="7" fill="#475569">• media queries del documento</text>
  <text x="25" y="214" font-size="7" fill="#475569">• animaciones y transiciones CSS</text>
</svg>
```

---

## Ventajas y desventajas

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    balance del SVG inline
  </text>

  <!-- Ventajas -->
  <rect x="15" y="38" width="270" height="110" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ Ventajas</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• acceso completo al DOM desde JS externo</text>
  <text x="25" y="83" font-size="7" fill="#1e293b">• CSS externo aplica sin restricciones</text>
  <text x="25" y="96" font-size="7" fill="#1e293b">• sin petición HTTP adicional</text>
  <text x="25" y="109" font-size="7" fill="#1e293b">• mejor accesibilidad con lectores de pantalla</text>
  <text x="25" y="122" font-size="7" fill="#1e293b">• variables CSS del documento heredadas</text>
  <text x="25" y="135" font-size="7" fill="#1e293b">• eventos directos en cualquier elemento SVG</text>

  <!-- Desventajas -->
  <rect x="15" y="156" width="270" height="100" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="172" font-size="9" font-weight="bold" fill="#991b1b">✗ Desventajas</text>
  <text x="25" y="188" font-size="7" fill="#1e293b">• no se cachea independientemente del HTML</text>
  <text x="25" y="201" font-size="7" fill="#1e293b">• aumenta el peso del HTML</text>
  <text x="25" y="214" font-size="7" fill="#1e293b">• si se repite en varias páginas: descarga duplicada</text>
  <text x="25" y="227" font-size="7" fill="#1e293b">• dificulta la lectura del código HTML fuente</text>
  <text x="25" y="240" font-size="7" fill="#1e293b">• mantenimiento más complejo sin build tools</text>

  <text x="150" y="272" text-anchor="middle" font-size="7" fill="#94a3b8">
    balance: casi siempre vale la pena en apps web modernas
  </text>
</svg>
```

---

## Detalle: el `xmlns` y el parser HTML5

```svg
<svg width="300" height="180"
     viewBox="0 0 300 180"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿necesito xmlns en SVG inline?
  </text>

  <!-- Inline -->
  <rect x="15" y="38" width="270" height="54" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#166534">En SVG inline (dentro de HTML)</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="..."&gt;  /* sin xmlns */</text>
  <text x="25" y="82" font-size="7" fill="#166534">el parser HTML5 añade el namespace automáticamente</text>

  <!-- Archivo -->
  <rect x="15" y="100" width="270" height="58" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="116" font-size="8" font-weight="bold" fill="#991b1b">Como archivo .svg independiente</text>
  <text x="25" y="130" font-size="7" font-family="monospace" fill="#475569">&lt;svg xmlns="http://www.w3.org/2000/svg" ...&gt;</text>
  <text x="25" y="144" font-size="7" fill="#991b1b">obligatorio: el parser XML estricto lo exige</text>
</svg>
```
