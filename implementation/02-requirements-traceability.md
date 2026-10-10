# Matriz de Trazabilidad

| ID | Requisito / Tarea | Módulo | Repositorio | Prueba | Evidencia Esperada | Estado |
|---|---|---|---|---|---|---|
| HU-01 | Iniciar sesión | Auth | `api`, `app` | Caso de uso con fake; HTTP/JWT pendientes | Token JWT o 401 | EN PROCESO |
| HU-02 | Alta bibliotecarios | Auth/Users | `api` | Caso de uso con fake; permisos HTTP pendientes | Usuario creado, HTTP 201 | EN PROCESO |
| HU-03 | Gestionar libros | Catalog | `api`, `app` | Casos de uso con fake; persistencia HTTP pendiente | Libro en DB, cover subido | EN PROCESO |
| HU-04 | Buscar libros | Catalog | `api`, `app` | Caso de uso con fake; rendimiento/API pendientes | Lista filtrada, <= 300ms | EN PROCESO |
| HU-05 | Gestionar socios | Members | `api`, `app` | Casos de uso con fake; persistencia HTTP pendiente | UUID creado, HTTP 201 | EN PROCESO |
| HU-06 | Prestar libro | Loans | `api`, `app` | Caso de uso con fake; transaccion PostgreSQL pendiente | Líneas descontadas, max 3 | EN PROCESO |
| HU-07 | Devolver libro | Loans | `api`, `app` | Caso de uso con fake; persistencia pendiente | `returned_at` seteado, +1 copia | EN PROCESO |
| HU-08 | Ver préstamos | Loans | `api`, `app` | Estado dinamico probado; listado HTTP pendiente | Estados calculados correctos | EN PROCESO |
| HU-09 | Libros vencidos | Loans | `api`, `app` | Caso de uso con fake; consulta PostgreSQL pendiente | Lista de líneas ordenadas | EN PROCESO |
| HU-10 | Reporte prestados | Reports | `api`, `app` | Puerto/caso de uso con fake; agregacion PostgreSQL pendiente | Lista de libros más prestados | EN PROCESO |
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
| T-12 | Repos Persistencia (1) | Adapters | `api` | Integration tests | DB CRUD success | PENDIENTE |
| T-13 | Repos Persistencia (2) | Adapters | `api` | Integration tests | Concurrency success | PENDIENTE |
| T-14 | Adaptador Seguridad | Adapters | `api` | Unit tests | JWT generation, hashing | PENDIENTE |
| T-15 | Adaptador Portadas | Adapters | `api` | Integration tests | Disk upload success | PENDIENTE |
| T-16 | Adaptador HTTP | Adapters | `api` | E2E HTTP tests | Contract compliance | PENDIENTE |
| T-17 | Composición | Bootstrap | `api` | Migración y aprovisionamiento idempotente probados; login HTTP pendiente de ruta P4 | Admin autenticable | EN PROCESO |
| T-18 | DB CHECK Constraints | DB | `api` | SQL Error test | Domain rules in DB | PENDIENTE |
| T-19 | Arch Tests | Testing | `api` | Dependency test | Architecture respected | PENDIENTE |
| T-20 | Docker Infra | Infra | `infra` | Compose/API health/DB health verificados; arranque desde volumen limpio pendiente | Containers running, motor inicialmente vacío | EN PROCESO |
| T-21 | Seed Tool | Tool | `tool` | Script execution | DB populated, 3 overdue | PENDIENTE |
| T-22 | Frontend App | App | `app` | Manual testing | Full flow | PENDIENTE |
| T-23 | Static Page | Page | `page` | Manual testing | Responsive | PENDIENTE |
