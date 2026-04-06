# 18.7 SVG como data URI

Un **data URI** incrusta el SVG directamente en el HTML/CSS como una cadena de texto, sin archivo externo. Útil para SVGs muy pequeños o para contextos donde no se pueden servir archivos (emails, apps sin servidor).

---

## Dos contextos de uso

```svg
<svg width="300" height="240"
     viewBox="0 0 300 240"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    data URI en &lt;img&gt; y en CSS
  </text>

  <!-- En HTML -->
  <rect x="15" y="38" width="270" height="88" fill="#0f172a" rx="3"/>
  <text x="25" y="54" font-size="8" font-family="monospace" fill="#94a3b8">/* en HTML */</text>
  <text x="25" y="68" font-size="8" font-family="monospace" fill="#e2e8f0">&lt;img</text>
  <text x="35" y="82" font-size="8" font-family="monospace" fill="#60a5fa">  src="data:image/svg+xml,</text>
  <text x="35" y="94" font-size="7" font-family="monospace" fill="#fbbf24">    %3Csvg xmlns='...'%3E</text>
  <text x="35" y="106" font-size="7" font-family="monospace" fill="#fbbf24">    %3Ccircle cx='5' cy='5' r='4'/%3E</text>
  <text x="35" y="118" font-size="7" font-family="monospace" fill="#fbbf24">    %3C/svg%3E"</text>
  <text x="25" y="130" font-size="8" font-family="monospace" fill="#e2e8f0">  alt="Círculo"&gt;</text>

  <!-- En CSS -->
  <rect x="15" y="134" width="270" height="98" fill="#0f172a" rx="3"/>
  <text x="25" y="150" font-size="8" font-family="monospace" fill="#94a3b8">/* en CSS */</text>
  <text x="25" y="164" font-size="8" font-family="monospace" fill="#e2e8f0">.icono {</text>
  <text x="25" y="178" font-size="8" font-family="monospace" fill="#60a5fa">  background-image: url("data:image/svg+xml,</text>
  <text x="25" y="190" font-size="7" font-family="monospace" fill="#fbbf24">    &lt;svg xmlns='...'&gt;</text>
  <text x="25" y="202" font-size="7" font-family="monospace" fill="#fbbf24">    &lt;circle cx='5' cy='5' r='4'/&gt;</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#fbbf24">    &lt;/svg&gt;");</text>
  <text x="25" y="228" font-size="8" font-family="monospace" fill="#e2e8f0">}</text>
</svg>
```

---

## URL-encoding vs. base64

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    dos codificaciones para el mismo SVG
  </text>

  <!-- URL-encoded -->
  <rect x="15" y="38" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">URL-encoded (recomendado para SVG)</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">data:image/svg+xml,%3Csvg...%3E</text>
  <text x="25" y="86" font-size="7" fill="#475569">• más legible, inspeccionable a ojo</text>
  <text x="25" y="100" font-size="7" fill="#475569">• más compacto para SVG (texto)</text>
  <text x="25" y="114" font-size="7" fill="#475569">• requiere escapar: &lt; &gt; # % " '</text>
  <text x="25" y="128" font-size="7" fill="#166534">• ✓ ideal para la mayoría de SVGs</text>

  <!-- Base64 -->
  <rect x="15" y="146" width="270" height="100" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#1e40af">Base64</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">data:image/svg+xml;base64,PHN2Zy4uLg==</text>
  <text x="25" y="194" font-size="7" fill="#475569">• ilegible para humanos</text>
  <text x="25" y="208" font-size="7" fill="#475569">• aumenta tamaño ~33%</text>
  <text x="25" y="222" font-size="7" fill="#475569">• útil si el SVG tiene muchos caracteres especiales</text>
  <text x="25" y="236" font-size="7" fill="#1e40af">• mejor para PNG/JPEG (binarios)</text>
