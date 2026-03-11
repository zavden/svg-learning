# 1.5 XML como base de SVG

SVG hereda la sintaxis de XML. Esto significa que hay reglas estrictas que, si se violan, hacen que el SVG no se renderice en absoluto — sin advertencias amigables.

---

## Regla 1: Todo elemento debe cerrarse

En HTML puedes escribir `<li>` sin cerrarlo. En XML/SVG, todos los elementos deben cerrarse: con etiqueta de cierre o con auto-cierre `/>`.

```svg
<svg width="300" height="130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- ✓ Auto-cierre (para elementos sin hijos) -->
  <text x="16" y="30" font-size="9" fill="#10b981">✓ Correcto — auto-cierre:</text>
  <text x="16" y="48" font-size="9" fill="#98c379">
    &lt;circle cx="50" cy="50" r="20" /&gt;
  </text>
  <text x="16" y="64" font-size="9" fill="#98c379">
    &lt;rect x="10" y="10" width="80" /&gt;
  </text>

  <!-- ✓ Etiqueta de cierre (para elementos con hijos) -->
  <text x="16" y="84" font-size="9" fill="#10b981">✓ Correcto — etiqueta de cierre:</text>
  <text x="16" y="100" font-size="9" fill="#98c379">
    &lt;g&gt; ... &lt;/g&gt;
  </text>

  <!-- ✗ Incorrecto -->
  <text x="16" y="118" font-size="9" fill="#ef4444">
    ✗ &lt;rect x="10"&gt;    ← sin cerrar: el SVG no se renderiza
  </text>
</svg>
```

---

## Regla 2: Atributos siempre entre comillas

HTML5 permite `width=100`. XML/SVG no: las comillas son **obligatorias** siempre.

```svg
<svg width="300" height="120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="26" font-size="9" fill="#10b981">✓ Con comillas dobles:</text>
  <text x="16" y="44" font-size="9" fill="#98c379">  width="100"  fill="#3b82f6"</text>

  <text x="16" y="62" font-size="9" fill="#10b981">✓ Con comillas simples (también válido):</text>
  <text x="16" y="80" font-size="9" fill="#98c379">  width='100'  fill='#3b82f6'</text>

  <text x="16" y="98" font-size="9" fill="#ef4444">✗ Sin comillas (HTML5 lo perdona; SVG/XML no):</text>
  <text x="16" y="114" font-size="9" fill="#f87171">  width=100   fill=#3b82f6</text>
</svg>
```

---

## Regla 3: Case-sensitive

XML distingue mayúsculas y minúsculas. `<linearGradient>` existe; `<lineargradient>` no.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="26" font-size="9" fill="#10b981">✓ Correcto (camelCase exacto):</text>
  <text x="16" y="44" font-size="9" fill="#98c379">  &lt;linearGradient&gt;</text>
  <text x="16" y="60" font-size="9" fill="#98c379">  &lt;feGaussianBlur&gt;</text>
  <text x="16" y="76" font-size="9" fill="#98c379">  &lt;textPath&gt;</text>

  <text x="16" y="96" font-size="9" fill="#ef4444">✗ Incorrecto (no existen estos elementos):</text>
  <text x="16" y="112" font-size="9" fill="#f87171">  &lt;LineaRgradient&gt;</text>
  <text x="16" y="126" font-size="9" fill="#f87171">  &lt;Textpath&gt;</text>
</svg>
```

---

## Regla 4: Caracteres especiales se escapan

Si el contenido de texto contiene `<`, `>`, `&` o `"`, deben escaparse con entidades XML.

```svg
<svg width="300" height="150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="24" font-size="9" fill="#64748b">Entidades XML más usadas en SVG:</text>

  <!-- Tabla de entidades -->
  <text x="16"  y="48" font-size="9" fill="#d19a66">&amp;amp;</text>
  <text x="80"  y="48" font-size="9" fill="#abb2bf">→</text>
  <text x="96"  y="48" font-size="9" fill="#98c379">&amp;</text>
  <text x="140" y="48" font-size="9" fill="#64748b">(ampersand)</text>

  <text x="16"  y="66" font-size="9" fill="#d19a66">&amp;lt;</text>
  <text x="80"  y="66" font-size="9" fill="#abb2bf">→</text>
  <text x="96"  y="66" font-size="9" fill="#98c379">&lt;</text>
  <text x="140" y="66" font-size="9" fill="#64748b">(menor que)</text>

  <text x="16"  y="84" font-size="9" fill="#d19a66">&amp;gt;</text>
  <text x="80"  y="84" font-size="9" fill="#abb2bf">→</text>
  <text x="96"  y="84" font-size="9" fill="#98c379">&gt;</text>
  <text x="140" y="84" font-size="9" fill="#64748b">(mayor que)</text>

  <text x="16"  y="102" font-size="9" fill="#d19a66">&amp;quot;</text>
  <text x="80"  y="102" font-size="9" fill="#abb2bf">→</text>
  <text x="96"  y="102" font-size="9" fill="#98c379">"</text>
  <text x="140" y="102" font-size="9" fill="#64748b">(comilla doble)</text>

  <text x="16"  y="120" font-size="9" fill="#d19a66">&amp;apos;</text>
  <text x="80"  y="120" font-size="9" fill="#abb2bf">→</text>
  <text x="96"  y="120" font-size="9" fill="#98c379">'</text>
  <text x="140" y="120" font-size="9" fill="#64748b">(comilla simple)</text>

  <text x="16"  y="140" font-size="8" fill="#475569">
    En atributos style="" el &amp; se escribe como &amp;amp; también
  </text>
</svg>
```

