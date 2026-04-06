# 19.6 Variables CSS (Custom Properties) en SVG

Las variables CSS son la herramienta más potente para hacer SVGs **tematizables**. Permiten que un mismo SVG cambie de color, tamaño o estilo según el contexto en el que se use, sin tocar el archivo fuente.

---

## Definir variables dentro del SVG

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    variables en :root del SVG
  </text>

  <rect x="15" y="38" width="270" height="194" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox="0 0 100 100"&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;style&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#fbbf24">    :root {</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">      --color-primario: steelblue;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">      --color-acento: #e74c3c;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#34d399">      --grosor: 2;</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#fbbf24">    }</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#fbbf24">    circle {</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">      fill: var(--color-primario);</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#34d399">      stroke: var(--color-acento);</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">      stroke-width: var(--grosor);</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#fbbf24">    }</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/style&gt;</text>
</svg>
```

---

## Variables que cruzan la frontera HTML→SVG

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el SVG inline hereda las variables del HTML
  </text>

  <!-- HTML CSS -->
  <rect x="15" y="38" width="270" height="78" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* hoja CSS del documento */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">:root {</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  --color-marca: #005ea5;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  --radio-icono: 40px;</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <!-- SVG inline -->
  <rect x="15" y="124" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="140" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- SVG inline --&gt;</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg&gt;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;circle</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#fbbf24">    style="fill: var(--color-marca);"</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#e2e8f0">  /&gt;</text>

  <rect x="15" y="212" width="270" height="40" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="228" font-size="7" font-weight="bold" fill="#166534">solo funciona con SVG inline</text>
  <text x="25" y="242" font-size="6" fill="#475569">si lo cargas con &lt;img&gt; las variables NO atraviesan el sandbox</text>
</svg>
```

---

## Tematización por contexto

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el mismo SVG, distintos temas
  </text>

  <rect x="15" y="38" width="270" height="232" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* SVG fuente */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#fbbf24">  fill="var(--c-relleno)"</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#fbbf24">  stroke="var(--c-trazo)"</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#e2e8f0">/&gt;</text>

  <text x="25" y="132" font-size="7" font-family="monospace" fill="#94a3b8">/* tema por defecto */</text>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#60a5fa">.componente svg {</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#34d399">  --c-relleno: white;</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#34d399">  --c-trazo: #333;</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="206" font-size="7" font-family="monospace" fill="#94a3b8">/* override por estado */</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#60a5fa">.componente:hover svg {</text>
  <text x="25" y="234" font-size="8" font-family="monospace" fill="#ec4899">  --c-relleno: steelblue;</text>
  <text x="25" y="248" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="262" font-size="7" fill="#94a3b8">cero JS, cero clases extra en el SVG</text>
</svg>
```

---

## Valor por defecto en `var()`

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    var(--nombre, fallback)
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#fbbf24">  fill="var(--c-relleno, steelblue)"</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#e2e8f0">/&gt;</text>
  <text x="25" y="100" font-size="7" fill="#94a3b8">si --c-relleno no está definido, usa steelblue</text>

  <rect x="15" y="128" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="144" font-size="9" font-weight="bold" fill="#166534">Por qué importa</text>
  <text x="25" y="160" font-size="7" fill="#475569">• el SVG funciona "por defecto" sin configuración previa</text>
  <text x="25" y="174" font-size="7" fill="#475569">• cualquiera puede sobrescribirlo cuando lo necesite</text>
  <text x="25" y="188" font-size="7" fill="#475569">• evita "el SVG no se ve" si olvidas declarar la variable</text>
  <text x="25" y="202" font-size="7" fill="#166534">→ patrón "API pública" del SVG: documenta las vars que acepta</text>
</svg>
```

---

## Variables y `<use>`: el truco del bicolor

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada &lt;use&gt; con sus propios colores
  </text>

  <rect x="15" y="38" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* sprite con dos colores parametrizados */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;symbol id="bicolor" viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;rect fill="var(--c1, currentColor)"/&gt;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;circle fill="var(--c2, currentColor)"/&gt;</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/symbol&gt;</text>
  <text x="25" y="130" font-size="7" font-family="monospace" fill="#94a3b8">/* las vars cruzan la frontera shadow DOM */</text>

  <rect x="15" y="152" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="168" font-size="7" font-family="monospace" fill="#94a3b8">/* cada instancia distinta */</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;use href="#bicolor"</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">  style="--c1: red; --c2: blue;"/&gt;</text>

  <text x="25" y="216" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;use href="#bicolor"</text>
  <text x="25" y="230" font-size="8" font-family="monospace" fill="#ec4899">  style="--c1: green; --c2: yellow;"/&gt;</text>
  <text x="25" y="252" font-size="7" fill="#94a3b8">los selectores CSS NO entran en &lt;use&gt;,</text>
  <text x="25" y="262" font-size="7" fill="#94a3b8">pero las custom properties SÍ se heredan</text>
</svg>
```

---

## Patrón completo: API de un ícono

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    documenta las variables como API
  </text>

  <rect x="15" y="38" width="270" height="194" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* archivo: mi-icono.svg */</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#94a3b8">/* Variables públicas:                    */</text>
  <text x="25" y="80" font-size="7" font-family="monospace" fill="#94a3b8">/*  --icono-color:    color principal      */</text>
  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8">/*  --icono-acento:   color secundario     */</text>
  <text x="25" y="104" font-size="7" font-family="monospace" fill="#94a3b8">/*  --icono-trazo:    grosor del trazo     */</text>
  <text x="25" y="120" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="134" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;path</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#34d399">    fill="var(--icono-color, #1e293b)"</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#34d399">    stroke="var(--icono-acento, none)"</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#34d399">    stroke-width="var(--icono-trazo, 0)"</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#fbbf24">    d="..."</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#e2e8f0">  /&gt;</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>
</svg>
```
