# Arquitectura y Diseño Técnico (Fase P1)

## 1. Arquitectura Hexagonal en Laravel

Para mantener la independencia del dominio y alinearse con la especificación original, la aplicación backend en Laravel implementará un patrón de Arquitectura Hexagonal estricto, organizado en los siguientes directorios dentro de `app/` o un directorio `src/` registrado en autoload:

```text
src/
  ├── Domain/               # Entidades, Value Objects, Excepciones de Dominio (Sin dependencias externas)
  ├── Application/          # Casos de Uso (Servicios) y Puertos (Interfaces)
  ├── Adapters/
  │   ├── Inbound/          # Controladores HTTP, Form Requests, Recursos (JSON)
  │   └── Outbound/         # Repositorios (Eloquent), Clock, PasswordHasher, TokenIssuer, CoverStorage
  └── Bootstrap/            # Service Providers para registrar dependencias (Composición)
```

**Punto Único de Composición:**
Se utilizará un `CompositionServiceProvider` de Laravel para enlazar las interfaces (Puertos) de `Application` con sus implementaciones (Adaptadores) en `Adapters/Outbound`. Fuera de este proveedor, no se instanciarán adaptadores de infraestructura directamente.

## 2. Modelo Entidad-Relación y Transacciones

El modelo respetará las 6 tablas principales descritas en la especificación (`category`, `book`, `member`, `user`, `loan`, `loan_item`).

**Estrategia de Transacciones:**
- Los casos de uso mutables (ej. Prestar un libro, Devolver una línea) ejecutarán todo su flujo dentro de una transacción `DB::transaction()`.
- Cualquier excepción de dominio detendrá la operación y la transacción hará un rollback automáticamente.

**Política de Concurrencia (ADR-A extendido):**
- La concurrencia optimista sobre los ejemplares se manejará mediante una consulta SQL condicionada.
  Ejemplo: `UPDATE books SET available_copies = available_copies - 1 WHERE id = ? AND available_copies = ?`
- Si la afectación de filas es 0, se aborta con 409 (Conflicto), asegurando que el último ejemplar solo se asigne una vez bajo condiciones de carrera concurrentes (RNF-2).

## 3. Matriz de Acceso por Roles

| Entidad / Acción | Público | Bibliotecario (`librarian`) | Administrador (`admin`) |
|---|---|---|---|
| Iniciar Sesión | Sí | Sí | Sí |
| Alta Bibliotecarios | No | No | Sí |
| Catálogo (Lectura) | No | Sí | Sí |
| Catálogo (Escritura/Portadas/Baja) | No | No | Sí |
| Socios (Lectura/Escritura) | No | Sí | Sí |
| Préstamos (Listar, Prestar, Devolver, Vencidos) | No | Sí | Sí |
| Reporte (Más Prestados) | No | No | Sí |

## 4. Política de Errores HTTP

Todas las respuestas de error de la API seguirán la estructura acordada en `api-contract.md`:
```json
{
  "code": "ERROR_CODE",
  "message": "Mensaje en español",
  "details": []
}
```
Se creará un manejador de excepciones personalizado en Laravel (`app/Exceptions/Handler.php` o un Middleware) que capturará las excepciones de Dominio y las traducirá al contrato JSON correcto, ocultando las trazas en producción y mapeando códigos como 401, 403, 404, 409 y 422.

## 5. Arquitectura del Frontend (React)

- **Tecnología:** React + Vite.
- **Estructura Modular:** Componentes agrupados por característica funcional (`features/auth`, `features/catalog`, `features/loans`, etc.).
- **Gestión de Estado:** `Context API` para la sesión actual y JWT.
- **Interacción API:** Llamadas con `fetch` manejando los códigos HTTP estandarizados y leyendo el formato común de errores para mostrarlos en UI.

## 6. Estrategia de Despliegue y Docker

- El repositorio `infra` proveerá un `docker-compose.yml`.
- Contendrá tres servicios principales:
  - `postgres`: Base de datos PostgreSQL 16 (vacía al arranque).
  - `api`: Contenedor basado en PHP/Nginx que alojará la aplicación Laravel.
  - `app`: Contenedor basado en Node.js que alojará el frontend React (modo desarrollo o producción usando serve).
- Las dependencias se enlazarán a través de redes de Docker, y la persistencia de datos mediante volúmenes.

## 7. Plan de Pruebas Automatizadas

1. **Unitarias (Dominio):** Prueba pura de PHPUnit sobre entidades y value objects (sin base de datos, sin dependencias de framework).
2. **Aplicación (Casos de uso con fakes):** Pruebas inyectando repositorios en memoria (arrays) y fakes de `Clock` y `Hasher`.
3. **Integración (Persistencia y HTTP):** Pruebas usando SQLite en memoria o un motor PostgreSQL de pruebas (`RefreshDatabase` trait de Laravel), llamando directamente a los Controladores para asegurar que el contrato y el enrutamiento son correctos.

## 8. Diferencias y Decisiones Específicas del Framework

- **Protección contra asignación masiva:** Evitaremos inyectar el DTO directamente al modelo Eloquent para las inserciones, para asegurar que los UUID y estados pasen rigurosamente por el Dominio primero.
- **Validación Laravel (Form Requests):** Se usarán Form Requests en la capa de Adaptadores Inbound para rechazar peticiones mal formadas antes de tocar el Dominio (422).
- **Control de UUID:** La especificación dicta que los UUID los genera la aplicación (UUID v4).
