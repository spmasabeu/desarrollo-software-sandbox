# Control 1 — borrador para discutir

Material generado con asistencia de IA para iterar el diseño. No es la respuesta final del control. Las decisiones pendientes deben resolverse y argumentarse con tus palabras; la entrega debe declarar el uso real de IA.

## Abrir los diagramas

1. [Arquitectura inicial](01-arquitectura.excalidraw): componentes y responsabilidades.
2. [Reserva y pago](02-reserva-y-pago.excalidraw): transacciones cortas, resultados y tareas posteriores.

Abrir con la extensión Excalidraw de VS Code. Azul: aplicación; verde: persistencia y consistencia; naranja: dependencias externas o excepciones; gris: contexto; amarillo: supuestos pendientes. Las cajas son editables y sus textos están agrupados. Las flechas muestran interacciones, no todos los mensajes de respuesta.

## Decisiones iniciales y lo que cuestan

| Propuesta | Qué resuelve | Qué cuesta o limita |
| --- | --- | --- |
| Un backend modular, replicado | Un mismo código para cuatro ingenieros; continuidad ante caída de una instancia | Las funcionalidades se despliegan juntas; dos instancias cuestan más que una |
| PostgreSQL gestionado con standby y backups | Consistencia de reservas y recuperación | Mayor costo fijo; el failover puede interrumpir temporalmente el servicio. Presupuestar y definir RPO/RTO, sin prometer cero caídas |
| Locks breves por jugador y bloque | Coordinar una sola retención por jugador y evitar disputar un bloque después de cobrar | Esperas en bloques populares; mismo orden de locks, timeout y reintento controlado |
| Tareas durables en la misma base | Crear orden y avisos atómicamente; recuperar trabajo tras caídas | Comparte recursos con las reservas; requiere reintentos, índices y limpieza de tareas terminadas |
| Caché de clima en PostgreSQL | Compartir resultados entre instancias sin incorporar Redis inicialmente | Más carga SQL; medir antes de separar la caché |
| Fotos en almacenamiento de objetos | Evitar archivos distintos en cada réplica del backend | Costo de almacenamiento y transferencia |

Los dos ejecutores de tareas pueden acompañar a los dos despliegues del backend; no son microservicios ni requieren necesariamente dos máquinas adicionales. Reclaman tareas con un plazo corto, liberan el lock antes de contactar al proveedor y registran el resultado. Si mueren, otro ejecutor recupera la tarea. Procesar al menos una vez exige deduplicación; no implica entrega exactamente una vez.

## Reglas que el dibujo resume

- Crear bloques identificados de forma única por cancha e instante de inicio. Consultar disponibilidad en el primario y volver a validarla dentro de la transacción de retención. Una ficha abierta no garantiza disponibilidad.
- Serializar también por jugador: bloquear sólo el bloque no impide que un jugador retenga dos canchas distintas. Todas las rutas de reserva, vencimiento y cambios del recinto deben respetar la coordinación. Los locks duran la transacción, no los diez minutos.
- Persistir `expires_at` y consultar el reloj de la base después de esperar locks. Mostrar horarios en `America/Santiago` y almacenar instantes inequívocos. Definir tratamiento de cambios de hora y del caso en que el bloque comienza durante la retención; no acortar silenciosamente los diez minutos exigidos.
- Guardar la copia del precio y bloque al retener; luego la orden conserva jugador, cancha, bloque, monto, fecha/hora del pago e identificador Webpay. Baja lógica y cambios de horario afectan ofertas nuevas, no compromisos existentes. Prohibir cancelación de órdenes confirmadas.
- Persistir el intento antes de llamar a Webpay y validar identidad de la compra y monto en servidor. Deduplicar confirmaciones y devoluciones. Un timeout deja un resultado desconocido, no demuestra rechazo.
- Correo a contactos verificados del jugador y recinto, con estado independiente por destinatario. Propuesta inicial: revisar pendientes cada 5 segundos, timeouts cortos, reintentos y alerta antes de agotar el minuto. El ritmo se debe validar con carga y fallas simuladas.
- Pronóstico sólo para canchas al aire libre: compartir por coordenadas del recinto, almacenar todos los días devueltos y permitir una sola actualización por clave. TTL propuesto de 30 minutos, ajustable según frescura y presupuesto. Con N ubicaciones continuamente consultadas supone aproximadamente 48N llamadas diarias; no implica mantenerse dentro del cupo gratuito. Si falla la API, mostrar antigüedad del último dato o indisponibilidad sin impedir reservar.

