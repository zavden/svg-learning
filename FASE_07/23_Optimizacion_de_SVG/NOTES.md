# NOTES — Optimización de SVG

Apuntes prácticos: cómo medir antes de optimizar, casos donde "el SVG ya está bien", los plugins menos conocidos que dan sorpresas, cuándo Canvas gana la partida y cómo auditar producción.

---

## Compatibilidad y soporte

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué funciona dónde
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Feature</text>
  <text x="180" y="46" font-size="7" font-weight="bold" fill="#1e293b">Soporte</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">SVGO (runtime Node 18+)</text>
  <text x="180" y="66" font-size="7" fill="#10b981">todos los entornos</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">gzip image/svg+xml</text>
  <text x="180" y="88" font-size="7" fill="#10b981">universal</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">brotli (Content-Encoding: br)</text>
  <text x="180" y="110" font-size="7" fill="#10b981">evergreen, no IE</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">.svgz (en uso)</text>
  <text x="180" y="132" font-size="7" fill="#f59e0b">en desuso</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">will-change</text>
  <text x="180" y="154" font-size="7" fill="#10b981">evergreen</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">CSS filter: blur/drop-shadow</text>
  <text x="180" y="176" font-size="7" fill="#10b981">universal</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">OffscreenCanvas</text>
  <text x="180" y="198" font-size="7" fill="#f59e0b">no Safari &lt;17</text>

  <text x="150" y="230" text-anchor="middle" font-size="7" fill="#94a3b8">SVGO corre en cualquier entorno Node; los</text>
  <text x="150" y="244" text-anchor="middle" font-size="7" fill="#94a3b8">conceptos de rendimiento (capas GPU) son universales</text>
</svg>
```

---

## Medir antes de optimizar

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    primera regla: medir, no asumir
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Peso del archivo</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">du -h icon.svg</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">gzip -c icon.svg | wc -c</text>

  <rect x="15" y="106" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#166534">Peso en red</text>
  <text x="25" y="138" font-size="7" fill="#475569">DevTools → Network → columna "Size"</text>
  <text x="25" y="152" font-size="7" fill="#475569">dos números: transferido / descomprimido</text>

  <rect x="15" y="174" width="270" height="58" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#5b21b6">Rendimiento de render</text>
  <text x="25" y="206" font-size="7" fill="#475569">DevTools → Performance → grabar interacción</text>
  <text x="25" y="220" font-size="7" fill="#475569">buscar bars rojos en "Rendering" y "Painting"</text>

  <rect x="15" y="240" width="270" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="256" font-size="9" font-weight="bold" fill="#92400e">Lighthouse</text>
  <text x="25" y="268" font-size="7" fill="#475569">auditoría completa con métricas Core Web Vitals</text>
</svg>
```

---

## Cuándo el SVG ya está bien

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    no todo SVG necesita SVGO
  </text>

  <rect x="15" y="38" width="270" height="76" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Ya optimizado</text>
  <text x="25" y="70" font-size="7" fill="#475569">• Heroicons, Lucide, Tabler, Phosphor</text>
  <text x="25" y="84" font-size="7" fill="#475569">• iconos de Material Symbols (Google)</text>
  <text x="25" y="98" font-size="7" fill="#475569">• SVGs escritos a mano siguiendo las reglas</text>

  <rect x="15" y="124" width="270" height="72" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="140" font-size="9" font-weight="bold" fill="#92400e">Sí hay que optimizar</text>
  <text x="25" y="156" font-size="7" fill="#475569">• exportado de Figma, Sketch, Illustrator</text>
  <text x="25" y="170" font-size="7" fill="#475569">• descargado de bancos como freesvg o flaticon</text>
  <text x="25" y="184" font-size="7" fill="#475569">• trazado automático (vectorize, AutoTrace)</text>

  <rect x="15" y="206" width="270" height="66" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="222" font-size="9" font-weight="bold" fill="#991b1b">Cuidado al optimizar</text>
  <text x="25" y="238" font-size="7" fill="#475569">• SVGs con filtros o animaciones complejas</text>
  <text x="25" y="252" font-size="7" fill="#475569">• SVGs que son manipulados por JS</text>
  <text x="25" y="266" font-size="7" fill="#475569">• ilustraciones "artísticas" con degradados</text>
</svg>
```

---

## El plugin `removeDimensions` y cuando quererlo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    width/height vs viewBox
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Por defecto</text>
  <text x="25" y="70" font-size="7" fill="#475569">el SVG se renderiza al tamaño de width/height,</text>
  <text x="25" y="84" font-size="7" fill="#475569">no al del contenedor. Puede ignorar el CSS si</text>
  <text x="25" y="98" font-size="7" fill="#475569">el consumidor no sabe sobreescribirlo.</text>
  <text x="25" y="114" font-size="7" font-family="monospace" fill="#475569">&lt;svg width="24" height="24" viewBox="..."&gt;</text>

  <rect x="15" y="136" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="152" font-size="9" font-weight="bold" fill="#166534">Con removeDimensions</text>
  <text x="25" y="168" font-size="7" fill="#475569">solo queda viewBox. El SVG se escala al 100%</text>
  <text x="25" y="182" font-size="7" fill="#475569">del contenedor automáticamente por defecto.</text>
  <text x="25" y="196" font-size="7" fill="#475569">CSS puede controlar el tamaño libremente.</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 24 24"&gt;</text>

  <text x="150" y="250" text-anchor="middle" font-size="7" fill="#94a3b8">recomendado para iconos consumidos en componentes</text>
</svg>
```

