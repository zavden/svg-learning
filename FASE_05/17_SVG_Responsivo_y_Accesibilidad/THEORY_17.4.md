# 17.4 Accesibilidad en SVG

Un SVG transmite información visual que puede ser **completamente inaccesible** para usuarios de lectores de pantalla, personas con baja visión o quienes navegan solo con teclado. La accesibilidad no es opcional: es parte de la calidad del producto.

---

## El patrón completo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    anatomía de un SVG accesible
  </text>

  <!-- Bloque de código -->
  <rect x="15" y="38" width="270" height="190" fill="#0f172a" rx="3"/>

  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg</text>
  <text x="40" y="70" font-size="8" font-family="monospace" fill="#fbbf24">  role="img"</text>
  <text x="165" y="70" font-size="7" fill="#64748b">← "soy una imagen"</text>
  <text x="40" y="84" font-size="8" font-family="monospace" fill="#fbbf24">  aria-labelledby=</text>
  <text x="40" y="96" font-size="8" font-family="monospace" fill="#fbbf24">    "titulo desc"</text>
  <text x="165" y="90" font-size="7" fill="#64748b">← conecta</text>
  <text x="165" y="100" font-size="7" fill="#64748b">   título + desc</text>
  <text x="40" y="110" font-size="8" font-family="monospace" fill="#60a5fa">  viewBox="0 0 200 100"&gt;</text>

  <text x="40" y="128" font-size="8" font-family="monospace" fill="#34d399">  &lt;title id="titulo"&gt;</text>
  <text x="50" y="140" font-size="8" font-family="monospace" fill="#34d399">    Ventas por trimestre 2024</text>
  <text x="40" y="152" font-size="8" font-family="monospace" fill="#34d399">  &lt;/title&gt;</text>

  <text x="40" y="168" font-size="8" font-family="monospace" fill="#c084fc">  &lt;desc id="desc"&gt;</text>
  <text x="50" y="180" font-size="8" font-family="monospace" fill="#c084fc">    T1: 45, T2: 62, T3: 78, T4: 91.</text>
  <text x="50" y="192" font-size="8" font-family="monospace" fill="#c084fc">    Tendencia ascendente.</text>
  <text x="40" y="204" font-size="8" font-family="monospace" fill="#c084fc">  &lt;/desc&gt;</text>

  <text x="40" y="218" font-size="8" font-family="monospace" fill="#94a3b8">  &lt;!-- contenido visual --&gt;</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#94a3b8"> </text>
</svg>
```

---

## El atributo `role="img"`

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿por qué role="img" es necesario?
  </text>

  <!-- Sin role -->
  <rect x="15" y="38" width="270" height="72" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ Sin role="img"</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="..."&gt;&lt;circle .../&gt;&lt;/svg&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">• algunos lectores lo ignoran por completo</text>
  <text x="25" y="96" font-size="7" fill="#475569">• otros leen las etiquetas internas de forma caótica</text>

  <!-- Con role -->
  <rect x="15" y="118" width="270" height="72" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="134" font-size="9" font-weight="bold" fill="#166534">✓ Con role="img"</text>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#475569">&lt;svg role="img" ...&gt;</text>
  <text x="25" y="164" font-size="7" fill="#475569">• el SVG se anuncia como una única imagen</text>
  <text x="25" y="176" font-size="7" fill="#475569">• el lector usa title/aria-label como texto alternativo</text>

  <text x="150" y="210" text-anchor="middle" font-size="7" fill="#94a3b8">
    excepción: decorativos puros → aria-hidden="true" en su lugar
  </text>
</svg>
```

---

## `<title>` y `<desc>`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;title&gt; vs &lt;desc&gt;: cuándo usar cada uno
  </text>

  <!-- title -->
  <rect x="15" y="38" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">&lt;title&gt; — el equivalente al alt</text>
  <text x="25" y="68" font-size="7" fill="#475569">• texto corto y descriptivo, como el alt de &lt;img&gt;</text>
  <text x="25" y="80" font-size="7" fill="#475569">• debe ser el primer hijo del elemento que describe</text>
  <text x="25" y="92" font-size="7" fill="#475569">• el lector lo anuncia al llegar al SVG</text>
  <text x="25" y="110" font-size="7" font-weight="bold" fill="#475569">Ejemplos:</text>
  <text x="25" y="122" font-size="7" font-family="monospace" fill="#166534">"Logotipo de Acme Corp"</text>
  <text x="25" y="132" font-size="7" font-family="monospace" fill="#166534">"Gráfico de ventas trimestrales 2024"</text>

  <!-- desc -->
  <rect x="15" y="148" width="270" height="104" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="164" font-size="9" font-weight="bold" fill="#166534">&lt;desc&gt; — descripción extendida</text>
  <text x="25" y="178" font-size="7" fill="#475569">• cuando el título no basta: datos concretos, tendencias</text>
  <text x="25" y="190" font-size="7" fill="#475569">• para gráficos y diagramas con información estructural</text>
  <text x="25" y="204" font-size="7" font-weight="bold" fill="#475569">Ejemplo útil vs. inútil:</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#ef4444">✗ "Gráfico de barras"</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#166534">✓ "Cuatro barras: Ene 45, Feb 62,</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#166534">   Mar 78, Abr 91. Tendencia ascendente"</text>
