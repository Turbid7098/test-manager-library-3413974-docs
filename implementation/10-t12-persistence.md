# T-12 — Adaptadores de persistencia PostgreSQL

**Estado:** verificado contra PostgreSQL real de Compose. El alcance es libros, socios, usuarios y categorías; no incluye préstamos/concurrencia ni cierra la API HTTP.

## Implementación

- `DatabaseBookRepository`: lecturas y escritura, ISBN activo, búsqueda por título/autor y categoría, paginación, búsqueda por varios IDs y detección de préstamos abiertos. Las búsquedas tratan `%`, `_` y `!` como texto literal.
- `DatabaseMemberRepository`: lecturas y escritura por ID/documento, búsqueda parcial por nombre o documento y paginación.
- `DatabaseUserRepository`: adaptador existente, ahora cubierto también por integración para ID, nombre de usuario y escritura.
- `DatabaseCategoryRepository`: existencia y lectura de nombres de categoría por IDs.
- Bindings de los puertos en `AppServiceProvider`.
- `tests/Integration/PostgresPersistenceAdaptersTest.php`, registrado explícitamente como suite `Integration` en PHPUnit. Cada caso abre una transacción PostgreSQL y la revierte en `tearDown`; no crea ni deja datos de prueba persistentes.

## Evidencia

- Suite completa `php artisan test --compact`: **26 pruebas, 250 aserciones, todas aprobadas**.
- Suite de integración y bindings: **4 pruebas, 46 aserciones, todas aprobadas** contra PostgreSQL.
- Laravel Pint sobre los siete archivos PHP de los adaptadores y pruebas: aprobado.
- `php artisan migrate:status`: esquema existente aplicado; las pruebas no ejecutan migraciones destructivas.
- `git diff --check`: aprobado.

## Siguiente según el orden

T-13: implementar el repositorio/transacciones de préstamos y probar con solicitudes concurrentes que solo una pueda prestar el último ejemplar. Después siguen portadas (T-15) y el resto del adaptador HTTP (T-16), según sus dependencias.
