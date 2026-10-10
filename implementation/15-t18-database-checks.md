# T-18 — Restricciones CHECK de dominio en PostgreSQL

**Estado:** verificado en PostgreSQL 16 con una migración aditiva; no se recreó el esquema ni se eliminó el volumen.

## Implementación

- La migración `2026_10_12_000000_add_domain_check_constraints` agrega `ck_book_total_positive` (`total_copies >= 1`) y `ck_user_role` (`role IN ('admin', 'librarian')`).
- Las filas existentes respetan ambas invariantes. La migración preserva tablas y datos; el `down` elimina únicamente las dos restricciones añadidas.
- `PostgresDomainConstraintsTest` también comprueba el rechazo en el dominio y que PostgreSQL falle las inserciones inválidas con SQLSTATE `23514`, nombrando la restricción esperada.

## Evidencia

- Migración aplicada al PostgreSQL persistente: ambas migraciones posteriores a la inicial aparecen en `migrate:status`.
- Filas inválidas (`total_copies = 0`, `role = superuser`) rechazadas por las restricciones concretas; administradores y bibliotecarios válidos pasan.
- Las inserciones de prueba se ejecutan en transacciones anidadas con savepoint y la transacción del test se revierte; no quedan fixtures.
- Suite completa: **37 pruebas y 436 aserciones**, todas aprobadas; Laravel Pint y `git diff --check` aprobados.

## Siguiente

T-19: automatizar límites de importación y dependencias, incluido un test que falla si se viola la arquitectura en código fuente.
