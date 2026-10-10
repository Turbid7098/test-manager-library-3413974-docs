# T-19 — Contratos automatizados de arquitectura

**Estado:** verificado en la implementación Laravel/PHP con un contrato PHPUnit ejecutable.

## Adaptación al stack

El spec original propone `import-linter`, herramienta de Python. La API real está implementada en PHP/Laravel, por lo que el contrato equivalente vive en `tests/Architecture/ArchitectureRulesTest.php`, usa el tokenizer incluido en PHP y se ejecuta con PHPUnit; no se añade una dependencia Python ajena al proyecto.

## Reglas verificadas

- `Domain` solo importa su propio namespace y clases globales de PHP.
- `Application` solo importa `Application` y `Domain`.
- Las clases de adaptadores solo se instancian en el punto de composición `AppServiceProvider`.
- Un fixture con un import prohibido se detecta como violación; el test confirma que el contrato también falla ante esa clase de cambio.
- PHPUnit incluye `Architecture` como suite independiente y Composer ofrece `composer test:architecture`.

## Evidencia

- Suite de arquitectura: **3 pruebas, 173 aserciones**, aprobadas con `composer run test:architecture`.
- Suite completa de API: **40 pruebas, 609 aserciones**, aprobadas.
- `composer validate --no-check-publish` y Laravel Pint aprobados.

## Siguiente

T-20: comprobar el arranque completo con almacenamiento PostgreSQL aislado, preservando el volumen de desarrollo existente.
