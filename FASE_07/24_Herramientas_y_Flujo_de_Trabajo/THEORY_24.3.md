# 24.3 Herramientas online

No todo pasa por un editor instalado. Para tareas puntuales —optimizar un SVG, entender un `d` complejo, convertir a componente React, generar un fondo— hay **herramientas online** que resuelven en 30 segundos lo que llevaría 10 minutos de setup local.

---

## SVGOMG — el frontend de SVGO

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVGO en el navegador
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Qué hace</text>
  <text x="25" y="70" font-size="7" fill="#475569">• interfaz web oficial para SVGO</text>
  <text x="25" y="84" font-size="7" fill="#475569">• sliders y toggles por cada plugin</text>
  <text x="25" y="98" font-size="7" fill="#475569">• preview antes/después en tiempo real</text>
  <text x="25" y="112" font-size="7" fill="#475569">• porcentaje de reducción visible</text>

  <rect x="15" y="130" width="270" height="88" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="146" font-size="9" font-weight="bold" fill="#1e40af">Flujo típico</text>
  <text x="25" y="162" font-size="7" fill="#475569">1. arrastrar el .svg a la ventana</text>
  <text x="25" y="176" font-size="7" fill="#475569">2. desactivar removeViewBox y cleanupIds</text>
  <text x="25" y="190" font-size="7" fill="#475569">3. verificar visualmente que nada se rompe</text>
  <text x="25" y="204" font-size="7" fill="#475569">4. Download o Copy Markup</text>

  <rect x="15" y="226" width="270" height="40" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="242" font-size="9" font-weight="bold" fill="#92400e">URL</text>
  <text x="25" y="258" font-size="7" font-family="monospace" fill="#475569">jakearchibald.github.io/svgomg/</text>
</svg>
```

---

## SVG Path Editor y Visualizer

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    trabajar con el atributo d
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">SVG Path Editor</text>
  <text x="25" y="70" font-size="7" fill="#475569">editor visual punto a punto del atributo d</text>
  <text x="25" y="84" font-size="7" fill="#475569">arrastra nodos, cambia curvas, añade comandos</text>
  <text x="25" y="98" font-size="7" fill="#475569">el string d se actualiza en tiempo real</text>
  <text x="25" y="112" font-size="7" fill="#475569">ideal para retocar paths generados automáticamente</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#1e40af">yqnn.github.io/svg-path-editor/</text>

  <rect x="15" y="146" width="270" height="116" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">SVG Path Visualizer</text>
  <text x="25" y="178" font-size="7" fill="#475569">explica cada comando del path paso a paso</text>
  <text x="25" y="192" font-size="7" fill="#475569">• M moveTo, L lineTo, C curveTo</text>
  <text x="25" y="206" font-size="7" fill="#475569">• anota cada segmento con su tipo y valores</text>
  <text x="25" y="220" font-size="7" fill="#475569">• indispensable para entender paths complejos</text>
  <text x="25" y="234" font-size="7" fill="#475569">• aprendizaje de Arc (A) y curvas Bézier</text>
  <text x="25" y="250" font-size="7" font-family="monospace" fill="#166534">svg-path-visualizer.netlify.app</text>
</svg>
```

---

## Editores online alternativos

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    sin instalar nada
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Herramienta</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Para qué</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Boxy SVG</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">editor completo tipo Inkscape</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Method Draw</text>
  <text x="170" y="88" font-size="7" fill="#3b82f6">editor simple para empezar</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">SVG Viewer (svgviewer.dev)</text>
  <text x="170" y="110" font-size="7" fill="#3b82f6">preview + optimize + React</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">SVGR Playground</text>
  <text x="170" y="132" font-size="7" fill="#3b82f6">SVG → componente React</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">URL Encoder for SVG</text>
  <text x="170" y="154" font-size="7" fill="#3b82f6">convierte a data URI CSS</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">SVGOMG</text>
  <text x="170" y="176" font-size="7" fill="#3b82f6">optimización visual</text>

  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">navegador = entorno de desarrollo completo</text>
  <text x="150" y="222" text-anchor="middle" font-size="7" fill="#94a3b8">para tareas rápidas no necesitas instalar nada</text>
</svg>
```

---

## Generadores de SVG

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    generar fondos y patrones
  </text>

  <rect x="15" y="38" width="270" height="68" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#5b21b6">Haikei (haikei.app)</text>
  <text x="25" y="70" font-size="7" fill="#475569">generador de fondos orgánicos: blobs, ondas,</text>
  <text x="25" y="84" font-size="7" fill="#475569">gradientes, partículas, manchas</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ ideal para heros de landing pages</text>

  <rect x="15" y="114" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="130" font-size="9" font-weight="bold" fill="#1e40af">Pattern Monster</text>
  <text x="25" y="146" font-size="7" fill="#475569">patrones SVG repetibles (geométricos, tramas)</text>
  <text x="25" y="160" font-size="7" fill="#475569">descarga como SVG o CSS data URI</text>

  <rect x="15" y="182" width="270" height="58" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="198" font-size="9" font-weight="bold" fill="#166534">Otros</text>
  <text x="25" y="214" font-size="7" fill="#475569">• Hero Patterns — fondos SVG gratis con tinte</text>
  <text x="25" y="226" font-size="7" fill="#475569">• Blobmaker — blob shapes orgánicos</text>
  <text x="25" y="238" font-size="7" fill="#475569">• Transform.tools — conversión entre formatos</text>
</svg>
```

---

## URL encoder para data URI

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG como background-image CSS
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">El problema</text>
  <text x="25" y="70" font-size="7" fill="#475569">meter un SVG dentro de url() en CSS requiere</text>
  <text x="25" y="84" font-size="7" fill="#475569">escapar caracteres: #, &amp;, &lt;, &gt;, ", '</text>
  <text x="25" y="98" font-size="7" fill="#475569">hacerlo a mano es tedioso y propenso a errores</text>

  <rect x="15" y="126" width="270" height="118" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// URL-encode con herramienta online</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#60a5fa">.hero {</text>
  <text x="25" y="172" font-size="8" font-family="monospace" fill="#34d399">  background-image: url(</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#fbbf24">   "data:image/svg+xml,%3Csvg...</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#fbbf24">    %3Ccircle cx='50' cy='50'/%3E</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#fbbf24">    %3C/svg%3E");</text>
  <text x="25" y="228" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
  <text x="25" y="238" font-size="7" fill="#94a3b8">→ "URL Encoder for SVG" (yoksel.github.io)</text>
</svg>
```
