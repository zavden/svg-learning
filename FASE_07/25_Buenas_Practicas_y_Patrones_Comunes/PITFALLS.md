# Pitfalls — Buenas Prácticas y Patrones Comunes

Los errores de esta sección son los que **no se detectan en el navegador** — se detectan en producción, por un usuario con lector de pantalla, un atacante, un diseñador de otro equipo o un navegador en Android gama baja. Son precisamente los que esta sección entera intenta evitar.

---

## 1. Iconos "accesibles" que son ruido para lectores de pantalla

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    "icono decorativo" con title = spam de audio
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Qué pasa</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">&lt;button&gt;</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  &lt;svg&gt;&lt;title&gt;icon-close&lt;/title&gt;...&lt;/svg&gt;</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">  Cerrar</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">&lt;/button&gt;</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">NVDA lee: "icon-close Cerrar botón" — duplicado</text>

  <rect x="15" y="146" width="270" height="116" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Regla</text>
  <text x="25" y="178" font-size="7" fill="#475569">Si el texto del botón ya dice qué hace:</text>
  <text x="25" y="192" font-size="7" font-family="monospace" fill="#475569">&lt;svg aria-hidden="true"&gt;</text>
  <text x="25" y="208" font-size="7" fill="#475569">Si el botón no tiene texto:</text>
  <text x="25" y="222" font-size="7" font-family="monospace" fill="#475569">&lt;button aria-label="Cerrar"&gt;</text>
  <text x="25" y="236" font-size="7" font-family="monospace" fill="#475569">  &lt;svg aria-hidden="true"&gt;</text>
  <text x="25" y="252" font-size="7" fill="#166534">→ nunca poner &lt;title&gt; "por si acaso"</text>
</svg>
```

---

## 2. Animar propiedades que fuerzan layout

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    jank a 30fps en vez de seda a 60fps
  </text>

  <rect x="15" y="38" width="270" height="108" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Mal</text>
  <text x="25" y="70" font-size="7" font-family="monospace" fill="#475569">@keyframes mover {</text>
  <text x="25" y="84" font-size="7" font-family="monospace" fill="#475569">  from { x: 0 }</text>
  <text x="25" y="98" font-size="7" font-family="monospace" fill="#475569">  to { x: 100 }</text>
  <text x="25" y="112" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="128" font-size="7" fill="#991b1b">→ cada frame recalcula layout del &lt;rect&gt;</text>

  <rect x="15" y="154" width="270" height="108" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="170" font-size="9" font-weight="bold" fill="#166534">Bien</text>
  <text x="25" y="186" font-size="7" font-family="monospace" fill="#475569">@keyframes mover {</text>
  <text x="25" y="200" font-size="7" font-family="monospace" fill="#475569">  from { transform: translateX(0) }</text>
  <text x="25" y="214" font-size="7" font-family="monospace" fill="#475569">  to { transform: translateX(100px) }</text>
  <text x="25" y="228" font-size="7" font-family="monospace" fill="#475569">}</text>
  <text x="25" y="244" font-size="7" fill="#166534">→ GPU compositing, sin tocar layout</text>
  <text x="25" y="258" font-size="7" fill="#166534">→ 60fps estables en mobile</text>
</svg>
```

---

## 3. SVG de terceros pegado inline sin sanitizar

```svg
<svg width="300" height="280"
     viewBox="0 0 300 280"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    XSS almacenado vía upload de avatar
  </text>

  <rect x="15" y="38" width="270" height="112" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Escenario</text>
  <text x="25" y="70" font-size="7" fill="#475569">• usuario sube un avatar.svg con &lt;script&gt;</text>
  <text x="25" y="84" font-size="7" fill="#475569">• el servidor lo guarda "tal cual" porque es .svg</text>
  <text x="25" y="98" font-size="7" fill="#475569">• la app lo muestra con innerHTML en perfiles</text>
  <text x="25" y="112" font-size="7" fill="#475569">• cada visitante ejecuta el script del atacante</text>
  <text x="25" y="128" font-size="7" font-family="monospace" fill="#475569">  → fetch('/api/me/cookie').then(ex_filtrar)</text>
  <text x="25" y="142" font-size="7" fill="#991b1b">→ XSS almacenado, cuentas comprometidas</text>

  <rect x="15" y="158" width="270" height="104" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="174" font-size="9" font-weight="bold" fill="#166534">Mitigación en capas</text>
  <text x="25" y="190" font-size="7" fill="#475569">1. servir uploads como &lt;img&gt;, no inline</text>
  <text x="25" y="204" font-size="7" fill="#475569">2. servir desde un dominio separado (cookies aisladas)</text>
  <text x="25" y="218" font-size="7" fill="#475569">3. sanitizar con DOMPurify antes de guardar</text>
  <text x="25" y="232" font-size="7" fill="#475569">4. CSP script-src 'self' como red de seguridad</text>
  <text x="25" y="246" font-size="7" fill="#166534">→ una sola defensa nunca es suficiente</text>
</svg>
```

