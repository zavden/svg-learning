# 23.3 Optimización manual

SVGO es automático y resuelve el 90% del trabajo, pero hay cosas que solo un humano puede decidir: cuándo usar `<use>` para repeticiones, cuándo preferir una forma básica a un path, cuándo simplificar un trazado o combinar varios paths iguales. Esta subsección es el "qué haría un humano después de SVGO".

---

## Reducir decimales con criterio

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la precisión "correcta"
  </text>

  <rect x="15" y="38" width="270" height="66" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Demasiada</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">d="M 49.99998328 100.00021345"</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">cx="149.9999837"  r="29.99998"</text>
  <text x="25" y="98" font-size="7" fill="#991b1b">→ bytes innecesarios sin beneficio visual</text>

  <rect x="15" y="112" width="270" height="66" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="128" font-size="9" font-weight="bold" fill="#166534">Justa</text>
  <text x="25" y="144" font-size="7" font-family="monospace" fill="#475569">d="M 50 100"     cx="150"  r="30"</text>
  <text x="25" y="158" font-size="7" fill="#475569">→ iconos y UI: 0-1 decimales basta</text>
  <text x="25" y="172" font-size="7" fill="#166534">→ ilustración técnica: 2-3 decimales máximo</text>

  <rect x="15" y="186" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="202" font-size="9" font-weight="bold" fill="#92400e">Regla de oro</text>
  <text x="25" y="218" font-size="7" fill="#475569">la precisión útil depende del viewBox:</text>
  <text x="25" y="232" font-size="7" fill="#475569">viewBox 24×24 → enteros; viewBox 1000×1000 → 1 dec</text>
</svg>
```

---

## Coordenadas relativas vs absolutas

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    menos cifras = mejor gzip
  </text>

  <rect x="15" y="38" width="270" height="76" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// absolutas (letras mayúsculas)</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#ef4444">d="M 150 200 L 200 250 L 250 200 Z"</text>
  <text x="25" y="86" font-size="7" fill="#94a3b8">números grandes (150, 200, 250…)</text>
  <text x="25" y="100" font-size="7" fill="#991b1b">→ 33 caracteres</text>

  <rect x="15" y="124" width="270" height="86" fill="#0f172a" rx="3"/>
  <text x="25" y="140" font-size="7" font-family="monospace" fill="#94a3b8">// relativas (letras minúsculas)</text>
  <text x="25" y="156" font-size="8" font-family="monospace" fill="#34d399">d="M150 200l50 50l50-50z"</text>
  <text x="25" y="172" font-size="7" fill="#94a3b8">cada comando parte del punto anterior</text>
  <text x="25" y="186" font-size="7" fill="#166534">→ 25 caracteres (-24%)</text>
  <text x="25" y="200" font-size="7" fill="#94a3b8">mejor porque los delta son pequeños</text>

  <rect x="15" y="218" width="270" height="30" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="238" font-size="7" fill="#92400e">SVGO lo hace con convertPathData — raramente a mano</text>
</svg>
```

---

## Eliminar grupos innecesarios

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    aplanar la estructura
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal — jerarquía de editor</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;g id="Layer_1"&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;g id="Group_3"&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">    &lt;g&gt;</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">      &lt;circle cx="50" cy="50" r="30"/&gt;</text>
  <text x="25" y="126" font-size="7" font-family="monospace" fill="#475569">    &lt;/g&gt;&lt;/g&gt;&lt;/g&gt;</text>

  <rect x="15" y="146" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Bien — aplanado</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;circle cx="50" cy="50" r="30"/&gt;</text>
  <text x="25" y="194" font-size="7" fill="#166534">→ los &lt;g&gt; solo deben existir si aportan transform,</text>
  <text x="25" y="204" font-size="7" fill="#166534">   fill, stroke, id útil o agrupación semántica</text>

  <rect x="15" y="214" width="270" height="54" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="230" font-size="9" font-weight="bold" fill="#92400e">Excepción: grupos con semántica</text>
  <text x="25" y="246" font-size="7" fill="#475569">mantén &lt;g id="brazos"&gt; en ilustraciones que</text>
  <text x="25" y="258" font-size="7" fill="#475569">animas por grupo con JavaScript o CSS</text>
