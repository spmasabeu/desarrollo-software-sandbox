**Caso de referencia**

Un equipo de cuatro ingenieros está desarrollando una plataforma para arrendar canchas
deportivas por hora. Clubes y colegios, en adelante *recintos*, publican sus canchas de fútbol, tenis
o pádel, y las personas, en adelante *jugadores*, las reservan. Los inversionistas financian solo un
producto mínimo viable (MVP) para la Región Metropolitana, con un presupuesto fijo para sueldos,
infraestructura en la nube y servicios externos. El plazo de desarrollo, de 6 meses, es
improrrogable. Si el MVP tiene éxito, se financiará su expansión a otras ciudades.

Cada recinto publica sus canchas indicando deporte, dirección, comuna, coordenadas, fotografías,
si está techada o al aire libre, precio por bloque de 60 minutos y horario semanal de arriendo, con
bloques que comienzan a la hora en punto.

Cualquier persona puede buscar canchas por deporte, comuna, fecha y precio, pero reservar
requiere cuenta. La ficha de cada cancha es la pantalla más visitada. Muestra, para hoy o uno de
los 7 días siguientes, los bloques disponibles (definidos en el paso 1). Si la cancha es al aire libre,
también muestra el pronóstico del tiempo, obtenido de la One Call API de OpenWeather, que cobra
por cada llamada que exceda un cupo diario gratuito.

Los pagos y las devoluciones se procesan con Webpay, de Transbank. Suponga que Webpay
cobra una comisión por cada pago aprobado y por cada devolución, y que la plataforma asume
esas comisiones. El flujo de reserva es el siguiente:

1. Cada reserva es de un solo bloque. Cuando el jugador elige un bloque, la plataforma lo retiene
   a nombre del jugador por 10 minutos, siempre que en ese momento el bloque esté disponible
   (aún no comienza y no está reservado ni retenido) y el jugador no tenga otra retención vigente.
2. El jugador debe pagar con Webpay antes de que venza la retención. Si el pago se aprueba a
   tiempo, se crea la reserva, que no puede cancelarse, y se notifica al jugador y al recinto.
3. Si la retención vence sin un pago aprobado, el bloque se libera. Si Webpay aprueba un pago
   después del vencimiento, no se crea la reserva y se devuelve ese pago.

La plataforma debe conservar indefinidamente el historial de reservas, también llamadas órdenes,
por motivos de auditoría. Para cada reserva se registran jugador, cancha, bloque, monto, fecha y
hora del pago e identificador de la transacción de Webpay. Cada jugador puede consultar todas
sus reservas desde su perfil. Las del último año se consultan con frecuencia. Las más antiguas se
consultan rara vez, y se acepta que tarden algunos segundos en mostrarse. Si un recinto da de
baja una cancha, esta deja de mostrarse en las búsquedas. Ni la baja ni un cambio de precio u
horario afectan a las retenciones, a las reservas ni al historial.

Como punto de partida, el equipo contempla un *frontend*, un *backend* con API y una base de datos,
arquitectura que el diseño puede ampliar si lo justifica.

La plataforma debe estar disponible las 24 horas y enfrenta dos desafíos. El primero es que
existen bloques con más demanda, como los de días hábiles entre las 19:00 y las 22:00, los cuales
suelen ser elegidos por muchos jugadores a la vez al mismo tiempo. Si dos jugadores logran pagar
el mismo bloque, la plataforma debe devolver uno de los pagos y asumir dos comisiones
adicionales: la de ese pago y la de su devolución. El segundo es la entrega de notificaciones: las
del paso 2 deben enviarse dentro del minuto siguiente a la creación de la reserva, aunque el
destinatario no tenga la plataforma abierta.

Según los antecedentes de este caso y los que Ud. pueda deducir, conteste las siguientes preguntas.
Indique las suposiciones que estime necesarias en cada caso, que ayuden a explicar, orientar,
contextualizar y justificar su respuesta.

**Para la siguiente pregunta, en su respuesta contemple las distintas ventajas y desventajas de
sus decisiones (*trade-offs*).** *Tip: en sus respuestas considere el uso de recursos (tiempo, dinero,
recursos de ingeniería, conocimiento técnico y de negocio, entre otros), <u>qué ganan y qué pierden por
cada decisión</u>, dedicación de desarrollo de nuevas y mantención de lo existente. <u>El puntaje
se otorga según la calidad de la argumentación</u>. Si su respuesta solo es un checklist o una lista de
tecnologías o librerías, no se otorgará puntaje. Cada decisión debe tener una argumentación basada
en el contexto del problema.*

1) **(4 puntos)** Usando su conocimiento de *System Design*, realice el diseño del sistema propuesto
que resuelva los requerimientos planteados. Para esto, justifique las decisiones tomadas. Se
recomienda apoyar su respuesta con el uso de diagramas, por lo que puede usar páginas como
<https://excalidraw.com/> para ello.

Si utiliza literatura ajena al curso, agregue la referencia utilizada (libro, publicación, documentación,
página, etc.).

2) **(2 puntos)** Planificando a futuro, esperando que la plataforma escale con el tiempo y mantenga las
órdenes históricas por años, ¿qué mecanismo de almacenamiento de datos emplearía para dar
solución a este requerimiento? Justifique. Considere rendimiento, costo de mantención y costo
monetario.

En caso de que se detecte que las ideas generadas no son de ustedes y es un copy/paste de la IA se
califica con la nota mínima 1.0.

**Puntaje total: 1 (punto base) + 6 puntos (máximo) = 7.**

**Instrucciones de entrega**

1. Guarde sus respuestas en un archivo con extensión <mark><em>.pdf</em></mark>. Este archivo debe llamarse:
<mark><strong>Control-Nombre Apellido.pdf</strong></mark>

<u>Ejemplo:</u> Control-Gonzalo Martinez.pdf

2. Envíe este archivo al correo <mark><strong>gonzalo.martinez@uc.cl</strong></mark> con el asunto:
**[IIC3143] [Control] Nombre Apellido**

<mark><strong>IMPORTANTE: El uso de IA debe ser declarado y debe indicar en que lo uso. Lo puede usar
para iterar en su solución, ortografía y redacción pero lo que no puede pasar es un copy/paste
de la misma o que la redacción sea completamente IA, en este último caso si se detecta que
las ideas y redacción no son suyas se califica con nota mínima al no tener evidencia de que
efectivamente usted realizó su evaluación.</strong></mark>
<mark><u>Cualquier uso de dichas herramientas para revisar la ortografía o la redacción, o para obtener
fuentes de información, debe ser declarado en su respuesta.</u></mark>

<mark>IMPORTANTE: Puede usar toda la literatura que desee, como páginas web, libros o artículos
científicos, entre otros, pero debe citarla correctamente en su documento. En caso contrario,
se considerará plagio y se calificará con nota mínima.</mark>

**Criterios de evaluación**

| Escala de evaluación en cada pregunta | Evaluación |
| --- | --- |
| 0 | La respuesta no guarda relación con el contexto. |
| 0,25 | Respuesta con muchas falencias o sin justificación. |
| 0,50 | Respuesta incompleta. |
| 0,75 | Respuesta con detalles menores. |
| 1 | La respuesta abarca la totalidad de la pregunta y el contexto del problema, con una justificación clara y precisa. |
