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

## 2. Integración continua

Cada microservicio automatiza en su pipeline de GitHub Actions: compilación,
ejecución de pruebas, empaquetado y publicación de la imagen Docker en GitHub
Container Registry. Varios servicios agregan además un gate de cobertura
(JaCoCo) y análisis estático (SonarQube/SonarCloud) antes de permitir el
merge — ver [Pruebas Funcionales](pruebas-funcionales.md).

## 3. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Entregables completados a partir de la documentación de cada repositorio |