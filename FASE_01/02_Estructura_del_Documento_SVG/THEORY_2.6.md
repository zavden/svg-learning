# 2.6 Estructura mínima de un SVG válido

Conocer qué partes son obligatorias, cuáles se recomiendan y cuáles dependen del contexto permite escribir SVGs correctos y accesibles desde el primer intento.

---

## La versión mínima absoluta

Para experimentar rápidamente, solo necesitas el elemento raíz con `xmlns` y `viewBox`.

```svg
<svg width="300" height="90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Mínimo para un archivo .svg externo -->
  <text x="16" y="26" font-size="9" fill="#e06c75">&lt;svg</text>
  <text x="50" y="26" font-size="9" fill="#d19a66">xmlns</text>
  <text x="86" y="26" font-size="9" fill="#56b6c2">=</text>
  <text x="94" y="26" font-size="9" fill="#98c379">"http://www.w3.org/2000/svg"</text>

  <text x="50" y="44" font-size="9" fill="#d19a66">viewBox</text>
  <text x="92" y="44" font-size="9" fill="#56b6c2">=</text>
  <text x="100" y="44" font-size="9" fill="#98c379">"0 0 100 100"</text>
  <text x="176" y="44" font-size="9" fill="#56b6c2">&gt;</text>

  <text x="32" y="62" font-size="9" fill="#5c6370" font-style="italic">
    &lt;!-- contenido --&gt;
  </text>

  <text x="16" y="80" font-size="9" fill="#e06c75">&lt;/svg&gt;</text>
</svg>
```

---

## La versión recomendada para producción

Para un SVG accesible y correcto en producción, cada parte tiene su función.

```svg
<svg width="340" height="260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:monospace;">

  <!-- Prólogo (opcional en .svg, omitir en inline) -->
  <text x="16" y="24" font-size="8" fill="#56b6c2">
    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
  </text>
  <text x="220" y="24" font-size="7" fill="#5c6370" font-style="italic">← solo en .svg</text>

  <!-- Elemento raíz -->
  <text x="16" y="42" font-size="9" fill="#e06c75">&lt;svg</text>

  <text x="50" y="58" font-size="9" fill="#d19a66">xmlns</text>
  <text x="86" y="58" font-size="9" fill="#98c379">="http://www.w3.org/2000/svg"</text>
  <text x="260" y="58" font-size="7" fill="#5c6370" font-style="italic">← namespace</text>

  <text x="50" y="74" font-size="9" fill="#d19a66">viewBox</text>
  <text x="92" y="74" font-size="9" fill="#98c379">="0 0 200 100"</text>
  <text x="260" y="74" font-size="7" fill="#5c6370" font-style="italic">← coordenadas</text>

  <text x="50" y="90" font-size="9" fill="#d19a66">width</text>
  <text x="82" y="90" font-size="9" fill="#98c379">="200"</text>
  <text x="130" y="90" font-size="9" fill="#d19a66">height</text>
  <text x="170" y="90" font-size="9" fill="#98c379">="100"</text>
  <text x="260" y="90" font-size="7" fill="#5c6370" font-style="italic">← viewport</text>

  <text x="50" y="106" font-size="9" fill="#d19a66">role</text>
  <text x="78" y="106" font-size="9" fill="#98c379">="img"</text>
  <text x="260" y="106" font-size="7" fill="#5c6370" font-style="italic">← ARIA</text>

  <text x="50" y="122" font-size="9" fill="#d19a66">aria-labelledby</text>
  <text x="158" y="122" font-size="9" fill="#98c379">="titulo desc"</text>
  <text x="260" y="122" font-size="7" fill="#5c6370" font-style="italic">← accesibilidad</text>

  <text x="50" y="138" font-size="9" fill="#56b6c2">&gt;</text>

  <!-- title -->
  <text x="32" y="156" font-size="9" fill="#e06c75">&lt;title</text>
  <text x="72" y="156" font-size="9" fill="#d19a66">id</text>
  <text x="84" y="156" font-size="9" fill="#56b6c2">=</text>
  <text x="90" y="156" font-size="9" fill="#98c379">"titulo"</text>
  <text x="136" y="156" font-size="9" fill="#56b6c2">&gt;</text>
  <text x="148" y="156" font-size="9" fill="#abb2bf">Nombre del gráfico</text>
  <text x="260" y="156" font-size="9" fill="#e06c75">&lt;/title&gt;</text>

  <!-- desc -->
  <text x="32" y="172" font-size="9" fill="#e06c75">&lt;desc</text>
  <text x="68" y="172" font-size="9" fill="#d19a66">id</text>
  <text x="80" y="172" font-size="9" fill="#56b6c2">=</text>
  <text x="86" y="172" font-size="9" fill="#98c379">"desc"</text>
  <text x="122" y="172" font-size="9" fill="#56b6c2">&gt;</text>
  <text x="134" y="172" font-size="9" fill="#abb2bf">Descripción larga</text>
  <text x="248" y="172" font-size="9" fill="#e06c75">&lt;/desc&gt;</text>

  <!-- defs -->
  <text x="32" y="190" font-size="9" fill="#e06c75">&lt;defs&gt;</text>
  <text x="78" y="190" font-size="9" fill="#5c6370" font-style="italic">...recursos...</text>
  <text x="168" y="190" font-size="9" fill="#e06c75">&lt;/defs&gt;</text>

  <!-- contenido -->
  <text x="32" y="208" font-size="9" fill="#5c6370" font-style="italic">
    &lt;!-- contenido gráfico --&gt;
  </text>

  <!-- cierre -->
  <text x="16" y="226" font-size="9" fill="#e06c75">&lt;/svg&gt;</text>

  <!-- Nota -->
  <text x="170" y="248" text-anchor="middle"
        font-size="7" fill="#475569">
    Plantilla recomendada para SVGs accesibles en producción
  </text>
</svg>
```

