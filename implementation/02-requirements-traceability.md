# Matriz de Trazabilidad

| ID | Requisito / Tarea | Módulo | Repositorio | Prueba | Evidencia Esperada | Estado |
|---|---|---|---|---|---|---|
| HU-01 | Iniciar sesión | Auth | `api`, `app` | Test login fake/real | Token JWT o 401 | PENDIENTE |
| HU-02 | Alta bibliotecarios | Auth/Users | `api` | Test admin rights | Usuario creado, HTTP 201 | PENDIENTE |
| HU-03 | Gestionar libros | Catalog | `api`, `app` | Test CRUD / portadas | Libro en DB, cover subido | PENDIENTE |
| HU-04 | Buscar libros | Catalog | `api`, `app` | Test paginación/filtro | Lista filtrada, <= 300ms | PENDIENTE |
| HU-05 | Gestionar socios | Members | `api`, `app` | Test creación | UUID creado, HTTP 201 | PENDIENTE |
| HU-06 | Prestar libro | Loans | `api`, `app` | Test reglas negocio | Líneas descontadas, max 3 | PENDIENTE |
| HU-07 | Devolver libro | Loans | `api`, `app` | Test devolución | `returned_at` seteado, +1 copia | PENDIENTE |
| HU-08 | Ver préstamos | Loans | `api`, `app` | Test calculo estados | Estados calculados correctos | PENDIENTE |
| HU-09 | Libros vencidos | Loans | `api`, `app` | Test vencidos | Lista de líneas ordenadas | PENDIENTE |
| HU-10 | Reporte prestados | Reports | `api`, `app` | Test congelados | Lista de libros más prestados | PENDIENTE |
| T-01 | Esqueleto API | Bootstrap | `api` | GET `/health` | `{"status":"ok"}` | PENDIENTE |
| T-02 | Migración inicial | Infra/DB | `api`, `infra` | Schema inspect | Esquema físico coincide | PENDIENTE |
| T-03 | Dominio: Value Objects | Domain | `api` | Unit tests | ISBN/Quantity invariants | PENDIENTE |
| T-04 | Dominio: Book | Domain | `api` | Unit tests | Book invariants | PENDIENTE |
| T-05 | Dominio: Loan | Domain | `api` | Unit tests | Loan invariants (14 days, max 3) | PENDIENTE |
| T-06 | Dominio: Member/User | Domain | `api` | Unit tests | Normalization, roles | PENDIENTE |
| T-07 | PU Auth | App | `api` | App tests (fakes) | Auth success | PENDIENTE |
| T-08 | PU Books | App | `api` | App tests (fakes) | Books logic success | PENDIENTE |
| T-09 | PU Lend | App | `api` | App tests (fakes) | Lend process success | PENDIENTE |
| T-10 | PU Return & Overdue | App | `api` | App tests (fakes) | Return process success | PENDIENTE |
| T-11 | PU Report | App | `api` | App tests (fakes) | Report logic success | PENDIENTE |
| T-12 | Repos Persistencia (1) | Adapters | `api` | Integration tests | DB CRUD success | PENDIENTE |
| T-13 | Repos Persistencia (2) | Adapters | `api` | Integration tests | Concurrency success | PENDIENTE |
| T-14 | Adaptador Seguridad | Adapters | `api` | Unit tests | JWT generation, hashing | PENDIENTE |
| T-15 | Adaptador Portadas | Adapters | `api` | Integration tests | Disk upload success | PENDIENTE |
| T-16 | Adaptador HTTP | Adapters | `api` | E2E HTTP tests | Contract compliance | PENDIENTE |
| T-17 | Composición | Bootstrap | `api` | Manual test | Admin created | PENDIENTE |
| T-18 | DB CHECK Constraints | DB | `api` | SQL Error test | Domain rules in DB | PENDIENTE |
| T-19 | Arch Tests | Testing | `api` | Dependency test | Architecture respected | PENDIENTE |
| T-20 | Docker Infra | Infra | `infra` | `docker compose up` | Containers running | PENDIENTE |
| T-21 | Seed Tool | Tool | `tool` | Script execution | DB populated, 3 overdue | PENDIENTE |
| T-22 | Frontend App | App | `app` | Manual testing | Full flow | PENDIENTE |
| T-23 | Static Page | Page | `page` | Manual testing | Responsive | PENDIENTE |
