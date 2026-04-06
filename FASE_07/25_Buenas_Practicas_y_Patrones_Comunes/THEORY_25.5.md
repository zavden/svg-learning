# 25.5 Seguridad

Un SVG no es una imagen. Es un **documento XML** que puede contener `<script>`, handlers de eventos (`onload`, `onclick`), enlaces `javascript:`, o `<foreignObject>` con HTML arbitrario. Si aceptas SVG de fuentes externas y lo muestras inline, tienes un vector XSS sin saberlo.

---

## SVG es código, no píxeles

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#991b1b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    lo que un SVG puede contener y no debería
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">&lt;script&gt; ejecutable</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;svg&gt;&lt;script&gt;alert('XSS')&lt;/script&gt;&lt;/svg&gt;</text>
  <text x="25" y="86" font-size="7" fill="#475569">→ mismo origen que tu página si es inline</text>

  <rect x="15" y="106" width="270" height="68" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#991b1b">Event handlers en atributos</text>
  <text x="25" y="138" font-size="7" font-family="monospace" fill="#475569">&lt;circle onload="fetch('/exfil?c='+</text>
  <text x="25" y="152" font-size="7" font-family="monospace" fill="#475569">  document.cookie)" /&gt;</text>
  <text x="25" y="166" font-size="7" fill="#475569">→ onload, onclick, onerror, onmouseover...</text>

  <rect x="15" y="182" width="270" height="68" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="198" font-size="9" font-weight="bold" fill="#991b1b">&lt;foreignObject&gt; con HTML</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#475569">&lt;foreignObject&gt;</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#475569">  &lt;img src=x onerror="..."&gt;</text>
  <text x="25" y="242" font-size="7" font-family="monospace" fill="#475569">&lt;/foreignObject&gt;</text>

  <rect x="15" y="258" width="270" height="30" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="274" font-size="9" font-weight="bold" fill="#991b1b">&lt;a href="javascript:..."&gt;</text>
  <text x="25" y="286" font-size="7" fill="#475569">SVG permite enlaces con esquema javascript:</text>
</svg>
```

---

## Niveles de riesgo por método de uso

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿se ejecutan los scripts?
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Método</text>
  <text x="170" y="46" font-size="7" font-weight="bold" fill="#1e293b">Scripts ejecutan</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">&lt;img src="foo.svg"&gt;</text>
  <text x="170" y="66" font-size="7" fill="#10b981">✓ No — seguro</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">background-image: url(...)</text>
  <text x="170" y="88" font-size="7" fill="#10b981">✓ No — seguro</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">&lt;object&gt; / &lt;embed&gt;</text>
  <text x="170" y="110" font-size="7" fill="#f59e0b">⚠ Sí (aislado)</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">&lt;iframe src="foo.svg"&gt;</text>
  <text x="170" y="132" font-size="7" fill="#f59e0b">⚠ Sí (sandboxable)</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">SVG inline en HTML</text>
  <text x="170" y="154" font-size="7" fill="#ef4444">✗ Sí — mismo origen</text>

  <rect x="15" y="176" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="192" font-size="9" font-weight="bold" fill="#166534">Regla de oro</text>
  <text x="25" y="208" font-size="7" fill="#475569">• SVG de fuente confiable → inline está bien</text>
  <text x="25" y="222" font-size="7" fill="#475569">• SVG de usuario → &lt;img&gt; o sanitizar antes</text>
  <text x="25" y="236" font-size="7" fill="#475569">• SVG de terceros → nunca inline sin sanitizar</text>
  <text x="25" y="252" font-size="7" fill="#166534">→ la regla se aplica incluso a SVGs "simples"</text>
  <text x="25" y="266" font-size="7" fill="#166534">→ no confíes en inspección visual</text>
</svg>
```

---

