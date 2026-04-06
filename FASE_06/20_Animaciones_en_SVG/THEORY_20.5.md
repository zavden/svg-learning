# 20.5 `<set>` — cambios discretos sin interpolación

`<set>` es el primo "instantáneo" de `<animate>`. En vez de generar valores intermedios suaves, **salta directamente** al valor final en el momento `begin`. Es la herramienta de SMIL para mostrar/ocultar elementos, cambiar colores de golpe o reaccionar a eventos sin animar.

---

## Diferencia con `<animate>`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    interpolación vs salto
  </text>

  <rect x="15" y="38" width="270" height="86" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">&lt;animate&gt; — interpola</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;animate attributeName="fill"</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#475569">  from="blue" to="red" dur="2s"/&gt;</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ pasa por todos los colores intermedios</text>
  <text x="25" y="112" font-size="7" fill="#475569">→ azul → púrpura → rojo, en 2 segundos</text>

  <rect x="15" y="132" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="148" font-size="9" font-weight="bold" fill="#166534">&lt;set&gt; — salta</text>
  <text x="25" y="164" font-size="7" font-family="monospace" fill="#475569">&lt;set attributeName="fill"</text>
  <text x="25" y="176" font-size="7" font-family="monospace" fill="#475569">  to="red" begin="2s"/&gt;</text>
  <text x="25" y="192" font-size="7" fill="#475569">→ a los 2s, cambia INSTANTÁNEAMENTE a rojo</text>
  <text x="25" y="206" font-size="7" fill="#475569">→ sin valores intermedios, sin transición</text>
</svg>
```

---

## Caso de uso: mostrar/ocultar temporal

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    aparecer entre 2s y 5s
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;rect visibility="hidden"&gt;</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;set</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#fbbf24">    attributeName="visibility"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">    to="visible"</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">    begin="2s"</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#34d399">    dur="3s"</text>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#ec4899">    fill="remove"</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#60a5fa">  /&gt;</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/rect&gt;</text>

  <text x="25" y="194" font-size="7" fill="#94a3b8">/* invisible al inicio, aparece a 2s, desaparece a 5s */</text>
</svg>
```

---

## Caso de uso: reacción al click

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cambiar atributo al click sin JS
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle r="30" fill="blue"&gt;</text>
  <text x="25" y="72" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;set</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#fbbf24">    attributeName="fill"</text>
  <text x="25" y="100" font-size="8" font-family="monospace" fill="#34d399">    to="red"</text>
  <text x="25" y="114" font-size="8" font-family="monospace" fill="#34d399">    begin="click"/&gt;</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/circle&gt;</text>

  <rect x="15" y="146" width="270" height="62" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Por qué importa</text>
  <text x="25" y="178" font-size="7" fill="#475569">• reacción a eventos sin código JavaScript</text>
  <text x="25" y="192" font-size="7" fill="#475569">• funciona en SVG &lt;img&gt; donde JS está bloqueado</text>
  <text x="25" y="204" font-size="7" fill="#475569">• combina con fill="freeze" para cambios permanentes</text>
</svg>
```

---

## `set` vs `animate` vs CSS — cuál elegir

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cuándo usar &lt;set&gt;
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Caso</text>
  <text x="200" y="46" font-size="7" font-weight="bold" fill="#1e293b">Mejor opción</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">Cambio gradual de color</text>
  <text x="200" y="66" font-size="7" font-family="monospace" fill="#3b82f6">&lt;animate&gt;</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Cambio instantáneo en t=2s</text>
  <text x="200" y="88" font-size="7" font-family="monospace" fill="#10b981">&lt;set&gt;</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Mostrar tras delay temporal</text>
  <text x="200" y="110" font-size="7" font-family="monospace" fill="#10b981">&lt;set&gt;</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Hover con interpolación suave</text>
  <text x="200" y="132" font-size="7" font-family="monospace" fill="#8b5cf6">CSS transition</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Click → cambio permanente</text>
  <text x="200" y="154" font-size="7" font-family="monospace" fill="#10b981">&lt;set&gt; freeze</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Cambio que depende de datos</text>
  <text x="200" y="176" font-size="7" font-family="monospace" fill="#ec4899">JavaScript</text>

  <text x="150" y="206" text-anchor="middle" font-size="7" fill="#94a3b8">&lt;set&gt; brilla cuando necesitas un cambio</text>
  <text x="150" y="218" text-anchor="middle" font-size="7" fill="#94a3b8">declarativo, instantáneo y sin JavaScript</text>
</svg>
```
