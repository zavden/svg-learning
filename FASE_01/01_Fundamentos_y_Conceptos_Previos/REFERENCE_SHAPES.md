# Referencia visual — Sección 01: Fundamentos

Cheat sheet visual de los conceptos clave de esta sección.

---

## SVG vs formatos raster — comparación rápida

```svg
<svg width="400" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cabecera -->
  <rect width="400" height="32" fill="#1e293b"/>
  <text x="100" y="21" text-anchor="middle" font-size="10" fill="#f1f5f9" font-weight="600">Propiedad</text>
  <text x="210" y="21" text-anchor="middle" font-size="10" fill="#f1f5f9" font-weight="600">SVG</text>
  <text x="315" y="21" text-anchor="middle" font-size="10" fill="#f1f5f9" font-weight="600">PNG / JPEG</text>
  <line x1="158" y1="0" x2="158" y2="200" stroke="#334155" stroke-width="1"/>
  <line x1="265" y1="0" x2="265" y2="200" stroke="#334155" stroke-width="1"/>

  <!-- Filas -->
  <rect x="0" y="32" width="400" height="24" fill="#eff6ff"/>
  <text x="100" y="48" text-anchor="middle" font-size="9" fill="#1e293b">Formato</text>
  <text x="210" y="48" text-anchor="middle" font-size="9" fill="#3b82f6" font-weight="500">Texto (XML)</text>
  <text x="315" y="48" text-anchor="middle" font-size="9" fill="#64748b">Binario</text>

  <rect x="0" y="56" width="400" height="24" fill="#f8fafc"/>
  <text x="100" y="72" text-anchor="middle" font-size="9" fill="#1e293b">Escalado</text>
  <text x="210" y="72" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">∞ sin pérdida</text>
  <text x="315" y="72" text-anchor="middle" font-size="9" fill="#ef4444">Limitado (pixela)</text>

  <rect x="0" y="80" width="400" height="24" fill="#eff6ff"/>
  <text x="100" y="96" text-anchor="middle" font-size="9" fill="#1e293b">Editable con texto</text>
  <text x="210" y="96" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">Sí</text>
  <text x="315" y="96" text-anchor="middle" font-size="9" fill="#ef4444">No</text>

  <rect x="0" y="104" width="400" height="24" fill="#f8fafc"/>
  <text x="100" y="120" text-anchor="middle" font-size="9" fill="#1e293b">Manipulable con JS/CSS</text>
  <text x="210" y="120" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">Sí (DOM)</text>
  <text x="315" y="120" text-anchor="middle" font-size="9" fill="#ef4444">No</text>

  <rect x="0" y="128" width="400" height="24" fill="#eff6ff"/>
  <text x="100" y="144" text-anchor="middle" font-size="9" fill="#1e293b">Fotografías</text>
  <text x="210" y="144" text-anchor="middle" font-size="9" fill="#ef4444">No (ineficiente)</text>
  <text x="315" y="144" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">Sí</text>

  <rect x="0" y="152" width="400" height="24" fill="#f8fafc"/>
  <text x="100" y="168" text-anchor="middle" font-size="9" fill="#1e293b">Versionable (git diff)</text>
  <text x="210" y="168" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">Sí (texto plano)</text>
  <text x="315" y="168" text-anchor="middle" font-size="9" fill="#ef4444">No (binario)</text>

  <rect x="0" y="176" width="400" height="24" fill="#eff6ff"/>
  <text x="100" y="192" text-anchor="middle" font-size="9" fill="#1e293b">Peso (gráfico simple)</text>
  <text x="210" y="192" text-anchor="middle" font-size="9" fill="#10b981" font-weight="600">Muy bajo (~500B)</text>
  <text x="315" y="192" text-anchor="middle" font-size="9" fill="#f59e0b">Mayor (~3–10KB)</text>
</svg>
```

---

## Reglas XML que SVG hereda

