# Adaptación Tecnológica: Python/.NET a Laravel y React

## 1. Patrón Arquitectónico (Backend Laravel)
Dado el requisito estricto de Arquitectura Hexagonal y Domain-Driven Design (DDD):
- **Evitaremos acoplamiento con Eloquent en el Dominio**: Los modelos Eloquent de Laravel se considerarán adaptadores de persistencia de salida. El dominio (entidades) estará representado por clases puras en PHP (`src/Domain`).
- **Interfaces de Puertos**: Los puertos se definirán en `src/Application/Ports` como interfaces PHP puras.
- **Inyección de Dependencias**: El Service Container de Laravel actuará como punto de composición (`Composition Root`), registrando las implementaciones en los Service Providers (`AppServiceProvider` u otros específicos).
- **Controladores como Adaptadores**: Los Controladores de Laravel estarán aislados. Solo consumirán DTOs y llamarán a Servicios de Aplicación.

## 2. Bases de Datos (PostgreSQL 16)
- Laravel utiliza migraciones (`database/migrations`), allí definiremos las 6 tablas necesarias con los esquemas precisos en `library`, UUIDs, y sentencias `DB::statement()` para agregar `CHECK` constraints específicas que Laravel Schema Builder no soporte nativamente de la misma forma exacta, o usando las herramientas disponibles.
- No usaremos `created_at` ni `updated_at` predeterminados de Laravel, salvo los campos solicitados (`deleted_at`, `loaned_at`, `due_at`, `returned_at`).

## 3. Frontend (React)
- Usaremos Vite para agilizar la construcción de React.
- Módulos independientes por feature o pantalla (Login, Dashboard, Catálogo, Préstamos).
- Comunicación con la API usando `fetch` o `axios` pasándole el JWT en headers.
- Estado con Context API o librerías ligeras para el manejo de sesión del token.

## 4. Infraestructura
- Usaremos `docker-compose` con un contenedor para PostgreSQL, un contenedor PHP-FPM / Nginx o un servidor simplificado, y uno para la App.
