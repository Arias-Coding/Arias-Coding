# Arias Coding Github

### 🛠️ Stack Técnico

| Área | Tecnologías |
| --- | --- |
| **Frontend / Core** | React 19.2, Next.js 16.3 (App Router, Server Components, Server Actions, Route Handlers, Middleware), TypeScript 5 (modo `strict`) |
| **Estilos & UI** | Tailwind CSS 4 (`@theme` + tokens CSS), sistema de diseño propio *neumórfico* (`shadow-neu`, `shadow-neu-inset`, `radius-bento/panel`), iconografía **SVG inline** handmade (`src/components/icons.tsx`), `next/font` con **Geist / Geist Mono**, `color-scheme: dark` nativo, transiciones CSS + `ResizeObserver` / `IntersectionObserver` en lugar de una librería de animación |
| **Backend & BaaS** | **Supabase** (Auth email/contraseña, Postgres, RLS, grants por rol), **Supabase SSR** (`@supabase/ssr`) con sesiones en cookies httpOnly, API REST autogenerada vía `supabase-js`, capa de acceso a datos con **actualizaciones optimistas y rollback** (`src/lib/api.ts`, `src/lib/dashboard.tsx`) |
| **Servidor de aplicación** | **Hono 4** sobre `@hono/node-server` (Node.js), **Argon2** (hashing de contraseñas), **JOSE** (JWT de sesión con `issuer`/`audience`), serialización de transacciones para PGlite, middleware CORS y cookies |
| **Base de datos** | **PostgreSQL** gestionado por Supabase + **PGlite** (Postgres WASM) para dev local, **13 migraciones SQL** idempotentes (`db/migrations/0001…0013`), enums nativos, índices por `user_id`, triggers `updated_at`, `RLS` por fila (`auth.uid() = user_id`) y `DEFAULT PRIVILEGES` |
| **Persistencia de sesiones Pomodoro** | **GitHub Gist** privado vía REST API (`data.json`) como almacén externo, caché local en disco (`data/data.json`) como *fallback* best‑effort, expuesta por `GET/POST /api/pomodoros` |
| **Estado & Arquitectura** | **React Context + Provider** (`DashboardProvider`) como store global, hooks de dominio (`useDock`), *Server Components* para datos y *Client Components* para interacción, separación por dominio en `src/lib` y `src/components` |
| **Herramientas & Calidad** | ESLint 9 (`eslint-config-next` core-web-vitals + typescript), TypeScript estricto, pnpm 11, PostCSS, `@electric-sql/pglite`, scripts `db:migrate` / `db:seed` / `db:reset` / `db:start`, Git |
| **Despliegue** | Next.js standalone sobre **Vercel/Netlify** (build `next build`, `next start`), variables de entorno para Supabase, `GITHUB_TOKEN`, `GITHUB_GIST_ID` y `JWT_SECRET` |

---

### 📂 Proyectos Destacados

#### 1. FocusFlow: Productivity Suite
**[Ver Repositorio](https://github.com/Arias-Coding/FocusFlow)** | **[Web en vivo](https://dashboard-personal-ten-delta.vercel.app/)**

**Suite integral de productividad** que fusiona la metodología Pomodoro con gestión de tareas, hábitos, agenda y analítica, bajo una arquitectura **full‑stack** moderna, tipada y escalable.

- **Stack Tecnológico Core:** Desarrollado con **Next.js 16 (App Router)** y **React 19** + **TypeScript**, con *Server Components* para el acceso a datos, *middleware* para el control de rutas y una capa de componentes cliente altamente reutilizable y estrictamente tipada.

- **Autenticación y Gestión de Usuarios (Supabase Auth):** El acceso se valida en el servidor mediante un **middleware** que crea un `ServerClient` con `@supabase/ssr` y llama a `auth.getUser()` en cada petición. Las rutas públicas (`/login`, `/register`) expulsan a los usuarios ya autenticados y todo el resto (`/dashboard`) redirige al login con el parámetro `redirect`; la sesión se mantiene en **cookies httpOnly** refrescadas por el middleware.

- **Capa de Datos y Persistencia:** La lógica de negocio se conecta directamente a **Supabase Postgres** mediante `supabase-js` (cliente de navegador) y **Supabase Auth**. El estado global se gestiona con un **Context + Provider** propio (`DashboardProvider`) en lugar de un store externo, con **actualizaciones optimistas** y *rollback* automático ante fallo de red, evitando recargas completa del dashboard.

- **Seguridad a nivel de fila (RLS):** Cada tabla (`tasks`, `boards`, `board_columns`, `habits`, `habit_entries`, `calendar_events`, `agenda_blocks`, `urgent_items`, `quick_notes`, `pomodoro_sessions`, `profiles`) tiene **políticas `auth.uid() = user_id`**, por lo que cada usuario solo puede leer y modificar sus propias filas. El esquema se versiona en **13 migraciones SQL** idempotentes (enums, índices, triggers `updated_at`, grants por rol y backfills).

- **Sistema de Tareas Inteligente:** Dashboard Kanban multi‑tablero con **columnas configurables por tablero** (`board_columns`), arrastre de tarjetas, estados (`pendiente / en‑curso / revisión / hecho`), prioridades, puntos, fecha límite y contadores derivados; incluye un **pipeline anual** por cuatrimestres (objetivos + tareas) con editor dedicado.

- **Hábitos y Analítica:** Rastreador de hábitos con **unidad de medición**, objetivo configurable (número / horario / minutos de focus) y entradas por fecha con *upsert* idempotente; la vista de **Análisis** agrega sesiones de foco y cumplimiento de hábitos por día.

- **Agenda y Contexto Diario:** Bloques de agenda recurrente multi‑día (`weekdays`), eventos de calendario, y una **hero de briefing** que resume el día a partir de agenda, urgentes y hábitos.

- **Pomodoro con persistencia externa:** Temporizador de ciclos *focus / short‑break / long‑break* con sesiones almacenadas en Supabase, y un **historial exportable vía Route Handler** (`/api/pomodoros`) que persiste los logs en un **Gist privado de GitHub** (`data.json`) con **caché local** en disco como *fallback* best‑effort.

- **Entorno de desarrollo local autocontenido:** Stack **PGlite** (Postgres WASM) + **Hono** (`@hono/node-server`) que replica la API con las mismas políticas RLS, más **Argon2** y **JOSE** para el hashing de contraseñas y la emisión/verificación de **JWT** de sesión, con scripts `db:migrate`, `db:seed`, `db:reset` y `db:start`.

- **Interfaz de Usuario:** Sistema de diseño propio sobre **Tailwind CSS v4** con tokens semánticos (`bg-background`, `bg-surface`, `text-primary`), estética *neumórfica*, layout *dock* y componentes modulares (shell, sidebar, editores de hábito/pipeline/agenda), optimizado para tema oscuro.

---

### 📫 Contacto

* **LinkedIn:** 
* **Email:**

---
