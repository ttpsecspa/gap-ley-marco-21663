# Changelog

Todos los cambios notables en este proyecto se documentan en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es/1.0.0/) y este proyecto adhiere a [Versionado Semántico](https://semver.org/lang/es/).

---

## [1.4] - 2026-03-30

### Added
- README.md profesional: badges (MIT, OWASP, CWE, GitHub Pages), tabla resumen, diagrama ASCII de arquitectura, requisitos previos, instalación (4 opciones), quick start, configuración, 9 funciones JS documentadas, sección de seguridad con CWE/OWASP inline, testing checklist, roadmap
- SECURITY.md completo: versiones soportadas, modelo de amenazas (9 mitigadas + 7 N/A), CSP detallada, proceso de divulgación coordinada (90 días), PGP placeholder, timeline de respuesta (48h-30d), alcance, Hall of Fame, hardening para deployment propio
- CONTRIBUTING.md: código de conducta, 7 tipos de contribución, setup paso a paso, convenciones CSS/HTML/JS con ejemplos ✅/❌, Conventional Commits con tabla, checklist de PR (10 items)
- CHANGELOG.md con historial completo v1.0 → v1.4 (Keep a Changelog)
- .github/ISSUE_TEMPLATE/bug_report.md — con entorno, consola, checklist
- .github/ISSUE_TEMPLATE/feature_request.md — con contexto normativo (Ley/ANCI/ISO/NIST)
- .github/ISSUE_TEMPLATE/security_vulnerability.md — con severidad, PoC, CWE/OWASP refs
- docs/CWE_MAPPING.md — 38 CWEs mapeados (15 mitigados + 23 N/A), incluye web, OT/ICS y CLI
- docs/OWASP_MAPPING.md — 10/10 OWASP Top 10:2021, detalle por categoría, resumen visual ASCII, columna "Pendiente"

## [1.3] - 2026-03-30

### Added
- Logo TTPSEC en base64 (32x32) embebido en favicon, nav, hero y footer
- Hero/portada completa con badges semánticos, feature cards y context selector
- Selector de contexto organizacional: TI Corporativo, OT/ICS, Infraestructura Crítica
- Footer rico con referencias ANCI, marcos internacionales y badges de privacidad
- Versión y fecha de build visibles en nav y footer

### Changed
- Diseño homologado al TTPSEC Style Guide (rounded-2xl, shadow-sm/md, transitions 150ms)
- Versión mobile mejorada: nav compacto, feature cards horizontales, CTA full-width

### Fixed
- Logo base64 corruido (40x40) reemplazado por versión funcional (32x32)
- Hero no visible por footer empujando contenido fuera de pantalla

## [1.2] - 2026-03-30

### Added
- Disclaimer legal en banner superior
- Caracteres UTF-8 corregidos en toda la aplicación

### Fixed
- Acentos y caracteres especiales en español (tildes, eñes)

## [1.1] - 2026-03-30

### Added
- Logo TTPSEC como favicon e ícono en nav
- Badge de versión en navegación

### Changed
- Encabezado MIT License en todos los archivos HTML

## [1.0] - 2026-03-30

### Added
- Aplicación GAP Analysis completa con 15 dominios de evaluación
- Dashboard de resultados con scoring por dominio
- Módulo Quick Wins con priorización por impacto/esfuerzo
- Matriz de riesgos con clasificación automática
- Plan de remediación por fases
- Content Security Policy (CSP) estricta
- Headers de seguridad (X-Content-Type-Options, X-Frame-Options, Referrer-Policy)
- Sanitización de inputs del usuario
- SECURITY.md inicial
- LICENSE (MIT)
- GitHub Actions workflow para deploy a GitHub Pages
- Página index.html con redirect
- Tarjeta OG para redes sociales

---

## Convenciones

- **Added**: Nuevas funcionalidades
- **Changed**: Cambios en funcionalidad existente
- **Fixed**: Correcciones de bugs
- **Security**: Cambios relacionados con seguridad
- **Removed**: Funcionalidad eliminada
- **Deprecated**: Funcionalidad marcada para eliminación futura
