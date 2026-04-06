# 23.2 SVGO (SVG Optimizer)

**SVGO** es la herramienta estándar del ecosistema web para optimizar SVGs. Funciona con un sistema de **plugins**: cada plugin hace una tarea concreta (eliminar comentarios, reducir decimales, fusionar paths…) y tú decides cuáles aplicar. Lo ideal es que no tengas que configurar nada: el preset por defecto ya hace el 90% del trabajo.

---

## Cómo instalarlo y usarlo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    de 0 a optimizar en 30 segundos
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8"># global (una vez)</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">$ npm install -g svgo</text>
  <text x="25" y="90" font-size="7" font-family="monospace" fill="#94a3b8"># optimizar un archivo</text>
  <text x="25" y="106" font-size="8" font-family="monospace" fill="#34d399">$ svgo icono.svg</text>
  <text x="25" y="120" font-size="7" font-family="monospace" fill="#fbbf24"># ✔ icono.svg → 4.2 KB - 0.8 KB (-81%)</text>

  <rect x="15" y="138" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8"># salida en otro archivo</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#34d399">$ svgo src.svg -o dst.svg</text>
  <text x="25" y="190" font-size="7" font-family="monospace" fill="#94a3b8"># optimizar una carpeta entera</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">$ svgo -f ./src/icons -o ./dist/icons</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#94a3b8"># con glob para seleccionar</text>
  <text x="25" y="240" font-size="8" font-family="monospace" fill="#34d399">$ svgo *.svg</text>
</svg>
```

---

## El modelo de plugins

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada plugin hace una cosa
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">preset-default</text>
  <text x="25" y="70" font-size="7" fill="#475569">conjunto curado de ~35 plugins que funcionan</text>
  <text x="25" y="84" font-size="7" fill="#475569">bien sin romper nada en el 95% de los casos</text>

  <rect x="15" y="106" width="270" height="120" fill="#0f172a" rx="3"/>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#60a5fa">// svgo.config.js</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#e2e8f0">export default {</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#34d399">  plugins: [</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#fbbf24">    'preset-default',</text>
  <text x="25" y="186" font-size="8" font-family="monospace" fill="#fbbf24">    'sortAttrs',</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#fbbf24">    'removeDimensions'</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#34d399">  ]</text>
</svg>
```

---

## Los plugins que más peso quitan

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los 6 más impactantes
  </text>

  <rect x="15" y="38" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="53" font-size="8" font-weight="bold" fill="#166534">convertPathData</text>
  <text x="25" y="68" font-size="7" fill="#475569">redondea coordenadas, usa relativas, simplifica d</text>

  <rect x="15" y="84" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="99" font-size="8" font-weight="bold" fill="#166534">cleanupNumericValues</text>
  <text x="25" y="114" font-size="7" fill="#475569">49.9999 → 50, 0.50 → .5, ajusta precisión</text>

  <rect x="15" y="130" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="145" font-size="8" font-weight="bold" fill="#166534">collapseGroups</text>
  <text x="25" y="160" font-size="7" fill="#475569">elimina &lt;g&gt; sin atributos, aplana el árbol</text>

  <rect x="15" y="176" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="191" font-size="8" font-weight="bold" fill="#166534">removeEditorsNSData</text>
  <text x="25" y="206" font-size="7" fill="#475569">borra namespaces inkscape:, sodipodi:, illustrator:</text>

  <rect x="15" y="222" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="237" font-size="8" font-weight="bold" fill="#166534">convertColors</text>
  <text x="25" y="252" font-size="7" fill="#475569">#FF0000 → red, #FFFFFF → #fff, rgb → hex</text>
