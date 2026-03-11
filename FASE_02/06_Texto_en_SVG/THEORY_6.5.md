# 6.5 Texto en trazados: `<textPath>`

`<textPath>` hace que el texto siga la curva de un `<path>`. El texto se "dobla" siguiendo la geometría del trazado.

---

## Estructura básica

`<textPath>` debe ser hijo de `<text>` y referenciar un path por su `id`.

```svg
<svg width="300" height="130"
     viewBox="0 0 300 130"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <!-- El path que el texto seguirá -->
    <path id="curva-demo" d="M 20 100 C 80 20, 220 20, 280 100"/>
  </defs>

  <!-- El path visible (para contexto) -->
  <use href="#curva-demo" stroke="#e2e8f0" stroke-width="1" fill="none"/>

  <!-- Texto que sigue el path -->
  <text font-size="13" fill="#3b82f6">
    <textPath href="#curva-demo">
      El texto sigue la curva del path
    </textPath>
  </text>

  <text x="150" y="125" text-anchor="middle" font-size="7" fill="#94a3b8">
    textPath href="#id-del-path"
  </text>
</svg>
```

---

## `startOffset` — dónde empieza el texto

```svg
<svg width="300" height="140"
     viewBox="0 0 300 140"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <defs>
    <path id="linea-offset" d="M 20 60 H 280"/>
  </defs>

  <!-- Línea base de referencia -->
  <use href="#linea-offset" stroke="#e2e8f0" stroke-width="1" fill="none"/>

  <!-- startOffset=0% -->
  <text font-size="11" fill="#3b82f6">
    <textPath href="#linea-offset" startOffset="0%">startOffset=0%</textPath>
  </text>

  <defs>
    <path id="linea-offset2" d="M 20 100 H 280"/>
  </defs>
  <use href="#linea-offset2" stroke="#e2e8f0" stroke-width="1" fill="none"/>

  <!-- startOffset=50% + text-anchor=middle → centrado -->
  <text font-size="11" fill="#10b981" text-anchor="middle">
    <textPath href="#linea-offset2" startOffset="50%">centrado en el path</textPath>
  </text>

  <text x="150" y="130" text-anchor="middle" font-size="7.5" fill="#64748b">
    startOffset="50%" + text-anchor="middle" → centrado
  </text>
</svg>
```

---

## Texto en un círculo

Para texto que rodea un círculo, se define el path del círculo en `<defs>`.

```svg
<svg width="300" height="160"
     viewBox="0 0 300 160"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#0f172a;display:block; font-family:sans-serif;">

  <defs>
    <!-- Círculo superior: va de derecha hacia arriba (texto en la parte superior) -->
    <path id="circulo-top"
          d="M 90 80 m -65 0 a 65 65 0 1 1 130 0"/>
    <!-- Círculo inferior: va de izquierda hacia abajo (texto en la parte inferior) -->
    <path id="circulo-bottom"
          d="M 90 80 m -65 0 a 65 65 0 1 0 130 0"/>
  </defs>

  <!-- Círculo decorativo -->
  <circle cx="90" cy="80" r="65" fill="none" stroke="#1e3a5f" stroke-width="2"/>
  <circle cx="90" cy="80" r="50" fill="#0f172a"/>
  <!-- Contenido central -->
  <text x="90" y="84" text-anchor="middle" dominant-baseline="middle"
        font-size="18" fill="#f59e0b" font-weight="700">★</text>
  <text x="90" y="100" text-anchor="middle" font-size="9" fill="#64748b">PREMIUM</text>

  <!-- Texto en la parte superior del círculo -->
  <text font-size="10" fill="#60a5fa" letter-spacing="3">
    <textPath href="#circulo-top" startOffset="50%" text-anchor="middle">
      MIEMBRO OFICIAL
    </textPath>
  </text>

  <!-- Texto en la parte inferior del círculo -->
  <text font-size="9" fill="#94a3b8" letter-spacing="2">
    <textPath href="#circulo-bottom" startOffset="50%" text-anchor="middle">
      SVG AVANZADO
    </textPath>
  </text>

  <!-- Segundo ejemplo: path en defs (invisible) -->
  <defs>
    <path id="onda-texto" d="M 175 80 C 195 50, 225 110, 245 80 S 275 50, 290 80"/>
  </defs>
  <text font-size="9" fill="#f59e0b">
    <textPath href="#onda-texto">texto en onda</textPath>
  </text>
  <text x="230" y="140" text-anchor="middle" font-size="7" fill="#475569">path invisible en defs</text>
</svg>
```

---

## Limitaciones de `<textPath>`

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">Limitaciones de &lt;textPath&gt;</text>

  <text x="10" y="38" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="38" font-size="7.5" fill="#64748b">El texto NO se ajusta si es más largo que el path — se recorta</text>

  <text x="10" y="54" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="54" font-size="7.5" fill="#64748b">No hay "segunda línea" sobre un path curvo</text>

  <text x="10" y="70" font-size="7.5" fill="#ef4444">✗</text>
  <text x="22" y="70" font-size="7.5" fill="#64748b">La línea base del texto siempre descansa sobre el path</text>

  <text x="10" y="86" font-size="7.5" fill="#10b981">✓</text>
  <text x="22" y="86" font-size="7.5" fill="#64748b">El path puede estar en &lt;defs&gt; y ser invisible</text>
</svg>
```
