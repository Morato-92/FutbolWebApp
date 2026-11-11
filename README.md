# FutbolWebApp — DataFlex 2025 (Web Mobile) · DAM / DataControl

Proyecto del Sprint 1: aplicación web de gestión de fútbol sobre MySQL 8.0,
desarrollado con DataFlex 2025 y metodología GitFlow (rama de integración:
`develop`; rama de entrega/release: `main` = v1.0).

> ⚠️ **LEE ANTES DE EJECUTAR.** Al clonar el repo la app **NO arranca tal cual**:
> las credenciales (`Data/DFConnId.ini`) están **excluidas del versionado por
> seguridad** (`.gitignore`) y la base de datos **`futbol` vive en un MySQL local**,
> no en el repo. **No es un fallo del proyecto**, es la puesta a punto estándar.
> Sigue los 4 pasos de la sección **"Puesta a punto"** más abajo.

## Requisitos

- DataFlex 2025 (Web Mobile Project) + Visual Studio.
- MySQL 8.0 con ODBC Connector 64-bit.
- Usuario/servidor MySQL accesibles (ver paso 3).

## Puesta a punto (la app NO conecta sin esto)

1. **Base de datos**: importa el script `futbol.sql` (raíz del repo) en MySQL:
   `mysql -u root -p < futbol.sql`  (o vía Workbench → Data Import).
   Crea la BD `futbol` con las tablas: liga, equipo, entrenador, jugador,
   posicion, jugadorposicion (y las embebidas migradas WebApp*).
2. **DSN ODBC 64-bit**: crea un DSN de sistema llamado `FutbolMySQL`
   apuntando a la BD `futbol` (Control Panel → Administrador de Orígenes de datos ODBC → 64-bit).
3. **Credenciales de conexión** (NO versionadas por seguridad):
   crea el archivo `Data/DFConnId.ini` con tus credenciales de MySQL
   (formato DataFlex: usuario/contraseña/DSN). Este archivo está en `.gitignore`
   a propósito; cada entorno genera el suyo.
4. **Abrir y ejecutar**: abre el workspace en DataFlex, Build (Ctrl+Shift+B)
   y Run (F5). Login por defecto de la plantilla: `admin` / `admin`.

## GitFlow

**Ramas:** `main` = **v1.0 entregable** (release del Sprint 1, con tag `v1.0`);
`develop` = rama de integración (historial de los PRs feature). Cada hito del
sprint se desarrolló en una rama `feature/*` y se integró a `develop` mediante
Pull Request con *merge commit* (historial auditable en la pestaña Pull requests
→ Closed): configuracion-inicial (#1), vista-posicion, vista-liga, vista-equipo,
vista-entrenador, vista-jugador, dashboard-tiles (#7).

## Estructura del Sprint 1 (puntos 1-7 del enunciado)

1-5. BD MySQL + workspace + conexión ODBC + embebidas migradas (DFLastID native)
     + tablas importadas a DataFlex.  → PR #1
6.   Vistas Select + Zoom por entidad (posicion, liga, equipo, entrenador, jugador).
7.   Tiles del Dashboard con nombres y conteos (Posiciones 6, Ligas 2, Equipos 6,
     Entrenadores 7, Jugadores 60) + Welcome informativo.  → PR #7