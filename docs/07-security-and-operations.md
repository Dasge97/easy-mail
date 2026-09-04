# 7. Seguridad y operación

## 7.1 Naturaleza de los datos

Easy Mail tratará comunicaciones empresariales, datos personales y adjuntos potencialmente sensibles. Aunque sea una herramienta interna, debe diseñarse como un sistema de datos confidenciales.

## 7.2 Autenticación

Requisitos mínimos:

- Contraseñas con el hasher recomendado por Symfony.
- Protección frente a fuerza bruta o rate limiting.
- Cookies de sesión seguras, `HttpOnly` y `SameSite` apropiado.
- HTTPS obligatorio en producción.
- Cierre de sesión disponible.
- Desactivación inmediata de usuarios.

SSO corporativo puede añadirse más adelante si existe proveedor de identidad.

## 7.3 Autorización

Toda consulta de proyectos, mensajes, adjuntos y secciones debe aplicar autorización en servidor.

Se recomienda:

- `ProjectVoter`.
- `EmailMessageVoter`, delegando el acceso en el proyecto.
- `MailboxVoter` para administración.
- `ROLE_ADMIN` para configuración global.

Evitar controladores que acepten un ID y consulten una entidad sin verificar su proyecto.

## 7.4 Secretos de las cuentas

- Preferir OAuth o tokens revocables cuando el proveedor lo permita.
- Si se requiere contraseña, cifrarla antes de persistir.
- La clave maestra vive fuera de la base de datos y del repositorio.
- No incluir secretos en excepciones, trazas, profiler, fixtures o logs.
- No devolver el secreto existente al formulario de edición.
- Permitir sustituirlo y revocar la conexión.

Las copias de seguridad de base de datos también contienen secretos cifrados y deben protegerse.

## 7.5 Protección frente al contenido de correo

El correo debe tratarse como entrada hostil.

- Sanitizar HTML con una política restrictiva.
- Bloquear JavaScript, iframes, formularios y URLs peligrosas.
- Bloquear recursos remotos por defecto.
- Escapar siempre asunto, remitente, nombres de archivo y cabeceras.
- No confiar en el tipo MIME declarado.
- Aplicar límites al cuerpo y adjuntos.
- Evitar path traversal al almacenar archivos.
- Servir adjuntos con headers seguros y nombre saneado.
- Evaluar antivirus antes del despliegue real.

## 7.6 Red y SSRF

La configuración de host IMAP por un administrador abre una posible vía SSRF.

En producción se debe decidir entre:

- Lista permitida de dominios/hosts corporativos, opción preferida.
- Resolución y bloqueo de rangos internos no autorizados.
- Configuración de cuentas mediante variables o administración restringida.

No permitir esquemas arbitrarios ni desactivar validación TLS como solución permanente.

## 7.7 Logs y auditoría

Registrar:

- Usuario que crea o modifica proyectos y cuentas.
- Cambio de accesos.
- Pruebas de conexión.
- Solicitudes manuales de sincronización.
- Inicio y resultado de cada `SyncRun`.
- Descargas de adjuntos si el nivel de auditoría lo exige.

No registrar:

- Contraseñas, tokens o cadenas de conexión completas.
- Cuerpo completo de correos.
- Contenido de adjuntos.
- Cabeceras de autenticación.

Usar un ID de correlación para vincular logs con `SyncRun`.

## 7.8 Copias de seguridad

La estrategia debe cubrir conjuntamente:

- Base de datos.
- Almacenamiento de adjuntos.
- Clave/configuración necesaria para descifrar credenciales.

Una copia incompleta puede dejar adjuntos huérfanos o credenciales irrecuperables. Se debe probar la restauración, no solo la creación del backup.

## 7.9 Retención

Antes de producción se debe acordar:

- Cuánto historial importar.
- Cuánto tiempo conservar mensajes locales.
- Cuánto tiempo conservar adjuntos.
- Cuánto tiempo conservar logs y `SyncRun`.
- Procedimiento de borrado de un proyecto.

El borrado debe incluir base de datos y archivos privados, con registro administrativo de la operación.

## 7.10 Procesos de producción

Servicios mínimos:

- Aplicación web PHP-FPM.
- Servidor web.
- Base de datos.
- Worker de Messenger supervisado.
- Scheduler o cron.
- Almacenamiento persistente para adjuntos.

El worker debe reiniciarse tras despliegues y tener límites razonables de memoria/tiempo. La cola de fallos debe revisarse y alertarse.

## 7.11 Health checks

Separar:

- **Liveness:** la aplicación responde.
- **Readiness:** base de datos y dependencias internas disponibles.
- **Estado funcional:** cuentas con sincronización atrasada o fallida.

No conectar a todas las cuentas IMAP durante un health check HTTP.

## 7.12 Métricas mínimas

- Duración de sincronización.
- Mensajes importados por cuenta.
- Última sincronización correcta.
- Número de cuentas fallidas.
- Trabajos pendientes y fallidos.
- Uso de almacenamiento de adjuntos.
- Tiempo de respuesta y errores HTTP.

## 7.13 Entornos

- Desarrollo: cuentas y mensajes sintéticos.
- Pruebas: servidor IMAP controlado y fixtures EML.
- Producción: credenciales reales inyectadas de forma segura.

No copiar mensajes reales a entornos de desarrollo sin un proceso aprobado de anonimización.

