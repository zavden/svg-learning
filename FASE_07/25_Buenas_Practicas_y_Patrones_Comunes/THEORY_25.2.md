# 25.2 Nombrado y semántica

Los IDs auto-generados por los editores (`path4532`, `g17`) son una herencia que se paga después, cuando alguien intenta estilizar desde CSS o animar desde JS. Nombrar con intención cuesta lo mismo que no hacerlo y se paga solo la primera vez que hay que mantener el SVG.

---

## IDs: cuándo descriptivos, cuándo cortos

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    la pregunta clave: ¿quién llama a ese id?
  </text>

  <rect x="15" y="38" width="270" height="104" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Referenciado externamente → descriptivo</text>
  <text x="25" y="70" font-size="7" fill="#475569">desde JS: document.getElementById('icon-close')</text>
  <text x="25" y="84" font-size="7" fill="#475569">desde CSS: #grad-primario { ... }</text>
  <text x="25" y="98" font-size="7" fill="#475569">desde sprite: &lt;use href="#icon-menu" /&gt;</text>
  <text x="25" y="114" font-size="7" fill="#475569">desde A11y: aria-labelledby="titulo-mapa"</text>
  <text x="25" y="130" font-size="7" fill="#166534">→ SVGO debe dejarlo tal cual (cleanupIds: false)</text>

  <rect x="15" y="150" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="166" font-size="9" font-weight="bold" fill="#1e40af">Solo interno → corto está bien</text>
  <text x="25" y="182" font-size="7" fill="#475569">id solo usado dentro del mismo SVG:</text>
  <text x="25" y="196" font-size="7" font-family="monospace" fill="#475569">&lt;linearGradient id="a"&gt;</text>
  <text x="25" y="210" font-size="7" font-family="monospace" fill="#475569">&lt;rect fill="url(#a)"/&gt;</text>
  <text x="25" y="226" font-size="7" fill="#475569">→ SVGO puede acortar a 1 letra sin riesgo</text>
  <text x="25" y="240" font-size="7" fill="#1e40af">→ cleanupIds: true es seguro</text>

  <rect x="15" y="258" width="270" height="16" fill="#fef9c3" stroke="#f59e0b" rx="2"/>
  <text x="25" y="269" font-size="7" fill="#92400e">la duda → déjalo descriptivo</text>
</svg>
```

---

## Convención de nombres

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    prefijo por tipo + kebab-case
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Prefijo</text>
  <text x="150" y="46" font-size="7" font-weight="bold" fill="#1e293b">Ejemplo</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">icon-</text>
  <text x="150" y="66" font-size="7" font-family="monospace" fill="#3b82f6">icon-close, icon-arrow-up</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">grad-</text>
  <text x="150" y="88" font-size="7" font-family="monospace" fill="#3b82f6">grad-primario, grad-hero</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">clip-</text>
  <text x="150" y="110" font-size="7" font-family="monospace" fill="#3b82f6">clip-avatar-circle</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">mask-</text>
  <text x="150" y="132" font-size="7" font-family="monospace" fill="#3b82f6">mask-fade-bottom</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">filter-</text>
  <text x="150" y="154" font-size="7" font-family="monospace" fill="#3b82f6">filter-shadow-soft</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">pattern-</text>
  <text x="150" y="176" font-size="7" font-family="monospace" fill="#3b82f6">pattern-dots-small</text>

  <rect x="15" y="192" width="270" height="98" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="208" font-size="9" font-weight="bold" fill="#991b1b">IDs inválidos</text>
  <text x="25" y="224" font-size="7" font-family="monospace" fill="#475569">2xl-icon</text>
  <text x="25" y="238" font-size="7" fill="#475569">→ no puede empezar por número</text>
  <text x="25" y="254" font-size="7" font-family="monospace" fill="#475569">"mi icono"</text>
  <text x="25" y="268" font-size="7" fill="#475569">→ no puede tener espacios</text>
  <text x="25" y="284" font-size="7" fill="#991b1b">→ guiones y _ están permitidos; # &amp; . no</text>
</svg>
```

---

## `<title>` y `<desc>` para accesibilidad

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG con significado → A11y obligatoria
  </text>

  <rect x="15" y="38" width="270" height="120" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8">// SVG informativo</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">&lt;svg role="img"</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">  aria-labelledby="t d"&gt;</text>
  <text x="25" y="104" font-size="8" font-family="monospace" fill="#34d399">  &lt;title id="t"&gt;</text>
  <text x="25" y="118" font-size="7" font-family="monospace" fill="#fbbf24">    Ventas 2024</text>
  <text x="25" y="132" font-size="8" font-family="monospace" fill="#34d399">  &lt;/title&gt;</text>
  <text x="25" y="148" font-size="8" font-family="monospace" fill="#34d399">  &lt;desc id="d"&gt;...&lt;/desc&gt;</text>

  <rect x="15" y="170" width="270" height="60" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="186" font-size="9" font-weight="bold" fill="#166534">SVG decorativo</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#475569">&lt;svg aria-hidden="true"</text>
  <text x="25" y="216" font-size="7" font-family="monospace" fill="#475569">     focusable="false"&gt;</text>

  <rect x="15" y="240" width="270" height="52" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="256" font-size="9" font-weight="bold" fill="#92400e">Icono en botón sin texto</text>
  <text x="25" y="272" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Cerrar"&gt;</text>
  <text x="25" y="284" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"&gt;...</text>
</svg>
```

---

## Clases CSS: mismas reglas que HTML

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    clases = estado visual, IDs = identidad
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Clases para estado reutilizable</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;path class="icono" /&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">&lt;path class="icono activo" /&gt;</text>
  <text x="25" y="100" font-size="7" fill="#475569">→ varios elementos comparten comportamiento,</text>
  <text x="25" y="114" font-size="7" fill="#475569">  el estado se añade con JS: el.classList.add</text>

  <rect x="15" y="134" width="270" height="110" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#166534">BEM si el proyecto lo usa</text>
  <text x="25" y="166" font-size="7" font-family="monospace" fill="#475569">.card__icono</text>
  <text x="25" y="180" font-size="7" font-family="monospace" fill="#475569">.card__icono--activo</text>
  <text x="25" y="194" font-size="7" font-family="monospace" fill="#475569">.btn--primario .btn__icono</text>
  <text x="25" y="214" font-size="7" fill="#475569">→ SVG dentro del sistema de diseño general,</text>
  <text x="25" y="228" font-size="7" fill="#166534">no un lenguaje aparte</text>
</svg>
```
