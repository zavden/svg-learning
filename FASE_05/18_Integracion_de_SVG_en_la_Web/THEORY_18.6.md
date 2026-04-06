# 18.6 SVG con `<iframe>`

Un `<iframe>` incrusta un **documento HTML/SVG completo** con su propio contexto de navegación: historial, JavaScript, origin, política de seguridad. Para SVG es la opción más pesada, pero la única que proporciona **aislamiento total**.

---

## Sintaxis

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;iframe&gt; para SVG
  </text>

  <rect x="15" y="38" width="270" height="150" fill="#0f172a" rx="3"/>
  <text x="25" y="58" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;iframe</text>
  <text x="35" y="72" font-size="8" font-family="monospace" fill="#60a5fa">  src="grafico.svg"</text>
  <text x="35" y="86" font-size="8" font-family="monospace" fill="#34d399">  width="400"</text>
  <text x="35" y="100" font-size="8" font-family="monospace" fill="#34d399">  height="300"</text>
  <text x="35" y="114" font-size="8" font-family="monospace" fill="#fbbf24">  title="Gráfico de ventas"</text>
  <text x="25" y="128" font-size="8" font-family="monospace" fill="#e2e8f0">&gt;&lt;/iframe&gt;</text>

  <text x="25" y="148" font-size="7" font-family="monospace" fill="#94a3b8">/* el title es obligatorio para accesibilidad */</text>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#94a3b8">/* crea un contexto de navegación completo */</text>
  <text x="25" y="176" font-size="7" font-family="monospace" fill="#94a3b8">/* cada iframe es un "mini-navegador" anidado */</text>
</svg>
```

---

## Aislamiento total

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    qué implica el aislamiento del iframe
  </text>

  <!-- Contexto JS -->
  <rect x="15" y="38" width="270" height="52" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#5b21b6">Contexto JavaScript propio</text>
  <text x="25" y="68" font-size="7" fill="#475569">window, document, variables globales todos separados</text>
  <text x="25" y="80" font-size="7" fill="#475569">los scripts del padre y del iframe viven en "mundos distintos"</text>

  <!-- CSS -->
  <rect x="15" y="98" width="270" height="52" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="114" font-size="9" font-weight="bold" fill="#1e40af">CSS interno aplica normalmente</text>
  <text x="25" y="128" font-size="7" fill="#475569">el SVG dentro del iframe puede tener su propio &lt;style&gt;</text>
  <text x="25" y="140" font-size="7" fill="#475569">las reglas CSS del documento padre no cruzan la frontera</text>

  <!-- Historial -->
  <rect x="15" y="158" width="270" height="52" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="174" font-size="9" font-weight="bold" fill="#166534">Historial y navegación independientes</text>
  <text x="25" y="188" font-size="7" fill="#475569">el iframe puede navegar sin afectar al documento padre</text>
  <text x="25" y="200" font-size="7" fill="#475569">útil para SVGs con enlaces internos (&lt;a xlink:href&gt;)</text>

  <!-- Coste -->
  <rect x="15" y="218" width="270" height="32" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="234" font-size="8" font-weight="bold" fill="#92400e">Coste: un proceso + memoria adicional</text>
  <text x="25" y="246" font-size="7" fill="#475569">cada iframe cuesta como un tab mínimo del navegador</text>
</svg>
```

---

## Comunicación `postMessage`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    puente de comunicación controlado
  </text>

  <!-- Padre -->
  <rect x="15" y="38" width="270" height="88" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">// documento padre → iframe</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">const frame = document.querySelector('iframe');</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">frame.contentWindow.postMessage(</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">  { tipo: 'cambiar-color', color: 'red' },</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">  '*'</text>
  <text x="25" y="122" font-size="8" font-family="monospace" fill="#e2e8f0">);</text>

  <!-- Hijo -->
  <rect x="15" y="136" width="270" height="114" fill="#0f172a" rx="3"/>
  <text x="25" y="152" font-size="8" font-family="monospace" fill="#94a3b8">// iframe escucha mensajes del padre</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#e2e8f0">window.addEventListener('message', (e) =&gt; {</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#34d399">  if (e.data.tipo === 'cambiar-color') {</text>
  <text x="25" y="194" font-size="8" font-family="monospace" fill="#34d399">    document.querySelector('circle')</text>
  <text x="25" y="208" font-size="8" font-family="monospace" fill="#34d399">      .setAttribute('fill', e.data.color);</text>
  <text x="25" y="222" font-size="8" font-family="monospace" fill="#34d399">  }</text>

  <text x="25" y="240" font-size="8" font-family="monospace" fill="#94a3b8">  // responder al padre</text>
  <text x="25" y="252" font-size="8" font-family="monospace" fill="#fbbf24">  window.parent.postMessage({ ok: true }, '*');</text>
</svg>
```

---

## Cuándo tiene sentido

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    casos donde iframe es apropiado
  </text>

  <!-- Sí -->
  <rect x="15" y="38" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ Sí, para</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• editores SVG embebidos con scripts extensos propios</text>
  <text x="25" y="84" font-size="7" fill="#1e293b">• SVGs de origen externo (otro dominio) con aislamiento</text>
  <text x="25" y="98" font-size="7" fill="#1e293b">• gráficos interactivos con historial de navegación</text>
  <text x="25" y="112" font-size="7" fill="#1e293b">• componentes que deben estar "sandboxed" por seguridad</text>
  <text x="25" y="126" font-size="7" fill="#1e293b">• reutilizar SVGs con JS que no puedes modificar</text>

  <!-- No -->
  <rect x="15" y="146" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#991b1b">✗ No, para</text>
  <text x="25" y="178" font-size="7" fill="#1e293b">• íconos y logos simples (usa &lt;img&gt; o inline)</text>
  <text x="25" y="192" font-size="7" fill="#1e293b">• ilustraciones decorativas</text>
  <text x="25" y="206" font-size="7" fill="#1e293b">• gráficos que deben integrarse con el CSS del sitio</text>
  <text x="25" y="220" font-size="7" fill="#1e293b">• cualquier caso donde inline sea viable</text>
  <text x="25" y="234" font-size="7" fill="#991b1b">el coste de memoria no se justifica para casos simples</text>
</svg>
```

---

## Accesibilidad: el `title` obligatorio

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el atributo title es obligatorio
  </text>

  <!-- Mal -->
  <rect x="15" y="38" width="270" height="56" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="8" font-weight="bold" fill="#991b1b">✗ sin title</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;iframe src="grafico.svg"&gt;&lt;/iframe&gt;</text>
  <text x="25" y="82" font-size="7" fill="#991b1b">el lector anuncia "frame, sin nombre" — confuso</text>

  <!-- Bien -->
  <rect x="15" y="104" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="120" font-size="8" font-weight="bold" fill="#166534">✓ con title descriptivo</text>
  <text x="25" y="134" font-size="7" font-family="monospace" fill="#475569">&lt;iframe</text>
  <text x="25" y="146" font-size="7" font-family="monospace" fill="#475569">  src="grafico.svg"</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">  title="Gráfico de ventas trimestrales 2024"&gt;</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">&lt;/iframe&gt;</text>
  <text x="25" y="184" font-size="7" fill="#166534">el lector anuncia el title como nombre del frame</text>
</svg>
```