---

## Variantes según contexto de uso

La estructura varía según si el SVG es decorativo, un ícono o un gráfico complejo.

```svg
<svg width="340" height="200"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Decorativo -->
  <rect x="8" y="8" width="100" height="180" rx="8"
        fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>
  <text x="58" y="26" text-anchor="middle" font-size="8" fill="#64748b" font-weight="600">
    Decorativo
  </text>
  <rect x="16" y="34" width="84" height="60" rx="4" fill="#0f172a"/>
  <text x="58" y="50" text-anchor="middle" font-size="6" fill="#abb2bf" font-family="monospace">
    &lt;svg
  </text>
  <text x="58" y="62" text-anchor="middle" font-size="6" fill="#d19a66" font-family="monospace">
    aria-hidden
  </text>
  <text x="58" y="74" text-anchor="middle" font-size="6" fill="#98c379" font-family="monospace">
    ="true"
  </text>
  <text x="58" y="86" text-anchor="middle" font-size="6" fill="#56b6c2" font-family="monospace">
    &gt;
  </text>
  <text x="58" y="112" text-anchor="middle" font-size="8" fill="#64748b">Sin &lt;title&gt;</text>
  <text x="58" y="124" text-anchor="middle" font-size="8" fill="#64748b">AT lo ignoran</text>
  <text x="58" y="140" text-anchor="middle" font-size="8" fill="#94a3b8">fondos, olas,</text>
  <text x="58" y="152" text-anchor="middle" font-size="8" fill="#94a3b8">separadores</text>

  <!-- Ícono -->
  <rect x="120" y="8" width="100" height="180" rx="8"
        fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>
  <text x="170" y="26" text-anchor="middle" font-size="8" fill="#64748b" font-weight="600">
    Ícono con acción
  </text>
  <rect x="128" y="34" width="84" height="70" rx="4" fill="#0f172a"/>
  <text x="170" y="50" text-anchor="middle" font-size="6" fill="#abb2bf" font-family="monospace">
    &lt;svg
  </text>
  <text x="170" y="62" text-anchor="middle" font-size="6" fill="#d19a66" font-family="monospace">
    role="img"
  </text>
  <text x="170" y="74" text-anchor="middle" font-size="6" fill="#d19a66" font-family="monospace">
    aria-label
  </text>
  <text x="170" y="86" text-anchor="middle" font-size="6" fill="#98c379" font-family="monospace">
    ="Cerrar"
  </text>
  <text x="170" y="98" text-anchor="middle" font-size="6" fill="#56b6c2" font-family="monospace">
    &gt;
  </text>
  <text x="170" y="124" text-anchor="middle" font-size="8" fill="#64748b">aria-label</text>
  <text x="170" y="136" text-anchor="middle" font-size="8" fill="#64748b">directo</text>
  <text x="170" y="152" text-anchor="middle" font-size="8" fill="#94a3b8">botones,</text>
  <text x="170" y="164" text-anchor="middle" font-size="8" fill="#94a3b8">links con ícono</text>

  <!-- Gráfico complejo -->
  <rect x="232" y="8" width="100" height="180" rx="8"
        fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1"/>
  <text x="282" y="26" text-anchor="middle" font-size="8" fill="#64748b" font-weight="600">
    Gráfico complejo
  </text>
  <rect x="240" y="34" width="84" height="90" rx="4" fill="#0f172a"/>
  <text x="282" y="50" text-anchor="middle" font-size="6" fill="#abb2bf" font-family="monospace">
    &lt;svg
  </text>
  <text x="282" y="62" text-anchor="middle" font-size="6" fill="#d19a66" font-family="monospace">
    role="img"
  </text>
  <text x="282" y="74" text-anchor="middle" font-size="6" fill="#d19a66" font-family="monospace">
    aria-labelledby
  </text>
  <text x="282" y="86" text-anchor="middle" font-size="6" fill="#98c379" font-family="monospace">
    ="t d"&gt;
  </text>
  <text x="282" y="100" text-anchor="middle" font-size="6" fill="#e06c75" font-family="monospace">
    &lt;title id="t"&gt;
  </text>
  <text x="282" y="112" text-anchor="middle" font-size="6" fill="#e06c75" font-family="monospace">
    &lt;desc id="d"&gt;
  </text>
  <text x="282" y="140" text-anchor="middle" font-size="8" fill="#64748b">title + desc</text>
  <text x="282" y="152" text-anchor="middle" font-size="8" fill="#64748b">completos</text>
  <text x="282" y="168" text-anchor="middle" font-size="8" fill="#94a3b8">charts, mapas,</text>
  <text x="282" y="180" text-anchor="middle" font-size="8" fill="#94a3b8">diagramas</text>
</svg>
```

