# 24.1 Editores gráficos

Antes de escribir SVG a mano, la mayoría de iconos e ilustraciones nacen en un **editor gráfico**: Inkscape, Illustrator, Figma, Sketch, Affinity. Cada uno exporta SVG con su propia "huella" (metadatos, namespaces, ids crípticos). Elegir editor es menos importante que saber **cómo sale** el SVG y **qué toca limpiar** después.

---

## Cuándo usar editor vs escribir a mano

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada herramienta a lo suyo
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Caso</text>
  <text x="160" y="46" font-size="7" font-weight="bold" fill="#1e293b">Herramienta</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Iconos e ilustraciones</text>
  <text x="160" y="66" font-size="7" fill="#3b82f6">editor → export → SVGO</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">SVG desde datos</text>
  <text x="160" y="88" font-size="7" fill="#3b82f6">código (D3.js, manual)</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Prototipo rápido</text>
  <text x="160" y="110" font-size="7" fill="#3b82f6">SVGOMG + edición manual</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Sistema de iconos grande</text>
  <text x="160" y="132" font-size="7" fill="#3b82f6">editor + SVGR + build</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Animaciones complejas</text>
  <text x="160" y="154" font-size="7" fill="#3b82f6">editor + GSAP / Lottie</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">UI chart / gráfico datos</text>
  <text x="160" y="176" font-size="7" fill="#3b82f6">D3, Observable Plot</text>

  <text x="150" y="212" text-anchor="middle" font-size="7" fill="#94a3b8">"¿los datos dictan la forma?" → código</text>
  <text x="150" y="226" text-anchor="middle" font-size="7" fill="#94a3b8">"¿es una ilustración fija?" → editor</text>
  <text x="150" y="240" text-anchor="middle" font-size="7" fill="#94a3b8">los dos mundos se encuentran en el build step</text>
</svg>
```

---

## Inkscape — el vectorial libre

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG como formato nativo
  </text>

  <rect x="15" y="38" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Ventajas</text>
  <text x="25" y="70" font-size="7" fill="#475569">• gratis y open source (Linux/Mac/Win)</text>
  <text x="25" y="84" font-size="7" fill="#475569">• SVG es el formato interno, no de exportación</text>
  <text x="25" y="98" font-size="7" fill="#475569">• editor XML integrado (Ctrl+Shift+X)</text>
  <text x="25" y="112" font-size="7" fill="#475569">• herramienta de nodos muy completa</text>

  <rect x="15" y="132" width="270" height="68" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="148" font-size="9" font-weight="bold" fill="#92400e">Contras</text>
  <text x="25" y="164" font-size="7" fill="#475569">• añade namespaces inkscape:, sodipodi:</text>
  <text x="25" y="178" font-size="7" fill="#475569">• UI con curva de aprendizaje</text>
  <text x="25" y="192" font-size="7" fill="#475569">• lento con archivos grandes</text>

  <rect x="15" y="208" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="224" font-size="9" font-weight="bold" fill="#1e40af">Flujo limpio</text>
  <text x="25" y="240" font-size="7" fill="#475569">1. diseñar en Inkscape</text>
  <text x="25" y="252" font-size="7" fill="#475569">2. File → Save As → Plain SVG</text>
  <text x="25" y="264" font-size="7" fill="#475569">3. pasar por SVGO para limpieza final</text>
</svg>
```

---

## Adobe Illustrator — el profesional verboso

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG verboso, opciones críticas
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">El problema histórico</text>
  <text x="25" y="70" font-size="7" fill="#475569">SVGs con metadatos de Adobe, namespaces</text>
  <text x="25" y="84" font-size="7" fill="#475569">illustrator: y decimales a 6+ cifras</text>

  <rect x="15" y="106" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#1e40af">Opciones al exportar (File → Export As)</text>
  <text x="25" y="138" font-size="7" fill="#475569">Styling: Presentation Attributes (editable JS)</text>
  <text x="25" y="152" font-size="7" fill="#475569">Font: Convert to Outlines (no depende de fuentes)</text>
  <text x="25" y="166" font-size="7" fill="#475569">Images: Embed (autocontenido) o Link (externo)</text>
  <text x="25" y="180" font-size="7" fill="#475569">Object IDs: Minimal (ids cortos)</text>
  <text x="25" y="194" font-size="7" fill="#475569">Decimal Places: 1-2 (no 6)</text>

  <rect x="15" y="214" width="270" height="54" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="230" font-size="9" font-weight="bold" fill="#166534">Regla de oro</text>
  <text x="25" y="246" font-size="7" fill="#475569">siempre pasar el export de Illustrator por SVGO</text>
  <text x="25" y="260" font-size="7" fill="#475569">la reducción suele ser del 70-90% en iconos</text>
