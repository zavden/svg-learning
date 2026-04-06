# 23.5 Rendimiento de renderizado

Un SVG pequeño en bytes puede ser **lento en pantalla** si abusa de filtros caros, anima propiedades equivocadas o construye miles de nodos en el DOM. Optimizar el peso del archivo es solo la mitad del trabajo: la otra mitad es lo que el navegador hace con él al pintarlo. Esta subsección es sobre frames por segundo, no sobre kilobytes.

---

## El costo de los filtros

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    filtros = rasterización pixel a pixel
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Los caros</text>
  <text x="25" y="70" font-size="7" fill="#475569">• feGaussianBlur con stdDeviation grande</text>
  <text x="25" y="84" font-size="7" fill="#475569">• feTurbulence (ruido procedural)</text>
  <text x="25" y="98" font-size="7" fill="#475569">• feDisplacementMap</text>
  <text x="25" y="112" font-size="7" fill="#475569">• feConvolveMatrix</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">cada pixel del área se recalcula en cada frame</text>

  <rect x="15" y="146" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Alternativas CSS</text>
  <text x="25" y="178" font-size="7" fill="#475569">filter: blur(8px)       → acelerado por GPU</text>
  <text x="25" y="192" font-size="7" fill="#475569">filter: drop-shadow()   → más barato que feBlur</text>
  <text x="25" y="206" font-size="7" fill="#475569">backdrop-filter         → nativo del compositor</text>
  <text x="25" y="220" font-size="7" fill="#166534">→ el navegador puede hacerlos en la GPU</text>

  <rect x="15" y="234" width="270" height="40" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="250" font-size="8" font-weight="bold" fill="#92400e">Regla práctica</text>
  <text x="25" y="264" font-size="7" fill="#475569">no animes elementos grandes con filtros SVG internos</text>
</svg>
```

---

## Animar solo `transform` y `opacity`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    las dos propiedades mágicas
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Sí acelera GPU</text>
  <text x="25" y="70" font-size="7" fill="#475569">transform (translate, rotate, scale, skew)</text>
  <text x="25" y="84" font-size="7" fill="#475569">opacity</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ el navegador crea una capa de composición</text>
  <text x="25" y="112" font-size="7" fill="#166534">→ cada frame: solo desplazar/alfa, no re-pintar</text>

  <rect x="15" y="138" width="270" height="106" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#991b1b">Fuerza layout / repaint</text>
  <text x="25" y="170" font-size="7" fill="#475569">d (animar el path)     → reconstruye la geometría</text>
  <text x="25" y="184" font-size="7" fill="#475569">width, height, x, y    → recalcula el layout</text>
  <text x="25" y="198" font-size="7" fill="#475569">fill, stroke           → repinta el área</text>
  <text x="25" y="212" font-size="7" fill="#475569">stroke-width, stroke-dasharray → repinta</text>
  <text x="25" y="226" font-size="7" fill="#991b1b">en muchos elementos: jank visible</text>
  <text x="25" y="240" font-size="7" fill="#991b1b">en pocos: imperceptible</text>
</svg>
```

---

