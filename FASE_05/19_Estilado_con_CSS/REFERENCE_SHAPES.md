# Referencia rápida — Estilado de SVG con CSS

---

## Cheat sheet: la cascada SVG

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">prioridad de menor a mayor</text>

  <rect x="20" y="32" width="260" height="22" fill="#f1f5f9" stroke="#94a3b8" rx="3"/>
  <text x="30" y="46" font-size="8" fill="#475569">1. valor inicial / herencia</text>

  <rect x="20" y="58" width="260" height="22" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="30" y="72" font-size="8" fill="#1e40af">2. atributos de presentación (fill="red")</text>

  <rect x="20" y="84" width="260" height="22" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="30" y="98" font-size="8" fill="#166534">3. CSS hoja de estilos (por especificidad)</text>

  <rect x="20" y="110" width="260" height="22" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="30" y="124" font-size="8" fill="#5b21b6">4. estilo inline (style="...")</text>

  <rect x="20" y="136" width="260" height="22" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="30" y="150" font-size="8" fill="#991b1b">5. !important</text>

  <rect x="15" y="170" width="270" height="58" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="186" font-size="8" font-weight="bold" fill="#92400e">recordar</text>
  <text x="25" y="200" font-size="7" fill="#475569">los atributos pierden contra CUALQUIER regla CSS</text>
  <text x="25" y="214" font-size="7" fill="#475569">→ exporta SVGs con atributos para poder tematizarlos</text>
</svg>
```

---

## Tabla: tres formas de aplicar CSS

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">forma → cuándo usarla</text>

  <rect x="10" y="28" width="280" height="18" fill="#e2e8f0"/>
  <text x="20" y="40" font-size="7" font-weight="bold" fill="#1e293b">Forma</text>
  <text x="120" y="40" font-size="7" font-weight="bold" fill="#1e293b">Caso típico</text>
  <text x="240" y="40" font-size="7" font-weight="bold" fill="#1e293b">Prioridad</text>

  <rect x="10" y="46" width="280" height="22" fill="white"/>
  <text x="20" y="60" font-size="7" font-family="monospace" fill="#3b82f6">fill="x"</text>
  <text x="120" y="60" font-size="7" fill="#475569">defaults sobrescribibles</text>
  <text x="240" y="60" font-size="7" fill="#94a3b8">baja</text>

  <rect x="10" y="68" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="82" font-size="7" font-family="monospace" fill="#10b981">&lt;style&gt;</text>
  <text x="120" y="82" font-size="7" fill="#475569">muchos elementos comunes</text>
  <text x="240" y="82" font-size="7" fill="#94a3b8">media</text>

  <rect x="10" y="90" width="280" height="22" fill="white"/>
  <text x="20" y="104" font-size="7" font-family="monospace" fill="#10b981">CSS externo</text>
  <text x="120" y="104" font-size="7" fill="#475569">SVG inline en HTML</text>
  <text x="240" y="104" font-size="7" fill="#94a3b8">media</text>

  <rect x="10" y="112" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="126" font-size="7" font-family="monospace" fill="#8b5cf6">style="..."</text>
  <text x="120" y="126" font-size="7" fill="#475569">var(), calc(), no-override</text>
  <text x="240" y="126" font-size="7" fill="#94a3b8">alta</text>

  <rect x="10" y="134" width="280" height="22" fill="white"/>
  <text x="20" y="148" font-size="7" font-family="monospace" fill="#ef4444">!important</text>
  <text x="120" y="148" font-size="7" fill="#475569">override de un default</text>
  <text x="240" y="148" font-size="7" fill="#94a3b8">máxima</text>

  <text x="150" y="180" text-anchor="middle" font-size="7" fill="#94a3b8">en la práctica se mezclan: defaults (atributos) +</text>
  <text x="150" y="192" text-anchor="middle" font-size="7" fill="#94a3b8">tema (CSS) + estado (clases) + vars (theming)</text>
  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#475569">cada capa tiene su rol y su prioridad</text>
</svg>
```

---

## Snippets copy-paste

