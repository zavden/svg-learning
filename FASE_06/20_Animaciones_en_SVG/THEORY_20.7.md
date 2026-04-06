# 20.7 Animaciones CSS en SVG

CSS Animations y Transitions funcionan en SVG **igual que en HTML** para la mayoría de propiedades. Esto significa la misma sintaxis (`@keyframes`, `transition`, `animation`), las mismas funciones de timing (`ease`, `cubic-bezier`), el mismo soporte en DevTools, y el mismo respeto automático por `prefers-reduced-motion`.

---

## Qué se puede animar con CSS

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    propiedades animables con CSS
  </text>

  <!-- Bien soportado -->
  <rect x="15" y="38" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ buen soporte universal</text>
  <text x="25" y="70" font-size="7" fill="#475569">fill, stroke, stroke-width, stroke-opacity, fill-opacity</text>
  <text x="25" y="84" font-size="7" fill="#475569">opacity, transform (con transform-origin)</text>
  <text x="25" y="98" font-size="7" fill="#475569">stroke-dasharray, stroke-dashoffset (line drawing)</text>
  <text x="25" y="112" font-size="7" fill="#475569">filter, clip-path (formas básicas)</text>

  <!-- SVG 2 -->
  <rect x="15" y="132" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="148" font-size="9" font-weight="bold" fill="#92400e">~ SVG 2 (soporte creciente)</text>
  <text x="25" y="164" font-size="7" fill="#475569">r (radio de circle, ellipse)</text>
  <text x="25" y="178" font-size="7" fill="#475569">cx, cy, x, y</text>
  <text x="25" y="192" font-size="7" fill="#475569">width, height de rect</text>
  <text x="25" y="204" font-size="7" fill="#92400e">Chrome 65+, Firefox parcial</text>

  <!-- No -->
  <rect x="15" y="220" width="270" height="50" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="236" font-size="9" font-weight="bold" fill="#991b1b">✗ no animable con CSS</text>
  <text x="25" y="250" font-size="7" fill="#475569">d (atributo del path) — solo SMIL o JS</text>
  <text x="25" y="262" font-size="7" fill="#475569">points (polygon, polyline)</text>
</svg>
```

---

## CSS Transitions: el caso `:hover`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    transition para hover y cambios de estado
  </text>

  <rect x="15" y="38" width="270" height="194" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">circle {</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">  fill: steelblue;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  transition:</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">    fill 0.3s ease,</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#34d399">    transform 0.3s ease;</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#34d399">  transform-origin: center;</text>
  <text x="25" y="136" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="156" font-size="8" font-family="monospace" fill="#60a5fa">circle:hover {</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#34d399">  fill: #e74c3c;</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#34d399">  transform: scale(1.2);</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#34d399">  cursor: pointer;</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="228" font-size="7" fill="#94a3b8">/* la forma más rápida de hacer SVG interactivo */</text>
</svg>
```

---

## CSS `@keyframes` en SVG

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    @keyframes para animaciones complejas
  </text>

  <rect x="15" y="38" width="270" height="180" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">@keyframes pulso {</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">  0%, 100% {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">    opacity: 1;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">    transform: scale(1);</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">  }</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#34d399">  50% {</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#34d399">    opacity: 0.5;</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#34d399">    transform: scale(1.15);</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">  }</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#fbbf24">.indicador {</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#ec4899">  animation: pulso 1.5s ease-in-out infinite;</text>
  <text x="25" y="228" font-size="8" font-family="monospace" fill="#fbbf24">}</text>

  <text x="150" y="252" text-anchor="middle" font-size="7" fill="#94a3b8">recuerda transform-origin: center si rotas o escalas</text>
</svg>
```

---

## `prefers-reduced-motion` — accesibilidad nativa

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    respetar la preferencia del usuario
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Por qué importa</text>
  <text x="25" y="70" font-size="7" fill="#475569">• personas con vestibulares sensibles o epilepsia</text>
  <text x="25" y="84" font-size="7" fill="#475569">• el SO ofrece "reducir movimiento" en accesibilidad</text>
  <text x="25" y="98" font-size="7" fill="#475569">• animaciones excesivas pueden causar mareos o crisis</text>
  <text x="25" y="112" font-size="7" fill="#92400e">→ obligación ética y normativa (WCAG)</text>

  <rect x="15" y="126" width="270" height="142" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">/* animación por defecto */</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#60a5fa">.spinner {</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#34d399">  animation: rotar 1s linear infinite;</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="204" font-size="7" font-family="monospace" fill="#94a3b8">/* respeta la preferencia */</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">@media (prefers-reduced-motion: reduce) {</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#ec4899">  .spinner {</text>
  <text x="25" y="246" font-size="8" font-family="monospace" fill="#34d399">    animation: none;</text>
  <text x="25" y="260" font-size="8" font-family="monospace" fill="#ec4899">  }</text>
</svg>
```

---

## Patrones útiles

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    snippets reutilizables
  </text>

  <rect x="15" y="38" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* spinner clásico */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">@keyframes rotar {</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  to { transform: rotate(360deg); }</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#fbbf24">.spinner {</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#34d399">  animation: rotar 1s linear infinite;</text>
  <text x="25" y="136" font-size="8" font-family="monospace" fill="#34d399">  transform-origin: center;</text>

  <rect x="15" y="152" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="168" font-size="7" font-family="monospace" fill="#94a3b8">/* fade in al hover del padre */</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#60a5fa">.tooltip {</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#34d399">  opacity: 0;</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#34d399">  transition: opacity 0.2s;</text>
  <text x="25" y="226" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="240" font-size="8" font-family="monospace" fill="#fbbf24">.boton:hover .tooltip {</text>
  <text x="25" y="254" font-size="8" font-family="monospace" fill="#34d399">  opacity: 1;</text>
  <text x="25" y="266" font-size="8" font-family="monospace" fill="#fbbf24">}</text>
</svg>
```
