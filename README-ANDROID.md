# RET para Android — estado real de este proyecto

## Qué es esto

Un proyecto Android (Kotlin + Chaquopy) que empaqueta `ret.py` completo
tal cual — no es una reescritura ni una versión reducida. Chaquopy
embebe un intérprete Python real dentro del APK, así que toda la
lógica (gossip, DHT, cifrado, SOS, puentes, mapa) sigue siendo
exactamente el mismo código que ya probamos en escritorio.

## Novedades de esta vuelta: notificaciones + persistencia en segundo plano

- **Notificaciones reales**: el servicio revisa cada 15s (vía
  `app_bridge.novedades()`) si hay mensajes nuevos en #general o SOS
  activas de otros nodos, y muestra una notificación del sistema —
  antes esto solo se veía si tenías la app abierta mirando el WebView.
  Canal separado y de alta prioridad para SOS (con vibración).
- **Botón "No cerrar RET en segundo plano"** en la pantalla de
  configuración: pide quedar afuera de la optimización de batería del
  fabricante (Xiaomi/Huawei/Samsung matan procesos agresivamente, más
  allá de lo que Android permite normalmente). Esto es la pieza que
  más ayuda contra el cierre en segundo plano — un foreground service
  por sí solo no siempre alcanza en esos fabricantes.
- Ícono adaptativo ya declarado como `roundIcon` también, para
  launchers que piden ícono redondo.

## Videollamada

El dashboard ya trae toda la señalización WebRTC (`SenalizacionLlamadas`
en `ret.py`) — no se construyó ninguna pantalla nueva, la videollamada
vive dentro del mismo WebView del dashboard, igual que en escritorio.
Lo que sí es específico de Android y agregué en `MainActivity.kt`:

- El WebView carga el dashboard por el puerto **HTTPS** (el mismo
  certificado autofirmado que ya arma `ret.py`), no por HTTP — es el
  camino que ya estaba probado en escritorio para cámara/mic.
- `onReceivedSslError`: acepta ese certificado autofirmado, pero
  **solo si la URL es 127.0.0.1/localhost** — nunca para otro host, así
  que esto no abre ningún hueco de seguridad hacia internet real.
- `onPermissionRequest`: el WebView bloquea cámara/mic de cualquier
  página por defecto; acá se autoriza, pero solo si Android ya le dio
  el permiso real al usuario primero (si el usuario rechazó cámara/mic
  en el diálogo del sistema, sigue bloqueado — esto no lo salta).
- Arreglé de paso un bug latente: los reintentos de carga del
  dashboard antes podían recargar la página cada 800ms sin parar
  aunque ya hubiera cargado bien — eso habría cortado una llamada en
  curso. Ahora se detiene apenas `onPageFinished` confirma que cargó.

**No pude probar esto en un WebView real** (sin emulador/dispositivo
aquí) — es la pieza más nueva y la que más conviene revisar primero al
correrlo.

## Lo que SÍ está probado (aquí, en este entorno)

- `ret.iniciar_nodo_app(nombre, puerto, secreto, modo)`: la nueva
  función que arranca el nodo SIN pedir nada por teclado (antes solo
  existía la versión interactiva de `main()`, que no sirve sin
  terminal). La probé con un servidor real: arrancó, respondió
  `/dashboard` con `200 OK`, y se apagó limpio.
- `app_bridge.novedades()`: probado con un nodo real corriendo —
  inserté un mensaje de otro nodo y una alerta SOS directamente en la
  base de datos, y confirmé que aparecen en la respuesta la primera
  vez y NO se repiten en la segunda (así deben verse las notificaciones:
  una vez, no en bucle).
- Todo el resto de `ret.py` (SOS, puentes, mapa, chat, etc.) — igual
  que en las pruebas anteriores.
- Cada `R.id`/`R.layout` que usa el Kotlin existe en el XML correspondiente
  (lo crucé a mano, línea por línea) y todos los XML son válidos.

## Lo que NO pude probar, y por qué

Este entorno donde trabajo no tiene Android SDK, ni emulador, ni
Gradle con acceso a internet para bajar dependencias (Chaquopy, las
librerías de Android, etc. — todo bloqueado a propósito aquí). Así que:

- **Nunca compilé este proyecto.** No sé si hay un typo, una versión de
  Gradle/Chaquopy incompatible, o un import que falta. Es un scaffold
  bien escrito a mano, no un build verificado.
- No probé el foreground service en un teléfono real (si la notificación
  se ve bien, si Android lo mata igual en algún fabricante agresivo con
  batería como Xiaomi/Huawei, etc.)
- No probé permisos en tiempo real (el flujo de "aceptar/rechazar" en
  pantalla).

**Esto significa que vas a necesitar abrir este proyecto en Android
Studio y compilarlo tú (o alguien con ese entorno) para encontrar y
corregir lo que seguro aparezca.** Es lo esperable en un primer intento
de un proyecto de este tamaño — no es que el código esté mal a propósito,
es que compilar Android requiere herramientas que no tengo aquí.

## Pasos para compilarlo (con Android Studio)

1. Instala **Android Studio** (gratis, de Google).
2. Abre la carpeta `RET-Android/` como proyecto.
3. Deja que Gradle sincronice (va a bajar Chaquopy, Kotlin, etc. — la
   primera vez tarda, necesita internet).
4. **Antes de compilar**, edita `MainActivity.kt`:
   - Reemplaza `claveRed = ""` por una clave real, o mejor, cámbialo
     por un formulario de verdad (nombre/puerto/clave/modo) — ahora
     mismo es texto fijo, puesto así para poder probarlo rápido.
5. Conecta un teléfono Android (modo desarrollador + depuración USB) o
   usa un emulador, y dale "Run".
6. Revisa el Logcat si algo falla — lo más probable en un primer
   intento: alguna versión de dependencia que Google actualizó desde
   que escribí esto, o un permiso que falta pedir en tiempo de
   ejecución en tu versión de Android.

## Lo que falta para que esto sea una app "de verdad" (no solo compile)

- Un formulario real en `MainActivity` en vez de la clave fija.
- Guardar nombre/puerto/clave en `SharedPreferences` para que sobreviva
  a que Android mate y reinicie el proceso.
- Probar en varios fabricantes (algunos matan foreground services más
  agresivamente que el Android "puro").
- Bluetooth: `ret.py` usa PyBluez, que es para Linux de escritorio —
  en Android hace falta la API de Bluetooth nativa de Android
  (`BluetoothAdapter`), que es distinta. El Bluetooth de RET **no va
  a funcionar tal cual en este APK** hasta que se adapte esa parte
  específica — es la pieza más grande que queda pendiente.
- Firma de la app (keystore) para publicarla, aunque sea como APK
  directo sin pasar por Play Store.

## Por qué esto sí resuelve lo que pediste ("que no sea un script")

Google Play (y en general, cualquier revisor serio) no acepta un
archivo `.py` suelto — necesita un paquete Android real: un
`AndroidManifest.xml`, un ícono, un `applicationId`, permisos
declarados, un ciclo de vida de Activity/Service correcto. Eso es
exactamente lo que este proyecto agrega alrededor de `ret.py`, sin
reescribir ni una línea de la lógica que ya funciona.
