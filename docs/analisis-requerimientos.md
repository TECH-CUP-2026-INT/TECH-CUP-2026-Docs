# Análisis de Requerimientos

## 1. Introducción

TECH CUP 2026 digitaliza el torneo semestral de fútbol de la Escuela
Colombiana de Ingeniería Julio Garavito. Este documento consolida el catálogo
oficial de requerimientos funcionales del torneo (TC-01 a TC-108), repartido
en cuatro dominios — uno por equipo — y contrastado con la plataforma
descrita en [Back](back.md) y [APP](app.md).

## 2. Propósito y alcance

- **Propósito:** dar una vista única y trazable de los 108 requerimientos
  funcionales oficiales del torneo, organizados por dominio.
- **Alcance:** las funcionalidades de identidad y usuarios, torneos y pagos,
  partidos/logística/comunicaciones, y analítica.
- **Fuera de alcance:** el detalle de flujo básico/alterno y las reglas de
  negocio ficha por ficha (viven en el documento de requerimientos original
  de cada equipo).

## 3. Actores del sistema

| Actor | Descripción |
|---|---|
| Estudiante | Se registra con su correo institucional; queda con rol Jugador. |
| Invitado | Familiar o amigo de un estudiante; se registra con Gmail declarando su asociación. |
| Egresado | Se registra con correo institucional (si sigue activo) o con Gmail. |
| Jugador | Rol base de todo usuario registrado (estudiante, invitado o egresado). |
| Capitán | Jugador con permisos adicionales sobre su equipo, activados mediante un interruptor de rol. |
| Árbitro | Cuenta creada por el Organizador; gestiona el desarrollo en vivo de los partidos asignados. |
| Organizador | Crea y administra torneos, canchas, calendario, logística y validación de inscripciones. |
| Administrador | Creado junto con el Organizador; gestión general de la plataforma y consulta de auditoría. |
| Usuario autenticado / público | Consulta calendario, llaves, historial, rankings y estadísticas — varias vistas son accesibles sin autenticarse. |
| Mercado Pago | Sistema externo que procesa transacciones PSE y notifica su resultado vía webhook. |

## 4. Requerimientos funcionales

Catálogo oficial de las 108 fichas de requerimiento del torneo, una por
funcionalidad, organizadas por dominio.

### <span class="tc-badge tc-badge-cc">CC</span> Identidad y Usuarios (TC-01 – TC-28)

