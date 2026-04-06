# 16.2 El elemento `<foreignObject>`

`<foreignObject>` es la puerta que permite incrustar **contenido no-SVG** dentro de un SVG — típicamente HTML. Lo que consigue: texto multilínea nativo, formularios, listas y cualquier elemento del DOM HTML posicionados como si fueran parte del vector.

---

## Sintaxis mínima: HTML dentro de SVG

```svg
<svg width="300" height="200"
     viewBox="0 0 300 200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">HTML real dentro del SVG</text>

  <rect x="20" y="40" width="260" height="90" fill="white" stroke="#cbd5e1" rx="4"/>

  <foreignObject x="30" y="50" width="240" height="70">
    <div xmlns="http://www.w3.org/1999/xhtml"
         style="font-family:sans-serif; font-size:10px; color:#1e293b; line-height:1.4;">
      <p style="margin:0 0 4px 0;"><strong>párrafo HTML real</strong> con word-wrap nativo y estilos CSS que se respetan completamente.</p>
      <ul style="margin:4px 0; padding-left:16px;">
        <li>listas</li>
        <li>con bullets</li>
      </ul>
    </div>
  </foreignObject>

  <text x="150" y="155" text-anchor="middle" font-size="7" fill="#94a3b8">
    el &lt;div&gt; necesita xmlns="http://www.w3.org/1999/xhtml"
  </text>
  <text x="150" y="170" text-anchor="middle" font-size="7" fill="#94a3b8">
    sin el xmlns, el navegador puede ignorar el contenido
  </text>
  <text x="150" y="188" text-anchor="middle" font-size="7" fill="#ef4444">
    solo funciona en SVG inline, no en &lt;img src="..."&gt;
  </text>
</svg>
```

---

## El caso de uso número 1: texto multilínea

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">texto SVG vs HTML en foreignObject</text>

  <!-- Columna izquierda: texto SVG clásico -->
  <rect x="15" y="35" width="130" height="190" fill="#fef2f2" stroke="#fca5a5" rx="4"/>
  <text x="80" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#991b1b">&lt;text&gt;</text>
  <text x="80" y="64" text-anchor="middle" font-size="6" fill="#64748b">sin word-wrap</text>

  <!-- Texto SVG: se desborda -->
  <text x="25" y="85" font-size="7" fill="#1e293b">Este texto largo no</text>
  <text x="25" y="97" font-size="7" fill="#1e293b">hace salto automático.</text>
  <text x="25" y="109" font-size="7" fill="#1e293b">Cada línea requiere un</text>
  <text x="25" y="121" font-size="7" fill="#1e293b">&lt;tspan&gt; o &lt;text&gt; nuevo.</text>
  <text x="25" y="140" font-size="6" fill="#ef4444">- tedioso</text>
  <text x="25" y="152" font-size="6" fill="#ef4444">- inflexible</text>
  <text x="25" y="164" font-size="6" fill="#ef4444">- no responsive</text>

  <!-- Columna derecha: foreignObject con HTML -->
  <rect x="155" y="35" width="130" height="190" fill="#dcfce7" stroke="#86efac" rx="4"/>
  <text x="220" y="52" text-anchor="middle" font-size="8" font-weight="bold" fill="#166534">&lt;foreignObject&gt;</text>
  <text x="220" y="64" text-anchor="middle" font-size="6" fill="#64748b">word-wrap automático</text>

  <foreignObject x="165" y="75" width="110" height="140">
    <div xmlns="http://www.w3.org/1999/xhtml"
         style="font-family:sans-serif; font-size:7px; color:#1e293b; line-height:1.5;">
      <p style="margin:0 0 8px 0;">Este texto largo hace salto automático gracias al motor de layout de HTML.</p>
      <p style="margin:0 0 6px 0; color:#166534;">+ flexible</p>
      <p style="margin:0 0 6px 0; color:#166534;">+ sin cálculos manuales</p>
      <p style="margin:0; color:#166534;">+ CSS completo</p>
    </div>
  </foreignObject>