</svg>
```

---

## `aria-labelledby` vs. `aria-describedby`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    conectar el SVG con sus textos
  </text>

  <!-- aria-labelledby -->
  <rect x="15" y="38" width="270" height="90" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">aria-labelledby</text>
  <text x="25" y="68" font-size="7" fill="#475569">conecta el SVG con uno o varios ids como <tspan font-weight="bold">nombre</tspan></text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#166534">aria-labelledby="titulo"</text>
  <text x="25" y="96" font-size="7" fill="#64748b">→ el lector dice "titulo"</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#166534">aria-labelledby="titulo desc"</text>
  <text x="25" y="124" font-size="7" fill="#64748b">→ el lector dice "titulo desc" como nombre completo</text>

  <!-- aria-describedby -->
  <rect x="15" y="136" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="152" font-size="9" font-weight="bold" fill="#166534">aria-describedby</text>
  <text x="25" y="166" font-size="7" fill="#475569">conecta con una <tspan font-weight="bold">descripción</tspan> adicional, no con el nombre</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#166534">aria-labelledby="titulo"</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#166534">aria-describedby="desc"</text>
  <text x="25" y="210" font-size="7" fill="#64748b">→ el lector anuncia el nombre (título),</text>
  <text x="25" y="220" font-size="7" fill="#64748b">   y luego la descripción opcional como detalle</text>
</svg>
```

---

## SVGs decorativos: ocultarlos

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG decorativo: invisible para lectores
  </text>

  <!-- Código -->
  <rect x="15" y="38" width="270" height="90" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg</text>
  <text x="35" y="70" font-size="8" font-family="monospace" fill="#fbbf24">  aria-hidden="true"</text>
  <text x="35" y="82" font-size="8" font-family="monospace" fill="#fbbf24">  focusable="false"</text>
  <text x="35" y="94" font-size="8" font-family="monospace" fill="#60a5fa">  viewBox="0 0 24 24"&gt;</text>
  <text x="35" y="108" font-size="8" font-family="monospace" fill="#94a3b8">  &lt;!-- ícono decorativo --&gt;</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>

  <!-- Explicación -->
  <rect x="15" y="138" width="270" height="68" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="154" font-size="8" font-weight="bold" fill="#92400e">aria-hidden="true"</text>
  <text x="25" y="166" font-size="7" fill="#475569">el lector de pantalla lo ignora por completo</text>

  <text x="25" y="182" font-size="8" font-weight="bold" fill="#92400e">focusable="false"</text>
  <text x="25" y="194" font-size="7" fill="#475569">evita foco por teclado en IE/Edge legacy (prudente por consistencia)</text>
</svg>
```

---

## Íconos en botones: tres patrones

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    íconos accesibles en botones
  </text>

  <!-- Opción 1 -->
  <rect x="15" y="38" width="270" height="72" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">1. Texto visible + ícono oculto</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"&gt;...&lt;/svg&gt;</text>
  <text x="25" y="94" font-size="7" font-family="monospace" fill="#475569">  Enviar</text>
  <text x="25" y="106" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>

  <!-- Opción 2 -->
  <rect x="15" y="118" width="270" height="72" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="134" font-size="9" font-weight="bold" fill="#166534">2. Ícono solo + aria-label en el botón</text>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Enviar formulario"&gt;</text>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"&gt;...&lt;/svg&gt;</text>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>
  <text x="25" y="186" font-size="7" fill="#166534">preferido cuando no hay texto visible</text>

  <!-- Opción 3 -->
  <rect x="15" y="198" width="270" height="72" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="214" font-size="9" font-weight="bold" fill="#5b21b6">3. title dentro del SVG (menos común)</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;</text>
  <text x="25" y="242" font-size="7" font-family="monospace" fill="#475569">  &lt;svg role="img" aria-labelledby="t"&gt;</text>
  <text x="25" y="254" font-size="7" font-family="monospace" fill="#475569">    &lt;title id="t"&gt;Enviar&lt;/title&gt;...&lt;/svg&gt;</text>
  <text x="25" y="266" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>
</svg>
```

---

## Gráficos de datos: tabla oculta visualmente

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    patrón para gráficos complejos
  </text>

  <!-- Bloque código -->
  <rect x="15" y="38" width="270" height="165" fill="#0f172a" rx="3"/>

  <text x="25" y="56" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- SVG visual, oculto a lectores --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg aria-hidden="true"&gt;...&lt;/svg&gt;</text>

  <text x="25" y="90" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- Tabla oculta visualmente --&gt;</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;table class="sr-only"&gt;</text>
  <text x="25" y="116" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;caption&gt;Ventas 2024&lt;/caption&gt;</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;tr&gt;&lt;th&gt;Trimestre&lt;/th&gt;&lt;th&gt;Ventas&lt;/th&gt;&lt;/tr&gt;</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;tr&gt;&lt;td&gt;T1&lt;/td&gt;&lt;td&gt;45&lt;/td&gt;&lt;/tr&gt; ...</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/table&gt;</text>

  <text x="25" y="172" font-size="8" font-family="monospace" fill="#94a3b8">/* CSS: oculto visualmente, accesible */</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#fbbf24">.sr-only { position: absolute; width: 1px;</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#fbbf24">  height: 1px; overflow: hidden; clip: rect(0,0,0,0); }</text>

  <text x="150" y="222" text-anchor="middle" font-size="7" fill="#94a3b8">
    los datos están disponibles para lectores, pero no ocupan espacio visual
  </text>
</svg>
```
