# Referencia rápida — Integración de SVG en la web

---

## Cheat sheet: los 7 métodos de un vistazo

```svg
<svg width="300" height="320"
     viewBox="0 0 300 320"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    siete métodos de integración
  </text>

  <!-- Header -->
  <rect x="10" y="30" width="280" height="18" fill="#e2e8f0"/>
  <text x="18" y="42" font-size="7" font-weight="bold" fill="#1e293b">Método</text>
  <text x="100" y="42" font-size="7" font-weight="bold" fill="#1e293b">CSS ext.</text>
  <text x="145" y="42" font-size="7" font-weight="bold" fill="#1e293b">Script</text>
  <text x="185" y="42" font-size="7" font-weight="bold" fill="#1e293b">Cache</text>
  <text x="225" y="42" font-size="7" font-weight="bold" fill="#1e293b">A11y</text>
  <text x="260" y="42" font-size="7" font-weight="bold" fill="#1e293b">Peso</text>

  <rect x="10" y="48" width="280" height="20" fill="white"/>
  <text x="18" y="61" font-size="7" font-family="monospace" fill="#3b82f6">inline</text>
  <text x="100" y="61" font-size="7" fill="#10b981">sí</text>
  <text x="145" y="61" font-size="7" fill="#10b981">sí</text>
  <text x="185" y="61" font-size="7" fill="#ef4444">no</text>
  <text x="225" y="61" font-size="7" fill="#10b981">total</text>
  <text x="260" y="61" font-size="7" fill="#f59e0b">HTML</text>

  <rect x="10" y="68" width="280" height="20" fill="#f8fafc"/>
  <text x="18" y="81" font-size="7" font-family="monospace" fill="#3b82f6">&lt;img&gt;</text>
  <text x="100" y="81" font-size="7" fill="#ef4444">no</text>
  <text x="145" y="81" font-size="7" fill="#ef4444">no</text>
  <text x="185" y="81" font-size="7" fill="#10b981">sí</text>
  <text x="225" y="81" font-size="7" fill="#10b981">alt</text>
  <text x="260" y="81" font-size="7" fill="#10b981">óptimo</text>

  <rect x="10" y="88" width="280" height="20" fill="white"/>
  <text x="18" y="101" font-size="7" font-family="monospace" fill="#3b82f6">CSS bg</text>
  <text x="100" y="101" font-size="7" fill="#ef4444">no</text>
  <text x="145" y="101" font-size="7" fill="#ef4444">no</text>
  <text x="185" y="101" font-size="7" fill="#10b981">sí</text>
  <text x="225" y="101" font-size="7" fill="#ef4444">nula</text>
  <text x="260" y="101" font-size="7" fill="#10b981">óptimo</text>

  <rect x="10" y="108" width="280" height="20" fill="#f8fafc"/>
  <text x="18" y="121" font-size="7" font-family="monospace" fill="#3b82f6">&lt;object&gt;</text>
  <text x="100" y="121" font-size="7" fill="#ef4444">no</text>
  <text x="145" y="121" font-size="7" fill="#10b981">sí*</text>
  <text x="185" y="121" font-size="7" fill="#10b981">sí</text>
  <text x="225" y="121" font-size="7" fill="#f59e0b">parcial</text>
  <text x="260" y="121" font-size="7" fill="#f59e0b">medio</text>

  <rect x="10" y="128" width="280" height="20" fill="white"/>
  <text x="18" y="141" font-size="7" font-family="monospace" fill="#3b82f6">&lt;embed&gt;</text>
  <text x="100" y="141" font-size="7" fill="#ef4444">no</text>
  <text x="145" y="141" font-size="7" fill="#f59e0b">lim.</text>
  <text x="185" y="141" font-size="7" fill="#10b981">sí</text>
  <text x="225" y="141" font-size="7" fill="#ef4444">pobre</text>
  <text x="260" y="141" font-size="7" fill="#f59e0b">medio</text>

  <rect x="10" y="148" width="280" height="20" fill="#f8fafc"/>
  <text x="18" y="161" font-size="7" font-family="monospace" fill="#3b82f6">&lt;iframe&gt;</text>
  <text x="100" y="161" font-size="7" fill="#ef4444">no</text>
  <text x="145" y="161" font-size="7" fill="#10b981">sí</text>
  <text x="185" y="161" font-size="7" fill="#10b981">sí</text>
  <text x="225" y="161" font-size="7" fill="#f59e0b">title</text>
  <text x="260" y="161" font-size="7" fill="#ef4444">alto</text>

  <rect x="10" y="168" width="280" height="20" fill="white"/>
  <text x="18" y="181" font-size="7" font-family="monospace" fill="#3b82f6">data URI</text>
  <text x="100" y="181" font-size="7" fill="#ef4444">no</text>
  <text x="145" y="181" font-size="7" fill="#ef4444">no</text>
  <text x="185" y="181" font-size="7" fill="#ef4444">no</text>
  <text x="225" y="181" font-size="7" fill="#f59e0b">ctx.</text>
  <text x="260" y="181" font-size="7" fill="#f59e0b">inline</text>

  <text x="18" y="200" font-size="6" fill="#94a3b8">* &lt;object&gt; ejecuta scripts pero en su propio contexto</text>

  <!-- Leyenda rápida -->
  <rect x="10" y="210" width="280" height="100" fill="#0f172a" rx="3"/>
  <text x="20" y="226" font-size="7" font-weight="bold" fill="#94a3b8">resumen en una línea</text>
  <text x="20" y="240" font-size="7" fill="#60a5fa">inline →</text>
  <text x="80" y="240" font-size="7" fill="#e2e8f0">máxima flexibilidad, sin cache</text>
  <text x="20" y="252" font-size="7" fill="#60a5fa">&lt;img&gt; →</text>
  <text x="80" y="252" font-size="7" fill="#e2e8f0">como foto, cache nativa, sin CSS/JS</text>
  <text x="20" y="264" font-size="7" fill="#60a5fa">CSS bg →</text>
  <text x="80" y="264" font-size="7" fill="#e2e8f0">solo decorativo, cero accesibilidad</text>
  <text x="20" y="276" font-size="7" fill="#60a5fa">&lt;object&gt; →</text>
  <text x="80" y="276" font-size="7" fill="#e2e8f0">scripts internos + fallback</text>
  <text x="20" y="288" font-size="7" fill="#60a5fa">&lt;iframe&gt; →</text>
  <text x="80" y="288" font-size="7" fill="#e2e8f0">aislamiento total, alto coste</text>
  <text x="20" y="300" font-size="7" fill="#60a5fa">data URI →</text>
  <text x="80" y="300" font-size="7" fill="#e2e8f0">SVGs pequeños, sin petición HTTP</text>
</svg>
```

