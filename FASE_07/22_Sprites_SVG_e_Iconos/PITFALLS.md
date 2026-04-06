# PITFALLS — Sprites SVG e Iconos

Errores típicos al montar un sistema de iconos: escalado roto, tematización imposible, sprites que no cargan, CSS que no aplica y accesibilidad confusa. Cinco trampas comunes y cómo esquivarlas.

---

## 1. Usar `<g>` en vez de `<symbol>` para agrupar iconos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    &lt;g&gt; no escala, &lt;symbol&gt; sí
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;g id="home"&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;path d="M10 20v-6h4..."/&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">&lt;/g&gt;</text>
  <text x="25" y="114" font-size="7" font-family="monospace" fill="#475569">&lt;use href="#home" width="48" height="48"/&gt;</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">→ el icono sale con tamaño original, no 48×48</text>

  <rect x="15" y="146" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;symbol id="home" viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#475569">  &lt;path d="M10 20v-6h4..."/&gt;</text>
  <text x="25" y="206" font-size="7" font-family="monospace" fill="#475569">&lt;/symbol&gt;</text>
  <text x="25" y="220" font-size="7" font-family="monospace" fill="#475569">&lt;use href="#home" width="48" height="48"/&gt;</text>
  <text x="25" y="234" font-size="7" fill="#166534">→ el viewBox interno escala al tamaño del &lt;use&gt;</text>

  <rect x="15" y="250" width="270" height="20" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="263" font-size="7" fill="#92400e">regla: &lt;g&gt; agrupa, &lt;symbol&gt; define una plantilla escalable</text>
</svg>
```

---

## 2. Hardcodear `fill="#..."` en el símbolo

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    sin currentColor no hay tematización
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;symbol id="check" viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;path fill="#1e293b" d="..."/&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">&lt;/symbol&gt;</text>
  <text x="25" y="114" font-size="7" fill="#991b1b">→ el check siempre es gris oscuro, ignora CSS</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">  tendrías que duplicar el símbolo por cada color</text>

  <rect x="15" y="146" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;symbol id="check" viewBox="0 0 24 24"&gt;</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#475569">  &lt;path fill="currentColor" d="..."/&gt;</text>
  <text x="25" y="206" font-size="7" font-family="monospace" fill="#475569">&lt;/symbol&gt;</text>
  <text x="25" y="220" font-size="7" font-family="monospace" fill="#475569">&lt;button style="color: red"&gt;...&lt;/button&gt;</text>
  <text x="25" y="234" font-size="7" fill="#166534">→ un símbolo vale para todos los temas</text>

  <rect x="15" y="250" width="270" height="20" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="263" font-size="7" fill="#92400e">regla: los iconos heredan color por defecto, no se pintan</text>
</svg>
```

---

## 3. Sprite externo de otro origen sin CORS

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el sprite debe hablar CORS con el navegador
  </text>

  <rect x="15" y="38" width="270" height="106" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal — y sin error claro en la consola</text>
  <text x="25" y="70" font-size="7" fill="#475569">desde https://miapp.com</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">&lt;use href="https://cdn.otro.com/sprite.svg#home"/&gt;</text>
  <text x="25" y="100" font-size="7" fill="#475569">el CDN no envía Access-Control-Allow-Origin</text>
  <text x="25" y="114" font-size="7" fill="#475569">el fetch del sprite es bloqueado silenciosamente</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">→ el icono no aparece, no hay pista obvia</text>

  <rect x="15" y="152" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="168" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="184" font-size="7" fill="#475569">1. servir el sprite desde el mismo dominio, o</text>
  <text x="25" y="198" font-size="7" fill="#475569">2. habilitar CORS en el CDN:</text>
  <text x="25" y="212" font-size="7" font-family="monospace" fill="#475569">   Access-Control-Allow-Origin: *</text>
  <text x="25" y="226" font-size="7" fill="#475569">3. o hacer fetch manual e incrustar en el DOM</text>
  <text x="25" y="240" font-size="7" fill="#166534">→ si el icono no aparece, abre la pestaña Network</text>

  <rect x="15" y="256" width="270" height="16" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="267" font-size="7" fill="#92400e">regla: mismo origen por defecto, o CORS explícito</text>
</svg>
```

---

## 4. Intentar estilar elementos internos del `<use>` con CSS

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    el shadow tree del &lt;use&gt; bloquea el CSS
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;symbol id="mail"&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;path class="sobre" d="..."/&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">&lt;/symbol&gt;</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">.sobre { fill: #3b82f6; }  /* ❌ no aplica */</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ los hijos clonados viven en un shadow tree</text>
  <text x="25" y="140" font-size="7" fill="#991b1b">  los selectores del documento no cruzan</text>

  <rect x="15" y="150" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#166534">Bien — lo que sí cruza</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">&lt;path fill="currentColor"/&gt;</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#475569">&lt;path fill="var(--bg, #e2e8f0)"/&gt;</text>
  <text x="25" y="210" font-size="7" fill="#475569">desde fuera: .icon { color: red; --bg: blue; }</text>
  <text x="25" y="224" font-size="7" fill="#166534">propiedades heredables + variables CSS sí entran</text>
  <text x="25" y="238" font-size="7" fill="#166534">→ diseña el símbolo pensando en qué puede heredar</text>

  <rect x="15" y="258" width="270" height="16" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="269" font-size="7" fill="#92400e">regla: nada de selectores de clase dentro del símbolo</text>
</svg>
```

---

## 5. Olvidar `aria-hidden` + `focusable="false"`

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    iconos decorativos mal anunciados
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;svg&gt;&lt;use href="#home"/&gt;&lt;/svg&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">  Inicio</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ en IE el &lt;svg&gt; recibe foco por tab</text>
  <text x="25" y="140" font-size="7" fill="#991b1b">→ lectores pueden anunciar el icono como extra</text>

  <rect x="15" y="150" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#166534">Bien (decorativo)</text>
  <text x="25" y="182" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"</text>
  <text x="25" y="210" font-size="7" font-family="monospace" fill="#475569">       focusable="false"&gt;</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#475569">    &lt;use href="#home"/&gt;</text>
  <text x="25" y="238" font-size="7" font-family="monospace" fill="#475569">  &lt;/svg&gt; Inicio</text>

  <rect x="15" y="256" width="270" height="28" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="270" font-size="8" fill="#1e40af">Sin texto visible → aria-label en el botón:</text>
  <text x="25" y="282" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Volver al inicio"&gt;...&lt;/button&gt;</text>
</svg>
```
