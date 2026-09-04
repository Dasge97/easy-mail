# 9. Decisiones pendientes

Este documento separa lo confirmado de lo que todavía debe decidirse con el responsable. Un agente no debe convertir una propuesta en requisito definitivo sin confirmación.

## 9.1 Bloqueantes antes de implementar la conexión

### D-01 — Proveedor real de las cuentas

**Pregunta:** ¿las cuentas viven en Microsoft 365, IONOS, un servidor propio u otro proveedor?

**Por qué importa:** define autenticación, soporte OAuth, límites y cliente técnico.

**Propuesta:** mantener `MailProviderInterface` y hacer un spike con una cuenta de prueba del proveedor real antes de construir toda la sincronización.

### D-02 — Método de autenticación

**Pregunta:** ¿OAuth2, token específico de aplicación o contraseña IMAP?

**Propuesta:** OAuth2 cuando esté disponible; secreto cifrado únicamente cuando sea necesario.

### D-03 — Carpeta y volumen histórico

**Pregunta:** ¿solo `INBOX`? ¿Cuántos días o mensajes deben importarse al conectar una cuenta?

**Propuesta:** solo `INBOX` y últimos 90 días para el piloto.

## 9.2 Bloqueantes antes de cerrar la experiencia

### D-04 — Qué significa “revisado”

Opciones:

1. Abrir el mensaje lo marca como revisado automáticamente.
2. Abrir registra “visto”, pero revisar requiere una acción explícita.

**Propuesta:** opción 1 para un MVP más ligero, permitiendo volver a marcar como no revisado.

### D-05 — Ocultar mensajes

**Pregunta:** ¿el usuario necesita ocultar localmente mensajes que no le interesan o basta con revisado y destacado?

**Propuesta:** omitir `dismissedAt` en la primera iteración salvo necesidad clara.

### D-06 — Secciones iniciales reales

**Pregunta:** ¿qué tipos de correo reciben hoy los proyectos y cuáles son las primeras secciones útiles?

**Propuesta genérica:** errores, tickets, avisos automáticos y facturación, además de las secciones del sistema.

### D-07 — Notificaciones

**Pregunta:** ¿el MVP necesita notificaciones del navegador o basta con indicadores dentro del panel?

**Propuesta:** indicadores internos en el MVP; navegador o email más adelante.

## 9.3 Infraestructura y operación

### D-08 — Base de datos

**Pregunta:** ¿PostgreSQL o MySQL 8 según el entorno disponible?

**Propuesta:** PostgreSQL por capacidades de consulta textual, salvo que la infraestructura existente favorezca claramente MySQL.

### D-09 — Despliegue

**Pregunta:** ¿Docker en VPS, Plesk u otro entorno?

**Por qué importa:** define web server, supervisor del worker, cron, volúmenes y backups.

### D-10 — Almacenamiento de adjuntos

**Pregunta:** ¿filesystem persistente del servidor o almacenamiento compatible con S3?

**Propuesta:** filesystem privado en el piloto, detrás de `AttachmentStorageInterface`.

### D-11 — Política de retención

**Pregunta:** ¿los mensajes y adjuntos se conservan indefinidamente o durante un periodo?

**Propuesta:** no borrar automáticamente durante el piloto; acordar límites antes de incorporar todas las cuentas.

### D-12 — Antivirus

**Pregunta:** ¿qué solución corporativa se utilizará para analizar adjuntos?

**Propuesta:** integrar ClamAV o el servicio aprobado antes de habilitar descargas a todo el equipo.

## 9.4 Tecnología de interfaz

### D-13 — Sistema visual

**Pregunta:** ¿Bootstrap, Tailwind u otro sistema existente de la empresa?

**Propuesta:** Twig + Symfony UX Stimulus; decidir la capa CSS antes de crear componentes para evitar una migración visual temprana.

### D-14 — Vista de dos o tres paneles

**Pregunta:** ¿la primera bandeja abre el mensaje en una página propia o en una previsualización lateral?

**Propuesta:** lista + página de detalle primero; panel lateral en una mejora posterior si aporta velocidad real.

## 9.5 Decisiones ya cerradas

- Aplicación interna para un equipo.
- Symfony y Doctrine.
- Proyecto y correo como entidades persistidas.
- Una cuenta por proyecto en el caso habitual, con modelo preparado para varias.
- Sincronización asíncrona.
- Solo lectura respecto al servidor de correo.
- Estado de revisión local por usuario.
- IMAP detrás de una interfaz de proveedor.
- Monolito modular.
- Sin IA durante el MVP.

