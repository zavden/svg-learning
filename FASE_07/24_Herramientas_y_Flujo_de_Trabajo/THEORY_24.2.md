# 24.2 Editores de código

Un editor de código moderno con las extensiones adecuadas te da preview en vivo, IntelliSense de atributos, snippets de Emmet y formateo automático. Es la diferencia entre pelearse con un XML verboso y editar SVG con la misma fluidez que HTML.

---

## VS Code — el entorno por defecto

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    las 4 extensiones imprescindibles
  </text>

  <rect x="15" y="38" width="270" height="48" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">SVG (jock.svg)</text>
  <text x="25" y="70" font-size="7" fill="#475569">preview en vivo, IntelliSense, snippets,</text>
  <text x="25" y="80" font-size="7" fill="#475569">hover docs — la más completa</text>

  <rect x="15" y="94" width="270" height="48" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="110" font-size="9" font-weight="bold" fill="#166534">SVG Preview</text>
  <text x="25" y="126" font-size="7" fill="#475569">panel lateral con preview al guardar,</text>
  <text x="25" y="136" font-size="7" fill="#475569">ligera alternativa si jock.svg es demasiado</text>

  <rect x="15" y="150" width="270" height="48" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#5b21b6">SVGViewer (Simon Siefke)</text>
  <text x="25" y="182" font-size="7" fill="#475569">preview interactivo con zoom y pan,</text>
  <text x="25" y="192" font-size="7" fill="#475569">útil para ilustraciones grandes</text>

  <rect x="15" y="206" width="270" height="48" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="222" font-size="9" font-weight="bold" fill="#92400e">Prettier</text>
  <text x="25" y="238" font-size="7" fill="#475569">formatea SVG como XML al guardar,</text>
  <text x="25" y="248" font-size="7" fill="#475569">no es específico de SVG pero funciona bien</text>
</svg>
```

---

## IntelliSense y hover

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el editor te guía
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="7" font-family="monospace" fill="#94a3b8">// escribir &lt;c abre dropdown</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">&lt;c...</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#fbbf24">    circle</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#fbbf24">    clipPath</text>
  <text x="25" y="110" font-size="7" font-family="monospace" fill="#fbbf24">    cursor</text>
  <text x="25" y="124" font-size="7" fill="#94a3b8">→ autocompletado de elementos válidos</text>

  <rect x="15" y="142" width="270" height="102" fill="#0f172a" rx="3"/>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#94a3b8">// atributos específicos por tipo</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#60a5fa">&lt;circle</text>
  <text x="25" y="188" font-size="7" font-family="monospace" fill="#fbbf24">    cx     cy     r</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#fbbf24">    fill   stroke stroke-width</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#fbbf24">    transform  opacity  class</text>
  <text x="25" y="230" font-size="7" fill="#94a3b8">→ hover sobre un atributo = docs W3C</text>
</svg>
```

---

## Emmet en SVG

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    escribir SVG como HTML
  </text>

  <rect x="15" y="38" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// escribe esto + Tab</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">svg&gt;circle[cx=50 cy=50 r=40]</text>
  <text x="25" y="94" font-size="7" font-family="monospace" fill="#94a3b8">// expande a:</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#34d399">&lt;svg&gt;&lt;circle cx="50" cy="50" r="40"/&gt;&lt;/svg&gt;</text>

  <rect x="15" y="128" width="270" height="120" fill="#0f172a" rx="3"/>
  <text x="25" y="144" font-size="7" font-family="monospace" fill="#94a3b8">// múltiples elementos con multiplicación</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#60a5fa">g&gt;circle[r=5]*5</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#94a3b8">// y atributos con numeración</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#60a5fa">rect[x=$0 y=10 width=20 height=20]*5</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#94a3b8">// los snippets SVG: path, g, rect, line, text</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#fbbf24">→ 10 segundos para crear 5 elementos</text>
</svg>
```

---

## Formato y linting

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    consistencia automática
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// .prettierrc.json</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">{</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  "printWidth": 100,</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  "htmlWhitespaceSensitivity": "strict"</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <rect x="15" y="130" width="270" height="68" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="146" font-size="9" font-weight="bold" fill="#1e40af">Linters que entienden SVG</text>
  <text x="25" y="162" font-size="7" fill="#475569">• HTMLHint (para SVG inline en HTML)</text>
  <text x="25" y="176" font-size="7" fill="#475569">• markuplint (más estricto, reglas de accesibilidad)</text>
  <text x="25" y="190" font-size="7" fill="#475569">• eslint-plugin-jsx-a11y (React SVG)</text>

  <rect x="15" y="208" width="270" height="44" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="224" font-size="9" font-weight="bold" fill="#166534">formatOnSave + lintOnSave</text>
  <text x="25" y="240" font-size="7" fill="#475569">= no hay que pensar en formato, solo escribir</text>
</svg>
```

---

## Otros editores

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    si no usas VS Code
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Editor</text>
  <text x="150" y="46" font-size="7" font-weight="bold" fill="#1e293b">Soporte SVG</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">WebStorm / IntelliJ</text>
  <text x="150" y="66" font-size="7" fill="#10b981">nativo + preview</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Neovim / Vim</text>
  <text x="150" y="88" font-size="7" fill="#f59e0b">sin preview, editing puro</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Sublime Text</text>
  <text x="150" y="110" font-size="7" fill="#f59e0b">highlight XML, sin preview</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Zed</text>
  <text x="150" y="132" font-size="7" fill="#f59e0b">basic, sin extensiones aún</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Notepad++</text>
  <text x="150" y="154" font-size="7" fill="#ef4444">solo syntax highlight</text>

  <text x="150" y="196" text-anchor="middle" font-size="7" fill="#94a3b8">preview es útil, pero un navegador abierto</text>
  <text x="150" y="210" text-anchor="middle" font-size="7" fill="#94a3b8">con el archivo cargado también funciona</text>
  <text x="150" y="224" text-anchor="middle" font-size="7" fill="#94a3b8">(Cmd+R para recargar tras guardar)</text>
</svg>
```
