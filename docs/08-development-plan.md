# 8. Plan de desarrollo y aceptación

El plan está dividido en entregas verticales. Cada fase debe dejar una aplicación demostrable y probada.

## Fase 0 — Cerrar decisiones y preparar el repositorio

### Trabajo

- Resolver las decisiones bloqueantes de `09-open-decisions.md`.
- Crear el proyecto Symfony.
- Configurar entorno local y variables de ejemplo.
- Configurar Doctrine, migraciones y base de datos.
- Configurar pruebas, estilo y análisis estático.
- Crear CI.
- Documentar arranque local.

### Salida

- La aplicación arranca localmente.
- CI ejecuta pruebas y comprobaciones.
- No hay secretos en el repositorio.

## Fase 1 — Acceso, proyectos y permisos

### Trabajo

- Entidades `User`, `Project` y `ProjectMembership`.
- Login y logout.
- Roles y voters.
- CRUD administrativo de proyectos.
- Asignación de miembros.
- Identidad visual básica.

### Aceptación

- Un miembro no puede acceder por URL a un proyecto no asignado.
- Un administrador puede crear y desactivar proyectos.
- Desactivar un proyecto conserva los datos.
- Las pruebas funcionales cubren autorización positiva y negativa.

## Fase 2 — Cuentas y prueba IMAP

### Trabajo

- Entidades `Mailbox` y `SyncRun`.
- Cifrado de secretos.
- Formulario administrativo.
- `MailProviderInterface`.
- Adaptador IMAP inicial.
- Caso de uso de prueba de conexión.
- Estados operativos de cuenta.

### Aceptación

- Se puede validar una cuenta sin guardar el secreto en claro.
- Un error se muestra de forma comprensible y segura.
- Un miembro no puede consultar ni modificar configuración.
- Existen pruebas de conexión correcta e incorrecta con un servidor controlado.

## Fase 3 — Importación de mensajes

### Trabajo

- `EmailMessage`, `EmailAttachment` y migraciones.
- Normalización MIME.
- Almacenamiento privado de adjuntos.
- `SyncMailbox` y handler de Messenger.
- Importación inicial e incremental.
- Bloqueo por cuenta.
- Reintentos y cola de fallos.

### Aceptación

- Un conjunto conocido de mensajes se importa con campos correctos.
- Ejecutar dos veces la misma sincronización no duplica mensajes ni adjuntos.
- Una caída antes de confirmar el cursor es recuperable.
- Dos workers no sincronizan simultáneamente la misma cuenta.
- El historial muestra el resultado.

## Fase 4 — Bandeja y detalle

### Trabajo

- Bandeja paginada por proyecto.
- Búsqueda y filtros rápidos.
- Detalle de mensaje.
- Sanitización HTML.
- Descarga autenticada de adjuntos.
- Estado vacío y estado de sincronización.

### Aceptación

- Las pantallas no realizan llamadas IMAP.
- Un usuario no puede abrir mensajes o adjuntos de otro proyecto.
- El HTML hostil de las fixtures no ejecuta código.
- La bandeja funciona con un volumen representativo de datos.

## Fase 5 — Estado personal y panel global

### Trabajo

- `EmailUserState`.
- Revisado/no revisado y destacado.
- Recuentos personales.
- Panel global.
- Actividad reciente.
- Avisos de cuentas con problemas.

### Aceptación

- La acción de un usuario no cambia el estado de otro.
- Los recuentos se actualizan correctamente.
- El panel solo combina proyectos autorizados.
- No se envían cambios al servidor de correo.

## Fase 6 — Secciones personalizadas

### Trabajo

- `InboxSection` y esquema de filtros versión 1.
- CRUD administrativo.
- Compilador seguro de criterios.
- Recuentos por sección.
- Ordenación visual.

### Aceptación

- Un mensaje puede aparecer en varias secciones.
- Las reglas `all` y `any` producen resultados esperados.
- No es posible inyectar SQL mediante filtros.
- Los cambios de sección se reflejan sin reclasificación destructiva.

## Fase 7 — Endurecimiento y despliegue piloto

### Trabajo

- Política de retención.
- Antivirus o decisión explícita sobre adjuntos.
- Backups y prueba de restauración.
- Monitorización y health checks.
- Revisión de índices y rendimiento.
- Revisión de accesibilidad.
- Despliegue con un grupo reducido de proyectos.

### Aceptación

- Una restauración de prueba recupera base de datos y adjuntos.
- Los fallos de worker y sincronización generan una señal visible.
- No se encuentran secretos ni datos reales en repositorio o CI.
- El equipo valida el flujo durante una semana.

## Estrategia de ramas y entregas

Recomendación:

- `main` siempre desplegable.
- Una rama corta por tarea o entrega.
- Pull requests pequeñas con migraciones y pruebas asociadas.
- No mezclar refactors generales con una funcionalidad.

## Definición de terminado

Una tarea no está terminada hasta que:

- Cumple los criterios funcionales asociados.
- Incluye pruebas proporcionadas al riesgo.
- Pasa análisis estático y estilo.
- Incluye migración cuando cambia persistencia.
- No introduce secretos ni datos reales.
- Actualiza documentación si cambia una decisión.
- Incluye instrucciones de verificación manual cuando afectan a la interfaz.

## Casos de prueba transversales imprescindibles

1. Usuario sin acceso intenta enumerar y abrir datos de otro proyecto.
2. Repetición de una sincronización completa.
3. Mensaje con MIME malformado.
4. Mensaje HTML con XSS.
5. Adjunto con nombre malicioso o tamaño excesivo.
6. Credenciales IMAP inválidas.
7. Cambio de `UIDVALIDITY`.
8. Worker interrumpido a mitad de un lote.
9. Dos usuarios revisan el mismo mensaje.
10. Proyecto o cuenta desactivados conservan historial sin volver a sincronizar.