</svg>
```

---

## Caracteres que deben codificarse

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    tabla de URL-encoding para SVG
  </text>

  <!-- Header -->
  <rect x="15" y="38" width="270" height="20" fill="#e2e8f0"/>
  <text x="30" y="52" font-size="8" font-weight="bold" fill="#1e293b">Carácter</text>
  <text x="110" y="52" font-size="8" font-weight="bold" fill="#1e293b">Codificado</text>
  <text x="200" y="52" font-size="8" font-weight="bold" fill="#1e293b">Contexto</text>

  <!-- Rows -->
  <rect x="15" y="58" width="270" height="22" fill="white"/>
  <text x="30" y="72" font-size="8" font-family="monospace" fill="#3b82f6">&lt;</text>
  <text x="110" y="72" font-size="8" font-family="monospace" fill="#1e293b">%3C</text>
  <text x="200" y="72" font-size="7" fill="#64748b">apertura de tags</text>

  <rect x="15" y="80" width="270" height="22" fill="#f8fafc"/>
  <text x="30" y="94" font-size="8" font-family="monospace" fill="#3b82f6">&gt;</text>
  <text x="110" y="94" font-size="8" font-family="monospace" fill="#1e293b">%3E</text>
  <text x="200" y="94" font-size="7" fill="#64748b">cierre de tags</text>

  <rect x="15" y="102" width="270" height="22" fill="white"/>
  <text x="30" y="116" font-size="8" font-family="monospace" fill="#3b82f6">#</text>
  <text x="110" y="116" font-size="8" font-family="monospace" fill="#1e293b">%23</text>
  <text x="200" y="116" font-size="7" fill="#64748b">colores hex: #ff0000</text>

  <rect x="15" y="124" width="270" height="22" fill="#f8fafc"/>
  <text x="30" y="138" font-size="8" font-family="monospace" fill="#3b82f6">"</text>
  <text x="110" y="138" font-size="8" font-family="monospace" fill="#1e293b">%22 o '</text>
  <text x="200" y="138" font-size="7" fill="#64748b">atributos; usa ' si puedes</text>

  <rect x="15" y="146" width="270" height="22" fill="white"/>
  <text x="30" y="160" font-size="8" font-family="monospace" fill="#3b82f6">%</text>
  <text x="110" y="160" font-size="8" font-family="monospace" fill="#1e293b">%25</text>
  <text x="200" y="160" font-size="7" fill="#64748b">el propio símbolo de encoding</text>

  <rect x="15" y="168" width="270" height="22" fill="#f8fafc"/>
  <text x="30" y="182" font-size="8" font-family="monospace" fill="#3b82f6">espacio</text>
  <text x="110" y="182" font-size="8" font-family="monospace" fill="#1e293b">%20</text>
  <text x="200" y="182" font-size="7" fill="#64748b">o déjalo si el contexto lo permite</text>

  <!-- Truco -->
  <rect x="15" y="200" width="270" height="54" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="216" font-size="8" font-weight="bold" fill="#92400e">Truco práctico</text>
  <text x="25" y="230" font-size="7" fill="#475569">usa comillas simples dentro del SVG: fill='red' stroke='blue'</text>
  <text x="25" y="244" font-size="7" fill="#475569">así no tienes que escapar las comillas dobles del atributo padre</text>
</svg>
```

---

## Cuándo conviene

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    ¿cuándo data URI es la mejor opción?
  </text>

  <!-- Sí -->
  <rect x="15" y="38" width="270" height="120" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">✓ Sí, para</text>
  <text x="25" y="70" font-size="7" fill="#1e293b">• SVGs muy pequeños (&lt; 1-2 KB): la petición HTTP cuesta más</text>
  <text x="25" y="84" font-size="7" fill="#1e293b">• emails HTML donde los recursos externos son bloqueados</text>
  <text x="25" y="98" font-size="7" fill="#1e293b">• CSS generado donde el color del ícono cambia por contexto</text>
  <text x="25" y="112" font-size="7" fill="#1e293b">• apps sin servidor (archivos HTML offline, file://)</text>
  <text x="25" y="126" font-size="7" fill="#1e293b">• build tools que generan íconos parametrizados</text>
  <text x="25" y="140" font-size="7" fill="#1e293b">• evitar CORS con canvas: los data URI no lo activan</text>

  <!-- No -->
  <rect x="15" y="166" width="270" height="96" fill="#fef2f2" stroke="#ef4444" rx="3"/>
  <text x="25" y="182" font-size="9" font-weight="bold" fill="#991b1b">✗ No, para</text>
  <text x="25" y="198" font-size="7" fill="#1e293b">• SVGs medianos o grandes (&gt; 2 KB)</text>
  <text x="25" y="212" font-size="7" fill="#1e293b">• SVGs reutilizados en múltiples páginas (no se cachea)</text>
  <text x="25" y="226" font-size="7" fill="#1e293b">• SVGs con muchos colores hex (muchos %23)</text>
  <text x="25" y="240" font-size="7" fill="#1e293b">• contenido que debe actualizarse fácilmente</text>
  <text x="25" y="254" font-size="7" fill="#991b1b">el peso inline duplica por ser texto crudo</text>
</svg>
```
