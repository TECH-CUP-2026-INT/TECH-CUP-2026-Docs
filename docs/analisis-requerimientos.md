# Análisis de Requerimientos

## 1. Introducción

TECH CUP 2026 digitaliza el torneo semestral de fútbol de la Escuela
Colombiana de Ingeniería Julio Garavito (DOSW). Cada microservicio de la
plataforma mantiene su propia hoja de requerimientos funcionales, contrastada
contra su implementación (ver `docs/requerimientos.md` de cada repositorio,
enlazados en [Inicio](index.md)). Este documento consolida esa información a
nivel de plataforma.

## 2. Propósito y alcance

- **Propósito:** dar una vista única de qué hace la plataforma, para quién, y
  qué microservicio resuelve cada capacidad — sin duplicar el detalle técnico
  que ya vive en cada repositorio.
- **Alcance:** las funcionalidades expuestas por los microservicios de los
  dominios CC, MK, AM y GA, y por el frontend (`TECH-CUP-FRONT`).
- **Fuera de alcance:** el detalle línea a línea de cada requerimiento (vive
  en el `docs/requerimientos.md` de cada servicio) y la infraestructura de
  observabilidad (`TECH-CUP-Observability`), que no expone funcionalidad de
  negocio.

## 3. Actores del sistema

| Actor | Descripción |
|---|---|
| Jugador | Estudiante, invitado o egresado inscrito en un equipo. Consulta su perfil, su equipo, el calendario y sus estadísticas. |
| Capitán | Jugador con permisos adicionales sobre su equipo: invitar miembros, inscribir el equipo a un torneo, pagar la inscripción. |
| Árbitro | Gestiona el desarrollo en vivo de los partidos que tiene asignados. |
| Organizador | Crea y administra torneos, refrigerios y dotación; aprueba inscripciones. |
| Administrador | Crea árbitros y administradores; supervisión general de la plataforma. |
| Público / Aficionado | Consulta calendario, llaves, historial y rankings sin necesidad de autenticarse. |
| Mercado Pago | Sistema externo que procesa transacciones PSE y notifica su resultado vía webhook. |

## 4. Requerimientos funcionales

Cada fila resume un bloque de requerimientos ya documentado y con IDs propios
en el repositorio del servicio dueño — el detalle completo (uno por uno) vive
ahí, no se duplica aquí.

| Rango de IDs | Servicio | Resumen | Repositorio |
|---|---|---|---|
| TC-01 – TC-19 | Usuarios y Jugadores | Registro (estudiante/invitado/egresado/árbitro/admin), perfil general y deportivo, promoción a capitán | [cc-users-players-service](https://github.com/TECH-CUP-2026-INT/cc-users-players-service) |
| — | Identidad | Login, registro de credenciales, refresh token | [cc-identity-service](https://github.com/TECH-CUP-2026-INT/cc-identity-service) |
| TC-20 – TC-28 | Equipos | Crear/consultar/inscribir equipo, invitaciones, transferencia de capitanía | [cc-teams-service](https://github.com/TECH-CUP-2026-INT/cc-teams-service) |
| TC-29 – TC-54 (excepto TC-52) | Torneos | Creación y ciclo de vida del torneo, reglamento, canchas, emparejamientos, calendario, llaves, posiciones | [mk-tournament-service](https://github.com/TECH-CUP-2026-INT/mk-tournament-service) |
| RF-01 – RF-07+ | Pagos | Orden de pago PSE, límites de monto, webhook de resultado, expiración automática | [mk-payment-service](https://github.com/TECH-CUP-2026-INT/mk-payment-service) |
| RF-01 – RF-12 | Partidos | Arbitraje en vivo: cronómetro, goles, tarjetas, sustituciones, planilla | [am-matches-service](https://github.com/TECH-CUP-2026-INT/am-matches-service) |
| RF-01 – RF-05 | Logística | Refrigerios y dotación (petos, balones, kits) | [am-logistic-service](https://github.com/TECH-CUP-2026-INT/am-logistic-service) |
| RF-01 – RF-07 | Notificaciones | Alertas in-app y correo best-effort a partir de eventos de otros servicios | [am-notification-service](https://github.com/TECH-CUP-2026-INT/am-notification-service) |
| RF-01 – RF-05 | Comunicaciones | Chat de soporte, mensajería directa, chat de equipo, moderación | [am-communication-service](https://github.com/TECH-CUP-2026-INT/am-communication-service) |
| RF1 – RF15 | Estadísticas | Estadísticas y rankings de jugador/equipo/torneo, reconocimientos | [ga-statistics-service](https://github.com/TECH-CUP-2026-INT/ga-statistics-service) |

Ver [Back](back.md) para el detalle de cada servicio, y [APP](app.md) para la
misma información organizada por actor en vez de por servicio.

## 5. Requerimientos no funcionales

| ID | Categoría | Descripción |
|---|---|---|
| RNF-01 | Seguridad | El JWT se emite en `cc-identity-service` y se valida una única vez en el API Gateway (`ga-api-gateway-service`); los microservicios internos confían en los claims ya validados y no exponen sus puertos directo a internet. |
| RNF-02 | Rendimiento | `ga-statistics-service` documenta un máximo de 2 segundos de respuesta para sus consultas. |
| RNF-03 | Calidad de código | Varios servicios exigen un mínimo de cobertura de pruebas del 80% (JaCoCo) como gate de CI, además de análisis estático con SonarQube/SonarCloud. |
| RNF-04 | Observabilidad | Métricas (Prometheus/Grafana), trazas distribuidas (Zipkin) y logs centralizados (ELK) vía `TECH-CUP-Observability`, para los servicios que exponen `/actuator/prometheus` y Micrometer Tracing. |
| RNF-05 | Resiliencia | Las integraciones no críticas entre servicios (auditoría, estadísticas, notificaciones) son *best-effort*: un fallo se registra pero no bloquea ni revierte la operación que lo originó. |
| RNF-06 | Accesibilidad | El color nunca es el único canal de información — p. ej. cada evento de partido expone un `eventType` explícito además del color, para no depender de la percepción de color. |
| RNF-07 | Escalabilidad | Los servicios están diseñados sin estado de sesión en servidor (`STATELESS`), para poder escalar horizontalmente. |

## 6. Reglas de negocio

| ID | Descripción |
|---|---|
| RN-01 | Un jugador acumula 2 tarjetas amarillas en el mismo partido, o una roja directa, para ser sancionado (umbral configurable en `am-matches-service`). |
| RN-02 | Una orden de pago de inscripción vence a los 60 minutos si queda pendiente o esperando confirmación bancaria (`mk-payment-service`). |
| RN-03 | Un equipo necesita un mínimo de 7 integrantes para poder inscribirse a un torneo (`cc-teams-service`). |
| RN-04 | Un torneo requiere mínimo 3 equipos para poder crearse (`mk-tournament-service`). |

## 7. Glosario

| Término | Definición |
|---|---|
| RF / TC | Requerimiento funcional — cada servicio usa su propia convención de numeración (`RF-XX` o `TC-XX`), heredada de su hoja de requerimientos original. |
| RNF | Requerimiento no funcional. |
| Best-effort | Llamada o integración cuyo fallo se registra pero nunca bloquea ni revierte la operación principal. |
| PSE | Pagos Seguros en Línea — único medio de pago habilitado para inscripciones, procesado vía Mercado Pago. |
| Capitán | Jugador con permisos de gestión sobre su equipo. |

## 8. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Consolidación de requerimientos a partir de la documentación real de cada microservicio |