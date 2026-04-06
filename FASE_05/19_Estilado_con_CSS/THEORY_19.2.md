# 19.2 Prioridad de estilos en la cascada SVG

La cascada SVG sigue la misma lógica que CSS, pero con un giro **contraintuitivo**: los atributos de presentación tienen **menor** prioridad que cualquier regla CSS, no mayor. Entender esto cambia cómo diseñas SVGs reutilizables.

---

## La pirámide de prioridad

```svg
<svg width="300" height="320"
     viewBox="0 0 300 320"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cascada SVG (de menor a mayor)
  </text>

  <!-- Nivel 1 -->
  <rect x="20" y="36" width="260" height="32" fill="#f1f5f9" stroke="#94a3b8" rx="3"/>
  <text x="30" y="52" font-size="8" font-weight="bold" fill="#475569">1. Valor inicial del navegador</text>
  <text x="30" y="62" font-size="7" fill="#64748b">defaults SVG (fill="black", stroke="none"…)</text>

  <!-- Nivel 2 -->
  <rect x="20" y="74" width="260" height="32" fill="#fef3c7" stroke="#f59e0b" rx="3"/>
  <text x="30" y="90" font-size="8" font-weight="bold" fill="#92400e">2. Herencia del padre</text>
  <text x="30" y="100" font-size="7" fill="#475569">fill, font-*, color heredables</text>

  <!-- Nivel 3 -->
  <rect x="20" y="112" width="260" height="36" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="30" y="128" font-size="8" font-weight="bold" fill="#1e40af">3. Atributos de presentación</text>
  <text x="30" y="140" font-size="7" font-family="monospace" fill="#475569">&lt;circle fill="red" stroke="blue"/&gt;</text>

  <!-- Nivel 4 -->
  <rect x="20" y="154" width="260" height="36" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="30" y="170" font-size="8" font-weight="bold" fill="#166534">4. CSS hoja de estilos</text>
  <text x="30" y="182" font-size="7" font-family="monospace" fill="#475569">circle { fill: green; }  /* gana al atributo */</text>

  <!-- Nivel 5 -->
  <rect x="20" y="196" width="260" height="36" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="30" y="212" font-size="8" font-weight="bold" fill="#5b21b6">5. Estilo inline</text>
  <text x="30" y="224" font-size="7" font-family="monospace" fill="#475569">style="fill: purple;"</text>

  <!-- Nivel 6 -->
  <rect x="20" y="238" width="260" height="36" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="30" y="254" font-size="8" font-weight="bold" fill="#991b1b">6. !important (lo más alto)</text>
  <text x="30" y="266" font-size="7" font-family="monospace" fill="#475569">fill: black !important;</text>

  <!-- Flecha -->
  <line x1="290" y1="40" x2="290" y2="270" stroke="#1e293b" stroke-width="1.5" marker-end="url(#flechaAbajo)"/>
  <defs>
    <marker id="flechaAbajo" markerWidth="6" markerHeight="6" refX="3" refY="5" orient="auto">
      <path d="M0,0 L6,0 L3,6 z" fill="#1e293b"/>
    </marker>
  </defs>
  <text x="295" y="155" font-size="6" fill="#475569" transform="rotate(90 295 155)">prioridad creciente</text>

  <text x="150" y="295" text-anchor="middle" font-size="7" fill="#94a3b8">los niveles 4-6 son los mismos que en HTML/CSS</text>
  <text x="150" y="307" text-anchor="middle" font-size="7" fill="#94a3b8">el nivel 3 es la peculiaridad de SVG</text>
</svg>
```

---