</svg>
```

---

## Formularios y elementos interactivos

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">inputs HTML dentro del SVG</text>

  <!-- Fondo decorativo SVG -->
  <defs>
    <linearGradient id="fo-bg" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0" stop-color="#dbeafe"/>
      <stop offset="1" stop-color="#ede9fe"/>
    </linearGradient>
  </defs>
  <rect x="25" y="35" width="250" height="170" fill="url(#fo-bg)" rx="8"/>
  <circle cx="245" cy="55" r="12" fill="#3b82f6" opacity="0.3"/>

  <!-- Formulario como HTML real -->
  <foreignObject x="45" y="55" width="210" height="130">
    <div xmlns="http://www.w3.org/1999/xhtml"
         style="font-family:sans-serif; font-size:9px; color:#1e293b;">
      <label style="display:block; margin-bottom:3px; font-weight:bold;">Email</label>
      <input type="email"
             style="width:100%; padding:4px 6px; border:1px solid #cbd5e1; border-radius:3px; box-sizing:border-box; font-size:9px; margin-bottom:8px;"
             value="user@example.com"/>

      <label style="display:block; margin-bottom:3px; font-weight:bold;">Mensaje</label>
      <textarea style="width:100%; padding:4px 6px; border:1px solid #cbd5e1; border-radius:3px; box-sizing:border-box; font-size:9px; resize:none; height:30px;">Hola desde foreignObject</textarea>

      <button style="margin-top:6px; padding:4px 10px; background:#3b82f6; color:white; border:none; border-radius:3px; font-size:9px; cursor:pointer;">Enviar</button>
    </div>
  </foreignObject>
</svg>
```

---

## Tablas HTML en diagramas

```svg
<svg width="300" height="230"
     viewBox="0 0 300 230"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">una tabla HTML posicionada como elemento SVG</text>

  <!-- "Caja" SVG con sombra -->
  <defs>
    <filter id="fo-shadow" x="-10%" y="-10%" width="120%" height="120%">
      <feDropShadow dx="2" dy="3" stdDeviation="3" flood-opacity="0.15"/>
    </filter>
  </defs>
  <rect x="25" y="40" width="250" height="155" fill="white" stroke="#e2e8f0" rx="4" filter="url(#fo-shadow)"/>
  <rect x="25" y="40" width="250" height="24" fill="#1e293b" rx="4"/>
  <rect x="25" y="55" width="250" height="9" fill="#1e293b"/>
  <text x="150" y="56" text-anchor="middle" font-size="10" font-weight="bold" fill="white">Usuarios activos</text>

  <!-- Tabla HTML -->
  <foreignObject x="40" y="75" width="220" height="115">
    <table xmlns="http://www.w3.org/1999/xhtml"
           style="width:100%; font-family:sans-serif; font-size:8px; border-collapse:collapse;">
      <thead>
        <tr style="border-bottom:1.5px solid #cbd5e1;">
          <th style="text-align:left; padding:4px;">Nombre</th>
          <th style="text-align:left; padding:4px;">Rol</th>
          <th style="text-align:right; padding:4px;">Score</th>
        </tr>
      </thead>
      <tbody>
        <tr style="border-bottom:1px solid #f1f5f9;">
          <td style="padding:4px;">Ana</td>
          <td style="padding:4px; color:#64748b;">admin</td>
          <td style="padding:4px; text-align:right;">2410</td>
        </tr>
        <tr style="border-bottom:1px solid #f1f5f9;">
          <td style="padding:4px;">Ben</td>
          <td style="padding:4px; color:#64748b;">user</td>
          <td style="padding:4px; text-align:right;">1820</td>
        </tr>
        <tr style="border-bottom:1px solid #f1f5f9;">
          <td style="padding:4px;">Carla</td>
          <td style="padding:4px; color:#64748b;">user</td>
          <td style="padding:4px; text-align:right;">1645</td>
        </tr>
        <tr>
          <td style="padding:4px;">Dan</td>
          <td style="padding:4px; color:#64748b;">guest</td>
          <td style="padding:4px; text-align:right;">980</td>
        </tr>
      </tbody>
    </table>
  </foreignObject>

  <text x="150" y="213" text-anchor="middle" font-size="7" fill="#94a3b8">
    &lt;thead&gt;, &lt;tbody&gt;, colspan, CSS… todo funciona
  </text>
</svg>
```

