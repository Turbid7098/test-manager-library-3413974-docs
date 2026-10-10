# T-17 — Composición y arranque desde PostgreSQL vacío

**Estado:** verificado en una base PostgreSQL nueva y aislada. La base y el volumen persistente de desarrollo se conservaron: la comprobación creó una base de verificación con nombre único en el servidor PostgreSQL existente y la eliminó al terminar.

## Procedimiento reproducible

En el contenedor API, ejecuta `tests/Integration/Fixtures/verify_empty_database_bootstrap.php` pasando `DB_DATABASE`, `ADMIN_USERNAME` y `ADMIN_PASSWORD` de una base recién creada. El script se niega a correr si ya existen tablas de la aplicación o historial de migraciones. No utiliza `migrate:fresh`, no borra tablas y no muestra la contraseña, el hash ni el JWT.

## Evidencia

- La base nueva empezó sin esquema/tablas de la aplicación ni historial de migraciones.
- `php artisan migrate --force` construyó el esquema; las cinco categorías se sembraron y el adaptador registró todas las migraciones.
- `app:create-admin` creó el administrador desde el entorno; ejecutarlo de nuevo no duplicó usuarios. La contraseña configurada se validó contra el hash persistido.
- Se inició un proceso HTTP Laravel aislado (`artisan serve --no-reload`) apuntando a esa base. Health respondió `200`; login respondió `200` con JWT Bearer, rol `admin` y expiración de 28 800 segundos; ese JWT accedió a `GET /api/v1/categories`, que devolvió las cinco categorías.
- El servidor temporal se detuvo y solo la base de verificación, identificada por su nombre aleatorio, se eliminó. El volumen de PostgreSQL y los datos persistentes quedaron intactos.
- T-17 no cierra T-20: el requisito de `docker compose up` con un volumen físico PostgreSQL inicialmente vacío todavía necesita su prueba controlada.

## Siguiente

T-18: agregar y probar en PostgreSQL las restricciones `ck_book_total_positive` y `ck_user_role`, conservando las restricciones y los datos existentes.
