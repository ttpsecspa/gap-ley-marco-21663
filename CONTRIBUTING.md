<![CDATA[# 🤝 Guía de Contribución

¡Gracias por tu interés en contribuir a GAP Analysis Ley 21.663! Este documento describe las convenciones y procesos para contribuir al proyecto.

---

## 📋 Tabla de Contenidos

- [Código de Conducta](#-código-de-conducta)
- [Cómo Contribuir](#-cómo-contribuir)
- [Configuración del Entorno](#-configuración-del-entorno)
- [Convenciones de Código](#-convenciones-de-código)
- [Convenciones de Commits](#-convenciones-de-commits)
- [Pull Requests](#-pull-requests)
- [Reportar Issues](#-reportar-issues)
- [Seguridad](#-seguridad)

---

## 📜 Código de Conducta

Este proyecto se adhiere a un estándar de conducta profesional. Al contribuir, te comprometes a:

- Mantener un ambiente respetuoso, inclusivo y constructivo
- Aceptar críticas constructivas con profesionalismo
- Enfocarte en lo mejor para la comunidad y el proyecto
- Respetar la privacidad y seguridad de los usuarios

---

## 🎯 Cómo Contribuir

### Tipos de Contribución Bienvenidos

| Tipo | Descripción | Label |
|---|---|---|
| 🐛 Bug fixes | Corrección de errores en lógica o UI | `bug` |
| ✨ Features | Nuevos dominios, controles, mejoras UX | `enhancement` |
| 📖 Documentación | Mejoras en README, guías, mapeos | `docs` |
| 🔒 Seguridad | Mejoras en CSP, sanitización, hardening | `security` |
| ♿ Accesibilidad | ARIA, contraste, navegación por teclado | `a11y` |
| 🌐 i18n | Traducción a otros idiomas | `i18n` |
| 🧪 Testing | Mejoras en testing y verificación | `testing` |

### Lo Que NO Aceptamos

- ❌ Dependencias externas (CDNs, npm packages, frameworks, librerías)
- ❌ Código que transmita datos a servidores externos
- ❌ Cookies, localStorage, sessionStorage o cualquier persistencia
- ❌ Analytics, tracking, fingerprinting o pixels
- ❌ Cambios que debiliten la CSP existente
- ❌ Cambios que rompan la arquitectura single-file
- ❌ `eval()`, `Function()`, `new Function()`

---

## 🔧 Configuración del Entorno

### Requisitos

- Git
- Navegador moderno (Chrome 90+, Firefox 88+, Safari 15+)
- Node.js >= 18 (solo para servidor de desarrollo)
- Editor con soporte HTML/CSS/JS (VS Code recomendado)

### Setup

```bash
# 1. Fork del repositorio en GitHub

# 2. Clonar tu fork
git clone https://github.com/TU-USUARIO/gap-ley-marco-21663.git
cd gap-ley-marco-21663

# 3. Agregar upstream
git remote add upstream https://github.com/ttpsecspa/gap-ley-marco-21663.git

# 4. Crear rama de trabajo
git checkout -b feature/mi-mejora

# 5. Iniciar servidor de desarrollo
node static-server.mjs
# Abrir http://localhost:3457
```

### Sincronizar con upstream

```bash
git fetch upstream
git rebase upstream/main
```

---

## 📏 Convenciones de Código

### Arquitectura

- **Single-file**: Todo el código de la aplicación en `gap-analysis.html`
- **Sin build step**: No TypeScript, SCSS, JSX ni herramientas de compilación
- **Sin dependencias**: No agregar librerías externas ni CDNs
- **CSP intacta**: No debilitar ninguna directiva de la Content Security Policy

### HTML

```html
<!-- ✅ Correcto -->
<div id="step-1" class="section active">
<input class="form-input" id="empresa-nombre" placeholder="Nombre">

<!-- ❌ Incorrecto -->
<div ID="Step1" class="Section Active">
<INPUT CLASS='FormInput'>
```

- HTML5 semántico
- Atributos en minúsculas, valores entre comillas dobles
- IDs en `kebab-case`: `id="step-1"`, `id="empresa-nombre"`
- Clases en `kebab-case` con prefijo de componente: `hero-badge`, `nav-brand`

### CSS

```css
/* ✅ Correcto — usa variables del Design System */
.hero-feature {
  background: white;
  border: 1px solid var(--slate-200);
  border-radius: var(--radius-lg);   /* 16px */
  box-shadow: var(--shadow-sm);
  transition: all 150ms ease;
}

/* ❌ Incorrecto — valores hardcodeados */
.hero-feature {
  border: 1px solid #e2e8f0;
  border-radius: 16px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.05);
  transition: all 0.15s ease;
}
```

- Variables CSS definidas en `:root` (TTPSEC Design System)
- **Colores**: siempre via variables (`var(--blue-600)`)
- **Bordes**: `var(--radius)` (12px), `var(--radius-lg)` (16px), `var(--radius-sm)` (6px)
- **Sombras**: `var(--shadow-sm)`, `var(--shadow)`, `var(--shadow-md)`, `var(--shadow-lg)`
- **Transiciones**: `150ms ease`
- **Breakpoint**: mobile-first con `@media (max-width: 768px)`
- **Font stack**: `system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif`

### JavaScript

```javascript
// ✅ Correcto — ES5, var, function declaration, sanitize
var orgContext = 'ti';

function selectContext(el, ctx) {
  document.querySelectorAll('.hero-context-card').forEach(function(c) {
    c.classList.remove('selected');
  });
  el.classList.add('selected');
  orgContext = ctx;
}

function displayName(name) {
  var clean = sanitize(name);
  element.textContent = clean;
}

// ❌ Incorrecto — ES6+, sin sanitizar
const selectContext = (el, ctx) => {
  element.innerHTML = name; // XSS!
};
```

- **ES5 compatible**: sin arrow functions, sin `let`/`const`, sin template literals, sin destructuring
- **Variables**: `var`
- **Funciones**: `function nombre() {}`
- **Seguridad**: `sanitize()` en todo input antes del DOM
- **Prohibido**: `eval()`, `Function()`, `new Function()`
- **Naming**: `camelCase` para funciones y variables

### Idioma

- **UI (contenido visible)**: Español
- **Comentarios**: Español o inglés
- **Commits**: Inglés
- **Nombres de archivos**: Inglés

### Linter / Formatter

No hay linter configurado actualmente. Sigue las convenciones descritas arriba y mantén consistencia con el código existente.

---

## 📝 Convenciones de Commits

Usamos [Conventional Commits](https://www.conventionalcommits.org/):

```
<tipo>: <descripción corta en inglés>
```

### Tipos

| Tipo | Uso | Ejemplo |
|---|---|---|
| `feat` | Nueva funcionalidad | `feat: add risk matrix visualization` |
| `fix` | Corrección de bug | `fix: correct scoring for domain 7` |
| `style` | Cambios de CSS (sin cambio funcional) | `style: homologate to TTPSEC Style Guide` |
| `refactor` | Refactorización | `refactor: simplify calculate function` |
| `docs` | Documentación | `docs: add CWE mapping table` |
| `security` | Mejoras de seguridad | `security: add CSP headers` |
| `ci` | CI/CD | `ci: add GitHub Pages workflow` |
| `license` | Licencia | `license: add MIT header` |
| `chore` | Mantenimiento | `chore: update .gitignore` |

### Reglas

- Primera línea ≤ 72 caracteres
- Usar imperativo: "add", no "added" ni "adds"
- No terminar con punto
- Cuerpo opcional separado por línea en blanco

---

## 🔀 Pull Requests

### Proceso

1. Asegúrate de que tu rama está actualizada con `main`
2. Verifica que la aplicación funciona en el navegador
3. Verifica 0 errores en la consola del navegador (F12)
4. Verifica que funciona en móvil (responsive)
5. Crea el PR con descripción clara

### Checklist del PR

Antes de solicitar revisión, verifica:

- [ ] Código sigue las convenciones del proyecto
- [ ] No se agregaron dependencias externas
- [ ] No se transmiten datos a servidores externos
- [ ] La CSP no fue debilitada
- [ ] Inputs del usuario sanitizados con `sanitize()`
- [ ] Sin `eval()`, `Function()` ni `new Function()`
- [ ] Funciona en Chrome, Firefox, Safari y Edge
- [ ] Funciona en móvil (responsive ≤768px)
- [ ] 0 errores en consola del navegador
- [ ] Commits siguen Conventional Commits

### Proceso de Revisión

- Todos los PRs requieren **al menos una revisión** antes de merge
- Cambios de seguridad requieren **revisión adicional**
- Se usa **squash merge** a `main`
- El deploy a GitHub Pages es automático tras merge

---

## 🐛 Reportar Issues

### Bug Reports

Usa la [plantilla de bug report](.github/ISSUE_TEMPLATE/bug_report.md). Incluye:

- Navegador y versión
- Sistema operativo
- Pasos para reproducir
- Comportamiento esperado vs. actual
- Errores de consola (si los hay)
- Screenshots (si aplica)

### Feature Requests

Usa la [plantilla de feature request](.github/ISSUE_TEMPLATE/feature_request.md). Incluye:

- Problema que resuelve
- Solución propuesta
- Contexto normativo (si aplica: artículos de la Ley, IG ANCI)

---

## 🔒 Seguridad

**NUNCA** reportes vulnerabilidades como issues públicos.

Consulta [SECURITY.md](SECURITY.md) para el proceso de divulgación coordinada.

---

## 📄 Licencia

Al contribuir, aceptas que tus contribuciones se licencien bajo la [Licencia MIT](LICENSE) del proyecto.

---

<div align="center">

**TTPSEC SPA** — Powered by TTPSEC.AI

</div>
]]>