| ID | Nombre | Descripción |
|---|---|---|
| TC-01 | Registro de estudiante | Permite a un estudiante crear una cuenta con su correo institucional universitario. |
| TC-02 | Registro de invitado (familiar/amigo) | Permite a un familiar o amigo de un estudiante crear una cuenta con Gmail, declarando su asociación con el estudiante. |
| TC-03 | Registro de egresado | Permite a un egresado crear una cuenta con su correo institucional (si sigue activo) o con Gmail. |
| TC-04 | Creación de cuenta de árbitro | Permite al Organizador crear la cuenta de un árbitro; los árbitros no se autoregistran. |
| TC-05 | Creación de usuarios Admin y Organizador | Permite que un Admin o un Organizador se autoregistren con permisos elevados. |
| TC-06 | Inicio de sesión (correo institucional) | Permite autenticarse con correo institucional y contraseña. |
| TC-07 | Inicio de sesión con Gmail | Permite autenticarse con Gmail a invitados, árbitros, egresados sin correo institucional y organizadores. |
| TC-08 | Validación de token JWT | Valida que cada solicitud protegida incluya un JWT vigente antes de conceder acceso. |
| TC-09 | Recuperación de contraseña | Permite restablecer la contraseña mediante un enlace o código enviado al correo registrado. |
| TC-10 | Validación de doble factor por OTP | Valida la identidad en el login mediante un código de un solo uso enviado al correo. |
| TC-11 | Expiración automática de sesión | Expira la sesión automáticamente tras un periodo de inactividad definido. |
| TC-12 | Consultar eventos del Servicio de Identidad | Permite al Admin consultar el log de auditoría del Servicio de Identidad. |
| TC-13 | Crear perfil deportivo | Permite a un jugador crear su perfil deportivo (posición, dorsal, foto opcional). |
| TC-14 | Consultar perfil deportivo | Permite a cualquier usuario autenticado ver el perfil deportivo de un jugador. |
| TC-15 | Consultar eventos del Servicio de Usuarios y Jugadores | Permite al Admin consultar el log de auditoría de este servicio. |
| TC-16 | Actualizar datos básicos del usuario | Permite actualizar datos personales básicos (correo y contraseña no se modifican por esta vía). |
| TC-17 | Actualizar perfil deportivo | Permite actualizar posición, dorsal y foto del perfil deportivo. |
| TC-18 | Ascender a Capitán | Permite a un jugador activar el rol de Capitán mediante un interruptor, habilitando la gestión de equipos. |
| TC-19 | Deshabilitar usuario (nivel general) | Permite al Admin deshabilitar una cuenta a nivel general de la plataforma. |
| TC-20 | Crear equipo | Permite a un Capitán crear un equipo con nombre y logo. |
| TC-21 | Consultar alineación del equipo | Permite ver la alineación configurada para un partido: titulares, suplentes y formación. |
| TC-22 | Consultar plantilla del equipo (álbum de jugadores) | Muestra a todos los jugadores del equipo en formato de galería tipo álbum de figuritas. |
| TC-23 | Consultar eventos del Servicio de Equipos | Permite al Admin consultar el log de auditoría del Servicio de Equipos. |
| TC-24 | Actualizar equipo | Permite al Capitán actualizar nombre y logo del equipo, si no está inscrito en un torneo activo/en curso. |
| TC-25 | Gestionar invitaciones de equipo | Permite al Capitán invitar jugadores registrados y a estos aceptar o rechazar la invitación. |
| TC-26 | Retirar jugador del equipo | Permite al Capitán retirar a un jugador del equipo, si no participa en un torneo activo/en curso. |
| TC-27 | Transferir capitanía | Permite transferir la capitanía a otro jugador, o que un jugador se postule para ella. |
| TC-28 | Eliminar equipo | Permite al Capitán eliminar el equipo, si no está inscrito en un torneo activo/en curso. |

### <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos (TC-29 – TC-56)

