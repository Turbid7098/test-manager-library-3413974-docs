# P2 — Verificacion de infraestructura, migracion y administrador

**Estado:** parcialmente verificado. Infraestructura en Compose, health, esquema PostgreSQL y login HTTP del administrador pasaron; P2 no se marca completa porque no se probó un arranque desde un volumen vacío.

## Cambios aplicados

- Compose exige `POSTGRES_PASSWORD`, `APP_KEY` y `ADMIN_PASSWORD` desde `infra/.env`; no quedan credenciales predeterminadas versionadas.
- `infra/.env` se ignora en Git; `infra/.env.example` contiene solo marcadores que deben reemplazarse localmente.
- La imagen de API instala `pdo_pgsql` durante el build y Composer se ejecuta si falta `vendor/autoload.php`, en lugar de instalar paquetes del sistema al iniciar cada contenedor.
- Vite usa `npm ci` cuando no existe `node_modules`.
- El comando `app:create-admin` devuelve error si faltan variables o si el username ya pertenece a un bibliotecario. Si existe el admin, sincroniza el hash solo si cambió la clave de entorno; nunca imprime la clave o el hash.
- El servicio API ejecuta `php artisan serve --no-reload`: Laravel filtra variables de entorno al iniciar su servidor de desarrollo si encuentra `.env`; sin esta opción, el proceso HTTP puede caer en SQLite aunque Compose configure PostgreSQL.
- El README de `infra` documenta el procedimiento PowerShell.

## Evidencia ejecutada

- `docker compose config --quiet`: correcto.
- `docker compose up --build -d`: correcto.
- PostgreSQL 16 queda healthy; `pg_isready`: acepta conexiones.
- `GET http://localhost:8000/api/v1/health`: `{"status":"ok"}`.
- `POST http://localhost:8000/api/v1/auth/login`: autenticación del administrador contra `library.user` en PostgreSQL; se verificó la forma Bearer/JWT, rol `admin` y expiración de ocho horas sin registrar ni mostrar el token.
- `GET http://localhost:5173`: HTTP 200.
- `php artisan migrate --force`: sin migraciones pendientes.
- `php artisan migrate:status`: migración `2026_10_10_000000_create_library_schema` aplicada.
- Consulta de esquema: exactamente seis tablas de negocio; cinco categorías oficiales; cero columnas con `DEFAULT`; FK, índices únicos y `ck_book_available_range` presentes.
- `app:create-admin` ejecutado repetidamente sin duplicar filas; comprobación local del hash contra `ADMIN_PASSWORD`: `ADMIN_AUTH_OK`.
- Confirmado que `infra/.env` está ignorado por Git y que `infra` no contiene `CREATE TABLE` ni `ALTER TABLE`.
- PostgreSQL ya tenía un volumen persistente al iniciar esta validación. No se borró; por ello, la condición de arranque desde un motor vacío queda pendiente.

## Pendiente antes de cerrar P2

Se requiere validar el arranque de una base limpia. El volumen persistente actual no se eliminó ni se reinicializó; no usar `docker compose down -v` como sustituto de una prueba controlada.
