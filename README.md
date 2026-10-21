
- Las operaciones sobre citas requieren una sesión autenticada y validación CSRF.
- Las sesiones usan cookies `HttpOnly` y `SameSite=Strict`, con aislamiento por instalación y puerto.
- El acceso renueva el token antes de enviar el formulario y la sesión se cierra por inactividad.
- La salida de texto utiliza escape HTML y las respuestas incluyen cabeceras de seguridad.

El servidor integrado de PHP está destinado al uso local de esta versión portable. Su configuración actual no publica el sistema en Internet.

## Problemas frecuentes

| Problema | Qué revisar |
| --- | --- |
| No se encontró PHP | Comprueba que exista `php/php.exe`. Consulta `Guía de configuraciones/`. |
| El driver `pdo_sqlite` no está activo | Revisa `php/php.ini`, las extensiones de `php/ext/` y la guía incluida. |
| Otro programa está usando el puerto 8080 | Detén ese programa o cambia el puerto en el lanzador. |
| La página no abre | Comprueba que el servidor siga funcionando y usa la dirección y el puerto correctos. |
| El sistema ya estaba abierto en otra ventana | El lanzador detectó una instancia activa y abrirá su dirección. |
| La sesión se actualizó o el formulario caducó | Recarga la página e inicia sesión otra vez. Las acciones vencidas deben volver a enviarse desde el formulario actualizado. |
| Los estilos no reflejan los cambios | Ejecuta `npm run build:css` y recarga con `Ctrl+F5`. |
| Se olvidó la contraseña | Utiliza `RESTABLECER_CLAVE.bat`. |

Cuando arrancas mediante el lanzador, las peticiones y errores se registran en **`data/servidor.log`**. El archivo se reinicia en cada arranque.
