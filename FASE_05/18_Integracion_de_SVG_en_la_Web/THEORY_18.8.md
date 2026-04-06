# 18.8 Comparativa de métodos de integración

Siete formas de incluir un SVG, siete perfiles de capacidades distintos. Esta sección es la **guía de decisión**: dado un caso de uso, qué método elegir y por qué.

---

## Tabla maestra

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">capacidades de cada método</text>

  <!-- Header -->
  <rect x="5" y="26" width="290" height="24" fill="#e2e8f0"/>
  <text x="15" y="41" font-size="7" font-weight="bold" fill="#1e293b">Método</text>
  <text x="90" y="41" font-size="6" font-weight="bold" fill="#1e293b">CSS ext</text>
  <text x="125" y="41" font-size="6" font-weight="bold" fill="#1e293b">JS ext</text>
  <text x="158" y="41" font-size="6" font-weight="bold" fill="#1e293b">Scripts</text>
  <text x="196" y="41" font-size="6" font-weight="bold" fill="#1e293b">Cache</text>
  <text x="228" y="41" font-size="6" font-weight="bold" fill="#1e293b">A11y</text>
  <text x="258" y="41" font-size="6" font-weight="bold" fill="#1e293b">Fallb.</text>

  <!-- Inline -->
  <rect x="5" y="50" width="290" height="22" fill="#dcfce7"/>
  <text x="15" y="64" font-size="7" font-family="monospace" fill="#1e293b">inline</text>
  <text x="95" y="64" font-size="8" fill="#10b981">✓</text>
  <text x="130" y="64" font-size="8" fill="#10b981">✓</text>
  <text x="165" y="64" font-size="8" fill="#10b981">✓</text>
  <text x="200" y="64" font-size="8" fill="#ef4444">✗</text>
  <text x="233" y="64" font-size="8" fill="#10b981">✓</text>
  <text x="263" y="64" font-size="8" fill="#ef4444">✗</text>

  <!-- img -->
  <rect x="5" y="72" width="290" height="22" fill="white"/>
  <text x="15" y="86" font-size="7" font-family="monospace" fill="#1e293b">&lt;img&gt;</text>
  <text x="95" y="86" font-size="8" fill="#ef4444">✗</text>
  <text x="130" y="86" font-size="8" fill="#ef4444">✗</text>
  <text x="165" y="86" font-size="8" fill="#ef4444">✗</text>
  <text x="200" y="86" font-size="8" fill="#10b981">✓</text>
  <text x="233" y="86" font-size="8" fill="#10b981">alt</text>
  <text x="263" y="86" font-size="8" fill="#10b981">alt</text>

  <!-- CSS bg -->
  <rect x="5" y="94" width="290" height="22" fill="#f8fafc"/>
  <text x="15" y="108" font-size="7" font-family="monospace" fill="#1e293b">CSS bg</text>
  <text x="95" y="108" font-size="8" fill="#ef4444">✗</text>
  <text x="130" y="108" font-size="8" fill="#ef4444">✗</text>
  <text x="165" y="108" font-size="8" fill="#ef4444">✗</text>
  <text x="200" y="108" font-size="8" fill="#10b981">✓</text>
  <text x="233" y="108" font-size="8" fill="#ef4444">✗</text>
  <text x="263" y="108" font-size="8" fill="#ef4444">✗</text>

  <!-- object -->
  <rect x="5" y="116" width="290" height="22" fill="white"/>
  <text x="15" y="130" font-size="7" font-family="monospace" fill="#1e293b">&lt;object&gt;</text>
  <text x="95" y="130" font-size="8" fill="#ef4444">✗</text>
  <text x="130" y="130" font-size="6" fill="#f59e0b">getSVG</text>
  <text x="165" y="130" font-size="8" fill="#10b981">✓</text>
  <text x="200" y="130" font-size="8" fill="#10b981">✓</text>
  <text x="233" y="130" font-size="8" fill="#10b981">✓</text>
  <text x="263" y="130" font-size="8" fill="#10b981">✓</text>

  <!-- embed -->
  <rect x="5" y="138" width="290" height="22" fill="#f8fafc"/>
  <text x="15" y="152" font-size="7" font-family="monospace" fill="#1e293b">&lt;embed&gt;</text>
  <text x="95" y="152" font-size="8" fill="#ef4444">✗</text>
  <text x="130" y="152" font-size="6" fill="#f59e0b">limit.</text>
  <text x="165" y="152" font-size="8" fill="#10b981">✓</text>
  <text x="200" y="152" font-size="8" fill="#10b981">✓</text>
  <text x="233" y="152" font-size="8" fill="#ef4444">✗</text>
  <text x="263" y="152" font-size="8" fill="#ef4444">✗</text>

  <!-- iframe -->
  <rect x="5" y="160" width="290" height="22" fill="white"/>
  <text x="15" y="174" font-size="7" font-family="monospace" fill="#1e293b">&lt;iframe&gt;</text>
  <text x="95" y="174" font-size="8" fill="#ef4444">✗</text>
  <text x="130" y="174" font-size="6" fill="#f59e0b">postMsg</text>
  <text x="165" y="174" font-size="8" fill="#10b981">✓</text>
  <text x="200" y="174" font-size="8" fill="#10b981">✓</text>
  <text x="233" y="174" font-size="6" fill="#f59e0b">title</text>
  <text x="263" y="174" font-size="7" fill="#10b981">HTML</text>

  <!-- data URI -->
  <rect x="5" y="182" width="290" height="22" fill="#f8fafc"/>
  <text x="15" y="196" font-size="7" font-family="monospace" fill="#1e293b">data URI</text>
  <text x="95" y="196" font-size="8" fill="#ef4444">✗</text>
  <text x="130" y="196" font-size="8" fill="#ef4444">✗</text>
  <text x="165" y="196" font-size="8" fill="#ef4444">✗</text>
  <text x="200" y="196" font-size="8" fill="#ef4444">✗</text>
  <text x="233" y="196" font-size="8" fill="#10b981">alt</text>
  <text x="263" y="196" font-size="8" fill="#10b981">alt</text>

  <!-- Leyenda -->
  <rect x="5" y="212" width="290" height="82" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="15" y="226" font-size="7" font-weight="bold" fill="#92400e">Leyenda:</text>
  <text x="15" y="238" font-size="7" fill="#475569">CSS ext: el CSS del documento padre aplica al interior del SVG</text>
  <text x="15" y="250" font-size="7" fill="#475569">JS ext: JavaScript externo puede manipular elementos SVG</text>
  <text x="15" y="262" font-size="7" fill="#475569">Scripts: los &lt;script&gt; dentro del SVG se ejecutan</text>
  <text x="15" y="274" font-size="7" fill="#475569">Cache: el SVG se cachea como recurso independiente</text>
  <text x="15" y="286" font-size="7" fill="#475569">A11y: nivel de accesibilidad / Fallb: contenido alternativo</text>
