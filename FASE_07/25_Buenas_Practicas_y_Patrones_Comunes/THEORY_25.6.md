# 25.6 Patrones de diseño comunes

Los "patrones" son soluciones ya digeridas a problemas que aparecen una y otra vez: un spinner, una barra de progreso, un icono con estado, un mapa interactivo. Conocerlos ahorra el coste de reinventarlos cada vez — y evita los errores que ya cometió otra persona por ti.

---

## Spinner / loader

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    círculo con stroke-dasharray y rotación
  </text>

  <rect x="15" y="38" width="270" height="98" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Componentes</text>
  <text x="25" y="70" font-size="7" fill="#475569">• &lt;circle&gt; con fill="none" y solo stroke</text>
  <text x="25" y="84" font-size="7" fill="#475569">• stroke-dasharray = longitud de la circunferencia</text>
  <text x="25" y="98" font-size="7" fill="#475569">• stroke-dashoffset parcial → arco visible</text>
  <text x="25" y="112" font-size="7" fill="#475569">• rotación CSS continua del grupo contenedor</text>
  <text x="25" y="126" font-size="7" fill="#166534">→ 2 animaciones componen el efecto</text>

  <rect x="15" y="146" width="270" height="124" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">// SVG</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#60a5fa">&lt;svg viewBox="0 0 50 50" class="spin"&gt;</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#34d399">  &lt;circle cx="25" cy="25" r="20"</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#34d399">    fill="none" stroke="#3b82f6"</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#34d399">    stroke-width="4"</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#34d399">    stroke-dasharray="90 60"/&gt;</text>
  <text x="25" y="256" font-size="7" font-family="monospace" fill="#fbbf24">.spin { animation: rot 1s linear infinite }</text>
</svg>
```

---

## Barra de progreso

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    línea base + línea de progreso
  </text>

  <rect x="15" y="38" width="270" height="48" fill="#e2e8f0" rx="3"/>
  <rect x="15" y="38" width="160" height="48" fill="#10b981" rx="3"/>
  <text x="95" y="68" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">60%</text>

  <rect x="15" y="98" width="270" height="168" fill="#0f172a" rx="3"/>
  <text x="25" y="114" font-size="7" font-family="monospace" fill="#94a3b8">// rect + rect con transition</text>
  <text x="25" y="130" font-size="7" font-family="monospace" fill="#60a5fa">&lt;svg viewBox="0 0 200 20"&gt;</text>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#34d399">  &lt;rect width="200" height="20"</text>
  <text x="25" y="160" font-size="7" font-family="monospace" fill="#34d399">    fill="#e5e7eb" rx="10"/&gt;</text>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#34d399">  &lt;rect width="120" height="20"</text>
  <text x="25" y="188" font-size="7" font-family="monospace" fill="#34d399">    fill="#10b981" rx="10"</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#34d399">    id="bar"/&gt;</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#94a3b8">// JS — animar width</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#fbbf24">bar.setAttribute('width', pct * 2)</text>
  <text x="25" y="256" font-size="7" font-family="monospace" fill="#94a3b8">// o mejor: transform scaleX + transition</text>
</svg>
```

---

## Icono con estado (heart empty / filled)

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dos paths superpuestos, toggle con CSS
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// SVG con 2 paths</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#60a5fa">&lt;svg viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#34d399">  &lt;path class="contorno"</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#34d399">    d="..." fill="none"</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#34d399">    stroke="currentColor"/&gt;</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#fbbf24">  &lt;path class="relleno"</text>

  <rect x="15" y="146" width="270" height="124" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">// CSS</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#60a5fa">.relleno {</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#34d399">  opacity: 0;</text>
  <text x="25" y="206" font-size="7" font-family="monospace" fill="#34d399">  transition: opacity .2s;</text>
  <text x="25" y="220" font-size="7" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#60a5fa">.activo .relleno {</text>
  <text x="25" y="254" font-size="7" font-family="monospace" fill="#ec4899">  opacity: 1;</text>
  <text x="25" y="268" font-size="7" font-family="monospace" fill="#60a5fa">}</text>
