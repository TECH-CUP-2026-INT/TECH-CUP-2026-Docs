# APP

TECH CUP 2026 es una sola aplicación de cara al usuario (el frontend en
[TECH-CUP-FRONT](https://github.com/TECH-CUP-2026-INT/TECH-CUP-FRONT)),
sostenida por los microservicios documentados en [Back](back.md). Esta
página describe la plataforma **por actor**: qué puede hacer cada uno y qué
servicio resuelve esa funcionalidad. Para el detalle técnico de cada
servicio, ver [Back](back.md); para el detalle de pantallas, ver
[Front](front.md).

## <span class="tc-badge tc-badge-cc">CC</span> Jugador / Capitán

| Funcionalidad | Servicio(s) |
|---|---|
| Registro (estudiante, invitado, egresado) e inicio de sesión | `cc-users-players-service` + `cc-identity-service` |
| Ver y editar perfil (general y deportivo: posición, dorsal) | `cc-users-players-service` |
| Crear equipo, invitar jugadores, aceptar/rechazar invitación, transferir capitanía | `cc-teams-service` (rol Capitán) |
| Inscribir el equipo a un torneo y pagar la inscripción por PSE | `mk-tournament-service` + `mk-payment-service` |
| Ver calendario, llaves/emparejamientos y mapa de canchas | `mk-tournament-service` |
| Ver sus partidos y resultado en vivo | `am-matches-service` (lectura) |
| Ver estadísticas propias y rankings públicos | `ga-statistics-service` |
| Recibir notificaciones (sanciones, invitaciones, cambios de inscripción, resultados) | `am-notification-service` |
| Chat de soporte, mensajería directa y chat de equipo | `am-communication-service` |

## <span class="tc-badge tc-badge-am">AM</span> Árbitro

| Funcionalidad | Servicio(s) |
|---|---|
| Inicio de sesión como árbitro | `cc-identity-service` |
| Ver los partidos asignados | `am-matches-service` |
| Gestionar el partido en vivo: iniciar, pausar, reanudar, tiempo añadido, finalizar | `am-matches-service` |
| Registrar goles, tarjetas (con sanción por acumulación), sustituciones y observaciones | `am-matches-service` |
| Subir la planilla/acta del partido | `am-matches-service` |
| Recibir y devolver dotación (petos, balones, kits) | `am-logistic-service` |

## <span class="tc-badge tc-badge-mk">MK</span> Organizador / Administrador

| Funcionalidad | Servicio(s) |
|---|---|
| Crear administradores y árbitros | `cc-users-players-service` |
| Crear y editar torneos, reglamento, canchas, calendario | `mk-tournament-service` |
| Aprobar/rechazar inscripciones de equipos | `mk-tournament-service` |
| Definir y registrar entrega de refrigerios; controlar dotación de árbitros | `am-logistic-service` |
| Recibir escalamiento de conversaciones de soporte | `am-communication-service` |
| Consultar estadísticas y reconocimientos del torneo | `ga-statistics-service` |

## Público / Aficionado (sin autenticación)

| Funcionalidad | Servicio(s) |
|---|---|
| Ver historial de torneos finalizados | `mk-tournament-service` |
| Ver calendario, llaves y mapa de canchas | `mk-tournament-service` |
| Ver tabla de posiciones y rankings públicos (goleo, victorias, fair play) | `ga-statistics-service` |

## Notas de accesibilidad

Varios servicios documentan explícitamente que el color nunca es el único
canal de información: `am-matches-service` expone un campo `eventType` en
cada evento de partido (gol, tarjeta, inicio, fin) además del color, para no
depender del color en usuarios con daltonismo — coherente con la sección de
accesibilidad del [manual de identidad](index.md) aplicado a esta
documentación.