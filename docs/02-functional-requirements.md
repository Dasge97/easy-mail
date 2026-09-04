# 2. Requisitos funcionales

Las referencias `RF-XX` se usarán en tareas, pruebas y criterios de aceptación.

## 2.1 Autenticación y autorización

### RF-01 — Inicio de sesión

El usuario debe autenticarse antes de acceder a cualquier dato de la aplicación.

### RF-02 — Roles

El sistema debe diferenciar entre administrador y miembro.

### RF-03 — Acceso por proyecto

Un miembro solo debe poder consultar los proyectos para los que exista una membresía activa. El administrador puede consultar y gestionar todos los proyectos.

La autorización debe comprobarse en servidor mediante voters o políticas equivalentes; ocultar enlaces en la interfaz no es suficiente.

## 2.2 Proyectos

### RF-04 — Listado de proyectos

El usuario debe ver los proyectos autorizados con:

- Nombre, color e icono.
- Número de mensajes nuevos para ese usuario.
- Fecha del último mensaje sincronizado.
- Estado de la cuenta.
- Momento de la última sincronización correcta.

### RF-05 — Gestión de proyectos

Un administrador puede crear, editar, activar y desactivar proyectos.

Desactivar un proyecto no borra sus mensajes ni sus adjuntos.

### RF-06 — Identidad visual

Cada proyecto puede definir nombre, descripción, color e icono. Estos elementos deben repetirse de forma consistente en su bandeja y en el panel global.

## 2.3 Cuentas de correo

### RF-07 — Configuración

Un administrador puede asociar una o más cuentas a un proyecto. Para el primer MVP se configurará normalmente una cuenta por proyecto.

### RF-08 — Prueba de conexión

Antes de guardar o activar una cuenta, el administrador puede comprobar host, puerto, cifrado, autenticación y carpeta de origen.

El resultado debe ser comprensible y no revelar secretos.

### RF-09 — Sincronización manual

Un administrador puede solicitar una sincronización manual. La petición encola el trabajo y no mantiene bloqueada la respuesta HTTP.

### RF-10 — Estado de sincronización

La interfaz muestra al menos:

- Pendiente.
- Sincronizando.
- Correcta.
- Con advertencias.
- Fallida.
- Desactivada.

## 2.4 Mensajes

### RF-11 — Listado

La bandeja de un proyecto muestra mensajes locales paginados y ordenados de más reciente a más antiguo.

Cada fila o tarjeta muestra:

- Remitente.
- Asunto.
- Extracto.
- Fecha y hora.
- Indicador de adjuntos.
- Importancia, si existe.
- Estado personal de revisión.

### RF-12 — Detalle

El usuario puede abrir un mensaje y consultar:

- Remitente y destinatarios.
- Asunto.
- Fechas disponibles.
- Cuerpo HTML sanitizado o alternativa de texto.
- Adjuntos.
- Datos técnicos básicos bajo una sección secundaria.

Abrir el detalle registra el mensaje como visto para ese usuario dentro de Easy Mail.

### RF-13 — Acciones locales

El usuario puede:

- Marcar o desmarcar como destacado.
- Marcar como revisado o no revisado.
- Ocultar de su vista personal, si esta función se confirma antes de implementar.

Estas acciones no modifican el servidor de correo.

### RF-14 — Adjuntos

El usuario puede ver la lista de adjuntos y descargar los permitidos mediante una ruta autenticada. Los adjuntos nunca se sirven directamente desde un directorio público.

### RF-15 — Búsqueda

El usuario puede buscar dentro de un proyecto por asunto, remitente, dirección y contenido textual indexable.

La primera versión puede usar capacidades de la base de datos. No se requiere un motor externo.

### RF-16 — Filtros rápidos

La bandeja permite filtrar por:

- No revisados.
- Destacados.
- Con adjuntos.
- Importancia.
- Intervalo de fechas.
- Cuenta, si el proyecto tiene más de una.

## 2.5 Secciones

### RF-17 — Secciones del sistema

Cada proyecto dispone inicialmente de:

- Todos.
- No revisados.
- Destacados.
- Con adjuntos.

### RF-18 — Secciones personalizadas

Un administrador puede crear secciones visuales con nombre, icono, color, posición y reglas.

Criterios iniciales admitidos:

- El remitente es o contiene un valor.
- La dirección del remitente pertenece a un dominio.
- El asunto contiene una o varias expresiones.
- El cuerpo contiene una o varias expresiones.
- Tiene adjuntos.
- Tiene una importancia determinada.

Una sección combina sus criterios mediante `AND` u `OR`. Un mismo mensaje puede aparecer en varias secciones.

### RF-19 — Recuento por sección

Cada sección muestra el número de mensajes no revisados por el usuario actual.

## 2.6 Panel global

### RF-20 — Resumen

El panel muestra:

- Proyectos con actividad reciente.
- Recuentos no revisados por proyecto.
- Últimos mensajes recibidos.
- Cuentas con problemas de sincronización.

### RF-21 — Navegación directa

El usuario puede entrar desde cualquier elemento del resumen al proyecto, sección o mensaje correspondiente.

## 2.7 Administración

### RF-22 — Usuarios y accesos

Un administrador puede crear o desactivar usuarios y asignar proyectos.

### RF-23 — Historial de sincronización

Un administrador puede consultar ejecuciones recientes con duración, resultado, mensajes detectados y errores sanitizados.

### RF-24 — Retención y borrado administrativo

La política de retención se definirá antes de producción. Cualquier borrado local debe ser explícito, auditable y limitado al proyecto o cuenta seleccionados.

## 2.8 Requisitos no funcionales

### RNF-01 — Rendimiento

Las vistas normales deben responder usando exclusivamente la base de datos y el almacenamiento local. Ninguna pantalla debe esperar una conexión IMAP.

### RNF-02 — Idempotencia

Procesar dos veces el mismo mensaje remoto no debe crear duplicados.

### RNF-03 — Observabilidad

Los fallos de sincronización deben quedar registrados con contexto suficiente para diagnosticar, pero sin credenciales ni cuerpos completos.

### RNF-04 — Accesibilidad y adaptación

La interfaz debe ser utilizable en escritorio, tableta y móvil, con navegación por teclado, contraste suficiente y estados que no dependan solo del color.

### RNF-05 — Zona horaria

Las fechas se almacenan en UTC y se muestran en la zona horaria configurada para la aplicación o el usuario.

