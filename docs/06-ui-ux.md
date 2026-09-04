# 6. Interfaz y experiencia de usuario

## 6.1 Dirección de diseño

Easy Mail debe sentirse como un panel operativo moderno, no como una réplica de Outlook.

Características:

- Jerarquía visual fuerte por proyecto.
- Navegación corta y predecible.
- Resúmenes antes que tablas densas.
- Identificación rápida de novedades y errores.
- Diseño responsive, con prioridad a escritorio pero uso correcto en móvil.
- Acciones locales claramente diferenciadas de las acciones del correo original.

## 6.2 Estructura global

### Barra lateral

- Logotipo y nombre Easy Mail.
- Inicio.
- Proyectos autorizados.
- Estado o incidencias de sincronización para administradores.
- Administración para `ROLE_ADMIN`.

La lista de proyectos usa su color e icono, pero mantiene texto visible para no depender solo del color.

### Barra superior

- Migas de pan.
- Búsqueda contextual.
- Indicador de última actualización.
- Perfil del usuario.

## 6.3 Panel global

Orden recomendado:

1. Avisos de sincronización, solo si existen.
2. Tarjetas de proyectos con recuento personal no revisado.
3. Actividad reciente transversal.
4. Acceso a todos los proyectos.

Una tarjeta de proyecto incluye:

- Icono y color.
- Nombre.
- Número de mensajes pendientes de revisar.
- Remitente y asunto del último mensaje.
- Hora de última actividad.
- Estado de sincronización cuando no sea correcto.

## 6.4 Bandeja de proyecto

Diseño de escritorio sugerido:

```text
┌ Secciones ┐ ┌ Lista de mensajes ────────────┐ ┌ Vista previa ───────┐
│ Todos     │ │ Remitente / asunto / extracto │ │ Mensaje seleccionado│
│ Nuevos    │ │ Remitente / asunto / extracto │ │ Cuerpo y adjuntos   │
│ Errores   │ │ Remitente / asunto / extracto │ │                     │
│ Adjuntos  │ │ ...                           │ │                     │
└───────────┘ └───────────────────────────────┘ └─────────────────────┘
```

En pantallas medianas, la vista previa se abre como panel lateral. En móvil, lista y detalle son pantallas independientes.

La implementación puede comenzar con dos columnas y añadir la previsualización de tres paneles cuando la bandeja básica esté validada.

## 6.5 Lista de mensajes

Cada elemento debe priorizar:

1. Estado no revisado.
2. Remitente.
3. Asunto.
4. Extracto.
5. Momento de recepción.
6. Adjuntos e importancia.

No debe mostrarse cada campo disponible. Los detalles técnicos pertenecen al detalle del mensaje.

Estados visuales:

- No revisado: mayor peso y marcador accesible.
- Revisado: menor contraste, sin perder legibilidad.
- Destacado: icono consistente.
- Importancia alta: indicador semántico, no solo rojo.

## 6.6 Detalle del mensaje

Cabecera compacta:

- Asunto.
- Remitente.
- Fecha.
- Destinatarios expandibles.
- Acciones locales: revisar y destacar.

Cuerpo:

- HTML sanitizado en un contenedor aislado visualmente.
- Alternativa de texto plano.
- Aviso cuando las imágenes remotas estén bloqueadas.

Adjuntos:

- Nombre, tipo y tamaño.
- Acción de descarga autorizada.
- Advertencia para tipos potencialmente peligrosos.

## 6.7 Secciones y filtros

Las secciones aparecen dentro del proyecto con recuento personal.

Los filtros rápidos deben reflejarse en la URL para poder recargar, navegar atrás y compartir un enlace interno:

```text
/projects/acme/inbox?section=errors&unreviewed=1&q=timeout
```

Los parámetros se validan mediante una lista cerrada.

## 6.8 Estados vacíos y errores

Preparar estados específicos:

- Proyecto sin cuenta configurada.
- Cuenta esperando primera sincronización.
- Bandeja sin mensajes.
- Filtro sin resultados.
- Sincronización en curso.
- Credenciales inválidas.
- Datos desactualizados.
- Usuario sin proyectos asignados.

Cada estado debe explicar qué sucede y ofrecer la siguiente acción cuando el usuario tenga permisos.

## 6.9 Comportamiento de revisión

Decisión recomendada para el MVP:

- Abrir un mensaje crea `firstViewedAt`.
- El mensaje se considera revisado al abrirlo y establece `reviewedAt`.
- El usuario puede devolverlo manualmente a no revisado.
- Destacar es una acción independiente.

Esta decisión debe confirmarse antes de implementar porque afecta los recuentos y la interacción de la bandeja.

## 6.10 Accesibilidad

- Navegación completa por teclado.
- Foco visible.
- Etiquetas accesibles en iconos.
- Contraste WCAG AA como mínimo.
- No depender únicamente de color o animación.
- Controles táctiles de tamaño suficiente.
- Fechas legibles y fecha completa disponible como texto auxiliar.

