# 21.1 Acceso al DOM SVG

SVG inline en HTML no es un "objeto extraño": sus elementos son ciudadanos de primera clase del DOM. Todo lo que JavaScript puede hacer con `<div>` o `<span>`, puede hacerlo con `<circle>` o `<path>`. Pero hay un pequeño mapa de tipos — cada elemento SVG pertenece a una interfaz específica (`SVGCircleElement`, `SVGPathElement`…) — que conviene conocer.

---

## SVG inline = mismo DOM

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    jerarquía de interfaces
  </text>

  <!-- Pirámide -->
  <rect x="60" y="44" width="180" height="22" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="150" y="58" text-anchor="middle" font-size="8" font-family="monospace" fill="#166534">EventTarget</text>

  <rect x="70" y="74" width="160" height="22" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="150" y="88" text-anchor="middle" font-size="8" font-family="monospace" fill="#1e40af">Node</text>

  <rect x="80" y="104" width="140" height="22" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="150" y="118" text-anchor="middle" font-size="8" font-family="monospace" fill="#5b21b6">Element</text>

  <rect x="90" y="134" width="120" height="22" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="150" y="148" text-anchor="middle" font-size="8" font-family="monospace" fill="#92400e">SVGElement</text>

  <rect x="100" y="164" width="100" height="22" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="150" y="178" text-anchor="middle" font-size="8" font-family="monospace" fill="#9d174d">SVGCircleElement</text>

  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">
    un &lt;circle&gt; hereda de todas las capas de arriba
  </text>
  <text x="150" y="222" text-anchor="middle" font-size="7" fill="#94a3b8">
    → puede usar addEventListener, querySelector, classList…
  </text>
</svg>
```

---

## Métodos de selección

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los mismos selectores que en HTML
  </text>

  <rect x="15" y="38" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// por tipo</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">document.querySelector('circle')</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#60a5fa">document.querySelectorAll('path')</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#94a3b8">// por id</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">document.getElementById('mi-circulo')</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#94a3b8">// por clase</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#fbbf24">document.querySelectorAll('.punto')</text>

  <rect x="15" y="158" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#94a3b8">// scoped: dentro de un SVG específico</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#e2e8f0">const svg = document.querySelector('svg');</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#60a5fa">svg.querySelector('circle');</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#60a5fa">svg.querySelectorAll('[data-id]');</text>
  <text x="25" y="242" font-size="7" fill="#94a3b8">→ buena práctica: limitar búsquedas al SVG</text>
  <text x="25" y="256" font-size="7" fill="#94a3b8">   cuando hay varios en la página</text>
</svg>
```

---

## Las interfaces específicas por tipo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada elemento SVG tiene su clase
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Elemento</text>
  <text x="160" y="46" font-size="7" font-weight="bold" fill="#1e293b">Interfaz DOM</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;</text>
  <text x="160" y="66" font-size="7" font-family="monospace" fill="#3b82f6">SVGSVGElement</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" font-family="monospace" fill="#475569">&lt;g&gt;</text>
  <text x="160" y="88" font-size="7" font-family="monospace" fill="#3b82f6">SVGGElement</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" font-family="monospace" fill="#475569">&lt;path&gt;</text>
  <text x="160" y="110" font-size="7" font-family="monospace" fill="#3b82f6">SVGPathElement</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" font-family="monospace" fill="#475569">&lt;circle&gt;</text>
  <text x="160" y="132" font-size="7" font-family="monospace" fill="#3b82f6">SVGCircleElement</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" font-family="monospace" fill="#475569">&lt;rect&gt;</text>
  <text x="160" y="154" font-size="7" font-family="monospace" fill="#3b82f6">SVGRectElement</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" font-family="monospace" fill="#475569">&lt;text&gt;</text>
  <text x="160" y="176" font-size="7" font-family="monospace" fill="#3b82f6">SVGTextElement</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" font-family="monospace" fill="#475569">&lt;use&gt;</text>
  <text x="160" y="198" font-size="7" font-family="monospace" fill="#3b82f6">SVGUseElement</text>

  <text x="150" y="230" text-anchor="middle" font-size="7" fill="#94a3b8">cada interfaz aporta métodos específicos</text>
  <text x="150" y="244" text-anchor="middle" font-size="7" fill="#94a3b8">ej: SVGPathElement.getTotalLength()</text>
</svg>
```

---

## Verificación rápida en la consola

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    confirmar que es un elemento SVG
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// inspeccionar tipos en la consola</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">const c = document.querySelector('circle');</text>

  <text x="25" y="90" font-size="8" font-family="monospace" fill="#60a5fa">c instanceof SVGCircleElement</text>
  <text x="25" y="102" font-size="7" font-family="monospace" fill="#94a3b8">→ true</text>

  <text x="25" y="120" font-size="8" font-family="monospace" fill="#60a5fa">c instanceof SVGElement</text>
  <text x="25" y="132" font-size="7" font-family="monospace" fill="#94a3b8">→ true</text>

  <text x="25" y="150" font-size="8" font-family="monospace" fill="#60a5fa">c instanceof HTMLElement</text>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#f87171">→ false ← CUIDADO</text>

  <text x="25" y="184" font-size="7" fill="#fbbf24">los elementos SVG NO son HTMLElement</text>
  <text x="25" y="198" font-size="7" fill="#94a3b8">algunas librerías que asumen HTML fallan aquí</text>
</svg>
```
