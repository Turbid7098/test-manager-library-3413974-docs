# Frontend — Primer flujo React

**Estado:** primera pantalla de frontend y acceso conectado implementados; la aplicación de biblioteca completa sigue en proceso.

## Entregado

- Se sustituyó la plantilla inicial de Vite por React + TypeScript.
- Pantalla de inicio de sesión en español, adaptable a móvil y escritorio, con validación requerida, mostrar/ocultar contraseña, estado de carga y mensaje de error accesible.
- Cliente HTTP para `POST /api/v1/auth/login`, base URL configurable por `VITE_API_URL` y validación de la forma de respuesta JWT.
- Al autenticar se muestra un panel inicial con el rol, acción de cerrar sesión y adelanto explícito de los módulos pendientes. El token solo permanece en memoria; tras recargar, el usuario debe iniciar sesión nuevamente.

## Evidencia

- `npm run build`: TypeScript y Vite completan sin errores.
- Docker Compose sirve la pantalla en `http://localhost:5173` y devuelve HTTP 200.
- Login inválido contra el backend: respuesta `401 INVALID_CREDENTIALS` presentada sin exponer detalles del usuario.
- El estado autenticado y la acción de logout se comprobaron en navegador con una respuesta JWT simulada para aislar la interfaz. El login real del administrador se validó separadamente contra PostgreSQL en [`08-p4-authentication.md`](./08-p4-authentication.md).
- En 390 px, el panel inicial no genera desbordamiento horizontal.
- Las dependencias del frontend usan un volumen Linux independiente de `node_modules` del host, para el desarrollo desde Windows.

## Siguiente

Conectar el catálogo de libros a los endpoints CRUD/búsqueda, agregar formularios para socios y préstamos cuando esos endpoints existan, y aplicar el Bearer token al cliente HTTP de las rutas protegidas. No mostrar los módulos como operativos hasta que la API correspondiente esté implementada y probada.
