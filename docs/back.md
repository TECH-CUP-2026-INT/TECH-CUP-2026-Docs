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

- **Cobertura funcional:** TC-06 a TC-12 del catálogo oficial (login
  institucional y con Gmail, validación de JWT, recuperación de contraseña,
  OTP, expiración de sesión y consulta de eventos del servicio).

### cc-users-players-service

Fuente de verdad del perfil de usuario: registro de estudiantes, invitados y
egresados, creación de árbitros/administradores, perfil general y perfil
deportivo (posición, dorsal), y promoción de jugador a capitán.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB · OpenFeign (Identity, Teams) · Docker.
- **Cobertura funcional:** TC-01 a TC-05 (registro de estudiantes, invitados,
  egresados, árbitros y administradores) y TC-13 a TC-19 (perfil general y
  deportivo, ascenso a capitán, deshabilitar usuario) del catálogo oficial.

### cc-teams-service

Gestión de equipos: creación, invitaciones, capitanía e inscripción a
torneos. Actor principal: el **Capitán**.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB · OpenFeign (Identity, Torneos).
- **Cobertura funcional:** TC-20 a TC-28 del catálogo oficial — creación,
  consulta de alineación y plantilla, actualización y eliminación de equipo,
  invitaciones y transferencia de capitanía.

## <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos

### mk-tournament-service

Ciclo de vida completo del torneo: creación, reglamento, canchas, generación
de emparejamientos (brackets / grupos / liga), calendario, mapa de canchas,
tabla de posiciones y llaves en tiempo real.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · arquitectura hexagonal.
- **Cobertura funcional:** TC-29 a TC-51 y TC-54 (creación, reglamento,
  canchas, emparejamientos, calendario, ciclo de vida y consulta de eventos
  del torneo); la inscripción de equipos (TC-52, TC-55, TC-56) se resuelve en
  conjunto con `mk-payment-service`. El acceso está controlado por rol
  (Organizador, Capitán, Jugador) mediante el JWT emitido por Identidad.

### mk-payment-service

Procesa los pagos de inscripción al torneo exclusivamente vía **PSE** (Mercado
Pago): crear orden de pago, consultar límites de monto habilitados, iniciar
transacción PSE, recibir webhook de resultado (aprobada/rechazada/cancelada),
consultar estado de una orden, sincronización diaria de límites, y expiración
automática de órdenes pendientes (60 min, revisado cada 5 min).

- **Stack:** Java 21 · Spring Boot · Maven · PostgreSQL 16 · Mercado Pago (sandbox PSE).
- **Cobertura funcional:** implementa la inscripción y validación de pago del
  torneo (TC-52, TC-53, TC-55, TC-56 del catálogo oficial) con su propia
  numeración interna RF-01 a RF-07+, ya integrada con Mercado Pago vía PSE.

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
- **Cobertura funcional:** TC-57 a TC-69 del catálogo oficial. Endpoints bajo
  `/api/partidos`, rol requerido: árbitro.
- **Seguridad:** incluyó una revisión dedicada que reforzó la validación de
  archivos y rutas en la subida de la planilla del partido.

### am-logistic-service

Operación no deportiva del torneo: refrigerios y dotación (petos, balones,
kits) para equipos, jugadores y árbitros. Actor principal: el **Organizador**.

- **Stack:** Java 21 · Spring Boot · Maven · MongoDB · Testcontainers · JaCoCo.
- **Cobertura funcional:** TC-70 a TC-74 del catálogo oficial, incluyendo
  devolución de dotación con estado `DEVUELTO`. Las integraciones con Torneos
  y Equipos están implementadas detrás de interfaces (puertos).

### am-notification-service

Consumidor de eventos de otros microservicios (sanciones, mensajes,
solicitudes/invitaciones de equipo, cambios de inscripción, agendamiento de
partidos) vía **9 webhooks REST** más consumo directo de dos tipos de evento
del exchange RabbitMQ compartido (`techcup.exchange`). Produce historial de
notificaciones in-app y correo best-effort.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · Spring AMQP · Spring Mail ·
  Micrometer + Prometheus + Zipkin.
- **Cobertura funcional:** TC-79 a TC-88 del catálogo oficial. TC-79 (sanción
  por tarjetas) confirmado end-to-end con `am-matches-service`; el resto de
  eventos se integra conjuntamente con Equipos, Comunicaciones e Inscripción.

### am-communication-service

Mensajería de la plataforma: chat de soporte asistido por chatbot, escalamiento
a un organizador humano, mensajería directa entre jugadores, chat grupal por
equipo y moderación de mensajes reportados.

- **Stack:** Java 21 · Spring Boot 3.x · WebSocket/STOMP (tiempo real) ·
  PostgreSQL + JPA · OpenFeign · JWT (jjwt) · MapStruct.
- **Cobertura funcional:** TC-75 a TC-78 del catálogo oficial, sobre una
  estructura en capas (controller, service, repository, entity, dto, mapper).

## <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría

### ga-statistics-service

Cálculo y consulta de estadísticas de jugadores, equipos, partidos y torneo
(promedios, totales, rankings, tabla de posiciones, reconocimientos como
goleador y mejor defensa) a partir de los eventos de partido. Los agregados se
calculan con streams de Java sobre los documentos necesarios, no con
`AVG`/`GROUP BY` en la base de datos.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · Lombok.
- **Cobertura funcional:** TC-89 a TC-108 — ver [Análisis de Requerimientos](analisis-requerimientos.md).
- **Pruebas:** JUnit 5 + Mockito + AssertJ, cobertura 97% (mínimo exigido: 80%).

### ga-api-gateway-service

Puerta de enlace única de la plataforma (Spring Cloud Gateway + WebFlux):
enruta las peticiones del frontend a cada microservicio, aplica el filtro de
autenticación JWT (`AuthenticationFilter`) y centraliza la documentación
OpenAPI/Swagger de todos los servicios. Repositorio **privado**.

La auditoría no es un servicio centralizado propio: cada microservicio expone
su propio log de eventos (p. ej. TC-12 en Identidad, TC-23 en Equipos, TC-40
en Torneos, TC-73 en Logística), consultable por los roles autorizados.

## Infraestructura transversal

**TECH-CUP-Observability** (repositorio privado, solo infraestructura, sin
lógica de negocio) centraliza métricas, trazas distribuidas y logs de todos
los microservicios sobre Docker Compose: Prometheus + Grafana (métricas vía
`/actuator/prometheus`), Zipkin (trazas vía Micrometer Tracing) y
Elasticsearch + Logstash + Kibana (logs estructurados vía Logback).