| ID | Nombre | Descripción |
|---|---|---|
| TC-29 | Crear torneo | Permite al Organizador registrar un torneo; se crea en estado Activo por defecto. |
| TC-30 | Registrar reglamento del torneo | Permite adjuntar el reglamento oficial del torneo en PDF. |
| TC-31 | Registrar canchas del torneo | Registra nombre, imagen y descripción opcionales, y ubicación en el mapa del campus. |
| TC-32 | Generar emparejamientos | Genera automáticamente la estructura del torneo (Brackets, Grupos o Liga) al pasar a estado Preparación. |
| TC-33 | Programar partidos | Crea un partido asignando emparejamiento, cancha, árbitro, fecha y hora. |
| TC-34 | Consultar reglamento interno | Permite ver o descargar el reglamento oficial del torneo. |
| TC-35 | Consultar mapa interactivo de canchas | Muestra cada cancha registrada con su partido asignado y estado actual. |
| TC-36 | Consultar torneos históricos | Consulta en modo solo lectura la información completa de torneos finalizados, incluso sin autenticarse. |
| TC-37 | Ver equipos inscritos en el torneo | Lista los equipos inscritos con su nombre y logo. |
| TC-38 | Ver emparejamientos | Muestra la estructura de emparejamientos (bracket, grupos o tabla de liga) de forma visual. |
| TC-39 | Ver canchas de los partidos | Muestra la cancha asignada a un partido con nombre, imagen y ubicación. |
| TC-40 | Consultar eventos del Servicio de Torneos | Consulta el log de auditoría del Servicio de Torneos. |
| TC-41 | Editar torneo | Permite corregir cualquier campo de la configuración del torneo; bloqueado si está Finalizado. |
| TC-42 | Pausar torneo | Suspende temporalmente las inscripciones, sin perder la consulta de datos. |
| TC-43 | Inactivar torneo | Bloquea todas las funcionalidades asociadas al torneo; puede reactivarse. |
| TC-44 | Inactivar equipo en el torneo | Inactiva temporalmente a un equipo (medida administrativa, no descalificación). |
| TC-45 | Inactivar usuario (nivel torneo) | Inactiva a un usuario dentro de un torneo específico. |
| TC-46 | Inactivar partido | Impide el registro de eventos sobre un partido específico. |
| TC-47 | Sancionar jugador (dentro del torneo) | Aplica una suspensión por acumulación de tarjetas o por decisión del Organizador. |
| TC-48 | Descalificar equipo | Excluye a un equipo de futuros emparejamientos, conservando sus estadísticas previas. |
| TC-49 | Finalizar torneo (archivar al histórico) | Marca el torneo como Finalizado al pasar su fecha de cierre. |
| TC-50 | Asignar equipo campeón | Registra al campeón al finalizar el partido final; el empate se resuelve por definición a penales. |
| TC-51 | Eliminar torneo | Elimina permanentemente un torneo, solo una vez está Finalizado. |
| TC-52 | Inscribir equipo al torneo | El Capitán inscribe su equipo; la inscripción queda en estado Pendiente de Pago. |
| TC-53 | Subir comprobante de pago | El Capitán sube el comprobante de la inscripción, que pasa a estado En Revisión. |
| TC-54 | Consultar eventos del Servicio de Inscripción | Consulta el log de auditoría del Servicio de Inscripción. |
| TC-55 | Validar inscripción de equipo | El Organizador revisa el comprobante y aprueba o rechaza la inscripción. |
| TC-56 | Retirar inscripción de equipo | El Capitán cancela la inscripción, mientras el torneo esté Activo. |

### <span class="tc-badge tc-badge-am">AM</span> Partidos, Logística y Comunicación (TC-57 – TC-88)

