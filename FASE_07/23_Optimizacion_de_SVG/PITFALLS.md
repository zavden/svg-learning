# PITFALLS — Optimización de SVG

Cinco errores recurrentes al optimizar: romper el escalado con `removeViewBox`, perder IDs que JS necesita, animar propiedades caras, aplicar filtros SVG en elementos enormes y olvidar que el servidor comprime (o no comprime) nada. Cada uno con su receta.

---

## 1. Dejar `removeViewBox` activo en SVGO

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    sin viewBox no hay escalado
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">// svgo lo quita si puede dedu-</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">// cirlo de width/height</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">&lt;svg width="24" height="24"&gt;...&lt;/svg&gt;</text>
  <text x="25" y="114" font-size="7" fill="#991b1b">→ con CSS width:100% ya no escala</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">→ se queda a 24×24 fijo</text>

  <rect x="15" y="146" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Bien — desactívalo explícitamente</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">// svgo.config.js</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#475569">preset-default: {</text>
  <text x="25" y="206" font-size="7" font-family="monospace" fill="#475569">  overrides: { removeViewBox: false }</text>
  <text x="25" y="220" font-size="7" font-family="monospace" fill="#475569">}</text>

  <rect x="15" y="242" width="270" height="28" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="260" font-size="7" fill="#92400e">regla: el viewBox es la columna vertebral del SVG</text>
</svg>
```

---

## 2. Dejar `cleanupIds` activo cuando los IDs importan

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVGO renombra o borra IDs externos
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">// en el .svg</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">&lt;circle id="planeta"&gt;  → id="a"</text>
  <text x="25" y="100" font-size="7" font-family="monospace" fill="#475569">// en tu JS</text>
  <text x="25" y="114" font-size="7" font-family="monospace" fill="#475569">document.getElementById('planeta')</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">→ devuelve null tras optimizar</text>

  <rect x="15" y="150" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">overrides: {</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#475569">  cleanupIds: {</text>
  <text x="25" y="210" font-size="7" font-family="monospace" fill="#475569">    preserve: ['planeta', 'luna']</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#475569">  }</text>
  <text x="25" y="238" font-size="7" fill="#166534">// o desactivar entero si dudas</text>

  <rect x="15" y="248" width="270" height="24" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="264" font-size="7" fill="#92400e">regla: los IDs son un API, trátalos como tal</text>
</svg>
```

---

## 3. Animar `width`, `d` o `fill` en bucle

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    propiedades que fuerzan layout
  </text>

  <rect x="15" y="38" width="270" height="98" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">@keyframes crece {</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  from { width: 100px; }</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">  to   { width: 200px; }</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ recálculo de layout en cada frame</text>

  <rect x="15" y="148" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="164" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#475569">@keyframes crece {</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">  from { transform: scaleX(1); }</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#475569">  to   { transform: scaleX(2); }</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="236" font-size="7" fill="#166534">→ solo composición, sin layout ni pintado</text>

  <rect x="15" y="252" width="270" height="20" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="265" font-size="7" fill="#92400e">regla: transform + opacity siempre que sea posible</text>
</svg>
```

---

## 4. Aplicar `feGaussianBlur` en un elemento grande animado

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada frame se rasteriza otra vez
  </text>

  <rect x="15" y="38" width="270" height="102" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;filter id="blur"&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;feGaussianBlur stdDeviation="20"/&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">&lt;/filter&gt;</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">&lt;rect filter="url(#blur)" ... animate /&gt;</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">→ un rect 1000×1000 animado: &lt;10 fps</text>

  <rect x="15" y="148" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="164" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#475569">// CSS filter, no SVG filter</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">.rect { filter: blur(20px); }</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#475569">// o: usa el blur en elementos estáticos</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#475569">// y anima solo transform encima</text>

  <rect x="15" y="248" width="270" height="24" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="264" font-size="7" fill="#92400e">regla: filtros SVG solo en elementos estáticos y pequeños</text>
</svg>
```

---

## 5. Suponer que el servidor comprime automáticamente

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    image/svg+xml no siempre está en la lista
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal — detectarlo tarde</text>
  <text x="25" y="70" font-size="7" fill="#475569">"gzip está activo para HTML y CSS, luego</text>
  <text x="25" y="84" font-size="7" fill="#475569">los SVG también comprimirán"</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ muchos servidores solo listan text/* y</text>
  <text x="25" y="112" font-size="7" fill="#475569">  application/javascript por defecto</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ el SVG viaja sin comprimir</text>

  <rect x="15" y="150" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#166534">Bien — verificar y declarar</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">$ curl -sI -H "Accept-Encoding: gzip" \</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#475569">       https://sitio.com/icon.svg</text>
  <text x="25" y="216" font-size="7" fill="#475569">en nginx: añade image/svg+xml a gzip_types</text>
  <text x="25" y="230" font-size="7" fill="#166534">→ verás el tamaño caer ~70% sin tocar el SVG</text>

  <rect x="15" y="250" width="270" height="22" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="264" font-size="7" fill="#92400e">regla: si no está en DevTools, no está pasando</text>
</svg>
```
