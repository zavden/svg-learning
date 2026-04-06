# 22.2 SVG Sprite con `<symbol>` y `<use>`

Un **sprite SVG** es un único archivo (o bloque inline) que contiene todos los iconos empaquetados como `<symbol>`. Cada lugar del HTML que necesita un icono usa `<use href="#id"/>` para traerlo. Es el patrón más eficiente y mantenible para un sistema de iconos.

---

## La arquitectura del sprite

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un SVG padre lleno de &lt;symbol&gt;
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg xmlns="..." style="display:none"&gt;</text>

  <text x="25" y="76" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-home"</text>
  <text x="25" y="90" font-size="8" font-family="monospace" fill="#fbbf24">          viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#34d399">    &lt;path d="M10 20v-6h4v6..."/&gt;</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/symbol&gt;</text>

  <text x="25" y="140" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-user"</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#fbbf24">          viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">    &lt;path d="M12 12c2.21..."/&gt;</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/symbol&gt;</text>

  <text x="25" y="204" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-close"</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">          viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#34d399">    &lt;path d="M19 6.41L17.59..."/&gt;</text>
  <text x="25" y="246" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/symbol&gt;</text>

  <text x="25" y="260" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>
</svg>
```

---

## Por qué `<symbol>` y no `<g>`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    symbol tiene su propio viewBox
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">&lt;g&gt; — problema</text>
  <text x="25" y="70" font-size="7" fill="#475569">vive en el sistema de coordenadas del padre</text>
  <text x="25" y="84" font-size="7" fill="#475569">→ si el icono se dibujó pensando en 0-24, tienes</text>
  <text x="25" y="98" font-size="7" fill="#475569">  que reescalar manualmente cada vez que lo uses</text>
  <text x="25" y="112" font-size="7" fill="#92400e">no escala automáticamente al tamaño del &lt;use&gt;</text>

  <rect x="15" y="142" width="270" height="106" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="158" font-size="9" font-weight="bold" fill="#166534">&lt;symbol&gt; — solución</text>
  <text x="25" y="174" font-size="7" fill="#475569">• tiene su propio viewBox interno</text>
  <text x="25" y="188" font-size="7" fill="#475569">• al usarlo con &lt;use width="48" height="48"&gt;</text>
  <text x="25" y="202" font-size="7" fill="#475569">  el contenido se escala automáticamente</text>
  <text x="25" y="216" font-size="7" fill="#475569">• nunca se renderiza directamente (biblioteca)</text>
  <text x="25" y="230" font-size="7" fill="#166534">→ el patrón correcto para bibliotecas de iconos</text>
</svg>
```

---

## Usar el icono con `<use>`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    referenciar desde cualquier parte
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- uso más simple --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;svg width="24" height="24"&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  &lt;use href="#icon-home"/&gt;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/svg&gt;</text>
  <text x="25" y="120" font-size="7" fill="#94a3b8">→ el símbolo se clona en un shadow tree</text>

  <rect x="15" y="146" width="270" height="102" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- con clase para control CSS --&gt;</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#60a5fa">&lt;svg class="icono" aria-hidden="true"</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#60a5fa">     focusable="false"&gt;</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">  &lt;use href="#icon-home"/&gt;</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/svg&gt;</text>
  <text x="25" y="240" font-size="7" fill="#fbbf24">aria-hidden + focusable="false" = invisible a a11y</text>
</svg>
```

---

## CSS para el contenedor

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el tamaño y el color vienen del CSS
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">.icono {</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">  width: 1.5rem;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  height: 1.5rem;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  fill: currentColor;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">  vertical-align: middle;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="150" font-size="7" fill="#94a3b8">• em/rem permite escalar con el texto del contexto</text>
  <text x="25" y="166" font-size="7" fill="#94a3b8">• currentColor hereda del color del padre</text>
  <text x="25" y="182" font-size="7" fill="#94a3b8">• vertical-align evita el descolocado en líneas</text>
  <text x="25" y="198" font-size="7" fill="#fbbf24">→ con 4 líneas, un sistema entero de iconos</text>
</svg>
```

---

## Cómo ocultar el sprite contenedor

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    tres formas, una ganadora
  </text>

  <rect x="15" y="38" width="270" height="74" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">style="display:none" — recomendado</text>
  <text x="25" y="70" font-size="7" fill="#475569">• no ocupa espacio en el layout</text>
  <text x="25" y="84" font-size="7" fill="#475569">• los &lt;symbol&gt; siguen siendo referenciables</text>
  <text x="25" y="98" font-size="7" fill="#475569">• funciona en todos los navegadores modernos</text>

  <rect x="15" y="120" width="270" height="64" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#92400e">hidden — aceptable</text>
  <text x="25" y="152" font-size="7" fill="#475569">atributo HTML nativo, equivale a display:none</text>
  <text x="25" y="168" font-size="7" fill="#475569">algún lector de pantalla antiguo puede comportarse raro</text>

  <rect x="15" y="192" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="208" font-size="9" font-weight="bold" fill="#991b1b">width="0" height="0" — desfasado</text>
  <text x="25" y="224" font-size="7" fill="#475569">antes de que display:none funcionara bien con</text>
  <text x="25" y="238" font-size="7" fill="#475569">sprites SVG se usaba esta técnica</text>
  <text x="25" y="252" font-size="7" fill="#991b1b">ya no es necesaria en navegadores modernos</text>
</svg>
```

---

## Accesibilidad del uso

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dos casos, dos patrones
  </text>

  <rect x="15" y="38" width="270" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Decorativo (con texto visible)</text>
  <text x="25" y="72" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">       focusable="false"&gt;</text>
  <text x="25" y="110" font-size="7" font-family="monospace" fill="#475569">    &lt;use href="#icon-home"/&gt;</text>
  <text x="25" y="122" font-size="7" font-family="monospace" fill="#475569">  &lt;/svg&gt; Inicio</text>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>

  <rect x="15" y="154" width="270" height="108" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="170" font-size="9" font-weight="bold" fill="#1e40af">Con significado (sin texto)</text>
  <text x="25" y="188" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Volver al inicio"&gt;</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#475569">       focusable="false"&gt;</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#475569">    &lt;use href="#icon-home"/&gt;</text>
  <text x="25" y="238" font-size="7" font-family="monospace" fill="#475569">  &lt;/svg&gt;</text>
  <text x="25" y="250" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>
</svg>
```
