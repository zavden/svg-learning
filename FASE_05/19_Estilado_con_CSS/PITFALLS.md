# Errores comunes — Estilado de SVG con CSS

---

## 1. Esperar que el atributo de presentación gane al CSS

Es la confusión más frecuente. Vienes de HTML donde un atributo `style="..."` gana a casi todo, así que asumes que `<circle fill="blue">` gana a `circle { fill: red; }`. **No**: en SVG, los atributos de presentación tienen la **menor** prioridad de todas las fuentes de estilo.

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">la regla CSS gana al atributo</text>

  <rect x="15" y="32" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Lo que crees</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;circle fill="blue" class="x"/&gt;</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">.x { fill: red; }</text>
  <text x="25" y="88" font-size="7" fill="#991b1b">"el azul gana porque está más cerca del elemento"</text>
  <text x="25" y="102" font-size="7" fill="#991b1b">→ FALSO: el círculo es ROJO</text>

  <rect x="15" y="120" width="270" height="68" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#166534">✓ La regla real</text>
  <text x="25" y="150" font-size="7" fill="#475569">cualquier regla CSS &gt; cualquier atributo de presentación</text>
  <text x="25" y="164" font-size="7" fill="#475569">para que el atributo gane: usa style="..." inline</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;circle style="fill: blue;" class="x"/&gt;  → azul</text>
</svg>
```

---

## 2. Usar `style="..."` en un SVG que querías tematizar

El editor gráfico (Figma, Illustrator, Sketch) muchas veces exporta los colores como `style="fill: #005ea5;"` en lugar de `fill="#005ea5"`. Resultado: ya no puedes sobrescribir esos colores con CSS porque el inline gana a tu hoja de estilos.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">style inline rompe la tematización</text>

  <rect x="15" y="32" width="270" height="78" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Mal: editor exporta con style</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;path style="fill: #005ea5;"/&gt;</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">/* CSS del documento */</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#475569">.icono path { fill: red; }  /* IGNORADO */</text>
  <text x="25" y="100" font-size="7" fill="#991b1b">solo !important rompe el inline</text>

  <rect x="15" y="118" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="134" font-size="9" font-weight="bold" fill="#166534">✓ Exporta como atributo de presentación</text>
  <text x="25" y="150" font-size="7" fill="#475569">• Figma: "Use presentation attributes"</text>
  <text x="25" y="164" font-size="7" fill="#475569">• SVGO con removeStyleElement deshabilitado</text>
  <text x="25" y="178" font-size="7" fill="#475569">• o haz el reemplazo a mano: style="..." → atributos</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">&lt;path fill="#005ea5"/&gt;  /* sobrescribible */</text>
</svg>
```

---

## 3. Olvidar que `transform-origin` por defecto es `0 0` en SVG

En HTML, `transform: rotate(45deg)` rota desde el centro del elemento. En SVG, rota desde el origen del sistema de coordenadas (`0 0`), lo que hace que el elemento "salga volando". Hay que poner `transform-origin: center` (o las coordenadas absolutas) explícitamente.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">transform-origin en SVG</text>

  <rect x="15" y="32" width="270" height="86" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ "Por qué rota desde la esquina"</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">.elem {</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">  transform: rotate(45deg);</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="102" font-size="7" fill="#991b1b">defecto SVG: transform-origin: 0 0 (no center)</text>

  <rect x="15" y="126" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#166534">✓ Especifica el origen</text>
  <text x="25" y="156" font-size="7" font-family="monospace" fill="#475569">.elem {</text>
  <text x="25" y="168" font-size="7" font-family="monospace" fill="#475569">  transform: rotate(45deg);</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#475569">  transform-origin: 50px 50px;  /* coords del SVG */</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#475569">  /* o "center" si el elemento ocupa todo el SVG */</text>
  <text x="25" y="204" font-size="7" font-family="monospace" fill="#475569">}</text>
</svg>
```

---

## 4. Intentar estilizar el contenido de `<use>` con selectores CSS

Cuando reutilizas un símbolo con `<use href="#icono">`, el contenido vive en un **shadow DOM**. Los selectores CSS del documento no pueden alcanzarlo. Esto rompe la intuición de "todo se estiliza igual con CSS".

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">selectores no entran en &lt;use&gt;</text>

  <rect x="15" y="32" width="270" height="76" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ no aplica nada</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;use href="#icono"/&gt;</text>
  <text x="25" y="76" font-size="7" font-family="monospace" fill="#475569">use circle { fill: red; }</text>
  <text x="25" y="88" font-size="7" font-family="monospace" fill="#475569">use .activo { stroke: blue; }</text>
  <text x="25" y="100" font-size="7" fill="#991b1b">el shadow DOM bloquea los selectores</text>

  <rect x="15" y="116" width="270" height="92" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="132" font-size="9" font-weight="bold" fill="#166534">✓ Tres caminos</text>
  <text x="25" y="146" font-size="7" fill="#475569">1) usa currentColor en el símbolo</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">   &lt;path fill="currentColor"/&gt;</text>
  <text x="25" y="172" font-size="7" fill="#475569">2) usa variables CSS heredadas</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#475569">   &lt;path fill="var(--c1, red)"/&gt;</text>
  <text x="25" y="198" font-size="7" fill="#475569">3) define atributos en el propio &lt;use fill="..."/&gt;</text>
</svg>
```

---

## 5. Confundir `display: none` con `visibility: hidden` en SVG

`display: none` saca al elemento del flujo (no ocupa espacio, no se renderiza). `visibility: hidden` lo oculta visualmente pero **mantiene su espacio** y sus eventos no se disparan. En SVG, donde todo se posiciona por coordenadas, la diferencia es menos obvia que en HTML pero igual de importante.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">display none vs visibility hidden</text>

  <rect x="15" y="32" width="130" height="80" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="80" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#1e40af">display: none</text>
  <text x="22" y="64" font-size="6" fill="#475569">• no renderiza</text>
  <text x="22" y="76" font-size="6" fill="#475569">• no eventos</text>
  <text x="22" y="88" font-size="6" fill="#475569">• no ocupa hit area</text>
  <text x="22" y="100" font-size="6" fill="#475569">• se "borra" del DOM visual</text>

  <rect x="155" y="32" width="130" height="80" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="220" y="48" text-anchor="middle" font-size="8" font-weight="bold" fill="#92400e">visibility: hidden</text>
  <text x="162" y="64" font-size="6" fill="#475569">• no se ve</text>
  <text x="162" y="76" font-size="6" fill="#475569">• ocupa el hit area</text>
  <text x="162" y="88" font-size="6" fill="#475569">• puede recibir clicks</text>
  <text x="162" y="100" font-size="6" fill="#475569">• útil para reservar espacio</text>

  <rect x="15" y="120" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#166534">Cuándo usar cuál</text>
  <text x="25" y="152" font-size="7" fill="#475569">• ocultar definitivamente: display: none</text>
  <text x="25" y="166" font-size="7" fill="#475569">• fade in/out con transition: visibility + opacity</text>
  <text x="25" y="180" font-size="7" fill="#475569">• mantener layout pero esconder: visibility: hidden</text>
  <text x="25" y="194" font-size="7" fill="#475569">• solo lectores de pantalla pero no visual: opacity 0 + aria-hidden</text>
</svg>
```
