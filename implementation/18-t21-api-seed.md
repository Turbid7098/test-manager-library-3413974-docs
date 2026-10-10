# T-21 — Sembrador de demostración mediante la API

**Estado:** verificado con llamadas HTTP reales contra la API Laravel y PostgreSQL.

## Implementación

- `tool/seed.mjs` usa `fetch` nativo de Node 22; no añade dependencias.
- Requiere credenciales admin del entorno local y crea todos los registros por los endpoints protegidos.
- El lote contiene 40 libros, 10 socios y 15 préstamos. Tres préstamos usan `X-Test-Time` histórico en su creación; 12 usan el reloj real y siguen abiertos.
- El adaptador `LaravelClock` solo permite ese encabezado en `local`/`testing`, con usuario autenticado y formato ISO 8601 válido. JWT siempre usa el reloj real.
- `SEED_RUN_ID` permite reanudar un lote parcial; los recursos del mismo lote se recuperan antes de crear otros y nunca se borran datos.

## Evidencia

Ejecución real completada con `run_id` `13039740`:

- 40 libros listos.
- 10 socios listos.
- 15 préstamos listos.
- El endpoint `GET /loans/overdue` devolvió exactamente **3** líneas de este lote.
- El reporte de más prestados devolvió **15 filas**.
- El servidor verificó `loaned_at` histórico y vencimiento a 14 días. Suite API tras el cambio del clock: **43 pruebas, 613 aserciones**, todas aprobadas; sintaxis Node 22 aprobada.

## Compuerta backend

T-12–T-21 están verificadas. Los flujos backend de libros, socios, préstamos, devoluciones, vencidos y reporte tienen contrato HTTP; procede completar su interfaz React (T-22).
