# Política de Privacidad — RET (Red Inteligente Persistente)

**Última actualización:** [completar con la fecha de publicación]

## Resumen en una frase

RET no tiene servidor propio. No hay ninguna empresa, ni el creador de la
app, que pueda ver, recibir o almacenar tus datos — todo vive únicamente
en tu teléfono y, cuando tú decides compartirlo, en los teléfonos de la
malla local a la que te conectas.

## Qué datos usa la app, y por qué

| Dato | Para qué se usa | ¿Sale de tu teléfono? |
|---|---|---|
| Ubicación (GPS) | Mapa de malla, SOS, mensajes anclados, Nodo Testigo, Juego Encuentro | Solo si tú activas esas funciones — se comparte cifrado con la malla local, nunca con un servidor externo |
| Cámara y micrófono | Videollamada 1 a 1 y en sala, notas de voz | Solo durante una llamada activa o al enviar una nota de voz — directo al otro dispositivo, nunca a un servidor |
| Bluetooth | Descubrir otros nodos RET cercanos | No, es local al dispositivo |
| Contactos de red local (WiFi) | Descubrir y conectar con otros nodos RET | No sale de la red local |
| Mensajes, fotos, videos que envías | Comunicación dentro de tu malla | Solo a los dispositivos de tu malla que tú elijas — nunca a un servidor de la app |

## Lo que NO hacemos

- No tenemos servidores donde se guarden tus datos.
- No vendemos ni compartimos datos con terceros — no hay terceros en la arquitectura.
- No mostramos publicidad ni usamos rastreadores de análisis (analytics) de ningún tipo.
- No pedimos que crees una cuenta ni un correo electrónico.
- No accedemos a internet salvo que tú actives explícitamente la función opcional de "puentes" para unir dos mallas en ubicaciones distintas.

## Dónde se guardan tus datos

Únicamente en una base de datos local (SQLite) dentro de tu propio
dispositivo. Si desinstalas la app, esos datos se borran con ella. Nadie
más — ni el desarrollador de RET — tiene acceso a esa base de datos.

## Permisos que pide la app, uno por uno

- **Ubicación**: para el mapa de malla, SOS y funciones que dependen de
  cercanía física. Puedes usar RET sin darlo, pero esas funciones no
  van a andar.
- **Cámara / Micrófono**: solo se activan durante una videollamada o al
  grabar una nota de voz — nunca en segundo plano sin que tú lo pidas.
- **Bluetooth**: para descubrir otros nodos cercanos sin necesidad de
  estar en la misma red WiFi.
- **Notificaciones**: para avisarte de mensajes nuevos o alertas SOS
  aunque no tengas la app abierta.

## Menores de edad

RET no está dirigida a menores de 13 años y no recopila
intencionalmente información de menores.

## Cambios a esta política

Si esta política cambia, se actualizará la fecha de arriba y, si el
cambio es significativo, se avisará dentro de la propia app.

## Contacto

[Completar: correo o forma de contacto del desarrollador — obligatorio
para publicar en Play Store / App Store]
