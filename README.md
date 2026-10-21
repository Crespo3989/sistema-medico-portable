# Sistema Médico Portable

Aplicación web local para gestionar citas médicas. Incluye PHP portable y una base de datos SQLite, por lo que puede ejecutarse en Windows sin instalar un servidor ni depender de Internet.

La interfaz actual utiliza Tailwind CSS 4 y daisyUI 5, con una estética inspirada en Fórmula 1: rojo, superficies oscuras, títulos en mayúsculas y navegación horizontal. El acceso incorpora una fotografía médica y permite elegir apariencia clara, oscura o del sistema.

## Inicio rápido

1. Conserva completa la carpeta del proyecto, incluidas `php/`, `app/`, `public/` y `data/`.
2. Ejecuta **EJECUTAR_SISTEMA.bat** con doble clic.
3. El navegador se abrirá en **http://127.0.0.1:8080/**.
4. Mantén abierta la ventana del servidor mientras uses la aplicación.
5. Para detener el sistema, cierra esa ventana o pulsa `Ctrl+C`.

Si cierras solo el navegador, puedes volver a abrir la dirección local. El lanzador también detecta cuando el sistema ya está funcionando.

### Primer acceso

| Usuario | Contraseña inicial |
| --- | --- |
| `admin` | `1234` |

Estas credenciales corresponden al primer uso. Si ya cambiaste la contraseña, utiliza la nueva.

Al entrar con la contraseña inicial aparece un aviso para cambiarla. Accede a **Seguridad** o a **Mi cuenta → Cambiar contraseña** y establece una contraseña de al menos ocho caracteres.

## Funciones

- Registrar citas con paciente, especialidad, fecha y hora.
- Validar los datos y comprobar la disponibilidad del horario por especialidad.
- Consultar y filtrar citas por paciente, especialidad, estado y fecha.
- Marcar citas como registradas, atendidas o canceladas.
- Reabrir o reactivar citas cuando el horario esté disponible.
- Eliminar citas después de revisar una pantalla de confirmación. El borrado es definitivo.
- Consultar el resumen de citas de hoy, pendientes, atendidas y canceladas.
- Imprimir el listado desde **Imprimir** o con `Ctrl+P`.
- Cambiar la contraseña del usuario.

Las especialidades iniciales son **Medicina General**, **Pediatría** y **Odontología**.

## Apariencia

El selector **Apariencia**, situado arriba a la derecha, ofrece tres opciones:

| Opción | Comportamiento |
| --- | --- |
| Sistema | Sigue la preferencia del sistema operativo o navegador. |
| Claro | Mantiene las superficies claras. |
| Oscuro | Mantiene las superficies oscuras. |

La elección se guarda en este navegador y se conserva al recargar o cerrar sesión. La interfaz adapta los formularios y el listado a pantallas pequeñas.

Las fuentes y la fotografía de acceso se sirven desde archivos locales. Se utiliza **Titillium Web**; la fuente de títulos **Formula1** no está incluida. El diseño es una adaptación visual, no una reproducción exacta de la web oficial.

## Configuración

Los ajustes generales están en [`app/config.php`](app/config.php):

| Ajuste | Valor inicial / uso |
| --- | --- |
| `nombre_sistema` | `Sistema Médico`. Nombre mostrado en la aplicación. |
| `zona_horaria` | `America/Guayaquil`. Referencia para fechas y horas. |
| `ruta_db` | Ruta al archivo SQLite dentro de `data/`. |
| `especialidades` | Lista de especialidades disponibles al registrar citas. |
| `minutos_inactividad` | `30`. Tiempo sin actividad antes de cerrar la sesión. |

Para cambiar el puerto, edita `set "PUERTO=8080"` en `EJECUTAR_SISTEMA.bat` y reinicia el servidor.

## Datos y copias de seguridad

La base de datos se guarda en **`data/citas_medicas.db`** e incluye las citas y las cuentas de acceso.