| ID | Nombre | Descripción |
|---|---|---|
| TC-57 | Iniciar partido | El Árbitro inicia oficialmente el partido, habilitando cronómetro y registro de eventos. |
| TC-58 | Registrar goles del partido | Asocia cada gol a jugador, equipo y minuto; el marcador se actualiza en vivo. |
| TC-59 | Registrar tarjetas del partido | Registra tarjetas amarillas o rojas; la acumulación dispara sanciones automáticas (TC-47). |
| TC-60 | Registrar sustituciones | Registra jugador que sale, jugador que entra y el minuto. |
| TC-61 | Registrar observaciones del partido | Registra notas en tiempo real que quedan en el acta permanente. |
| TC-62 | Subir planilla del árbitro | Sube la planilla oficial (PDF o imagen) con notas adicionales. |
| TC-63 | Publicar calendario de partidos | Publica el calendario del torneo para todos los usuarios autenticados. |
| TC-64 | Consultar calendario de partidos | Permite filtrar el calendario por equipo, fase o fecha. |
| TC-65 | Consultar eventos del Servicio de Partidos | Consulta el log de auditoría del Servicio de Partidos. |
| TC-66 | Controlar desarrollo cronológico del partido | Gestiona el cronómetro: iniciar, pausar, reanudar, cerrar periodos y tiempo extra. |
| TC-67 | Editar partido | Modifica cancha, árbitro, fecha y hora, si la fecha programada no ha pasado. |
| TC-68 | Finalizar partido | Registra el resultado final; si un equipo no se presenta, se falla por walkover. |
| TC-69 | Eliminar partido | Elimina un partido, solo si un equipo participante fue descalificado o hubo error de emparejamiento. |
| TC-70 | Definir refrigerios por equipo | Define tipo y cantidad de refrigerios para los equipos clasificados a la siguiente ronda. |
| TC-71 | Registrar entrega de refrigerios | Registra la entrega al Capitán del equipo clasificado, evitando duplicados. |
| TC-72 | Entregar dotación | Registra la entrega de dotación (silbatos, tarjetas, kits) a un árbitro. |
| TC-73 | Consultar eventos del Servicio de Logística | Consulta el log de auditoría del Servicio de Logística. |
| TC-74 | Devolver dotación | Registra la devolución de dotación por parte de un árbitro. |
| TC-75 | Chat de ayuda y soporte con chatbot | Un chatbot responde preguntas frecuentes sobre el torneo y la plataforma. |
| TC-76 | Escalar conversación de soporte al Organizador | Escala una conversación no resuelta; el Organizador ve el historial completo. |
| TC-77 | Chat grupal de equipo | Chat creado automáticamente para cada equipo; los miembros se agregan/quitan automáticamente. |
| TC-78 | Consultar eventos del Servicio de Comunicaciones | Consulta el log de auditoría del Servicio de Comunicaciones. |
| TC-79 | Notificación de sanción por acumulación de tarjetas | Notifica automáticamente al jugador sancionado. |
| TC-80 | Notificación de nuevo mensaje de chat | Notifica a los miembros del equipo cuando llega un mensaje al chat grupal. |
| TC-81 | Notificación de solicitud de unión a equipo | Notifica al Capitán cuando un jugador solicita unirse a su equipo. |
| TC-82 | Notificación de respuesta a solicitud de unión | Notifica al jugador la respuesta del Capitán. |
| TC-83 | Notificación de invitación a equipo | Notifica a un jugador cuando el Capitán le envía una invitación. |
| TC-84 | Notificación de cambio de estado de inscripción | Notifica al Capitán cuando cambia el estado de la inscripción de su equipo. |
| TC-85 | Notificación de programación/reprogramación/cancelación de partido | Notifica al Capitán y jugadores. |
| TC-86 | Notificación de inscripción completada con comprobante | Notifica al Organizador cuando un Capitán sube el comprobante de pago. |
| TC-87 | Notificación de transferencia de capitanía | Notifica al jugador (si el Capitán delega) o al Capitán (si un jugador se postula). |
| TC-88 | Consultar historial de notificaciones | Cada usuario autenticado consulta su propio historial de notificaciones. |

### <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría (TC-89 – TC-108)

| ID | Nombre | Descripción |
|---|---|---|
| TC-89 | Consultar promedios de victorias/derrotas/empates | Promedios por equipo, calculados a partir de resultados finalizados. |
| TC-90 | Consultar goleador del torneo | Ranking de goleadores con nombre, equipo y goles totales. |
| TC-91 | Consultar mejor portero (menos goles recibidos) | Ranking de porteros con menos goles recibidos. |
| TC-92 | Consultar promedio de goles por partido de un equipo | Promedio de goles anotados por partido para un equipo específico. |
| TC-93 | Consultar promedio de faltas por partido | Promedio general o por equipo. |
| TC-94 | Generar rankings públicos | Tabla de posiciones, goleadores y fair play, visibles incluso sin autenticarse. |
| TC-95 | Consultar estadísticas generales del torneo | Resumen de posiciones, puntos y resultados. |
| TC-96 | Enviar reconocimientos (goleador y mejor portero) | Publica los reconocimientos de fin de torneo al finalizar. |
| TC-97 | Consultar promedio de minutos jugados | Promedio de minutos jugados por un jugador. |
| TC-98 | Consultar faltas cometidas | Total de faltas cometidas por un jugador. |
| TC-99 | Consultar goles anotados | Total de goles anotados por un jugador. |
| TC-100 | Consultar número de partidos jugados | Total de partidos finalizados en los que participó un jugador. |
| TC-101 | Consultar asistencias | Total de asistencias de un jugador. |
| TC-102 | Consultar tarjetas | Total de tarjetas amarillas y rojas recibidas por un jugador. |
| TC-103 | Consultar estadísticas del equipo (torneo activo) | Estadísticas completas de un equipo en el torneo activo/en curso. |
| TC-104 | Consultar faltas del equipo | Total de faltas cometidas por un equipo. |
| TC-105 | Consultar goles del equipo | Total de goles a favor y en contra de un equipo. |
| TC-106 | Consultar promedios del partido | Promedios de goles, faltas y tarjetas por partido en el torneo. |
| TC-107 | Consultar total de tarjetas | Total de tarjetas amarillas y rojas de un partido o del torneo. |
| TC-108 | Consultar estado del partido (ganado/empatado/perdido) | Resultado de un partido para un equipo dado. |

