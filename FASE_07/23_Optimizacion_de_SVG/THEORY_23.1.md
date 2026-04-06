# 23.1 Por qué optimizar SVG

Un SVG recién exportado de Illustrator, Figma o Inkscape arrastra un equipaje que no sirve en la web: metadatos del editor, namespaces propietarios, coordenadas con 10 decimales, grupos vacíos, IDs generados al azar. Un icono simple puede pesar 5KB cuando su versión optimizada ocuparía 0.5KB. Esta subsección explica **qué** sobra y **por qué** importa.

---

## Lo que el editor mete en el archivo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el "bagaje" de un .svg exportado
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#ef4444">&lt;?xml version="1.0"?&gt;</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#ef4444">&lt;!DOCTYPE svg PUBLIC "-//W3C..."&gt;</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#ef4444">&lt;!-- Generator: Adobe Illustrator 27.0 --&gt;</text>
  <text x="25" y="96" font-size="7" font-family="monospace" fill="#ef4444">&lt;svg xmlns:xlink="..."</text>
  <text x="25" y="110" font-size="7" font-family="monospace" fill="#ef4444">     xmlns:inkscape="..."</text>
  <text x="25" y="124" font-size="7" font-family="monospace" fill="#ef4444">     xmlns:sodipodi="..."</text>
  <text x="25" y="138" font-size="7" font-family="monospace" fill="#f59e0b">     inkscape:version="1.2.2"</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#f59e0b">     sodipodi:docname="icono.svg"&gt;</text>

  <text x="25" y="172" font-size="7" font-family="monospace" fill="#94a3b8">  &lt;metadata id="metadata6"&gt;</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#94a3b8">    &lt;rdf:RDF&gt;&lt;cc:Work&gt;...&lt;/cc:Work&gt;</text>
  <text x="25" y="200" font-size="7" font-family="monospace" fill="#94a3b8">  &lt;/metadata&gt;</text>

  <text x="25" y="220" font-size="7" font-family="monospace" fill="#60a5fa">  &lt;g id="Layer_1" display="inline"&gt;</text>
  <text x="25" y="234" font-size="7" font-family="monospace" fill="#60a5fa">    &lt;g&gt;&lt;g id="grupo_vacio"&gt;&lt;/g&gt;&lt;/g&gt;</text>
  <text x="25" y="248" font-size="7" font-family="monospace" fill="#34d399">    &lt;path d="M49.9998,100.0002..."/&gt;</text>
  <text x="25" y="262" font-size="7" font-family="monospace" fill="#60a5fa">  &lt;/g&gt;</text>
</svg>
```

---

## Qué sobra, categorizado

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    tipos de basura en un SVG
  </text>

  <rect x="15" y="38" width="270" height="46" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Metadatos</text>
  <text x="25" y="70" font-size="7" fill="#475569">DOCTYPE, &lt;metadata&gt;, comentarios del editor,</text>
  <text x="25" y="80" font-size="7" fill="#475569">namespaces inkscape:/sodipodi:/illustrator:</text>

  <rect x="15" y="92" width="270" height="46" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="108" font-size="9" font-weight="bold" fill="#92400e">Estructura inútil</text>
  <text x="25" y="124" font-size="7" fill="#475569">grupos &lt;g&gt; vacíos o sin atributos,</text>
  <text x="25" y="134" font-size="7" fill="#475569">IDs autogenerados (Layer_1, path4532)</text>

  <rect x="15" y="146" width="270" height="46" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#1e40af">Redundancia numérica</text>
  <text x="25" y="178" font-size="7" fill="#475569">cx="49.9999999" en vez de cx="50",</text>
  <text x="25" y="188" font-size="7" fill="#475569">stroke-width="1.000" en vez de stroke-width="1"</text>

  <rect x="15" y="200" width="270" height="46" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="216" font-size="9" font-weight="bold" fill="#5b21b6">Atributos por defecto</text>
  <text x="25" y="232" font-size="7" fill="#475569">display="inline", fill-opacity="1",</text>
  <text x="25" y="242" font-size="7" fill="#475569">stroke-linecap="butt" — valores implícitos</text>
</svg>
```

---

