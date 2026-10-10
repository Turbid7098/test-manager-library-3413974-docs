# T-20 — Arranque Compose desde una base inicialmente vacía

**Estado:** verificado mediante un proyecto Compose temporal e independiente del stack de desarrollo.

## Método seguro

- Se reutilizó `docker-compose.yml` sin modificarlo; se generó una copia temporal junto a `Dockerfile.api` con puertos host alternativos para no interrumpir los servicios que ya estaban activos.
- El proyecto `manager-library-t20-verification` creó su propio network y volúmenes `db-data`/`app-node-modules`. Antes de migrar, una consulta a PostgreSQL confirmó cero tablas de `library` y cero historial `public.migrations`.
- No se ejecutaron migraciones durante el arranque de Compose. Se corrieron explícitamente solo después de comprobar la base vacía.

## Evidencia

- `docker compose up -d`: PostgreSQL healthy, API y frontend arrancados.
- `GET /api/v1/health`: `{"status":"ok"}`; frontend: HTTP 200.
- Tras verificar el estado vacío, `php artisan migrate --force` aplicó las tres migraciones y `migrate:status` las mostró como ejecutadas.
- Se retiraron únicamente los contenedores, network y volúmenes etiquetados con el proyecto de verificación. El stack original siguió arriba y su health devolvió `ok`.

## Siguiente

T-21: generar los datos de demostración requeridos mediante llamadas HTTP a la API.
