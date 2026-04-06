# 24.4 Herramientas de desarrollo del navegador

Los SVG inline en HTML son parte del DOM: las DevTools los inspeccionan igual que cualquier otro elemento. Añadir a eso el panel de Animations, la consola con APIs como `getBBox()` o `getTotalLength()`, y el panel de Accessibility de Firefox, y tienes un entorno de debugging completo sin instalar nada.

---

## Inspeccionar SVG en el panel Elements

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el SVG inline es DOM
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Qué puedes hacer</text>
  <text x="25" y="70" font-size="7" fill="#475569">• expandir el árbol: ver cada path, circle, g</text>
  <text x="25" y="84" font-size="7" fill="#475569">• doble click en un atributo: edición en vivo</text>
  <text x="25" y="98" font-size="7" fill="#475569">• los cambios se reflejan al instante</text>
  <text x="25" y="112" font-size="7" fill="#475569">• panel Styles: ver la cascada CSS aplicada</text>

  <rect x="15" y="138" width="270" height="104" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Debugging visual</text>
  <text x="25" y="170" font-size="7" fill="#475569">• hover sobre un elemento en el árbol:</text>
  <text x="25" y="184" font-size="7" fill="#475569">  se resalta con overlay en la página</text>
  <text x="25" y="198" font-size="7" fill="#475569">• el panel Box Model muestra dimensiones</text>
  <text x="25" y="212" font-size="7" fill="#475569">• transformaciones visibles con guías</text>
  <text x="25" y="226" font-size="7" fill="#166534">→ ideal para encontrar el elemento correcto</text>
</svg>
```

---

## Panel de Animations

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    depurar timelines sin console.log
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Cómo abrirlo</text>
  <text x="25" y="70" font-size="7" fill="#475569">Cmd+Shift+P → "Show Animations"</text>
  <text x="25" y="84" font-size="7" fill="#475569">o: ⋮ → More tools → Animations</text>

  <rect x="15" y="106" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#166534">Qué puedes hacer</text>
  <text x="25" y="138" font-size="7" fill="#475569">• capturar animaciones CSS y Web Animations API</text>
  <text x="25" y="152" font-size="7" fill="#475569">• ralentizar a 10% o 25% para ver detalles</text>
  <text x="25" y="166" font-size="7" fill="#475569">• pausar individualmente cada animación</text>
  <text x="25" y="180" font-size="7" fill="#475569">• editar keyframes en tiempo real</text>
  <text x="25" y="194" font-size="7" fill="#166534">→ útil para afinar curvas de easing</text>

  <rect x="15" y="214" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="230" font-size="9" font-weight="bold" fill="#92400e">SMIL: la vía alternativa</text>
  <text x="25" y="246" font-size="7" font-family="monospace" fill="#475569">svgEl.pauseAnimations()</text>
  <text x="25" y="260" font-size="7" font-family="monospace" fill="#475569">svgEl.unpauseAnimations()</text>
</svg>
```

---

## Comandos útiles en la consola

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    inspeccionar SVG desde JS
  </text>

  <rect x="15" y="38" width="270" height="232" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// seleccionar por id</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">const c = document.getElementById('circulo')</text>

  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8">// listar todos los atributos</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#34d399">Array.from(c.attributes).forEach(</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#34d399">  a =&gt; console.log(a.name, a.value))</text>

  <text x="25" y="144" font-size="7" font-family="monospace" fill="#94a3b8">// modificar en vivo</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#fbbf24">c.setAttribute('fill', 'red')</text>

  <text x="25" y="182" font-size="7" font-family="monospace" fill="#94a3b8">// bounding box real</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#ec4899">c.getBBox()</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#94a3b8">// {x, y, width, height}</text>

  <text x="25" y="234" font-size="7" font-family="monospace" fill="#94a3b8">// longitud de un path (para dasharray)</text>
  <text x="25" y="250" font-size="8" font-family="monospace" fill="#60a5fa">document.querySelector('path')</text>
  <text x="25" y="264" font-size="8" font-family="monospace" fill="#60a5fa">  .getTotalLength()</text>
</svg>
```

---

## Firefox DevTools: las ventajas únicas

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Firefox brilla en estas 3 cosas
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#5b21b6">Panel de accesibilidad</text>
  <text x="25" y="70" font-size="7" fill="#475569">muestra el árbol ARIA real del SVG,</text>
  <text x="25" y="84" font-size="7" fill="#475569">incluidos aria-label, role, descendencia</text>

  <rect x="15" y="106" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#1e40af">Inspector de fuentes</text>
  <text x="25" y="138" font-size="7" fill="#475569">en &lt;text&gt; SVG muestra qué fuente se resolvió,</text>
  <text x="25" y="152" font-size="7" fill="#475569">detecta fallbacks y fuentes no cargadas</text>

  <rect x="15" y="174" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#166534">Inspector de filtros</text>
  <text x="25" y="206" font-size="7" fill="#475569">debug visual de filtros SVG complejos,</text>
  <text x="25" y="220" font-size="7" fill="#475569">editor de matriz para feColorMatrix</text>
</svg>
```

---

## Editar atributos en vivo

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    iteración rápida visual
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Flujo típico</text>
  <text x="25" y="70" font-size="7" fill="#475569">1. inspeccionar el elemento SVG</text>
  <text x="25" y="84" font-size="7" fill="#475569">2. doble click en el atributo (ej: stroke-width)</text>
  <text x="25" y="98" font-size="7" fill="#475569">3. probar valores con flechas ↑ ↓</text>
  <text x="25" y="112" font-size="7" fill="#475569">4. cuando encuentres el valor, copiarlo al código</text>
  <text x="25" y="126" font-size="7" fill="#166534">→ experimentación sin reload ni rebuild</text>

  <rect x="15" y="146" width="270" height="102" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#1e40af">Tricks de DevTools</text>
  <text x="25" y="178" font-size="7" fill="#475569">• Shift + ↑/↓: cambia de 10 en 10</text>
  <text x="25" y="192" font-size="7" fill="#475569">• Alt + ↑/↓: cambia de 0.1 en 0.1</text>
  <text x="25" y="206" font-size="7" fill="#475569">• Cmd/Ctrl + ↑/↓: cambia de 100 en 100</text>
  <text x="25" y="220" font-size="7" fill="#475569">• $0 en consola: último elemento seleccionado</text>
  <text x="25" y="234" font-size="7" fill="#1e40af">→ afinar un transform visualmente en segundos</text>
</svg>
```
