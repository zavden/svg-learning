# 25.1 Estructura y organización del código SVG

Un SVG bien estructurado se lee como un edificio: cimientos primero (estilos, defs), luego las paredes (contenido). Los navegadores parsean de arriba a abajo y el orden en el DOM **es** el z-order. Saber esto convierte el SVG en algo navegable en lugar de una maraña de paths.

---

## El orden canónico dentro de un `<svg>`

```svg
<svg width="300" height="300"
     viewBox="0 0 300 300"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    4 capas de arriba a abajo
  </text>

  <rect x="15" y="38" width="270" height="56" fill="#dbeafe" stroke="#3b82f6" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#1e40af">1. &lt;style&gt; — reglas CSS</text>
  <text x="25" y="70" font-size="7" fill="#475569">variables, clases, selectores locales</text>
  <text x="25" y="84" font-size="7" fill="#475569">opcional, útil en SVG standalone</text>

  <rect x="15" y="102" width="270" height="64" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="118" font-size="9" font-weight="bold" fill="#166534">2. &lt;defs&gt; — elementos reutilizables</text>
  <text x="25" y="134" font-size="7" fill="#475569">gradientes, patrones, filtros, clipPaths,</text>
  <text x="25" y="148" font-size="7" fill="#475569">masks, symbols, markers</text>
  <text x="25" y="162" font-size="7" fill="#166534">→ invisibles hasta que algo los referencia</text>

  <rect x="15" y="174" width="270" height="52" fill="#ede9fe" stroke="#8b5cf6" rx="3"/>
  <text x="25" y="190" font-size="9" font-weight="bold" fill="#5b21b6">3. Fondo</text>
  <text x="25" y="206" font-size="7" fill="#475569">rectángulos de base, gradientes de fondo</text>
  <text x="25" y="220" font-size="7" fill="#475569">todo lo que debe quedar detrás</text>

  <rect x="15" y="234" width="270" height="58" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="250" font-size="9" font-weight="bold" fill="#92400e">4. Contenido principal</text>
  <text x="25" y="266" font-size="7" fill="#475569">grupos lógicos, elementos en orden Z</text>
  <text x="25" y="280" font-size="7" fill="#475569">los últimos elementos se pintan encima</text>
</svg>
```

---

## El orden visual ES el z-index

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    SVG no tiene z-index — solo orden del DOM
  </text>

  <circle cx="100" cy="130" r="40" fill="#3b82f6"/>
  <circle cx="130" cy="130" r="40" fill="#10b981"/>
  <circle cx="160" cy="130" r="40" fill="#f59e0b"/>

  <text x="150" y="200" text-anchor="middle" font-size="8" fill="#475569">
    azul primero → verde encima → ámbar el último
  </text>

  <rect x="15" y="214" width="270" height="34" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="230" font-size="8" font-weight="bold" fill="#991b1b">Si necesitas reordenar:</text>
  <text x="25" y="242" font-size="7" fill="#475569">mueve el elemento en el DOM (no hay shortcut CSS)</text>
</svg>
```

---

## viewBox: siempre, sin excusa

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    sin viewBox no hay SVG escalable
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Sin viewBox (mal)</text>
  <text x="25" y="70" font-size="8" font-family="monospace" fill="#475569">&lt;svg width="100" height="100"&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">→ tamaño fijo, no escala</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ el SVG no sabe cuál es su "interior"</text>
  <text x="25" y="112" font-size="7" fill="#475569">→ CSS width:100% lo estira horrible</text>

  <rect x="15" y="134" width="270" height="88" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#166534">Con viewBox (bien)</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#475569">&lt;svg viewBox="0 0 100 100"&gt;</text>
  <text x="25" y="180" font-size="7" fill="#475569">→ sistema interno 100×100 independiente</text>
  <text x="25" y="194" font-size="7" fill="#475569">→ el contenedor HTML decide el tamaño final</text>
  <text x="25" y="208" font-size="7" fill="#166534">→ responsive out of the box</text>

  <rect x="15" y="230" width="270" height="40" fill="#fef9c3" stroke="#f59e0b" rx="3"/>
  <text x="25" y="246" font-size="9" font-weight="bold" fill="#92400e">Regla</text>
  <text x="25" y="262" font-size="7" fill="#475569">viewBox="0 0 {ancho-lógico} {alto-lógico}" siempre</text>
</svg>
```

---

## Coordenadas legibles

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    valores redondos en vez de accidentales
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">viewBox="0 0 100 100"</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">&lt;circle cx="49.73" cy="51.2" r="23.1"/&gt;</text>
  <text x="25" y="104" font-size="7" fill="#475569">¿qué es 49.73? ¿por qué 23.1?</text>
  <text x="25" y="118" font-size="7" fill="#991b1b">→ coordenadas arbitrarias del editor</text>

  <rect x="15" y="138" width="270" height="106" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="170" font-size="7" font-family="monospace" fill="#475569">viewBox="0 0 100 100"</text>
  <text x="25" y="184" font-size="7" font-family="monospace" fill="#475569">&lt;circle cx="50" cy="50" r="25"/&gt;</text>
  <text x="25" y="204" font-size="7" fill="#475569">centro del cuadrado, radio 25% del ancho</text>
  <text x="25" y="218" font-size="7" fill="#166534">→ cualquiera entiende la intención</text>
  <text x="25" y="234" font-size="7" fill="#166534">→ SVGO puede redondear, tú diseñas redondo</text>
</svg>
```

---

## Grupos con propósito

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#1e293b"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#f8fafc">
    los &lt;g&gt; no son gratis — deben hacer algo
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#166534">Grupos útiles</text>
  <text x="25" y="70" font-size="7" fill="#475569">• aplican transform, opacity o filter compartidos</text>
  <text x="25" y="84" font-size="7" fill="#475569">• clip/mask aplicado al conjunto</text>
  <text x="25" y="98" font-size="7" fill="#475569">• marcan secciones lógicas con id</text>
  <text x="25" y="112" font-size="7" fill="#475569">• punto de anclaje para listeners de JS</text>
  <text x="25" y="128" font-size="7" fill="#166534">→ cada &lt;g&gt; debe justificar su existencia</text>

  <rect x="15" y="146" width="270" height="118" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#991b1b">Grupos basura</text>
  <text x="25" y="178" font-size="7" font-family="monospace" fill="#475569">&lt;g&gt;&lt;g&gt;&lt;g&gt;&lt;path .../&gt;&lt;/g&gt;&lt;/g&gt;&lt;/g&gt;</text>
  <text x="25" y="196" font-size="7" fill="#475569">• anidamiento fruto del editor</text>
  <text x="25" y="210" font-size="7" fill="#475569">• grupos con un solo hijo sin atributos</text>
  <text x="25" y="224" font-size="7" fill="#475569">• grupos vacíos que sobreviven a refactors</text>
  <text x="25" y="242" font-size="7" fill="#991b1b">→ SVGO (collapseGroups) los elimina automáticamente</text>
  <text x="25" y="256" font-size="7" fill="#991b1b">→ pero el código fuente debería ya estar limpio</text>
</svg>
```