</svg>
```

---

## Mapa interactivo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    paths con id, eventos en el grupo padre
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Estructura</text>
  <text x="25" y="70" font-size="7" fill="#475569">• cada región = un &lt;path&gt; con id único</text>
  <text x="25" y="84" font-size="7" fill="#475569">• data-attrs para metadata (nombre, valor)</text>
  <text x="25" y="98" font-size="7" fill="#475569">• :hover en CSS para feedback visual</text>
  <text x="25" y="112" font-size="7" fill="#475569">• un solo listener en el &lt;g&gt; padre (delegación)</text>
  <text x="25" y="128" font-size="7" fill="#166534">→ N listeners por N regiones = mal</text>

  <rect x="15" y="152" width="270" height="118" fill="#0f172a" rx="3"/>
  <text x="25" y="168" font-size="7" font-family="monospace" fill="#60a5fa">&lt;g id="regiones"&gt;</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#34d399">  &lt;path id="r-norte"</text>
  <text x="25" y="198" font-size="7" font-family="monospace" fill="#34d399">    data-name="Norte" d="..."/&gt;</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#34d399">  &lt;path id="r-sur" ...</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#60a5fa">&lt;/g&gt;</text>
  <text x="25" y="248" font-size="7" font-family="monospace" fill="#fbbf24">regiones.addEventListener('click',</text>
  <text x="25" y="262" font-size="7" font-family="monospace" fill="#fbbf24">  e =&gt; mostrar(e.target.dataset))</text>
</svg>
```

---

## Logo responsivo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    2 variantes en el mismo SVG
  </text>

  <rect x="15" y="38" width="270" height="76" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Idea</text>
  <text x="25" y="70" font-size="7" fill="#475569">• logo completo (isotipo + wordmark) como default</text>
  <text x="25" y="84" font-size="7" fill="#475569">• isotipo solo cuando hay poco espacio</text>
  <text x="25" y="98" font-size="7" fill="#475569">• toggle con media query en el CSS del SVG</text>

  <rect x="15" y="122" width="270" height="148" fill="#0f172a" rx="3"/>
  <text x="25" y="138" font-size="7" font-family="monospace" fill="#60a5fa">&lt;svg viewBox="0 0 200 60"&gt;</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#34d399">  &lt;style&gt;</text>
  <text x="25" y="166" font-size="7" font-family="monospace" fill="#fbbf24">   .compact { display: none }</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#fbbf24">   @media (max-width: 200px) {</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#ec4899">    .full { display: none }</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#ec4899">    .compact { display: block }</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#fbbf24">   }</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#34d399">  &lt;/style&gt;</text>
  <text x="25" y="250" font-size="7" font-family="monospace" fill="#34d399">  &lt;g class="full"&gt;...&lt;/g&gt;</text>
  <text x="25" y="264" font-size="7" font-family="monospace" fill="#34d399">  &lt;g class="compact"&gt;...&lt;/g&gt;</text>
</svg>
```

---

## Texto en curva con `<textPath>`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    texto que sigue un path
  </text>

  <defs>
    <path id="curva" d="M 30,140 Q 150,60 270,140" fill="none"/>
  </defs>
  <path d="M 30,140 Q 150,60 270,140" fill="none" stroke="#cbd5e1" stroke-dasharray="3 3"/>
  <text font-size="14" font-weight="bold" fill="#3b82f6">
    <textPath href="#curva">Texto en curva con textPath</textPath>
  </text>

  <rect x="15" y="170" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#60a5fa">&lt;defs&gt;</text>
  <text x="25" y="200" font-size="7" font-family="monospace" fill="#34d399">  &lt;path id="curva"</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#34d399">    d="M 20,60 Q 100,10 180,60"/&gt;</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#60a5fa">&lt;/defs&gt;</text>
  <text x="25" y="248" font-size="7" font-family="monospace" fill="#60a5fa">&lt;text&gt;</text>
  <text x="25" y="262" font-size="7" font-family="monospace" fill="#fbbf24">  &lt;textPath href="#curva"&gt;...</text>
</svg>
```

---

## Gráfico de barras desde datos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    20 líneas de JS vs importar D3
  </text>

  <g transform="translate(40 50)">
    <rect x="0" y="60" width="30" height="60" fill="#3b82f6"/>
    <rect x="40" y="20" width="30" height="100" fill="#3b82f6"/>
    <rect x="80" y="40" width="30" height="80" fill="#3b82f6"/>
    <rect x="120" y="0" width="30" height="120" fill="#3b82f6"/>
    <rect x="160" y="30" width="30" height="90" fill="#3b82f6"/>
    <line x1="-5" y1="120" x2="220" y2="120" stroke="#475569"/>
  </g>

  <rect x="15" y="184" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="200" font-size="7" font-family="monospace" fill="#94a3b8">// datos → rects, sin librería</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#60a5fa">const datos = [30, 70, 45, 90, 60]</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#34d399">datos.forEach((v, i) =&gt; {</text>
  <text x="25" y="244" font-size="7" font-family="monospace" fill="#fbbf24">  const r = createElementNS(NS,'rect')</text>
  <text x="25" y="258" font-size="7" font-family="monospace" fill="#fbbf24">  r.setAttribute('y', maxH - v)</text>
</svg>
```
