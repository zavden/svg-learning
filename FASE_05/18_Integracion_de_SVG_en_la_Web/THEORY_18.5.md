# 18.5 SVG con `<embed>`

`<embed>` es un elemento legado para incrustar recursos externos. Históricamente usado para plugins (Flash, Java applets), hoy sigue existiendo pero su uso para SVG está **desaconsejado**: no aporta nada que `<object>` o `<img>` no hagan mejor.

---

## Sintaxis

```svg
<svg width="300" height="170"
     viewBox="0 0 300 170"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;embed&gt; — elemento de autocierre
  </text>

  <rect x="15" y="38" width="270" height="120" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;embed</text>
  <text x="35" y="72" font-size="8" font-family="monospace" fill="#fbbf24">  type="image/svg+xml"</text>
  <text x="35" y="86" font-size="8" font-family="monospace" fill="#60a5fa">  src="grafico.svg"</text>
  <text x="35" y="100" font-size="8" font-family="monospace" fill="#34d399">  width="400"</text>
  <text x="35" y="114" font-size="8" font-family="monospace" fill="#34d399">  height="300" /&gt;</text>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#94a3b8">/* sin contenido interno, sin fallback */</text>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#94a3b8">/* el SVG se incrusta directamente */</text>
</svg>
```

---

## Diferencias con `<object>`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;embed&gt; vs &lt;object&gt;
  </text>

  <!-- Header tabla -->
  <rect x="15" y="38" width="270" height="20" fill="#e2e8f0"/>
  <text x="22" y="52" font-size="7" font-weight="bold" fill="#1e293b">Característica</text>
  <text x="155" y="52" font-size="7" font-weight="bold" fill="#1e293b">&lt;embed&gt;</text>
  <text x="225" y="52" font-size="7" font-weight="bold" fill="#1e293b">&lt;object&gt;</text>

  <rect x="15" y="58" width="270" height="20" fill="white"/>
  <text x="22" y="72" font-size="7" fill="#475569">Elemento autocerrado</text>
  <text x="155" y="72" font-size="7" fill="#10b981">sí</text>
  <text x="225" y="72" font-size="7" fill="#ef4444">no</text>

  <rect x="15" y="78" width="270" height="20" fill="#f8fafc"/>
  <text x="22" y="92" font-size="7" fill="#475569">Acepta fallback</text>
  <text x="155" y="92" font-size="7" fill="#ef4444">no</text>
  <text x="225" y="92" font-size="7" fill="#10b981">sí</text>

  <rect x="15" y="98" width="270" height="20" fill="white"/>
  <text x="22" y="112" font-size="7" fill="#475569">getSVGDocument()</text>
  <text x="155" y="112" font-size="7" fill="#f59e0b">limitado</text>
  <text x="225" y="112" font-size="7" fill="#10b981">sí</text>

  <rect x="15" y="118" width="270" height="20" fill="#f8fafc"/>
  <text x="22" y="132" font-size="7" fill="#475569">Estándar moderno</text>
  <text x="155" y="132" font-size="7" fill="#f59e0b">heredado</text>
  <text x="225" y="132" font-size="7" fill="#10b981">HTML5</text>

  <rect x="15" y="138" width="270" height="20" fill="white"/>
  <text x="22" y="152" font-size="7" fill="#475569">Comportamiento consistente</text>
  <text x="155" y="152" font-size="7" fill="#ef4444">no</text>
  <text x="225" y="152" font-size="7" fill="#f59e0b">parcial</text>

  <rect x="15" y="158" width="270" height="20" fill="#f8fafc"/>
  <text x="22" y="172" font-size="7" fill="#475569">Uso recomendado</text>
  <text x="155" y="172" font-size="7" fill="#ef4444">evitar</text>
  <text x="225" y="172" font-size="7" fill="#f59e0b">casos específicos</text>

  <text x="150" y="210" text-anchor="middle" font-size="7" fill="#94a3b8">
    &lt;embed&gt; nació para plugins; nunca fue pensado para SVG
  </text>
  <text x="150" y="224" text-anchor="middle" font-size="7" fill="#94a3b8">
    &lt;object&gt; es más versátil si hay que elegir entre los dos
  </text>
</svg>
```

---

## Recomendación

```svg
<svg width="300" height="250"
     viewBox="0 0 300 250"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿cuándo usar &lt;embed&gt; para SVG?
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="56" font-size="9" font-weight="bold" fill="#991b1b">✗ Evítalo en proyectos nuevos</text>
  <text x="25" y="74" font-size="7" fill="#1e293b">• sin ventajas respecto a &lt;object&gt; o &lt;img&gt;</text>
  <text x="25" y="88" font-size="7" fill="#1e293b">• sin fallback integrado</text>
  <text x="25" y="102" font-size="7" fill="#1e293b">• comportamiento histórico ligado a plugins obsoletos</text>
  <text x="25" y="116" font-size="7" fill="#1e293b">• poco documentado en recursos modernos</text>
  <text x="25" y="130" font-size="7" fill="#1e293b">• herramientas de accesibilidad lo manejan peor</text>

  <rect x="15" y="148" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#166534">✓ Alternativas según tu caso</text>
  <text x="25" y="184" font-size="7" fill="#475569">manipulación JS / CSS externo →</text>
  <text x="170" y="184" font-size="7" font-family="monospace" fill="#166534">inline</text>
  <text x="25" y="198" font-size="7" fill="#475569">imagen simple con cache →</text>
  <text x="170" y="198" font-size="7" font-family="monospace" fill="#166534">&lt;img&gt;</text>
  <text x="25" y="212" font-size="7" fill="#475569">necesitas fallback PNG →</text>
  <text x="170" y="212" font-size="7" font-family="monospace" fill="#166534">&lt;object&gt;</text>
  <text x="25" y="226" font-size="7" fill="#475569">aislamiento completo →</text>
  <text x="170" y="226" font-size="7" font-family="monospace" fill="#166534">&lt;iframe&gt;</text>
</svg>
```
