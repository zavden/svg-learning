# 22.4 SVG inline como componentes (React, Vue…)

En apps con React, Vue o Svelte, cada icono se suele definir como un **componente** que retorna un `<svg>` inline. Es el enfoque más popular hoy: props para tamaño y color, tree-shaking automático, y herramientas como SVGR que convierten `.svg` en componentes con un solo comando.

---

## El patrón de componente (React)

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    HomeIcon.jsx
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">export function HomeIcon({</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">  size = 24,</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  color = 'currentColor',</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  ...props</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">}) {</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#fbbf24">  return (</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#ec4899">    &lt;svg width={size} height={size}</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#ec4899">         viewBox="0 0 24 24" fill={color}</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#ec4899">         aria-hidden="true"</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#ec4899">         focusable="false" {...props}&gt;</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">      &lt;path d="M10 20v-6..."/&gt;</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#ec4899">    &lt;/svg&gt;</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#fbbf24">  );</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
</svg>
```

---

## El mismo patrón en Vue

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    HomeIcon.vue
  </text>

  <rect x="15" y="38" width="270" height="230" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">&lt;template&gt;</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;svg :width="size" :height="size"</text>
  <text x="25" y="82" font-size="8" font-family="monospace" fill="#60a5fa">       viewBox="0 0 24 24" :fill="color"&gt;</text>
  <text x="25" y="96" font-size="8" font-family="monospace" fill="#34d399">    &lt;path d="M10 20v-6..."/&gt;</text>
  <text x="25" y="110" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/svg&gt;</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#94a3b8">&lt;/template&gt;</text>

  <text x="25" y="148" font-size="8" font-family="monospace" fill="#94a3b8">&lt;script setup&gt;</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#fbbf24">defineProps({</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#34d399">  size:  { type: Number, default: 24 },</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#34d399">  color: { type: String,</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#34d399">           default: 'currentColor' }</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">});</text>
  <text x="25" y="232" font-size="8" font-family="monospace" fill="#94a3b8">&lt;/script&gt;</text>

  <text x="25" y="254" font-size="7" fill="#94a3b8">misma idea, sintaxis distinta</text>
</svg>
```

---

## Ventajas del enfoque de componente

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    por qué lo usa todo el mundo
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Control y flexibilidad</text>
  <text x="25" y="70" font-size="7" fill="#475569">• props para tamaño, color, variante, clase</text>
  <text x="25" y="84" font-size="7" fill="#475569">• lógica condicional: dos versiones del mismo icono</text>
  <text x="25" y="98" font-size="7" fill="#475569">• eventos del componente (onClick, hover) natural</text>
  <text x="25" y="112" font-size="7" fill="#166534">→ máxima composabilidad con el resto del UI</text>

  <rect x="15" y="126" width="270" height="120" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#1e40af">Herramientas de desarrollo</text>
  <text x="25" y="158" font-size="7" fill="#475569">• autocompletado del nombre del icono en IDE</text>
  <text x="25" y="172" font-size="7" fill="#475569">• tree-shaking: solo entran al bundle los usados</text>
  <text x="25" y="186" font-size="7" fill="#475569">• type safety con TypeScript</text>
  <text x="25" y="200" font-size="7" fill="#475569">• búsqueda trivial: ¿quién usa HomeIcon?</text>
  <text x="25" y="214" font-size="7" fill="#475569">• fácil de testear y mockear</text>
  <text x="25" y="228" font-size="7" fill="#1e40af">→ integración total con el tooling del framework</text>
</svg>
```

---

## SVGR: del .svg al componente automáticamente

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    no escribas los componentes a mano
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">SVGR (React)</text>
  <text x="25" y="70" font-size="7" fill="#475569">convierte archivos .svg en componentes React</text>
  <text x="25" y="84" font-size="7" fill="#475569">optimiza con SVGO, añade accesibilidad, props, ref</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">npx @svgr/cli --out-dir src/icons src/assets</text>
  <text x="25" y="114" font-size="7" fill="#166534">plugins: @svgr/vite, @svgr/webpack</text>

  <rect x="15" y="136" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#94a3b8">// en Vite con @svgr/vite</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#60a5fa">import HomeIcon from './home.svg?react';</text>

  <text x="25" y="190" font-size="7" font-family="monospace" fill="#94a3b8">// se usa directamente como componente</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">&lt;HomeIcon width={32}</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">          className="text-blue-500"/&gt;</text>
  <text x="25" y="238" font-size="7" fill="#fbbf24">→ cero código manual, pipelineado por el bundler</text>
</svg>
```

---

## Alternativas por framework

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el equivalente en cada ecosistema
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Framework</text>
  <text x="130" y="46" font-size="7" font-weight="bold" fill="#1e293b">Herramienta</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">React / Next.js</text>
  <text x="130" y="66" font-size="7" font-family="monospace" fill="#3b82f6">@svgr/webpack, @svgr/vite</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Vue 3</text>
  <text x="130" y="88" font-size="7" font-family="monospace" fill="#3b82f6">vite-svg-loader</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Svelte</text>
  <text x="130" y="110" font-size="7" font-family="monospace" fill="#3b82f6">svelte-preprocess</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Astro</text>
  <text x="130" y="132" font-size="7" font-family="monospace" fill="#3b82f6">astro-icon (nativo)</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Solid.js</text>
  <text x="130" y="154" font-size="7" font-family="monospace" fill="#3b82f6">vite-plugin-solid-svg</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Cualquier bundler</text>
  <text x="130" y="176" font-size="7" font-family="monospace" fill="#3b82f6">SVGO + fs.readFile</text>

  <text x="150" y="216" text-anchor="middle" font-size="7" fill="#94a3b8">o en modo "manual": copias el &lt;svg&gt; en el componente</text>
  <text x="150" y="230" text-anchor="middle" font-size="7" fill="#94a3b8">para iconos contados (&lt;10), es perfectamente viable</text>
</svg>
```