---

## Árbol de decisión

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿qué método elijo?
  </text>

  <!-- Pregunta 1 -->
  <rect x="60" y="36" width="180" height="26" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="150" y="53" text-anchor="middle" font-size="8" fill="#92400e">¿necesitas CSS/JS del padre?</text>

  <line x1="100" y1="62" x2="70" y2="80" stroke="#94a3b8" stroke-width="1"/>
  <text x="80" y="76" font-size="7" fill="#64748b">sí</text>
  <line x1="200" y1="62" x2="230" y2="80" stroke="#94a3b8" stroke-width="1"/>
  <text x="215" y="76" font-size="7" fill="#64748b">no</text>

  <!-- Rama sí -->
  <rect x="10" y="80" width="120" height="22" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="70" y="94" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">inline</text>

  <!-- Pregunta 2 -->
  <rect x="170" y="80" width="120" height="26" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="230" y="97" text-anchor="middle" font-size="7" fill="#92400e">¿es decorativo?</text>

  <line x1="200" y1="106" x2="175" y2="122" stroke="#94a3b8" stroke-width="1"/>
  <text x="180" y="118" font-size="7" fill="#64748b">sí</text>
  <line x1="260" y1="106" x2="285" y2="122" stroke="#94a3b8" stroke-width="1"/>
  <text x="270" y="118" font-size="7" fill="#64748b">no</text>

  <rect x="150" y="122" width="80" height="22" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="190" y="136" text-anchor="middle" font-size="8" font-weight="bold" fill="#5b21b6">CSS bg</text>

  <!-- Pregunta 3 -->
  <rect x="240" y="122" width="55" height="40" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="268" y="138" text-anchor="middle" font-size="6" fill="#92400e">¿tiene JS</text>
  <text x="268" y="148" text-anchor="middle" font-size="6" fill="#92400e">propio?</text>

  <line x1="250" y1="162" x2="240" y2="180" stroke="#94a3b8" stroke-width="1"/>
  <text x="242" y="176" font-size="6" fill="#64748b">sí</text>
  <line x1="285" y1="162" x2="285" y2="180" stroke="#94a3b8" stroke-width="1"/>
  <text x="287" y="176" font-size="6" fill="#64748b">no</text>

  <rect x="210" y="180" width="60" height="20" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="240" y="193" text-anchor="middle" font-size="7" font-weight="bold" fill="#1e40af">&lt;object&gt;</text>

  <rect x="275" y="180" width="20" height="20" fill="#dbeafe" stroke="#3b82f6" rx="3"/>

  <!-- Pregunta 4: tamaño -->
  <rect x="10" y="150" width="120" height="26" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="70" y="167" text-anchor="middle" font-size="8" fill="#92400e">¿es muy pequeño (&lt;1KB)?</text>

  <line x1="40" y1="176" x2="25" y2="194" stroke="#94a3b8" stroke-width="1"/>
  <text x="22" y="190" font-size="7" fill="#64748b">sí</text>
  <line x1="100" y1="176" x2="115" y2="194" stroke="#94a3b8" stroke-width="1"/>
  <text x="112" y="190" font-size="7" fill="#64748b">no</text>

  <rect x="0" y="194" width="70" height="22" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="35" y="208" text-anchor="middle" font-size="7" font-weight="bold" fill="#9f1239">data URI</text>

  <rect x="90" y="194" width="70" height="22" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="125" y="208" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">&lt;img&gt;</text>

  <line x1="70" y1="150" x2="70" y2="106" stroke="#94a3b8" stroke-width="1" stroke-dasharray="2 2"/>

  <!-- Resumen -->
  <rect x="10" y="230" width="280" height="60" fill="#0f172a" rx="3"/>
  <text x="20" y="246" font-size="7" font-weight="bold" fill="#94a3b8">atajo</text>
  <text x="20" y="258" font-size="7" fill="#60a5fa">• ícono reutilizado en muchos sitios  → &lt;img&gt;</text>
  <text x="20" y="270" font-size="7" fill="#60a5fa">• ícono que cambia color con el tema  → inline</text>
  <text x="20" y="282" font-size="7" fill="#60a5fa">• patrón repetido de fondo  → CSS background</text>