</svg>
```

---

## Plugins potencialmente peligrosos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los que pueden romper cosas
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Plugin</text>
  <text x="150" y="46" font-size="7" font-weight="bold" fill="#1e293b">Rompe si…</text>

  <rect x="10" y="52" width="280" height="32" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">removeViewBox</text>
  <text x="150" y="66" font-size="7" fill="#991b1b">siempre — pierde escalado</text>
  <text x="150" y="78" font-size="7" fill="#475569">desactívalo en preset-default</text>

  <rect x="10" y="84" width="280" height="32" fill="#f8fafc"/>
  <text x="20" y="98" font-size="7" fill="#475569">cleanupIds</text>
  <text x="150" y="98" font-size="7" fill="#991b1b">el SVG tiene &lt;use&gt;, CSS o JS</text>
  <text x="150" y="110" font-size="7" fill="#475569">que apuntan a IDs por nombre</text>

  <rect x="10" y="116" width="280" height="32" fill="white"/>
  <text x="20" y="130" font-size="7" fill="#475569">convertShapeToPath</text>
  <text x="150" y="130" font-size="7" fill="#991b1b">JS busca circle/rect por tipo</text>
  <text x="150" y="142" font-size="7" fill="#475569">— los convierte todos a &lt;path&gt;</text>

  <rect x="10" y="148" width="280" height="32" fill="#f8fafc"/>
  <text x="20" y="162" font-size="7" fill="#475569">inlineStyles</text>
  <text x="150" y="162" font-size="7" fill="#991b1b">hay animaciones CSS en &lt;style&gt;</text>
  <text x="150" y="174" font-size="7" fill="#475569">que dependen de selectores</text>

  <rect x="10" y="180" width="280" height="32" fill="white"/>
  <text x="20" y="194" font-size="7" fill="#475569">removeHiddenElems</text>
  <text x="150" y="194" font-size="7" fill="#991b1b">los ocultos se muestran luego</text>
  <text x="150" y="206" font-size="7" fill="#475569">con JS o CSS (display:none)</text>

  <rect x="10" y="212" width="280" height="32" fill="#f8fafc"/>
  <text x="20" y="226" font-size="7" fill="#475569">mergePaths</text>
  <text x="150" y="226" font-size="7" fill="#991b1b">cada path tiene un id/class</text>
  <text x="150" y="238" font-size="7" fill="#475569">que se usa individualmente</text>

  <text x="150" y="266" text-anchor="middle" font-size="7" fill="#94a3b8">conoce qué hace tu SVG antes de optimizarlo agresivamente</text>
</svg>
```

---

## Configuración típica para un sistema de iconos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    svgo.config.js — preset pragmático
  </text>

  <rect x="15" y="38" width="270" height="228" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#60a5fa">export default {</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#34d399">  multipass: true,</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#34d399">  plugins: [</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#fbbf24">    {</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#fbbf24">      name: 'preset-default',</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#fbbf24">      params: {</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#fbbf24">        overrides: {</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#ec4899">          removeViewBox: false,</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#ec4899">          cleanupIds: false</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#fbbf24">        }</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#fbbf24">      }</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#fbbf24">    },</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#ec4899">    'removeDimensions',</text>
  <text x="25" y="236" font-size="8" font-family="monospace" fill="#ec4899">    'sortAttrs'</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#34d399">  ]</text>
  <text x="25" y="264" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
</svg>
```

---

## Integración en el pipeline

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVGO en tu workflow
  </text>

  <rect x="15" y="38" width="270" height="54" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Pre-commit (lint-staged)</text>
  <text x="25" y="70" font-size="7" fill="#475569">optimiza al hacer commit, el repo queda limpio</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">"*.svg": "svgo"</text>

  <rect x="15" y="100" width="270" height="54" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="116" font-size="9" font-weight="bold" fill="#1e40af">Build step (Vite, Webpack)</text>
  <text x="25" y="132" font-size="7" fill="#475569">@svgr/vite y svgo-loader optimizan en build</text>
  <text x="25" y="146" font-size="7" fill="#475569">el .svg original no se toca en el repo</text>

  <rect x="15" y="162" width="270" height="54" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="178" font-size="9" font-weight="bold" fill="#5b21b6">CI check</text>
  <text x="25" y="194" font-size="7" fill="#475569">falla el build si un SVG nuevo no está optimizado</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#475569">svgo --check</text>

  <text x="150" y="240" text-anchor="middle" font-size="7" fill="#94a3b8">elige uno: automatizar es mejor que recordar</text>
</svg>
```
