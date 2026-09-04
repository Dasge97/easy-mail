# 1. Visión y alcance

## 1.1 Visión

Easy Mail debe convertir una colección de bandejas de entrada monótonas en un centro visual de supervisión de proyectos.

La unidad mental principal no es la cuenta de correo: es el proyecto. El correo actúa como una fuente de eventos e información del proyecto.

Una persona debería poder abrir Easy Mail y responder rápidamente a estas preguntas:

- ¿Qué proyectos han recibido actividad nueva?
- ¿Qué mensajes todavía no he revisado?
- ¿Hay errores, tickets o avisos importantes?
- ¿Qué cuenta no se está sincronizando correctamente?
- ¿Qué mensajes contienen adjuntos o requieren atención especial?

## 1.2 Usuarios

El MVP está dirigido a un equipo interno.

Roles iniciales:

- **Administrador:** gestiona usuarios, proyectos, accesos, cuentas, secciones y sincronizaciones.
- **Miembro:** consulta los proyectos autorizados y gestiona su estado personal de revisión.

No hay organizaciones, clientes, suscripciones ni facturación.

## 1.3 Objetivos del MVP

1. Centralizar visualmente los buzones de los proyectos.
2. Reducir el riesgo de pasar por alto información relevante.
3. Dar identidad y contexto a los mensajes según el proyecto.
4. Separar el estado personal de revisión del estado remoto del correo.
5. Comprobar que la sincronización IMAP es fiable antes de añadir automatización avanzada.
6. Crear una base de datos útil sobre la que se puedan construir reglas, notificaciones e IA en fases posteriores.

## 1.4 Fuera del MVP

- Enviar o responder correos.
- Reenviar mensajes.
- Crear borradores.
- Mover, borrar o archivar mensajes en el servidor.
- Marcar mensajes como leídos en el servidor.
- Sincronización bidireccional.
- Calendario y contactos.
- Gestión de tareas, tickets o asignaciones.
- Aplicación móvil nativa.
- Clasificación con IA.
- Resúmenes generativos.
- SaaS multicliente.
- API pública.
- Integraciones con Odoo, Jira, Slack u otras plataformas.

Estas capacidades pueden incorporarse después, pero no deben condicionar ni retrasar la validación inicial.

## 1.5 Principios de producto

### El proyecto está por encima del buzón

La navegación, los colores, los filtros y las métricas se organizan alrededor de `Project`.

### El correo se convierte en dato estructurado

Cada mensaje sincronizado se persiste como `EmailMessage`. No se consulta directamente al servidor durante la navegación normal.

### Solo lectura remota

El usuario puede abrir, revisar, destacar u ocultar mensajes dentro de Easy Mail. Ninguna de esas acciones modifica el buzón original durante el MVP.

### Claridad antes que densidad

El diseño debe ayudar a detectar actividad y prioridad. No debe reproducir la interfaz de Outlook ni mostrar todos los metadatos a la vez.

### Fallar de forma visible

Una cuenta desconectada o una sincronización fallida deben mostrarse claramente. Un panel con datos antiguos que aparenta estar actualizado es peor que un error explícito.

## 1.6 Indicadores de validación

El MVP se considerará útil si:

- El equipo consulta Easy Mail diariamente para revisar actividad.
- Un usuario identifica los proyectos con novedades sin abrir cada cuenta.
- La sincronización funciona durante una semana sin duplicados ni pérdidas conocidas.
- Los errores de conexión son detectables desde la propia plataforma.
- La organización visual reduce el tiempo necesario para revisar las bandejas.

