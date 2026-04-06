# 24.6 Integración con build tools

El SVG deja de ser un archivo suelto cuando entra en un proyecto moderno: Vite, Webpack, esbuild, Rollup. Cada bundler tiene su forma de importar SVG — como URL, como string, como componente, como sprite. La decisión correcta depende del caso de uso, no del bundler.

---

## Vite — 4 formas de importar un SVG

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el mismo .svg, 4 importaciones distintas
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// 1. como URL (default)</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">import logoUrl from './logo.svg'</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#94a3b8">// '/assets/logo-abc123.svg'</text>

  <rect x="15" y="106" width="270" height="60" fill="#0f172a" rx="3"/>
  <text x="25" y="122" font-size="7" font-family="monospace" fill="#94a3b8">// 2. como string raw</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#34d399">import logo from './logo.svg?raw'</text>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">// '&lt;svg&gt;...&lt;/svg&gt;'</text>

  <rect x="15" y="174" width="270" height="60" fill="#0f172a" rx="3"/>
  <text x="25" y="190" font-size="7" font-family="monospace" fill="#94a3b8">// 3. como componente (vite-plugin-svgr)</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#fbbf24">import Logo from './logo.svg?react'</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#94a3b8">// &lt;Logo className="size-8" /&gt;</text>

  <rect x="15" y="242" width="270" height="48" fill="#0f172a" rx="3"/>
  <text x="25" y="258" font-size="7" font-family="monospace" fill="#94a3b8">// 4. como URL inline (data URI)</text>
  <text x="25" y="274" font-size="8" font-family="monospace" fill="#ec4899">import u from './logo.svg?inline'</text>
</svg>
```

---

## vite-plugin-svgr para React

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG → componente React tipado
  </text>

  <rect x="15" y="38" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// vite.config.ts</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">import svgr from 'vite-plugin-svgr'</text>
  <text x="25" y="90" font-size="8" font-family="monospace" fill="#34d399">export default defineConfig({</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#34d399">  plugins: [svgr(), react()]</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#34d399">})</text>
  <text x="25" y="138" font-size="7" fill="#94a3b8">→ los .svg pasan por SVGO + @svgr/core</text>

  <rect x="15" y="154" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="170" font-size="9" font-weight="bold" fill="#166534">Ventajas del enfoque</text>
  <text x="25" y="186" font-size="7" fill="#475569">• el color se hereda con currentColor</text>
  <text x="25" y="200" font-size="7" fill="#475569">• el tamaño con className o props</text>
  <text x="25" y="214" font-size="7" fill="#475569">• tree-shaking: solo los iconos usados</text>
  <text x="25" y="228" font-size="7" fill="#166534">→ ideal para sistemas de iconos</text>

  <rect x="15" y="242" width="270" height="30" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="260" font-size="7" fill="#475569">añadir svgo.config.js para limpiar en build</text>
</svg>
```

---

## Webpack — @svgr/webpack y asset modules

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    webpack.config.js — 3 reglas
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// @svgr/webpack</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">{</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  test: /\.svg$/,</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  use: ['@svgr/webpack']</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <rect x="15" y="130" width="270" height="66" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">// svg-inline-loader</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#60a5fa">{</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#34d399">  test: /\.svg$/,</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#34d399">  loader: 'svg-inline-loader'</text>

  <rect x="15" y="204" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="220" font-size="7" font-family="monospace" fill="#94a3b8">// asset modules (Webpack 5)</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#60a5fa">{</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#34d399">  test: /\.svg$/,</text>
  <text x="25" y="264" font-size="8" font-family="monospace" fill="#ec4899">  type: 'asset/resource'</text>
  <text x="25" y="280" font-size="7" fill="#94a3b8">// o 'asset/inline' para data URI</text>
</svg>
```

---

## SVGO en el pipeline de build

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    2 maneras de meter SVGO al build
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// 1. npm script (CLI)</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">{</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  "scripts": {</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#fbbf24">    "opt": "svgo -f src/svg -o public",</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#fbbf24">    "build": "npm run opt &amp;&amp; vite build"</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#34d399">  }</text>

  <rect x="15" y="138" width="270" height="134" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">// 2. API programática (Node)</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#60a5fa">import { optimize } from 'svgo'</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#34d399">const raw = readFileSync('in.svg', 'utf8')</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">const { data } = optimize(raw, {</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#34d399">  plugins: [</text>
  <text x="25" y="238" font-size="7" font-family="monospace" fill="#fbbf24">   { name: 'preset-default',</text>
  <text x="25" y="252" font-size="7" font-family="monospace" fill="#fbbf24">     params: { overrides: {...} } }</text>
  <text x="25" y="264" font-size="8" font-family="monospace" fill="#34d399">  ] })</text>
</svg>
```

---

## Decisión: ¿qué enfoque usar?

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el caso de uso manda
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Enfoque</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Cuándo</text>

  <rect x="10" y="52" width="280" height="32" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">URL (src='logo.svg')</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">logos, imágenes decorativas,</text>
  <text x="170" y="76" font-size="7" fill="#3b82f6">sin necesidad de estilar</text>

  <rect x="10" y="84" width="280" height="32" fill="#f8fafc"/>
  <text x="20" y="98" font-size="7" fill="#475569">Inline en HTML</text>
  <text x="170" y="98" font-size="7" fill="#3b82f6">CSS del documento afecta al SVG,</text>
  <text x="170" y="108" font-size="7" fill="#3b82f6">animación, interactividad JS</text>

  <rect x="10" y="116" width="280" height="32" fill="white"/>
  <text x="20" y="130" font-size="7" fill="#475569">Componente SVGR</text>
  <text x="170" y="130" font-size="7" fill="#3b82f6">iconos React/Vue, tree-shaking,</text>
  <text x="170" y="140" font-size="7" fill="#3b82f6">props (color, size)</text>

  <rect x="10" y="148" width="280" height="32" fill="#f8fafc"/>
  <text x="20" y="162" font-size="7" fill="#475569">Sprite externo &lt;use&gt;</text>
  <text x="170" y="162" font-size="7" fill="#3b82f6">muchos iconos compartidos,</text>
  <text x="170" y="172" font-size="7" fill="#3b82f6">caching agresivo del sprite</text>

  <rect x="10" y="180" width="280" height="32" fill="white"/>
  <text x="20" y="194" font-size="7" fill="#475569">data URI en CSS</text>
  <text x="170" y="194" font-size="7" fill="#3b82f6">backgrounds, bullets, patrones</text>
  <text x="170" y="204" font-size="7" fill="#3b82f6">pequeños en hojas de estilo</text>

  <rect x="15" y="224" width="270" height="68" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="240" font-size="9" font-weight="bold" fill="#166534">Regla práctica</text>
  <text x="25" y="256" font-size="7" fill="#475569">• 1 icono único → componente SVGR</text>
  <text x="25" y="270" font-size="7" fill="#475569">• 20+ iconos → sprite + &lt;use&gt;</text>
  <text x="25" y="284" font-size="7" fill="#475569">• imagen compleja → URL estático</text>
</svg>
```