### Tematización con variables CSS

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <style>
    :root {
      --c-relleno: var(--icono-color, steelblue);
      --c-trazo: var(--icono-acento, #1e293b);
      --grosor: var(--icono-grosor, 2);
    }
    circle {
      fill: var(--c-relleno);
      stroke: var(--c-trazo);
      stroke-width: var(--grosor);
    }
  </style>
  <circle cx="50" cy="50" r="40"/>
</svg>
```

```css
/* en el documento padre */
.tema-claro svg { --icono-color: white; --icono-acento: #1e293b; }
.tema-oscuro svg { --icono-color: #1e293b; --icono-acento: white; }
.estado-error svg { --icono-color: #ef4444; }
```

### Hover sobre elementos individuales

```svg
<svg viewBox="0 0 200 100" xmlns="http://www.w3.org/2000/svg">
  <style>
    circle {
      fill: steelblue;
      transition: fill 0.3s, transform 0.3s;
      transform-origin: center;
    }
    circle:hover {
      fill: #e74c3c;
      transform: scale(1.2);
      cursor: pointer;
    }
  </style>
  <circle cx="50" cy="50" r="30"/>
  <circle cx="150" cy="50" r="30"/>
</svg>
```

### Animación con `@keyframes` dentro del SVG

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <style>
    @keyframes pulso {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.4; }
    }
    .destacar { animation: pulso 1.5s ease-in-out infinite; }
  </style>
  <circle cx="50" cy="50" r="40" fill="red" class="destacar"/>
</svg>
```

### Modo oscuro automático

```svg
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <style>
    :root { --bg: white; --fg: #1e293b; }
    @media (prefers-color-scheme: dark) {
      :root { --bg: #1e293b; --fg: white; }
    }
    rect { fill: var(--bg); }
    text { fill: var(--fg); }
  </style>
  <rect width="100" height="100"/>
  <text x="50" y="55" text-anchor="middle">Auto</text>
</svg>
```

### `currentColor` para íconos heredables

```svg
<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
  <path fill="currentColor" d="..."/>
</svg>
```

```css
.icono { color: steelblue; }
.icono:hover { color: red; }
.icono.error { color: #ef4444; }
```

### CDATA para SVG standalone con CSS

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <style>
    <![CDATA[
      circle:hover > .activo { fill: red; }
      [data-estado="on"] { opacity: 1; }
    ]]>
  </style>
  <circle cx="50" cy="50" r="40" class="activo"/>
</svg>
```

### Resetear `transform-origin` al centro

```css
.elemento {
  transform: rotate(45deg);
  transform-origin: 50px 50px;  /* coordenadas del SVG */
  /* o: transform-origin: center center; */
}
```

---

## Checklist: SVG bien estilado

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="20" fill="#1e293b"/>
  <text x="150" y="14" text-anchor="middle" font-size="10" fill="#f8fafc">checklist antes de publicar</text>

  <rect x="15" y="28" width="270" height="200" fill="#0f172a" rx="3"/>
  <text x="25" y="46" font-size="9" fill="#34d399">✓ atributos de presentación (no style="...") en defaults</text>
  <text x="25" y="62" font-size="9" fill="#34d399">✓ currentColor o variables CSS para theming</text>
  <text x="25" y="78" font-size="9" fill="#34d399">✓ transform-origin definido si hay rotaciones</text>
  <text x="25" y="94" font-size="9" fill="#34d399">✓ media queries dentro del SVG si es standalone</text>
  <text x="25" y="110" font-size="9" fill="#34d399">✓ CDATA si es archivo .svg con CSS</text>
  <text x="25" y="126" font-size="9" fill="#34d399">✓ pointer-events: none en decoraciones</text>
  <text x="25" y="142" font-size="9" fill="#34d399">✓ :hover y :focus para interactividad básica</text>
  <text x="25" y="158" font-size="9" fill="#34d399">✓ prefers-reduced-motion respetado</text>
  <text x="25" y="174" font-size="9" fill="#34d399">✓ contrastes verificados en modo oscuro</text>
  <text x="25" y="190" font-size="9" fill="#34d399">✓ no !important salvo para overrides puntuales</text>
  <text x="25" y="206" font-size="9" fill="#34d399">✓ vector-effect: non-scaling-stroke si se escala</text>
  <text x="25" y="222" font-size="9" fill="#34d399">✓ probado en todos los métodos de integración previstos</text>
</svg>
```
