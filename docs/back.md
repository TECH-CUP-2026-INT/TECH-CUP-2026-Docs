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
emisor del JWT que valida el API Gateway y que el resto de servicios confía
sin volver a verificar la firma. Java (con scripts Python auxiliares),
Dockerizado. Su documentación técnica (arquitectura, requerimientos) todavía
está en borrador en el propio repositorio.

### cc-users-players-service

Fuente de verdad del perfil de usuario: registro de estudiantes, invitados y
egresados, creación de árbitros/administradores, perfil general y perfil
deportivo (posición, dorsal), y promoción de jugador a capitán.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB · OpenFeign (Identity, Teams).
- **Estado:** requerimientos TC-01 a TC-19 documentados y contrastados contra la
  implementación. Huecos conocidos: TC-16 (historial de torneos de un jugador)
  depende de un endpoint que Torneos aún no expone; TC-18 (promover a capitán)
  está roto en la práctica porque el endpoint de sincronización de rol en
  Identity Service no existe todavía.

!!! warning "Sin Dockerfile"
    A diferencia de los demás servicios, este repositorio no tiene
    `Dockerfile` en la raíz, aunque su workflow de CI sí tiene un job de
    `dockerize-publish` — ese job falla hasta que se agregue el Dockerfile.

### cc-teams-service

Gestión de equipos: creación, invitaciones, capitanía e inscripción a
torneos. Actor principal: el **Capitán**.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB · OpenFeign (Identity, Torneos).
- **Estado:** requerimientos TC-20 a TC-28. Implementados: crear equipo,
  consultar equipos de un torneo, inscribir equipo (mínimo 7 integrantes),
  invitar jugador, aceptar/rechazar invitación, transferir capitanía. **No
  implementados:** editar equipo (TC-23) y eliminar equipo (TC-28). El detalle
  completo de equipo con perfiles de miembros (TC-22) solo existe como
  endpoint servicio-a-servicio, no de cara al capitán.

## <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos

### mk-tournament-service

Ciclo de vida completo del torneo: creación, reglamento, canchas, generación
de emparejamientos (brackets / grupos / liga), calendario, mapa de canchas,
tabla de posiciones y llaves en tiempo real.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · arquitectura hexagonal.
- **Requerimientos:** TC-29 a TC-54 (excluyendo TC-52, que pertenece al
  Servicio de Inscripción).

!!! warning "Seguridad pendiente"
    El control de acceso por rol (Organizador, Capitán, Jugador) vía JWT de
    `cc-identity-service` está documentado como intención de negocio, pero
    los endpoints **están actualmente abiertos** — todavía no se aplica.

### mk-payment-service

Procesa los pagos de inscripción al torneo exclusivamente vía **PSE** (Mercado
Pago): crear orden de pago, consultar límites de monto habilitados, iniciar
transacción PSE, recibir webhook de resultado (aprobada/rechazada/cancelada),
consultar estado de una orden, sincronización diaria de límites, y expiración
automática de órdenes pendientes (60 min, revisado cada 5 min).

- **Stack:** Java 21 · Spring Boot · Maven · PostgreSQL 16 · Mercado Pago (sandbox PSE).
- **Requerimientos:** RF-01 a RF-07+, todos implementados.

## <span class="tc-badge tc-badge-am">AM</span> Partidos y Logística

Los tres servicios de este dominio (equipo **astromerge**) comparten el mismo
modelo de seguridad: el API Gateway valida la firma del JWT y estos servicios
**no la vuelven a verificar** — solo leen los claims. Por diseño, ninguno debe
exponerse directo a internet, solo detrás del Gateway.

### am-matches-service

Arbitraje en tiempo real: iniciar/pausar/reanudar/finalizar partido, tiempo
añadido, goles, tarjetas (con regla de sanción por acumulación), sustituciones,
observaciones y subida de la planilla del partido.

- **Stack:** Java 21 · Spring Boot 3.5.6 · Maven · MongoDB.
- **Requerimientos:** RF-01 a RF-12, todos implementados. Endpoints bajo
  `/api/partidos`, rol requerido: árbitro.