## Sanitización con DOMPurify

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la librería estándar del lado del cliente
  </text>

  <rect x="15" y="38" width="270" height="248" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// instalación</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">npm i dompurify</text>

  <text x="25" y="92" font-size="7" font-family="monospace" fill="#94a3b8">// uso específico para SVG</text>
  <text x="25" y="108" font-size="8" font-family="monospace" fill="#60a5fa">import DOMPurify from 'dompurify'</text>

  <text x="25" y="130" font-size="8" font-family="monospace" fill="#34d399">const limpio = DOMPurify.sanitize(</text>
  <text x="25" y="144" font-size="8" font-family="monospace" fill="#34d399">  svgSucio, {</text>
  <text x="25" y="158" font-size="8" font-family="monospace" fill="#fbbf24">    USE_PROFILES: {</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#ec4899">      svg: true,</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#ec4899">      svgFilters: true</text>
  <text x="25" y="200" font-size="8" font-family="monospace" fill="#fbbf24">    },</text>
  <text x="25" y="214" font-size="8" font-family="monospace" fill="#fbbf24">    FORBID_TAGS: [</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#ec4899">      'script', 'foreignObject'],</text>
  <text x="25" y="242" font-size="8" font-family="monospace" fill="#fbbf24">    FORBID_ATTR: [</text>
  <text x="25" y="256" font-size="7" font-family="monospace" fill="#ec4899">      'onload', 'onclick', 'onerror']</text>
  <text x="25" y="270" font-size="8" font-family="monospace" fill="#34d399">  })</text>
</svg>
```

---

## Validación en el servidor

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    no confíes solo en el cliente
  </text>

  <rect x="15" y="38" width="270" height="112" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Errores comunes</text>
  <text x="25" y="70" font-size="7" fill="#475569">• confiar en la extensión .svg</text>
  <text x="25" y="84" font-size="7" fill="#475569">• confiar en Content-Type del cliente</text>
  <text x="25" y="98" font-size="7" fill="#475569">• guardar el SVG "tal cual" en disco</text>
  <text x="25" y="112" font-size="7" fill="#475569">• servirlo después como inline</text>
  <text x="25" y="126" font-size="7" fill="#475569">• asumir que los uploads son "solo imágenes"</text>
  <text x="25" y="140" font-size="7" fill="#991b1b">→ XSS almacenado garantizado</text>

  <rect x="15" y="162" width="270" height="84" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="178" font-size="9" font-weight="bold" fill="#166534">Qué hacer en el backend</text>
  <text x="25" y="194" font-size="7" fill="#475569">• detectar tipo real (magic bytes + XML parse)</text>
  <text x="25" y="208" font-size="7" fill="#475569">• sanitizar con una librería del lenguaje</text>
  <text x="25" y="222" font-size="7" fill="#475569">  (bleach en Python, sanitize en Ruby...)</text>
  <text x="25" y="236" font-size="7" fill="#166534">→ doble barrera: servidor + cliente</text>
</svg>
```

---

## CSP: la red de seguridad

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Content Security Policy
  </text>

  <rect x="15" y="38" width="270" height="110" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// cabecera HTTP</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#60a5fa">Content-Security-Policy:</text>
  <text x="25" y="86" font-size="7" font-family="monospace" fill="#34d399">  default-src 'self';</text>
  <text x="25" y="100" font-size="7" font-family="monospace" fill="#34d399">  img-src 'self' data:;</text>
  <text x="25" y="114" font-size="7" font-family="monospace" fill="#34d399">  script-src 'self';</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#34d399">  style-src 'self' 'unsafe-inline'</text>

  <rect x="15" y="158" width="270" height="112" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="174" font-size="9" font-weight="bold" fill="#166534">Qué bloquea</text>
  <text x="25" y="190" font-size="7" fill="#475569">• script-src 'self' → no scripts inline</text>
  <text x="25" y="204" font-size="7" fill="#475569">  → un &lt;script&gt; dentro de un SVG no corre</text>
  <text x="25" y="218" font-size="7" fill="#475569">• event handlers en HTML tampoco corren</text>
  <text x="25" y="232" font-size="7" fill="#475569">  (onclick="...", onload="...")</text>
  <text x="25" y="248" font-size="7" fill="#166534">→ red de seguridad incluso si la sanitización</text>
  <text x="25" y="262" font-size="7" fill="#166534">  tiene un bug</text>
</svg>
```
