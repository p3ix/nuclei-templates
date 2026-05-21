# Personal Nuclei Web Misconfiguration Templates

Plantillas personales para detectar misconfiguraciones web comunes en programas de Bug Bounty.

## Primer bloque: misconfig-web

Esta primera tanda cubre:

- CORS permisivo con `Access-Control-Allow-Credentials: true` y reflejo de un `Origin` arbitrario.
- Host header injection mediante cabeceras de proxy como `X-Forwarded-Host`, `X-Host`, `Forwarded` y similares.
- Candidatos a web cache poisoning cuando una cabecera controlada se refleja en una respuesta cacheable.
- Candidatos a web cache deception en rutas sensibles con sufijos estáticos como `.css`, `.js` o `.png`.
- Open redirect por parámetros comunes (`next`, `url`, `redirect`, `return`, `continue`, etc.).
- Content Security Policy permisiva o fácilmente bypassable.
- Clickjacking en páginas sensibles sin `X-Frame-Options` ni `frame-ancestors`.
- HSTS ausente en hosts sensibles HTTPS.
- Métodos HTTP peligrosos anunciados por `OPTIONS` (`PUT`, `DELETE`, `TRACE`, `CONNECT`, WebDAV).
- Método `TRACE` habilitado con reflejo de cabeceras.
- Cookies de sesión/autenticación sin `Secure`, `HttpOnly` o `SameSite`.
- Páginas sensibles con varias cabeceras de seguridad ausentes.
- `Referrer-Policy` permisiva en páginas sensibles.
- `Permissions-Policy`/`Feature-Policy` permisiva para cámara, micrófono, geolocalización, pago o APIs locales.
- Cabeceras verbosas que filtran framework, versión, backend, upstream o infraestructura interna.
- Stack traces y errores verbosos provocados por rutas malformadas.
- Formularios de login servidos sobre HTTP claro.
- CORS con `Origin: null` y credenciales.
- Preflight CORS permisivo con métodos peligrosos y cabecera `Authorization`.
- Private Network Access permitido para orígenes arbitrarios.
- Variantes de open redirect con payloads protocol-relative, encoded y userinfo.
- Cookies `SameSite=None` sin `Secure`.
- Cookies sensibles con `Domain=.` amplio sobre páginas de sesión.
- Assets JS/CSS sin `X-Content-Type-Options: nosniff`.
- Falta de cabeceras COOP/COEP/CORP en páginas sensibles.
- Downgrade de HTTPS a HTTP en rutas sensibles.
- Fuzzing de parámetros que reflejan un marcador aleatorio en HTML, JSON, redirects o cabeceras para priorizar XSS, redirect, cache poisoning e inyecciones.
- Parámetros `debug`, `trace`, `verbose`, `show_errors` y similares que activan errores verbosos con reflejo del marcador de prueba.
- Fuzzing de JSONP/callbacks legacy con marcador JavaScript aleatorio y verificación de envoltorio JSON-like.

## Targets recomendados

Estas plantillas se lanzan contra hosts HTTP/HTTPS vivos, no contra IPs a ciegas.

