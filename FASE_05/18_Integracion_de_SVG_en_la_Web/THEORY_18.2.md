# 18.2 SVG como imagen con `<img>`

La forma más simple y familiar de incluir un SVG: tratarlo como una imagen más dentro de la etiqueta `<img>`. El navegador lo carga como archivo externo y lo renderiza. Sencillo, cacheable... pero con **limitaciones claras** en manipulación.

---

## Sintaxis mínima

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG como imagen
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- como cualquier imagen --&gt;</text>
  <text x="25" y="74" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;img</text>
  <text x="35" y="88" font-size="8" font-family="monospace" fill="#60a5fa">  src="icono.svg"</text>
  <text x="35" y="100" font-size="8" font-family="monospace" fill="#fbbf24">  alt="Descripción del icono"</text>
  <text x="35" y="112" font-size="8" font-family="monospace" fill="#34d399">  width="50"</text>
  <text x="35" y="124" font-size="8" font-family="monospace" fill="#34d399">  height="50"&gt;</text>
</svg>
```

---

## Lo que funciona como imagen normal

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    compatible con todo el ecosistema de imágenes
  </text>

  <!-- Lazy loading -->
  <rect x="15" y="38" width="270" height="48" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#1e40af">Lazy loading</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;img src="foto.svg" loading="lazy" alt="..."&gt;</text>
  <text x="25" y="80" font-size="7" fill="#64748b">el SVG se carga solo cuando entra en viewport</text>

  <!-- srcset -->
  <rect x="15" y="94" width="270" height="48" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="110" font-size="8" font-weight="bold" fill="#166534">srcset / picture</text>
  <text x="25" y="124" font-size="7" font-family="monospace" fill="#475569">&lt;img src="logo.svg" srcset="logo-2x.svg 2x"&gt;</text>
  <text x="25" y="136" font-size="7" fill="#64748b">cambio entre variantes según resolución</text>

  <!-- Cache -->
  <rect x="15" y="150" width="270" height="48" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="166" font-size="8" font-weight="bold" fill="#5b21b6">Cache HTTP</text>
  <text x="25" y="180" font-size="7" fill="#475569">el archivo .svg se cachea por el navegador</text>
  <text x="25" y="192" font-size="7" fill="#64748b">usado en N páginas → una sola descarga</text>

  <!-- Fallback -->
  <rect x="15" y="206" width="270" height="48" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="222" font-size="8" font-weight="bold" fill="#92400e">Fallback de alt</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#475569">alt="Diagrama de ventas"</text>
  <text x="25" y="248" font-size="7" fill="#64748b">si el SVG no carga, aparece el alt como texto</text>
</svg>
```

---

## El sandbox: lo que NO funciona

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG en &lt;img&gt; está "sandboxed"
  </text>

  <rect x="15" y="38" width="270" height="210" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ Aislado del documento padre</text>

  <text x="25" y="72" font-size="7" font-weight="bold" fill="#1e293b">CSS externo:</text>
  <text x="35" y="84" font-size="7" font-family="monospace" fill="#475569">img.icono path { fill: red; }  /* no funciona */</text>
  <text x="35" y="95" font-size="7" fill="#64748b">las reglas CSS del padre no alcanzan el interior del SVG</text>

  <text x="25" y="114" font-size="7" font-weight="bold" fill="#1e293b">JavaScript externo:</text>
  <text x="35" y="126" font-size="7" font-family="monospace" fill="#475569">document.querySelector('img circle')  // null</text>
  <text x="35" y="137" font-size="7" fill="#64748b">el DOM interno del SVG es invisible desde afuera</text>

  <text x="25" y="156" font-size="7" font-weight="bold" fill="#1e293b">Scripts dentro del SVG:</text>
  <text x="35" y="168" font-size="7" fill="#475569">&lt;svg&gt;&lt;script&gt;...&lt;/script&gt;&lt;/svg&gt;</text>
  <text x="35" y="179" font-size="7" fill="#64748b">los &lt;script&gt; internos NO se ejecutan (seguridad)</text>

  <text x="25" y="198" font-size="7" font-weight="bold" fill="#1e293b">Eventos:</text>
  <text x="35" y="210" font-size="7" fill="#475569">click en un path concreto → el evento va al &lt;img&gt;</text>
  <text x="35" y="221" font-size="7" fill="#64748b">no se puede saber en qué parte del SVG hiciste click</text>

  <text x="25" y="240" font-size="7" fill="#10b981">✓ Las animaciones SMIL internas sí funcionan</text>
</svg>
```

---

## El truco: cambiar color con `filter`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    colorear un SVG en &lt;img&gt; sin editarlo
  </text>

  <!-- Original -->
  <text x="55" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">original</text>
  <rect x="25" y="55" width="60" height="60" fill="white" stroke="#cbd5e1"/>
  <path d="M 40 95 L 50 75 L 60 85 L 70 65" stroke="#1e293b" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>

  <!-- Hue-rotate -->
  <text x="150" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">hue-rotate</text>
  <rect x="120" y="55" width="60" height="60" fill="white" stroke="#cbd5e1"/>
  <path d="M 135 95 L 145 75 L 155 85 L 165 65" stroke="#3b82f6" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>

  <!-- Invert -->
  <text x="245" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e293b">invert</text>
  <rect x="215" y="55" width="60" height="60" fill="#1e293b"/>
  <path d="M 230 95 L 240 75 L 250 85 L 260 65" stroke="white" stroke-width="3" fill="none" stroke-linecap="round" stroke-linejoin="round"/>

  <!-- Código -->
  <rect x="15" y="130" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="8" font-family="monospace" fill="#94a3b8">/* aproximaciones con filter */</text>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#e2e8f0">img.icono {</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#60a5fa">  filter: hue-rotate(200deg) saturate(2);</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
  <text x="25" y="204" font-size="8" font-family="monospace" fill="#94a3b8">/* también: brightness, contrast, sepia, invert */</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">  filter: invert(1) brightness(1.2);</text>
</svg>
```

---

## Cuándo usar `<img>` vs. inline

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;img&gt; es perfecto para...
  </text>

  <!-- Casos ideales -->
  <rect x="15" y="38" width="270" height="120" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ Úsalo para</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• logotipos reutilizados en muchas páginas</text>
  <text x="25" y="84" font-size="7" fill="#1e293b">• ilustraciones decorativas sin interactividad</text>
  <text x="25" y="98" font-size="7" fill="#1e293b">• hero images y banners grandes</text>
  <text x="25" y="112" font-size="7" fill="#1e293b">• assets CDN que deben cachearse</text>
  <text x="25" y="126" font-size="7" fill="#1e293b">• contenido generado por usuarios (con sandboxing)</text>
  <text x="25" y="140" font-size="7" fill="#1e293b">• animaciones SMIL puras sin control externo</text>

  <!-- Casos no ideales -->
  <rect x="15" y="168" width="270" height="92" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="184" font-size="9" font-weight="bold" fill="#991b1b">✗ No lo uses para</text>
  <text x="25" y="200" font-size="7" fill="#1e293b">• íconos que deben cambiar de color dinámicamente</text>
  <text x="25" y="214" font-size="7" fill="#1e293b">• gráficos interactivos con eventos por elemento</text>
  <text x="25" y="228" font-size="7" fill="#1e293b">• SVGs que dependen de CSS del documento</text>
  <text x="25" y="242" font-size="7" fill="#1e293b">• diagramas manipulados con JavaScript</text>
</svg>
```
