# Seguridad de RET

## Modelo de amenazas — qué protege RET, y qué no

Esta sección existe para ser leída con el mismo peso que cualquier
función — no es letra chica.

### Sí protege

- **Contenido de los mensajes en tránsito**: cifrado con la clave
  compartida de la red (Fernet/AES) — alguien que intercepte el
  tráfico WiFi/Bluetooth no puede leer el contenido sin esa clave.
- **Identidad de cada nodo**: cada dispositivo tiene una identidad
  Ed25519 propia. Registros como Nodo Testigo, resultados de Juego
  Encuentro y Vinculación segura están firmados — no se pueden
  falsificar sin la clave privada del nodo que dice haberlos creado.
- **Manipulación de datos firmados**: cualquier alteración de un
  registro firmado después de crearlo invalida la firma y el registro
  se descarta — verificado con pruebas reales durante el desarrollo.
- **Un solo uso de los códigos de vinculación**: no se pueden reusar,
  y tienen límite de intentos fallidos.
- **Saturación básica del servidor**: límite de velocidad por IP y
  tamaño máximo de petición.

### No protege (y ningún sistema P2P sin infraestructura central puede prometerlo)

- **Quién está usando RET en un lugar y momento dado** — alguien con
  equipo para analizar tráfico Bluetooth/WiFi en la misma zona física
  puede notar que hay actividad, aunque no pueda leer el contenido.
  Esto se llama protección de metadatos, y RET no la tiene.
- **Que el GPS reportado sea real** — el mapa, el SOS, las anclas y
  Juego Encuentro confían en la posición que cada dispositivo reporta
  de sí mismo. Un dispositivo comprometido podría mentir sobre su
  propia ubicación.
- **Ataques con múltiples identidades falsas (Sybil)** — no hay
  autoridad central que impida que alguien cree muchos nodos falsos.
- **Auditoría de seguridad independiente** — el código es legible y
  abierto, pero todavía no pasó por una revisión de seguridad externa
  formal (a diferencia de proyectos como Briar, que sí la tienen).
- **Uso bajo condiciones extremas reales** — nunca se probó con
  cientos de nodos simultáneos, ni en escenarios de red hostil
  sostenida.

## Cómo reportar una vulnerabilidad

Si encontrás un problema de seguridad en RET:

1. **No lo publiques primero en un lugar abierto** (issues públicos,
   redes sociales) — dale tiempo al mantenedor de corregirlo.
2. Escribí a: [completar con el correo o canal de contacto del
   desarrollador — obligatorio antes de publicar en una tienda o
   repositorio público]
3. Incluí: qué encontraste, cómo reproducirlo, y qué impacto real
   tiene (¿expone datos? ¿permite falsificar algo? ¿tumba el nodo?).

Como es un proyecto de un solo desarrollador, no hay un equipo de
seguridad ni un tiempo de respuesta garantizado — pero cada reporte se
toma en serio.

## Buenas prácticas para quien instala RET

- Usá una clave de red compartida larga y que no compartas fuera de
  tu grupo de confianza — es la única barrera de entrada a tu malla.
- Los puentes de internet (opcionales, apagados por defecto) amplían
  la superficie de exposición — actívalos solo si de verdad los
  necesitás, y con alguien de confianza del otro lado.
- Revisá periódicamente la sección "Dispositivos vinculados" y revocá
  los que ya no reconozcas.
