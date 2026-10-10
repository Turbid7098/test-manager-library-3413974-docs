# P4 — Autenticación HTTP y JWT

**Estado:** primer bloque de P4 verificado. La evidencia cubre login, emisión/verificación JWT, middleware y composición Laravel; no representa el cierre de todas las rutas de negocio ni de P4 completa.

## Alcance implementado

- `POST /api/v1/auth/login` valida usuario y contraseña y devuelve `access_token`, `token_type`, `expires_in` y `role`.
- Las credenciales inválidas producen el mismo error `401 INVALID_CREDENTIALS`, sin revelar si existe el usuario.
- El repositorio de usuario consulta `library.user` en PostgreSQL y el adaptador de hash usa Laravel `Hash`.
- JWT firmado con HS256, duración de ocho horas y claims `sub`, `role`, `iat` y `exp`; no incluye datos personales.
- La verificación comprueba firma, algoritmo, estructura, vigencia, rol y tamaño máximo del token.
- El middleware `jwt` valida el Bearer token y que el usuario/rol sigan vigentes. Devuelve errores JSON normalizados `401` y `403`; admite restricción por rol, por ejemplo `jwt:admin`.
- La contraseña del caso de uso se marca sensible para evitar que PHP la incluya en trazas de excepciones.

## Evidencia ejecutada

- Suite completa al cierre de autenticación: **23 pruebas, 208 aserciones, todas aprobadas**. Incluye `JwtAuthenticationTest`, `AuthLoginTest`, `ServiceBindingsTest` y `LendBooksServiceTest`. Tras T-12, el total es 26 pruebas y 250 aserciones; el nuevo bloque se documenta en [`10-t12-persistence.md`](./10-t12-persistence.md).
- Laravel Pint sobre **17 archivos PHP relacionados**: aprobado.
- `docker compose config --quiet`: aprobado.
- `php artisan migrate:status`: migración `2026_10_10_000000_create_library_schema` aplicada en PostgreSQL.
- Login HTTP con el administrador configurado localmente: respuesta Bearer válida, rol `admin` y expiración de 28 800 segundos. No se guardó ni mostró el token.
- `GET /api/v1/health`: `{"status":"ok"}`.
- `.env` de API e infraestructura están ignorados por Git.

## Corrección de configuración de Compose

Laravel `artisan serve` filtra variables de entorno que no estén en su lista permitida si detecta un archivo `.env`. El archivo local de API contiene `JWT_SECRET`, por lo que el proceso HTTP no heredaba `DB_CONNECTION` y usaba SQLite aunque el contenedor tuviera PostgreSQL configurado. El servicio ahora inicia con `php artisan serve --no-reload`, lo que conserva las variables del entorno de Compose; el login HTTP posterior se verificó contra PostgreSQL.

## Pendiente

- La interfaz React ya consume el login y muestra un resumen inicial; los módulos de negocio siguen pendientes.
- Las rutas HTTP de negocio siguen pendientes; todavía no hay endpoints protegidos para validar autorización en un flujo HTTP de negocio. El middleware sí está cubierto unitariamente.
- P2 permanece parcial hasta probar la infraestructura desde un volumen PostgreSQL vacío. No se eliminó el volumen persistente durante esta validación.
- Los adaptadores de persistencia de T-12 y T-13 y el adaptador de portadas T-15 se verificaron posteriormente; evidencias en [`11-t13-loans-concurrency.md`](./11-t13-loans-concurrency.md) y [`12-t15-cover-storage.md`](./12-t15-cover-storage.md). Continuar con los endpoints de negocio de T-16.
