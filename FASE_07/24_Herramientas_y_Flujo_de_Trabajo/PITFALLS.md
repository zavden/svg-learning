# Pitfalls — Herramientas y Flujo de Trabajo

Los errores de esta sección no son de sintaxis SVG — son de **proceso**: elegir la herramienta equivocada para el trabajo, atarse a un plugin frágil, no versionar configs. Son errores que se pagan en fricción, no en runtime.

---

## 1. Usar D3 para dibujar un icono

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    el cañón de D3 para matar una mosca
  </text>

  <rect x="15" y="38" width="270" height="88" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">importar d3 entero (~250KB) para dibujar</text>
  <text x="25" y="84" font-size="7" fill="#475569">un gráfico estático, un icono, o una</text>
  <text x="25" y="98" font-size="7" fill="#475569">animación que un &lt;svg&gt; a pelo resolvería</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">→ bundle inflado sin ganar nada</text>

  <rect x="15" y="134" width="270" height="112" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#166534">Solución</text>
  <text x="25" y="166" font-size="7" fill="#475569">D3 solo si los datos dictan la forma:</text>
  <text x="25" y="180" font-size="7" fill="#475569">• escalas dinámicas, ejes, leyendas</text>
  <text x="25" y="194" font-size="7" fill="#475569">• mapas con proyecciones geográficas</text>
  <text x="25" y="208" font-size="7" fill="#475569">• layouts jerárquicos (tree, force)</text>
  <text x="25" y="224" font-size="7" fill="#475569">para el resto: SVG estático + CSS o</text>
  <text x="25" y="238" font-size="7" fill="#166534">submódulos de D3 (d3-scale, d3-shape)</text>
</svg>
```

---

## 2. No versionar svgo.config.js

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    "en mi máquina funcionaba"
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Qué pasa</text>
  <text x="25" y="70" font-size="7" fill="#475569">el dev A usa SVGOMG con ciertas opciones,</text>
  <text x="25" y="84" font-size="7" fill="#475569">el dev B con otras, el CI con las default</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ los SVGs optimizados difieren en cada pase</text>
  <text x="25" y="112" font-size="7" fill="#475569">→ diffs espurios en Git en cada PR</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ nadie confía en el pipeline</text>

  <rect x="15" y="138" width="270" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Solución</text>
  <text x="25" y="170" font-size="7" fill="#475569">• commitear svgo.config.js en la raíz</text>
  <text x="25" y="184" font-size="7" fill="#475569">• fijar la versión de svgo en package.json</text>
  <text x="25" y="198" font-size="7" fill="#475569">• correr svgo en el pre-commit hook</text>
  <text x="25" y="212" font-size="7" fill="#475569">• en CI: verificar que "svgo --check" no cambia</text>
  <text x="25" y="226" font-size="7" fill="#475569">→ todos usan la misma config, sin ambigüedad</text>
  <text x="25" y="240" font-size="7" fill="#166534">el resultado es reproducible</text>
</svg>
```

---

## 3. Depender solo del plugin del editor

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    el plugin de Figma ≠ pipeline
  </text>

  <rect x="15" y="38" width="270" height="92" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">el equipo confía en "SVGO Compressor" de</text>
  <text x="25" y="84" font-size="7" fill="#475569">Figma para optimizar antes de exportar</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ el plugin queda desactualizado</text>
  <text x="25" y="112" font-size="7" fill="#475569">→ un dev copia como SVG (sin plugin)</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ SVGs sucios entran al repo sin avisar</text>

  <rect x="15" y="138" width="270" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="154" font-size="9" font-weight="bold" fill="#166534">Solución</text>
  <text x="25" y="170" font-size="7" fill="#475569">• el pipeline de código es la verdad única</text>
  <text x="25" y="184" font-size="7" fill="#475569">• el SVG entra crudo desde el editor</text>
  <text x="25" y="198" font-size="7" fill="#475569">• SVGO corre en el commit o en el build</text>
  <text x="25" y="212" font-size="7" fill="#475569">• no importa cómo se exportó</text>
  <text x="25" y="226" font-size="7" fill="#166534">→ el plugin del editor es opcional,</text>
  <text x="25" y="240" font-size="7" fill="#166534">  no una dependencia crítica</text>
</svg>
```

---

## 4. Importar todos los iconos como componentes

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    200 iconos × 2KB en el main bundle
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Qué pasa</text>
  <text x="25" y="70" font-size="7" fill="#475569">import * as Icons from './icons'</text>
  <text x="25" y="84" font-size="7" fill="#475569">cada icono es un componente React importado</text>
  <text x="25" y="98" font-size="7" fill="#475569">barrel exports rompen el tree-shaking</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">→ todos los iconos entran al bundle inicial</text>

  <rect x="15" y="134" width="270" height="112" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="150" font-size="9" font-weight="bold" fill="#166534">Solución</text>
  <text x="25" y="166" font-size="7" fill="#475569">• imports específicos: import Close from './close'</text>
  <text x="25" y="180" font-size="7" fill="#475569">• o sprite &lt;use href="#close" /&gt; (un solo fetch)</text>
  <text x="25" y="194" font-size="7" fill="#475569">• si insistes en barrel → "sideEffects": false</text>
  <text x="25" y="208" font-size="7" fill="#475569">• verificar el bundle analyzer antes de release</text>
  <text x="25" y="224" font-size="7" fill="#475569">→ paga solo por los iconos que realmente</text>
  <text x="25" y="238" font-size="7" fill="#166534">  se usan en cada ruta</text>
</svg>
```

---

## 5. Editar el SVG exportado en lugar del original

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    la siguiente exportación borra tus cambios
  </text>

  <rect x="15" y="38" width="270" height="96" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Escenario típico</text>
  <text x="25" y="70" font-size="7" fill="#475569">1. diseñador exporta logo.svg desde Figma</text>
  <text x="25" y="84" font-size="7" fill="#475569">2. dev lo retoca a mano (añade id, ajusta stroke)</text>
  <text x="25" y="98" font-size="7" fill="#475569">3. diseñador actualiza → reexporta → overwrite</text>
  <text x="25" y="112" font-size="7" fill="#475569">4. los cambios del dev se pierden silenciosamente</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ nadie se da cuenta hasta que rompe algo</text>

  <rect x="15" y="142" width="270" height="104" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="158" font-size="9" font-weight="bold" fill="#166534">Soluciones</text>
  <text x="25" y="174" font-size="7" fill="#475569">• los cambios del dev deben vivir en el pipeline</text>
  <text x="25" y="188" font-size="7" fill="#475569">  (SVGO config, transformador JS posterior)</text>
  <text x="25" y="202" font-size="7" fill="#475569">• o en wrapper React: el .svg es inmutable,</text>
  <text x="25" y="216" font-size="7" fill="#475569">  el JSX alrededor es editable</text>
  <text x="25" y="230" font-size="7" fill="#475569">→ regla: nunca editar a mano lo que viene</text>
  <text x="25" y="244" font-size="7" fill="#166534">  de una herramienta que puede regenerarlo</text>
</svg>
```
