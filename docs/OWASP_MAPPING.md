<![CDATA[# 🔒 Mapeo OWASP Top 10 — GAP Analysis Ley 21.663

Este documento mapea los controles de seguridad implementados en la aplicación al [OWASP Top 10 (2021)](https://owasp.org/Top10/).

---

## OWASP Top 10:2021 — Mapeo Completo

| ID | Categoría | Aplica? | Controles Implementados | Pendiente |
|---|---|---|---|---|
| **A01** | Broken Access Control | ⚪ No aplica | `frame-ancestors 'none'` + `X-Frame-Options: DENY` (anti-clickjacking preventivo) | Ninguno — sin auth/authz |
| **A02** | Cryptographic Failures | ⚪ No aplica | HTTPS via GitHub Pages; sin datos en tránsito propio ni en reposo | Ninguno — sin crypto |
| **A03** | Injection | 🟡 Parcial | CSP (8 directivas) + `sanitize()` en todo input + sin `eval()`/`Function()` + `connect-src 'none'` | Considerar migrar a nonces CSP en futuro |
| **A04** | Insecure Design | 🟢 Aplica | Zero-trust architecture; privacy by design; minimal attack surface; defense in depth | Ninguno |
| **A05** | Security Misconfiguration | 🟢 Aplica | CSP completa + `X-Content-Type-Options: nosniff` + `X-Frame-Options: DENY` + `Referrer-Policy: no-referrer` | Agregar `Strict-Transport-Security` en deploys propios |
| **A06** | Vulnerable and Outdated Components | ⚪ No aplica | Zero dependencies — sin librerías, CDNs ni npm packages | Ninguno |
| **A07** | Identification and Authentication Failures | ⚪ No aplica | Sin autenticación, sesiones, tokens ni credenciales | Ninguno |
| **A08** | Software and Data Integrity Failures | 🟡 Parcial | GitHub Actions CI/CD automatizado; sin CDNs externos; `default-src 'self'` | Considerar SRI si se agregan scripts externos |
| **A09** | Security Logging and Monitoring Failures | ⚪ No aplica | Sin backend ni infraestructura que monitorear; GitHub Pages provee logging básico | Ninguno |
| **A10** | Server-Side Request Forgery (SSRF) | ⚪ No aplica | Sin servidor; `connect-src 'none'` previene requests client-side | Ninguno |

---

## Detalle por Categoría

### A01:2021 — Broken Access Control

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Sin sistema de autenticación ni autorización |
| **CWEs relacionados** | CWE-200, CWE-284, CWE-862, CWE-1021 |
| **Control preventivo** | Anti-clickjacking: `frame-ancestors 'none'` + `X-Frame-Options: DENY` |
| **Pendiente** | Ninguno |

---

### A02:2021 — Cryptographic Failures

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Sin datos en tránsito (propio) ni en reposo |
| **CWEs relacionados** | CWE-312, CWE-319 |
| **Controles** | Deploy HTTPS via GitHub Pages; datos solo en memoria JS |
| **Pendiente** | Ninguno |

---

### A03:2021 — Injection

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | XSS es relevante — app web con inputs de usuario |
| **CWEs relacionados** | CWE-79, CWE-94, CWE-95, CWE-116 |
| **Controles** | 1. CSP con `script-src 'self' 'unsafe-inline'` 2. `sanitize()` reemplaza `< > " ' &` en todo input 3. Sin `eval()`/`Function()`/`new Function()` 4. `connect-src 'none'` impide exfiltración post-XSS |
| **Pendiente** | Considerar migración a CSP con nonces para eliminar `unsafe-inline` |

---

### A04:2021 — Insecure Design

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Diseño seguro aplicado desde el inicio |
| **CWEs relacionados** | CWE-693 |
| **Principios aplicados** | Zero-trust (imposible transmitir datos), privacy by design (sin persistencia), minimal surface (single-file, zero deps), defense in depth (CSP + headers + sanitize) |
| **Pendiente** | Ninguno |

---

### A05:2021 — Security Misconfiguration

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Configuración de seguridad activa y verificable |
| **CWEs relacionados** | CWE-16 |
| **Headers configurados** | CSP (8 directivas), `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `Referrer-Policy: no-referrer` |
| **Pendiente** | `Strict-Transport-Security` y `Permissions-Policy` para deploys propios (GitHub Pages los maneja internamente) |

---

### A06:2021 — Vulnerable and Outdated Components

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Sin dependencias externas |
| **Controles** | Política de zero dependencies — todo el código es propio, contenido en un único archivo |
| **Pendiente** | Ninguno — mantener política de no agregar dependencias |

---

### A07:2021 — Identification and Authentication Failures

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Sin autenticación |
| **CWEs relacionados** | CWE-287, CWE-306, CWE-384 |
| **Pendiente** | Ninguno — no se requiere autenticación |

---

### A08:2021 — Software and Data Integrity Failures

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Integridad del código desplegado |
| **CWEs relacionados** | CWE-502 |
| **Controles** | 1. Deploy via GitHub Actions (pipeline inmutable) 2. Sin CDNs ni recursos externos 3. `default-src 'self'` — solo carga desde mismo origen 4. Sin deserialización |
| **Pendiente** | Subresource Integrity (SRI) si se agregan scripts externos en el futuro |

---

### A09:2021 — Security Logging and Monitoring Failures

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Sin backend ni infraestructura que monitorear |
| **CWEs relacionados** | CWE-532 |
| **Pendiente** | Ninguno — GitHub Pages provee logging a nivel de infraestructura |

---

### A10:2021 — Server-Side Request Forgery (SSRF)

| Aspecto | Detalle |
|---|---|
| **Aplicabilidad** | Sin servidor |
| **CWEs relacionados** | CWE-918 |
| **Control preventivo** | `connect-src 'none'` previene requests desde el cliente a cualquier destino |
| **Pendiente** | Ninguno |

---

## Resumen Visual

```
A01 Broken Access Control      ⚪ ████████████ N/A (sin auth)
A02 Cryptographic Failures     ⚪ ████████████ N/A (HTTPS)
A03 Injection                  🟡 ██████████░░ Mitigado (CSP + sanitize)
A04 Insecure Design            🟢 ████████████ Zero-trust by design
A05 Security Misconfiguration  🟢 ████████████ CSP + headers
A06 Vulnerable Components      ⚪ ████████████ N/A (zero deps)
A07 Auth Failures              ⚪ ████████████ N/A (sin auth)
A08 Integrity Failures         🟡 ██████████░░ Mitigado (CI/CD)
A09 Logging Failures           ⚪ ████████████ N/A (sin backend)
A10 SSRF                       ⚪ ████████████ N/A (sin server)
```

### Leyenda

| Símbolo | Significado |
|---|---|
| ⚪ **No aplica** | Categoría no relevante para la arquitectura |
| 🟡 **Parcial** | Algunos aspectos aplican y están mitigados |
| 🟢 **Aplica** | Categoría relevante y completamente controlada |
| ✅ **Mitigado** | Controles implementados y funcionando |

---

## 📚 Referencias

- [OWASP Top 10:2021](https://owasp.org/Top10/)
- [OWASP Application Security Verification Standard (ASVS) v4.0](https://owasp.org/www-project-application-security-verification-standard/)
- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [Mapeo CWE](CWE_MAPPING.md)
- [Política de Seguridad](../SECURITY.md)
]]>