---

## El "truco" de sortAttrs para gzip

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    gzip recompensa la predictibilidad
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Sin sortAttrs</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;circle fill="red" r="5" cx="10" cy="20"/&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">&lt;circle cy="20" r="5" cx="30" fill="red"/&gt;</text>
  <text x="25" y="100" font-size="7" fill="#475569">cada circle tiene atributos en orden distinto</text>
  <text x="25" y="114" font-size="7" fill="#991b1b">gzip no detecta el patrón tan bien</text>

  <rect x="15" y="138" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Con sortAttrs</text>
  <text x="25" y="170" font-size="7" fill="#475569">orden fijo: cx, cy, r, fill (o el que elijas)</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#475569">&lt;circle cx="10" cy="20" r="5" fill="red"/&gt;</text>
  <text x="25" y="198" font-size="7" font-family="monospace" fill="#475569">&lt;circle cx="30" cy="20" r="5" fill="red"/&gt;</text>
  <text x="25" y="212" font-size="7" fill="#166534">→ gzip detecta el patrón, comprime mejor</text>

  <text x="150" y="244" text-anchor="middle" font-size="7" fill="#94a3b8">ahorro extra típico: 2-5% sobre el ya comprimido</text>
</svg>
```

---

## DevTools: auditoría de SVG en producción

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    las 4 pestañas que hay que mirar
  </text>

  <rect x="15" y="38" width="270" height="56" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Network</text>
  <text x="25" y="70" font-size="7" fill="#475569">filtra por "svg" — columna Size muestra</text>
  <text x="25" y="84" font-size="7" fill="#475569">transferido vs descomprimido</text>

  <rect x="15" y="102" width="270" height="56" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="118" font-size="9" font-weight="bold" fill="#166534">Performance</text>
  <text x="25" y="134" font-size="7" fill="#475569">graba 5 segundos de interacción, busca</text>
  <text x="25" y="148" font-size="7" fill="#475569">"Paint" y "Composite Layers" en el timeline</text>

  <rect x="15" y="166" width="270" height="56" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="182" font-size="9" font-weight="bold" fill="#5b21b6">Rendering (⋮ → More tools)</text>
  <text x="25" y="198" font-size="7" fill="#475569">"Paint flashing" y "Layer borders": visualiza</text>
  <text x="25" y="212" font-size="7" fill="#475569">qué se repinta y qué está en su propia capa</text>

  <rect x="15" y="230" width="270" height="40" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="246" font-size="9" font-weight="bold" fill="#92400e">Lighthouse</text>
  <text x="25" y="260" font-size="7" fill="#475569">audit de Performance da sugerencias accionables</text>
</svg>
```

---

## Cuándo Canvas gana la partida

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la línea que no conviene cruzar con SVG
  </text>

  <rect x="15" y="38" width="270" height="66" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntomas: tiempo de migrar</text>
  <text x="25" y="70" font-size="7" fill="#475569">• scroll lagea en listas con gráficos SVG</text>
  <text x="25" y="84" font-size="7" fill="#475569">• miles de nodos visibles a la vez</text>
  <text x="25" y="98" font-size="7" fill="#475569">• animación JS sobre cientos de elementos</text>

  <rect x="15" y="112" width="270" height="82" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="128" font-size="9" font-weight="bold" fill="#1e40af">Híbrido: lo mejor de los dos mundos</text>
  <text x="25" y="144" font-size="7" fill="#475569">• Canvas para la capa de datos (miles puntos)</text>
  <text x="25" y="158" font-size="7" fill="#475569">• SVG superpuesto para ejes, labels, tooltips</text>
  <text x="25" y="172" font-size="7" fill="#475569">• el SVG se beneficia de eventos DOM nativos</text>
  <text x="25" y="186" font-size="7" fill="#1e40af">→ patrón común en D3, Observable Plot, Plotly</text>

  <text x="150" y="220" text-anchor="middle" font-size="7" fill="#94a3b8">D3 lleva años usando este patrón híbrido:</text>
  <text x="150" y="234" text-anchor="middle" font-size="7" fill="#94a3b8">Canvas para rendimiento, SVG para interactividad</text>
  <text x="150" y="248" text-anchor="middle" font-size="7" fill="#94a3b8">y accesibilidad donde más importa</text>
</svg>
```
