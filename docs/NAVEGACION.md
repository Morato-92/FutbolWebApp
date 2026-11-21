# Apuntes de navegación (Sprint 2)

## Los 3 modos de navegar (WebRegisterPath)
- ntNavigateBegin: menú hamburguesa (WebApp.src). Vacía la pila y empieza de nuevo (el Dashboard siempre se queda al fondo).
- ntNavigateForwardCustom: tarjetas del Dashboard. Avanza sin intercambiar información.
- ntNavigateForward: todo lo demás. Avanza intercambiando información (tWebNavigateData).

## Los 4 tipos de forward (según relación de tablas)
- from-main: misma tabla (Select -> Zoom).
- from-child: de hijo a padre (Zoom jugador -> Select equipo).
- from-parent: de padre a hijo (Zoom liga -> Select equipo, solo los de esa liga).
- undefined: sin relación (Dashboard -> Select, o Zoom jugador -> Select posicion).

## Los 4 participantes
- InvokingView: vista desde la que sales.
- InvokingObject: objeto que lanza el viaje (botón, lista o formulario con lupa).
- InvokedView: vista a la que llegas.
- InvokedObject: objeto de la vista destino que hace el volver (NavigateClose). En una Zoom es la propia Zoom (al guardar/borrar); en un Select al que vas a elegir padre, es la lista (OnRowClick).

## Los 4 eventos del ciclo
- OnGetNavigateForwardData: en el InvokingObject, antes de irse (rellena la maleta).
- OnNavigateForward: en la InvokedView, al llegar (case según NavigateType).
- OnGetNavigateBackData: en el InvokedObject, antes de volver.
- OnNavigateBack: en el InvokingObject, al volver (refresca).

## La maleta tWebNavigateData
- NavigateType: tipo (from-main / from-child / from-parent / undefined).
- eTable / iColumn: tabla y columna del invoking object.
- RowId: identificador único de la fila en toda la BD.
- bNewRecord: true si vas a crear uno nuevo.
- bReadOnly: true si la vista destino llega en solo lectura.
- bSaveBeforeNavigate: guarda sin preguntar si vuelves a navegar con cambios.
- NameValues: pares clave-valor para info propia.

## Respuestas de la tarea
1. From main (Select->Zoom): el InvokingObject es el detail button (o la lista si lanzas desde OnRowClick). Es quien registra la ruta, lanza el viaje, recibe OnGetNavigateForwardData al ir y OnNavigateBack al volver (refresca la lista). El InvokedObject coincide con la Zoom (cierra al guardar o borrar).

4. Undefined (Zoom jugador -> Select posicion): NO hay relación directa entre jugador y posicion; es muchos a muchos por la tabla puente jugadorposicion. Por eso el tipo sale undefined: navega, pero sin filtrar ni centrar nada.

---

## Estado final de las tareas del PDF

### ✅ From main → Select → Zoom (tiles)
- Dashboard con tarjetas de Ligas, Equipos, Jugadores, Entrenadores, Posiciones.
- Cada tarjeta navega con `ntNavigateForward` al Select correspondiente.
- El Select muestra la lista y el botón detalle (lupa) lleva a la Zoom.

### ✅ From child → Zoom hijo → Select padre
- **Zoom jugador → Select equipo:** desde el Zoom de un jugador, botón que lleva al Select de equipos filtrado por el equipo del jugador.
- **Zoom entrenador → Select equipo:** igual, filtrado por el equipo del entrenador.
- **Zoom equipo → Select liga:** desde el Zoom de un equipo, botón que lleva al Select de ligas filtrado por la liga del equipo.

### ✅ From parent → Zoom liga (1º: botón)
- En `Zoomliga.wo`, botón **"Ver equipos de esta liga"** que navega a `Selectequipo` filtrado por la liga actual.
- Usa `WebRegisterPath ntNavigateForward oSelectequipo` + `Send NavigatePath` en el `OnClick`.
- El Select de equipos recibe el contexto `nfFromParent` y aplica la restricción automáticamente (gracias a `Set DDO_Server to oliga_DD` en `oequipo_DD`).

### ⏳ From parent → Zoom liga (2º: lista embebida) — PENDIENTE
- El PDF pide: *"En vez del botón, en Zoom liga, los equipos de esa liga"* (lista anidada master-detail dentro de la Zoom).
- **No implementado.** Pendiente de la explicación del profesor sobre NamedValues / sintaxis específica de DataFlex 2025 Web.
- **Verificado antes de rendirse:**
  - Datos de la tabla `equipo` correctos (6 equipos: LigaID=1 para LaLiga, LigaID=2 para Premier League).
  - Relación `equipo.LigaID → liga.ID` existe en el Table Editor.
  - Índice `uqEquipoNombreLiga` (LigaID + Nombre, tipo Server) existe.
- **Intentos fallidos (todos):**
  1. DDO hijo `oequipo_DD` con `Set DDO_Server to oliga_DD` + lista data-aware con `Entry_Item equipo.*` → lista vacía.
  2. Añadiendo `pbAutoFillFromFirst`, `peDbGridType gtAllData`, `OnLoad` + `GridRefresh` → lista vacía.
  3. Lista no data-aware con `OnManualLoadData` → errores 4345 (`Find First`, `Find GT`, `For Each` no reconocidos en DF2025 Web).
  4. `cWebChildView` → error 4345 (no existe en DF2025 Web).
  5. Modificar `cequipoDataDictionary.dd` con `Relation`/`Constrain` → error 4298 (comando no encontrado).
  6. `Set Main_DD to oequipo_DD` (como en `Selectequipo.wo`) + `Set Server to oliga_DD` en los formularios de liga → lista vacía.
  7. `Send Find of oequipo_DD FIRST_RECORD` en `OnNavigateForward` → error 57 "Falta argumento".
- **Solución temporal:** botón "Ver equipos de esta liga" (punto 1º ✅) que demuestra el concepto from-parent de forma funcional.

### ✅ Undefined → Zoom jugador → Select posición
- En `Zoomjugador.wo`, botón que navega a `Selectposicion`.
- Tipo de navegación: `undefined` (no hay relación directa entre `jugador` y `posicion`; es muchos-a-muchos vía `jugadorposicion`).
- Navega correctamente pero **sin filtrar ni centrar** (muestra todas las posiciones).

---

## Resumen del sprint

| Tarea | Estado |
|---|---|
| From main → Select → Zoom | ✅ Funciona |
| From child → Zoom hijo → Select padre | ✅ Funciona (jugador, entrenador, equipo) |
| From parent → Zoom liga (botón) | ✅ Funciona |
| From parent → Zoom liga (lista embebida) | ⏳ Pendiente de NamedValues (lunes) |
| Undefined → Zoom jugador → Select posición | ✅ Funciona |

**4 de 5 puntos funcionando, 1 documentado como pendiente.**