# 25.4 Compatibilidad entre navegadores

En 2026, SVG está bien soportado en los navegadores evergreen (Chrome, Firefox, Safari, Edge). Las diferencias relevantes aparecen en SMIL, filtros avanzados, y soporte de IE11 si todavía lo tienes como requisito. La estrategia es conocer las fronteras, no evitar el SVG por miedo.

---

## Soporte en navegadores modernos

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    evergreens: Chrome, Firefox, Safari, Edge
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Característica</text>
  <text x="180" y="46" font-size="7" font-weight="bold" fill="#1e293b">Estado</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Formas, paths, texto</text>
  <text x="180" y="66" font-size="7" fill="#10b981">✓ completo</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Gradientes, patrones</text>
  <text x="180" y="88" font-size="7" fill="#10b981">✓ completo</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Filtros (la mayoría)</text>
  <text x="180" y="110" font-size="7" fill="#10b981">✓ completo</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Animaciones CSS en SVG</text>
  <text x="180" y="132" font-size="7" fill="#10b981">✓ completo</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Variables CSS (--var)</text>
  <text x="180" y="154" font-size="7" fill="#10b981">✓ completo</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">SMIL (&lt;animate&gt;)</text>
  <text x="180" y="176" font-size="7" fill="#f59e0b">⚠ deprecación Chrome</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">SVG 2 (href simple)</text>
  <text x="180" y="198" font-size="7" fill="#10b981">✓ evergreen</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" fill="#475569">paint-order, context-fill</text>
  <text x="180" y="220" font-size="7" fill="#10b981">✓ completo</text>

  <text x="150" y="254" text-anchor="middle" font-size="7" fill="#94a3b8">el 99% del SVG funciona igual en todos</text>
  <text x="150" y="268" text-anchor="middle" font-size="7" fill="#94a3b8">los detalles viven en SMIL y filtros exóticos</text>
  <text x="150" y="282" text-anchor="middle" font-size="7" fill="#94a3b8">→ caniuse.com siempre antes de asumir</text>
</svg>
```

---

## SMIL: el estado incómodo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    funciona, pero no es el futuro
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">La historia corta</text>
  <text x="25" y="70" font-size="7" fill="#475569">• Chrome anunció deprecación en 2015</text>
  <text x="25" y="84" font-size="7" fill="#475569">• la revirtió por falta de alternativa</text>
  <text x="25" y="98" font-size="7" fill="#475569">• sigue funcionando en 2026 sin cambios</text>
  <text x="25" y="112" font-size="7" fill="#92400e">→ nadie sabe qué pasará a 5 años</text>

  <rect x="15" y="138" width="270" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Recomendación</text>
  <text x="25" y="170" font-size="7" fill="#475569">• producción nueva: CSS o Web Animations API</text>
  <text x="25" y="184" font-size="7" fill="#475569">• SMIL solo para cosas que CSS no hace</text>
  <text x="25" y="198" font-size="7" fill="#475569">  (animateMotion en path, por ejemplo)</text>
  <text x="25" y="212" font-size="7" fill="#475569">• existente en producción: no migrar por urgencia</text>
  <text x="25" y="226" font-size="7" fill="#166534">→ SMIL es código legacy funcional</text>
  <text x="25" y="240" font-size="7" fill="#166534">→ nuevas cosas → tecnologías con futuro claro</text>
</svg>
```

---

## IE11: si realmente lo necesitas

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    soporte legacy — pregúntate antes si vale la pena
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Falla</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Fix</text>

  <rect x="10" y="52" width="280" height="24" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">href en &lt;use&gt; no funciona</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">xlink:href="..."</text>

  <rect x="10" y="76" width="280" height="24" fill="#f8fafc"/>
  <text x="20" y="90" font-size="7" fill="#475569">sprite externo no carga</text>
  <text x="170" y="90" font-size="7" fill="#3b82f6">svg4everybody polyfill</text>

  <rect x="10" y="100" width="280" height="24" fill="white"/>
  <text x="20" y="114" font-size="7" fill="#475569">SVG sin width/height</text>
  <text x="170" y="114" font-size="7" fill="#3b82f6">añadir ambos explícitos</text>

  <rect x="10" y="124" width="280" height="24" fill="#f8fafc"/>
  <text x="20" y="138" font-size="7" fill="#475569">Variables CSS (--var)</text>
  <text x="170" y="138" font-size="7" fill="#3b82f6">hardcodear valores</text>

  <rect x="10" y="148" width="280" height="24" fill="white"/>
  <text x="20" y="162" font-size="7" fill="#475569">SMIL no se ejecuta</text>
  <text x="170" y="162" font-size="7" fill="#3b82f6">CSS o JS animations</text>

  <rect x="10" y="172" width="280" height="24" fill="#f8fafc"/>
  <text x="20" y="186" font-size="7" fill="#475569">clip-path CSS limitado</text>
  <text x="170" y="186" font-size="7" fill="#3b82f6">usar atributo SVG</text>

  <rect x="10" y="196" width="280" height="24" fill="white"/>
  <text x="20" y="210" font-size="7" fill="#475569">paint-order ignorado</text>
  <text x="170" y="210" font-size="7" fill="#3b82f6">duplicar elementos</text>

  <rect x="15" y="232" width="270" height="56" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="248" font-size="9" font-weight="bold" fill="#991b1b">Pregunta honesta</text>
  <text x="25" y="264" font-size="7" fill="#475569">¿el 0.3% de usuarios en IE11 justifica ese trabajo?</text>
  <text x="25" y="278" font-size="7" fill="#475569">en la mayoría de casos: no, y cortar soporte es mejor</text>
</svg>
```

---

## Diferencias sutiles a vigilar

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    lo que puede diferir entre evergreens
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Renderizado de texto</text>
  <text x="25" y="70" font-size="7" fill="#475569">font-kerning, text-rendering, anti-aliasing</text>
  <text x="25" y="84" font-size="7" fill="#475569">varían entre navegador y SO — probar siempre</text>

  <rect x="15" y="106" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#166534">Regiones de filtro</text>
  <text x="25" y="138" font-size="7" fill="#475569">x, y, width, height del &lt;filter&gt; se calculan</text>
  <text x="25" y="152" font-size="7" fill="#475569">ligeramente distinto en Safari vs Chrome</text>

  <rect x="15" y="174" width="270" height="60" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#5b21b6">feColorMatrix hueRotate</text>
  <text x="25" y="206" font-size="7" fill="#475569">pequeñas diferencias de matiz entre motores</text>
  <text x="25" y="220" font-size="7" fill="#475569">→ no depender de colores exactos tras filtro</text>

  <rect x="15" y="242" width="270" height="30" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="260" font-size="7" fill="#92400e">regla: test visual en al menos 3 navegadores</text>
</svg>
```

---

## Estrategia de fallback

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    degradación elegante, no "todo o nada"
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// fallback con &lt;picture&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;picture&gt;</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  &lt;source type="image/svg+xml"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">    srcset="logo.svg"&gt;</text>
  <text x="25" y="116" font-size="8" font-family="monospace" fill="#fbbf24">  &lt;img src="logo.png" alt=""&gt;</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/picture&gt;</text>

  <rect x="15" y="146" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">// detección con @supports</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#60a5fa">@supports not (clip-path:</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#60a5fa">   polygon(0 0)) {</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#34d399">  .elemento {</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#fbbf24">    /* fallback visual */</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#34d399">  }</text>
</svg>
```