Para realizar un respaldo:

1. Detén el servidor.
2. Copia `data/citas_medicas.db` a una ubicación de respaldo.
3. Vuelve a iniciar el sistema.

Para restaurarlo, detén el servidor, conserva una copia del archivo actual y sustitúyelo por el respaldo. Guarda las copias en una ubicación protegida: contienen información de pacientes.

### Restablecer la contraseña

Ejecuta **RESTABLECER_CLAVE.bat** y confirma la operación. La contraseña de `admin` volverá a ser `1234`; las citas se conservan. Después del acceso, cambia nuevamente la contraseña.

## Desarrollo y estilos

Para usar la aplicación no necesitas Node.js ni npm. Solo se requieren para instalar las herramientas de desarrollo y recompilar el CSS.

Desde la carpeta del proyecto:

```powershell
npm ci
npm run build:css
```

| Archivo | Función |
| --- | --- |
| [`ui/app.css`](ui/app.css) | Entrada de Tailwind, configuración de daisyUI y temas. |
| [`ui/f1.css`](ui/f1.css) | Capa visual activa inspirada en Fórmula 1. |
| [`ui/F1-CLASES.md`](ui/F1-CLASES.md) | Guía de clases Tailwind y daisyUI para reutilizar el estilo. |
| `public/css/app.css` | CSS compilado que carga el navegador. |
| `public/css/style.css` | Estilos base importados como capa de compatibilidad. |
| `public/js/app.js` | Interacciones de la interfaz y renovación del formulario de acceso. |
| `public/js/theme.js` | Aplicación y persistencia de la apariencia. |
| `public/img/fondo-login.jpg` | Fotografía médica del panel de acceso. |
| `public/fonts/` | Fuentes locales y licencia de Titillium Web. |

Edita las fuentes de `ui/` y recompila; evita modificar directamente el CSS generado. Las hojas antiguas `clinical.css` y `glass.css` no se cargan en la interfaz actual.

### Inicio manual del servidor

Como alternativa al lanzador, ejecuta desde la raíz del proyecto:

```powershell
.\php\php.exe -c .\php\php.ini -d extension_dir=".\php\ext" -S 127.0.0.1:8080 -t public
```

Después abre http://127.0.0.1:8080/. Para detenerlo, pulsa `Ctrl+C` en esa terminal.

### Pruebas disponibles

```powershell
.\php\php.exe -c .\php\php.ini -d extension_dir=".\php\ext" tests/sesiones.php
.\php\php.exe -c .\php\php.ini -d extension_dir=".\php\ext" tests/eliminar_cita.php
```

La primera comprueba CSRF, inactividad y aislamiento de sesiones entre servidores. La segunda verifica la eliminación y sus efectos sobre disponibilidad e indicadores usando SQLite en memoria, sin modificar la base de datos del consultorio.

## Estructura del proyecto

```text
Sistema_Medico_Portable/
├── EJECUTAR_SISTEMA.bat
├── RESTABLECER_CLAVE.bat
├── app/                    # Configuración, reglas, seguridad y vistas PHP
├── data/                   # Base SQLite, sesiones y registro del servidor
├── php/                    # PHP portable y extensiones
├── public/                 # Única carpeta publicada por el servidor
│   ├── index.php
│   ├── css/
│   ├── js/
│   ├── img/
│   └── fonts/
├── ui/                     # Fuentes CSS y guía de clases
├── tests/                  # Pruebas de sesiones y eliminación
├── Guía de configuraciones/
├── DESIGN.md               # Decisiones y evolución del diseño
├── package.json
├── package-lock.json
└── README.md
```

## Seguridad y funcionamiento local

- El lanzador escucha en `127.0.0.1`, para acceso desde el mismo equipo.
- Solo se publica `public/`; la base de datos permanece fuera de esa carpeta.
- Las contraseñas se guardan mediante hash.
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