## El punto contraintuitivo

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    "atributo &lt; CSS" — el círculo es ROJO
  </text>

  <!-- Demo visual -->
  <circle cx="80" cy="80" r="32" fill="#ef4444" stroke="#1e293b" stroke-width="2"/>
  <text x="80" y="125" text-anchor="middle" font-size="8" fill="#475569">resultado</text>

  <!-- Código -->
  <rect x="130" y="38" width="160" height="92" fill="#0f172a" rx="3"/>
  <text x="140" y="54" font-size="7" font-family="monospace" fill="#94a3b8">&lt;!-- HTML --&gt;</text>
  <text x="140" y="68" font-size="7" font-family="monospace" fill="#60a5fa">&lt;circle fill="blue"</text>
  <text x="140" y="80" font-size="7" font-family="monospace" fill="#60a5fa">  class="mi-clase" /&gt;</text>
  <text x="140" y="98" font-size="7" font-family="monospace" fill="#94a3b8">/* CSS */</text>
  <text x="140" y="112" font-size="7" font-family="monospace" fill="#34d399">.mi-clase {</text>
  <text x="140" y="124" font-size="7" font-family="monospace" fill="#34d399">  fill: red; /* gana */</text>

  <!-- Por qué importa -->
  <rect x="15" y="142" width="270" height="86" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="158" font-size="9" font-weight="bold" fill="#166534">Por qué importa</text>
  <text x="25" y="174" font-size="7" fill="#475569">• puedes "tematizar" un SVG de Figma sin tocarlo</text>
  <text x="25" y="188" font-size="7" fill="#475569">• los íconos exportados de editores son CSS-friendly</text>
  <text x="25" y="202" font-size="7" fill="#475569">• un solo archivo SVG sirve para muchos contextos</text>
  <text x="25" y="216" font-size="7" fill="#475569">• el SVG fuente puede tener "valores por defecto"</text>
</svg>
```

---

## La trampa del estilo inline

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    style="..." rompe la tematización
  </text>

  <!-- Mal -->
  <rect x="15" y="38" width="270" height="80" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">✗ Editor exporta con style="..."</text>
  <text x="25" y="68" font-size="7" font-family="monospace" fill="#475569">&lt;circle style="fill: #005ea5;" /&gt;</text>
  <text x="25" y="82" font-size="7" font-family="monospace" fill="#475569">/* CSS */</text>
  <text x="25" y="94" font-size="7" font-family="monospace" fill="#475569">circle { fill: red; }  /* IGNORADO */</text>
  <text x="25" y="108" font-size="7" fill="#991b1b">solo un !important puede sobrescribirlo</text>

  <!-- Bien -->
  <rect x="15" y="126" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="142" font-size="9" font-weight="bold" fill="#166534">✓ Configura el exportador para usar atributos</text>
  <text x="25" y="158" font-size="7" font-family="monospace" fill="#475569">&lt;circle fill="#005ea5" /&gt;</text>
  <text x="25" y="172" font-size="7" font-family="monospace" fill="#475569">/* CSS */</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#475569">circle { fill: red; }  /* gana */</text>
  <text x="25" y="200" font-size="7" fill="#475569">Figma: "Use presentation attributes"</text>
  <text x="25" y="214" font-size="7" fill="#475569">SVGO: { removeStyleElement: false } y similares</text>
</svg>
```

---

## Especificidad CSS (igual que en HTML)

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    especificidad entre selectores CSS
  </text>

  <!-- Tabla -->
  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Selector</text>
  <text x="160" y="46" font-size="7" font-weight="bold" fill="#1e293b">Especificidad</text>
  <text x="240" y="46" font-size="7" font-weight="bold" fill="#1e293b">Gana</text>

  <rect x="10" y="52" width="280" height="20" fill="white"/>
  <text x="20" y="65" font-size="7" font-family="monospace" fill="#3b82f6">circle</text>
  <text x="160" y="65" font-size="7" fill="#475569">(0,0,1)</text>
  <text x="240" y="65" font-size="7" fill="#94a3b8">—</text>

  <rect x="10" y="72" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="85" font-size="7" font-family="monospace" fill="#3b82f6">.activo</text>
  <text x="160" y="85" font-size="7" fill="#475569">(0,1,0)</text>
  <text x="240" y="85" font-size="7" fill="#10b981">&gt; tipo</text>

  <rect x="10" y="92" width="280" height="20" fill="white"/>
  <text x="20" y="105" font-size="7" font-family="monospace" fill="#3b82f6">.grupo circle</text>
  <text x="160" y="105" font-size="7" fill="#475569">(0,1,1)</text>
  <text x="240" y="105" font-size="7" fill="#10b981">&gt; clase sola</text>

  <rect x="10" y="112" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="125" font-size="7" font-family="monospace" fill="#3b82f6">.act.dest</text>
  <text x="160" y="125" font-size="7" fill="#475569">(0,2,0)</text>
  <text x="240" y="125" font-size="7" fill="#10b981">&gt; descendiente</text>

  <rect x="10" y="132" width="280" height="20" fill="white"/>
  <text x="20" y="145" font-size="7" font-family="monospace" fill="#3b82f6">#principal</text>
  <text x="160" y="145" font-size="7" fill="#475569">(1,0,0)</text>
  <text x="240" y="145" font-size="7" fill="#10b981">&gt; cualquier clase</text>

  <rect x="10" y="152" width="280" height="20" fill="#f8fafc"/>
  <text x="20" y="165" font-size="7" font-family="monospace" fill="#3b82f6">style="..."</text>
  <text x="160" y="165" font-size="7" fill="#475569">(1,0,0,0)</text>
  <text x="240" y="165" font-size="7" fill="#10b981">&gt; #id</text>

  <rect x="10" y="172" width="280" height="20" fill="white"/>
  <text x="20" y="185" font-size="7" font-family="monospace" fill="#3b82f6">!important</text>
  <text x="160" y="185" font-size="7" fill="#475569">∞</text>
  <text x="240" y="185" font-size="7" fill="#ef4444">&gt; todo</text>

  <text x="150" y="215" text-anchor="middle" font-size="7" fill="#94a3b8">los atributos de presentación quedan FUERA de esta tabla</text>
  <text x="150" y="227" text-anchor="middle" font-size="7" fill="#94a3b8">cualquier regla CSS los supera, sin importar el selector</text>
</svg>
```

---

## Estrategia práctica

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    elegir nivel de "fuerza" según objetivo
  </text>

  <rect x="10" y="32" width="280" height="20" fill="#e2e8f0"/>
  <text x="20" y="46" font-size="7" font-weight="bold" fill="#1e293b">Objetivo</text>
  <text x="160" y="46" font-size="7" font-weight="bold" fill="#1e293b">Técnica</text>

  <rect x="10" y="52" width="280" height="22" fill="white"/>
  <text x="20" y="66" font-size="7" fill="#475569">SVG de editor, valores base</text>
  <text x="160" y="66" font-size="7" font-family="monospace" fill="#3b82f6">atributos presentación</text>

  <rect x="10" y="74" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="88" font-size="7" fill="#475569">Sobrescribir desde fuera</text>
  <text x="160" y="88" font-size="7" font-family="monospace" fill="#10b981">selector CSS</text>

  <rect x="10" y="96" width="280" height="22" fill="white"/>
  <text x="20" y="110" font-size="7" fill="#475569">Estilo "blindado", no sobrescribible</text>
  <text x="160" y="110" font-size="7" font-family="monospace" fill="#8b5cf6">style="..." inline</text>

  <rect x="10" y="118" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="132" font-size="7" fill="#475569">Tematización dinámica</text>
  <text x="160" y="132" font-size="7" font-family="monospace" fill="#10b981">--variables CSS</text>

  <rect x="10" y="140" width="280" height="22" fill="white"/>
  <text x="20" y="154" font-size="7" fill="#475569">Heredar color del padre</text>
  <text x="160" y="154" font-size="7" font-family="monospace" fill="#3b82f6">currentColor</text>

  <rect x="10" y="162" width="280" height="22" fill="#f8fafc"/>
  <text x="20" y="176" font-size="7" fill="#475569">Override de un default específico</text>
  <text x="160" y="176" font-size="7" font-family="monospace" fill="#ef4444">!important (último recurso)</text>

  <text x="150" y="208" text-anchor="middle" font-size="7" fill="#94a3b8">la cascada es tu amiga si la diseñas en capas:</text>
  <text x="150" y="220" text-anchor="middle" font-size="7" fill="#94a3b8">defaults (atributos) → tema (CSS) → estado (clases)</text>
</svg>
```
