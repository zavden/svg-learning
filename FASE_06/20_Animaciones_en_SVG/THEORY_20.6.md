# 20.6 Sincronización de animaciones SMIL

SMIL no es solo "animar una propiedad": tiene un **sistema de tiempo** completo donde animaciones pueden esperar a otras, encadenarse, dispararse por eventos del usuario o controlarse desde JavaScript. Es lo que hace SMIL aún competitivo frente a CSS para coreografías declarativas.

---

## El sistema de tiempo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada animación tiene begin/end
  </text>

  <!-- Línea de tiempo -->
  <line x1="30" y1="80" x2="270" y2="80" stroke="#1e293b" stroke-width="1.5"/>
  <line x1="30" y1="76" x2="30" y2="84" stroke="#1e293b" stroke-width="1.5"/>
  <line x1="270" y1="76" x2="270" y2="84" stroke="#1e293b" stroke-width="1.5"/>
  <text x="30" y="98" font-size="6" fill="#475569" text-anchor="middle">0s</text>
  <text x="270" y="98" font-size="6" fill="#475569" text-anchor="middle">5s</text>

  <!-- anim1 -->
  <rect x="30" y="62" width="96" height="10" fill="#3b82f6" rx="2"/>
  <text x="78" y="56" text-anchor="middle" font-size="6" fill="#475569">anim1 (0-2s)</text>

  <!-- anim2 -->
  <rect x="126" y="62" width="48" height="10" fill="#10b981" rx="2"/>
  <text x="150" y="56" text-anchor="middle" font-size="6" fill="#475569">anim2 (anim1.end → +1s)</text>

  <!-- anim3 -->
  <rect x="60" y="46" width="48" height="10" fill="#ec4899" rx="2"/>
  <text x="84" y="40" text-anchor="middle" font-size="6" fill="#475569">anim3 (begin+0.3, dur 1s)</text>

  <rect x="15" y="118" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#94a3b8">/* tres animaciones encadenadas */</text>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#60a5fa">&lt;animate id="anim1" begin="0s"      dur="2s" .../&gt;</text>
  <text x="25" y="166" font-size="7" font-family="monospace" fill="#34d399">&lt;animate id="anim2" begin="anim1.end" dur="1s" .../&gt;</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#ec4899">&lt;animate id="anim3" begin="anim1.begin+0.3s" .../&gt;</text>
  <text x="25" y="202" font-size="7" fill="#94a3b8">anim2 espera a anim1, anim3 se solapa con anim1</text>
  <text x="25" y="216" font-size="7" fill="#94a3b8">la "coreografía" es declarativa, sin JavaScript</text>
</svg>
```

---

## Referencias entre animaciones por id

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    sintaxis de referencias temporales
  </text>

  <rect x="15" y="38" width="270" height="194" fill="#0f172a" rx="3"/>
  <text x="25" y="56" font-size="7" font-family="monospace" fill="#94a3b8">/* el id se usa como prefijo */</text>

  <text x="25" y="76" font-size="8" font-family="monospace" fill="#60a5fa">begin="anim1.begin"</text>
  <text x="25" y="88" font-size="7" font-family="monospace" fill="#94a3b8">  → al mismo tiempo que anim1 empieza</text>

  <text x="25" y="108" font-size="8" font-family="monospace" fill="#60a5fa">begin="anim1.end"</text>
  <text x="25" y="120" font-size="7" font-family="monospace" fill="#94a3b8">  → cuando anim1 termina</text>

  <text x="25" y="140" font-size="8" font-family="monospace" fill="#60a5fa">begin="anim1.end + 0.5s"</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#94a3b8">  → 0.5s después de que anim1 termine</text>

  <text x="25" y="172" font-size="8" font-family="monospace" fill="#60a5fa">begin="anim1.begin - 0.2s"</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#94a3b8">  → 0.2s antes de que anim1 empiece (anticipa)</text>

  <text x="25" y="204" font-size="8" font-family="monospace" fill="#60a5fa">begin="anim1.repeat(2)"</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#94a3b8">  → en la 2ª repetición de anim1</text>
</svg>
```

---

## Eventos como disparadores de `begin`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    eventos como triggers
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animate begin="click" .../&gt;</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#34d399">  &lt;animate begin="mouseover" .../&gt;</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/circle&gt;</text>

  <rect x="15" y="126" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="8" font-family="monospace" fill="#94a3b8">/* combinaciones */</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#fbbf24">begin="click; 5s"</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#94a3b8">  → al click O a los 5s, lo que ocurra primero</text>
  <text x="25" y="190" font-size="8" font-family="monospace" fill="#fbbf24">begin="otroElem.click"</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#94a3b8">  → al hacer click en otro elemento</text>

  <rect x="15" y="216" width="270" height="38" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="232" font-size="7" fill="#475569">eventos comunes: click, mouseover, mouseout, focusin, keypress</text>
  <text x="25" y="244" font-size="7" fill="#166534">interactividad sin JS, ideal para SVG en &lt;img&gt;</text>
</svg>
```

---

## Control desde JavaScript

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SMIL no excluye JavaScript
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">&lt;!-- la animación no arranca sola --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;circle&gt;</text>
  <text x="25" y="86" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;animate id="mover"</text>
  <text x="25" y="102" font-size="8" font-family="monospace" fill="#fbbf24">    begin="indefinite" .../&gt;</text>
  <text x="25" y="118" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/circle&gt;</text>

  <rect x="15" y="146" width="270" height="106" fill="#0f172a" rx="3"/>
  <text x="25" y="162" font-size="8" font-family="monospace" fill="#94a3b8">// arrancar/parar desde JS</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#e2e8f0">const a = document.getElementById('mover');</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">a.beginElement();      // arranca</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#34d399">a.endElement();        // termina</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#34d399">a.getCurrentTime();    // posición temporal</text>
  <text x="25" y="244" font-size="7" fill="#94a3b8">→ útil para arrancar coreografías SMIL desde un botón</text>
</svg>
```
