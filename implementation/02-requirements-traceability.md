# Matriz de Trazabilidad

| ID | Requisito / Tarea | Módulo | Repositorio | Prueba | Evidencia Esperada | Estado |
|---|---|---|---|---|---|---|
| HU-01 | Iniciar sesión | Auth | `api`, `app` | Login HTTP/JWT contra PostgreSQL; interfaz React con errores, sesión temporal y logout probados | Token JWT o 401 | EN PROCESO |
| HU-02 | Alta bibliotecarios | Auth/Users | `api` | Alta HTTP `201`, hash interno no serializado y `403` para librarian verificados | Usuario creado, HTTP 201 | EN PROCESO |
| HU-03 | Gestionar libros | Catalog | `api`, `app` | API CRUD, autorización y flujo multipart/binario de portada probados; pantallas app pendientes | Libro en DB, cover subido | EN PROCESO |
| HU-04 | Buscar libros | Catalog | `api`, `app` | Caso de uso con fake; rendimiento/API pendientes | Lista filtrada, <= 300ms | EN PROCESO |
| HU-05 | Gestionar socios | Members | `api`, `app` | Crear/buscar/detalle HTTP y conteo de activos verificados; pantallas app pendientes | UUID creado, HTTP 201 | EN PROCESO |
| HU-06 | Prestar libro | Loans | `api`, `app` | Endpoint HTTP transaccional probado; dos procesos: uno presta último ejemplar, el otro 409; pantallas app pendientes | Líneas descontadas, max 3 | EN PROCESO |
| HU-07 | Devolver libro | Loans | `api`, `app` | Endpoint HTTP, reposición única y devolución duplicada `409` comprobados; pantallas app pendientes | `returned_at` seteado, +1 copia | EN PROCESO |
| HU-08 | Ver préstamos | Loans | `api`, `app` | Filtro/listado HTTP y estados dinámicos verificados; pantallas app pendientes | Estados calculados correctos | EN PROCESO |
| HU-09 | Libros vencidos | Loans | `api`, `app` | Consulta HTTP PostgreSQL, socio y días de retraso verificados; pantallas app pendientes | Lista de líneas ordenadas | EN PROCESO |
| HU-10 | Reporte prestados | Reports | `api`, `app` | Agregación PostgreSQL HTTP probada por rango y sin datos de socios; pantalla app pendiente | Lista de libros más prestados | EN PROCESO |
| T-01 | Esqueleto API | Bootstrap | `api` | GET `/api/v1/health` en Compose | `{"status":"ok"}` | VERIFICADO |
| T-02 | Migración inicial | Infra/DB | `api`, `infra` | `artisan migrate:status`, consultas PostgreSQL, seis tablas/categorías/FK/CHECK/índices/defaults | Esquema físico coincide | VERIFICADO |
| T-03 | Dominio: Value Objects | Domain | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | ISBN/Quantity/DateRange invariants | VERIFICADO |
| T-04 | Dominio: Book | Domain | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Existencias, ajuste y baja logica | VERIFICADO |
| T-05 | Dominio: Loan | Domain | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Estados, 14 dias, 1-3 lineas y sin duplicados | VERIFICADO |
| T-06 | Dominio: Member/User | Domain | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Normalizacion, hash y roles | VERIFICADO |
| T-07 | PU Auth | App | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Login, clave invalida, alta y rechazo de admin | VERIFICADO |
| T-08 | PU Books | App | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | CRUD de dominio, busqueda/paginacion y portadas | VERIFICADO |
| T-09 | PU Lend | App | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Limites, vencidos, duplicados y disponibilidad | VERIFICADO |
| T-10 | PU Return & Overdue | App | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Devolucion unica y orden de vencidos | VERIFICADO |
| T-11 | PU Report | App | `api` | `tests/Unit/LendBooksServiceTest.php` con fakes | Rango valido y valores congelados en el nucleo | VERIFICADO |
| T-12 | Repos Persistencia (1) | Adapters | `api` | `PostgresPersistenceAdaptersTest` contra PostgreSQL real; búsquedas, paginación, baja lógica y bindings | CRUD de libro/socio/usuario y categorías | VERIFICADO |
| T-13 | Repos Persistencia (2) | Adapters | `api` | Integración PostgreSQL con dos procesos concurrentes; persistencia, devolución, vencidos, versión obsoleta y rollback | Un préstamo confirmado, conflicto `BOOK_NOT_AVAILABLE`, sin ejemplares ni préstamos parciales | VERIFICADO |
| T-14 | Adaptador Seguridad | Adapters | `api` | `JwtAuthenticationTest`, `AuthLoginTest` y login HTTP con PostgreSQL | JWT HS256, hash seguro y validación Bearer | VERIFICADO |
| T-15 | Adaptador Portadas | Adapters | `api` | PostgreSQL + disco local; PNG válido de 2 MiB, 3 MiB rechazado, MIME verificado, reemplazo y borrado | Clave opaca y contenido recuperable; sin portada `cover_key` nulo | VERIFICADO |
| T-16 | Adaptador HTTP | Adapters | `api` | Pruebas de contrato HTTP con PostgreSQL, roles, rutas, códigos, portada binaria y CORS; 35 pruebas/426 aserciones en suite completa | Todas las rutas y permisos de `api-contract.md` verificados | VERIFICADO |
| T-17 | Composición | Bootstrap | `api` | Verificador reproducible en una base PostgreSQL nueva: migración desde cero, cinco categorías, alta idempotente y login/route protegida por HTTP | Admin autenticable por JWT en base inicialmente vacía | VERIFICADO |
| T-18 | DB CHECK Constraints | DB | `api` | Migración aditiva y pruebas negativas PostgreSQL (SQLSTATE `23514`) para `ck_book_total_positive` y `ck_user_role`; dominio rechaza datos inválidos | Invariantes de copias y rol aplicadas por dominio y motor | VERIFICADO |
| T-19 | Arch Tests | Testing | `api` | `tests/Architecture/ArchitectureRulesTest.php`: imports limitados a Domain/Application, adaptadores solo instanciados en composición; fixture negativo | Capas aisladas y composición única; 3 pruebas de arquitectura aprobadas | VERIFICADO |
| T-20 | Docker Infra | Infra | `infra` | Compose aislado con volumen nuevo: PostgreSQL inicialmente sin tablas, API health `ok`, frontend HTTP 200; migraciones aplicadas después | API, frontend y DB arrancan; esquema vacío hasta migrar | VERIFICADO |
| T-21 | Seed Tool | Tool | `tool`, `api` | `seed.mjs` autenticado por HTTP; evidencia real: 40 libros, 10 socios, 15 préstamos, 3 líneas vencidas y 15 filas de reporte | Datos demo creados solo por API; endpoint de vencidos los devuelve | VERIFICADO |
| T-22 | Frontend App | App | `app` | Build TypeScript/Vite; login, error HTTP, dashboard responsive y logout comprobados | Flujo frontend completo con endpoints de negocio | EN PROCESO |
| T-23 | Static Page | Page | `page` | Manual testing | Responsive | PENDIENTE |
