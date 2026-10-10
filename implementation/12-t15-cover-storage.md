# T-15 — Adaptador local de portadas

**Estado:** verificado en integración con PostgreSQL y el disco local de Laravel. La carga/reemplazo/borrado del caso de uso pasa por el repositorio real de libros y el adaptador de archivos; los endpoints HTTP quedan para T-16.

## Implementación

- `FilesystemCoverStorage` usa un disco privado `covers`; la clave persistida es un UUID opaco, no una ruta ni bytes.
- Valida tamaño máximo de 2 MiB, detecta MIME real con `fileinfo` y comprueba que el contenido se pueda identificar como PNG/JPEG. Rechaza contenido falso y discrepancia entre MIME detectado y declarado.
- `read` devuelve bytes y tipo detectado; una clave válida inexistente produce `null`. Las claves con formato no permitido y los errores de escritura/lectura/eliminación se comunican explícitamente.
- El binding del puerto `CoverStorage` usa el adaptador local; reemplazar portada almacena el nuevo objeto, actualiza `cover_key` y elimina la clave anterior. La eliminación de portada deja `cover_key = null`.

## Evidencia

- Se guardó y recuperó un PNG válido de exactamente **2 MiB**, con bytes idénticos y MIME `image/png`.
- Una portada de **3 MiB** produjo `ValidationException` (`VALIDATION_ERROR`), que la capa HTTP normaliza a **422**; se rechazaron bytes no-imagen y MIME declarado `image/jpeg` para bytes PNG.
- Reemplazo: clave UUID nueva, contenido disponible y anterior eliminado. Eliminación: archivo retirado y `cover_key` volvió a `null`; consultar la portada después devolvió not-found.
- Suite completa `php artisan test`: **31 pruebas, 322 aserciones, todas aprobadas**. Incluye la prueba de concurrencia PostgreSQL de T-13.
- Laravel Pint, health API y estado de migraciones: aprobados. No se requieren cambios de esquema ni dependencias.

## Siguiente

T-16: conectar estos casos de uso a las rutas HTTP autenticadas del contrato, incluida la carga multipart y las respuestas binarias de portada.
