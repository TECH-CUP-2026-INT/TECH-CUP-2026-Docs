# Entregables y pruebas

## 1. Entregables

Cada repositorio de la organización [TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT)
es un entregable independiente, con su propio pipeline de CI/CD y (salvo el
frontend y la infraestructura) su propio sitio MkDocs. Ver [Inicio](index.md)
para el listado completo con enlaces a cada repositorio y su documentación.

| Dominio | Entregables |
|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> Identidad y Usuarios | cc-identity-service, cc-users-players-service, cc-teams-service |
| <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos | mk-tournament-service, mk-payment-service |
| <span class="tc-badge tc-badge-am">AM</span> Partidos y Logística | am-matches-service, am-logistic-service, am-notification-service, am-communication-service |
| <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría | ga-statistics-service, ga-api-gateway-service |
| Frontend | TECH-CUP-FRONT |
| Infraestructura | TECH-CUP-Observability |
| Documentación | TECH-CUP-2026-Docs (este repositorio) |

## 2. Análisis de SonarQube

La mayoría de los microservicios analizan cada push con **SonarCloud**, bajo
la organización `tech-cup-2026-int`. Snapshot capturado el 2026-07-17 (los
valores cambian con cada análisis — consulta
[sonarcloud.io/organizations/tech-cup-2026-int](https://sonarcloud.io/organizations/tech-cup-2026-int/projects)
para el estado actual):

| Servicio | Quality Gate | Cobertura | Bugs | Vulnerabilidades | Code Smells | Duplicación |
|---|---|---|---|---|---|---|
| cc-users-players-service | ✅ OK | 88.3% | 1 | 0 | 17 | 2.7% |
| mk-tournament-service | ✅ OK | 91.0% | 0 | 0 | 1 | 0.5% |
| mk-payment-service | ✅ OK | 95.1% | 0 | 0 | 7 | 0.0% |
| am-matches-service | ✅ OK | 98.1% | 0 | 0 | 21 | 0.0% |
| am-logistic-service | ✅ OK | 90.9% | 0 | 0 | 5 | 0.0% |
| am-notification-service | ✅ OK | 93.3% | 0 | 0 | 10 | 0.0% |
| am-communication-service | ✅ OK | 92.1% | 0 | 0 | 4 | 0.0% |

`ga-statistics-service` corre su análisis con una instancia local de
SonarQube en vez de SonarCloud (97% de cobertura, ver su `docs/pruebas.md`).
`ga-api-gateway-service` usa **Qodana** (JetBrains) como análisis estático en
vez de SonarQube. `cc-identity-service` y `cc-teams-service` todavía no tienen
un paso de análisis estático en su pipeline de CI.

## 3. Cobertura, Swagger y observabilidad por servicio

| Servicio | Cobertura de pruebas | Swagger / OpenAPI | Observabilidad |
|---|---|---|---|
| cc-identity-service | Sin reporte en SonarCloud | ✅ Listo (springdoc 2.8.4) | — |
| cc-users-players-service | 88.3% (SonarCloud) | ✅ Listo (springdoc 2.8.5) | — |
| cc-teams-service | Gate JaCoCo en CI, sin reporte en SonarCloud | ✅ Listo (springdoc 2.8.4) | — |
| mk-tournament-service | 91.0% (SonarCloud) | ✅ Listo | — |
| mk-payment-service | 95.1% (SonarCloud) | ✅ Listo | Métricas — Actuator + Micrometer/Prometheus |
| am-matches-service | 98.1% (SonarCloud) | ✅ Listo | Métricas + Trazas — Actuator, Prometheus, Zipkin (OpenTelemetry) |
| am-logistic-service | 90.9% (SonarCloud) | ✅ Listo | Métricas + Trazas — Actuator, Prometheus, Zipkin (Brave) |
| am-notification-service | 93.3% (SonarCloud) | ✅ Listo | Métricas + Trazas — Actuator, Prometheus, Zipkin (Brave) |
| am-communication-service | 92.1% (SonarCloud) | ✅ Listo | Métricas + Trazas — Actuator, Prometheus, Zipkin (Brave) |
| ga-statistics-service | 97% (JUnit 5 + Mockito + AssertJ) | ✅ Listo | Métricas — Actuator + Micrometer/Prometheus |
| ga-api-gateway-service | Gate JaCoCo ≥ 30% | ✅ Listo (WebFlux UI) | Métricas — Actuator |

Los cuatro servicios del dominio AM (`am-matches`, `am-logistic`,
`am-notification`, `am-communication`) tienen la observabilidad más completa
de la plataforma: métricas Prometheus **y** trazas distribuidas Zipkin. Los
servicios de pagos y estadísticas exponen métricas vía Actuator/Prometheus.
Observabilidad centralizada de toda la plataforma (dashboards, agregación de
logs) vive en `TECH-CUP-Observability` — ver [Back](back.md).

## 4. Integración continua

Cada microservicio automatiza en su pipeline de GitHub Actions: compilación,
ejecución de pruebas, empaquetado y publicación de la imagen Docker en GitHub
Container Registry. Varios servicios agregan además un gate de cobertura
(JaCoCo) y análisis estático (SonarQube/SonarCloud/Qodana) antes de permitir
el merge.

## 5. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Entregables completados a partir de la documentación de cada repositorio |
| 0.3 | 2026-07-17 | | Se agrega análisis de SonarQube y tabla de cobertura/Swagger/observabilidad por servicio |