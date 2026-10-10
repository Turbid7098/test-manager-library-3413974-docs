# T-23 — Sitio público estático

**Estado:** implementado, probado localmente y publicado en el fork `Turbid7098/test-manager-library-3413974-page`.

## Implementación

El repositorio `page` contiene una página estática en HTML, CSS y JavaScript nativo, sin gestor de paquetes ni servicio API. Presenta el producto, las funciones principales, el flujo de uso y enlaces a la aplicación y documentación.

La página incluye navegación adaptable con menú accesible en móvil, landmarks y encabezados semánticos, enlace para saltar al contenido, foco visible, soporte para `prefers-reduced-motion`, metadatos sociales y favicon SVG. No carga tipografías, imágenes ni bibliotecas de terceros y no hace solicitudes a la API.

## Evidencia local

- Servida con un servidor estático local: `GET /` y sus recursos devolvieron HTTP 200.
- Revisión en navegador a 320, 375 y 1365 píxeles: no hubo desbordamiento horizontal.
- En móvil, el menú se abre, permite seguir los enlaces de sección y se cierra después de elegir una sección.
- Anclas internas verificadas; cero errores JavaScript.
- La página se carga y navega sin que la API esté levantada.

## Publicación

Se creó el fork desde `code-sena/test-manager-library-3413974-page`, integrando su commit inicial con el historial local sin force-push. Sitio y README publicados en `main`, commit `7e05fc6`.

La publicación del código en GitHub está verificada. No se configuró GitHub Pages ni se afirma que exista una URL de despliegue web.
