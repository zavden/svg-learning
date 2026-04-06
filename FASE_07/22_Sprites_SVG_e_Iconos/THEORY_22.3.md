# 22.3 SVG Sprite externo

En lugar de incluir el sprite inline en cada página, se guarda en un archivo `.svg` separado. El HTML referencia los iconos con `<use href="/assets/sprite.svg#icon-home"/>`. Es más limpio, cacheable y centralizado — pero viene con restricciones de CORS y styling que conviene conocer.

---

## Cómo funciona

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un archivo, referenciado por URL
  </text>

  <rect x="15" y="38" width="270" height="82" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- sprite.svg --&gt;</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg xmlns="..."&gt;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-home" ...&gt;...&lt;/symbol&gt;</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">  &lt;symbol id="icon-user" ...&gt;...&lt;/symbol&gt;</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>

  <rect x="15" y="128" width="270" height="112" fill="#0f172a" rx="3"/>
  <text x="25" y="144" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- index.html --&gt;</text>
  <text x="25" y="160" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;svg class="icono"&gt;</text>
  <text x="25" y="174" font-size="8" font-family="monospace" fill="#34d399">  &lt;use href="/assets/sprite.svg#icon-home"/&gt;</text>
  <text x="25" y="188" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;/svg&gt;</text>
  <text x="25" y="210" font-size="7" fill="#94a3b8">→ el archivo se descarga una vez y se cachea</text>
  <text x="25" y="224" font-size="7" fill="#94a3b8">→ todos los iconos de la app desde un único recurso</text>
</svg>
```

---

## Ventajas del sprite externo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿cuándo compensa?
  </text>

  <rect x="15" y="38" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Cache independiente</text>
  <text x="25" y="70" font-size="7" fill="#475569">el archivo se cachea aparte del HTML</text>
  <text x="25" y="84" font-size="7" fill="#475569">entre páginas solo se descarga una vez</text>

  <rect x="15" y="106" width="270" height="60" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="122" font-size="9" font-weight="bold" fill="#1e40af">HTML limpio</text>
  <text x="25" y="138" font-size="7" fill="#475569">el HTML no carga con un bloque SVG enorme</text>
  <text x="25" y="152" font-size="7" fill="#475569">mejor legibilidad y menor peso de HTML inicial</text>

  <rect x="15" y="174" width="270" height="60" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#5b21b6">Un único lugar</text>
  <text x="25" y="206" font-size="7" fill="#475569">toda la app usa el mismo sprite, con un único</text>
  <text x="25" y="220" font-size="7" fill="#475569">versionado. Actualizar iconos = redeploy sprite</text>
</svg>
```

---

## Limitación 1: CORS y same-origin

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el sprite debe estar en el mismo origen
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">El problema</text>
  <text x="25" y="70" font-size="7" fill="#475569">&lt;use href="https://cdn.com/sprite.svg#home"&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">desde https://miapp.com → bloqueado</text>
  <text x="25" y="98" font-size="7" fill="#475569">el navegador trata el fetch del sprite como recurso</text>
  <text x="25" y="112" font-size="7" fill="#475569">cross-origin y aplica la política same-origin</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">resultado: el icono no aparece, sin error claro</text>

  <rect x="15" y="142" width="270" height="106" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="158" font-size="9" font-weight="bold" fill="#166534">Soluciones</text>
  <text x="25" y="174" font-size="7" fill="#475569">1. servir el sprite desde el mismo dominio</text>
  <text x="25" y="188" font-size="7" fill="#475569">2. habilitar CORS en el CDN con cabecera:</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#475569">   Access-Control-Allow-Origin: *</text>
  <text x="25" y="216" font-size="7" fill="#475569">3. descargar e incrustar inline con fetch al cargar</text>
  <text x="25" y="232" font-size="7" fill="#166534">→ en CDNs propios, habilitar CORS es trivial</text>
</svg>
```

---

## Limitación 2: el CSS no "entra" al sprite

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    CSS no cruza la frontera del &lt;use&gt;
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#92400e">Qué no funciona</text>
  <text x="25" y="70" font-size="7" fill="#475569">el CSS del documento no puede apuntar a elementos</text>
  <text x="25" y="84" font-size="7" fill="#475569">internos del sprite externo — están en un shadow tree</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">use &gt; path { fill: red; } // ❌ no aplica</text>
  <text x="25" y="116" font-size="7" fill="#92400e">es la misma frontera que vimos en la sección 19</text>

  <rect x="15" y="138" width="270" height="110" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Qué sí cruza</text>
  <text x="25" y="170" font-size="7" fill="#475569">• fill/stroke del contenedor si el interno los hereda</text>
  <text x="25" y="184" font-size="7" fill="#475569">• currentColor del padre → los path con fill="currentColor"</text>
  <text x="25" y="198" font-size="7" fill="#475569">• variables CSS pasadas por var(--color)</text>
  <text x="25" y="218" font-size="7" fill="#166534">→ diseña el símbolo con currentColor y/o variables</text>
  <text x="25" y="232" font-size="7" fill="#166534">   desde el principio</text>
</svg>
```

---

## Servir el sprite con headers correctos

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    Content-Type y caché
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8"># el MIME correcto</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">Content-Type: image/svg+xml</text>
  <text x="25" y="92" font-size="7" fill="#94a3b8">servidores como nginx/apache suelen hacerlo bien</text>
  <text x="25" y="106" font-size="7" fill="#94a3b8">pero en APIs custom a veces hay que declararlo</text>

  <rect x="15" y="138" width="270" height="92" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8"># cache agresiva con nombre versionado</text>
  <text x="25" y="168" font-size="8" font-family="monospace" fill="#34d399">Cache-Control: public,</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#34d399">               max-age=31536000,</text>
  <text x="25" y="192" font-size="8" font-family="monospace" fill="#34d399">               immutable</text>
  <text x="25" y="212" font-size="7" fill="#fbbf24">requiere hash en el nombre del archivo:</text>
  <text x="25" y="224" font-size="7" fill="#fbbf24">sprite.a7b3c9.svg — y actualizar el html al deploy</text>
</svg>
```

---

## Inline vs externo — la decisión

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cuándo usar cuál
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Inline (en el HTML)</text>
  <text x="25" y="70" font-size="7" fill="#475569">• una sola página (SPA con pocas rutas)</text>
  <text x="25" y="84" font-size="7" fill="#475569">• pocos iconos usados (3-15)</text>
  <text x="25" y="98" font-size="7" fill="#475569">• necesitas estilar elementos internos del símbolo</text>
  <text x="25" y="112" font-size="7" fill="#475569">• evitas la request extra</text>
  <text x="25" y="126" font-size="7" fill="#1e40af">→ lo más simple de empezar</text>

  <rect x="15" y="146" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Externo (sprite.svg)</text>
  <text x="25" y="178" font-size="7" fill="#475569">• multi-página (websites tradicionales)</text>
  <text x="25" y="192" font-size="7" fill="#475569">• decenas o cientos de iconos</text>
  <text x="25" y="206" font-size="7" fill="#475569">• los símbolos son autocontenidos (currentColor)</text>
  <text x="25" y="220" font-size="7" fill="#475569">• quieres cache agresiva</text>
  <text x="25" y="234" font-size="7" fill="#166534">→ lo más escalable para sitios grandes</text>
</svg>
```
