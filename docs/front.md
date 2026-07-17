# Front

Aplicación web de TECH CUP 2026, en el repositorio
[TECH-CUP-FRONT](https://github.com/TECH-CUP-2026-INT/TECH-CUP-FRONT).

## Stack

| Capa | Tecnología |
|---|---|
| Framework | React 19 + TypeScript |
| Build / dev server | Vite 8 |
| Estilos | Tailwind CSS 4 (`@tailwindcss/vite`) |
| Componentes UI | Radix UI + `class-variance-authority` + `tailwind-merge` |
| Animaciones | Framer Motion |
| Enrutamiento | React Router DOM 7 |
| Autenticación social | `@react-oauth/google` |
| Visualización específica | `react-soccer-lineup` (alineaciones de equipo) |
| Lint | Oxlint |

## Estructura

```
src/
  api/          -> Clientes HTTP hacia los microservicios (client.ts, partidos.ts, torneos.ts, tipos.ts)
  services/     -> Lógica de acceso a datos por dominio (campus.ts, partidos.ts, torneos.ts)
  components/   -> Componentes reutilizables (calendario de torneo, temporizador de pago Mercado Pago,
                   selector de rol, partidos próximos, etc.)
  configs/      -> Configuración (tema)
  hooks/        -> Hooks de React reutilizables
  pages/        -> Una vista por ruta (ver mapa de páginas abajo)
  utils/        -> Utilidades generales
  assets/       -> Imágenes y recursos estáticos
```

## Mapa de páginas

| Área | Páginas |
|---|---|
| Público / autenticación | `Landing`, `Contacto`, `Login`, `Register`, `RecoverPassword` |
| Árbitro | `RefereeLogin`, `RefereeDashboard`, `Arbitraje`, `MatchDetail`, `MisPartidos` |
| Torneo | `TorneosPublic`, `DetalleTorneo`, `CrearTorneo`, `Calendario`, `Llaves`, `Campus` |
| Equipo | `CrearEquipo`, `InscribirEquipo`, `MiEquipo` |
| Perfil y cuenta | `Perfil`, `EditarPerfil` |
| Dashboards por rol | `Dashboard`, `DashboardAdmin`, `DashboardJugador` |
| Estadísticas | `Estadisticas`, `Rankings` |
| Comunicación | `Chat`, `Notificaciones`, `Soporte` |

Varios componentes reflejan directamente reglas de negocio del backend: por
ejemplo `MercadoPagoTimer` visualiza la ventana de expiración de la orden de
pago (60 minutos) que implementa `mk-payment-service`, y `TournamentCalendar`
/ `Llaves` consumen la generación de emparejamientos y calendario de
`mk-tournament-service`.

## Cómo correrlo

```bash
npm install
npm run dev       # servidor de desarrollo (Vite)
npm run build     # build de producción (tsc + vite build)
npm run lint      # Oxlint
```

## Documentación completa

Este repositorio todavía no publica un sitio MkDocs propio (a diferencia de
los microservicios backend) — su README usa la plantilla por defecto de Vite.
Si el equipo agrega documentación técnica extendida (arquitectura de
componentes, rutas protegidas, manejo de estado, contratos de API consumidos),
enlázala aquí.