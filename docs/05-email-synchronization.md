# 5. Sincronización de correo

## 5.1 Principio

IMAP es una fuente externa lenta y potencialmente inestable. La sincronización debe ejecutarse fuera del ciclo HTTP y convertir mensajes remotos en datos locales normalizados.

## 5.2 Importación inicial

Al activar una cuenta por primera vez:

1. Se valida la conexión y el acceso a la carpeta configurada.
2. Se obtiene `UIDVALIDITY` y el UID máximo conocido.
3. Se determina la ventana histórica aprobada.
4. Se importan mensajes por lotes, del más antiguo al más reciente dentro de esa ventana.
5. Se guardan cuerpo, cabeceras permitidas y metadatos.
6. Los adjuntos se procesan respetando tamaño y tipos permitidos.
7. Se actualiza el cursor únicamente después de persistir correctamente el lote.
8. Se registra el resultado en `SyncRun`.

La ventana inicial está pendiente de confirmar. Propuesta: últimos 90 días, con opción administrativa de ampliar posteriormente.

## 5.3 Sincronización incremental

La cuenta conserva:

- `uidValidity` de la carpeta.
- `lastUid` confirmado.
- Fecha de última sincronización correcta.

Cada ejecución solicita UIDs posteriores al cursor y procesa los resultados por lotes.

El cursor no avanza por encima de un mensaje que no se haya podido procesar sin que el fallo quede registrado y gestionado explícitamente.

## 5.4 Cambio de `UIDVALIDITY`

Los UIDs solo son válidos mientras no cambie `UIDVALIDITY`. Si cambia:

1. La cuenta pasa a estado de advertencia.
2. Se inicia una reconciliación controlada.
3. Se compara mediante `Message-ID` y fingerprint.
4. Se asignan los nuevos UIDs a mensajes reconocidos.
5. Se importan los no reconocidos.
6. No se borran automáticamente mensajes locales.

Este escenario requiere una prueba de integración específica.

## 5.5 Idempotencia

Defensas en orden:

1. Restricción única `mailbox + externalUid`.
2. Búsqueda por `Message-ID` dentro de la cuenta cuando exista.
3. Comparación del fingerprint cuando falte una identidad fiable.
4. Transacción por mensaje o lote pequeño.

El handler puede reintentarse tras una caída sin duplicar registros ni adjuntos.

## 5.6 Normalización MIME

El normalizador debe manejar:

- Mensajes de texto plano.
- Mensajes HTML.
- Multipart alternativo.
- Multipart mixto.
- Codificaciones y charsets habituales.
- Nombres de adjunto codificados.
- Adjuntos inline con `Content-ID`.
- Mensajes sin asunto, sin nombre de remitente o con fechas inválidas.

Se conserva solo el conjunto de cabeceras necesario. No debe almacenarse el mensaje RAW completo por defecto.

## 5.7 HTML y contenido externo

Antes de renderizar:

- Eliminar scripts, formularios, iframes y atributos peligrosos.
- Eliminar manejadores de eventos.
- Bloquear esquemas de URL no permitidos.
- Evitar que el CSS del correo afecte al resto de la aplicación.
- Bloquear imágenes remotas por defecto para evitar tracking y filtración de la IP del usuario.
- Permitir mostrar imágenes remotas solo mediante una acción explícita, si se aprueba.

El texto plano se muestra como alternativa cuando el HTML no sea seguro o no exista.

## 5.8 Adjuntos

Flujo recomendado:

1. Leer el stream del adjunto.
2. Aplicar un límite de tamaño configurable.
3. Calcular SHA-256.
4. Detectar el tipo real cuando sea posible.
5. Guardarlo bajo una clave aleatoria fuera del directorio público.
6. Persistir metadatos.
7. Servirlo mediante un controlador autenticado y autorizado.

No se ejecutan macros ni se previsualizan formatos activos. El análisis antivirus se considera obligatorio antes de producción si se permiten descargas de adjuntos no confiables.

## 5.9 Concurrencia y bloqueos

No pueden ejecutarse dos sincronizaciones simultáneas para la misma cuenta.

Opciones aceptables:

- Symfony Lock con almacenamiento compartido.
- Bloqueo transaccional/advisory de la base de datos.

Una sincronización bloqueada debe finalizar como omitida o reprogramarse; no debe esperar indefinidamente.

## 5.10 Frecuencia y reintentos

Propuesta inicial:

- Solicitud programada cada 2 minutos para cuentas activas.
- Lotes de tamaño configurable.
- Reintentos con espera creciente para errores transitorios.
- Sin reintento automático infinito ante credenciales inválidas.
- Cola de fallos para ejecuciones agotadas.

La frecuencia definitiva dependerá del número de cuentas y del servidor de correo.

## 5.11 Borrados y cambios remotos

El MVP no replica borrados, movimientos entre carpetas ni cambios de leído/no leído.

Consecuencias deliberadas:

- Un mensaje importado puede seguir visible localmente aunque luego se archive o borre en el servidor.
- `remoteFlags` es solo una instantánea informativa.
- No existe riesgo de que Easy Mail borre o modifique correo de producción.

Si más adelante se necesita reconciliación remota, se diseñará como una fase distinta con reglas explícitas de retención.

## 5.12 Errores esperados

Clasificar al menos:

- Credenciales o token inválidos.
- Host inaccesible.
- Error TLS/certificado.
- Carpeta inexistente.
- Límite o throttling del proveedor.
- Mensaje MIME no procesable.
- Adjunto demasiado grande.
- Almacenamiento lleno o inaccesible.
- Base de datos no disponible.

Los códigos internos deben ser estables para mostrarlos y medirlos. Los detalles técnicos completos quedan en logs restringidos.

## 5.13 Estrategia de pruebas

- Fixtures EML sintéticas para formatos MIME.
- Servidor IMAP de prueba o contenedor en integración.
- Reejecución del mismo lote para comprobar idempotencia.
- Cambio simulado de `UIDVALIDITY`.
- Caída entre persistencia y avance del cursor.
- Mensajes malformados.
- Adjuntos grandes, inline, duplicados y con nombres hostiles.
- Dos workers intentando sincronizar la misma cuenta.

