# Instrucciones para agentes de desarrollo

## Objetivo

Construir Easy Mail respetando la documentación del repositorio. El producto es un panel interno de supervisión de correos por proyecto, no un cliente de correo completo.

## Orden de trabajo obligatorio

1. Leer `README.md`.
2. Leer todos los documentos de `docs/` en orden numérico.
3. Revisar `docs/09-open-decisions.md`.
4. Preguntar al responsable únicamente las decisiones todavía abiertas que bloqueen la fase que se va a iniciar.
5. Proponer el plan de implementación de esa fase.
6. Esperar confirmación antes de generar la aplicación o escribir funcionalidad.
7. Implementar en incrementos pequeños, probados y revisables.
8. Actualizar la documentación cuando una decisión cambie.

## Reglas de producto no negociables para el MVP

- El acceso al servidor de correo es de solo lectura.
- No implementar envío, respuesta, reenvío, borrado, archivado ni modificación remota de flags.
- No consultar IMAP durante una petición web para construir una pantalla.
- Los mensajes se sincronizan en segundo plano y se leen desde la base de datos local.
- `EmailMessage` representa un mensaje remoto; el servidor de correo sigue siendo la fuente original.
- Los estados personales, como visto o destacado, pertenecen a Easy Mail y no alteran el mensaje remoto.
- Un usuario solo puede ver proyectos a los que tenga acceso.
- Las credenciales no pueden persistirse en texto plano ni aparecer en logs.
- El HTML de los correos nunca se renderiza sin sanitización.
- La sincronización debe ser incremental, idempotente, reintentable y observable.
- No añadir IA, microservicios, API pública ni multitenencia SaaS al MVP.

## Criterio arquitectónico

Usar un monolito Symfony convencional con límites claros:

- `Controller`: HTTP y autorización; sin lógica de sincronización.
- `Application`: casos de uso y orquestación.
- `Domain`: entidades, enums y reglas del producto.
- `Infrastructure`: Doctrine, IMAP, almacenamiento y adaptadores externos.
- `Message` y `MessageHandler`: trabajos asíncronos mediante Symfony Messenger.

No introducir patrones abstractos sin un caso real. Las interfaces se justifican en los límites externos, especialmente el proveedor de correo, el cifrado de secretos y el almacenamiento de adjuntos.

## Calidad mínima

- PHP con tipado estricto.
- Migraciones versionadas.
- Validación de entrada mediante DTOs o formularios, no entidades expuestas directamente.
- Pruebas unitarias de normalización, filtrado e idempotencia.
- Pruebas de integración de repositorios y sincronización.
- Pruebas funcionales de autorización y pantallas críticas.
- Fixtures sin datos personales ni correos reales.
- Análisis estático y estilo automatizados en CI.

## Gestión del alcance

Si una petición contradice el MVP documentado, señalarlo antes de implementarla. Registrar las decisiones aprobadas en la documentación y, cuando corresponda, crear un ADR en `docs/adr/`.