## Dos decisiones que aún no están cerradas

**Qué instante determina un pago a tiempo.** El enunciado habla de aprobación antes del vencimiento, no de cuándo llega el navegador. Hay que confirmar qué dato del proveedor acredita esa aprobación; no asumir que `transaction_date` tiene esa semántica. El dibujo propone conciliar resultados inciertos antes de reasignar el bloque. Esto puede retrasar su liberación y entra en tensión con la liberación inmediata: es una decisión abierta que debe explicitarse, no una solución completa a ese requisito. Una notificación tardía de un pago aprobado a tiempo no equivale a una aprobación tardía.

**Qué significa notificar dentro de un minuto.** El borrador supone envío aceptado por el proveedor, con contactos válidos y proveedor operativo. No garantiza recepción en la bandeja del usuario. Si el requisito exige entrega efectiva incluso durante una caída del proveedor, habrá que discutir canal alternativo y sus costos; los reintentos por sí solos no aseguran el plazo.

## Historial: evolución para la pregunta 2

Comenzar con órdenes en SQL e índice por jugador/fecha, consultas paginadas y sin eliminación por antigüedad. A medida que el volumen lo justifique, evaluar particiones temporales y archivo de órdenes antiguas en almacenamiento de objetos, conservando un índice pequeño por jugador y periodo. Comparar esa complejidad con mantener todo en PostgreSQL antes de decidir.

El archivo debe permitir lectura directa en segundos: no una clase que requiera horas de restauración. Copiar, verificar integridad y cobertura, publicar el índice y sólo después retirar la copia de la base activa. Proteger permisos y retención, preservar el historial indefinidamente y considerar costos de recuperación, solicitudes y operación. El perfil debe consultar ambos niveles sin perder ni duplicar órdenes durante la migración.

## Referencias de trabajo

- Enunciado: [control-1.md](../control-1.md).
- Diapositivas del curso, *System Design, clases 1–5*: negocio y trade-offs (pp. 3–12), réplicas y balanceo (26–43), rate limiting (44–51), caché y costos (61–79), comunicación (91–106), connection pools y WAL (137–143), historial y baja lógica (146–147), índices y concurrencia (155–170). Numeración de páginas del PDF.
- Notas de Obsidian del 17 de agosto: persistencia de correos; del 26 de agosto: concurrencia; del 2 de septiembre: supuestos y acción/reacción frente a fallas.
- [PostgreSQL: Explicit Locking](https://www.postgresql.org/docs/current/explicit-locking.html): comportamiento y duración de locks de fila. Sustenta la coordinación propuesta, no demuestra por sí solo todos los invariantes del sistema.
- [Transbank: documentación Webpay](https://github.com/TransbankDevelopers/transbank-developers-docs/blob/master/documentacion/webpay/README.md): confirmación, consulta de estado y reversa/anulación. La interpretación temporal sigue pendiente. Las comisiones se toman del enunciado.
- [OpenWeather: One Call API 3.0](https://openweathermap.org/api/one-call-3): ejemplo de consulta por coordenadas y pronóstico diario de ocho días. El diseño no fija la versión ni asume precios actuales para el control.

Fuentes externas consultadas el 23-09-2026. Citar las que se usen en la entrega final.
