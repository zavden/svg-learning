# Errores comunes — Integración de SVG en la web

---

## 1. Intentar cambiar el color de un SVG en `<img>` con CSS externo

Un clásico error: copiar un ícono SVG, incluirlo con `<img src="icon.svg">` y luego escribir `img path { fill: red; }` esperando que funcione. El SVG dentro de `<img>` está **sandboxed**: las reglas CSS del documento padre no alcanzan su interior, aunque el selector parezca apuntar ahí.

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">CSS externo no aplica dentro de &lt;img&gt;</text>

  <rect x="15" y="32" width="270" height="68" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Mal</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;img src="heart.svg" class="rojo"&gt;</text>
  <text x="25" y="76" font-size="7" font-family="monospace" fill="#475569">img.rojo path { fill: red; }  /* ignorado */</text>
  <text x="25" y="92" font-size="7" fill="#991b1b">el SVG sigue con su color original</text>

  <rect x="15" y="108" width="270" height="78" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="124" font-size="9" font-weight="bold" fill="#166534">✓ Soluciones</text>
  <text x="25" y="138" font-size="7" fill="#475569">• usa SVG inline: el CSS externo sí aplica</text>
  <text x="25" y="152" font-size="7" fill="#475569">• usa fill="currentColor" en el SVG + CSS color en el padre</text>
  <text x="25" y="166" font-size="7" fill="#475569">• aproxima con filter: hue-rotate() sobre el &lt;img&gt;</text>
  <text x="25" y="180" font-size="7" fill="#475569">• genera el SVG con el color correcto (build time)</text>
</svg>
```

---

## 2. Poner un SVG decorativo como `background-image` y perder accesibilidad

`background-image` esconde el SVG **completamente** de los lectores de pantalla. Usarlo para íconos informativos (estado, alerta, significado) deja al usuario sin forma de saber qué representa.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">background-image = accesibilidad cero</text>

  <rect x="15" y="32" width="270" height="88" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Ícono crítico como background</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">.alerta {</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">  background-image: url('warning.svg');</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#475569">  padding-left: 24px;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">el usuario de lector de pantalla nunca se entera de la alerta</text>

  <rect x="15" y="128" width="270" height="78" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="144" font-size="9" font-weight="bold" fill="#166534">✓ Usa &lt;img&gt; con alt o SVG inline</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">&lt;div class="alerta"&gt;</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">  &lt;img src="warning.svg" alt="Advertencia"&gt;</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">  Atención: acción irreversible</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">&lt;/div&gt;</text>
</svg>
```

---

## 3. Usar `<iframe>` para un ícono simple

Un `<iframe>` crea un **contexto de navegación completo**: memoria, proceso, historial. Usarlo para un ícono de 200 bytes es como montar un estadio para una partida de ajedrez. El coste de memoria se multiplica por cada iframe, y los navegadores limitan el número total.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">iframe es overkill para íconos</text>

  <rect x="15" y="32" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ iframe para un ícono</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;iframe src="icon.svg" width="24" height="24"</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">        title="Enviar"&gt;&lt;/iframe&gt;</text>
  <text x="25" y="90" font-size="7" fill="#991b1b">crea un subproceso completo + contexto JS</text>
  <text x="25" y="102" font-size="7" fill="#991b1b">la página con 30 íconos sufre seriamente</text>

  <rect x="15" y="120" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#166534">✓ Para íconos: inline o &lt;img&gt;</text>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 24 24" aria-hidden="true"&gt;</text>
  <text x="25" y="162" font-size="7" font-family="monospace" fill="#475569">  &lt;path d="..."/&gt;</text>
  <text x="25" y="174" font-size="7" font-family="monospace" fill="#475569">&lt;/svg&gt;</text>
  <text x="25" y="190" font-size="7" fill="#166534">reserva iframe para SVGs grandes y aislados (editores)</text>
</svg>
```

---

## 4. Incrustar SVGs grandes como data URI en CSS

Los data URIs inflados con SVGs medianos destruyen el rendimiento: (a) el CSS crece mucho y se bloquea el render inicial, (b) el SVG no se cachea independientemente, (c) cada uso en el CSS duplica el peso. Lo que en un archivo `.svg` ocupa 15 KB puede convertirse en 20 KB de CSS URL-encoded o 25 KB en base64.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">data URI solo para SVGs pequeños</text>

  <rect x="15" y="32" width="270" height="82" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Ilustración grande inline en CSS</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">.hero {</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">  background: url("data:image/svg+xml;base64,</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#475569">    [20 KB de base64]");</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="108" font-size="7" fill="#991b1b">bloquea el parser CSS y no se cachea por separado</text>

  <rect x="15" y="122" width="270" height="80" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="138" font-size="9" font-weight="bold" fill="#166534">✓ Regla empírica</text>
  <text x="25" y="152" font-size="7" fill="#475569">• SVG &lt; 1 KB: data URI razonable</text>
  <text x="25" y="166" font-size="7" fill="#475569">• SVG 1-4 KB: evalúa caso por caso</text>
  <text x="25" y="180" font-size="7" fill="#475569">• SVG &gt; 4 KB: siempre archivo externo con cache</text>
  <text x="25" y="194" font-size="7" fill="#475569">• si se repite en varios lugares: externo sí o sí</text>
</svg>
```

---

## 5. Esperar que los scripts internos funcionen en `<img>`

Una confusión común: poner `<script>` dentro de un SVG y referenciarlo con `<img>`. Los scripts **no se ejecutan**. Esto es una medida de seguridad del navegador — un SVG externo que ejecutara JS sería un vector de ataques XSS.

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">scripts SVG en &lt;img&gt; — no se ejecutan</text>

  <rect x="15" y="32" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="48" font-size="9" font-weight="bold" fill="#991b1b">✗ Mal (falso supuesto)</text>
  <text x="25" y="62" font-size="7" font-family="monospace" fill="#475569">&lt;!-- chart.svg --&gt;</text>
  <text x="25" y="74" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;&lt;script&gt;animate();&lt;/script&gt;&lt;/svg&gt;</text>
  <text x="25" y="90" font-size="7" font-family="monospace" fill="#475569">&lt;img src="chart.svg"&gt;  /* script NO corre */</text>
  <text x="25" y="102" font-size="7" fill="#991b1b">decisión de seguridad del navegador, irreversible</text>

  <rect x="15" y="120" width="270" height="82" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="136" font-size="9" font-weight="bold" fill="#166534">✓ Alternativas</text>
  <text x="25" y="150" font-size="7" fill="#475569">• SVG inline: los scripts corren en el contexto del HTML</text>
  <text x="25" y="164" font-size="7" fill="#475569">• &lt;object&gt;: sí ejecuta scripts internos del SVG</text>
  <text x="25" y="178" font-size="7" fill="#475569">• &lt;iframe&gt;: ejecuta en contexto completamente aislado</text>
  <text x="25" y="192" font-size="7" fill="#475569">• animaciones SMIL: sí funcionan en &lt;img&gt; (no son scripts)</text>
</svg>
```
