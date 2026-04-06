# 25.3 Rendimiento

En SVG, rendimiento se traduce a tres cosas concretas: **número de nodos** (DOM), **complejidad de los filtros**, y **qué animas**. Si cuidas esas tres, un SVG moderno se comporta bien. Si las descuidas, ningún truco posterior lo arregla.

---

## Nodos DOM: la métrica clave

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cada elemento cuesta parseo + layout + paint
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Rango</text>
  <text x="150" y="46" font-size="7" font-weight="bold" fill="#1e293b">Estado</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">&lt; 100 nodos</text>
  <text x="150" y="66" font-size="7" fill="#10b981">✓ sin preocupación</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">100 – 1.000</text>
  <text x="150" y="88" font-size="7" fill="#10b981">✓ bien en desktop</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">1.000 – 5.000</text>
  <text x="150" y="110" font-size="7" fill="#f59e0b">⚠ medir, evitar animar</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">&gt; 5.000</text>
  <text x="150" y="132" font-size="7" fill="#ef4444">✗ considerar Canvas/WebGL</text>

  <rect x="15" y="150" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#166534">Cómo bajar nodos</text>
  <text x="25" y="182" font-size="7" fill="#475569">• combinar paths con el mismo estilo: "M... M..."</text>
  <text x="25" y="196" font-size="7" fill="#475569">• &lt;use&gt; en vez de duplicar el mismo path N veces</text>
  <text x="25" y="210" font-size="7" fill="#475569">• eliminar grupos vacíos y grupos de 1 hijo sin attrs</text>
  <text x="25" y="224" font-size="7" fill="#475569">• sprite externo: un fetch, muchos iconos</text>
  <text x="25" y="238" font-size="7" fill="#166534">→ Lighthouse y el profiler lo cuentan</text>
</svg>
```

---

## Filtros: el elemento más caro

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los filtros rasterizan — vector → bitmap
  </text>

  <rect x="15" y="38" width="270" height="86" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Costo real de un filtro</text>
  <text x="25" y="70" font-size="7" fill="#475569">1. rasterizar el elemento (vector → pixels)</text>
  <text x="25" y="84" font-size="7" fill="#475569">2. procesar cada pixel del área del filtro</text>
  <text x="25" y="98" font-size="7" fill="#475569">3. en Retina: 4× píxeles = 4× trabajo</text>
  <text x="25" y="114" font-size="7" fill="#991b1b">→ animar filter = repintar cada frame</text>

  <rect x="15" y="134" width="270" height="76" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#92400e">Los más caros</text>
  <text x="25" y="166" font-size="7" fill="#475569">feGaussianBlur (stdDeviation alto)</text>
  <text x="25" y="180" font-size="7" fill="#475569">feTurbulence (ruido procedural)</text>
  <text x="25" y="194" font-size="7" fill="#475569">feDisplacementMap (distorsión)</text>

  <rect x="15" y="220" width="270" height="48" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="236" font-size="9" font-weight="bold" fill="#166534">Alternativas</text>
  <text x="25" y="252" font-size="7" fill="#475569">• sombra → CSS filter: drop-shadow() (GPU)</text>
  <text x="25" y="264" font-size="7" fill="#475569">• blur → CSS filter: blur() en el contenedor</text>
</svg>
```

---

## Lo que puedes animar barato

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    GPU-aceleradas vs layout-bound
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Baratas (GPU)</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">transform: translate/rotate/scale</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">opacity</text>
  <text x="25" y="104" font-size="7" fill="#475569">→ el compositor reusa la capa sin repintar</text>
  <text x="25" y="118" font-size="7" fill="#475569">→ 60fps estables incluso en mobile</text>
  <text x="25" y="132" font-size="7" fill="#166534">anima estas libremente</text>

  <rect x="15" y="148" width="270" height="124" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="164" font-size="9" font-weight="bold" fill="#991b1b">Caras (fuerzan layout/paint)</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#475569">d (datos del path) → reparsea path</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">fill / stroke → repaint</text>
  <text x="25" y="208" font-size="7" font-family="monospace" fill="#475569">stroke-width → layout + paint</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#475569">x / y / width / height → layout</text>
  <text x="25" y="242" font-size="7" fill="#991b1b">→ evítalas en loop de animación</text>
  <text x="25" y="256" font-size="7" fill="#991b1b">→ si necesitas mover: transform="translate(...)"</text>
</svg>
```

---

## `will-change` con cabeza

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la promesa al compositor tiene coste
  </text>

  <rect x="15" y="38" width="270" height="94" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Qué hace</text>
  <text x="25" y="70" font-size="7" fill="#475569">will-change: transform</text>
  <text x="25" y="84" font-size="7" fill="#475569">promociona el elemento a su propia capa GPU</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ el compositor puede animarlo sin repintar</text>
  <text x="25" y="112" font-size="7" fill="#1e40af">→ memoria GPU adicional por elemento</text>

  <rect x="15" y="144" width="270" height="100" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="160" font-size="9" font-weight="bold" fill="#92400e">Reglas de uso</text>
  <text x="25" y="176" font-size="7" fill="#475569">• activar justo antes de la animación</text>
  <text x="25" y="190" font-size="7" fill="#475569">• desactivar ("auto") cuando termina</text>
  <text x="25" y="204" font-size="7" fill="#475569">• nunca a elementos estáticos "por si acaso"</text>
  <text x="25" y="218" font-size="7" fill="#475569">• nunca a selectores amplios (* , svg *)</text>
  <text x="25" y="234" font-size="7" fill="#92400e">→ cada capa GPU cuesta memoria real</text>
</svg>
```

---

## Lazy-load para SVGs pesados

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    no cargar lo que no se ve todavía
  </text>

  <rect x="15" y="38" width="270" height="72" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// opción 1: &lt;img&gt; con loading nativo</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;img src="mapa.svg"</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  loading="lazy"</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#34d399">  alt="Mapa interactivo" /&gt;</text>

  <rect x="15" y="118" width="270" height="144" fill="#0f172a" rx="3"/>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#94a3b8">// opción 2: Intersection Observer</text>
  <text x="25" y="150" font-size="8" font-family="monospace" fill="#60a5fa">const io = new IntersectionObserver(</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#60a5fa">  entries =&gt; {</text>
  <text x="25" y="182" font-size="8" font-family="monospace" fill="#34d399">    entries.forEach(e =&gt; {</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#fbbf24">      if (e.isIntersecting) {</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#ec4899">        fetch(e.target.dataset.svg)</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#ec4899">         .then(r =&gt; r.text())</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#ec4899">         .then(t =&gt; e.target.innerHTML = t)</text>
  <text x="25" y="254" font-size="8" font-family="monospace" fill="#fbbf24">      }</text>
</svg>
```