</svg>
```

---

## Figma — el estándar de 2024

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    diseño colaborativo en el navegador
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Export limpio por defecto</text>
  <text x="25" y="70" font-size="7" fill="#475569">• más limpio que Illustrator de fábrica</text>
  <text x="25" y="84" font-size="7" fill="#475569">• click derecho → Copy as SVG</text>
  <text x="25" y="98" font-size="7" fill="#475569">• o panel Export → SVG → Export</text>
  <text x="25" y="112" font-size="7" fill="#475569">• frames 24×24 con Auto Layout ideales para iconos</text>

  <rect x="15" y="130" width="270" height="68" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="146" font-size="9" font-weight="bold" fill="#1e40af">Plugin: SVGO Compressor</text>
  <text x="25" y="162" font-size="7" fill="#475569">aplica SVGO directamente dentro de Figma,</text>
  <text x="25" y="176" font-size="7" fill="#475569">el archivo sale ya optimizado sin paso extra</text>
  <text x="25" y="190" font-size="7" fill="#475569">→ comunidad Figma → buscar "SVGO"</text>

  <rect x="15" y="206" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="222" font-size="9" font-weight="bold" fill="#92400e">Atención</text>
  <text x="25" y="238" font-size="7" fill="#475569">• efectos avanzados → filtros SVG costosos</text>
  <text x="25" y="252" font-size="7" fill="#475569">• texto: ¿outline o &lt;text&gt;? elige según uso</text>
</svg>
```

---

## Sketch y Affinity Designer

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    las alternativas menos mainstream
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#5b21b6">Sketch (solo macOS)</text>
  <text x="25" y="70" font-size="7" fill="#475569">• export SVG comparable a Figma</text>
  <text x="25" y="84" font-size="7" fill="#475569">• adopción bajando en favor de Figma</text>
  <text x="25" y="98" font-size="7" fill="#475569">• flujo idéntico: export → SVGO</text>
  <text x="25" y="112" font-size="7" fill="#5b21b6">→ si el equipo lo usa, no cambies</text>

  <rect x="15" y="126" width="270" height="98" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#1e40af">Affinity Designer</text>
  <text x="25" y="158" font-size="7" fill="#475569">• macOS, Windows, iPad</text>
  <text x="25" y="172" font-size="7" fill="#475569">• pago único (no suscripción)</text>
  <text x="25" y="186" font-size="7" fill="#475569">• export SVG limpio y rápido</text>
  <text x="25" y="200" font-size="7" fill="#475569">• buena relación calidad/precio vs Illustrator</text>
  <text x="25" y="214" font-size="7" fill="#1e40af">→ menor adopción = menos plugins/recursos</text>
</svg>
```

---

## El patrón común: export → limpiar

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el flujo funciona para todos
  </text>

  <rect x="30" y="50" width="60" height="40" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="60" y="74" text-anchor="middle" font-size="8" fill="#1e40af">diseño</text>
  <text x="60" y="85" text-anchor="middle" font-size="7" fill="#475569">editor</text>

  <text x="100" y="74" font-size="12" fill="#64748b">→</text>

  <rect x="115" y="50" width="60" height="40" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="145" y="74" text-anchor="middle" font-size="8" fill="#92400e">export</text>
  <text x="145" y="85" text-anchor="middle" font-size="7" fill="#475569">.svg</text>

  <text x="185" y="74" font-size="12" fill="#64748b">→</text>

  <rect x="200" y="50" width="60" height="40" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="230" y="74" text-anchor="middle" font-size="8" fill="#166534">limpiar</text>
  <text x="230" y="85" text-anchor="middle" font-size="7" fill="#475569">SVGO</text>

  <text x="150" y="125" text-anchor="middle" font-size="8" fill="#475569">→ sprite / componente / asset</text>

  <rect x="15" y="148" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="164" font-size="7" font-family="monospace" fill="#94a3b8">// script del pipeline típico</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#60a5fa">$ svgo -f src/icons -o dist/icons</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#60a5fa">$ svg-sprite --symbol dist/icons/*.svg</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">$ npx @svgr/cli src/icons -o src/components</text>
  <text x="25" y="226" font-size="7" fill="#fbbf24">→ el editor elegido es solo la primera pieza</text>
</svg>
```