---

## El namespace xmlns

El namespace le dice al parser qué vocabulario XML se está usando. En archivos `.svg` independientes es **obligatorio**. En SVG inline dentro de HTML5 puede omitirse (el browser lo infiere).

```svg
<svg width="300" height="150"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <text x="16" y="26" font-size="9" fill="#6a9955">
    &lt;!-- archivo.svg externo — xmlns obligatorio --&gt;
  </text>
  <text x="16" y="44" font-size="9" fill="#98c379">
    &lt;svg
  </text>
  <text x="50" y="44" font-size="9" fill="#d19a66">
    xmlns
  </text>
  <text x="86" y="44" font-size="9" fill="#56b6c2">
    =
  </text>
  <text x="96" y="44" font-size="9" fill="#98c379">
    "http://www.w3.org/2000/svg"
  </text>
  <text x="50" y="60" font-size="9" fill="#d19a66">width</text>
  <text x="90" y="60" font-size="9" fill="#56b6c2">=</text>
  <text x="100" y="60" font-size="9" fill="#98c379">"200"</text>
  <text x="50" y="76" font-size="9" fill="#d19a66">height</text>
  <text x="92" y="76" font-size="9" fill="#56b6c2">=</text>
  <text x="102" y="76" font-size="9" fill="#98c379">"100"</text>
  <text x="50" y="92" font-size="9" fill="#56b6c2">&gt;</text>

  <line x1="16" y1="102" x2="284" y2="102"
        stroke="#334155" stroke-width="1"/>

  <text x="16" y="116" font-size="9" fill="#6a9955">
    &lt;!-- SVG inline en HTML5 — xmlns opcional --&gt;
  </text>
  <text x="16" y="132" font-size="9" fill="#abb2bf">
    &lt;!DOCTYPE html&gt; → el browser ya sabe que es SVG
  </text>
  <text x="16" y="146" font-size="9" fill="#abb2bf">
    pero incluirlo no hace daño y mejora la portabilidad
  </text>
</svg>
```

---

## SVG bien formado vs SVG roto

Un SVG con errores de XML no muestra nada. El navegador no hace "best effort" como con HTML — lo rechaza en silencio.

```svg
<svg width="300" height="140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block;">

  <!-- Bien formado (izquierda) -->
  <rect x="10" y="10" width="130" height="120" rx="6"
        fill="#f0fdf4" stroke="#10b981" stroke-width="1.5"/>
  <text x="75" y="30" text-anchor="middle"
        font-size="9" fill="#166534" font-family="sans-serif" font-weight="600">
    SVG bien formado
  </text>
  <circle cx="75" cy="80" r="34" fill="#10b981" opacity="0.9"/>
  <text x="75" y="85" text-anchor="middle"
        font-size="18" fill="white" font-family="sans-serif" font-weight="900">
    OK
  </text>
  <text x="75" y="122" text-anchor="middle"
        font-size="8" fill="#166534" font-family="sans-serif">
    Se renderiza correctamente
  </text>

  <!-- Roto (derecha) -->
  <rect x="160" y="10" width="130" height="120" rx="6"
        fill="#fef2f2" stroke="#ef4444" stroke-width="1.5"/>
  <text x="225" y="30" text-anchor="middle"
        font-size="9" fill="#991b1b" font-family="sans-serif" font-weight="600">
    SVG con error XML
  </text>
  <text x="225" y="55" text-anchor="middle"
        font-size="28" fill="#ef4444" font-family="sans-serif">✗</text>
  <text x="225" y="82" text-anchor="middle"
        font-size="8" fill="#991b1b" font-family="sans-serif">elemento sin cerrar</text>
  <text x="225" y="96" text-anchor="middle"
        font-size="8" fill="#991b1b" font-family="sans-serif">atributo sin comillas</text>
  <text x="225" y="110" text-anchor="middle"
        font-size="8" fill="#991b1b" font-family="sans-serif">tag con typo</text>
  <text x="225" y="122" text-anchor="middle"
        font-size="8" fill="#991b1b" font-family="sans-serif">
    No se renderiza nada
  </text>
</svg>
```
