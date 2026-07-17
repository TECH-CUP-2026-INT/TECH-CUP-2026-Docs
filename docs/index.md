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
| [back](back.md)                                          | back                                                      |
| [front](front.md)                                        | front                                                     |                                       |
| [app](app.md)                                            | app                                                       |  
| [Entregables](entregablespruebas.md)                     | Entregables                                               | 

## Repositorios de la organización

Los siguientes repositorios conforman la plataforma TECH CUP 2026, agrupados por dominio. La columna **Docs** enlaza al sitio MkDocs propio de cada repositorio, publicado en GitHub Pages.

### Identidad y Usuarios (`cc-`)

| Repositorio | Descripción | Docs |
|---|---|---|
| [cc-identity-service](https://github.com/TECH-CUP-2026-INT/cc-identity-service) | Servicio de identidad y autenticación | [📖 Docs](https://tech-cup-2026-int.github.io/cc-identity-service/) |
| [cc-users-players-service](https://github.com/TECH-CUP-2026-INT/cc-users-players-service) | Gestión de usuarios y jugadores | [📖 Docs](https://tech-cup-2026-int.github.io/cc-users-players-service/) |
| [cc-teams-service](https://github.com/TECH-CUP-2026-INT/cc-teams-service) | Gestión de equipos | [📖 Docs](https://tech-cup-2026-int.github.io/cc-teams-service/) |

### Torneos y Pagos (`mk-`)

| Repositorio | Descripción | Docs |
|---|---|---|
| [mk-tournament-service](https://github.com/TECH-CUP-2026-INT/mk-tournament-service) | Gestión de torneos | [📖 Docs](https://tech-cup-2026-int.github.io/mk-tournament-service/) |
| [mk-payment-service](https://github.com/TECH-CUP-2026-INT/mk-payment-service) | Procesamiento de pagos | [📖 Docs](https://tech-cup-2026-int.github.io/mk-payment-service/) |

### Partidos y Logística (`am-`)

| Repositorio | Descripción | Docs |
|---|---|---|
| [am-matches-service](https://github.com/TECH-CUP-2026-INT/am-matches-service) | Gestión de partidos | [📖 Docs](https://tech-cup-2026-int.github.io/am-matches-service/) |
| [am-logistic-service](https://github.com/TECH-CUP-2026-INT/am-logistic-service) | Logística de eventos | [📖 Docs](https://tech-cup-2026-int.github.io/am-logistic-service/) |
| [am-notification-service](https://github.com/TECH-CUP-2026-INT/am-notification-service) | Notificaciones | [📖 Docs](https://tech-cup-2026-int.github.io/am-notification-service/) |
| [am-communication-service](https://github.com/TECH-CUP-2026-INT/am-communication-service) | Comunicación entre servicios/usuarios | [📖 Docs](https://tech-cup-2026-int.github.io/am-communication-service/) |

### Analítica y Auditoría (`ga-`)

| Repositorio | Descripción | Docs |
|---|---|---|
| [ga-statistics-service](https://github.com/TECH-CUP-2026-INT/ga-statistics-service) | Estadísticas de la plataforma | [📖 Docs](https://tech-cup-2026-int.github.io/ga-statistics-service/) |
| [ga-audit-service](https://github.com/TECH-CUP-2026-INT/ga-audit-service) | Auditoría de eventos y acciones | [📖 Docs](https://tech-cup-2026-int.github.io/ga-audit-service/) |

### Frontend

| Repositorio | Descripción | Docs |
|---|---|---|
| [TECH-CUP-FRONT](https://github.com/TECH-CUP-2026-INT/TECH-CUP-FRONT) | Aplicación frontend de la plataforma | [📖 Docs](https://tech-cup-2026-int.github.io/TECH-CUP-FRONT/) |

### Documentación

| Repositorio | Descripción | Docs |
|---|---|---|
| [TECH-CUP-2026-Docs](https://github.com/TECH-CUP-2026-INT/TECH-CUP-2026-Docs) | Este repositorio: documentación central del proyecto | [📖 Docs](https://tech-cup-2026-int.github.io/TECH-CUP-2026-Docs/) |

!!! note "Agrupación por dominio"
    La agrupación anterior se infiere del prefijo de nombre de cada repositorio (`cc-`, `mk-`, `am-`, `ga-`). Si los nombres de dominio oficiales difieren, actualiza esta tabla en `docs/index.md`.

!!! warning "Enlaces Docs pendientes"
    Los enlaces de la columna **Docs** siguen el patrón estándar de GitHub Pages (`https://tech-cup-2026-int.github.io/<repo>/`) que tendrá cada repositorio una vez se le agregue MkDocs y se habilite Pages con fuente "GitHub Actions". Hasta que cada equipo lo configure en su propio repositorio, esos enlaces devolverán 404.
