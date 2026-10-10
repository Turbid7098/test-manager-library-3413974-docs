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
- En la validación actual, el catálogo mostró su estado vacío con respuestas HTTP simuladas.
- Se contrastaron los payloads y rutas usadas por la interfaz con los controladores Laravel. Esto es una revisión de contrato, no evidencia de ejecución del recorrido completo.
- El recorrido de alta, préstamo, devolución, vencidos y reporte contra la API real aún no se ha confirmado desde el navegador.

## Cierre de T-22

T-22 permanece en curso hasta validar los flujos de escritura y permisos con la API en ejecución y completar una comprobación responsive de las pantallas de negocio. No se marcan como terminadas las historias HU-03 a HU-10 solo por tener sus pantallas implementadas.

Después de esa compuerta, el siguiente paso del plan es T-23: sitio público estático y responsive.
