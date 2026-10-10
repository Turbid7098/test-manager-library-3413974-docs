# T-13 — Persistencia de préstamos y concurrencia optimista

**Estado:** verificado contra PostgreSQL 16 real de Compose. La prueba de competencia usa dos procesos Laravel independientes y transacciones simultáneas sobre el último ejemplar.

## Implementación

- Migración aditiva `2026_10_11_000000_add_version_to_books`: agrega `book.version bigint NOT NULL`, inicializa las filas existentes en 0 y elimina el valor por defecto para escrituras posteriores.
- `Book` conserva el testigo leído; `DatabaseBookRepository` actualiza con `WHERE id = ? AND version = ?`, incrementa la versión de forma atómica y convierte una escritura obsoleta en `409 BOOK_NOT_AVAILABLE`.
- `DatabaseLoanRepository` persiste cabeceras y líneas en una transacción, reconstruye el agregado, registra devoluciones una sola vez, cuenta líneas activas y consulta vencidos con días de retraso calculados.
- `DatabaseTransactionManager` delega en transacciones Laravel; `LaravelIdGenerator` genera UUID y quedan enlazados los puertos de préstamo, transacción e identificador.
- La columna `version` es técnica y no sustituye las restricciones existentes. `ck_book_available_range` permanece como última barrera.

## Evidencia

- Dos procesos de aplicación leyeron la versión 0 del mismo libro antes de continuar. Resultado: exactamente un préstamo guardado y un conflicto `BOOK_NOT_AVAILABLE`; PostgreSQL quedó con `available_copies = 0`, `version = 1` y una sola línea abierta.
- Pruebas PostgreSQL adicionales: lectura obsoleta rechazada, checkout de dos libros revierte todas las escrituras ante conflicto, ciclo de préstamo/devolución, conteo activo, vencimiento y rechazo de devolución doble sin reponer inventario dos veces.
- Las pruebas revierten sus transacciones o eliminan fixtures por UUID; al finalizar la suite, los conteos de fixtures de prueba en PostgreSQL quedaron en cero.
- Suite completa `php artisan test`: **30 pruebas y 296 aserciones** (incluye auth, unidad, feature, adaptadores T-12 y T-13).
- Laravel Pint, estado de migraciones y health HTTP: aprobados.

## Límite de alcance

T-13 verifica el caso de uso y el adaptador con transacciones PostgreSQL realmente competidoras. La traducción HTTP final a `201`/`409` se comprobará al implementar los endpoints de negocio en T-16. T-15 (portadas) sigue antes que T-16 conforme al orden del plan.
