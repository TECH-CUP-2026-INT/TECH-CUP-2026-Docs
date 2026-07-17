# Entregables y pruebas

## 1. Entregables

Cada repositorio de la organización [TECH-CUP-2026-INT](https://github.com/TECH-CUP-2026-INT)
es un entregable independiente, con su propio pipeline de CI/CD y (salvo el
frontend y la infraestructura) su propio sitio MkDocs. Ver [Inicio](index.md)
para el listado completo con enlaces.

## 2. Estado de CI/CD

Snapshot del último workflow de cada repositorio (capturado el 2026-07-17 —
consulta `gh run list --repo TECH-CUP-2026-INT/<repo>` para el estado actual,
ya que esta tabla no se actualiza sola).

| Repositorio | Último resultado de CI |
|---|---|
| <span class="tc-badge tc-badge-cc">CC</span> cc-identity-service | ✅ Exitoso |
| <span class="tc-badge tc-badge-cc">CC</span> cc-users-players-service | ❌ Falla — ver BUG-01 en [Pruebas Funcionales](pruebas-funcionales.md) |
| <span class="tc-badge tc-badge-cc">CC</span> cc-teams-service | ✅ Exitoso |
| <span class="tc-badge tc-badge-mk">MK</span> mk-tournament-service | ✅ Exitoso |
| <span class="tc-badge tc-badge-mk">MK</span> mk-payment-service | ✅ Exitoso |
| <span class="tc-badge tc-badge-am">AM</span> am-matches-service | 🔄 En progreso al momento de la captura |
| <span class="tc-badge tc-badge-am">AM</span> am-logistic-service | ❌ Falla |
| <span class="tc-badge tc-badge-am">AM</span> am-notification-service | 🔄 En progreso al momento de la captura |
| <span class="tc-badge tc-badge-am">AM</span> am-communication-service | ✅ Exitoso |
| <span class="tc-badge tc-badge-ga">GA</span> ga-statistics-service | ✅ Exitoso |
| <span class="tc-badge tc-badge-ga">GA</span> ga-api-gateway-service | ❌ Falla |
| Frontend — TECH-CUP-FRONT | — Sin workflows de CI configurados todavía |
| Infraestructura — TECH-CUP-Observability | — Sin workflows de CI configurados todavía |

## 3. Huecos conocidos entre entregables

| Hueco | Repositorios involucrados |
|---|---|
| `ga-audit-service` no existe, pero varios servicios reportan eventos de auditoría hacia él (best-effort) | am-logistic-service, am-matches-service |
| Contratos de webhook "propuestos" (no confirmados) entre equipos | am-notification-service ↔ am-communication-service, cc-teams-service |
| Seguridad por rol documentada pero no implementada (endpoints abiertos) | mk-tournament-service |
| Sincronización de rol capitán entre servicios rota | cc-users-players-service ↔ cc-identity-service |
| Lógica de negocio no implementada (fase de estructura base) | am-communication-service |

Ver [Pruebas Funcionales — Registro de defectos](pruebas-funcionales.md#5-registro-de-defectos)
para el detalle con severidad y estado.

## 4. Historial de cambios

| Versión | Fecha | Autor | Descripción |
|---|---|---|---|
| 0.1 | | | Versión inicial |
| 0.2 | 2026-07-17 | | Entregables y estado de CI completados a partir del estado real de cada repositorio |