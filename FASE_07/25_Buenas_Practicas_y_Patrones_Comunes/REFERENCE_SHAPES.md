# Reference Shapes — Buenas Prácticas y Patrones Comunes

Esta referencia es el **resumen ejecutivo** de todo lo aprendido. Snippets listos para copiar, un checklist mínimo, y las recetas más comunes que reaparecen en producción.

---

## Esqueleto canónico de un SVG bien estructurado

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    plantilla base para empezar
  </text>

  <rect x="15" y="38" width="270" height="248" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">&lt;svg xmlns="http://www.w3.org/2000/svg"</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">     viewBox="0 0 100 100"</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#60a5fa">     role="img"</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#60a5fa">     aria-labelledby="t"&gt;</text>

  <text x="25" y="118" font-size="8" font-family="monospace" fill="#34d399">  &lt;title id="t"&gt;Título&lt;/title&gt;</text>

  <text x="25" y="140" font-size="7" font-family="monospace" fill="#94a3b8">  &lt;!-- estilos locales --&gt;</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#34d399">  &lt;style&gt;.primario{fill:var(--c)}&lt;/style&gt;</text>

  <text x="25" y="176" font-size="7" font-family="monospace" fill="#94a3b8">  &lt;!-- reutilizable --&gt;</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;defs&gt;</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#fbbf24">    &lt;linearGradient id="g"&gt;...</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;/defs&gt;</text>

  <text x="25" y="240" font-size="7" font-family="monospace" fill="#94a3b8">  &lt;!-- contenido visual --&gt;</text>
  <text x="25" y="254" font-size="8" font-family="monospace" fill="#ec4899">  &lt;rect class="primario" .../&gt;</text>
  <text x="25" y="268" font-size="8" font-family="monospace" fill="#ec4899">  &lt;path .../&gt;</text>
  <text x="25" y="280" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/svg&gt;</text>
</svg>
```

---

## Iconos con `currentColor` + tamaño

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el patrón de icono que escala bien
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// icono</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;svg viewBox="0 0 24 24"</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  width="1em" height="1em"</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">  fill="currentColor"</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">  aria-hidden="true"&gt;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#34d399">  &lt;path d="..."/&gt;</text>

  <rect x="15" y="154" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="170" font-size="9" font-weight="bold" fill="#166534">Por qué funciona</text>
  <text x="25" y="186" font-size="7" fill="#475569">• width/height 1em → hereda tamaño del texto</text>
  <text x="25" y="200" font-size="7" fill="#475569">• fill currentColor → hereda color del texto</text>
  <text x="25" y="214" font-size="7" fill="#475569">• aria-hidden → no duplica info con label</text>
  <text x="25" y="228" font-size="7" fill="#166534">→ un botón con icono adapta tamaño y color solo</text>
</svg>
```

---

## A11y cheat sheet

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    3 casos, 3 recetas
  </text>

  <rect x="15" y="38" width="270" height="64" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Decorativo</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;svg aria-hidden="true"</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  focusable="false"&gt;</text>
  <text x="25" y="96" font-size="7" fill="#166534">→ ni title ni desc</text>

  <rect x="15" y="112" width="270" height="74" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="128" font-size="9" font-weight="bold" fill="#1e40af">Informativo</text>
  <text x="25" y="144" font-size="7" font-family="monospace" fill="#475569">&lt;svg role="img"</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">  aria-labelledby="t d"&gt;</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#475569">  &lt;title id="t"&gt;...&lt;/title&gt;</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#475569">  &lt;desc id="d"&gt;...&lt;/desc&gt;</text>

  <rect x="15" y="196" width="270" height="72" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="212" font-size="9" font-weight="bold" fill="#92400e">Icon-only button</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Cerrar"&gt;</text>
  <text x="25" y="242" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"&gt;</text>
  <text x="25" y="256" font-size="7" font-family="monospace" fill="#475569">    &lt;path d="..."/&gt;</text>
