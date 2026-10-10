# Riesgos, Decisiones y Ambigüedades

## A. Concurrencia Optimista vs Modelo de Datos
**Problema:** ADR-002 especifica control de concurrencia optimista y un "testigo de versión", pero el esquema físico (`data-model.md`) prohíbe añadir columnas nuevas y declara no poseer columnas de auditoría (`updated_at` / `version`).
**Decisión (ADR-A):** El "testigo de versión" será el campo de negocio `available_copies` mismo u operado de la siguiente manera: La consulta de actualización en base de datos descontará un ejemplar asumiendo que las copias disponibles no han cambiado (ej. `UPDATE book SET available_copies = available_copies - 1 WHERE id = X AND available_copies = Y`). Si las filas afectadas son 0, significa que la versión (el valor) cambió simultáneamente, disparando el código HTTP 409. Esto satisface el contrato sin alterar el esquema físico.

## B. Datos Vencidos (Test Clock)
**Problema:** T-21 exige generar líneas vencidas mediante la API, pero las fechas de vencimiento están controladas por el Dominio y no pueden forzarse por parámetros HTTP.
**Decisión (ADR-B):** Implementaremos un `Clock` Port en la capa de aplicación. En producción, usará `SystemClock`. Para ambientes de pruebas (`APP_ENV=local` o equivalente), usaremos un `TestClock` que tomará la "fecha actual" proveniente de un Header HTTP específico (ej. `X-Test-Time`). La API validará que esta inyección de tiempo solo funcione en un ambiente de desarrollo o semilla, preservando la inmutabilidad de fechas en producción real.

## C. Endpoints Faltantes: Portadas
**Problema:** `api-contract.md` menciona `GET /books/{id}/cover` en descripciones pero omite el endpoint en su lista tabulada.
**Decisión (ADR-C):** Implementaremos `GET /books/{id}/cover` para devolver explícitamente el archivo binario (`image/jpeg` o `image/png`) bajo código 200, o 404 si el recurso no existe.

## Riesgos y Mitigaciones
- **Over-engineering:** Mantener los casos de uso simples e imperativos.
- **Acoplamiento Framework:** Crear la estructura de directorios pura (`src/Domain`, `src/Application`) para aislar PHP puro y luego inyectarlo en las rutas de Laravel usando Providers y dependencias resueltas.
