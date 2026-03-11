# Referencia rápida — Formas Básicas

---

## Cheat sheet: las 6 formas

```svg
<svg width="300" height="210"
     viewBox="0 0 300 210"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <!-- Título -->
  <text x="150" y="16" text-anchor="middle" font-size="9" fill="#94a3b8">SVG — 6 formas primitivas</text>

  <!-- rect -->
  <rect x="15" y="25" width="70" height="50" fill="#3b82f6" rx="6"/>
  <text x="50" y="88" text-anchor="middle" font-size="8" fill="#60a5fa" font-family="monospace">&lt;rect&gt;</text>
  <text x="50" y="99" text-anchor="middle" font-size="6.5" fill="#475569">x y w h rx ry</text>

  <!-- circle -->
  <circle cx="160" cy="50" r="28" fill="#10b981"/>
  <text x="160" y="88" text-anchor="middle" font-size="8" fill="#34d399" font-family="monospace">&lt;circle&gt;</text>
  <text x="160" y="99" text-anchor="middle" font-size="6.5" fill="#475569">cx cy r</text>

  <!-- ellipse -->
  <ellipse cx="255" cy="50" rx="38" ry="22" fill="#a855f7"/>
  <text x="255" y="88" text-anchor="middle" font-size="8" fill="#c084fc" font-family="monospace">&lt;ellipse&gt;</text>
  <text x="255" y="99" text-anchor="middle" font-size="6.5" fill="#475569">cx cy rx ry</text>

  <!-- line -->
  <line x1="15" y1="155" x2="85" y2="125"
        stroke="#f59e0b" stroke-width="3" stroke-linecap="round"/>
  <text x="50" y="175" text-anchor="middle" font-size="8" fill="#fbbf24" font-family="monospace">&lt;line&gt;</text>
  <text x="50" y="186" text-anchor="middle" font-size="6.5" fill="#475569">x1 y1 x2 y2</text>
  <text x="50" y="196" text-anchor="middle" font-size="6" fill="#374151">necesita stroke</text>

  <!-- polyline -->
  <polyline points="110,155 130,120 155,145 175,115"
            stroke="#ef4444" stroke-width="2.5" fill="none" stroke-linejoin="round"/>
  <text x="142" y="175" text-anchor="middle" font-size="8" fill="#f87171" font-family="monospace">&lt;polyline&gt;</text>
  <text x="142" y="186" text-anchor="middle" font-size="6.5" fill="#475569">points="x,y ..."</text>
  <text x="142" y="196" text-anchor="middle" font-size="6" fill="#374151">abierta — fill=none</text>

  <!-- polygon -->
  <polygon points="237,155 255,115 273,155"
           fill="#6366f1" stroke="#818cf8" stroke-width="1.5"/>
  <text x="255" y="175" text-anchor="middle" font-size="8" fill="#818cf8" font-family="monospace">&lt;polygon&gt;</text>
  <text x="255" y="186" text-anchor="middle" font-size="6.5" fill="#475569">points="x,y ..."</text>
  <text x="255" y="196" text-anchor="middle" font-size="6" fill="#374151">cerrada automática</text>
</svg>
```

---

