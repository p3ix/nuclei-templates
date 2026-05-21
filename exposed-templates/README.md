# Personal Nuclei Exposed Templates

Templates personales para detectar archivos, metadatos y artefactos expuestos en programas de Bug Bounty.

## Primer bloque: exposed

Esta primera tanda cubre:

- Variables de entorno y ficheros `.env`.
- Configuraciones con secretos.
- Metadatos VCS (`.git`, `.svn`, `.hg`, `.bzr`).
- Credenciales cloud y estados IaC.
- Ficheros CI/CD.
- Backups y artefactos comprimidos.
- Logs y salidas de debug.
- Directory listing.
- Sourcemaps JavaScript.
- Manifests y lockfiles de dependencias.
- Swagger, OpenAPI, GraphQL, Postman y documentación API.
- Endpoints de debug y diagnóstico de frameworks.
- Ficheros IDE/editor/workspace.
- Leaks sensibles de CMS.
- Artefactos móviles Android/iOS, deep links y configs móviles.
- Permutaciones agresivas de nombres de backup por dominio, entorno y fecha.
- Paneles web de administración de bases de datos y consolas relacionadas.
- Listados y configuraciones de object storage.
- Ficheros `.well-known` y metadatos públicos del sitio.
- Configuración OIDC/OAuth/JWKS/SAML.
- Artefactos públicos de build frontend/backend.
- Artefactos Kubernetes, Docker, Helm y orquestación de contenedores.
- Logs de cloud, CDN, WAF, balanceadores y reverse proxies.
- Configuración de correo, DNS, SMTP, DKIM, DMARC y mensajería.
- Tooling de desarrollo, automatización, linters, test runners y scripts.
- Configuración AI/LLM, agents, prompts y vector databases.
- Dashboards y endpoints de monitoring/observability.
- Directory listing específico en directorios de backups, dumps y exports.
- Ficheros de configuración de servidor: `.htaccess`, `.htpasswd`, `web.config`, `nginx.conf`, `apache2.conf`, `httpd.conf` y vhosts.
- Apache `server-status` y `server-info`.
- Policies legacy `crossdomain.xml` y `clientaccesspolicy.xml`.
- Ficheros swap, temporales y copias de editor: `.swp`, `.swo`, `.bak`, `~`, `#file#`, `.tmp`, `.temp`, `.orig`.
- Framework/runtime expuesto: Spring Boot Actuator, Django Debug/Admin, Laravel Telescope, Rails info/mailers, PHP info/test, Express/Node debug routes y Werkzeug debugger.
- Observabilidad ampliada: Prometheus `/metrics`, Grafana expuesto/anónimo, Jaeger, Zipkin, Tempo, health/status con información excesiva y Sentry DSN en JS cliente.
- Claves, certificados y secretos: SSH/GPG/TLS private keys, `.pem`, `.key`, `.p12`, `.pfx`, `.jks`, `.csr`, `.npmrc`, `.pypirc`, `.gem/credentials`, Maven `settings.xml`, `gradle.properties`, `.env.*`, Rails `secrets.yml`, `credentials.yml.enc` y `master.key`.
- Serverless, edge y cloud moderno: `serverless.yml/json`, Netlify, Vercel, Render, Fly.io, Railway, Cloudflare Workers `wrangler.toml`, Firebase configs/rules y AWS SAM/CloudFormation templates.
- Build, CI/CD y tooling ampliado: Webpack Bundle Analyzer, `stats.json`, `.wasm.map`, cobertura `coverage/`, `lcov.info`, `coverage.xml`, reportes JUnit/Allure/Playwright/Cypress y reportes SAST/DAST de Sonar, Semgrep, Trivy, Snyk, ZAP, Burp, Gitleaks, Checkov/tfsec.
- Volcados y artefactos de datos: dumps SQL/DB, `.sqlite`, `.sqlite3`, `.db`, `.mdb`, `.accdb`, heap/thread dumps JVM, `.hprof`, memory/core dumps, `.dmp`, `core.*`, pickle/ML artifacts `.pkl`/`.pickle` y Jupyter/JupyterHub expuesto.
- Consolas expuestas de mensajería y streaming: RabbitMQ, Kafka UI/Kafdrop/AKHQ/Redpanda, ActiveMQ, Artemis, EMQX/MQTT, NATS, Pulsar, NSQ, Flower/Celery.
- Orquestadores y schedulers: Airflow, Argo Workflows/CD, Temporal, Prefect, Dagster, Luigi, Azkaban, Rundeck, Jenkins, Concourse, GoCD, Spinnaker, Tekton y Cronicle.
- Acceso remoto y terminales web: Apache Guacamole, noVNC, ttyd, WeTTY, GoTTY, WebSSH, Cockpit, Webmin, File Browser, code-server, Coder, Cloud9 y terminales Jupyter.
- Identidad y SSO: Keycloak, oauth2-proxy, Dex, ORY Hydra/Kratos, ZITADEL, authentik, Authelia, CAS, SAML, ADFS y metadatos OpenID Connect.
- Low-code, headless CMS e internal tools: Strapi, Directus, Ghost Admin, Keystone, Payload, Sanity Studio, Hasura, Appsmith, ToolJet, Budibase, NocoDB, Baserow, Retool, Supabase y PocketBase.
- Artefactos HAR de navegador/proxy con cookies, Authorization, tokens, payloads GraphQL/API y tráfico autenticado.
- WSDL/SOAP metadata expuesto para descubrir operaciones internas, namespaces, bindings y endpoints legacy.
- Jolokia/JMX expuesto, incluyendo variantes bajo `/jolokia` y `/actuator/jolokia`.
- Tokens y ficheros de service account de Kubernetes expuestos desde rutas de secretos montados.
- Migraciones y esquemas de base de datos: Prisma, Rails, Django, Alembic, Liquibase, Flyway, Knex, TypeORM y Sequelize.
- Consolas de administración Adobe ColdFusion, Lucee y Railo.
- SBOMs e inventarios de dependencias: CycloneDX, SPDX, Dependency-Check, licencias y paquetes exactos.
- Backstage catalog metadata y APIs de catálogo con owners, sistemas, servicios, repositorios y relaciones internas.
- Buzones de desarrollo expuestos: MailHog, Mailpit, MailCatcher, MailDev, Papercut y smtp4dev.
- PHP-FPM status/ping expuesto con métricas de pools, procesos y colas.
- Configuración de feature flags, experiments y remote config: LaunchDarkly, Unleash, Flagsmith, Split, GrowthBook, Optimizely, Statsig, PostHog y ConfigCat.
- Configuración de pagos y webhooks: Stripe, PayPal, Adyen, Braintree, Square, ngrok y secretos de firma.
- WebDAV real mediante `PROPFIND` con listados `207 Multi-Status`.
- Spring Cloud Config Server expuesto con `propertySources`, perfiles, labels y secretos de configuración.
- APIs Nacos expuestas para config/service discovery, namespaces, servicios y configs.
- Eureka service registry expuesto con servicios, instancias, IPs internas y health/status URLs.
- Apache Solr Admin API con cores, collections, schema, config, métricas y propiedades.
- Patrones cliente `postMessage` con wildcard `*` y señales de tokens/DOM sinks para revisión manual.
- Artefactos PrestaShop/Symfony de desarrollo: `Makefile`, `phpstan.neon`, Psalm, Rector, composer, paths `src`, `webservice`, `.github/workflows` y scripts de build.
- Configuración XML de módulos PrestaShop, incluyendo módulos custom de marketplace, payment, checkout y backoffice.
- Paths sensibles PrestaShop: `webservice`, `admin-dev`, `Backoffice`, `install`, logs, cache, configs y metadata de versión/módulos.

## Uso

```bash
nuclei -u https://target.tld -t exposed-templates/
nuclei -l targets.txt -t exposed-templates/ -severity low,medium,high,critical
```

Para empezar fino en Bug Bounty:

```bash
nuclei -l targets.txt -t exposed-templates/ -rl 50 -c 25
```