- **Seguridad:** se corrigió un path traversal en la subida de planilla y se
  agregó límite de tamaño/tipo de archivo durante una revisión de seguridad.

### am-logistic-service

Operación no deportiva del torneo: refrigerios y dotación (petos, balones,
kits) para equipos, jugadores y árbitros. Actor principal: el **Organizador**.

- **Stack:** Java 21 · Spring Boot · Maven · MongoDB · Testcontainers · JaCoCo.
- **Requerimientos:** RF-01 a RF-05, todos implementados (incluye devolución
  de dotación con estado `DEVUELTO`).
- **Pendiente:** confirmar con Torneos, Equipos y Auditoría los contratos REST
  reales que hoy están asumidos tras interfaces/puertos (`TorneoClientPort`,
  `EquipoClientPort`, `AuditoriaClientPort`).

### am-notification-service

Consumidor puro de eventos de otros microservicios (sanciones, mensajes,
solicitudes/invitaciones de equipo, cambios de inscripción, agendamiento de
partidos) vía **9 webhooks REST** más consumo directo de dos tipos de evento
del exchange RabbitMQ compartido (`techcup.exchange`). Produce historial de
notificaciones in-app y correo best-effort.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · Spring AMQP · Spring Mail ·
  Micrometer + Prometheus + Zipkin.
- **Requerimientos:** RF-01 confirmado end-to-end con `am-matches-service`.
  RF-02 a RF-06 tienen el endpoint implementado pero el contrato está
  **propuesto**, pendiente de confirmación con Equipos, Comunicaciones e
  Inscripción. RF-07 (comprobante de pago) fue **descontinuado**: con la
  integración de Mercado Pago ya no hay comprobante que subir.

### am-communication-service

Mensajería de la plataforma: chat de soporte asistido por chatbot, escalamiento
a un organizador humano, mensajería directa entre jugadores, chat grupal por
equipo y moderación de mensajes reportados.

- **Stack:** Java 21 · Spring Boot 3.x · WebSocket/STOMP (tiempo real) ·
  PostgreSQL + JPA · OpenFeign · JWT (jjwt) · MapStruct.

!!! info "Fase temprana"
    A diferencia de los demás servicios de este dominio, aquí la estructura
    de paquetes está definida pero **la lógica de negocio todavía no está
    implementada** — los 5 requerimientos funcionales (RF-01 a RF-05) están
    marcados como *Planned*, no *Implemented*.

## <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría

### ga-statistics-service

Cálculo y consulta de estadísticas de jugadores, equipos, partidos y torneo
(promedios, totales, rankings, tabla de posiciones, reconocimientos como
goleador y mejor defensa) a partir de los eventos de partido. Los agregados se
calculan con streams de Java sobre los documentos necesarios, no con
`AVG`/`GROUP BY` en la base de datos.

- **Stack:** Java 21 · Spring Boot 3.5.6 · MongoDB · Lombok.
- **Requerimientos:** RF1 a RF15.
- **Pruebas:** JUnit 5 + Mockito + AssertJ, cobertura actual 97% (mínimo
  exigido: 80%).

### ga-api-gateway-service

Puerta de enlace única de la plataforma (Spring Cloud Gateway + WebFlux):
enruta las peticiones del frontend a cada microservicio, aplica el filtro de
autenticación JWT (`AuthenticationFilter`) y centraliza la documentación
OpenAPI/Swagger de todos los servicios. Repositorio **privado**.

!!! warning "`ga-audit-service` no existe"
    Referenciado en `arquitectura.md` y en varios README de otros servicios
    como destino de auditoría (`AuditoriaClientPort`, reportes best-effort),
    pero no está creado en la organización `TECH-CUP-2026-INT`. Hasta que
    exista, esas integraciones de auditoría no tienen un servicio real del
    otro lado.

## Infraestructura transversal

**TECH-CUP-Observability** (repositorio privado, solo infraestructura, sin
lógica de negocio) centraliza métricas, trazas distribuidas y logs de todos
los microservicios sobre Docker Compose: Prometheus + Grafana (métricas vía
`/actuator/prometheus`), Zipkin (trazas vía Micrometer Tracing) y
Elasticsearch + Logstash + Kibana (logs estructurados vía Logback).