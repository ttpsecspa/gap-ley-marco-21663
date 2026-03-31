<![CDATA[# 🔒 Mapeo CWE — GAP Analysis Ley 21.663

Este documento mapea los controles de seguridad implementados en la aplicación a las debilidades del [Common Weakness Enumeration (CWE)](https://cwe.mitre.org/).

---

## Controles Implementados vs. CWE

### Prevención de Inyección

| CWE-ID | Nombre | Relevancia para este proyecto | Mitigación | Estado |
|---|---|---|---|---|
| CWE-79 | Cross-Site Scripting (XSS) | Alta — app web con inputs de usuario | CSP `script-src` + función `sanitize()` reemplaza `< > " ' &` | ✅ Mitigado |
| CWE-94 | Improper Control of Code Generation (Code Injection) | Alta — JavaScript client-side | Sin `eval()`, sin `Function()`, sin `new Function()` | ✅ Mitigado |
| CWE-95 | Improper Neutralization of Directives in Dynamically Evaluated Code (Eval Injection) | Alta — JavaScript client-side | Prohibición absoluta de eval en todo el codebase | ✅ Mitigado |
| CWE-116 | Improper Encoding or Escaping of Output | Media — output dinámico en DOM | Sanitización via `sanitize()` antes de inserción, templates con datos sanitizados | ✅ Mitigado |

### Exposición de Información

| CWE-ID | Nombre | Relevancia para este proyecto | Mitigación | Estado |
|---|---|---|---|---|
| CWE-200 | Exposure of Sensitive Information to an Unauthorized Actor | Alta — datos organizacionales en formulario | `connect-src 'none'` bloquea toda transmisión + `Referrer-Policy: no-referrer` | ✅ Mitigado |
| CWE-209 | Generation of Error Message Containing Sensitive Information | Baja — sin errores server-side | Sin stack traces expuestos, errores genéricos | ✅ Mitigado |
| CWE-532 | Insertion of Sensitive Information into Log File | Baja — sin logging | Sin `console.log` de datos del usuario en producción | ✅ Mitigado |

### Configuración de Seguridad

| CWE-ID | Nombre | Relevancia para este proyecto | Mitigación | Estado |
|---|---|---|---|---|
| CWE-16 | Configuration | Media — headers y CSP deben estar correctos | `X-Content-Type-Options: nosniff` + CSP completa | ✅ Mitigado |
| CWE-693 | Protection Mechanism Failure | Media — defensa en profundidad | CSP (8 directivas) + 3 security headers + sanitización | ✅ Mitigado |
| CWE-1021 | Improper Restriction of Rendered UI Layers (Clickjacking) | Media — podría ser embebida en iframe malicioso | `frame-ancestors 'none'` + `X-Frame-Options: DENY` | ✅ Mitigado |

### Manejo de Datos

| CWE-ID | Nombre | Relevancia para este proyecto | Mitigación | Estado |
|---|---|---|---|---|
| CWE-312 | Cleartext Storage of Sensitive Information | Media — datos organizacionales temporales | Sin almacenamiento — datos solo en memoria JS de la sesión | ✅ Mitigado |
| CWE-319 | Cleartext Transmission of Sensitive Information | Alta — datos no deben salir del navegador | `connect-src 'none'` — imposible transmitir | ✅ Mitigado |
| CWE-359 | Exposure of Private Personal Information to an Unauthorized Actor | Media — RUT y datos de empresa | Sin persistencia, sin cookies, sin tracking, sin terceros | ✅ Mitigado |

### Validación de Entrada

| CWE-ID | Nombre | Relevancia para este proyecto | Mitigación | Estado |
|---|---|---|---|---|
| CWE-20 | Improper Input Validation | Alta — múltiples campos de formulario | Sanitización de todos los inputs con `sanitize()` | ✅ Mitigado |
| CWE-74 | Improper Neutralization of Special Elements in Output (Injection) | Alta — datos se insertan en DOM | Sin concatenación directa de inputs en HTML sin sanitizar | ✅ Mitigado |

---

## CWE Relevantes al Stack Web — No Aplicables por Arquitectura

| CWE-ID | Nombre | Relevancia típica web | Razón de No Aplicabilidad |
|---|---|---|---|
| CWE-89 | SQL Injection | Alta | Sin base de datos |
| CWE-352 | Cross-Site Request Forgery (CSRF) | Alta | `form-action 'none'` + sin backend + sin sesiones |
| CWE-918 | Server-Side Request Forgery (SSRF) | Alta | Sin servidor |
| CWE-287 | Improper Authentication | Alta | Sin sistema de autenticación |
| CWE-502 | Deserialization of Untrusted Data | Media | Sin deserialización server-side |
| CWE-284 | Improper Access Control | Alta | Sin recursos protegidos por autorización |
| CWE-311 | Missing Encryption of Sensitive Data | Media | HTTPS via GitHub Pages + sin datos en tránsito propio |
| CWE-346 | Origin Validation Failure | Media | Sin backend que valide orígenes |

---

## CWE Relevantes a OT/ICS — No Aplicables por Arquitectura

> La herramienta **evalúa** entornos OT/ICS pero **no opera** en ellos. Estos CWE aplican a los sistemas evaluados, no a la herramienta.

| CWE-ID | Nombre | Relevancia para sistemas OT evaluados | Estado en la herramienta |
|---|---|---|---|
| CWE-319 | Cleartext Transmission | Alta en protocolos OT (Modbus, DNP3) | ⚪ N/A — la herramienta no transmite datos |
| CWE-306 | Missing Authentication for Critical Function | Alta en HMI/SCADA | ⚪ N/A — sin funciones críticas |
| CWE-400 | Uncontrolled Resource Consumption | Media en PLCs/RTUs | ⚪ N/A — app web estática |

---

## CWE de Sistema/CLI — No Aplicables

| CWE-ID | Nombre | Razón de No Aplicabilidad |
|---|---|---|
| CWE-78 | OS Command Injection | Sin ejecución de comandos server-side |
| CWE-22 | Path Traversal | Sin acceso a filesystem |
| CWE-732 | Incorrect Permission Assignment for Critical Resource | Sin procesos ni archivos críticos |
| CWE-798 | Use of Hard-coded Credentials | Sin credenciales en el código |
| CWE-250 | Execution with Unnecessary Privileges | Sin procesos privilegiados |
| CWE-269 | Improper Privilege Management | Sin sistema de roles/permisos |
| CWE-306 | Missing Authentication for Critical Function | Sin backend que proteger |
| CWE-384 | Session Fixation | Sin sesiones |
| CWE-611 | Improper Restriction of XML External Entity Reference | Sin parsing XML |
| CWE-862 | Missing Authorization | Sin recursos que requieran autorización |
| CWE-90 | LDAP Injection | Sin directorio LDAP |
| CWE-91 | XML Injection | Sin procesamiento XML |

---

## Resumen de Cobertura

| Categoría | CWEs Mitigados | CWEs No Aplicables | Total Mapeados |
|---|---|---|---|
| Inyección | 4 | 5 | 9 |
| Exposición de información | 3 | 2 | 5 |
| Configuración | 3 | 0 | 3 |
| Manejo de datos | 3 | 1 | 4 |
| Validación de entrada | 2 | 1 | 3 |
| Autenticación/Autorización | 0 | 4 | 4 |
| OT/ICS | 0 | 3 | 3 |
| Sistema/CLI | 0 | 7 | 7 |
| **Total** | **15** | **23** | **38** |

---

## Nota sobre `unsafe-inline`

La directiva `script-src 'self' 'unsafe-inline'` es necesaria debido a la arquitectura single-file donde JavaScript está embebido en el HTML. Esto se mitiga mediante:

1. **Sin `eval()`** — ningún camino de ejecución dinámica de código
2. **Sanitización de inputs** — ningún input del usuario llega sin sanitizar al DOM
3. **Sin dependencias externas** — no hay código de terceros que pueda explotar `unsafe-inline`
4. **`connect-src 'none'`** — incluso con XSS exitoso, no hay canal de exfiltración

---

## 📚 Referencias

- [CWE - Common Weakness Enumeration](https://cwe.mitre.org/)
- [CWE/SANS Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)
- [Mapeo OWASP Top 10](OWASP_MAPPING.md)
- [Política de Seguridad](../SECURITY.md)
]]>