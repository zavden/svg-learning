# Reference Shapes — Herramientas y Flujo de Trabajo

Este archivo es una **chuleta operativa**: los comandos, configs y flujos que vas a copiar pegar. Cada bloque resuelve un problema concreto del pipeline SVG.

---

## Setup mínimo de un proyecto con iconos SVG

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    de cero a componente React
  </text>

  <rect x="15" y="38" width="270" height="248" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// 1. instalar dependencias</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">npm i -D svgo vite-plugin-svgr</text>

  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8">// 2. svgo.config.js en raíz</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#34d399">export default {</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#34d399">  plugins: [{</text>
  <text x="25" y="136" font-size="8" font-family="monospace" fill="#fbbf24">    name: 'preset-default',</text>
  <text x="25" y="150" font-size="8" font-family="monospace" fill="#fbbf24">    params: { overrides: {</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#ec4899">      removeViewBox: false,</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#ec4899">      cleanupIds: false</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#fbbf24">    } }</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">  }]</text>
  <text x="25" y="220" font-size="8" font-family="monospace" fill="#34d399">}</text>

  <text x="25" y="242" font-size="7" font-family="monospace" fill="#94a3b8">// 3. vite.config.ts → añadir svgr()</text>
  <text x="25" y="258" font-size="7" font-family="monospace" fill="#94a3b8">// 4. import Close from 'icons/close.svg?react'</text>
  <text x="25" y="274" font-size="7" font-family="monospace" fill="#94a3b8">// listo: tree-shaking + optimización</text>
</svg>
```

---

## Comandos SVGO frecuentes

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cheat sheet CLI
  </text>

  <rect x="15" y="38" width="270" height="232" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// un archivo in-place</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">svgo icon.svg</text>

  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8">// carpeta a otra carpeta</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#60a5fa">svgo -f src/svg -o public/svg</text>

  <text x="25" y="130" font-size="7" font-family="monospace" fill="#94a3b8">// recursivo</text>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#60a5fa">svgo -rf src/ -o dist/</text>

  <text x="25" y="168" font-size="7" font-family="monospace" fill="#94a3b8">// con config custom</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#60a5fa">svgo --config=svgo.dev.js icon.svg</text>

  <text x="25" y="206" font-size="7" font-family="monospace" fill="#94a3b8">// pipe desde stdin</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#60a5fa">cat icon.svg | svgo -i - -o -</text>

  <text x="25" y="244" font-size="7" font-family="monospace" fill="#94a3b8">// desactivar plugin puntualmente</text>
  <text x="25" y="260" font-size="8" font-family="monospace" fill="#60a5fa">svgo --disable=removeViewBox a.svg</text>
</svg>
```

---

## VS Code settings.json para SVG

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    config que hace SVG editable en paz
  </text>

  <rect x="15" y="38" width="270" height="232" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// .vscode/settings.json</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">{</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  "[svg]": {</text>
  <text x="25" y="102" font-size="7" font-family="monospace" fill="#fbbf24">    "editor.defaultFormatter":</text>
  <text x="25" y="116" font-size="7" font-family="monospace" fill="#fbbf24">      "esbenp.prettier-vscode",</text>
  <text x="25" y="132" font-size="7" font-family="monospace" fill="#fbbf24">    "editor.formatOnSave": true,</text>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#fbbf24">    "editor.wordWrap": "on"</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#34d399">  },</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#fbbf24">  "emmet.includeLanguages": {</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#fbbf24">    "svg": "xml"</text>
  <text x="25" y="210" font-size="7" font-family="monospace" fill="#fbbf24">  },</text>
  <text x="25" y="230" font-size="7" font-family="monospace" fill="#fbbf24">  "svg.preview.mode": "svg",</text>
  <text x="25" y="246" font-size="7" font-family="monospace" fill="#fbbf24">  "files.associations":</text>
  <text x="25" y="260" font-size="7" font-family="monospace" fill="#fbbf24">    { "*.svg": "svg" }</text>
</svg>
```

---

## Snippets de consola útiles

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    diagnóstico rápido desde DevTools
  </text>

  <rect x="15" y="38" width="270" height="232" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// contar elementos SVG en la página</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">document.querySelectorAll('svg *').length</text>

  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8">// todos los paths con su longitud</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#34d399">[...document.querySelectorAll('path')]</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#34d399"> .map(p =&gt; p.getTotalLength())</text>

  <text x="25" y="144" font-size="7" font-family="monospace" fill="#94a3b8">// bbox del elemento seleccionado</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#fbbf24">$0.getBBox()</text>

  <text x="25" y="182" font-size="7" font-family="monospace" fill="#94a3b8">// pausar todas las animaciones SMIL</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#ec4899">document.querySelectorAll('svg')</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#ec4899"> .forEach(s =&gt; s.pauseAnimations())</text>

  <text x="25" y="234" font-size="7" font-family="monospace" fill="#94a3b8">// ver el SVG outerHTML del seleccionado</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#60a5fa">copy($0.outerHTML)</text>
  <text x="25" y="264" font-size="7" fill="#94a3b8">// → copiado al portapapeles</text>
</svg>
```

---

## Tabla: cuándo qué librería

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    decisión en 30 segundos
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Tarea</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Herramienta</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Gráfico de barras con datos</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">D3.js</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Animar 10 elementos en timeline</text>
  <text x="170" y="88" font-size="7" fill="#3b82f6">GSAP</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Animación simple en landing</text>
  <text x="170" y="110" font-size="7" fill="#3b82f6">Anime.js o CSS</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Logo que se dibuja solo</text>
  <text x="170" y="132" font-size="7" fill="#3b82f6">Vivus.js</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Onboarding animado AE</text>
  <text x="170" y="154" font-size="7" fill="#3b82f6">Lottie</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Crear SVG dinámico sin datos</text>
  <text x="170" y="176" font-size="7" fill="#3b82f6">SVG.js</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">App de dibujo interactiva</text>
  <text x="170" y="198" font-size="7" fill="#3b82f6">Paper.js (Canvas)</text>

  <rect x="10" y="206" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="220" font-size="7" fill="#475569">Icono estático en React</text>
  <text x="170" y="220" font-size="7" fill="#3b82f6">SVGR (nada más)</text>

  <rect x="10" y="228" width="280" height="22" fill="white"/>
  <text x="20" y="242" font-size="7" fill="#475569">Ilustración fija de hero</text>
  <text x="170" y="242" font-size="7" fill="#3b82f6">Figma → SVGO → &lt;img&gt;</text>

  <text x="150" y="268" text-anchor="middle" font-size="7" fill="#94a3b8">→ cada fila es una decisión tomada, no una opción</text>
</svg>
```

---

## Pre-commit hook con husky + lint-staged

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    optimizar SVG automáticamente al commitear
  </text>

  <rect x="15" y="38" width="270" height="232" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// 1. instalar</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">npm i -D husky lint-staged svgo</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#60a5fa">npx husky init</text>

  <text x="25" y="108" font-size="7" font-family="monospace" fill="#94a3b8">// 2. package.json</text>
  <text x="25" y="124" font-size="8" font-family="monospace" fill="#34d399">"lint-staged": {</text>
  <text x="25" y="138" font-size="8" font-family="monospace" fill="#fbbf24">  "*.svg": "svgo"</text>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#34d399">}</text>

  <text x="25" y="174" font-size="7" font-family="monospace" fill="#94a3b8">// 3. .husky/pre-commit</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#60a5fa">npx lint-staged</text>

  <text x="25" y="212" font-size="7" font-family="monospace" fill="#94a3b8">// resultado: cada SVG commiteado</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#94a3b8">// pasa por SVGO antes de entrar al repo</text>
  <text x="25" y="246" font-size="7" fill="#ec4899">→ imposible commitear SVG sucio</text>
  <text x="25" y="262" font-size="7" fill="#94a3b8">→ CI puede verificar con svgo --check</text>
</svg>
```

---

## Checklist: flujo de trabajo sano

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    si todo esto es ✓, el flujo es sólido
  </text>

  <text x="20" y="50" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="50" font-size="7" fill="#475569">svgo.config.js versionado en el repo</text>

  <text x="20" y="70" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="70" font-size="7" fill="#475569">SVGO corre en pre-commit (husky + lint-staged)</text>

  <text x="20" y="90" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="90" font-size="7" fill="#475569">CI verifica que ningún SVG sucio entra a main</text>

  <text x="20" y="110" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="110" font-size="7" fill="#475569">el bundler tiene una regla clara por tipo de SVG</text>

  <text x="20" y="130" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="130" font-size="7" fill="#475569">los iconos se importan individualmente (tree-shaking)</text>

  <text x="20" y="150" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="150" font-size="7" fill="#475569">el equipo sabe cuándo editor vs código</text>

  <text x="20" y="170" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="170" font-size="7" fill="#475569">bundle analyzer en CI para catch regresiones</text>

  <text x="20" y="190" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="190" font-size="7" fill="#475569">librerías pesadas (D3, Lottie) solo donde hacen falta</text>

  <text x="20" y="210" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="210" font-size="7" fill="#475569">las modificaciones viven en el pipeline, no en el .svg</text>

  <text x="20" y="230" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="230" font-size="7" fill="#475569">DevTools panel Animations conocido por el equipo</text>

  <text x="20" y="250" font-size="8" fill="#10b981">✓</text>
  <text x="35" y="250" font-size="7" fill="#475569">SVGOMG y SVG Path Visualizer en bookmarks del equipo</text>

  <rect x="15" y="262" width="270" height="30" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="150" y="282" text-anchor="middle" font-size="8" fill="#166534">nadie pelea con SVGs sucios, todo es reproducible</text>
</svg>
```
