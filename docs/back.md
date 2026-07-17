# Back

Visión general de los microservicios backend de TECH CUP 2026. Todos son
Spring Boot / Java 21 salvo donde se indique, se documentan con MkDocs en su
propio repositorio (ver columna **Docs** en el [Inicio](index.md)), y se
integran entre sí vía REST (algunos también vía eventos/colas) detrás de un
API Gateway común.

## <span class="tc-badge tc-badge-cc">CC</span> Identidad y Usuarios

### cc-identity-service

Autenticación y autorización de la plataforma: `POST /auth/login`,
`POST /auth/register`, `POST /auth/refresh` bajo `/api/v1/identity`. Es el
emisor del JWT que valida el API Gateway y en el que confía el resto de
servicios. Java (con scripts Python auxiliares), Dockerizado.

### cc-users-players-service

Fuente de verdad del perfil de usuario: registro de estudiantes, invitados y
egresados, creación de árbitros/administradores, perfil general y perfil
deportivo (posición, dorsal), y promoción de jugador a capitán.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB · OpenFeign (Identity, Teams) · Docker.
- **Cobertura funcional:** requerimientos TC-01 a TC-19, con historial de
  torneos por jugador y sincronización de rol de capitán en el roadmap de
  integración con Torneos e Identity.

### cc-teams-service

Gestión de equipos: creación, invitaciones, capitanía e inscripción a
torneos. Actor principal: el **Capitán**.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB · OpenFeign (Identity, Torneos).
- **Cobertura funcional:** requerimientos TC-20 a TC-28 — creación y consulta
  de equipos, inscripción a torneo (mínimo 7 integrantes), invitaciones,
  aceptación/rechazo y transferencia de capitanía. Edición y eliminación de
  equipo, y el detalle enriquecido de equipo con perfiles de miembros, están
  en el roadmap del servicio.

## <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos

### mk-tournament-service

Ciclo de vida completo del torneo: creación, reglamento, canchas, generación
de emparejamientos (brackets / grupos / liga), calendario, mapa de canchas,
tabla de posiciones y llaves en tiempo real.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · arquitectura hexagonal.
- **Cobertura funcional:** TC-29 a TC-54 (excluyendo TC-52, que pertenece al
  Servicio de Inscripción). El control de acceso por rol (Organizador,
  Capitán, Jugador) vía JWT está definido a nivel de diseño y forma parte del
  roadmap de seguridad del servicio.

### mk-payment-service

Procesa los pagos de inscripción al torneo exclusivamente vía **PSE** (Mercado
Pago): crear orden de pago, consultar límites de monto habilitados, iniciar
transacción PSE, recibir webhook de resultado (aprobada/rechazada/cancelada),
consultar estado de una orden, sincronización diaria de límites, y expiración
automática de órdenes pendientes (60 min, revisado cada 5 min).

- **Stack:** Java 21 · Spring Boot · Maven · PostgreSQL 16 · Mercado Pago (sandbox PSE).
- **Cobertura funcional:** RF-01 a RF-07+.

## <span class="tc-badge tc-badge-am">AM</span> Partidos y Logística

Los servicios de este dominio (equipo **astromerge**) comparten el mismo
modelo de seguridad: el API Gateway valida la firma del JWT y la propaga a
cada servicio, que confía en esos claims sin volver a verificarlos —
diseñados para vivir siempre detrás del Gateway.

### am-matches-service

Arbitraje en tiempo real: iniciar/pausar/reanudar/finalizar partido, tiempo
añadido, goles, tarjetas (con regla de sanción por acumulación), sustituciones,
observaciones y subida de la planilla del partido.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB.
- **Cobertura funcional:** RF-01 a RF-12. Endpoints bajo `/api/partidos`, rol
  requerido: árbitro.
- **Seguridad:** incluyó una revisión dedicada que reforzó la validación de
  archivos y rutas en la subida de la planilla del partido.

### am-logistic-service

Operación no deportiva del torneo: refrigerios y dotación (petos, balones,
kits) para equipos, jugadores y árbitros. Actor principal: el **Organizador**.

- **Stack:** Java 21 · Spring Boot · Maven · MongoDB · Testcontainers · JaCoCo.
- **Cobertura funcional:** RF-01 a RF-05, incluyendo devolución de dotación
  con estado `DEVUELTO`. Las integraciones con Torneos, Equipos y Auditoría
  están implementadas detrás de interfaces (puertos), listas para confirmar
  el contrato final con cada equipo dueño.

### am-notification-service

Consumidor de eventos de otros microservicios (sanciones, mensajes,
solicitudes/invitaciones de equipo, cambios de inscripción, agendamiento de
partidos) vía **9 webhooks REST** más consumo directo de dos tipos de evento
del exchange RabbitMQ compartido (`techcup.exchange`). Produce historial de
notificaciones in-app y correo best-effort.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · Spring AMQP · Spring Mail ·
  Micrometer + Prometheus + Zipkin.
- **Cobertura funcional:** RF-01 confirmado end-to-end con
  `am-matches-service`; RF-02 a RF-06 con endpoint disponible y contrato en
  proceso de confirmación conjunta con Equipos, Comunicaciones e Inscripción.

### am-communication-service

Mensajería de la plataforma: chat de soporte asistido por chatbot, escalamiento
a un organizador humano, mensajería directa entre jugadores, chat grupal por
equipo y moderación de mensajes reportados.

- **Stack:** Java 21 · Spring Boot 3.x · WebSocket/STOMP (tiempo real) ·
  PostgreSQL + JPA · OpenFeign · JWT (jjwt) · MapStruct.
- **Cobertura funcional:** RF-01 a RF-05 definidos, con la estructura base del
  servicio (controller, service, repository, entity, dto, mapper) en su lugar
  y el desarrollo de la lógica de negocio en curso.

## <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría

### ga-statistics-service

Cálculo y consulta de estadísticas de jugadores, equipos, partidos y torneo
(promedios, totales, rankings, tabla de posiciones, reconocimientos como
goleador y mejor defensa) a partir de los eventos de partido. Los agregados se
calculan con streams de Java sobre los documentos necesarios, no con
`AVG`/`GROUP BY` en la base de datos.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · Lombok.
- **Cobertura funcional:** RF1 a RF15.
- **Pruebas:** JUnit 5 + Mockito + AssertJ, cobertura 97% (mínimo exigido: 80%).

### ga-api-gateway-service

Puerta de enlace única de la plataforma (Spring Cloud Gateway + WebFlux):
enruta las peticiones del frontend a cada microservicio, aplica el filtro de
autenticación JWT (`AuthenticationFilter`) y centraliza la documentación
OpenAPI/Swagger de todos los servicios. Repositorio **privado**.

El dominio GA también contempla un Servicio de Auditoría (`ga-audit-service`)
como destino de los reportes best-effort de Partidos y Logística — está en el
roadmap del dominio.

## Infraestructura transversal

**TECH-CUP-Observability** (repositorio privado, solo infraestructura, sin
lógica de negocio) centraliza métricas, trazas distribuidas y logs de todos
los microservicios sobre Docker Compose: Prometheus + Grafana (métricas vía
`/actuator/prometheus`), Zipkin (trazas vía Micrometer Tracing) y
Elasticsearch + Logstash + Kibana (logs estructurados vía Logback).