---

## Qué incluir y qué omitir — tabla rápida

```svg
<svg width="340" height="220"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cabecera -->
  <rect width="340" height="30" fill="#1e293b"/>
  <text x="100" y="20" text-anchor="middle" font-size="9" fill="#f1f5f9" font-weight="600">Parte del SVG</text>
  <text x="210" y="20" text-anchor="middle" font-size="9" fill="#f1f5f9" font-weight="600">¿Obligatoria?</text>
  <text x="300" y="20" text-anchor="middle" font-size="9" fill="#f1f5f9" font-weight="600">Incluir si...</text>
  <line x1="160" y1="0" x2="160" y2="220" stroke="#334155" stroke-width="1"/>
  <line x1="260" y1="0" x2="260" y2="220" stroke="#334155" stroke-width="1"/>

  <rect x="0" y="30" width="340" height="24" fill="#eff6ff"/>
  <text x="100" y="47" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">xmlns</text>
  <text x="210" y="47" text-anchor="middle" font-size="8" fill="#f59e0b">Solo en .svg</text>
  <text x="300" y="47" text-anchor="middle" font-size="8" fill="#64748b">Siempre en .svg</text>

  <rect x="0" y="54" width="340" height="24" fill="#f8fafc"/>
  <text x="100" y="71" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">viewBox</text>
  <text x="210" y="71" text-anchor="middle" font-size="8" fill="#10b981">Recomendada</text>
  <text x="300" y="71" text-anchor="middle" font-size="8" fill="#64748b">Siempre</text>

  <rect x="0" y="78" width="340" height="24" fill="#eff6ff"/>
  <text x="100" y="95" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">width/height</text>
  <text x="210" y="95" text-anchor="middle" font-size="8" fill="#f59e0b">Contexto</text>
  <text x="300" y="95" text-anchor="middle" font-size="8" fill="#64748b">Tamaño fijo</text>

  <rect x="0" y="102" width="340" height="24" fill="#f8fafc"/>
  <text x="100" y="119" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">role="img"</text>
  <text x="210" y="119" text-anchor="middle" font-size="8" fill="#10b981">Recomendada</text>
  <text x="300" y="95" text-anchor="middle" font-size="8" fill="#64748b">Tamaño fijo</text>
  <text x="300" y="119" text-anchor="middle" font-size="8" fill="#64748b">SVG con info</text>

  <rect x="0" y="126" width="340" height="24" fill="#eff6ff"/>
  <text x="100" y="143" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;title&gt;</text>
  <text x="210" y="143" text-anchor="middle" font-size="8" fill="#10b981">Recomendada</text>
  <text x="300" y="143" text-anchor="middle" font-size="8" fill="#64748b">SVG con info</text>

  <rect x="0" y="150" width="340" height="24" fill="#f8fafc"/>
  <text x="100" y="167" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;desc&gt;</text>
  <text x="210" y="167" text-anchor="middle" font-size="8" fill="#f59e0b">Opcional</text>
  <text x="300" y="167" text-anchor="middle" font-size="8" fill="#64748b">SVG complejo</text>

  <rect x="0" y="174" width="340" height="24" fill="#eff6ff"/>
  <text x="100" y="191" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;defs&gt;</text>
  <text x="210" y="191" text-anchor="middle" font-size="8" fill="#f59e0b">Opcional</text>
  <text x="300" y="191" text-anchor="middle" font-size="8" fill="#64748b">Hay reutilizables</text>

  <rect x="0" y="198" width="340" height="22" fill="#f8fafc"/>
  <text x="100" y="213" text-anchor="middle" font-size="8" fill="#1e293b" font-family="monospace">&lt;?xml?&gt; / DOCTYPE</text>
  <text x="210" y="213" text-anchor="middle" font-size="8" fill="#ef4444">No recomendado</text>
  <text x="300" y="213" text-anchor="middle" font-size="8" fill="#64748b">Solo .svg legacy</text>
</svg>
```