</svg>
```

---

## Pattern: Spinner minimal

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    receta lista de pegar
  </text>

  <rect x="15" y="38" width="270" height="208" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">&lt;svg viewBox="0 0 50 50"</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">  class="spinner"</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#60a5fa">  aria-label="Cargando"&gt;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">  &lt;circle cx="25" cy="25" r="20"</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#34d399">    fill="none"</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#34d399">    stroke="currentColor"</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#34d399">    stroke-width="4"</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#34d399">    stroke-dasharray="90 60"</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#34d399">    stroke-linecap="round"/&gt;</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/svg&gt;</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#fbbf24">.spinner { animation: rot 1s linear infinite }</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#fbbf24">@keyframes rot { to { transform: rotate(1turn) }}</text>
  <text x="25" y="236" font-size="7" fill="#94a3b8">→ respetar prefers-reduced-motion</text>
</svg>
```

---

## Sanitización segura

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    DOMPurify mínimo para SVG
  </text>

  <rect x="15" y="38" width="270" height="188" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">import DOMPurify from 'dompurify'</text>

  <text x="25" y="76" font-size="8" font-family="monospace" fill="#34d399">const limpio = DOMPurify.sanitize(raw, {</text>
  <text x="25" y="90" font-size="7" font-family="monospace" fill="#fbbf24">  USE_PROFILES: {</text>
  <text x="25" y="104" font-size="7" font-family="monospace" fill="#ec4899">    svg: true,</text>
  <text x="25" y="118" font-size="7" font-family="monospace" fill="#ec4899">    svgFilters: true</text>
  <text x="25" y="132" font-size="7" font-family="monospace" fill="#fbbf24">  },</text>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#fbbf24">  FORBID_TAGS: [</text>
  <text x="25" y="160" font-size="7" font-family="monospace" fill="#ec4899">    'script','foreignObject','iframe'],</text>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#fbbf24">  FORBID_ATTR: [</text>
  <text x="25" y="188" font-size="7" font-family="monospace" fill="#ec4899">    'onload','onclick','onerror']</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#34d399">})</text>
  <text x="25" y="218" font-size="7" font-family="monospace" fill="#94a3b8">// insertar con innerHTML es ahora seguro</text>
</svg>
```

---

## Checklist de 10 puntos antes de merge

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    si los 10 son ✓, haz merge
  </text>

  <text x="20" y="50" font-size="10" fill="#10b981">1 ✓</text>
  <text x="50" y="50" font-size="8" fill="#475569">viewBox correcto, sin width/height fijos</text>

  <text x="20" y="72" font-size="10" fill="#10b981">2 ✓</text>
  <text x="50" y="72" font-size="8" fill="#475569">A11y: aria-hidden o title+desc según rol</text>

  <text x="20" y="94" font-size="10" fill="#10b981">3 ✓</text>
  <text x="50" y="94" font-size="8" fill="#475569">SVGO pasado, sin basura de editor</text>

  <text x="20" y="116" font-size="10" fill="#10b981">4 ✓</text>
  <text x="50" y="116" font-size="8" fill="#475569">currentColor o CSS vars para tematización</text>

  <text x="20" y="138" font-size="10" fill="#10b981">5 ✓</text>
  <text x="50" y="138" font-size="8" fill="#475569">animaciones solo transform/opacity</text>

  <text x="20" y="160" font-size="10" fill="#10b981">6 ✓</text>
  <text x="50" y="160" font-size="8" fill="#475569">prefers-reduced-motion respetado</text>

  <text x="20" y="182" font-size="10" fill="#10b981">7 ✓</text>
  <text x="50" y="182" font-size="8" fill="#475569">sin &lt;script&gt;, sin event handlers inline</text>

  <text x="20" y="204" font-size="10" fill="#10b981">8 ✓</text>
  <text x="50" y="204" font-size="8" fill="#475569">probado en Chrome, Firefox, Safari</text>

  <text x="20" y="226" font-size="10" fill="#10b981">9 ✓</text>
  <text x="50" y="226" font-size="8" fill="#475569">medido en mobile throttled (FPS, LCP)</text>

  <text x="20" y="248" font-size="10" fill="#10b981">10 ✓</text>
  <text x="52" y="248" font-size="8" fill="#475569">método de integración correcto para el caso</text>

  <rect x="15" y="262" width="270" height="30" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="150" y="282" text-anchor="middle" font-size="8" fill="#166534">"10/10 → ship it" es la meta, no "compila"</text>
</svg>
```
