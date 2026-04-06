# 23.4 Compresión

SVG es texto plano lleno de patrones repetidos (`<path`, `fill`, `stroke`…), lo que lo hace **un candidato ideal** para los algoritmos de compresión como gzip o brotli. Optimizar con SVGO reduce el tamaño "en bruto"; la compresión HTTP reduce el tamaño **en la red**. Las dos se acumulan: SVGO + gzip pueden llevar un archivo de 10KB a 1.5KB.

---

## Por qué SVG comprime tan bien

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    texto + patrones = oro para gzip
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Qué repite un SVG</text>
  <text x="25" y="70" font-size="7" fill="#475569">• etiquetas: &lt;path, &lt;rect, &lt;g, &lt;/g&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">• atributos: fill=, stroke=, d=, x=, y=</text>
  <text x="25" y="98" font-size="7" fill="#475569">• comandos de path: M, L, C, Z</text>
  <text x="25" y="112" font-size="7" fill="#475569">• valores de color: #3b82f6, currentColor</text>
  <text x="25" y="126" font-size="7" fill="#1e40af">→ gzip reemplaza cada patrón por un alias</text>

  <rect x="15" y="146" width="270" height="96" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Reducción típica</text>
  <text x="25" y="178" font-size="7" fill="#475569">• raw:        10 KB</text>
  <text x="25" y="192" font-size="7" fill="#475569">• + SVGO:      3 KB  (-70%)</text>
  <text x="25" y="206" font-size="7" fill="#475569">• + gzip:      1 KB  (-90%)</text>
  <text x="25" y="220" font-size="7" fill="#475569">• + brotli:   0.8 KB (-92%)</text>
  <text x="25" y="234" font-size="7" fill="#166534">las dos compresiones son acumulativas</text>
</svg>
```

---

## Configurar gzip en el servidor

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    image/svg+xml debe comprimirse
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8"># nginx</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">gzip on;</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">gzip_types image/svg+xml</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">           text/css</text>
  <text x="25" y="112" font-size="8" font-family="monospace" fill="#60a5fa">           application/javascript;</text>

  <rect x="15" y="132" width="270" height="46" fill="#0f172a" rx="3"/>
  <text x="25" y="148" font-size="7" font-family="monospace" fill="#94a3b8"># Apache (.htaccess)</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#34d399">AddOutputFilterByType DEFLATE image/svg+xml</text>

  <rect x="15" y="188" width="270" height="60" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="204" font-size="9" font-weight="bold" fill="#92400e">Vite / Webpack / Next.js</text>
  <text x="25" y="220" font-size="7" fill="#475569">ya viene activada en producción por defecto</text>
  <text x="25" y="232" font-size="7" fill="#475569">los assets estáticos se sirven comprimidos</text>
</svg>
```

---

## El formato `.svgz`

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    un .svg comprimido de fábrica
  </text>

  <rect x="15" y="38" width="270" height="72" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Qué es</text>
  <text x="25" y="70" font-size="7" fill="#475569">SVGZ = un archivo .svg ya comprimido con gzip</text>
  <text x="25" y="84" font-size="7" fill="#475569">se guarda en disco con extensión .svgz</text>
  <text x="25" y="98" font-size="7" fill="#475569">el navegador lo descomprime al vuelo</text>

  <rect x="15" y="118" width="270" height="70" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="134" font-size="9" font-weight="bold" fill="#166534">Cómo generarlo</text>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#475569">gzip -9 -k icono.svg</text>
  <text x="25" y="164" font-size="7" font-family="monospace" fill="#475569">mv icono.svg.gz icono.svgz</text>
  <text x="25" y="180" font-size="7" fill="#166534">→ un archivo pequeño listo para servir</text>

  <rect x="15" y="196" width="270" height="76" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="212" font-size="9" font-weight="bold" fill="#92400e">Cabeceras que debe enviar el servidor</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#475569">Content-Type: image/svg+xml</text>
  <text x="25" y="242" font-size="7" font-family="monospace" fill="#475569">Content-Encoding: gzip</text>
  <text x="25" y="258" font-size="7" fill="#92400e">sin Content-Encoding, el navegador no lo descomprime</text>
