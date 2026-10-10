# T-22 — Flujos de negocio del frontend

**Estado:** implementación y compilación verificadas; validación integral con la API real en curso.

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
- La prueba visual previa verificó inicio de sesión simulado, dashboard y navegación por sus seis vistas.
- El usuario confirmó el acceso al dashboard local.
- Prueba HTTP autenticada contra la API y PostgreSQL: login admin; lectura de categorías, libros, socios, vencidos y reporte; creación de socio de prueba; préstamo de un ejemplar y devolución registrada. Se comprobó que la disponibilidad del libro volvió al valor inicial.
- La devolución de prueba usó `X-Test-Time` posterior al préstamo, encabezado reservado al entorno local/testing; no se alteraron los préstamos vencidos existentes.
- Se contrastaron los payloads y rutas usados por la interfaz con los controladores Laravel. La sesión del usuario no estuvo disponible en el navegador de pruebas, así que no se afirma que se haya completado un recorrido automatizado de formularios en la UI.

## Cierre de T-22

T-22 permanece en curso hasta validar visualmente desde la UI los formularios de escritura, permisos por rol y responsive de las pantallas de negocio. No se marcan como terminadas las historias HU-03 a HU-10 solo por tener sus pantallas implementadas.

Después de esa compuerta, el siguiente paso del plan es T-23: sitio público estático y responsive.
