<![CDATA[<!-- Banner -->
<div align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![HTML5](https://img.shields.io/badge/HTML5-CSS3-JS-E34F26?logo=html5&logoColor=white)](#-arquitectura--diagrama)
[![Estado: Stable](https://img.shields.io/badge/estado-stable-brightgreen)](#)
[![GitHub Pages](https://img.shields.io/badge/deploy-GitHub%20Pages-222?logo=github)](https://ttpsecspa.github.io/gap-ley-marco-21663/)
[![OWASP](https://img.shields.io/badge/OWASP-Top%2010%20Mapped-purple)](docs/OWASP_MAPPING.md)
[![CWE](https://img.shields.io/badge/CWE-32%20Mapped-purple)](docs/CWE_MAPPING.md)
[![Security Policy](https://img.shields.io/badge/security-policy-green.svg)](SECURITY.md)
[![100% Client-Side](https://img.shields.io/badge/privacy-100%25%20client--side-brightgreen)](#-seguridad)

# GAP Analysis — Ley 21.663

*Software de análisis de brechas para cumplimiento de la Ley 21.663 — Marco de Ciberseguridad de Chile*

| Stack | Licencia | Estado | Versión |
|---|---|---|---|
| HTML5 + CSS3 + JS vanilla (ES5) | MIT | Stable | v1.4 |

### [▶️ Ver Demo en Vivo](https://ttpsecspa.github.io/gap-ley-marco-21663/)

[Reportar Bug](.github/ISSUE_TEMPLATE/bug_report.md) · [Solicitar Feature](.github/ISSUE_TEMPLATE/feature_request.md) · [Reportar Vulnerabilidad](.github/ISSUE_TEMPLATE/security_vulnerability.md)

</div>

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Arquitectura / Diagrama](#-arquitectura--diagrama)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación](#-instalación)
- [Uso / Quick Start](#-uso--quick-start)
- [Configuración](#-configuración)
- [Funciones Documentadas](#-funciones-documentadas)
- [Seguridad](#-seguridad)
- [Testing](#-testing)
- [Contribución](#-contribución)
- [Roadmap](#-roadmap)
- [Licencia](#-licencia)
- [Contacto / Soporte](#-contacto--soporte)

---

## 📦 Descripción

GAP Analysis Ley 21.663 es una herramienta web **100% client-side** que permite a equipos GRC, CISOs, auditores y oficiales de cumplimiento evaluar el nivel de madurez de su organización frente a los requerimientos de la **Ley 21.663 — Marco de Ciberseguridad de Chile** y las **Instrucciones Generales de la ANCI**.

Ningún dato sale del navegador. Sin registro, sin cookies, sin backend.

**Features principales:**

- **15 dominios de evaluación** mapeados a la Ley 21.663, Instrucciones Generales ANCI 1-4 y Resolución N° 7
- **3 contextos organizacionales**: TI Corporativo, OT/ICS, Infraestructura Crítica
- **Dashboard de resultados** con scoring global, heatmap y tabla de brechas
- **Quick Wins** priorizados por impacto inmediato y bajo esfuerzo
- **Matriz de riesgos** con exposición en UTM y clasificación de severidad
- **Plan de remediación** en 5 fases (24 semanas) con timeline visual
- **Exportación** de informe completo en texto plano
- **Mapeo a marcos internacionales**: ISO 27001, NIST CSF 2.0, NIST 800-53, CIS v8.1, ISA 62443, NERC CIP, MITRE ATT&CK

> ⚠️ **Disclaimer**: Plataforma académica y educativa. No afiliada ni respaldada por la ANCI ni el Gobierno de Chile. No reemplaza una auditoría formal ni constituye asesoría legal.

---

## 🏗️ Arquitectura / Diagrama

```
┌─────────────────────────────────────────────────────────────┐
│                        NAVEGADOR                             │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              gap-analysis.html (single-file)          │   │
│  │                                                       │   │
│  │  ┌─────────┐  ┌──────────┐  ┌─────────────────────┐ │   │
│  │  │  HTML5   │  │  CSS3    │  │  JavaScript (ES5)   │ │   │
│  │  │ 6 steps  │  │ TTPSEC   │  │ - sanitize()        │ │   │
│  │  │ 15 forms │  │ Design   │  │ - calculate()       │ │   │
│  │  │ hero     │  │ System   │  │ - generateQuickWins()│ │   │
│  │  │ footer   │  │ Mobile   │  │ - generateRisks()   │ │   │
│  │  │          │  │ First    │  │ - generatePlan()    │ │   │
│  │  │          │  │          │  │ - exportPlan()      │ │   │
│  │  └─────────┘  └──────────┘  └─────────────────────┘ │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  CSP: connect-src 'none' ← BLOQUEA TODA SALIDA DE DATOS    │
│  Sin cookies | Sin localStorage | Sin fetch/XHR              │
└─────────────────────────────────────────────────────────────┘
        │                                    ▲
        │ Deploy (GitHub Actions)            │ HTTPS
        ▼                                    │
┌──────────────────┐              ┌─────────────────┐
│  GitHub Actions   │──────────→  │  GitHub Pages    │
│  pages.yml        │  artifact   │  (hosting)       │
└──────────────────┘              └─────────────────┘
```

### Estructura del Repositorio

```
gap-ley-marco-21663/
├── gap-analysis.html          # Aplicación completa (HTML + CSS + JS, ~1400 líneas)
├── index.html                 # Redirect → gap-analysis.html (con CSP + OG tags)
├── og-card.html               # Tarjeta Open Graph para redes sociales
├── static-server.mjs          # Servidor local de desarrollo (Node.js, port 3457)
├── server.mjs                 # Servidor OG card (Node.js, port 3456)
├── .github/
│   ├── workflows/
│   │   └── pages.yml          # CI/CD: Deploy automático a GitHub Pages
│   └── ISSUE_TEMPLATE/
│       ├── bug_report.md      # Template: reporte de bugs
│       ├── feature_request.md # Template: solicitud de features
│       └── security_vulnerability.md  # Template: reporte de seguridad
├── docs/
│   ├── CWE_MAPPING.md         # Mapeo de controles a CWE (32 entries)
│   └── OWASP_MAPPING.md       # Mapeo a OWASP Top 10 2021 (10/10)
├── *.pdf                      # 7 documentos normativos (Ley 21.663, IG 1-4, Res. N°7)
├── SECURITY.md                # Política de seguridad completa
├── CONTRIBUTING.md            # Guía de contribución
├── CHANGELOG.md               # Historial de cambios (Keep a Changelog)
├── LICENSE                    # MIT License
├── CLAUDE.md                  # Instrucciones para Claude Code
└── .gitignore                 # Exclusiones de seguridad
```

### Componentes Clave

| Componente | Archivo | Descripción |
|---|---|---|
| **Hero/Portada** | `gap-analysis.html` | Landing con badges de privacidad, selector de contexto (TI/OT/IC), CTA |
| **Step 1: Empresa** | `gap-analysis.html` | Formulario de datos organizacionales (razón social, RUT, sector, calificación Ley) |
| **Step 2: Evaluación** | `gap-analysis.html` | 15 dominios con escala 0-5, cada uno mapeado a artículos específicos de la Ley |
| **Step 3: Resultados** | `gap-analysis.html` | Score global, heatmap, tabla de brechas ordenada por criticidad |
| **Step 4: Quick Wins** | `gap-analysis.html` | Acciones priorizadas automáticamente según gaps detectados |
| **Step 5: Riesgos** | `gap-analysis.html` | Matriz con exposición UTM, clasificación por severidad |
| **Step 6: Plan** | `gap-analysis.html` | Timeline de 5 fases (24 semanas) generado dinámicamente |
| **Exportación** | `gap-analysis.html` | Descarga de informe completo en `.txt` |

---

## 📋 Requisitos Previos

### Sistemas Operativos Soportados

| OS | Soporte |
|---|---|
| Windows 10/11 | ✅ |
| macOS 12+ | ✅ |
| Linux (cualquier distro) | ✅ |
| iOS 15+ (Safari) | ✅ |
| Android 12+ (Chrome) | ✅ |

### Dependencias

| Dependencia | Versión Mínima | Requerida | Uso |
|---|---|---|---|
| Navegador moderno | Chrome 90+ / Firefox 88+ / Safari 15+ / Edge 90+ | ✅ | Ejecución de la aplicación |
| JavaScript habilitado | — | ✅ | Lógica de evaluación |
| Node.js | 18+ | ❌ | Solo servidor de desarrollo local |
| Conexión a internet | — | ❌ | Solo para carga inicial desde GitHub Pages |

### Variables de Entorno

| Variable | Descripción | Requerida | Default |
|---|---|---|---|
| — | — | — | — |

> No se requieren variables de entorno. La aplicación es completamente estática y autocontenida.

---

## 🚀 Instalación

### Opción 1: GitHub Pages (Recomendado — sin instalación)

Accede directamente: **https://ttpsecspa.github.io/gap-ley-marco-21663/**

### Opción 2: Abrir archivo local

```bash
# 1. Clonar el repositorio
git clone https://github.com/ttpsecspa/gap-ley-marco-21663.git

# 2. Entrar al directorio
cd gap-ley-marco-21663

# 3. Abrir directamente en el navegador
# Windows
start gap-analysis.html
# macOS
open gap-analysis.html
# Linux
xdg-open gap-analysis.html
```

### Opción 3: Servidor de desarrollo local

```bash
# 1. Clonar el repositorio
git clone https://github.com/ttpsecspa/gap-ley-marco-21663.git
cd gap-ley-marco-21663

# 2. Iniciar servidor estático (requiere Node.js 18+)
node static-server.mjs

# 3. Abrir en navegador
# http://localhost:3457
```

### Opción 4: Docker (opcional)

```bash
# Servir con cualquier servidor estático
docker run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx:alpine
# Abrir http://localhost:8080/gap-analysis.html
```

---

## ⚡ Uso / Quick Start

### Flujo básico

1. **Abrir** la aplicación (GitHub Pages o local)
2. **Seleccionar contexto** organizacional: TI Corporativo, OT/ICS o Infraestructura Crítica
3. **Click** en "Comenzar Evaluación"
4. **Completar** datos de la empresa (Step 1)
5. **Evaluar** cada uno de los 15 dominios en escala 0-5 (Step 2)
6. **Revisar** resultados: score global, heatmap, brechas (Step 3)
7. **Consultar** Quick Wins priorizados (Step 4)
8. **Analizar** riesgos con exposición UTM (Step 5)
9. **Seguir** plan de remediación en 5 fases (Step 6)
10. **Exportar** informe completo en texto plano

### Escala de evaluación

| Nivel | Descripción |
|---|---|
| **0** | Inexistente — No hay control implementado |
| **1** | Inicial — Control ad-hoc, no documentado |
| **2** | Repetible — Control documentado pero no estandarizado |
| **3** | Definido — Control estandarizado y comunicado |
| **4** | Gestionado — Control medido y monitoreado |
| **5** | Optimizado — Mejora continua implementada |

### Ejemplo de output

```
INFORME GAP ANALYSIS - LEY 21.663
==================================================
Empresa: Empresa Nacional de Energía S.A.
Sector: energia
Fecha: 30-03-2026
Generado por: TTPSEC SPA - Powered by TTPSEC.AI

SCORE GLOBAL: 47%
Gaps Críticos: 4
Exposición: 10.001-20.000 UTM
```

---

## ⚙️ Configuración

### Archivo de configuración

La aplicación no requiere archivos de configuración externos. Toda la configuración está embebida en `gap-analysis.html`.

### Parámetros internos

| Parámetro | Tipo | Default | Descripción |
|---|---|---|---|
| `orgContext` | `string` | `'ti'` | Contexto organizacional (`'ti'`, `'ot'`, `'ic'`) |
| Niveles target | `int` | `3-5` | Nivel objetivo por dominio según calificación de la empresa |
| Fases del plan | `array` | 5 fases | Semanas 1-2, 3-5, 6-12, 13-20, 21-24 |
| Umbrales de score | `int` | `30/50/70/85` | Cortes para clasificación: crítico/bajo/medio/bueno/excelente |
| UTM por gap | `ranges` | Variable | 20.001-40.000 (≥3 críticos), 10.001-20.000, 5.000-10.000, <5.000 |

### Servidor de desarrollo

El archivo `.claude/launch.json` configura los servidores de desarrollo:

```json
{
  "configurations": [
    {
      "name": "gap-analysis",
      "runtimeExecutable": "node",
      "runtimeArgs": ["./static-server.mjs"],
      "port": 3457
    },
    {
      "name": "og-card",
      "runtimeExecutable": "node",
      "runtimeArgs": ["./server.mjs"],
      "port": 3456
    }
  ]
}
```

---

## 📖 Funciones Documentadas

### Funciones Públicas (JavaScript)

#### `startApp()`

Oculta el hero/portada y muestra el contenedor principal de la aplicación.

| Parámetro | Tipo | Descripción |
|---|---|---|
| — | — | Sin parámetros |

**Retorno:** `void`

```javascript
// Llamada desde el botón CTA del hero
startApp();
// Oculta #hero, muestra #app-container y #app-footer
```

---

#### `selectContext(el, ctx)`

Selecciona el contexto organizacional (TI, OT, IC).

| Parámetro | Tipo | Descripción |
|---|---|---|
| `el` | `HTMLElement` | Elemento de la tarjeta de contexto clickeada |
| `ctx` | `string` | Código del contexto: `'ti'`, `'ot'`, `'ic'` |

**Retorno:** `void`

```javascript
selectContext(this, 'ot');
// Marca la tarjeta OT/ICS como seleccionada, actualiza orgContext
```

---

#### `calculate()`

Ejecuta el cálculo completo de la evaluación GAP. Procesa los 15 dominios, genera resultados, heatmap, tabla de brechas, quick wins, riesgos y plan de remediación.

| Parámetro | Tipo | Descripción |
|---|---|---|
| — | — | Lee valores directamente del DOM |

**Retorno:** `void`

**Efectos:** Actualiza score global, heatmap, tabla de resultados, quick wins, riesgos, plan, y navega a Step 3.

---

#### `generateQuickWins(results)`

Genera acciones priorizadas según las brechas detectadas.

| Parámetro | Tipo | Descripción |
|---|---|---|
| `results` | `Array<Object>` | Array de resultados por dominio con `{id, name, ref, level, target, gap, cat}` |

**Retorno:** `void` — Renderiza cards en `#quickwins-grid`

**Lógica de priorización:**
- Gaps en dominio 9 (Delegado) y 10 (Inscripción ANCI) → prioridad máxima
- Gaps ≥3 → Fase 1 inmediata
- Esfuerzo bajo + impacto alto → primero

---

#### `generateRisks(results)`

Genera la matriz de riesgos con exposición UTM.

| Parámetro | Tipo | Descripción |
|---|---|---|
| `results` | `Array<Object>` | Misma estructura que `generateQuickWins` |

**Retorno:** `void` — Renderiza cards en `#risks-grid`

---

#### `generatePlan(results)`

Genera el plan de remediación en 5 fases (24 semanas).

| Parámetro | Tipo | Descripción |
|---|---|---|
| `results` | `Array<Object>` | Misma estructura que `generateQuickWins` |

**Retorno:** `void` — Renderiza timeline en `#plan-timeline`

**Fases:**
| Fase | Semanas | Contenido |
|---|---|---|
| 1: Inmediato | 1-2 | Gaps críticos (≥3) |
| 2: Gobernanza | 3-5 | Dominios de gobernanza |
| 3: Operacional | 6-12 | Reporte y operaciones |
| 4: Técnico | 13-20 | Controles técnicos |
| 5: Validación | 21-24 | Re-evaluación + evidencia para ANCI |

---

#### `exportPlan()`

Exporta el informe completo como archivo `.txt` descargable.

| Parámetro | Tipo | Descripción |
|---|---|---|
| — | — | Lee estado actual del DOM |

**Retorno:** `void` — Descarga archivo `gap-analysis-[empresa].txt`

---

#### `sanitize(str)`

Sanitiza strings para prevenir XSS al insertar en el DOM.

| Parámetro | Tipo | Descripción |
|---|---|---|
| `str` | `string` | String a sanitizar |

**Retorno:** `string` — String con `< > " ' &` reemplazados por entidades HTML

```javascript
sanitize('<script>alert(1)</script>');
// → '&lt;script&gt;alert(1)&lt;/script&gt;'
```

---

#### `goStep(n)`

Navega a un paso específico de la aplicación.

| Parámetro | Tipo | Descripción |
|---|---|---|
| `n` | `number` | Número del paso (1-6) |

**Retorno:** `void`

---

#### `autoComplete()`

Auto-completa campos según las selecciones del formulario (sector, calificación).

| Parámetro | Tipo | Descripción |
|---|---|---|
| — | — | Lee valores del DOM |

**Retorno:** `void`

---

## 🔒 Seguridad

### Modelo de Amenazas

| Protege | De |
|---|---|
| Datos del usuario | Exfiltración vía red (`connect-src: 'none'`) |
| Integridad del DOM | XSS (sanitización + CSP) |
| Sesión del usuario | Clickjacking (`frame-ancestors: 'none'`) |
| Privacidad | Tracking, fingerprinting (sin cookies/analytics) |

### CWE Relevantes

| CWE-ID | Nombre | Mitigación | Estado |
|---|---|---|---|
| CWE-79 | Cross-Site Scripting (XSS) | CSP + `sanitize()` en todo input | ✅ Mitigado |
| CWE-94 | Code Injection | Sin `eval()` / `Function()` | ✅ Mitigado |
| CWE-200 | Information Disclosure | `connect-src 'none'` bloquea toda salida | ✅ Mitigado |
| CWE-1021 | Clickjacking | `frame-ancestors 'none'` + `X-Frame-Options: DENY` | ✅ Mitigado |
| CWE-16 | Security Misconfiguration | CSP completa (8 directivas) | ✅ Mitigado |
| CWE-312 | Cleartext Storage | Sin almacenamiento — solo memoria JS | ✅ Mitigado |
| CWE-319 | Cleartext Transmission | `connect-src 'none'` — imposible transmitir | ✅ Mitigado |
| CWE-352 | CSRF | `form-action 'none'` + sin backend | ⚪ No aplica |
| CWE-89 | SQL Injection | Sin base de datos | ⚪ No aplica |

> Mapeo completo: [docs/CWE_MAPPING.md](docs/CWE_MAPPING.md) (32 CWEs)

### OWASP Top 10:2021

| ID | Categoría | Estado |
|---|---|---|
| A01 | Broken Access Control | ⚪ No aplica (sin auth) — anti-clickjacking preventivo |
| A02 | Cryptographic Failures | ⚪ No aplica — HTTPS vía GitHub Pages |
| A03 | Injection | ✅ Mitigado — CSP + sanitize() + sin eval |
| A04 | Insecure Design | ✅ Zero-trust architecture, privacy by design |
| A05 | Security Misconfiguration | ✅ CSP + 3 security headers |
| A06 | Vulnerable Components | ⚪ No aplica — zero dependencies |
| A07 | Auth Failures | ⚪ No aplica — sin autenticación |
| A08 | Integrity Failures | ✅ CI/CD GitHub Actions, sin CDNs |
| A09 | Logging Failures | ⚪ No aplica — sin backend |
| A10 | SSRF | ⚪ No aplica — `connect-src 'none'` preventivo |

> Mapeo completo: [docs/OWASP_MAPPING.md](docs/OWASP_MAPPING.md)

### Reporte de Vulnerabilidades

**NO** reportes vulnerabilidades como issues públicos. Consulta [SECURITY.md](SECURITY.md) para el proceso de divulgación responsable.

### Hardening para Deployment

Si despliegas en tu propia infraestructura (no GitHub Pages):

1. **HTTPS obligatorio** — nunca servir por HTTP
2. **Headers adicionales** recomendados en tu servidor/reverse proxy:
   ```
   Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
   Permissions-Policy: camera=(), microphone=(), geolocation=()
   Cross-Origin-Opener-Policy: same-origin
   Cross-Origin-Resource-Policy: same-origin
   ```
3. **No modificar la CSP** — `connect-src 'none'` es la garantía principal de privacidad
4. **Servir como archivo estático** — no procesar server-side

---

## 🧪 Testing

### Verificación Manual

```bash
# 1. Iniciar servidor local
node static-server.mjs

# 2. Abrir http://localhost:3457

# 3. Verificar en DevTools (F12):
#    - Console: sin errores
#    - Network: sin requests externos (CSP bloquea)
#    - Application: sin cookies, sin storage
```

### Checklist de Testing

- [ ] Hero se muestra correctamente (logo, badges, contexto, CTA)
- [ ] Click "Comenzar Evaluación" → muestra formulario (Step 1)
- [ ] Completar datos empresa → "Iniciar Evaluación" activo
- [ ] Evaluar 15 dominios → navega a Step 3 con resultados
- [ ] Score global calcula correctamente (promedio ponderado)
- [ ] Heatmap refleja niveles por dominio
- [ ] Quick Wins se generan según gaps detectados
- [ ] Matriz de riesgos muestra UTM correctos
- [ ] Plan de remediación distribuye gaps en fases
- [ ] Exportar genera archivo `.txt` con datos correctos
- [ ] Mobile: layout responsive, nav compacto, CTA full-width
- [ ] Consola del navegador: 0 errores

### Testing de Seguridad

- [ ] Inyectar `<script>alert(1)</script>` en campos de texto → sanitizado
- [ ] Verificar CSP: `fetch('https://example.com')` en consola → bloqueado
- [ ] Verificar anti-clickjacking: embeber en iframe → bloqueado
- [ ] Verificar sin cookies: `document.cookie` → vacío
- [ ] Verificar sin storage: `localStorage.length` → 0

---

## 🤝 Contribución

¡Las contribuciones son bienvenidas! Consulta [CONTRIBUTING.md](CONTRIBUTING.md) para la guía completa.

### Resumen

1. Fork del repositorio
2. Crear rama: `git checkout -b feature/mi-mejora`
3. Commit: `git commit -m 'feat: descripción'`
4. Push: `git push origin feature/mi-mejora`
5. Abrir Pull Request

### Convenciones

- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `style:`, `docs:`, `security:`)
- **Código**: ES5 vanilla JS, sin dependencias externas, sanitizar inputs
- **CSS**: Variables TTPSEC Design System, mobile-first, `150ms ease`
- **Idioma**: UI en español, commits en inglés

---

## 🗺️ Roadmap

- [ ] Exportación a PDF con diseño profesional
- [ ] Modo comparativo (evaluar múltiples períodos)
- [ ] Integración con framework MITRE ATT&CK detallado
- [ ] Soporte para Instrucciones Generales ANCI futuras (N° 5+)
- [ ] Módulo de evidencias (checklist documental por dominio)
- [ ] Dashboard ejecutivo con gráficos radar
- [ ] Soporte multi-idioma (EN, PT)
- [ ] PWA (Progressive Web App) para uso offline completo

---

## 📄 Licencia

Distribuido bajo la **Licencia MIT**. Ver [LICENSE](LICENSE) para el texto completo.

```
MIT License — Copyright (c) 2026 TTPSEC SPA
```

---

## 📬 Contacto / Soporte

**TTPSEC SPA** — Powered by TTPSEC.AI

| Canal | Detalle |
|---|---|
| 🌐 Web | [www.ttpsec.ai](https://www.ttpsec.ai) |
| 💼 LinkedIn | [TTPSEC SPA](https://www.linkedin.com/company/ttpsec) |
| 🐛 Issues | [GitHub Issues](https://github.com/ttpsecspa/gap-ley-marco-21663/issues) |
| 🔒 Seguridad | Ver [SECURITY.md](SECURITY.md) |

### Créditos

- Normativa: Ley 21.663 (BCN), Instrucciones Generales ANCI 1-4, Resolución N° 7
- Marcos: ISO/IEC 27001:2022, NIST CSF 2.0, NIST 800-53 Rev.5, CIS v8.1, ISA/IEC 62443, NERC CIP
- Taxonomía: MITRE ATT&CK

---

<div align="center">

**Hecho con 🛡️ por [TTPSEC SPA](https://www.ttpsec.ai) — Contribuyendo a la ciberseguridad de Chile**

</div>
]]>