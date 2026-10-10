# T-22 — Flujos de negocio del frontend

**Estado:** verificado para los flujos de T-22.

## Implementación

El repositorio `app` conecta React + TypeScript con los endpoints de negocio de la API:

- Resumen de catálogo, socios, préstamos activos y vencidos.
- Catálogo: búsqueda, alta, edición y baja lógica para administradores.
- Socios: búsqueda y registro.
- Préstamos: préstamo de hasta tres libros disponibles y devolución de ejemplares.
- Seguimiento de líneas vencidas y reporte de títulos más prestados para administradores.

El cliente tipado maneja respuestas JSON y HTTP 204. El JWT se conserva en memoria y la sesión se pierde al recargar la página.

## Evidencia disponible

- `npm run build`: TypeScript y Vite finalizaron correctamente.
- Recorrido UI automatizado con respuestas deterministas: login, catálogo y alta de libro, alta de socio, préstamo, lista de vencidos/devolución y reporte.
- La vista de bibliotecario oculta Reportes y las acciones de administrador del catálogo.
- Dashboard a 375 px sin desbordamiento horizontal.
- Prueba HTTP autenticada contra la API y PostgreSQL: login admin; lectura de categorías, libros, socios, vencidos y reporte; creación de socio de prueba; préstamo de un ejemplar y devolución registrada. Se comprobó que la disponibilidad del libro volvió al valor inicial.
- La devolución de prueba usó `X-Test-Time` posterior al préstamo, encabezado reservado al entorno local/testing; no se alteraron los préstamos vencidos existentes.
- Build final de producción aprobado después de completar el dashboard.

## Cierre de T-22

El recorrido de la interfaz y las operaciones autenticadas de la API se verificaron por separado; las respuestas del recorrido de UI son simuladas y la prueba HTTP es real contra PostgreSQL. Se verificaron los permisos de presentación por rol y responsive móvil. No se declara que la prueba del navegador haya escrito datos en la API real.

T-22 está cerrado para el alcance de tareas del spec. Las historias HU-03 a HU-10 conservan su estado individual si otros criterios fuera del flujo T-22 siguen abiertos.