---

## 4. Ignorar `prefers-reduced-motion`

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    mareas, migrañas, crisis vestibulares
  </text>

  <rect x="15" y="38" width="270" height="84" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Qué pasa</text>
  <text x="25" y="70" font-size="7" fill="#475569">el usuario tiene activado "reducir movimiento"</text>
  <text x="25" y="84" font-size="7" fill="#475569">en su sistema — pero la animación sigue corriendo</text>
  <text x="25" y="98" font-size="7" fill="#475569">→ puede causar mareos reales, no incomodidad menor</text>
  <text x="25" y="112" font-size="7" fill="#991b1b">→ A11y fail que debería ser estándar</text>

  <rect x="15" y="134" width="270" height="112" fill="#0f172a" rx="3"/>
  <text x="25" y="150" font-size="7" font-family="monospace" fill="#94a3b8">// respetar la preferencia</text>
  <text x="25" y="166" font-size="8" font-family="monospace" fill="#60a5fa">@media (prefers-reduced-motion:</text>
  <text x="25" y="180" font-size="8" font-family="monospace" fill="#60a5fa">   reduce) {</text>
  <text x="25" y="196" font-size="8" font-family="monospace" fill="#34d399">  .spinner {</text>
  <text x="25" y="210" font-size="8" font-family="monospace" fill="#fbbf24">    animation: none;</text>
  <text x="25" y="224" font-size="8" font-family="monospace" fill="#34d399">  }</text>
  <text x="25" y="238" font-size="8" font-family="monospace" fill="#60a5fa">}</text>
</svg>
```

---

## 5. "Mi SVG funciona, envío a prod" sin medir

```svg
<svg width="300" height="260"
     viewBox="0 0 300 260"
     xmlns="http://www.w3.org/2000/svg"
     style="border:2px solid #e2e8f0;background:#f8fafc;display:block; font-family:sans-serif;">

  <rect x="0" y="0" width="300" height="24" fill="#ef4444"/>
  <text x="150" y="16" text-anchor="middle" font-size="11" font-weight="bold" fill="#fff">
    la bajada de rendimiento que no se ve en dev
  </text>

  <rect x="15" y="38" width="270" height="100" fill="#fee2e2" stroke="#ef4444" rx="3"/>
  <text x="25" y="54" font-size="9" font-weight="bold" fill="#991b1b">Síntoma</text>
  <text x="25" y="70" font-size="7" fill="#475569">• todo va bien en el M1 del dev</text>
  <text x="25" y="84" font-size="7" fill="#475569">• Android de gama baja: jank, FPS a 15</text>
  <text x="25" y="98" font-size="7" fill="#475569">• nadie lo descubre hasta quejas reales</text>
  <text x="25" y="112" font-size="7" fill="#475569">• LCP pasa de 1.2s a 4.8s sin motivo aparente</text>
  <text x="25" y="126" font-size="7" fill="#991b1b">→ la ilustración "pequeña" tenía 3.000 nodos</text>

  <rect x="15" y="146" width="270" height="100" fill="#dcfce7" stroke="#10b981" rx="3"/>
  <text x="25" y="162" font-size="9" font-weight="bold" fill="#166534">Qué medir antes</text>
  <text x="25" y="178" font-size="7" fill="#475569">• número de nodos: querySelectorAll('svg *').length</text>
  <text x="25" y="192" font-size="7" fill="#475569">• peso en bytes y con gzip/brotli</text>
  <text x="25" y="206" font-size="7" fill="#475569">• DevTools Performance tab en throttle 4x</text>
  <text x="25" y="220" font-size="7" fill="#475569">• Lighthouse con throttled mobile</text>
  <text x="25" y="234" font-size="7" fill="#166534">→ medir en mobile real, no en la máquina del dev</text>
</svg>
```
