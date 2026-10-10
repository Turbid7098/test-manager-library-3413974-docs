# T-16 — Adaptador HTTP y contrato de API

**Estado:** rutas operativas implementadas y verificadas con requests Laravel contra PostgreSQL real. La prueba recorre operaciones, permisos y formas de respuesta; el recorrido visual de los módulos React permanece para T-22.

## Rutas y autorización

- Público: `GET /api/v1/health`, `POST /api/v1/auth/login`.
- Cualquier usuario autenticado: categorías; lectura/búsqueda de libros y portadas.
- `admin`: alta de bibliotecarios, escritura/baja de libros, escritura/borrado de portadas e informe de más prestados.
- `admin` o `librarian`: lectura/creación de socios, préstamos, devoluciones, vencidos y listados/detalle asociados.
- Los endpoints usan los casos de uso existentes, repositorios PostgreSQL y transacciones. Las respuestas serializan únicamente campos del contrato; ninguna respuesta de usuario incluye contraseña o hash.
- Errores de dominio, autorización y validación conservan un cuerpo JSON común `code`, `message`, `details`. El informe usa rangos de fecha UTC inclusivos y agrupa por valores congelados.
- La portada de lectura responde bytes con MIME validado y `X-Content-Type-Options: nosniff`. CORS acepta solo el origen `FRONTEND_ORIGIN`, configurado por entorno (valor Compose de desarrollo `http://localhost:5173`).

## Evidencia ejecutada

- `PostgresApiContractTest`: login, rol/admin, alta de bibliotecario, categorías ordenadas, creación/búsqueda/edición/baja de libro, subida multipart (PNG aceptado; 3 MiB → `422`) y descarga binaria, creación/búsqueda/detalle de socio, préstamo `201`, disponibilidad `409`, listado/detalle de préstamos, filtro de vencidos, devolución, devolución repetida `409`, reposición de ejemplar, reporte y `403` por rol.
- `ApiCorsTest`: preflight para origen permitido y rechazo de un origen ajeno.
- La suite completa `php artisan test`: **35 pruebas, 426 aserciones, todas aprobadas**, incluidas las pruebas concurrentes reales de T-13 y de portadas T-15.
- Laravel Pint: aprobado; Compose válido; migraciones aplicadas; `GET /api/v1/health` responde `{"status":"ok"}`; fixtures de PostgreSQL limpiados.

## Fuera de alcance

T-16 completa el backend HTTP, no las pantallas de negocio React ni el recorrido manual de feria (T-22). T-17 todavía debe verificarse desde la composición/arranque prescritos; T-20 requiere además un motor inicialmente vacío.