</svg>
```

---

## Snippets copy-paste

### SVG inline (la opción por defecto para iconografía)

```html
<svg xmlns="http://www.w3.org/2000/svg"
     viewBox="0 0 24 24"
     width="24" height="24"
     fill="currentColor"
     aria-hidden="true"
     focusable="false">
  <path d="..."/>
</svg>
```

### SVG como `<img>` (logos, ilustraciones reutilizadas)

```html
<img src="/logo.svg"
     alt="Mi empresa"
     width="120"
     height="40"
     loading="lazy"
     decoding="async">
```

### SVG como fondo CSS (patrones decorativos)

```css
.hero {
  background-image: url('pattern.svg');
  background-repeat: repeat;
  background-size: 60px 60px;
}

.icono-sm {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath d='...' fill='%233b82f6'/%3E%3C/svg%3E");
}
```

### SVG con `<object>` (cuando quieres fallback y JS interno)

```html
<object type="image/svg+xml" data="grafico.svg" aria-label="Ventas 2024">
  <img src="grafico.png" alt="Gráfico de ventas 2024">
</object>

<script>
  const obj = document.querySelector('object');
  obj.addEventListener('load', () => {
    const svgDoc = obj.getSVGDocument();
    svgDoc.querySelector('circle').setAttribute('fill', 'red');
  });
</script>
```

### SVG con `<iframe>` (aislamiento completo)

```html
<iframe src="editor.svg"
        width="600"
        height="400"
        title="Editor de diagramas"
        sandbox="allow-scripts"></iframe>
```

### SVG como data URI en HTML

```html
<img src="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 10 10'%3E%3Ccircle cx='5' cy='5' r='4' fill='%233b82f6'/%3E%3C/svg%3E"
     alt="Punto azul">
```

### Truco: colorear un `<img>` SVG con filter

```css
.icono-img {
  filter: invert(45%) sepia(90%) saturate(600%) hue-rotate(200deg);
}
```

### Build tool: inline vía import (Vite + SVGR)

```jsx
import LogoIcon from './logo.svg?react';

function Header() {
  return <LogoIcon className="text-blue-500 w-8 h-8" />;
}
```

---

## Plantilla accesible universal

```html
<!-- Decorativo -->
<svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
  <path d="..." fill="currentColor"/>
</svg>

<!-- Con significado -->
<svg role="img"
     aria-labelledby="titulo-grafico desc-grafico"
     viewBox="0 0 400 300">
  <title id="titulo-grafico">Ventas por mes</title>
  <desc id="desc-grafico">Enero 45, febrero 62, tendencia ascendente.</desc>
  <!-- contenido visual -->
</svg>

<!-- En un botón con texto -->
<button>
  <svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
    <path d="..."/>
  </svg>
  Guardar
</button>

<!-- En un botón solo con ícono -->
<button aria-label="Cerrar diálogo">
  <svg aria-hidden="true" focusable="false" viewBox="0 0 24 24">
    <path d="..."/>
  </svg>
</button>
```
