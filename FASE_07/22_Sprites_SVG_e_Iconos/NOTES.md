# NOTES — Sprites SVG e Iconos

Apuntes prácticos sobre el ecosistema de iconos en SVG: compatibilidad, el detalle del shadow tree, cuándo optimizar con SVGO, trade-offs entre inline/sprite/componente, y algunos atajos que ahorran tiempo.

---

## Compatibilidad de navegadores

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    soporte en navegadores modernos
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Feature</text>
  <text x="180" y="46" font-size="7" font-weight="bold" fill="#1e293b">Soporte</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">&lt;symbol&gt; + &lt;use&gt;</text>
  <text x="180" y="66" font-size="7" fill="#10b981">universal (IE11+)</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">href (sin xlink:)</text>
  <text x="180" y="88" font-size="7" fill="#10b981">evergreen, NO IE</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">currentColor</text>
  <text x="180" y="110" font-size="7" fill="#10b981">universal</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">CSS vars en SVG</text>
  <text x="180" y="132" font-size="7" fill="#10b981">evergreen</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">sprite externo + &lt;use&gt;</text>
  <text x="180" y="154" font-size="7" fill="#10b981">universal</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Shadow Parts en &lt;use&gt;</text>
  <text x="180" y="176" font-size="7" fill="#ef4444">no implementado</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">focusable="false"</text>
  <text x="180" y="198" font-size="7" fill="#f59e0b">solo IE/Edge legacy</text>

  <text x="150" y="230" text-anchor="middle" font-size="7" fill="#94a3b8">si necesitas IE11: añade xlink:href además de href</text>
  <text x="150" y="244" text-anchor="middle" font-size="7" fill="#94a3b8">y usa el polyfill svgxuse (en maintenance mode)</text>
</svg>
```

---

## Por qué el CSS no cruza el `<use>`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el shadow tree implícito
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Qué pasa internamente</text>
  <text x="25" y="70" font-size="7" fill="#475569">al encontrar un &lt;use href="#x"&gt;, el navegador clona</text>
  <text x="25" y="84" font-size="7" fill="#475569">el contenido de #x en un subárbol "sombra" que NO</text>
  <text x="25" y="98" font-size="7" fill="#475569">forma parte del DOM consultable. querySelector no</text>
  <text x="25" y="112" font-size="7" fill="#475569">lo ve, los selectores CSS del documento tampoco.</text>
  <text x="25" y="126" font-size="7" fill="#92400e">es el mismo modelo que Web Components internamente</text>

  <rect x="15" y="146" width="270" height="88" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Qué sí traspasa la frontera</text>
  <text x="25" y="178" font-size="7" fill="#475569">propiedades CSS heredables:</text>
  <text x="25" y="192" font-size="7" fill="#475569">  color, fill, stroke, font, visibility, opacity</text>
  <text x="25" y="206" font-size="7" fill="#475569">variables CSS custom properties (--mi-var)</text>
  <text x="25" y="220" font-size="7" fill="#166534">→ por eso currentColor es el mecanismo universal</text>

  <rect x="15" y="242" width="270" height="32" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="258" font-size="8" fill="#991b1b">el resto (clases, nth-child, id específicos)</text>
  <text x="25" y="270" font-size="8" fill="#991b1b">se queda fuera del símbolo</text>
</svg>
```

---

## Inline vs externo vs componente — el pragmático

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    lo que usa la gente de verdad
  </text>

  <rect x="15" y="38" width="270" height="74" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Apps React/Vue/Svelte → componentes</text>
  <text x="25" y="70" font-size="7" fill="#475569">el estándar de 2024+. SVGR + tree-shaking +</text>
  <text x="25" y="84" font-size="7" fill="#475569">autocompletado + TypeScript = ganador claro.</text>
  <text x="25" y="100" font-size="7" fill="#1e40af">cada Heroicon/Lucide/Tabler funciona así</text>

  <rect x="15" y="120" width="270" height="74" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#166534">Sitios estáticos / SSR → sprite externo</text>
  <text x="25" y="152" font-size="7" fill="#475569">Astro, Hugo, Jekyll, 11ty, WordPress…</text>
  <text x="25" y="166" font-size="7" fill="#475569">un sprite.svg cacheable es imbatible</text>
  <text x="25" y="182" font-size="7" fill="#166534">y el HTML queda limpio entre páginas</text>

  <rect x="15" y="202" width="270" height="70" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="218" font-size="9" font-weight="bold" fill="#5b21b6">Prototipo / landing única → inline</text>
  <text x="25" y="234" font-size="7" fill="#475569">no organices nada, pega el &lt;svg&gt; en el HTML</text>
  <text x="25" y="248" font-size="7" fill="#475569">si el proyecto crece, migras después</text>
  <text x="25" y="262" font-size="7" fill="#5b21b6">no hay premios por sobre-arquitecturar</text>