```svg
<svg width="400" height="220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="200" y="24" text-anchor="middle"
        font-size="11" fill="#f1f5f9" font-family="sans-serif" font-weight="600">
    Reglas XML obligatorias en SVG
  </text>

  <!-- Regla 1 -->
  <rect x="10" y="36" width="380" height="28" rx="4" fill="#0d2137"/>
  <text x="22" y="54" font-size="9" fill="#f59e0b">①</text>
  <text x="38" y="54" font-size="9" fill="#e2e8f0">Todo elemento debe cerrarse:</text>
  <text x="218" y="54" font-size="9" fill="#98c379">&lt;circle .../&gt;</text>
  <text x="290" y="54" font-size="9" fill="#abb2bf">o</text>
  <text x="304" y="54" font-size="9" fill="#98c379">&lt;g&gt;...&lt;/g&gt;</text>

  <!-- Regla 2 -->
  <rect x="10" y="70" width="380" height="28" rx="4" fill="#0d2137"/>
  <text x="22" y="88" font-size="9" fill="#f59e0b">②</text>
  <text x="38" y="88" font-size="9" fill="#e2e8f0">Atributos siempre entre comillas:</text>
  <text x="226" y="88" font-size="9" fill="#98c379">width="100"</text>
  <text x="295" y="88" font-size="9" fill="#abb2bf">o</text>
  <text x="309" y="88" font-size="9" fill="#98c379">width='100'</text>

  <!-- Regla 3 -->
  <rect x="10" y="104" width="380" height="28" rx="4" fill="#0d2137"/>
  <text x="22" y="122" font-size="9" fill="#f59e0b">③</text>
  <text x="38" y="122" font-size="9" fill="#e2e8f0">Case-sensitive:</text>
  <text x="130" y="122" font-size="9" fill="#98c379">linearGradient</text>
  <text x="222" y="122" font-size="9" fill="#ef4444">≠</text>
  <text x="234" y="122" font-size="9" fill="#f87171">lineargradient</text>

  <!-- Regla 4 -->
  <rect x="10" y="138" width="380" height="28" rx="4" fill="#0d2137"/>
  <text x="22" y="156" font-size="9" fill="#f59e0b">④</text>
  <text x="38" y="156" font-size="9" fill="#e2e8f0">Un único elemento raíz:</text>
  <text x="178" y="156" font-size="9" fill="#98c379">&lt;svg&gt;</text>
  <text x="216" y="156" font-size="9" fill="#abb2bf">— todo lo demás va dentro</text>

  <!-- Regla 5 -->
  <rect x="10" y="172" width="380" height="28" rx="4" fill="#0d2137"/>
  <text x="22" y="190" font-size="9" fill="#f59e0b">⑤</text>
  <text x="38" y="190" font-size="9" fill="#e2e8f0">Escapar caracteres especiales:</text>
  <text x="222" y="190" font-size="9" fill="#98c379">&amp; → &amp;amp;</text>
  <text x="280" y="190" font-size="9" fill="#98c379">&lt; → &amp;lt;</text>

  <!-- Nota -->
  <text x="200" y="214" text-anchor="middle"
        font-size="8" fill="#475569">
    Un error XML = el SVG no se renderiza en absoluto (sin mensaje amigable)
  </text>
</svg>
```

---

## Cuándo usar SVG — árbol de decisión

