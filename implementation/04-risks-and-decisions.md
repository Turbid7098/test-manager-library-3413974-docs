# Riesgos, Decisiones y Ambigüedades

## A. Concurrencia optimista y modelo de datos
**Decisión anterior (ADR-A, sustituida):** se propuso usar `available_copies` como testigo para evitar una columna adicional. Esa alternativa no implementaba el testigo de versión explícito exigido por ADR-002.

**Decisión vigente:** ADR-002 es la fuente de verdad. T-13 agrega `book.version bigint NOT NULL`, inicializa en 0 las filas existentes y elimina el valor por defecto después de la migración. Cada actualización compara e incrementa esa versión dentro de la transacción; una lectura obsoleta produce `BOOK_NOT_AVAILABLE` y HTTP 409. `ck_book_available_range` sigue siendo la última barrera de integridad.

## B. Datos Vencidos (Test Clock)
**Problema:** T-21 exige generar líneas vencidas mediante la API, pero las fechas de vencimiento están controladas por el Dominio y no pueden forzarse por parámetros HTTP.
**Decisión (ADR-B):** Implementaremos un `Clock` Port en la capa de aplicación. En producción, usará `SystemClock`. Para ambientes de pruebas (`APP_ENV=local` o equivalente), usaremos un `TestClock` que tomará la "fecha actual" proveniente de un Header HTTP específico (ej. `X-Test-Time`). La API validará que esta inyección de tiempo solo funcione en un ambiente de desarrollo o semilla, preservando la inmutabilidad de fechas en producción real.

## C. Endpoints Faltantes: Portadas
**Problema:** `api-contract.md` menciona `GET /books/{id}/cover` en descripciones pero omite el endpoint en su lista tabulada.
**Decisión (ADR-C):** Implementaremos `GET /books/{id}/cover` para devolver explícitamente el archivo binario (`image/jpeg` o `image/png`) bajo código 200, o 404 si el recurso no existe.

## Riesgos y Mitigaciones
- **Over-engineering:** Mantener los casos de uso simples e imperativos.
- **Acoplamiento Framework:** Crear la estructura de directorios pura (`src/Domain`, `src/Application`) para aislar PHP puro y luego inyectarlo en las rutas de Laravel usando Providers y dependencias resueltas.