---

## El problema: contextos donde `<foreignObject>` no funciona

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    ¿dónde funciona &lt;foreignObject&gt;?
  </text>

  <!-- Sí funciona -->
  <rect x="10" y="36" width="280" height="88" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="52" font-size="9" font-weight="bold" fill="#166534">✓ Funciona en</text>
  <text x="18" y="68" font-size="7" fill="#1e293b">• SVG inline en HTML: &lt;svg&gt;...&lt;/svg&gt; dentro de &lt;body&gt;</text>
  <text x="18" y="82" font-size="7" fill="#1e293b">• Renderizado del navegador moderno (Chrome, Firefox, Safari)</text>
  <text x="18" y="96" font-size="7" fill="#1e293b">• Herramientas JS de diagramación (mermaid, D3)</text>
  <text x="18" y="110" font-size="7" fill="#1e293b">• Capturas de pantalla con html2canvas con config adecuada</text>

  <!-- No funciona -->
  <rect x="10" y="134" width="280" height="90" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="18" y="150" font-size="9" font-weight="bold" fill="#991b1b">✗ No funciona en</text>
  <text x="18" y="166" font-size="7" fill="#1e293b">• &lt;img src="diagram.svg"&gt; · CSS background-image</text>
  <text x="18" y="180" font-size="7" fill="#1e293b">• SVG como archivo independiente abierto en navegador</text>
  <text x="18" y="194" font-size="7" fill="#1e293b">• Herramientas de conversión SVG → PNG (inkscape, rsvg)</text>
  <text x="18" y="208" font-size="7" fill="#1e293b">• Sanitizadores que eliminan &lt;foreignObject&gt; por seguridad</text>

  <text x="150" y="243" text-anchor="middle" font-size="7" fill="#94a3b8">
    regla: si el SVG será inline en HTML siempre → puedes usarlo
  </text>
  <text x="150" y="253" text-anchor="middle" font-size="7" fill="#94a3b8">
    si el SVG viaja como imagen → busca otra solución
  </text>
</svg>
```

---

## Alternativas a `<foreignObject>`

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" fill="#f8fafc" font-weight="bold">
    cuándo NO usar foreignObject
  </text>

  <!-- Alternativa 1: tspans -->
  <rect x="10" y="36" width="280" height="48" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="18" y="50" font-size="8" font-weight="bold" fill="#1e40af">Texto multilínea simple → &lt;tspan&gt;</text>
  <text x="18" y="63" font-size="7" fill="#64748b">manualmente controlando cada salto con dy="1.2em"</text>
  <text x="18" y="75" font-size="7" fill="#64748b">funciona en cualquier contexto donde aparezca el SVG</text>

  <!-- Alternativa 2: HTML externo absolutamente posicionado -->
  <rect x="10" y="92" width="280" height="48" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="18" y="106" font-size="8" font-weight="bold" fill="#166534">Formularios → HTML encima del SVG</text>
  <text x="18" y="119" font-size="7" fill="#64748b">position:absolute con coordenadas en px sobre el canvas</text>
  <text x="18" y="131" font-size="7" fill="#64748b">evita todas las limitaciones de foreignObject</text>

  <!-- Alternativa 3: pre-renderizar como imagen -->
  <rect x="10" y="148" width="280" height="48" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="18" y="162" font-size="8" font-weight="bold" fill="#92400e">Contenido rico portable → pre-renderizar</text>
  <text x="18" y="175" font-size="7" fill="#64748b">generar la tabla/texto como PNG y embeberla con &lt;image&gt;</text>
  <text x="18" y="187" font-size="7" fill="#64748b">pierde interactividad pero funciona en todos los contextos</text>

  <text x="150" y="218" text-anchor="middle" font-size="7" fill="#94a3b8">
    foreignObject es muy útil, pero no es portable
  </text>
  <text x="150" y="230" text-anchor="middle" font-size="7" fill="#94a3b8">
    elige según dónde vivirá el SVG
  </text>
</svg>
```
