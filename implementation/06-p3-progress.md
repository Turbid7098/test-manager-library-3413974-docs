# Fase P3 — Dominio y casos de uso

**Estado:** VERIFICADO para el nucleo de dominio y aplicacion descrito en P3. No significa que la API completa o las historias de usuario end-to-end esten terminadas.

## Alcance ejecutado

En el repositorio `api` se completaron y probaron:

- Objetos de valor `Isbn`, `Quantity`, `DocumentNumber` y `DateRange`.
- Entidades `Book`, `Member`, `User`, `Loan` y `LoanItem`, con existencias acotadas, baja logica, limite de 1 a 3 lineas, libros sin duplicar, vencimiento a 14 dias, valores congelados, devolucion individual y estado calculado.
- Puertos de repositorio, transaccion, reloj, hash de contrasenas, tokens, identificadores, categorias, almacenamiento de portadas y lectura del reporte.
- Casos de uso con dependencias inyectadas para login, alta de bibliotecarios, libros, socios, prestamos, devoluciones, vencidos, portadas y consulta del reporte.
- Pruebas unitarias con dobles en memoria. El dominio y la aplicacion no importan Laravel ni Eloquent.

## Evidencia reproducible

Desde la raiz del repositorio `api`, con Docker y `vendor/` disponible:

```powershell
$api = (Resolve-Path .).Path
docker run --rm --mount "type=bind,source=$api,target=/app" --workdir /app php:8.3-cli php vendor/bin/phpunit tests/Unit/LendBooksServiceTest.php
```

Resultado observado: **13 pruebas, 166 aserciones, todas aprobadas**. Tambien se ejecuto el conjunto completo de PHPUnit: **15 pruebas, 168 aserciones, todas aprobadas**, pasando una `APP_KEY` efimera al contenedor de pruebas. No se guardo esa clave en archivos.

Comprobaciones adicionales:

- `php -l` sobre todo `src/` y `tests/Unit/`: sin errores de sintaxis.
- Laravel Pint sobre los archivos P3 modificados: 39 archivos aprobados.

## Limites explicitos

- No se implementaron ni probaron adaptadores PostgreSQL/Eloquent, rutas HTTP, autenticacion JWT real, composicion Laravel ni Docker Compose integrado.
- Las pruebas con fakes no verifican atomicidad/concurrencia del motor. RNF-2 y T-13 siguen pendientes.
- La validacion del contenido binario de una portada corresponde al adaptador `CoverStorage`; su contrato exige deteccion por contenido, y debe probarse en P4.
- Las historias HU-01 a HU-10 siguen `EN PROCESO` hasta probar sus contratos completos en la API y las capas que correspondan.

## Estado observado en los seis repositorios

Se confirmaron los seis clones locales y que sus `origin` apuntan a los forks de `Turbid7098`, no al repositorio del instructor.

| Repositorio | Estado observado al continuar |
|---|---|
| `api` | Laravel instalado; migracion inicial y health verificados en PostgreSQL/Compose. Aprovisionamiento del admin probado; login HTTP y rutas de negocio aun pendientes. |
| `app` | Plantilla Vite/TypeScript; aun no es la aplicacion React requerida. |
| `docs` | Especificaciones Python/.NET originales y documentos de adaptacion; se actualizo la trazabilidad P3. |
| `infra` | Compose para PostgreSQL 16, API y Vite verificado. Secretos movidos a `.env` local ignorado; falta probar un arranque con volumen PostgreSQL limpio. |
| `page` | Solo README; falta el sitio estatico. |
| `tool` | Solo README; falta el sembrador por API. |

No se modificaron `app`, `infra`, `page` ni `tool`, pues esos cambios no pertenecen a P3. La auditoria de archivos existente en P0 contiene afirmaciones que deben revisarse contra este inventario; no se alteraron aqui las especificaciones originales.

## Siguiente compuerta

P2 esta verificada para Compose, health, esquema y hash del administrador, pero no se debe dar por cerrada: el endpoint HTTP de login todavia no existe y falta verificar un arranque desde un volumen limpio. T-17 permanece `EN PROCESO` y T-20 permanece `EN PROCESO`.

El login HTTP forma parte de P4; por tanto, continuar con el primer bloque de P4 (adaptador de autenticacion y composicion) es necesario para validar la ultima evidencia de P2. Mantener las demas rutas P4 y la concurrencia como pendientes hasta probarlas.
