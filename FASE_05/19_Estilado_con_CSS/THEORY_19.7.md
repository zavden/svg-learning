# 19.7 Bloque `<style>` dentro de SVG

El bloque `<style>` dentro de SVG es CSS de pleno derecho. Acepta toda la sintaxis (selectores, variables, `@keyframes`, `@media`, anidación moderna). El único matiz es la diferencia entre **HTML5 inline** y **archivo SVG standalone**, que afecta a si necesitas envolver el contenido en `<![CDATA[]]>`.

---

## Dónde colocarlo y cuántos puedes tener

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;style&gt; en cualquier parte
  </text>

  <rect x="15" y="38" width="270" height="190" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg viewBox="0 0 100 100"&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#94a3b8">  &lt;!-- estilos generales --&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;style&gt;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">    circle { fill: steelblue; }</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/style&gt;</text>

  <text x="25" y="130" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;defs&gt;</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#94a3b8">    &lt;!-- también vale aquí dentro --&gt;</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">    &lt;style&gt;</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#34d399">      .acento { fill: red; }</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#60a5fa">    &lt;/style&gt;</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;/defs&gt;</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;circle r="40" class="acento"/&gt;</text>

  <text x="150" y="262" text-anchor="middle" font-size="7" fill="#94a3b8">los estilos aplican a todo el SVG, sin importar dónde esté el bloque</text>
</svg>
```

---

## Sintaxis CSS completa

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    todo lo que CSS sabe hacer
  </text>

  <rect x="15" y="38" width="270" height="234" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">&lt;style&gt;</text>

  <text x="25" y="72" font-size="7" font-family="monospace" fill="#94a3b8">  /* variables */</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  :root { --c: steelblue; }</text>

  <text x="25" y="104" font-size="7" font-family="monospace" fill="#94a3b8">  /* media queries */</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#fbbf24">  @media (prefers-color-scheme: dark) {</text>
  <text x="25" y="132" font-size="8" font-family="monospace" fill="#34d399">    :root { --c: white; }</text>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#fbbf24">  }</text>

  <text x="25" y="164" font-size="7" font-family="monospace" fill="#94a3b8">  /* animaciones */</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#fbbf24">  @keyframes pulso {</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">    50% { opacity: 0.5; }</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#fbbf24">  }</text>

  <text x="25" y="224" font-size="8" font-family="monospace" fill="#ec4899">  circle {</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#34d399">    fill: var(--c);</text>
  <text x="25" y="252" font-size="8" font-family="monospace" fill="#34d399">    animation: pulso 2s infinite;</text>
  <text x="25" y="266" font-size="8" font-family="monospace" fill="#ec4899">  }</text>
</svg>
```

---

## La trampa del `<![CDATA[]]>`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cuándo necesitas CDATA
  </text>

  <!-- HTML5 -->
  <rect x="15" y="38" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">SVG inline en HTML5 → NO necesario</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;style&gt;</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#475569">  circle:hover &gt; .activo { fill: red; }</text>
  <text x="25" y="94" font-size="7" font-family="monospace" fill="#475569">  /* &gt; y &lt; sin escapar — funciona */</text>
  <text x="25" y="106" font-size="7" font-family="monospace" fill="#475569">&lt;/style&gt;</text>
  <text x="25" y="122" font-size="7" fill="#166534">el parser HTML5 trata &lt;style&gt; como texto literal</text>

  <!-- Standalone -->
  <rect x="15" y="138" width="270" height="120" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#92400e">Archivo .svg standalone → SÍ necesario</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">&lt;style&gt;</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">  &lt;![CDATA[</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">    circle:hover &gt; .activo { fill: red; }</text>
  <text x="25" y="206" font-size="7" font-family="monospace" fill="#475569">  ]]&gt;</text>
  <text x="25" y="218" font-size="7" font-family="monospace" fill="#475569">&lt;/style&gt;</text>
  <text x="25" y="234" font-size="7" fill="#92400e">XML interpreta &gt; y &amp; — sin CDATA puede romper</text>
  <text x="25" y="248" font-size="7" fill="#92400e">o no aplicar el CSS silenciosamente</text>
</svg>
```

---

## Por qué CDATA en XML

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    XML reserva &lt; y &amp; en texto
  </text>

  <rect x="15" y="38" width="270" height="194" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* XML estricto */</text>
  <text x="25" y="68" font-size="7" fill="#e2e8f0">• &lt; debe escaparse como &amp;lt; en texto</text>
  <text x="25" y="82" font-size="7" fill="#e2e8f0">• &amp; debe escaparse como &amp;amp;</text>
  <text x="25" y="96" font-size="7" fill="#e2e8f0">• comillas y &gt; suelen ser opcionales</text>

  <text x="25" y="116" font-size="7" font-family="monospace" fill="#94a3b8">/* CSS contiene esos caracteres */</text>
  <text x="25" y="130" font-size="7" fill="#34d399">a &gt; b   /* hijo directo */</text>
  <text x="25" y="144" font-size="7" fill="#34d399">a &lt; b   /* (no es CSS pero podría aparecer) */</text>
  <text x="25" y="158" font-size="7" fill="#34d399">[attr*="&amp;"]   /* selector con &amp; */</text>

  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">/* CDATA = "este texto NO se interpreta" */</text>
  <text x="25" y="192" font-size="7" fill="#fbbf24">&lt;![CDATA[ ... cualquier cosa ... ]]&gt;</text>
  <text x="25" y="208" font-size="7" fill="#e2e8f0">así escribes CSS sin pensar en escapes</text>
  <text x="25" y="222" font-size="7" fill="#94a3b8">solo afecta a archivos .svg que se sirven como image/svg+xml</text>
</svg>
```

---

## Regla práctica

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cómo decidir
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Contexto</text>
  <text x="180" y="46" font-size="7" font-weight="bold" fill="#1e293b">CDATA</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">SVG inline en HTML5</text>
  <text x="180" y="66" font-size="7" fill="#10b981">opcional (no daña)</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Archivo .svg cargado con &lt;img&gt;</text>
  <text x="180" y="88" font-size="7" fill="#f59e0b">recomendado</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Archivo .svg con &lt;object&gt;</text>
  <text x="180" y="110" font-size="7" fill="#f59e0b">recomendado</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">SVG en data URI</text>
  <text x="180" y="132" font-size="7" fill="#f59e0b">recomendado</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">SVG en email HTML</text>
  <text x="180" y="154" font-size="7" fill="#f59e0b">recomendado</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">SVG generado por SVGO sin CSS</text>
  <text x="180" y="176" font-size="7" fill="#94a3b8">irrelevante</text>

  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#166534">regla: si dudas, ponlo. Nunca hace daño y evita sorpresas</text>
  <text x="150" y="222" text-anchor="middle" font-size="7" fill="#94a3b8">cuando un SVG "pierde" su CSS al salir del HTML, mira CDATA</text>
</svg>
```
