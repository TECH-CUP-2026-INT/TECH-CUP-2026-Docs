# Organización

## 1. Equipo

Integrantes por dominio, según lo documentado en cada repositorio:

| Nombre | Rol | Repositorio(s) a cargo |
|---|---|---|
| Tomas Quiceno Ostos | Backend developer | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service, am-communication-service |
| Sara Viviana Arteaga Rodríguez | Backend developer / Diseño UML | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service, am-communication-service |
| Julian Tinjaca | Backend developer | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service |
| Johan Beltrán | Backend developer | <span class="tc-badge tc-badge-am">AM</span> am-matches-service, am-logistic-service, am-notification-service |
| Hernán Sánchez | Architect | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |
| Hever Barrera | Tech Lead | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service, mk-tournament-service |
| Nicolás Prieto | Backend developer | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |
| Mabel Bernal | QA | <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service |
| Juan Eduardo Vera | Domain lead | <span class="tc-badge tc-badge-cc">CC</span> cc-identity-service, cc-users-players-service, cc-teams-service |
| Jhonatan Peña | Domain lead | <span class="tc-badge tc-badge-ga">GA</span> ga-statistics-service, ga-api-gateway-service |

## 2. Miembros de la organización GitHub

Todas las cuentas con acceso a [TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT):

| Usuario de GitHub | Nombre |
|---|---|
| [TomasQ456](https://github.com/TomasQ456) | Tomas Quiceno Ostos |
| [saraarteagar91](https://github.com/saraarteagar91) | Sara Viviana Arteaga Rodríguez |
| [Julianizz18](https://github.com/Julianizz18) | Julian Tinjaca |
| [jsebastianBeltran2002](https://github.com/jsebastianBeltran2002) | Johan Beltrán |
| [Helacio](https://github.com/Helacio) | Hernán Sánchez |
| [heverthisday](https://github.com/heverthisday) | Hever Barrera |
| [NicolasPrieto12](https://github.com/NicolasPrieto12) | Nicolás Prieto |
| [MabelBernalAmaya](https://github.com/MabelBernalAmaya) | Mabel Bernal |
| [JUNE2908](https://github.com/JUNE2908) | Juan Eduardo Vera |
| [jhonatanpenamora-png](https://github.com/jhonatanpenamora-png) | Jhonatan Peña |
| [jhonatanmadero](https://github.com/jhonatanmadero) | Jhonatan Madero |
| [Gilian2612](https://github.com/Gilian2612) | William Santiago Ruiz |
| [Joslag](https://github.com/Joslag) | José García |
| [juanbueno5555-rgb](https://github.com/juanbueno5555-rgb) | Juan David Rangel Jiménez |
| [Caliche96](https://github.com/Caliche96) | Kaliche |

## 3. Estructura de la organización GitHub

La organización [TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT) agrupa los repositorios del proyecto por dominio funcional, identificado mediante un prefijo en el nombre del repositorio:

| Prefijo | Dominio |
|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> `cc-` | Identidad y Usuarios |
| <span class="tc-badge tc-badge-mk">MK</span> `mk-` | Torneos y Pagos |
| <span class="tc-badge tc-badge-am">AM</span> `am-` | Partidos y Logística |
| <span class="tc-badge tc-badge-ga">GA</span> `ga-` | Analítica y Auditoría |

## 4. Metodología de trabajo

- **Metodología:** ágil, coordinada íntegramente sobre GitHub (Issues para
  tareas, Pull Requests para revisión de código, Actions para integración
  continua).
- **Duración de sprint / iteración:** por dominio, alineada al cronograma
  académico del semestre.
- **Ceremonias:** revisión de código en cada Pull Request y seguimiento de
  avance por dominio.

## 5. Flujo de trabajo con Git

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

## 6. Canales de comunicación

| Canal | Uso |
|---|---|
| GitHub Issues | Seguimiento de tareas y hallazgos |
| Pull Requests | Revisión de código |
| GitHub Actions | Integración continua y despliegue |

## 7. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Equipo y miembros de la organización completados |