## `will-change`: avisar al navegador

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    promocionar a capa propia
  </text>

  <rect x="15" y="38" width="270" height="76" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">/* antes de que empiece la animación */</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">.animando {</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#34d399">  will-change: transform, opacity;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">}</text>

  <rect x="15" y="124" width="270" height="122" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="140" font-size="9" font-weight="bold" fill="#92400e">Reglas de uso</text>
  <text x="25" y="156" font-size="7" fill="#475569">• solo activarlo cuando la animación empieza</text>
  <text x="25" y="170" font-size="7" fill="#475569">• quitarlo cuando termina (libera GPU)</text>
  <text x="25" y="184" font-size="7" fill="#475569">• NO ponerlo "por si acaso" en muchos elementos</text>
  <text x="25" y="198" font-size="7" fill="#475569">• cada capa consume memoria de video</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#475569">el.style.willChange = 'transform';</text>
  <text x="25" y="226" font-size="7" font-family="monospace" fill="#475569">// animar...</text>
  <text x="25" y="240" font-size="7" font-family="monospace" fill="#475569">el.style.willChange = 'auto';</text>
</svg>
```

---

## El número de nodos del DOM

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el DOM tiene un límite práctico
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">&lt; 1000 nodos</text>
  <text x="25" y="70" font-size="7" fill="#475569">rendimiento fluido en cualquier navegador</text>
  <text x="25" y="84" font-size="7" fill="#475569">interactividad nativa, accesibilidad natural</text>

  <rect x="15" y="106" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#92400e">1000 - 5000 nodos</text>
  <text x="25" y="138" font-size="7" fill="#475569">parseo lento, scroll puede vibrar</text>
  <text x="25" y="152" font-size="7" fill="#475569">evaluar CSS selectors se vuelve costoso</text>

  <rect x="15" y="174" width="270" height="60" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#991b1b">&gt; 5000 nodos</text>
  <text x="25" y="206" font-size="7" fill="#475569">jank al interactuar, tiempo de parseo notable</text>
  <text x="25" y="220" font-size="7" fill="#475569">pinta inicial visible en móviles modestos</text>

  <rect x="15" y="242" width="270" height="30" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="260" font-size="7" fill="#1e40af">→ si necesitas más: es momento de mirar canvas</text>
</svg>
```

---

## Canvas vs SVG: cuándo migrar

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dos herramientas, dos casos
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Escenario</text>
  <text x="160" y="46" font-size="7" font-weight="bold" fill="#1e293b">SVG</text>
  <text x="220" y="46" font-size="7" font-weight="bold" fill="#1e293b">Canvas</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">UI icons, charts (&lt;1k)</text>
  <text x="160" y="66" font-size="7" fill="#10b981">✓ mejor</text>
  <text x="220" y="66" font-size="7" fill="#ef4444">✗</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">scatter plot 10k+ puntos</text>
  <text x="160" y="88" font-size="7" fill="#ef4444">✗ lento</text>
  <text x="220" y="88" font-size="7" fill="#10b981">✓</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">interactivo por elemento</text>
  <text x="160" y="110" font-size="7" fill="#10b981">✓ DOM events</text>
  <text x="220" y="110" font-size="7" fill="#f59e0b">hit test</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">responsive</text>
  <text x="160" y="132" font-size="7" fill="#10b981">✓ viewBox</text>
  <text x="220" y="132" font-size="7" fill="#f59e0b">redibujar</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">accesibilidad</text>
  <text x="160" y="154" font-size="7" fill="#10b981">✓ nativa</text>
  <text x="220" y="154" font-size="7" fill="#ef4444">manual</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">animación masiva</text>
  <text x="160" y="176" font-size="7" fill="#f59e0b">depende</text>
  <text x="220" y="176" font-size="7" fill="#10b981">✓ RAF</text>

  <rect x="10" y="184" width="280" height="22" fill="white"/>
  <text x="20" y="198" font-size="7" fill="#475569">export/SSR</text>
  <text x="160" y="198" font-size="7" fill="#10b981">✓ texto</text>
  <text x="220" y="198" font-size="7" fill="#ef4444">bitmap</text>

  <text x="150" y="234" text-anchor="middle" font-size="7" fill="#94a3b8">el punto de inflexión suele estar en torno</text>
  <text x="150" y="248" text-anchor="middle" font-size="7" fill="#94a3b8">a 1000-2000 elementos animados o interactivos</text>
  <text x="150" y="262" text-anchor="middle" font-size="7" fill="#94a3b8">híbrido: SVG para UI, canvas para datos densos</text>
</svg>
```

---

## Checklist de rendimiento

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    antes de culpar al navegador
  </text>

  <text x="25" y="50" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="50" font-size="7" fill="#475569">¿animas solo transform y opacity?</text>

  <text x="25" y="70" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="70" font-size="7" fill="#475569">¿los filtros están en elementos estáticos?</text>

  <text x="25" y="90" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="90" font-size="7" fill="#475569">¿will-change solo durante la animación?</text>

  <text x="25" y="110" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="110" font-size="7" fill="#475569">¿menos de 1000 nodos SVG en pantalla?</text>

  <text x="25" y="130" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="130" font-size="7" fill="#475569">¿usas requestAnimationFrame en vez de setInterval?</text>

  <text x="25" y="150" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="150" font-size="7" fill="#475569">¿calculas fuera del loop lo que no cambia por frame?</text>

  <text x="25" y="170" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="170" font-size="7" fill="#475569">¿prefieres CSS animation a JS cuando puedes?</text>

  <text x="25" y="190" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="190" font-size="7" fill="#475569">¿DevTools Performance muestra &gt; 60fps?</text>

  <text x="25" y="210" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="210" font-size="7" fill="#475569">¿probado en móvil, no solo en desktop?</text>

  <text x="25" y="230" font-size="8" fill="#10b981">✓</text>
  <text x="40" y="230" font-size="7" fill="#475569">¿respetas prefers-reduced-motion?</text>

  <text x="150" y="260" text-anchor="middle" font-size="7" fill="#94a3b8">medir &gt; suponer — DevTools &gt; intuición</text>
</svg>
```