</svg>
```

---

## Guía de decisión rápida

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dado mi caso, qué uso
  </text>

  <!-- Caso 1 -->
  <rect x="15" y="38" width="270" height="36" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="53" font-size="8" font-weight="bold" fill="#166534">Manipular SVG con JS / CSS externo</text>
  <text x="25" y="66" font-size="7" font-family="monospace" fill="#475569">→ SVG inline</text>

  <!-- Caso 2 -->
  <rect x="15" y="78" width="270" height="36" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="93" font-size="8" font-weight="bold" fill="#1e40af">Logo/ícono reutilizado en muchas páginas</text>
  <text x="25" y="106" font-size="7" font-family="monospace" fill="#475569">→ &lt;img src="logo.svg" alt="..."&gt;</text>

  <!-- Caso 3 -->
  <rect x="15" y="118" width="270" height="36" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="133" font-size="8" font-weight="bold" fill="#5b21b6">Patrón decorativo de fondo</text>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#475569">→ CSS background-image</text>

  <!-- Caso 4 -->
  <rect x="15" y="158" width="270" height="36" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="173" font-size="8" font-weight="bold" fill="#92400e">SVG con scripts y necesito fallback PNG</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#475569">→ &lt;object&gt; con &lt;img&gt; interno</text>

  <!-- Caso 5 -->
  <rect x="15" y="198" width="270" height="36" fill="#fce7f3" stroke="#ec4899" rx="3"/>
  <text x="25" y="213" font-size="8" font-weight="bold" fill="#9d174d">Ícono &lt; 1 KB que cambia por contexto</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#475569">→ data URI en CSS (URL-encoded)</text>

  <!-- Caso 6 -->
  <rect x="15" y="238" width="270" height="36" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="253" font-size="8" font-weight="bold" fill="#991b1b">Editor SVG embebido de otro origen</text>
  <text x="25" y="266" font-size="7" font-family="monospace" fill="#475569">→ &lt;iframe&gt; con postMessage</text>
</svg>
```