```svg
<svg width="340" height="260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Pregunta raíz -->
  <rect x="105" y="10" width="130" height="36" rx="8"
        fill="#1e293b"/>
  <text x="170" y="33" text-anchor="middle"
        font-size="10" fill="#f1f5f9" font-weight="600">¿Es una fotografía?</text>

  <!-- Ramas -->
  <line x1="170" y1="46" x2="80" y2="86" stroke="#94a3b8" stroke-width="1.5"/>
  <text x="105" y="74" font-size="8" fill="#ef4444">Sí</text>
  <line x1="170" y1="46" x2="260" y2="86" stroke="#94a3b8" stroke-width="1.5"/>
  <text x="235" y="74" font-size="8" fill="#10b981">No</text>

  <!-- Rama Sí → JPEG -->
  <rect x="20" y="86" width="120" height="36" rx="8"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"/>
  <text x="80" y="109" text-anchor="middle"
        font-size="10" fill="#991b1b" font-weight="600">JPEG / WebP</text>

  <!-- Rama No → segunda pregunta -->
  <rect x="200" y="86" width="130" height="36" rx="8"
        fill="#1e293b"/>
  <text x="265" y="109" text-anchor="middle"
        font-size="9" fill="#f1f5f9" font-weight="600">¿Necesita escalar?</text>

  <line x1="265" y1="122" x2="210" y2="158" stroke="#94a3b8" stroke-width="1.5"/>
  <text x="212" y="148" font-size="8" fill="#10b981">Sí</text>
  <line x1="265" y1="122" x2="310" y2="158" stroke="#94a3b8" stroke-width="1.5"/>
  <text x="302" y="148" font-size="8" fill="#f59e0b">No</text>

  <!-- SVG recomendado -->
  <rect x="155" y="158" width="100" height="36" rx="8"
        fill="#f0fdf4" stroke="#10b981" stroke-width="1.5"/>
  <text x="205" y="181" text-anchor="middle"
        font-size="11" fill="#166534" font-weight="700">SVG ✓</text>

  <!-- Depende -->
  <rect x="278" y="158" width="58" height="36" rx="8"
        fill="#fff7ed" stroke="#f59e0b" stroke-width="1.5"/>
  <text x="307" y="174" text-anchor="middle"
        font-size="8" fill="#92400e">SVG o</text>
  <text x="307" y="188" text-anchor="middle"
        font-size="8" fill="#92400e">PNG</text>

  <line x1="205" y1="194" x2="205" y2="214" stroke="#94a3b8" stroke-width="1.5"/>

  <!-- SVG usos -->
  <rect x="60" y="214" width="290" height="40" rx="8"
        fill="#eff6ff" stroke="#3b82f6" stroke-width="1"/>
  <text x="205" y="230" text-anchor="middle"
        font-size="8" fill="#1e40af">Iconos · Logos · Gráficos de datos</text>
  <text x="205" y="246" text-anchor="middle"
        font-size="8" fill="#1e40af">Decoración · Mapas · Animaciones UI</text>
</svg>
```

---

## Anatomía mínima de un SVG válido

```svg
<svg width="400" height="110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Elemento raíz -->
  <text x="16" y="28" font-size="10" fill="#e06c75">&lt;svg</text>
  <text x="56" y="28" font-size="10" fill="#d19a66">  width</text>
  <text x="107" y="28" font-size="10" fill="#56b6c2">=</text>
  <text x="116" y="28" font-size="10" fill="#98c379">"200"</text>
  <text x="157" y="28" font-size="10" fill="#d19a66">height</text>
  <text x="202" y="28" font-size="10" fill="#56b6c2">=</text>
  <text x="211" y="28" font-size="10" fill="#98c379">"100"</text>

  <text x="56" y="46" font-size="10" fill="#d19a66">  xmlns</text>
  <text x="105" y="46" font-size="10" fill="#56b6c2">=</text>
  <text x="114" y="46" font-size="10" fill="#98c379">"http://www.w3.org/2000/svg"</text>
  <text x="56" y="62" font-size="10" fill="#56b6c2">&gt;</text>

  <!-- Elemento hijo -->
  <text x="32" y="78" font-size="10" fill="#e06c75">  &lt;circle</text>
  <text x="97" y="78" font-size="10" fill="#d19a66">cx</text>
  <text x="114" y="78" font-size="10" fill="#56b6c2">=</text>
  <text x="122" y="78" font-size="10" fill="#98c379">"100"</text>
  <text x="160" y="78" font-size="10" fill="#d19a66">cy</text>
  <text x="177" y="78" font-size="10" fill="#56b6c2">=</text>
  <text x="185" y="78" font-size="10" fill="#98c379">"50"</text>
  <text x="220" y="78" font-size="10" fill="#d19a66">r</text>
  <text x="230" y="78" font-size="10" fill="#56b6c2">=</text>
  <text x="238" y="78" font-size="10" fill="#98c379">"30"</text>
  <text x="278" y="78" font-size="10" fill="#56b6c2">/&gt;</text>

  <!-- Cierre raíz -->
  <text x="16" y="96" font-size="10" fill="#e06c75">&lt;/svg&gt;</text>
</svg>
```
