# Ficha de seguridad de datos (Google Play Console)

Google Play exige llenar un formulario propio dentro de su consola —
esto no se sube como archivo, es un formulario web que hay que
completar a mano. Acá está cada respuesta ya resuelta, lista para
copiar campo por campo.

## Paso 1: ¿La app recopila o comparte alguno de los tipos de datos requeridos?

**Sí.**

## Paso 2: Tipos de datos

| Categoría | Tipo específico | ¿Se recopila? | ¿Se comparte con terceros? | ¿Es opcional? | Propósito |
|---|---|---|---|---|---|
| Ubicación | Ubicación aproximada / precisa | Sí | No | Sí | Funcionalidad de la app (mapa, SOS, funciones de cercanía) |
| Fotos y videos | Fotos, Videos | Sí | No | Sí | Comunicación entre usuarios de la malla |
| Audio | Grabaciones de voz | Sí | No | Sí | Comunicación entre usuarios de la malla (notas de voz, videollamada) |
| Mensajes | Otros mensajes en la app | Sí | No | No | Funcionalidad principal de la app (chat P2P) |
| Identificadores | ID de dispositivo u otros IDs | Sí | No | No | Identidad criptográfica del nodo dentro de la malla (no vinculada a una cuenta ni a una persona) |

**Importante:** en "¿Se comparte con terceros?" la respuesta es **No**
en todos los casos, porque no existe ningún tercero en la arquitectura
— los datos solo viajan entre los dispositivos de la propia malla del
usuario, nunca a un servidor de la app ni de ningún proveedor externo.

## Paso 3: Prácticas de seguridad

- **¿Los datos se cifran en tránsito?** Sí — todo el tráfico P2P usa
  cifrado (Fernet/AES sobre la clave compartida de la red, más
  X25519/Ed25519 para identidad y firmas).
- **¿Los usuarios pueden pedir que se borren sus datos?** Sí —
  desinstalar la app borra toda la base de datos local. No hay
  servidor donde pedir un borrado aparte, porque no hay servidor.
- **¿Cumplen con la Política de Familias de Google Play?** La app no
  está dirigida a niños; no marcarla como "diseñada para niños".

## Paso 4: Declaración final requerida por Google

Google pide una casilla de "Independientemente verificado" — RET no
tiene todavía una auditoría de seguridad externa formal, así que esa
casilla debe quedar **sin marcar**, con honestidad. Ver
`SECURITY.md`/modelo de amenazas para más contexto sobre este punto.
