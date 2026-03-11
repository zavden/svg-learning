# 6.6 Limitaciones del texto en SVG

El texto SVG no tiene word-wrap, no tiene párrafos, y depende de fuentes disponibles en el sistema.

---

## Sin flujo automático: el texto no se rompe

```svg
<svg width="300" height="90"
     viewBox="0 0 300 90"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- El texto simplemente se sale del viewport -->
  <rect x="5" y="5" width="290" height="70" rx="4" fill="#fef2f2" stroke="#fecaca" stroke-width="1"/>
  <text x="20" y="35" font-size="13" fill="#ef4444">
    Este texto es demasiado largo y no se rompe automáticamente
  </text>
  <!-- El texto se sale del contenedor -->
  <text x="20" y="55" font-size="8" fill="#94a3b8">
    SVG no tiene word-wrap — el texto sale del viewport
  </text>
  <text x="20" y="68" font-size="7.5" fill="#64748b">
    Hay que partir el texto manualmente
  </text>
</svg>
```

---

## Opción 1: múltiples `<text>` (simple, verboso)

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Cada línea es un elemento <text> independiente -->
  <text x="20" y="22" font-size="13" fill="#1e293b">Primera línea del texto</text>
  <text x="20" y="40" font-size="13" fill="#1e293b">Segunda línea del texto</text>
  <text x="20" y="58" font-size="13" fill="#1e293b">Tercera línea del texto</text>

  <text x="20" y="82" font-size="7.5" fill="#64748b">Spacing manual: y=22, y=40, y=58 (delta=18)</text>
  <text x="20" y="95" font-size="7" fill="#94a3b8">Simple pero hay que calcular el y de cada línea</text>
</svg>
```

---

## Opción 2: `<tspan dy>` (recomendado para texto relacionado)

```svg
<svg width="300" height="100"
     viewBox="0 0 300 100"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Un solo <text> con tspan para cada línea -->
  <text x="20" y="22" font-size="13" fill="#1e293b">
    <tspan>Primera línea del texto</tspan>
    <tspan x="20" dy="1.4em">Segunda línea del texto</tspan>
    <tspan x="20" dy="1.4em">Tercera línea del texto</tspan>
  </text>

  <text x="20" y="82" font-size="7.5" fill="#10b981">Mejor: dy="1.4em" → interlineado relativo ✓</text>
  <text x="20" y="95" font-size="7" fill="#94a3b8">Si cambia el font-size, el interlineado se ajusta</text>
</svg>
```

---

## Opción 3: `<foreignObject>` con HTML (máxima flexibilidad)

```svg
<svg width="300" height="120"
     viewBox="0 0 300 120"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- foreignObject incrusta HTML dentro del SVG -->
  <rect x="10" y="10" width="280" height="95" rx="6" fill="#f1f5f9"/>
  <foreignObject x="20" y="18" width="260" height="80">
    <div xmlns="http://www.w3.org/1999/xhtml"
         style="font-size:12px; color:#1e293b; font-family:sans-serif; line-height:1.5;">
      Este texto usa HTML nativo con word-wrap automático.
      Se rompe al llegar al borde del contenedor.
    </div>
  </foreignObject>

  <text x="150" y="115" text-anchor="middle" font-size="7.5" fill="#64748b">
    foreignObject: word-wrap nativo, pero no funciona en SVG como &lt;img&gt;
  </text>
</svg>
```

---

## Dependencia de fuentes

Si la fuente no está instalada en el sistema del usuario, el navegador usa un fallback — con métricas diferentes.

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <!-- Texto con fuente específica vs fuente genérica -->
  <text x="20" y="30" font-size="16" font-family="'Helvetica Neue', Arial, sans-serif" fill="#1e293b">
    Hello World
  </text>
  <text x="20" y="48" font-size="7.5" fill="#64748b">font-family con fallbacks (buena práctica)</text>

  <!-- La solución: @font-face en <style> -->
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter');
    /* En uso real: embeber la fuente en base64 o usar una fuente web */
  </style>

  <rect x="10" y="58" width="280" height="45" rx="4" fill="#fef9c3" stroke="#fde68a" stroke-width="1"/>
  <text x="20" y="73" font-size="8" fill="#92400e" font-weight="700">Opciones para independencia de fuentes:</text>
  <text x="20" y="87" font-size="7.5" fill="#78350f">1. @font-face con fuente embebida (base64) en &lt;style&gt;</text>
  <text x="20" y="100" font-size="7.5" fill="#78350f">2. Convertir texto a paths (Inkscape: Object → Path)</text>
</svg>
```

---

## Texto como path: cuándo convertir

```svg
<svg width="300" height="110"
     viewBox="0 0 300 110"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="22" fill="#1e293b"/>
  <text x="150" y="15" text-anchor="middle" font-size="8.5" fill="white">¿Texto SVG o texto como path?</text>

  <text x="10" y="38" font-size="8" fill="#3b82f6" font-weight="700">Mantener como texto SVG</text>
  <text x="10" y="52" font-size="7" fill="#64748b">• Contenido informativo (etiquetas, datos)</text>
  <text x="10" y="65" font-size="7" fill="#64748b">• Texto seleccionable y accesible importante</text>
  <text x="10" y="78" font-size="7" fill="#64748b">• Fuentes genéricas (sans-serif, serif)</text>

  <line x1="155" y1="22" x2="155" y2="110" stroke="#e2e8f0" stroke-width="1"/>

  <text x="165" y="38" font-size="8" fill="#f59e0b" font-weight="700">Convertir a path</text>
  <text x="165" y="52" font-size="7" fill="#64748b">• Logos con tipografía especial</text>
  <text x="165" y="65" font-size="7" fill="#64748b">• Fuentes que el usuario puede no tener</text>
  <text x="165" y="78" font-size="7" fill="#64748b">• SVG para descarga/impresión offline</text>
  <text x="165" y="91" font-size="7" fill="#ef4444">⚠ pierde selección y accesibilidad</text>
</svg>
```