</svg>
```

---

## El error de la doble compresión

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    comprimir dos veces = archivo mayor
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">El error típico</text>
  <text x="25" y="70" font-size="7" fill="#475569">tu servidor tiene gzip on para todos los assets</text>
  <text x="25" y="84" font-size="7" fill="#475569">subes un .svgz (ya comprimido)</text>
  <text x="25" y="98" font-size="7" fill="#475569">nginx vuelve a aplicar gzip encima</text>
  <text x="25" y="114" font-size="7" fill="#991b1b">→ gzip(gzip(x)) suele ser MÁS grande que gzip(x)</text>

  <rect x="15" y="138" width="270" height="130" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Soluciones</text>
  <text x="25" y="170" font-size="7" fill="#475569">• no usar .svgz — dejar que el servidor comprima</text>
  <text x="25" y="184" font-size="7" fill="#475569">• o excluir .svgz del gzip del servidor:</text>
  <text x="25" y="204" font-size="7" font-family="monospace" fill="#475569">location ~* \.svgz$ {</text>
  <text x="25" y="218" font-size="7" font-family="monospace" fill="#475569">  add_header Content-Encoding gzip;</text>
  <text x="25" y="232" font-size="7" font-family="monospace" fill="#475569">  gzip off;</text>
  <text x="25" y="246" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="262" font-size="7" fill="#166534">→ en la práctica moderna: olvida .svgz</text>
</svg>
```

---

## Verificar que comprime

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    cómo comprobar en producción
  </text>

  <rect x="15" y="38" width="270" height="94" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="7" font-family="monospace" fill="#94a3b8"># con curl</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#60a5fa">$ curl -sI -H "Accept-Encoding: gzip" \</text>
  <text x="25" y="84" font-size="8" font-family="monospace" fill="#60a5fa">       https://tu-sitio.com/icono.svg \</text>
  <text x="25" y="98" font-size="8" font-family="monospace" fill="#60a5fa">  | grep -i encoding</text>
  <text x="25" y="118" font-size="7" font-family="monospace" fill="#34d399">Content-Encoding: gzip   ✓</text>

  <rect x="15" y="146" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#1e40af">En DevTools (pestaña Network)</text>
  <text x="25" y="178" font-size="7" fill="#475569">clic en la request → Headers → Response</text>
  <text x="25" y="192" font-size="7" fill="#475569">busca Content-Encoding: gzip o br</text>
  <text x="25" y="208" font-size="7" fill="#475569">en "Size" verás dos números:</text>
  <text x="25" y="220" font-size="7" font-family="monospace" fill="#475569">  1.2 kB / 4.8 kB  (red / descomprimido)</text>
  <text x="25" y="236" font-size="7" fill="#1e40af">si son iguales, el servidor no está comprimiendo</text>
</svg>
```

---

## Brotli: un paso más allá

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    brotli &gt; gzip cuando está disponible
  </text>

  <rect x="15" y="38" width="270" height="90" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">Qué aporta</text>
  <text x="25" y="70" font-size="7" fill="#475569">• ratio mejor que gzip (~10-15% menos bytes)</text>
  <text x="25" y="84" font-size="7" fill="#475569">• diccionario precompilado para HTML/CSS/JS/SVG</text>
  <text x="25" y="98" font-size="7" fill="#475569">• soportado por todos los navegadores modernos</text>
  <text x="25" y="112" font-size="7" fill="#1e40af">Content-Encoding: br</text>

  <rect x="15" y="138" width="270" height="108" fill="#0f172a" rx="3"/>
  <text x="25" y="154" font-size="7" font-family="monospace" fill="#94a3b8"># nginx con módulo brotli</text>
  <text x="25" y="170" font-size="8" font-family="monospace" fill="#60a5fa">brotli on;</text>
  <text x="25" y="184" font-size="8" font-family="monospace" fill="#60a5fa">brotli_types image/svg+xml</text>
  <text x="25" y="198" font-size="8" font-family="monospace" fill="#60a5fa">             text/html text/css</text>
  <text x="25" y="212" font-size="8" font-family="monospace" fill="#60a5fa">             application/javascript;</text>
  <text x="25" y="232" font-size="7" fill="#fbbf24">→ CDN modernos (Cloudflare, Fastly) lo activan solos</text>
</svg>
```
