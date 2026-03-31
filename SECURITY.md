# 🔒 Política de Seguridad

## Versiones Soportadas

| Versión | Soporte | Notas |
|---|---|---|
| Última en `main` | ✅ Parches de seguridad activos | Versión recomendada |
| GitHub Pages deploy | ✅ Auto-deploy desde `main` | Siempre actualizado |
| Versiones anteriores | ❌ Sin soporte | Actualizar a `main` |

---

## Arquitectura de Seguridad

GAP Analysis Ley 21.663 es una aplicación **100% client-side** diseñada con un modelo de seguridad zero-trust respecto a datos del usuario.

### Principios Fundamentales

| Principio | Implementación | Garantía |
|---|---|---|
| **No transmite datos** | `connect-src 'none'` en CSP | Bloqueo a nivel de navegador |
| **No almacena datos** | Sin localStorage, sessionStorage, IndexedDB, cookies | Datos solo en memoria JS |
| **No tiene backend** | Sin servidor, BD ni APIs | Sin superficie server-side |
| **No rastrea usuarios** | Sin analytics, fingerprinting ni tracking pixels | Privacy by design |

---

## Controles de Seguridad Implementados

### Content Security Policy (CSP)

| Directiva | Valor | Propósito |
|---|---|---|
| `default-src` | `'self'` | Solo recursos del mismo origen |
| `script-src` | `'self' 'unsafe-inline'` | Scripts locales (single-file) |
| `style-src` | `'self' 'unsafe-inline'` | Estilos locales (single-file) |
| `img-src` | `'self' data:` | Imágenes locales y base64 |
| `connect-src` | `'none'` | **Bloquea toda conexión externa** |
| `form-action` | `'none'` | Prohíbe envío de formularios |
| `frame-ancestors` | `'none'` | Anti-clickjacking |
| `base-uri` | `'self'` | Previene inyección de base URI |

### Headers de Seguridad

| Header | Valor | Mitigación |
|---|---|---|
| `X-Content-Type-Options` | `nosniff` | Previene MIME type sniffing |
| `X-Frame-Options` | `DENY` | Previene clickjacking (complemento) |
| `Referrer-Policy` | `no-referrer` | No envía referrer a sitios externos |

### Sanitización de Inputs

- Función `sanitize()` aplicada a todo input del usuario antes de inserción en DOM
- Reemplaza `< > " ' &` por entidades HTML
- No se utiliza `eval()`, `Function()` ni `new Function()`
- `innerHTML` solo se usa con templates hardcodeados + datos sanitizados

---

## Modelo de Amenazas

### Amenazas Mitigadas

| Amenaza | CWE | Control | Nivel de Confianza |
|---|---|---|---|
| Cross-Site Scripting (XSS) | CWE-79 | CSP + sanitize() | Alto |
| Clickjacking | CWE-1021 | `frame-ancestors 'none'` + `X-Frame-Options` | Alto |
| Data exfiltration | CWE-200 | `connect-src 'none'` | Alto |
| MIME confusion | CWE-16 | `X-Content-Type-Options: nosniff` | Alto |
| Code injection | CWE-94 | Sin eval/Function, CSP | Alto |
| Information leakage via referrer | CWE-200 | `Referrer-Policy: no-referrer` | Alto |
| Form hijacking | CWE-352 | `form-action 'none'` | Alto |
| Cleartext storage | CWE-312 | Sin persistencia | Alto |
| Cleartext transmission | CWE-319 | `connect-src 'none'` | Alto |

### Amenazas No Aplicables (por arquitectura)

| Amenaza | Razón |
|---|---|
| SQL Injection (CWE-89) | Sin base de datos |
| Authentication bypass (CWE-287) | Sin autenticación |
| Session hijacking (CWE-384) | Sin sesiones ni cookies |
| SSRF (CWE-918) | Sin backend |
| File upload vulnerabilities | Sin upload |
| Deserialization (CWE-502) | Sin deserialización server-side |
| OS Command Injection (CWE-78) | Sin ejecución de comandos |

### Superficie de Ataque

La superficie de ataque es **mínima** por diseño:

1. **Único punto de entrada**: `gap-analysis.html` (archivo estático)
2. **Sin endpoints**: No hay APIs, webhooks ni servicios
3. **Sin dependencias externas**: No hay CDNs, librerías de terceros ni recursos remotos
4. **Sin estado persistente**: Cada sesión empieza limpia