## Atributos de presentación más usados

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cabecera tabla -->
  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="75"  y="15" text-anchor="middle" font-size="8.5" fill="#94a3b8">Atributo</text>
  <text x="165" y="15" text-anchor="middle" font-size="8.5" fill="#94a3b8">Valores típicos</text>
  <text x="255" y="15" text-anchor="middle" font-size="8.5" fill="#94a3b8">Hereda</text>

  <!-- Filas -->
  <!-- fill -->
  <rect x="0" y="22" width="300" height="22" fill="#f8fafc"/>
  <text x="75"  y="36" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">fill</text>
  <text x="165" y="36" text-anchor="middle" font-size="7.5" fill="#475569">color / none / url(#id)</text>
  <text x="255" y="36" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- stroke -->
  <rect x="0" y="44" width="300" height="22" fill="#f1f5f9"/>
  <text x="75"  y="58" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">stroke</text>
  <text x="165" y="58" text-anchor="middle" font-size="7.5" fill="#475569">color / none</text>
  <text x="255" y="58" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- stroke-width -->
  <rect x="0" y="66" width="300" height="22" fill="#f8fafc"/>
  <text x="75"  y="80" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">stroke-width</text>
  <text x="165" y="80" text-anchor="middle" font-size="7.5" fill="#475569">número (px) — default 1</text>
  <text x="255" y="80" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- opacity -->
  <rect x="0" y="88" width="300" height="22" fill="#f1f5f9"/>
  <text x="75"  y="102" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">opacity</text>
  <text x="165" y="102" text-anchor="middle" font-size="7.5" fill="#475569">0–1 — fill + stroke</text>
  <text x="255" y="102" text-anchor="middle" font-size="8" fill="#ef4444">no*</text>

  <!-- fill-opacity -->
  <rect x="0" y="110" width="300" height="22" fill="#f8fafc"/>
  <text x="75"  y="124" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">fill-opacity</text>
  <text x="165" y="124" text-anchor="middle" font-size="7.5" fill="#475569">0–1 — solo relleno</text>
  <text x="255" y="124" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- stroke-opacity -->
  <rect x="0" y="132" width="300" height="22" fill="#f1f5f9"/>
  <text x="75"  y="146" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">stroke-opacity</text>
  <text x="165" y="146" text-anchor="middle" font-size="7.5" fill="#475569">0–1 — solo trazo</text>
  <text x="255" y="146" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- stroke-linecap -->
  <rect x="0" y="154" width="300" height="22" fill="#f8fafc"/>
  <text x="75"  y="168" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">stroke-linecap</text>
  <text x="165" y="168" text-anchor="middle" font-size="7.5" fill="#475569">butt | round | square</text>
  <text x="255" y="168" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- stroke-dasharray -->
  <rect x="0" y="176" width="300" height="22" fill="#f1f5f9"/>
  <text x="75"  y="190" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">stroke-dasharray</text>
  <text x="165" y="190" text-anchor="middle" font-size="7.5" fill="#475569">"8,4" | "4,4,2,4"</text>
  <text x="255" y="190" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- fill-rule -->
  <rect x="0" y="198" width="300" height="22" fill="#f8fafc"/>
  <text x="75"  y="212" text-anchor="middle" font-size="8" fill="#3b82f6" font-family="monospace">fill-rule</text>
  <text x="165" y="212" text-anchor="middle" font-size="7.5" fill="#475569">nonzero | evenodd</text>
  <text x="255" y="212" text-anchor="middle" font-size="8" fill="#10b981">sí</text>

  <!-- Nota opacity -->
  <text x="150" y="232" text-anchor="middle" font-size="7" fill="#94a3b8">
    * opacity se hereda visualmente (efecto multiplicado) pero no como valor CSS
  </text>
</svg>
```

---

## Árbol de decisión: ¿qué forma usar?

```svg
<svg width="300" height="220"
     viewBox="0 0 300 220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- ¿Qué forma necesitas? -->
  <rect x="90" y="5" width="120" height="22" rx="4" fill="#1e293b"/>
  <text x="150" y="20" text-anchor="middle" font-size="8" fill="white">¿Qué forma necesitas?</text>

  <!-- Rectángulo -->
  <rect x="10" y="45" width="80" height="18" rx="3" fill="#dbeafe"/>
  <text x="50" y="57" text-anchor="middle" font-size="7.5" fill="#1e40af">lados rectos</text>
  <line x1="110" y1="27" x2="50" y2="45" stroke="#94a3b8" stroke-width="1"/>
  <rect x="5" y="70" width="90" height="15" rx="2" fill="#3b82f6"/>
  <text x="50" y="81" text-anchor="middle" font-size="7" fill="white">&lt;rect&gt;</text>

  <!-- Solo una línea -->
  <rect x="110" y="45" width="80" height="18" rx="3" fill="#dcfce7"/>
  <text x="150" y="57" text-anchor="middle" font-size="7.5" fill="#166534">solo una línea</text>
  <line x1="150" y1="27" x2="150" y2="45" stroke="#94a3b8" stroke-width="1"/>
  <rect x="105" y="70" width="90" height="15" rx="2" fill="#10b981"/>
  <text x="150" y="81" text-anchor="middle" font-size="7" fill="white">&lt;line&gt;</text>

  <!-- Curvo -->
  <rect x="210" y="45" width="80" height="18" rx="3" fill="#f3e8ff"/>
  <text x="250" y="57" text-anchor="middle" font-size="7.5" fill="#6b21a8">curvo / redondo</text>
  <line x1="190" y1="27" x2="250" y2="45" stroke="#94a3b8" stroke-width="1"/>

  <!-- ¿Círculo o elipse? -->
  <rect x="195" y="70" width="95" height="15" rx="2" fill="#ede9fe"/>
  <text x="242" y="81" text-anchor="middle" font-size="7.5" fill="#5b21b6">¿radios iguales?</text>

  <rect x="195" y="95" width="42" height="14" rx="2" fill="#a855f7"/>
  <text x="216" y="105" text-anchor="middle" font-size="7" fill="white">&lt;circle&gt;</text>
  <text x="216" y="118" text-anchor="middle" font-size="6.5" fill="#94a3b8">rx = ry</text>

  <rect x="248" y="95" width="42" height="14" rx="2" fill="#7c3aed"/>
  <text x="269" y="105" text-anchor="middle" font-size="7" fill="white">&lt;ellipse&gt;</text>
  <text x="269" y="118" text-anchor="middle" font-size="6.5" fill="#94a3b8">rx ≠ ry</text>

  <line x1="242" y1="85" x2="216" y2="95" stroke="#94a3b8" stroke-width="1"/>
  <line x1="242" y1="85" x2="269" y2="95" stroke="#94a3b8" stroke-width="1"/>

  <!-- Vértices múltiples -->
  <line x1="150" y1="27" x2="90" y2="130" stroke="#94a3b8" stroke-width="1" stroke-dasharray="2,2"/>

  <rect x="5" y="130" width="175" height="18" rx="3" fill="#fef9c3"/>
  <text x="92" y="142" text-anchor="middle" font-size="7.5" fill="#854d0e">múltiples vértices</text>

  <!-- ¿cerrada? -->
  <rect x="5" y="158" width="80" height="14" rx="2" fill="#f59e0b"/>
  <text x="45" y="168" text-anchor="middle" font-size="7" fill="white">&lt;polygon&gt;</text>
  <text x="45" y="181" text-anchor="middle" font-size="6.5" fill="#94a3b8">figura cerrada</text>
  <line x1="92" y1="148" x2="45" y2="158" stroke="#94a3b8" stroke-width="1"/>

  <rect x="100" y="158" width="80" height="14" rx="2" fill="#ef4444"/>
  <text x="140" y="168" text-anchor="middle" font-size="7" fill="white">&lt;polyline&gt;</text>
  <text x="140" y="181" text-anchor="middle" font-size="6.5" fill="#94a3b8">línea abierta</text>
  <line x1="92" y1="148" x2="140" y2="158" stroke="#94a3b8" stroke-width="1"/>

  <!-- Nota curvas -->
  <text x="150" y="210" text-anchor="middle" font-size="7.5" fill="#64748b">
    ¿Curvas libres? → &lt;path&gt; (sección 05)
  </text>
</svg>
```
