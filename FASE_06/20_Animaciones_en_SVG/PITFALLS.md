# PITFALLS — Animaciones en SVG

Los cinco errores más frecuentes al animar SVG. Casi todos vienen de mezclar mal los tres sistemas o de olvidar peculiaridades del modelo temporal de SMIL.

---

## 1. `fill="remove"` cuando querías que el estado final persistiera

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 1 — el valor "regresa" al terminar
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">la animación corre, llega al valor final,</text>
  <text x="25" y="84" font-size="7" fill="#475569">y de golpe vuelve al valor original del atributo</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">parece un "flash" de regreso al final</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">típico en fade-in que acaba desapareciendo otra vez</text>

  <rect x="15" y="126" width="270" height="120" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL (o comportamiento por defecto) */</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#f87171">&lt;animate attributeName="opacity"</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#f87171">  from="0" to="1" dur="1s"/&gt;</text>
  <text x="25" y="182" font-size="7" fill="#94a3b8">→ fill="remove" por defecto → vuelve a 0 al terminar</text>

  <text x="25" y="204" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN */</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#34d399">&lt;animate attributeName="opacity"</text>
  <text x="25" y="230" font-size="8" font-family="monospace" fill="#34d399">  from="0" to="1" dur="1s" fill="freeze"/&gt;</text>
  <text x="25" y="244" font-size="7" fill="#fbbf24">→ fill="freeze" → el valor final se conserva</text>
</svg>
```

---

## 2. Rotar sin especificar el centro

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 2 — "rota en círculos raros"
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">el elemento no gira sobre sí mismo,</text>
  <text x="25" y="84" font-size="7" fill="#475569">sino que orbita alrededor del origen (0,0)</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">ocurre tanto en CSS transform como en SMIL</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">razón: transform-origin por defecto ≠ el centro del elemento</text>

  <rect x="15" y="134" width="270" height="112" fill="#0f172a" rx="3"/>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#94a3b8">/* SMIL — mal */</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#f87171">&lt;animateTransform type="rotate"</text>
  <text x="25" y="176" font-size="8" font-family="monospace" fill="#f87171">  from="0" to="360" dur="2s"/&gt;</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#94a3b8">/* SMIL — bien: rotate acepta "angle cx cy" */</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">from="0 50 50" to="360 50 50"</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#94a3b8">/* CSS — bien */</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#34d399">transform-origin: center;  /* ¡imprescindible! */</text>
</svg>
```

---

## 3. Intentar animar el atributo `d` con CSS

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 3 — CSS no interpola paths
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">defines una transition para `d` y no pasa nada,</text>
  <text x="25" y="84" font-size="7" fill="#475569">o el path cambia de golpe sin transición</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">CSS puede cambiar d (Chrome/Safari), pero NO interpolarlo</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">morphing de paths es territorio de SMIL o JS</text>

  <rect x="15" y="130" width="270" height="116" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">/* MAL */</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#f87171">path { transition: d 0.3s; }</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#94a3b8">→ ni en Chrome ni en Firefox funciona la interpolación</text>

  <text x="25" y="194" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN — SMIL */</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#34d399">&lt;animate attributeName="d" values="..."/&gt;</text>

  <text x="25" y="228" font-size="7" font-family="monospace" fill="#94a3b8">/* BIEN — JS con librería (GSAP, Flubber) */</text>
  <text x="25" y="242" font-size="8" font-family="monospace" fill="#34d399">gsap.to(path, { attr: { d: newD }, duration: 1 });</text>
</svg>
```

---

## 4. Usar SMIL por costumbre cuando CSS bastaría

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#f59e0b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 4 — SMIL donde CSS era más simple
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">el SVG está inline en HTML, es un simple hover,</text>
  <text x="25" y="84" font-size="7" fill="#475569">y hay &lt;animate begin="mouseover"&gt; por todos lados</text>
  <text x="25" y="98" font-size="7" fill="#92400e">resultado: verbosidad, menos legibilidad,</text>
  <text x="25" y="112" font-size="7" fill="#92400e">no aparece en DevTools CSS, ignora prefers-reduced-motion</text>
  <text x="25" y="122" font-size="7" fill="#92400e">y no puedes editarlo con estilos globales</text>

  <rect x="15" y="138" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">/* INNECESARIO (SMIL para un hover) */</text>
  <text x="25" y="168" font-size="7" font-family="monospace" fill="#f87171">&lt;circle fill="blue"&gt;</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#f87171">  &lt;set attributeName="fill" to="red"</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#f87171">       begin="mouseover" end="mouseout"/&gt;</text>
  <text x="25" y="204" font-size="7" font-family="monospace" fill="#f87171">&lt;/circle&gt;</text>

  <text x="25" y="222" font-size="7" font-family="monospace" fill="#94a3b8">/* MEJOR */</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#34d399">circle { transition: fill .2s; }</text>
  <text x="25" y="248" font-size="7" font-family="monospace" fill="#34d399">circle:hover { fill: red; }</text>
</svg>
```

---

## 5. Loops infinitos sin `prefers-reduced-motion`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Pitfall 5 — accesibilidad olvidada
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">spinners, parpadeos o loops corriendo eternamente</text>
  <text x="25" y="84" font-size="7" fill="#475569">sin respetar "reducir movimiento" del SO</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">usuarios con trastornos vestibulares o fotosensibles</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">pueden sufrir mareos o incluso crisis epilépticas</text>
  <text x="25" y="122" font-size="7" fill="#991b1b">→ obligación ética y normativa (WCAG 2.3.3)</text>

  <rect x="15" y="138" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8">/* SIEMPRE envuelve loops en esta media query */</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#60a5fa">@media (prefers-reduced-motion: reduce) {</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#34d399">  .spinner,</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#34d399">  .pulse,</text>
  <text x="25" y="206" font-size="8" font-family="monospace" fill="#34d399">  [class*="animated"] {</text>
  <text x="25" y="218" font-size="8" font-family="monospace" fill="#fbbf24">    animation: none !important;</text>
  <text x="25" y="230" font-size="8" font-family="monospace" fill="#fbbf24">    transition: none !important;</text>
  <text x="25" y="242" font-size="8" font-family="monospace" fill="#60a5fa">  }</text>
</svg>
```

---

## Resumen

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    checklist antes de publicar
  </text>

  <rect x="15" y="38" width="270" height="170" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" fill="#34d399">✓ fill="freeze" si quiero conservar el estado final</text>
  <text x="25" y="78" font-size="8" fill="#34d399">✓ transform-origin: center (CSS) o "cx cy" en rotate (SMIL)</text>
  <text x="25" y="98" font-size="8" fill="#34d399">✓ para morphear d → SMIL o librería JS, nunca CSS</text>
  <text x="25" y="118" font-size="8" fill="#34d399">✓ si el SVG es inline y el efecto es simple → CSS</text>
  <text x="25" y="138" font-size="8" fill="#34d399">✓ todo loop infinito tiene prefers-reduced-motion</text>
  <text x="25" y="158" font-size="8" fill="#34d399">✓ evito SMIL solo por costumbre; elijo por contexto</text>
  <text x="25" y="188" font-size="7" fill="#94a3b8">si dudas entre tres sistemas, revisa 20.9 — la tabla</text>
  <text x="25" y="200" font-size="7" fill="#94a3b8">de decisión te da el sistema correcto caso por caso</text>
</svg>
```
