# 25.7 Checklist antes de publicar

Antes de hacer merge de un SVG a producción, pasa por esta lista. No todos los puntos aplican a todos los SVGs, pero si sabes por qué ignoras uno, ya estás haciendo las cosas bien.

---

## Estructura y escalabilidad

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    esqueleto correcto
  </text>

  <text x="20" y="54" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="54" font-size="8" fill="#475569">viewBox definido cubriendo todo el contenido</text>

  <text x="20" y="76" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="76" font-size="8" fill="#475569">sin width/height fijos si debe ser responsive</text>

  <text x="20" y="98" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="98" font-size="8" fill="#475569">nada del contenido se sale del viewBox</text>

  <text x="20" y="120" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="120" font-size="8" fill="#475569">IDs válidos (no empiezan por número, sin espacios)</text>

  <text x="20" y="142" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="142" font-size="8" fill="#475569">xmlns="http://www.w3.org/2000/svg" presente</text>

  <text x="20" y="164" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="164" font-size="8" fill="#475569">&lt;defs&gt; antes que el contenido que referencia</text>

  <text x="20" y="186" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="186" font-size="8" fill="#475569">orden del DOM = z-order visual deseado</text>

  <text x="20" y="208" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="208" font-size="8" fill="#475569">coordenadas redondas, no 49.73 ni 51.2</text>

  <text x="20" y="230" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="230" font-size="8" fill="#475569">sin grupos vacíos ni grupos de 1 hijo sin attrs</text>
</svg>
```

---

## Accesibilidad

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    accesible por defecto
  </text>

  <text x="20" y="54" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="54" font-size="8" fill="#475569">decorativo: aria-hidden="true" + focusable="false"</text>

  <text x="20" y="76" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="76" font-size="8" fill="#475569">con significado: role="img" + &lt;title&gt;</text>

  <text x="20" y="98" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="98" font-size="8" fill="#475569">aria-labelledby apunta a id de &lt;title&gt;</text>

  <text x="20" y="120" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="120" font-size="8" fill="#475569">icono en botón sin texto → aria-label en botón</text>

  <text x="20" y="142" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="142" font-size="8" fill="#475569">contraste WCAG AA (4.5:1 texto, 3:1 UI)</text>

  <text x="20" y="164" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="164" font-size="8" fill="#475569">no depender solo del color para transmitir info</text>

  <text x="20" y="186" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="186" font-size="8" fill="#475569">prefers-reduced-motion respetado en animaciones</text>

  <text x="20" y="208" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="208" font-size="8" fill="#475569">gráficos de datos: tabla equivalente disponible</text>
</svg>
```

---

## Optimización y colores

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    pasado por SVGO, tematizable
  </text>

  <text x="20" y="54" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="54" font-size="8" fill="#475569">procesado con SVGO (config versionada)</text>

  <text x="20" y="76" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="76" font-size="8" fill="#475569">sin metadatos de editor (inkscape:, illustrator:)</text>

  <text x="20" y="98" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="98" font-size="8" fill="#475569">decimales 1-2 para iconos, max 3 para ilustraciones</text>

  <text x="20" y="120" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="120" font-size="8" fill="#475569">currentColor en iconos que heredan del texto</text>

  <text x="20" y="142" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="142" font-size="8" fill="#475569">variables CSS si el SVG necesita tematización</text>

  <text x="20" y="164" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="164" font-size="8" fill="#475569">sin style="..." inline que bloquee CSS externo</text>

  <text x="20" y="186" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="186" font-size="8" fill="#475569">IDs conservados si son referenciados desde JS/CSS</text>

  <text x="20" y="208" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="208" font-size="8" fill="#475569">removeViewBox desactivado (nunca se borra)</text>

  <text x="20" y="230" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="230" font-size="8" fill="#475569">servidor con gzip/brotli para image/svg+xml</text>
</svg>
```

---

## Seguridad y rendimiento

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    seguro y eficiente
  </text>

  <text x="20" y="54" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="54" font-size="8" fill="#475569">si viene de fuera: sanitizado (DOMPurify)</text>

  <text x="20" y="76" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="76" font-size="8" fill="#475569">sin &lt;script&gt;, sin onload/onclick, sin foreignObject</text>

  <text x="20" y="98" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="98" font-size="8" fill="#475569">servido con Content-Type: image/svg+xml correcto</text>

  <text x="20" y="120" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="120" font-size="8" fill="#475569">nodos SVG &lt; 1.000 para uso general</text>

  <text x="20" y="142" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="142" font-size="8" fill="#475569">filtros caros (blur, turbulence) no se animan</text>

  <text x="20" y="164" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="164" font-size="8" fill="#475569">animaciones con transform y opacity (GPU)</text>

  <text x="20" y="186" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="186" font-size="8" fill="#475569">will-change solo durante animación activa</text>

  <text x="20" y="208" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="208" font-size="8" fill="#475569">lazy-loading en SVGs pesados fuera del viewport</text>

  <text x="20" y="230" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="230" font-size="8" fill="#475569">medir LCP/FPS antes de dar por bueno</text>
</svg>
```

---

## Compatibilidad e integración

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    funciona donde debe
  </text>

  <text x="20" y="54" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="54" font-size="8" fill="#475569">probado en Chrome, Firefox, Safari mínimo</text>

  <text x="20" y="76" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="76" font-size="8" fill="#475569">IE11 si aplica: xlink:href + width/height explícitos</text>

  <text x="20" y="98" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="98" font-size="8" fill="#475569">sprite externo: sin problemas de CORS</text>

  <text x="20" y="120" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="120" font-size="8" fill="#475569">método de integración correcto para el caso</text>

  <text x="20" y="142" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="142" font-size="8" fill="#475569">inline: sin conflictos de ID con otros SVGs</text>

  <text x="20" y="164" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="164" font-size="8" fill="#475569">si &lt;img&gt;: accept que los scripts no corren (bien)</text>

  <text x="20" y="186" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="186" font-size="8" fill="#475569">Cache-Control coherente (inmutable si nombre hasheado)</text>

  <text x="20" y="208" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="208" font-size="8" fill="#475569">bundle analyzer verifica que entró solo lo usado</text>

  <text x="20" y="230" font-size="10" fill="#10b981">✓</text>
  <text x="38" y="230" font-size="8" fill="#475569">test visual automatizado si forma parte del sistema</text>
</svg>
```
