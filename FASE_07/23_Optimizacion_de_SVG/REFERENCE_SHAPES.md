# REFERENCE_SHAPES — Optimización de SVG

Chuleta de optimización: comando mínimo de SVGO, config pragmática, comandos de diagnóstico, tabla de "qué quita cuánto" y checklist antes de commit.

---

## El comando mínimo

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVGO sin config
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="7" font-family="monospace" fill="#94a3b8"># un archivo</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">$ svgo icon.svg</text>
  <text x="25" y="94" font-size="7" font-family="monospace" fill="#94a3b8"># a otro archivo</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#60a5fa">$ svgo icon.svg -o icon.min.svg</text>
  <text x="25" y="132" font-size="7" font-family="monospace" fill="#94a3b8"># carpeta entera</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#60a5fa">$ svgo -f src/icons -o dist/icons</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#94a3b8"># modo check (CI)</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#60a5fa">$ svgo --check src/icons</text>
</svg>
```

---

## svgo.config.js pragmático

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    config que no rompe nada
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">export default {</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#34d399">  multipass: true,</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  js2svg: {</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#ec4899">    indent: 2,</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#ec4899">    pretty: false</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#34d399">  },</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#34d399">  plugins: [</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#fbbf24">    {</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#fbbf24">      name: 'preset-default',</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#fbbf24">      params: {</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#fbbf24">        overrides: {</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#ec4899">          removeViewBox: false,</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#ec4899">          cleanupIds: false</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#fbbf24">        }</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#fbbf24">      }</text>
  <text x="25" y="264" font-size="8" font-family="monospace" fill="#60a5fa">  ]}</text>
</svg>
```

---

## Comandos de diagnóstico

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿cuánto pesa? ¿comprime?
  </text>

  <rect x="15" y="38" width="270" height="206" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8"># tamaño en disco</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">$ du -h icon.svg</text>
  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8"># tamaño tras gzip local</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#34d399">$ gzip -c icon.svg | wc -c</text>
  <text x="25" y="130" font-size="7" font-family="monospace" fill="#94a3b8"># comparar antes/después</text>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#34d399">$ svgo icon.svg -o out.svg</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#34d399">  | grep -i "byte"</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#94a3b8"># verificar encoding del servidor</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#fbbf24">$ curl -sI -H "Accept-Encoding: gzip" \</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#fbbf24">  URL | grep -i encoding</text>
  <text x="25" y="232" font-size="7" fill="#94a3b8">Content-Encoding: gzip   ← esperado</text>
</svg>
```

---

## Qué quita cuánto

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    orden de magnitud del ahorro
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Plugin / técnica</text>
  <text x="200" y="46" font-size="7" font-weight="bold" fill="#1e293b">Ahorro típico</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">convertPathData</text>
  <text x="200" y="66" font-size="7" fill="#10b981">30-50%</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">removeEditorsNSData</text>
  <text x="200" y="88" font-size="7" fill="#10b981">10-20%</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">collapseGroups</text>
  <text x="200" y="110" font-size="7" fill="#10b981">5-15%</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">cleanupNumericValues</text>
  <text x="200" y="132" font-size="7" fill="#10b981">5-15%</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">removeMetadata</text>
  <text x="200" y="154" font-size="7" fill="#10b981">3-10%</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">convertColors</text>
  <text x="200" y="176" font-size="7" fill="#10b981">1-5%</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">mergePaths</text>
  <text x="200" y="198" font-size="7" fill="#10b981">0-20% (depende)</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" fill="#475569">+ gzip al final</text>
  <text x="200" y="220" font-size="7" fill="#3b82f6">×0.25-0.35</text>

  <text x="150" y="250" text-anchor="middle" font-size="7" fill="#94a3b8">preset-default combina la mayoría y el</text>
  <text x="150" y="262" text-anchor="middle" font-size="7" fill="#94a3b8">ahorro acumulado típico es del 60-80%</text>
</svg>
```

---

## Tabla de plugins "no tocar"

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    mantener DESACTIVADOS por defecto
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Plugin</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Por qué</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">removeViewBox</text>
  <text x="170" y="66" font-size="7" fill="#991b1b">rompe escalado</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">cleanupIds</text>
  <text x="170" y="88" font-size="7" fill="#991b1b">rompe &lt;use&gt;, JS, CSS</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">convertShapeToPath</text>
  <text x="170" y="110" font-size="7" fill="#991b1b">pierde semántica</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">inlineStyles</text>
  <text x="170" y="132" font-size="7" fill="#991b1b">rompe @keyframes CSS</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">removeHiddenElems</text>
  <text x="170" y="154" font-size="7" fill="#991b1b">elimina hooks JS</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">mergePaths</text>
  <text x="170" y="176" font-size="7" fill="#991b1b">pierde ids por path</text>

  <text x="150" y="206" text-anchor="middle" font-size="7" fill="#94a3b8">todos se activan con preset-default. Usa overrides.</text>
</svg>
```

---

## Checklist antes de commit

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    10 comprobaciones antes de commit
  </text>

  <text x="25" y="50" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="50" font-size="7" fill="#475569">SVGO pasado (pre-commit o manual)</text>

  <text x="25" y="70" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="70" font-size="7" fill="#475569">viewBox presente, no solo width/height</text>

  <text x="25" y="90" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="90" font-size="7" fill="#475569">IDs útiles conservados (usados por JS/CSS)</text>

  <text x="25" y="110" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="110" font-size="7" fill="#475569">sin namespaces de editor (inkscape:, sodipodi:)</text>

  <text x="25" y="130" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="130" font-size="7" fill="#475569">sin metadatos ni comentarios del editor</text>

  <text x="25" y="150" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="150" font-size="7" fill="#475569">sin scripts ni event handlers inline (on*)</text>

  <text x="25" y="170" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="170" font-size="7" fill="#475569">decimales reducidos a 1-2 cifras</text>

  <text x="25" y="190" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="190" font-size="7" fill="#475569">grupos innecesarios aplanados</text>

  <text x="25" y="210" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="210" font-size="7" fill="#475569">currentColor en iconos (si se tematizan)</text>

  <text x="25" y="230" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="230" font-size="7" fill="#475569">tamaño en red verificado en DevTools</text>

  <text x="25" y="250" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="250" font-size="7" fill="#475569">render correcto en el navegador tras optimizar</text>

  <text x="150" y="272" text-anchor="middle" font-size="7" fill="#94a3b8">la última revisión siempre es visual</text>
</svg>
```