</svg>
```

---

## SVGO: qué optimiza y qué no tocar

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVGO bien configurado
  </text>

  <rect x="15" y="38" width="270" height="106" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Sí optimiza</text>
  <text x="25" y="70" font-size="7" fill="#475569">• elimina metadatos, comentarios, editor info</text>
  <text x="25" y="84" font-size="7" fill="#475569">• redondea decimales a 2-3 cifras</text>
  <text x="25" y="98" font-size="7" fill="#475569">• colapsa grupos innecesarios</text>
  <text x="25" y="112" font-size="7" fill="#475569">• convierte colores a formato más corto (#fff)</text>
  <text x="25" y="126" font-size="7" fill="#475569">• minifica paths (M → m relativos)</text>
  <text x="25" y="140" font-size="7" fill="#166534">→ reducción típica: 40-70% del tamaño original</text>

  <rect x="15" y="152" width="270" height="116" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="168" font-size="9" font-weight="bold" fill="#92400e">Cuidado, desactivar si…</text>
  <text x="25" y="184" font-size="7" fill="#475569">• removeViewBox → NO, romperá el escalado</text>
  <text x="25" y="198" font-size="7" fill="#475569">• removeHiddenElems → cuidado con defs en uso</text>
  <text x="25" y="212" font-size="7" fill="#475569">• cleanupIds → si algún id se referencia por CSS/JS</text>
  <text x="25" y="226" font-size="7" fill="#475569">• convertColors → puede tocar currentColor a veces</text>
  <text x="25" y="240" font-size="7" fill="#475569">• inlineStyles → te fusiona &lt;style&gt; en atributos</text>
  <text x="25" y="256" font-size="7" fill="#92400e">preset-default ya es razonable para la mayoría</text>
</svg>
```

---

## Trampas específicas del sprite externo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    detalles que salen caros tarde
  </text>

  <rect x="15" y="38" width="270" height="50" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#991b1b">1. Content-Type incorrecto</text>
  <text x="25" y="70" font-size="7" fill="#475569">si el servidor sirve el sprite como text/plain</text>
  <text x="25" y="82" font-size="7" fill="#475569">algunos navegadores lo ignoran silenciosamente</text>

  <rect x="15" y="96" width="270" height="50" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="112" font-size="8" font-weight="bold" fill="#991b1b">2. Cache sin versionado</text>
  <text x="25" y="128" font-size="7" fill="#475569">si cacheas sprite.svg con max-age=1año</text>
  <text x="25" y="140" font-size="7" fill="#475569">sin hash en el nombre, los cambios no llegan</text>

  <rect x="15" y="154" width="270" height="50" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="170" font-size="8" font-weight="bold" fill="#991b1b">3. Fetch en paralelo con el HTML</text>
  <text x="25" y="186" font-size="7" fill="#475569">el primer render muestra huecos mientras llega</text>
  <text x="25" y="198" font-size="7" fill="#475569">el sprite. Preload con &lt;link rel="preload"&gt;</text>

  <rect x="15" y="212" width="270" height="40" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="228" font-size="8" font-weight="bold" fill="#991b1b">4. Gzip/Brotli no activado</text>
  <text x="25" y="244" font-size="7" fill="#475569">los SVGs comprimen bien, 50-70% menos típicamente</text>
</svg>
```

---

## El minimal kit del consumidor de iconos

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    estas 4 piezas cubren todo
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>

  <text x="25" y="58" font-size="8" fill="#94a3b8">1) un &lt;symbol&gt; por icono, viewBox uniforme</text>
  <text x="25" y="72" font-size="8" fill="#94a3b8">2) fill="currentColor" en los paths</text>
  <text x="25" y="86" font-size="8" fill="#94a3b8">3) clase .icon con width/height en em</text>
  <text x="25" y="100" font-size="8" fill="#94a3b8">4) wrapper aria-hidden + aria-label en el botón</text>

  <rect x="25" y="116" width="230" height="2" fill="#334155"/>

  <text x="25" y="138" font-size="8" font-family="monospace" fill="#60a5fa">&lt;button aria-label="Cerrar"&gt;</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#34d399">  &lt;svg class="icon" aria-hidden="true"</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#34d399">       focusable="false"&gt;</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#fbbf24">    &lt;use href="#icon-close"/&gt;</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#34d399">  &lt;/svg&gt;</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#60a5fa">&lt;/button&gt;</text>

  <text x="25" y="232" font-size="7" fill="#fbbf24">→ 7 líneas, tematizable, accesible y escalable</text>
</svg>
```

---

## Herramientas del ecosistema

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    conocer los que valen la pena
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Categoría</text>
  <text x="140" y="46" font-size="7" font-weight="bold" fill="#1e293b">Herramientas</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Optimizar</text>
  <text x="140" y="66" font-size="7" font-family="monospace" fill="#3b82f6">SVGO, svgcleaner</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Generar componentes</text>
  <text x="140" y="88" font-size="7" font-family="monospace" fill="#3b82f6">SVGR, svg-to-jsx</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Generar sprite</text>
  <text x="140" y="110" font-size="7" font-family="monospace" fill="#3b82f6">svg-sprite, gulp-svgstore</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Bibliotecas ya hechas</text>
  <text x="140" y="132" font-size="7" font-family="monospace" fill="#3b82f6">Heroicons, Lucide, Tabler</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Framework agnóstico</text>
  <text x="140" y="154" font-size="7" font-family="monospace" fill="#3b82f6">Iconify (50k+ iconos)</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Preview / catálogo</text>
  <text x="140" y="176" font-size="7" font-family="monospace" fill="#3b82f6">Storybook, icones.js.org</text>

  <text x="150" y="210" text-anchor="middle" font-size="7" fill="#94a3b8">antes de crear iconos a mano: revisa si Lucide</text>
  <text x="150" y="222" text-anchor="middle" font-size="7" fill="#94a3b8">o Heroicons ya tienen los que necesitas</text>
  <text x="150" y="244" text-anchor="middle" font-size="7" fill="#94a3b8">Iconify permite usar cualquier set sin instalarlo</text>
</svg>
```
