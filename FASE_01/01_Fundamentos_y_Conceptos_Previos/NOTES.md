# Notas de compatibilidad y referencias — Sección 01

Observaciones que no encajan en la teoría principal: quirks de browsers, casos edge, y recursos para profundizar.

---

## Compatibilidad de navegadores

### SVG 1.1 — soporte universal (2025)

SVG 1.1 tiene soporte completo en todos los navegadores modernos desde hace más de una década:

- Chrome / Edge: soporte desde versión 4+
- Firefox: soporte desde versión 3+
- Safari: soporte desde versión 3.1+
- Opera: soporte desde versión 9.5+

**Conclusión práctica:** en 2025, no hay que preocuparse por la compatibilidad de SVG 1.1. Si alguien en el equipo menciona IE8, SVG inline no funciona ahí — pero IE8 tiene cuota de mercado < 0.01%.

---

### SVG inline vs `<img src="archivo.svg">`

| Característica | SVG inline | `<img src="*.svg">` |
|---|---|---|
| JavaScript accede al DOM | Sí | No |
| CSS del documento estiliza el SVG | Sí | No |
| El SVG puede usar CSS del documento | Sí | No |
| HTTP request separado | No | Sí |
| Caché del browser | No (parte del HTML) | Sí |
| `<use>` cross-origin | No aplica | No funciona |

**Regla práctica:** para iconos interactivos o temizables con CSS → inline. Para imágenes decorativas estáticas → `<img>`.

---

### `<img src="*.svg">` y CORS

Cuando usas un SVG como `<img src="logo.svg">` desde un dominio diferente, el SVG se carga normalmente (es una imagen). Pero si el SVG referencia recursos externos (fuentes, imágenes), esos recursos están sujetos a CORS y pueden bloquearse.

---

## SVG como background CSS

```css
.icon {
  background-image: url("icono.svg");
  background-size: contain;
}
```

Funciona bien para iconos decorativos simples. Limitaciones:
- No se puede cambiar el color con CSS (el SVG tiene su propio color interno)
- El SVG no puede acceder a variables CSS del documento padre
- La solución para iconos coloreables con CSS: SVG inline o `mask-image`

---

## Cosas a revisar cuando algo no funciona

| Síntoma | Causa probable | Qué revisar |
|---|---|---|
| El SVG no muestra nada (archivo .svg externo) | Falta xmlns o error XML | Añadir `xmlns="http://www.w3.org/2000/svg"` y validar XML |
| El SVG no muestra nada (inline) | Error de sintaxis XML | Abrir DevTools → consola → buscar error de parseo |
| El SVG se ve negro en lugar del color esperado | `fill` o `stroke` no se aplica correctamente | Verificar que el nombre del elemento es correcto (case-sensitive) |
| Un elemento SVG no aparece | Nombre de elemento incorrecto (typo/case) | Comparar con la especificación, verificar camelCase |
| `linearGradient` no funciona | Escrito como `lineargradient` | Todo en camelCase exacto |
| Texto SVG no aparece | `<p>` o `<div>` usado en vez de `<text>` | Usar siempre `<text>` para texto en SVG |
| SVG en `<img>` no responde a JS | `<img>` no permite acceso al DOM del SVG | Usar SVG inline para interactividad |

---

## Recursos para profundizar

- **MDN Web Docs: SVG** — referencia completa de todos los elementos y atributos SVG: buscar "MDN SVG element reference"
- **SVG 1.1 Specification (W3C)** — la especificación oficial: árida pero definitiva para resolver dudas técnicas
- **SVGOMG** — optimizador SVG online que permite ver cómo se simplifica un SVG manteniendo su apariencia
- **Inkscape** — editor SVG de código abierto: útil para crear SVGs y ver el código que genera
- **Sara Soueidan** — artículos y tutoriales SVG de alta calidad: buscar su blog o CSS-Tricks SVG series

---

## Temas avanzados mencionados en esta sección (para fases posteriores)

- **SVG Sprites** — técnica para agrupar múltiples iconos SVG en un solo archivo y reusarlos con `<use>`. Se cubre en la sección de `<defs>` y `<use>`.
- **SVGZ** — SVG comprimido con gzip. Reduce el tamaño hasta un 80%. El servidor debe servir el Content-Type correcto.
- **SVG como CSS background con variables** — usando `mask-image` y `background-color` para colorear iconos SVG sin JavaScript. Técnica moderna para icon systems.
- **Generación programática de SVG** — crear SVG con JavaScript, Python, o cualquier lenguaje. El DOM SVG es manipulable desde JS (sección de interactividad, Fase 6+).
