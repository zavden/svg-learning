# 21.9 Scripts dentro de archivos SVG

Un archivo `.svg` puede llevar su propio JavaScript embebido dentro de un `<script>`. Es la forma de hacer un SVG "autónomo" e interactivo, que funcione incluso sin un HTML que lo contenga. Pero tiene sus reglas: CDATA, contextos donde se bloquea y serias implicaciones de seguridad.

---

## Sintaxis: `<script>` dentro del SVG

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el script vive dentro del SVG
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg xmlns="http://www.w3.org/2000/svg"</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">     viewBox="0 0 200 200"&gt;</text>

  <text x="25" y="88" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;script&gt;</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#fbbf24">    &lt;![CDATA[</text>
  <text x="25" y="116" font-size="8" font-family="monospace" fill="#34d399">      const c = document.querySelector('circle');</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#34d399">      c.addEventListener('click', () =&gt; {</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#34d399">        c.setAttribute('fill','red');</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#34d399">      });</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#fbbf24">    ]]&gt;</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/script&gt;</text>

  <text x="25" y="206" font-size="8" font-family="monospace" fill="#e2e8f0">  &lt;circle cx="100" cy="100" r="50" fill="blue"/&gt;</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>
  <text x="25" y="240" font-size="7" fill="#94a3b8">→ al abrir este .svg en el navegador funciona solo</text>
</svg>
```

---

## Por qué el `<![CDATA[ ]]>`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el archivo .svg es XML estricto
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">El problema</text>
  <text x="25" y="70" font-size="7" fill="#475569">• caracteres `&lt;` `&gt;` `&amp;` son reservados en XML</text>
  <text x="25" y="84" font-size="7" fill="#475569">• un código como `if (a &lt; b)` rompe el parser XML</text>
  <text x="25" y="98" font-size="7" fill="#475569">• habría que escapar a `&amp;lt;` manualmente — ilegible</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">el parser cree que `&lt;` inicia una etiqueta</text>

  <rect x="15" y="140" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="156" font-size="9" font-weight="bold" fill="#166534">La solución: CDATA</text>
  <text x="25" y="172" font-size="7" fill="#475569">• `&lt;![CDATA[ ... ]]&gt;` marca el contenido como</text>
  <text x="25" y="186" font-size="7" fill="#475569">  "texto literal" — el parser lo ignora</text>
  <text x="25" y="200" font-size="7" fill="#475569">• dentro puedes escribir JS normal sin escapar nada</text>
  <text x="25" y="216" font-size="7" fill="#166534">en SVG inline en HTML5 NO hace falta: el parser HTML es más tolerante</text>
</svg>
```

---

## Cuándo se ejecutan los scripts

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué contexto permite JS, cuál no
  </text>

  <rect x="15" y="38" width="270" height="112" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ se ejecuta</text>
  <text x="25" y="70" font-size="7" fill="#475569">• SVG inline en HTML (parte del DOM padre)</text>
  <text x="25" y="84" font-size="7" fill="#475569">• SVG abierto directo: http://.../foo.svg</text>
  <text x="25" y="98" font-size="7" fill="#475569">• SVG en &lt;object data="foo.svg"&gt; (contexto propio)</text>
  <text x="25" y="112" font-size="7" fill="#475569">• SVG en &lt;iframe src="foo.svg"&gt; (contexto iframe)</text>
  <text x="25" y="126" font-size="7" fill="#166534">→ en inline, el SVG comparte window con el HTML</text>
  <text x="25" y="140" font-size="7" fill="#166534">→ en object/iframe, tiene su propio window aislado</text>

  <rect x="15" y="158" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="174" font-size="9" font-weight="bold" fill="#991b1b">✗ bloqueado por seguridad</text>
  <text x="25" y="190" font-size="7" fill="#475569">• SVG en &lt;img src="foo.svg"&gt;</text>
  <text x="25" y="204" font-size="7" fill="#475569">• SVG como background-image en CSS</text>
  <text x="25" y="218" font-size="7" fill="#475569">• SVG como data: URI en img</text>
  <text x="25" y="232" font-size="7" fill="#991b1b">el navegador se niega a ejecutar JS en estos contextos</text>
  <text x="25" y="246" font-size="7" fill="#991b1b">por riesgo de XSS cuando el SVG viene de terceros</text>
</svg>
```

---

## Implicaciones de seguridad

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un SVG es una página web en miniatura
  </text>

  <rect x="15" y="38" width="270" height="108" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">El riesgo</text>
  <text x="25" y="70" font-size="7" fill="#475569">un SVG con script puede hacer todo lo que haría</text>
  <text x="25" y="84" font-size="7" fill="#475569">un &lt;script&gt; en HTML: leer cookies, enviar fetch,</text>
  <text x="25" y="98" font-size="7" fill="#475569">manipular el DOM, robar tokens, redirigir…</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">si insertas un SVG ajeno inline, estás ejecutando</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">código de un tercero con tus credenciales</text>

  <rect x="15" y="154" width="270" height="112" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="170" font-size="9" font-weight="bold" fill="#166534">Cómo protegerse</text>
  <text x="25" y="186" font-size="7" fill="#475569">• NUNCA incrustar inline un SVG de origen no confiable</text>
  <text x="25" y="200" font-size="7" fill="#475569">• usar un sanitizador (DOMPurify) que elimine</text>
  <text x="25" y="214" font-size="7" fill="#475569">  &lt;script&gt; y atributos onclick, onload, etc.</text>
  <text x="25" y="228" font-size="7" fill="#475569">• en su defecto: servirlo como &lt;img&gt; → el JS queda bloqueado</text>
  <text x="25" y="244" font-size="7" fill="#166534">regla de oro: sanear siempre el SVG antes de inline</text>
  <text x="25" y="258" font-size="7" fill="#166534">cuando viene de usuarios o APIs externas</text>
</svg>
```

---

## Atributos de evento (`onclick`, `onload`…)

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    onclick="..." en SVG también funciona
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- funciona pero no se recomienda --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;circle cx="50" cy="50" r="20"</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#fbbf24">  onclick="cambiarColor(this)"</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">  fill="blue"/&gt;</text>

  <rect x="15" y="130" width="270" height="116" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="146" font-size="9" font-weight="bold" fill="#92400e">Por qué evitarlos</text>
  <text x="25" y="162" font-size="7" fill="#475569">• mezclan marcado y lógica (peor mantenimiento)</text>
  <text x="25" y="176" font-size="7" fill="#475569">• el código vive como string inline, sin linting</text>
  <text x="25" y="190" font-size="7" fill="#475569">• son vectores de XSS en SVG de origen externo</text>
  <text x="25" y="204" font-size="7" fill="#475569">• los sanitizadores los eliminan por defecto</text>
  <text x="25" y="224" font-size="7" fill="#92400e">→ prefiere addEventListener desde un &lt;script&gt;</text>
  <text x="25" y="236" font-size="7" fill="#92400e">   o JS externo. Solo usa onXxx en demos mínimas.</text>
</svg>
```
