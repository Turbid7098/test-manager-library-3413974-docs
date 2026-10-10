# T-22 — Flujos de negocio del frontend

**Estado:** verificado para los flujos de T-22.

## Implementación

El repositorio `app` conecta React + TypeScript con los endpoints de negocio de la API:

- Resumen de catálogo, socios, préstamos activos y vencidos.
- Catálogo: búsqueda, alta, edición y baja lógica para administradores.
- Administradores: alta de bibliotecarios con rol fijo `librarian`.
- Catálogo: selección de portada al guardar y retirada de la portada actual desde la edición.
- Socios: búsqueda y registro.
- Préstamos: préstamo de hasta tres libros disponibles y devolución de ejemplares.
- Seguimiento de líneas vencidas y reporte de títulos más prestados para administradores.

El cliente tipado maneja respuestas JSON y HTTP 204. El JWT se conserva en memoria y la sesión se pierde al recargar la página.

## Evidencia disponible

- `npm run build`: TypeScript y Vite finalizaron correctamente.
- Recorrido UI automatizado con respuestas deterministas: login, catálogo y alta de libro, alta de socio, préstamo, lista de vencidos/devolución y reporte.
- La vista de bibliotecario oculta Reportes y las acciones de administrador del catálogo.
- Dashboard a 375 px sin desbordamiento horizontal.
- En navegador con API simulada se creó un bibliotecario desde la vista admin; se comprobó el `POST users` con rol `librarian`.
- En navegador con API simulada se creó un libro con portada. Se verificó que la solicitud `PUT .../cover` incluye el campo multipart `file`, MIME `image/png` y boundary generado por el navegador.
- También se retiró una portada, se editó un libro y se dio de baja lógicamente; la vista mostró el estado vacío tras la baja.
- Se simuló un fallo temporal al subir la portada de un libro nuevo y se reintentó. El libro se creó una sola vez y el segundo intento completó la actualización y la subida.
- Se comprobó que el rol bibliotecario no muestra Bibliotecarios ni Reportes.
- A 320 px, la vista Bibliotecarios y el diálogo de libro no producen desbordamiento horizontal (el diálogo mide 284 px).
- Prueba HTTP autenticada contra la API y PostgreSQL: login admin; lectura de categorías, libros, socios, vencidos y reporte; creación de socio de prueba; préstamo de un ejemplar y devolución registrada. Se comprobó que la disponibilidad del libro volvió al valor inicial.
- La devolución de prueba usó `X-Test-Time` posterior al préstamo, encabezado reservado al entorno local/testing; no se alteraron los préstamos vencidos existentes.
- Build final de producción aprobado después de completar el dashboard.
- Build TypeScript/Vite aprobado después de añadir los formularios de bibliotecarios y portadas.

## Cierre de T-22

El recorrido de la interfaz y las operaciones autenticadas de la API se verificaron por separado; las respuestas del recorrido de UI son simuladas y la prueba HTTP es real contra PostgreSQL. Se verificaron los permisos de presentación por rol y responsive móvil. No se declara que la prueba del navegador haya escrito datos en la API real.

Los recorridos de interfaz añadidos se ejecutaron contra una API simulada determinista; no se escribieron cuentas, libros ni portadas de prueba en la API activa. El smoke HTTP real existente se ejecutó por separado y la cobertura API/PostgreSQL correspondiente está descrita en las evidencias de backend. No se afirma que la interacción navegador→API activa se haya probado de extremo a extremo. T-22 y HU-02/HU-03 se cierran con esa limitación explícita; los demás requisitos que sigan abiertos en la matriz mantienen su estado propio.