- `permissive-cors-credentials.yaml`: APIs, frontends SPA, dominios con sesión, subdominios `api`, `app`, `mobile`, `graphql`, `admin`.
- `host-header-injection-reflection.yaml`: frontends detrás de proxies/CDN, apps con login/reset password, dominios con redirects absolutos.
- `cache-poisoning-header-reflection.yaml`: hosts detrás de CDN/cache (`Cloudflare`, `Fastly`, `Akamai`, `CloudFront`, `Varnish`, `nginx cache`) y páginas públicas cacheables.
- `cache-deception-candidates.yaml`: apps con rutas autenticadas o semiautenticadas como `account`, `profile`, `dashboard`, `settings`, `admin`, `billing`.
- `open-redirect-common-params.yaml`: login, logout, SSO, OAuth, auth callbacks, invitaciones, checkout y rutas que reciben parámetros de navegación.
- `permissive-csp-policy.yaml`: cualquier frontend web; prioriza dominios con login, paneles, checkout y datos personales.
- `clickjacking-sensitive-panels.yaml`: paneles `admin`, `login`, `account`, `dashboard`, `billing`, `checkout`, `portal`, `console`.
- `missing-hsts-sensitive-hosts.yaml`: solo hosts HTTPS sensibles: `auth.*`, `login.*`, `sso.*`, `account.*`, `billing.*`, `pay.*`, `checkout.*`, `admin.*`, `portal.*`.
- `dangerous-http-methods-enabled.yaml`: hosts web vivos, especialmente `api`, `upload`, `files`, `assets`, `webdav`, paneles y orígenes sin CDN.
- `trace-method-enabled.yaml`: cualquier host HTTP/HTTPS vivo; suele ser más útil en servidores legacy o detrás de proxies antiguos.
- `insecure-cookie-flags-sensitive.yaml`: login, auth, account, dashboard, checkout, billing, admin y portales con sesión.
- `missing-security-headers-sensitive.yaml`: páginas sensibles donde un finding de hardening pueda acompañar a impacto real.
- `permissive-referrer-policy.yaml`: rutas con tokens en URL, callbacks, invitaciones, checkout, SSO y account flows.
- `permissive-permissions-policy.yaml`: frontends con funcionalidades de cámara, micrófono, geolocalización, pagos o clipboard.
- `verbose-internal-response-headers.yaml`: todo el scope vivo; prioriza staging/dev, APIs, CDNs y apps detrás de balanceadores.
- `stack-trace-error-disclosure.yaml`: staging/dev, APIs, apps custom y entornos donde esté permitido provocar errores benignos.
- `insecure-login-form-http.yaml`: únicamente targets `http://` que no redirigen claramente a HTTPS.
- `cors-null-origin-credentials.yaml`: APIs y frontends con sesión, especialmente endpoints que devuelven datos de usuario.
- `cors-preflight-arbitrary-methods.yaml`: APIs, GraphQL, uploads, endpoints admin y rutas con métodos no-GET.
- `cors-private-network-access.yaml`: apps internas expuestas, dashboards, endpoints `status`, `health`, `metrics` y paneles detrás de VPN/CDN.
- `open-redirect-bypass-variants.yaml`: login, SSO/OAuth, logout, callback, invite y checkout.
- `cookie-samesite-none-without-secure.yaml`: login, SSO, account, dashboard, callbacks OAuth y APIs con cookies.
- `broad-cookie-domain-sensitive.yaml`: scopes con muchos subdominios, staging/dev mezclado con producción o subdominios user-controlled.
- `missing-nosniff-on-script-style.yaml`: frontends con uploads, CDNs, assets cacheados o rutas donde haya content-type confusion.
- `missing-cross-origin-isolation-headers.yaml`: apps con datos sensibles, browser APIs potentes, dashboards y paneles internos.
- `https-to-http-redirect-sensitive.yaml`: solo `https://` targets sensibles; login, account, checkout, admin, SSO.
- `fuzzing-reflected-input-candidates.yaml`: barridos amplios de recon para encontrar parámetros con reflexión real antes de profundizar con payloads manuales.
- `debug-parameter-error-disclosure.yaml`: apps custom, staging/dev, APIs y endpoints donde el scope permita fuzzing benigno de parámetros de debug.
- `jsonp-callback-reflection-candidates.yaml`: APIs legacy, endpoints de búsqueda/sugerencias/configuración y rutas antiguas que todavía podrían soportar callbacks JSONP.

## Uso

```bash
nuclei -l targets.txt -t misconfig-web-templates/ -rl 30 -c 15
```

Para lanzar solo checks de bajo ruido sobre todo el scope:

```bash
nuclei -l targets.txt -t misconfig-web-templates/dangerous-http-methods-enabled.yaml -t misconfig-web-templates/trace-method-enabled.yaml -t misconfig-web-templates/verbose-internal-response-headers.yaml
```

Para lanzar solo sobre hosts sensibles HTTPS:

```bash
rg '^https://(auth|login|sso|account|billing|pay|checkout|admin|portal)\\.' targets.txt | nuclei -t misconfig-web-templates/missing-hsts-sensitive-hosts.yaml
```

Para detectar login sobre HTTP:

```bash
rg '^http://' targets.txt | nuclei -t misconfig-web-templates/insecure-login-form-http.yaml
```

Para revisar CORS ampliado:

```bash
nuclei -l targets.txt -t misconfig-web-templates/permissive-cors-credentials.yaml -t misconfig-web-templates/cors-null-origin-credentials.yaml -t misconfig-web-templates/cors-preflight-arbitrary-methods.yaml -t misconfig-web-templates/cors-private-network-access.yaml
```

Para revisar redirects y downgrade:

```bash
nuclei -l targets.txt -t misconfig-web-templates/open-redirect-common-params.yaml -t misconfig-web-templates/open-redirect-bypass-variants.yaml
rg '^https://' targets.txt | nuclei -t misconfig-web-templates/https-to-http-redirect-sensitive.yaml
```

Para recon de parámetros reflejados y debug:

```bash
nuclei -l targets.txt -t misconfig-web-templates/fuzzing-reflected-input-candidates.yaml -t misconfig-web-templates/debug-parameter-error-disclosure.yaml -rl 20 -c 10
```

Para callbacks JSONP legacy:

```bash
nuclei -l targets.txt -t misconfig-web-templates/jsonp-callback-reflection-candidates.yaml -rl 20 -c 10
```