---

## Por qué inline gana la mayoría de veces

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    inline como default moderno
  </text>

  <rect x="15" y="38" width="270" height="120" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Ventajas únicas de inline</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• máximo control con CSS y JavaScript</text>
  <text x="25" y="84" font-size="7" fill="#1e293b">• sin petición HTTP adicional</text>
  <text x="25" y="98" font-size="7" fill="#1e293b">• mejor integración con el flujo del documento</text>
  <text x="25" y="112" font-size="7" fill="#1e293b">• accesibilidad predecible con lectores de pantalla</text>
  <text x="25" y="126" font-size="7" fill="#1e293b">• hereda variables CSS y reglas del documento</text>
  <text x="25" y="140" font-size="7" fill="#1e293b">• eventos nativos sobre elementos individuales</text>

  <rect x="15" y="168" width="270" height="80" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="184" font-size="9" font-weight="bold" fill="#1e40af">Las desventajas se mitigan con build tools</text>
  <text x="25" y="200" font-size="7" fill="#1e293b">• archivos SVG separados en desarrollo (cacheables)</text>
  <text x="25" y="214" font-size="7" fill="#1e293b">• inlining automático en el bundle final</text>
  <text x="25" y="228" font-size="7" fill="#1e293b">• el resultado: lo mejor de ambos mundos</text>
  <text x="25" y="240" font-size="7" fill="#1e40af">(Vite, webpack, SvelteKit, Next.js, Astro...)</text>
</svg>
```

---

## "Lo mejor de los dos mundos" con build tools

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    archivos separados → inline automático
  </text>

  <!-- Flujo -->
  <rect x="15" y="38" width="85" height="70" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="57" y="55" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">Desarrollo</text>
  <text x="57" y="72" text-anchor="middle" font-size="7" fill="#475569">icon.svg</text>
  <text x="57" y="84" text-anchor="middle" font-size="7" fill="#475569">(archivo</text>
  <text x="57" y="94" text-anchor="middle" font-size="7" fill="#475569">separado)</text>

  <!-- Flecha -->
  <path d="M 108 73 L 130 73" stroke="#64748b" stroke-width="2" fill="none"/>
  <polygon points="130,73 124,69 124,77" fill="#64748b"/>

  <!-- Build -->
  <rect x="135" y="38" width="85" height="70" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="177" y="55" text-anchor="middle" font-size="8" font-weight="bold" fill="#92400e">Build tool</text>
  <text x="177" y="72" text-anchor="middle" font-size="7" fill="#475569">Vite, webpack,</text>
  <text x="177" y="84" text-anchor="middle" font-size="7" fill="#475569">SvelteKit...</text>
  <text x="177" y="96" text-anchor="middle" font-size="7" fill="#475569">inlining</text>

  <!-- Flecha -->
  <path d="M 228 73 L 250 73" stroke="#64748b" stroke-width="2" fill="none"/>
  <polygon points="250,73 244,69 244,77" fill="#64748b"/>

  <!-- Producción -->
  <rect x="255" y="38" width="40" height="70" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="275" y="55" text-anchor="middle" font-size="7" font-weight="bold" fill="#166534">HTML</text>
  <text x="275" y="72" text-anchor="middle" font-size="6" fill="#475599">&lt;svg&gt;</text>
  <text x="275" y="82" text-anchor="middle" font-size="6" fill="#475599">...</text>
  <text x="275" y="92" text-anchor="middle" font-size="6" fill="#475599">&lt;/svg&gt;</text>

  <!-- Ejemplos -->
  <rect x="15" y="120" width="270" height="128" fill="#0f172a" rx="3"/>
  <text x="25" y="136" font-size="7" font-family="monospace" fill="#94a3b8">// Vite + vite-plugin-svgr</text>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#e2e8f0">import Icon from './icon.svg?react';</text>
  <text x="25" y="160" font-size="7" font-family="monospace" fill="#34d399">&lt;Icon className="w-6 h-6"/&gt;</text>

  <text x="25" y="180" font-size="7" font-family="monospace" fill="#94a3b8">// webpack + svg-inline-loader</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#e2e8f0">import svg from '!svg-inline-loader!./icon.svg';</text>

  <text x="25" y="212" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- Astro --&gt;</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#e2e8f0">import Icon from './icon.svg';</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#34d399">&lt;Icon/&gt;</text>
</svg>
```