### Nota sobre `unsafe-inline`

La directiva `script-src 'self' 'unsafe-inline'` es necesaria por la arquitectura single-file. Se mitiga mediante:

1. **Sin `eval()`** — ningún camino de ejecución dinámica
2. **Sanitización** — inputs nunca llegan sin sanitizar al DOM
3. **Sin dependencias** — no hay código de terceros que explote inline
4. **`connect-src 'none'`** — incluso con XSS exitoso, no hay exfiltración

---

## Datos y Privacidad

### Datos Procesados

| Dato | Almacenamiento | Persistencia | Transmisión |
|---|---|---|---|
| Nombre empresa, RUT, sector | Memoria JS | Ninguna (se pierde al cerrar) | Imposible (`connect-src: 'none'`) |
| Respuestas de evaluación (0-5) | Memoria JS | Ninguna | Imposible |
| Resultados calculados | Memoria JS + DOM | Ninguna | Imposible |
| Archivo exportado (.txt) | Descarga local | Archivo local del usuario | N/A |

### Documentos Incluidos

Los 7 PDFs son **documentos públicos** del Diario Oficial de Chile y la Biblioteca del Congreso Nacional. No contienen información confidencial.

---

## 🚨 Reporte de Vulnerabilidades

### Proceso de Divulgación Coordinada

Si descubres una vulnerabilidad de seguridad, sigue este proceso de **coordinated disclosure**:

1. **NO** abras un issue público
2. **NO** publiques detalles en redes sociales ni foros
3. Reporta a través de uno de estos canales:
   - Email: `security@ttpsec.ai`
   - Usa la [plantilla de seguridad](.github/ISSUE_TEMPLATE/security_vulnerability.md) como guía
4. Incluye: descripción, pasos para reproducir, impacto, PoC (si existe)

### PGP Key (opcional)

```
Fingerprint: [PENDIENTE — Se publicará cuando esté disponible]
```

> Si necesitas comunicación cifrada, solicita la clave PGP por email antes de enviar detalles sensibles.

### Timeline de Respuesta

| Fase | Plazo | Descripción |
|---|---|---|
| Acuse de recibo | 48 horas | Confirmación de recepción del reporte |
| Evaluación inicial | 5 días hábiles | Triaje y clasificación de severidad |
| Resolución (Crítica) | 7 días hábiles | Parche y deploy |
| Resolución (Alta) | 14 días hábiles | Parche y deploy |
| Resolución (Media/Baja) | 30 días hábiles | Parche en siguiente release |
| Divulgación pública | 90 días | Después de la resolución, o 90 días máximo |

### Política de Disclosure

- **Coordinated disclosure**: El reporter acuerda no divulgar públicamente hasta que el parche esté disponible, con un máximo de **90 días** desde el reporte
- Si no hay respuesta en 14 días, el reporter puede escalar
- Crédito completo al reporter en el advisory (si lo desea)

### Alcance

| En alcance | Fuera de alcance |
|---|---|
| `gap-analysis.html` | PDFs normativos (docs públicos) |
| `index.html` | Infraestructura de GitHub Pages |
| `og-card.html` | Ataques que requieren acceso físico |
| GitHub Actions workflow (`pages.yml`) | Ingeniería social |
| CSP y security headers | Navegadores sin soporte de CSP (<2%) |

---

## 🏆 Hall of Fame

Agradecemos a quienes han reportado vulnerabilidades de forma responsable:

| Reporter | Fecha | Severidad | Estado |
|---|---|---|---|
| *Aún no hay reportes* | — | — | — |

> ¿Encontraste algo? Tu nombre puede estar aquí. Reporta responsablemente.

---

## 📚 Cumplimiento y Referencias

- [Mapeo CWE completo](docs/CWE_MAPPING.md) — 32 CWEs mapeados (15 mitigados + 17 no aplicables)
- [Mapeo OWASP Top 10](docs/OWASP_MAPPING.md) — 10/10 categorías cubiertas
- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [CWE/SANS Top 25](https://cwe.mitre.org/top25/)

---

## 📄 Licencia

MIT License — Copyright (c) 2026 TTPSEC SPA. Powered by TTPSEC.AI.