## El impacto real en bytes

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un icono real, antes y después
  </text>

  <rect x="30" y="50" width="240" height="32" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="40" y="70" font-size="9" fill="#991b1b">Exportado de Figma: 4.2 KB</text>
  <rect x="30" y="50" width="240" height="32" fill="#ef4444" opacity="0.15"/>

  <rect x="30" y="96" width="240" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="40" y="116" font-size="9" fill="#92400e">SVGO con preset-default: 0.8 KB</text>
  <rect x="30" y="96" width="45" height="32" fill="#f59e0b" opacity="0.2"/>

  <rect x="30" y="142" width="240" height="32" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="40" y="162" font-size="9" fill="#166534">SVGO + compresión gzip: 0.3 KB</text>
  <rect x="30" y="142" width="17" height="32" fill="#10b981" opacity="0.2"/>

  <text x="150" y="200" text-anchor="middle" font-size="8" fill="#475569">reducción del 93% respecto al original</text>
  <text x="150" y="216" text-anchor="middle" font-size="7" fill="#94a3b8">sin perder un solo pixel de fidelidad visual</text>
</svg>
```

---

## Por qué importa más allá del peso

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los 4 beneficios de optimizar
  </text>

  <rect x="15" y="38" width="270" height="52" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">1. Bytes en la red</text>
  <text x="25" y="70" font-size="7" fill="#475569">menos tráfico, mejor TTFB de los assets,</text>
  <text x="25" y="82" font-size="7" fill="#475569">impacto directo en Core Web Vitals (LCP)</text>

  <rect x="15" y="98" width="270" height="52" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="114" font-size="9" font-weight="bold" fill="#1e40af">2. Parseo del DOM</text>
  <text x="25" y="130" font-size="7" fill="#475569">menos nodos que el navegador construye,</text>
  <text x="25" y="142" font-size="7" fill="#475569">render más rápido cuando el SVG es inline</text>

  <rect x="15" y="158" width="270" height="52" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="174" font-size="9" font-weight="bold" fill="#5b21b6">3. Legibilidad humana</text>
  <text x="25" y="190" font-size="7" fill="#475569">un SVG limpio se puede diffear en Git,</text>
  <text x="25" y="202" font-size="7" fill="#475569">revisar en PR y editar a mano si hace falta</text>

  <rect x="15" y="218" width="270" height="52" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="234" font-size="9" font-weight="bold" fill="#92400e">4. Seguridad</text>
  <text x="25" y="250" font-size="7" fill="#475569">eliminar scripts, event handlers y metadatos</text>
  <text x="25" y="262" font-size="7" fill="#475569">reduce la superficie de ataque XSS</text>
</svg>
```

---

## El mismo icono, antes vs después

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    diff visual: antes vs después
  </text>

  <rect x="15" y="38" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// antes (418 bytes)</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#ef4444">&lt;svg xmlns="..." xmlns:xlink="..."</text>
  <text x="25" y="80" font-size="7" font-family="monospace" fill="#ef4444">     version="1.1" id="Layer_1"</text>
  <text x="25" y="92" font-size="7" font-family="monospace" fill="#ef4444">     viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="104" font-size="7" font-family="monospace" fill="#ef4444">  &lt;g id="grupo_1" display="inline"&gt;</text>
  <text x="25" y="116" font-size="7" font-family="monospace" fill="#ef4444">    &lt;path fill="#3B82F6" fill-opacity="1"</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#ef4444">          d="M12.0001,2.0000 L22.0000,22.0..."/&gt;</text>
  <text x="25" y="140" font-size="7" font-family="monospace" fill="#ef4444">  &lt;/g&gt;</text>

  <rect x="15" y="162" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#94a3b8">// después (78 bytes)</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#34d399">&lt;svg xmlns="http://www.w3.org/2000/svg"</text>
  <text x="25" y="204" font-size="7" font-family="monospace" fill="#34d399">     viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#34d399">  &lt;path fill="#3b82f6"</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#34d399">        d="M12 2l10 20..."/&gt;</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#34d399">&lt;/svg&gt;</text>

  <text x="25" y="258" font-size="7" fill="#fbbf24">→ mismo pixel-perfect, 81% menos bytes</text>
</svg>
```
