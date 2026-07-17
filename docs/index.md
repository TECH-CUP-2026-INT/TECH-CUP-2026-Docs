<div class="tc-hero">
  <img src="assets/img/logo.png" alt="Logo TechCup" class="tc-hero-logo">
  <p class="tc-hero-eyebrow">Documentación oficial</p>
  <h1 class="tc-hero-title">TECH<span>CUP</span> 2026</h1>
  <p class="tc-hero-sub">&ldquo;La pasión nos une, la ingeniería nos impulsa&rdquo;</p>
</div>

Bienvenido al repositorio central de documentación de **TECH CUP 2026**. Este sitio reúne los documentos transversales del proyecto (requerimientos, arquitectura, pruebas y organización) y sirve como **índice** hacia los repositorios de código de la organización [TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT).

## Documentos del proyecto

| Documento                                                | Descripción                                               |
|----------------------------------------------------------|-----------------------------------------------------------|
| [Análisis de Requerimientos](analisis-requerimientos.md) | Requerimientos funcionales y no funcionales del sistema   |
| [Documento de Arquitectura](arquitectura.md)             | Arquitectura de la solución, vistas y decisiones técnicas |
| [Pruebas Funcionales](pruebas-funcionales.md)            | Estrategia y casos de prueba funcionales                  |
| [Organización](organizacion.md)                          | Equipo, roles y forma de trabajo                          |
| [Back](back.md)                                          | Microservicios backend por dominio                        |
| [Front](front.md)                                        | Aplicación web y stack de frontend                         |
| [APP](app.md)                                            | La plataforma explicada por tipo de usuario                |
| [Entregables y pruebas](entregablespruebas.md)           | Entregables de cada repositorio e integración continua     |

## Repositorios de la organización

Los siguientes repositorios conforman la plataforma TECH CUP 2026, agrupados por dominio. La columna **Docs** enlaza al sitio MkDocs propio de cada repositorio, publicado en GitHub Pages.

### <span class="tc-badge tc-badge-cc">CC</span> Identidad y Usuarios

| Repositorio | Descripción | Docs |
|---|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> [cc-identity-service](https://github.com/TECH-CUP-2026-INT/cc-identity-service) | Servicio de identidad y autenticación | [📖 Docs](https://tech-cup-2026-int.github.io/cc-identity-service/) |
| <span class="tc-badge tc-badge-cc">CC</span> [cc-users-players-service](https://github.com/TECH-CUP-2026-INT/cc-users-players-service) | Gestión de usuarios y jugadores | [📖 Docs](https://tech-cup-2026-int.github.io/cc-users-players-service/) |
| <span class="tc-badge tc-badge-cc">CC</span> [cc-teams-service](https://github.com/TECH-CUP-2026-INT/cc-teams-service) | Gestión de equipos | [📖 Docs](https://tech-cup-2026-int.github.io/cc-teams-service/) |

### <span class="tc-badge tc-badge-mk">MK</span> Torneos y Pagos

| Repositorio | Descripción | Docs |
|---|---|---|
| <span class="tc-badge tc-badge-mk">MK</span> [mk-tournament-service](https://github.com/TECH-CUP-2026-INT/mk-tournament-service) | Gestión de torneos | [📖 Docs](https://tech-cup-2026-int.github.io/mk-tournament-service/) |
| <span class="tc-badge tc-badge-mk">MK</span> [mk-payment-service](https://github.com/TECH-CUP-2026-INT/mk-payment-service) | Procesamiento de pagos | [📖 Docs](https://tech-cup-2026-int.github.io/mk-payment-service/) |

### <span class="tc-badge tc-badge-am">AM</span> Partidos y Logística

| Repositorio | Descripción | Docs |
|---|---|---|
| <span class="tc-badge tc-badge-am">AM</span> [am-matches-service](https://github.com/TECH-CUP-2026-INT/am-matches-service) | Gestión de partidos | [📖 Docs](https://tech-cup-2026-int.github.io/am-matches-service/) |
| <span class="tc-badge tc-badge-am">AM</span> [am-logistic-service](https://github.com/TECH-CUP-2026-INT/am-logistic-service) | Logística de eventos | [📖 Docs](https://tech-cup-2026-int.github.io/am-logistic-service/) |
| <span class="tc-badge tc-badge-am">AM</span> [am-notification-service](https://github.com/TECH-CUP-2026-INT/am-notification-service) | Notificaciones | [📖 Docs](https://tech-cup-2026-int.github.io/am-notification-service/) |
| <span class="tc-badge tc-badge-am">AM</span> [am-communication-service](https://github.com/TECH-CUP-2026-INT/am-communication-service) | Comunicación entre servicios/usuarios | [📖 Docs](https://tech-cup-2026-int.github.io/am-communication-service/) |

### <span class="tc-badge tc-badge-ga">GA</span> Analítica y Auditoría

| Repositorio | Descripción | Docs |
|---|---|---|
| <span class="tc-badge tc-badge-ga">GA</span> [ga-statistics-service](https://github.com/TECH-CUP-2026-INT/ga-statistics-service) | Estadísticas de la plataforma | [📖 Docs](https://tech-cup-2026-int.github.io/ga-statistics-service/) |
| <span class="tc-badge tc-badge-ga">GA</span> [ga-api-gateway-service](https://github.com/TECH-CUP-2026-INT/ga-api-gateway-service) | Puerta de enlace de APIs (repositorio privado) | — |

### Frontend

| Repositorio | Descripción | Docs |
|---|---|---|
| [TECH-CUP-FRONT](https://github.com/TECH-CUP-2026-INT/TECH-CUP-FRONT) | Aplicación frontend de la plataforma | — |

### Infraestructura

| Repositorio | Descripción | Docs |
|---|---|---|
| [TECH-CUP-Observability](https://github.com/TECH-CUP-2026-INT/TECH-CUP-Observability) | Observabilidad de la plataforma (repositorio privado) | — |

### Documentación

| Repositorio | Descripción | Docs |
|---|---|---|
| [TECH-CUP-2026-Docs](https://github.com/TECH-CUP-2026-INT/TECH-CUP-2026-Docs) | Este repositorio: documentación central del proyecto | [📖 Docs](https://tech-cup-2026-int.github.io/TECH-CUP-2026-Docs/) |

La agrupación por dominio se basa en el prefijo del nombre de cada repositorio
(`cc-`, `mk-`, `am-`, `ga-`), y los enlaces de la columna **Docs** siguen el
patrón estándar de GitHub Pages (`https://tech-cup-2026-int.github.io/<repo>/`).