</svg>
```

---

## Combinar paths con el mismo estilo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    M M M: sub-trazados en un path
  </text>

  <rect x="15" y="38" width="270" height="80" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// antes — 3 paths con los mismos atributos</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#ef4444">&lt;path fill="red" d="M10 10 L20 20"/&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#ef4444">&lt;path fill="red" d="M30 30 L40 40"/&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#ef4444">&lt;path fill="red" d="M50 50 L60 60"/&gt;</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">→ 3 nodos, 3 veces 'fill="red"'</text>

  <rect x="15" y="126" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="142" font-size="7" font-family="monospace" fill="#94a3b8">// después — un único path con M múltiples</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#34d399">&lt;path fill="red"</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#34d399">      d="M10 10 L20 20</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#34d399">         M30 30 L40 40</text>
  <text x="25" y="200" font-size="7" font-family="monospace" fill="#34d399">         M50 50 L60 60"/&gt;</text>

  <text x="25" y="226" font-size="7" fill="#fbbf24">→ 1 nodo, atributos una sola vez</text>
</svg>
```

---

## Usar `<use>` para elementos repetidos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    definir una vez, instanciar muchas
  </text>

  <rect x="15" y="38" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// antes: 5 círculos casi idénticos</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#ef4444">&lt;circle cx="10" cy="50" r="5" fill="blue"/&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#ef4444">&lt;circle cx="30" cy="50" r="5" fill="blue"/&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#ef4444">&lt;circle cx="50" cy="50" r="5" fill="blue"/&gt;</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#ef4444">&lt;circle cx="70" cy="50" r="5" fill="blue"/&gt;</text>

  <rect x="15" y="130" width="270" height="134" fill="#0f172a" rx="3"/>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#94a3b8">// después: plantilla + instancias</text>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#34d399">&lt;defs&gt;</text>
  <text x="25" y="176" font-size="7" font-family="monospace" fill="#34d399">  &lt;circle id="p" r="5" fill="blue"/&gt;</text>
  <text x="25" y="190" font-size="7" font-family="monospace" fill="#34d399">&lt;/defs&gt;</text>
  <text x="25" y="210" font-size="7" font-family="monospace" fill="#fbbf24">&lt;use href="#p" x="10" y="50"/&gt;</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#fbbf24">&lt;use href="#p" x="30" y="50"/&gt;</text>
  <text x="25" y="238" font-size="7" font-family="monospace" fill="#fbbf24">&lt;use href="#p" x="50" y="50"/&gt;</text>
  <text x="25" y="252" font-size="7" fill="#94a3b8">→ cambiar el color en un solo lugar</text>
</svg>
```

---

## Formas básicas vs paths equivalentes

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    legible &gt; minúsculo en muchos casos
  </text>

  <rect x="15" y="38" width="270" height="74" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Path-como-rectángulo (denso)</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;path d="M10 10 H90 V90 H10 Z" fill="blue"/&gt;</text>
  <text x="25" y="86" font-size="7" fill="#991b1b">→ funciona, pero ilegible y no semántico</text>
  <text x="25" y="100" font-size="7" fill="#991b1b">→ JS no lo encuentra con querySelector('rect')</text>

  <rect x="15" y="120" width="270" height="74" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#166534">Forma básica</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#475569">&lt;rect x="10" y="10" width="80" height="80"</text>
  <text x="25" y="166" font-size="7" font-family="monospace" fill="#475569">      fill="blue"/&gt;</text>
  <text x="25" y="182" font-size="7" fill="#166534">→ legible, mantenible, queryable por JS/CSS</text>

  <rect x="15" y="202" width="270" height="28" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="222" font-size="7" fill="#92400e">si SVGO tiene convertShapeToPath activado, desactívalo</text>
</svg>
```

---

## Simplificar trazados en el editor

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    menos puntos de control
  </text>

  <rect x="15" y="38" width="270" height="86" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Antes — trazado "dibujado"</text>
  <text x="25" y="70" font-size="7" fill="#475569">el vectorizador automático o el trazado a pulso</text>
  <text x="25" y="84" font-size="7" fill="#475569">crea 80 nodos cuando 20 serían suficientes</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ cada nodo = 4-10 bytes extra en el path d</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">una ilustración compleja puede doblar su peso</text>

  <rect x="15" y="132" width="270" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="148" font-size="9" font-weight="bold" fill="#166534">Cómo simplificar</text>
  <text x="25" y="164" font-size="7" fill="#475569">• Inkscape → Path → Simplify (Ctrl+L)</text>
  <text x="25" y="178" font-size="7" fill="#475569">• Illustrator → Object → Path → Simplify</text>
  <text x="25" y="192" font-size="7" fill="#475569">• Figma → Object → Flatten + manual</text>
  <text x="25" y="206" font-size="7" fill="#475569">• vivus.js, picosvg → CLI para simplify</text>
  <text x="25" y="222" font-size="7" fill="#166534">objetivo: el mínimo de puntos que mantiene</text>
  <text x="25" y="232" font-size="7" fill="#166534">la forma reconocible a tamaño real</text>
</svg>
```
