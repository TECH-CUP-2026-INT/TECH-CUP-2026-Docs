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
      contenedor) para los servicios con persistencia — requieren Docker
      disponible en el entorno de CI.
    - Análisis estático con **SonarQube / SonarCloud** como gate adicional
      del pipeline.
    - Verificación manual end-to-end documentada puntualmente (p. ej.
      `am-logistic-service` registra en su README una corrida completa con
      `docker compose` y mocks HTTP de los servicios externos).
- **Herramientas:** Maven (`mvnw test`), JaCoCo (cobertura), GitHub Actions
  (CI), SonarQube/SonarCloud (calidad estática).
- **Ambientes de prueba:** local (Docker Compose por servicio) y CI (GitHub
  Actions, on push a `main`/`develop`/`feature/**` y en cada Pull Request).
- **Criterios de entrada / salida:** el pipeline de CI debe pasar (build +
  pruebas + gate de cobertura, donde aplique) antes de mezclar un Pull
  Request — ver [Organización](organizacion.md#4-flujo-de-trabajo-con-git).

### Cobertura mínima por servicio (donde está documentada)

| Servicio | Cobertura mínima (CI) | Cobertura actual |
|---|---|---|
| am-notification-service | 80% (JaCoCo) | — |
| ga-statistics-service | 80% (documentado como requerimiento no funcional) | 97% |

## 3. Matriz de trazabilidad

| Dominio | Rango de requerimientos | Repositorio | Documentación de pruebas |
|---|---|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> | TC-01 – TC-19 | cc-users-players-service | `docs/pruebas.md` del repo |
| <span class="tc-badge tc-badge-cc">CC</span> | TC-20 – TC-28 | cc-teams-service | `docs/pruebas.md` del repo |
| <span class="tc-badge tc-badge-mk">MK</span> | TC-29 – TC-54 | mk-tournament-service | `docs/testing.md` del repo |
| <span class="tc-badge tc-badge-mk">MK</span> | RF-01 – RF-07+ | mk-payment-service | `docs/testing.md` del repo |
| <span class="tc-badge tc-badge-am">AM</span> | RF-01 – RF-12 | am-matches-service | `./mvnw test` — cubre sanciones, marcador en vivo, cronómetro, subida de planilla |
| <span class="tc-badge tc-badge-am">AM</span> | RF-01 – RF-05 | am-logistic-service | `./mvnw test` + verificación end-to-end manual documentada en el README |
| <span class="tc-badge tc-badge-am">AM</span> | RF-01 – RF-07 | am-notification-service | `./mvnw test` — un test por listener de evento + reglas de `NotificationService` |
| <span class="tc-badge tc-badge-ga">GA</span> | RF1 – RF15 | ga-statistics-service | JUnit 5 + Mockito + AssertJ, cobertura 97% |

!!! note "Casos de prueba detallados"
    El detalle caso a caso (precondiciones, pasos, resultado esperado) vive
    en el `docs/pruebas.md` de cada microservicio — no se duplica aquí para
    evitar que quede desactualizado respecto a la fuente real.

## 4. Casos de prueba

_Pendiente: enlazar aquí los casos de prueba end-to-end que crucen más de un
servicio (p. ej. inscripción de equipo → pago PSE → confirmación) una vez se
definan a nivel de plataforma. Los casos de prueba unitarios/de integración
por servicio ya existen en cada repositorio (ver matriz de trazabilidad)._

## 5. Registro de defectos

Huecos y defectos conocidos, documentados en el propio repositorio de cada
servicio (no son suposiciones):

| ID | Descripción | Severidad | Repositorio | Estado |
|---|---|---|---|---|
| BUG-01 | El pipeline de CI tiene un job `dockerize-publish` pero el repositorio no tiene `Dockerfile` en la raíz — falla en el próximo push a `main`. | Alta | cc-users-players-service | Abierto |
| BUG-02 | Promover un jugador a capitán (TC-18) actualiza el rol localmente pero la llamada de sincronización a Identity Service no existe todavía. | Alta | cc-users-players-service | Abierto |
| BUG-03 | Editar equipo (TC-23) y eliminar equipo (TC-28) no tienen endpoint implementado. | Media | cc-teams-service | Abierto |
| BUG-04 | El control de acceso por rol vía JWT está documentado pero no implementado — los endpoints están abiertos. | Alta | mk-tournament-service | Abierto |
| BUG-05 | Los contratos de 5 de los 7 webhooks de eventos están "propuestos", pendientes de confirmación con los equipos dueños (Equipos, Comunicaciones, Inscripción). | Media | am-notification-service | Abierto |
| BUG-06 | El servicio de auditoría (`ga-audit-service`) referenciado por Partidos y Logística no existe en la organización. | Alta | (transversal) | Abierto |

## 6. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Estrategia y matriz de trazabilidad completadas a partir de la documentación real de cada microservicio |