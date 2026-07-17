# Organización

## 1. Equipo

Integrantes documentados en los README/docs de cada repositorio. Los equipos de
<span class="tc-badge tc-badge-cc">CC</span> e <span class="tc-badge tc-badge-ga">GA</span>
todavía tienen su tabla de equipo como plantilla sin llenar en sus propios repos
(`docs/equipo.md`) — se actualizará aquí en cuanto la completen.

| Nombre | Rol | Repositorio(s) a cargo |
|---|---|---|
| Tomas Quiceno Ostos | Backend developer | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service, am-communication-service |
| Sara Viviana Arteaga Rodríguez | Backend developer / Diseño UML | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service, am-communication-service |
| Julian Tinjaca | Backend developer | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service |
| Johan Beltrán | Backend developer | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service |
| Hernán Sánchez | Architect | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |
| Hever Barrera | Tech Lead | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |
| Nicolás Prieto | Backend developer | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |
| Mabel Bernal | QA | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |

!!! note "Pendiente"
    Los equipos de `cc-identity-service`, `cc-users-players-service`,
    `cc-teams-service`, `mk-tournament-service`, `ga-statistics-service` y
    `ga-api-gateway-service` no tienen todavía su tabla de integrantes
    completa en el repo correspondiente. Actualiza esta tabla apenas se
    llenen esos `docs/equipo.md` / `docs/team.md`.

## 2. Estructura de la organización GitHub

La organización [TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT) agrupa los repositorios del proyecto por dominio funcional, identificado mediante un prefijo en el nombre del repositorio:

| Prefijo | Dominio |
|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> `cc-` | Identidad y Usuarios |
| <span class="tc-badge tc-badge-mk">MK</span> `mk-` | Torneos y Pagos |
| <span class="tc-badge tc-badge-am">AM</span> `am-` | Partidos y Logística |
| <span class="tc-badge tc-badge-ga">GA</span> `ga-` | Analítica y Auditoría |

## 3. Metodología de trabajo

- **Metodología:**
- **Duración de sprint / iteración:**
- **Ceremonias:**

## 4. Flujo de trabajo con Git

Patrón consistente documentado en los `docs/equipo.md` / `docs/team.md` de los
distintos servicios (con variaciones menores por equipo):

- **Estrategia de branching:** ramas de feature (`feat/**` o `feature/<descripción>`)
  contra `develop`; `develop` se integra a `main` por Pull Request. `mk-tournament-service`
  usa `develop` como rama principal de trabajo.
- **Convención de commits:** descriptivos, en español o inglés (consistente dentro de
  cada PR).
- **Proceso de Pull Request / revisión de código:** cada cambio pasa por el pipeline de
  CI (compilación, pruebas, cobertura y análisis estático con SonarQube/SonarCloud)
  antes de poder mezclarse. Varios repos exigen que `mvn test`/`./mvnw test` pase en
  local antes de abrir el PR.

## 5. Canales de comunicación

| Canal | Uso |
|---|---|
| | |

## 6. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