## 5. Requerimientos no funcionales

| ID | Categoría | Descripción |
|---|---|---|
| RNF-01 | Seguridad | El JWT se emite en `cc-identity-service` y se valida una única vez en el API Gateway (`ga-api-gateway-service`); los microservicios internos confían en los claims ya validados. |
| RNF-02 | Rendimiento | El Servicio de Estadísticas documenta un máximo de 2 segundos de respuesta para sus consultas. |
| RNF-03 | Calidad de código | Cobertura de pruebas exigida (JaCoCo) y análisis estático con SonarQube/SonarCloud como gates de CI. |
| RNF-04 | Observabilidad | Métricas (Prometheus/Grafana), trazas distribuidas (Zipkin) y logs centralizados (ELK) vía `TECH-CUP-Observability`. |
| RNF-05 | Resiliencia | Las integraciones no críticas entre servicios (auditoría, estadísticas, notificaciones) son best-effort. |
| RNF-06 | Accesibilidad | El color nunca es el único canal de información — cada evento de partido expone un `eventType` explícito además del color. |
| RNF-07 | Escalabilidad | Servicios sin estado de sesión en servidor (`STATELESS`), para escalar horizontalmente. |

## 6. Reglas de negocio

| ID | Descripción |
|---|---|
| RN-01 | Todo estudiante, invitado o egresado queda registrado con el rol Jugador por defecto (TC-01). |
| RN-02 | La contraseña se almacena siempre cifrada, nunca en texto plano (TC-01). |
| RN-03 | Un jugador acumula 2 tarjetas amarillas en el mismo partido, o una roja directa, para ser sancionado (TC-47, TC-59). |
| RN-04 | Una orden de pago de inscripción vence a los 60 minutos si queda pendiente o esperando confirmación bancaria (`mk-payment-service`). |
| RN-05 | El desempate del partido final del torneo se resuelve por definición a penales (TC-50). |
| RN-06 | La transferencia de capitanía puede iniciarla el Capitán (delegando) o un jugador (postulándose) (TC-27, TC-87). |

## 7. Glosario

| Término | Definición |
|---|---|
| TC | Identificador de cada ficha de requerimiento funcional del torneo (Test Case / Ticket de Capacidad), numerado de forma continua TC-01 a TC-108 entre los cuatro dominios. |
| RNF / RN | Requerimiento no funcional / regla de negocio. |
| Best-effort | Llamada o integración cuyo fallo se registra pero nunca bloquea ni revierte la operación principal. |
| PSE | Pagos Seguros en Línea — medio de pago habilitado para inscripciones, procesado vía Mercado Pago. |
| Capitán | Jugador con permisos de gestión sobre su equipo. |
| Walkover | Resultado de un partido en el que un equipo gana porque el rival no se presentó. |

## 8. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Primera consolidación a partir de la documentación de cada microservicio |
| 0.3 | 2026-07-17 | | Catálogo completo de los 108 requerimientos oficiales (TC-01 – TC-108) por dominio |