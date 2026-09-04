# Easy Mail

Easy Mail es una aplicación web interna que transforma las cuentas de correo de los proyectos en paneles visuales fáciles de revisar.

La aplicación no pretende sustituir Outlook ni convertirse en un cliente de correo completo. Su función es sincronizar en modo lectura los mensajes recibidos, almacenarlos como entidades propias y ofrecer una representación organizada por proyecto, secciones y estado personal de revisión.

## Problema

En el trabajo, gran parte de la información operativa llega por correo: fallos, tickets, avisos automáticos, facturas y comunicaciones de proveedores. Cada proyecto dispone de su propia cuenta, pero todas las bandejas se presentan de forma prácticamente idéntica en el cliente de correo. Esto dificulta identificar rápidamente qué ha ocurrido, qué es importante y qué queda por revisar.

## Propuesta

Easy Mail proporciona:

- Un panel global con todos los proyectos.
- Una identidad visual propia para cada proyecto.
- Una bandeja visual por proyecto.
- Secciones filtradas como errores, tickets, avisos o mensajes con adjuntos.
- Estado de revisión individual para cada usuario.
- Sincronización periódica y manual con la cuenta de correo.
- Visualización segura del contenido y los adjuntos.
- Supervisión del estado de cada conexión.

El servidor de correo continúa siendo la fuente original. Easy Mail mantiene una copia local, indexada y enriquecida para construir la experiencia de consulta.

## Decisiones confirmadas

- Es una herramienta interna para un equipo, no un SaaS multicliente.
- Cada proyecto tiene al menos una cuenta de correo asociada.
- El backend se desarrollará con Symfony y Doctrine.
- Los proyectos y los correos serán entidades persistidas en la base de datos.
- El MVP será de solo lectura respecto al servidor de correo.
- El usuario podrá organizar su revisión dentro de Easy Mail sin modificar el buzón remoto.
- La primera integración será mediante IMAP, aislada detrás de una interfaz de proveedor.
- La arquitectura será un monolito modular, sin microservicios.

## Documentación

1. [Visión y alcance](docs/01-vision-and-scope.md)
2. [Requisitos funcionales](docs/02-functional-requirements.md)
3. [Arquitectura técnica](docs/03-architecture.md)
4. [Modelo de datos](docs/04-data-model.md)
5. [Sincronización de correo](docs/05-email-synchronization.md)
6. [Interfaz y experiencia de usuario](docs/06-ui-ux.md)
7. [Seguridad y operación](docs/07-security-and-operations.md)
8. [Plan de desarrollo y aceptación](docs/08-development-plan.md)
9. [Decisiones pendientes](docs/09-open-decisions.md)

## Estado del repositorio

El repositorio contiene actualmente la especificación funcional y técnica del proyecto. Todavía no se ha generado la aplicación Symfony.

Antes de implementar, el agente debe leer [AGENTS.md](AGENTS.md) y después los documentos anteriores en orden.

