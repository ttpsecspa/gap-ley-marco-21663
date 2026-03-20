# Politica de Seguridad - TTPSEC SPA

## Arquitectura de seguridad

Este software es una aplicacion **100% client-side** (lado del cliente):

- **No transmite datos**: Toda la informacion ingresada se procesa localmente en el navegador
- **No almacena datos**: Al cerrar o recargar la pagina, los datos se pierden
- **No tiene backend**: No hay servidor, base de datos, ni APIs externas
- **No usa cookies ni tracking**: Sin analytics, sin cookies, sin rastreo

## Medidas implementadas

### Content Security Policy (CSP)
- `default-src 'self'` - Solo recursos propios
- `connect-src 'none'` - Prohibe conexiones externas (fetch, XHR)
- `form-action 'none'` - Prohibe envio de formularios a servidores
- `frame-ancestors 'none'` - Prohibe embedding en iframes (anti-clickjacking)

### Headers de seguridad
- `X-Content-Type-Options: nosniff` - Previene MIME sniffing
- `X-Frame-Options: DENY` - Previene clickjacking
- `Referrer-Policy: no-referrer` - No envia referrer a sitios externos

### Sanitizacion
- Todo input del usuario se sanitiza antes de usarse
- No se usa `eval()` ni `Function()`
- innerHTML solo se usa con datos hardcodeados, nunca con input del usuario

## Datos sensibles

- Los PDFs incluidos son **documentos publicos** del Diario Oficial de Chile y la Biblioteca del Congreso Nacional
- No contienen informacion confidencial
- El directorio `.claude/` (agentes y skills) esta excluido del repositorio

## Reporte de vulnerabilidades

Si encuentras una vulnerabilidad de seguridad, reportala de forma responsable:
- **No** abras un issue publico
- Contacta a TTPSEC SPA directamente

## Licencia

Propiedad de TTPSEC SPA. Powered by TTPSEC.AI.
