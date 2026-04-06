# REFERENCE_SHAPES — Sprites SVG e Iconos

Chuleta del sistema de iconos: esqueleto del sprite, consumo con `<use>`, CSS mínimo, componente React, tabla de decisión y checklist de revisión antes de lanzar.

---

## Esqueleto del sprite

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    sprite.svg — el patrón base
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg xmlns="http://www.w3.org/2000/svg"</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">     style="display:none"&gt;</text>

  <text x="25" y="90" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-home"</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#fbbf24">          viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#34d399">    &lt;path fill="currentColor"</text>
  <text x="25" y="132" font-size="8" font-family="monospace" fill="#34d399">          d="M3 11l9-8 9 8v10..."/&gt;</text>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/symbol&gt;</text>

  <text x="25" y="168" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-close"</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#fbbf24">          viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">    &lt;path fill="currentColor" d="..."/&gt;</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;/symbol&gt;</text>

  <text x="25" y="232" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>
</svg>
```

---

## Consumo en HTML

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los dos patrones típicos
  </text>

  <rect x="15" y="38" width="270" height="72" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- con texto visible (decorativo) --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;button&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  &lt;svg class="icon" aria-hidden="true"</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">       focusable="false"&gt;&lt;use href="#icon-home"/&gt;&lt;/svg&gt; Inicio</text>

  <rect x="15" y="118" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- sin texto visible (con significado) --&gt;</text>
  <text x="25" y="150" font-size="8" font-family="monospace" fill="#60a5fa">&lt;button aria-label="Cerrar diálogo"&gt;</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#34d399">  &lt;svg class="icon" aria-hidden="true"</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#34d399">       focusable="false"&gt;</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">    &lt;use href="#icon-close"/&gt;</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">  &lt;/svg&gt;</text>
</svg>
```

---

## CSS mínimo para el sistema

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    icons.css — base + tamaños
  </text>

  <rect x="15" y="38" width="270" height="190" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">.icon {</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">  width: 1.25em;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  height: 1.25em;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  fill: currentColor;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">  vertical-align: -0.15em;</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#34d399">  flex-shrink: 0;</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <text x="25" y="160" font-size="8" font-family="monospace" fill="#fbbf24">.icon-sm { width: 16px; height: 16px; }</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#fbbf24">.icon-md { width: 24px; height: 24px; }</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#fbbf24">.icon-lg { width: 32px; height: 32px; }</text>
  <text x="25" y="202" font-size="8" font-family="monospace" fill="#fbbf24">.icon-xl { width: 48px; height: 48px; }</text>

  <text x="25" y="222" font-size="7" fill="#94a3b8">→ con 5 clases cubres el 95% de los usos</text>
</svg>
```

---

## Componente React tipo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Icon.jsx — wrapper reusable
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#60a5fa">export function Icon({</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#34d399">  name,</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  size = 24,</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  label,</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#34d399">  ...props</text>
  <text x="25" y="126" font-size="8" font-family="monospace" fill="#60a5fa">}) {</text>
  <text x="25" y="140" font-size="8" font-family="monospace" fill="#fbbf24">  return (</text>
  <text x="25" y="154" font-size="8" font-family="monospace" fill="#ec4899">    &lt;svg width={size} height={size}</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#ec4899">         fill="currentColor"</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#ec4899">         aria-hidden={!label}</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#ec4899">         aria-label={label}</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#ec4899">         focusable="false" {...props}&gt;</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#34d399">      &lt;use href={`#icon-${name}`}/&gt;</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#ec4899">    &lt;/svg&gt;</text>
</svg>
```

---

## Tabla de decisión rápida

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué patrón elegir
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Escenario</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Recomendación</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">SPA con React/Vue</text>
  <text x="170" y="66" font-size="7" fill="#3b82f6">componente + SVGR</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">sitio multi-página</text>
  <text x="170" y="88" font-size="7" fill="#3b82f6">sprite externo + &lt;use&gt;</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">menos de 10 iconos</text>
  <text x="170" y="110" font-size="7" fill="#3b82f6">inline directo</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">catalog &gt;100 iconos</text>
  <text x="170" y="132" font-size="7" fill="#3b82f6">sprite + tree-shake</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">iconos animados</text>
  <text x="170" y="154" font-size="7" fill="#3b82f6">inline (no &lt;use&gt;)</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">iconos bicolor/temáticos</text>
  <text x="170" y="176" font-size="7" fill="#3b82f6">inline + CSS vars</text>

  <text x="150" y="210" text-anchor="middle" font-size="7" fill="#94a3b8">si dudas entre inline y sprite: empieza inline,</text>
  <text x="150" y="222" text-anchor="middle" font-size="7" fill="#94a3b8">migra a sprite cuando el bundle te duela</text>
</svg>
```

---

## Checklist antes de dar por hecho el sistema

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    revisa antes de merge
  </text>

  <text x="25" y="50" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="50" font-size="8" fill="#475569">todos los iconos comparten el mismo viewBox</text>

  <text x="25" y="72" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="72" font-size="8" fill="#475569">convención de nombres consistente (icon-*, ic-*)</text>

  <text x="25" y="94" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="94" font-size="8" fill="#475569">fill="currentColor" por defecto (o variable CSS)</text>

  <text x="25" y="116" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="116" font-size="8" fill="#475569">nada de width/height hardcoded en el &lt;symbol&gt;</text>

  <text x="25" y="138" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="138" font-size="8" fill="#475569">SVGs optimizados con SVGO (paths mínimos)</text>

  <text x="25" y="160" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="160" font-size="8" fill="#475569">aria-hidden+focusable="false" en el &lt;svg&gt; wrapper</text>

  <text x="25" y="182" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="182" font-size="8" fill="#475569">aria-label en el botón cuando no hay texto visible</text>

  <text x="25" y="204" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="204" font-size="8" fill="#475569">sprite externo servido con CORS y cache-control</text>

  <text x="25" y="226" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="226" font-size="8" fill="#475569">catálogo visual publicado y actualizado</text>

  <text x="25" y="248" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="248" font-size="8" fill="#475569">política de contribución documentada</text>

  <text x="25" y="270" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="270" font-size="8" fill="#475569">changelog con iconos añadidos/renombrados/deprecated</text>
</svg>
```
