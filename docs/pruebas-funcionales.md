# Pruebas Funcionales

## 1. Introducción

Cada microservicio mantiene su propia suite de pruebas y su propio
`docs/pruebas.md` (o `testing.md`), verificada en CI en cada push. Este
documento consolida la estrategia común y remite al detalle de casos de
prueba de cada repositorio en vez de duplicarlo.

## 2. Estrategia de pruebas

- **Tipos de prueba:**
    - Unitarias (JUnit 5 + Mockito, + AssertJ en algunos servicios) sobre la
      lógica de negocio: reglas de sanción, cronómetro de partido, cálculo de
      estadísticas, validaciones de duplicados, etc.
    - Integración con **Testcontainers** (MongoDB/PostgreSQL reales en
      contenedor) para los servicios con persistencia.
    - Análisis estático con **SonarQube / SonarCloud** como gate adicional
      del pipeline.
    - Verificación manual end-to-end documentada puntualmente por servicio
      (p. ej. `am-logistic-service` registra en su README una corrida
      completa con `docker compose` y mocks HTTP de los servicios externos).
- **Herramientas:** Maven (`mvnw test`), JaCoCo (cobertura), GitHub Actions
  (CI), SonarQube/SonarCloud (calidad estática).
- **Ambientes de prueba:** local (Docker Compose por servicio) y CI (GitHub
  Actions, on push a `main`/`develop`/`feature/**` y en cada Pull Request).
- **Criterios de entrada / salida:** el pipeline de CI debe pasar (build +
  pruebas + gate de cobertura, donde aplique) antes de mezclar un Pull
  Request — ver [Organización](organizacion.md#5-flujo-de-trabajo-con-git).

### Cobertura por servicio (donde está documentada)

| Servicio | Cobertura mínima exigida | Cobertura alcanzada |
|---|---|---|
| am-notification-service | 80% (JaCoCo) | — |
| ga-statistics-service | 80% | 97% |

## 3. Matriz de trazabilidad

| Dominio | Requerimientos oficiales | Repositorios | Documentación de pruebas |
|---|---|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> Identidad y Usuarios | TC-01 – TC-28 | cc-identity-service, cc-users-players-service, cc-teams-service | `docs/pruebas.md` de cada repo |
| <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos | TC-29 – TC-56 | mk-tournament-service, mk-payment-service | `docs/testing.md` de cada repo |
| <span class="tc-badge tc-badge-am">AM</span> Partidos, Logística y Comunicación | TC-57 – TC-88 | am-matches-service, am-logistic-service, am-notification-service, am-communication-service | `./mvnw test` en cada repo — sanciones, marcador en vivo, cronómetro, listeners de evento |
| <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría | TC-89 – TC-108 | ga-statistics-service | JUnit 5 + Mockito + AssertJ, cobertura 97% |

Ver [Análisis de Requerimientos](analisis-requerimientos.md) para el detalle
de cada uno de los 108 requerimientos.

!!! note "Casos de prueba detallados"
    El detalle caso a caso (precondiciones, pasos, resultado esperado) vive
    en el `docs/pruebas.md` de cada microservicio — no se duplica aquí para
    evitar que quede desactualizado respecto a la fuente real.

## 4. Casos de prueba

Los casos de prueba unitarios y de integración de cada servicio viven en su
propio repositorio (ver matriz de trazabilidad). Los flujos end-to-end que
cruzan varios servicios (por ejemplo, inscripción de equipo → pago PSE →
confirmación) se coordinan entre los equipos dueños de cada servicio
involucrado.

## 5. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Estrategia y matriz de trazabilidad completadas a partir de la documentación de cada microservicio |