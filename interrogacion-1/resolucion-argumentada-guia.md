# Interrogación 2026-02: resolución argumentada y guía de estudio

Documento de trabajo para contrastar conceptos, desarrollar tu juicio y escribir tus propias respuestas. No es un texto listo para entregar: el enunciado permite analizar información con IA, pero exige elaboración propia y declarar las herramientas utilizadas. Cambiar palabras de esta guía no sustituye desarrollar una posición propia.

## 0. Cómo usar y verificar esta guía

Cada respuesta separa conceptos de clase, datos del caso y decisiones propuestas. Las referencias indican dónde verificar el fundamento; una propuesta de esta guía no se presenta como una instrucción del profesor.

**Numeración:** “diapositiva” significa posición dentro del PPTX, contando la portada como 1. Algunas diapositivas muestran `‹#›` en vez de un número; usa el panel de miniaturas. Para el PDF se indica página del archivo, contando desde 1, que puede diferir del número impreso.

### Catálogo de fuentes locales

| Código | Archivo | Localizadores relevantes |
|---|---|---|
| E | [Desarrollo de Software Interrogación 2026-02.md](<Desarrollo de Software Interrogación 2026-02.md>) | Secciones “Caso de referencia”, “Arquitectura de la plataforma”, “Supuestos”, “Preguntas” e “Instrucciones de entrega”. |
| PM | [05- Project Managment - Metodologias de Proyecto.pptx](<05- Project Managment - Metodologias de Proyecto.pptx>) | 28–29: Kanban/WIP; 32: spike; 41 y 44–50: métricas. |
| T | [06-Testing.pptx](<06-Testing.pptx>) | 8: costos manual/automático; 13–18: comportamiento; 24–39: unitarias y aislamiento; 42: fixtures; 47–55: dobles; 59–71: integración; 72–73: E2E; 79 y 82–86: prácticas, costos y patrones. |
| F | [07- Testing Frontend.pptx](<07- Testing Frontend.pptx>) | 27–31: comportamiento de vistas; 34–37: Cypress e interceptación; 39–46: Testing Library y consultas. |
| IA | [Aplicacion de IA Engineering en el SDLC…pdf](<Aplicacion de IA Engineering en el SDLC (Incluye CI_CD, Métricas de gestión proyecto, Inicio testing) .pdf>) | Páginas del archivo 7 y 10–13: prompting, ejemplos y costo; 29–30: instrucciones específicas y estructuración del prompt. |

No se consultaron fuentes web, documentación de proveedores ni bibliografía externa para esta guía. Las condiciones de los proveedores se toman de E, aunque un producto real pudiera operar de otra manera. Las referencias al curso se mantienen por trazabilidad, pese a que E no exige citarlas en la entrega.

### Alcance y advertencias de lectura

- Son tres páginas, cuatro controllers y **seis services**: BusquedaService, DisponibilidadService, ClimaService, RetencionService, PagoService y NotificacionService. En el resumen previo de la conversación se escribió inicialmente “siete”; ese conteo era incorrecto.
- No se rediseñan concurrencia, escalabilidad, protocolos ni modelo de datos. Sí se permite modificar organización interna/configuración para facilitar pruebas. **Fuente:** E, “Supuestos”, primer y tercer puntos.
- Registro, inicio de sesión y publicación de canchas quedan fuera. Se usan jugador autenticado y canchas de prueba preexistentes. **Fuente:** E, últimos dos supuestos.
- E anuncia siete preguntas, pero contiene seis, cuyos puntajes suman 6 más el punto base. P4 anuncia tres partes, pero muestra solo a) y b). Esta guía cubre todo el contenido presente, sin inventar apartados faltantes.

## 1. Pregunta 1: métricas de Kanban

### 1.a. Definiciones y convención adoptada

**Lead time:** tiempo transcurrido desde que una tarjeta entra al backlog del proyecto hasta que llega a DONE. Aquí comienza al ingresar a **TO DO**. Incluye trabajo activo y todas las esperas posteriores.

**Touch time:** suma del tiempo en que la tarjeta recibe trabajo activo. Aquí cuentan **DOING**, **SPIKES** y **CODE REVIEW**, según la ruta de cada tarjeta. Revisar código es trabajo activo; esperar que alguien lo revise no lo es.

**Cycle time:** tiempo transcurrido desde que comienza el trabajo en la tarjeta hasta que llega a DONE. Aquí comienza al ingresar a **DOING**, o a **SPIKES** para investigación. Incluye la espera de revisión que ocurra después de ese inicio.

**Idle time o tiempo de ocio:** tiempo sin trabajo activo dentro del intervalo que se está midiendo. Para dar una respuesta inequívoca, **en esta resolución se informa el ocio dentro del cycle time**:

`Idle time del ciclo = Cycle time − Touch time`.

En este tablero corresponde a WAITING FOR CODE REVIEW. No se está afirmando que las esperas previas al inicio del trabajo desaparezcan: están incluidas en lead time, pero fuera del ciclo elegido.

**Justificación de los límites:** TO DO SPRINT significa priorización para la iteración, no necesariamente trabajo iniciado. Por eso se deja fuera del ciclo. Para un spike, comenzar el ciclo en DOING sería incorrecto: nunca pasa por esa columna. El término es DONE para ambas rutas, porque E establece que solo entonces una tarjeta está completada.

**Evidencia:** PM 41 y 45 definen lead desde backlog hasta término; PM 47 identifica las columnas de trabajo efectivo; PM 48 incluye esperas dentro del ciclo; PM 44 establece `Touch ≤ Cycle ≤ Lead`. E, “Tablero Kanban”, define las columnas y rutas; P4 permite cualquier criterio coherente y explicado, con inicios distintos para lead y cycle.

**Matiz sobre la fuente:** PM 48 expresa el inicio como asignación. La equivalencia entre asignación/inicio y DOING o SPIKES es nuestra convención operacional para este tablero, permitida por P4. La presentación no fija estos nombres particulares de columnas. La fórmula de ocio se deduce de separar trabajo y espera dentro del intervalo declarado; no se atribuye como cita textual a una diapositiva.

### 1.b. Diferencia entre lead y cycle

Lead mide la espera total de una solicitud desde que entra al proyecto; cycle mide cuánto tarda su ejecución una vez iniciado el trabajo. Una tarjeta puede permanecer mucho tiempo en backlog y luego resolverse rápido: tendría lead alto y cycle bajo.

Para las tarjetas ordinarias de este caso:

`Lead = TO DO + TO DO SPRINT + Cycle`.

Las dos métricas sirven para preguntas distintas: cuánto espera quien solicita el trabajo y cuánto tarda el flujo una vez iniciado. No deben comenzar en la misma columna en esta evaluación.

**Evidencia:** PM 45 y 48; E, condición final de P4.

**Para elaborar tu respuesta:** decide si entiendes el inicio del ciclo como priorización o como comienzo efectivo. Ambas lecturas pueden defenderse aquí, pero debes mantener la elegida al calcular P4.

## 2. Pregunta 2: qué es un spike

Un **spike** es una tarea de investigación o experimentación acotada para reducir una incertidumbre que impide decidir, estimar o implementar con suficiente conocimiento. Tiene un **timebox**, un criterio de término y un resultado accionable, como una conclusión documentada o una demostración que permita crear o ajustar tarjetas posteriores.

**Justificación:** la finalidad inmediata es adquirir conocimiento útil para decidir, no prometer una funcionalidad lista para producción. El límite de tiempo evita investigar indefinidamente y el resultado accionable conecta la investigación con el avance del proyecto.

**Evidencia:** PM, diapositiva 32: investigación/mini demo, timebox, criterio de finalización y resultado accionable sobre tarjetas.

## 3. Pregunta 3: spike aplicado al caso

**Único aspecto propuesto:** investigar la viabilidad y las condiciones de integrar Webpay para dividir el pago de una reserva entre varios jugadores.

**Por qué requiere spike:** E identifica esta petición como próxima funcionalidad y dice que el equipo nunca la ha implementado. No basta con estimarla como una integración conocida: primero debe aclararse si el mecanismo disponible permite el comportamiento esperado y qué implica coordinar aprobaciones parciales y vencimientos. Esto reduce incertidumbre técnica y de negocio antes de comprometer una entrega.

**Propuesta concreta, no dato del enunciado:** timebox de dos días hábiles de una persona. Revisar las capacidades del proveedor y, si procede, realizar una demostración mínima en su ambiente de integración. Terminar con una conclusión sobre viabilidad, restricciones y preguntas pendientes, más tarjetas implementables o una recomendación de ajustar la funcionalidad.

El spike puede concluir que la alternativa investigada no es viable: generar esa evidencia también cumple su finalidad. No se afirma que Webpay soporte actualmente un producto específico de pagos divididos; esa es precisamente la incertidumbre por investigar.

**Trade-off:** consume tiempo que podría usarse en funcionalidades conocidas, pero evita una estimación sin fundamento y retrabajo costoso. Dos días es un límite propuesto para este equipo, no una duración exigida por clase.

**Evidencia:** E, “Producto”, párrafo sobre próxima funcionalidad; PM 32; E, P3, exclusión expresa de notificaciones push. El ambiente gratuito de Webpay está en tabla 2 de E.

**Para elaborar tu respuesta:** explica qué decisión concreta podrá tomar el equipo al terminar. No agregues un segundo aspecto: E dice que solo se corrige el primero.

## 4. Pregunta 4: cálculos

### 4.a. Métricas por tarjeta

Se mantiene exactamente la convención de P1. Días hábiles; las permanencias en columnas activas cuentan íntegramente como trabajo, porque así lo indica P4. “No aplica” no aporta tiempo a la suma.

| Columna | Tarjeta 1: logs | Tarjeta 2: spike | Tarjeta 3: caché | Tratamiento |
|---|---:|---:|---:|---|
| TO DO | 14 | 20 | 11 | Espera previa al ciclo; cuenta en lead. |
| TO DO SPRINT | 4 | 3 | 2 | Espera previa al ciclo; cuenta en lead. |
| SPIKES | No aplica | 3 | No aplica | Trabajo activo; inicia el ciclo del spike. |
| DOING | 5 | No aplica | 4 | Trabajo activo; inicia el ciclo ordinario. |
| WAITING FOR CODE REVIEW | 3 | No aplica | 3 | Espera dentro del ciclo. |
| CODE REVIEW | 2 | No aplica | 2 | Trabajo activo dentro del ciclo. |

| Tarjeta | Lead time | Touch time | Cycle time | Idle time del ciclo |
|---|---:|---:|---:|---:|
| 1 | `14+4+5+3+2 = 28` | `5+2 = 7` | `5+3+2 = 10` | `10−7 = 3` |
| 2 | `20+3+3 = 26` | `3` | `3` | `3−3 = 0` |
| 3 | `11+2+4+3+2 = 22` | `4+2 = 6` | `4+3+2 = 9` | `9−6 = 3` |

El spike tiene ciclo de tres días y ocio de ciclo cero porque, según los datos y el supuesto de P4, recibió trabajo durante toda su permanencia en SPIKES. No significa que no esperara antes de empezar.

### 4.b. Promedios

Se consideran las tres tarjetas completadas, incluido el spike; el denominador es 3.

| Métrica | Operación | Promedio, días hábiles |
|---|---|---:|
| Lead time | `(28+26+22)/3` | **25,33** |
| Touch time | `(7+3+6)/3` | **5,33** |
| Cycle time | `(10+3+9)/3` | **7,33** |
| Idle time del ciclo | `(3+0+3)/3` | **2,00** |

**Controles de coherencia:** en cada tarjeta se cumple `Touch ≤ Cycle ≤ Lead` y `Cycle = Touch + Idle del ciclo`. El promedio de ciclo no se limita a los días de DOING: también incluye la espera y la revisión.

**Evidencia:** E, tabla 1 y P4; fundamentos de PM 44, 47 y 48.

### Convenciones alternativas: no mezclar con la respuesta principal

Si defines ocio sobre todo el lead, entonces `Idle total = Lead − Touch`: 21, 23 y 16 días; promedio **20,00**. Es otra ventana de observación. No escribas esa definición en P1 y luego entregues 3, 0 y 3 en P4.

Si decides iniciar cycle en TO DO SPRINT, sus valores serían 14, 6 y 11, promedio **10,33**; el ocio dentro de ese ciclo sería 7, 3 y 5, promedio **5,00**. Lead y touch no cambian. Este criterio requiere justificar que el compromiso/asignación comienza al priorizar para la iteración.

**Decisión para esta guía:** utilizar únicamente la tabla principal al desarrollar la respuesta; las alternativas sirven para entender por qué el enunciado exige explicitar límites.

## 5. Pregunta 5: reparto de 25 % por tipo

### 5.a. Patrón o antipatrón

La propuesta se aproxima al antipatrón **Cupcake** mostrado en clase: asigna un peso comparable a las capas automatizadas y mantiene una porción importante de pruebas manuales, sin una base claramente dominante de verificaciones rápidas.

**Precisión:** el enunciado habla de tiempo, mientras que la figura es una representación conceptual de la distribución de pruebas. Un 25 % de tiempo por categoría no demuestra igual cantidad de tests ni una coincidencia geométrica exacta. La identificación se sostiene por el reparto de esfuerzo plano y la falta de priorización según costo/riesgo; no porque se pueda deducir el número de pruebas. Tampoco hay datos para afirmar que domina E2E como en un Ice Cream Cone.

**Evidencia:** T 85, titulada “Antipatrones: Ice Cream Cone y Cupcake”, especialmente el esquema Cupcake. Comparar con la pirámide de T 84 y el trofeo de T 86.

### 5.b. Evaluación y trade-offs

**Qué gana el equipo:** reserva explícitamente espacio a cuatro enfoques complementarios. Las unitarias permiten localizar fallas en reglas; la integración descubre incompatibilidades entre componentes; E2E verifica el recorrido visible; la exploración manual puede encontrar problemas de usabilidad o situaciones no previstas. Un acuerdo simple también ayuda a comenzar cuando hoy no existe automatización.

**Qué pierde:** la igualdad de porcentajes no responde a los riesgos particulares ni al costo de mantener cada tipo. Un cuarto del tiempo en E2E puede rendir pocas comprobaciones y generar diagnósticos lentos. Otro cuarto manual debe repetirse para detectar regresiones. Con cuatro ingenieros y tres meses restantes, ese esfuerzo compite con cubrir pagos, vencimientos y caché mediante pruebas más rápidas y repetibles.

Las pruebas que contacten servicios reales añaden además solicitudes, correos, logs o comisiones según la dependencia. El costo no proviene del nombre “E2E” por sí solo, sino de qué servicios alcanza cada ejecución. Dobles de prueba pueden reducirlo, a cambio de verificar menos integración real.

**Juicio propuesto:** no adoptaría el reparto fijo como estrategia para este MVP. Reconozco el valor de los cuatro enfoques, pero el esfuerzo debe seguir el riesgo, la velocidad de feedback y el costo de repetición. No eliminaría la exploración ni E2E: cuestionaría que reciban automáticamente la misma proporción que las capas inferiores.

**Evidencia:** E, equipo/plazo/presupuesto y tabla 2; T 8, costo acumulativo manual e inversión inicial automatizada; T 82, lentitud, flakiness y complejidad de integración; T 83–86, recursos limitados y patrones.

**Para elaborar tu respuesta:** argumenta por qué una integración real podría justificar más esfuerzo que una unitaria en un caso concreto. No conviertas la pirámide en una regla universal ni repitas aquí toda la estrategia de P6.

## 6. Pregunta 6: estrategia propuesta, versión depurada

Esta sección presenta una propuesta para estudiar y discutir; los porcentajes, ambientes y frecuencias son decisiones justificadas para el caso, no exigencias de las diapositivas. En esta sección, **aserción** significa una comprobación programada que hace fallar el test si el resultado no es el esperado; **PR** es una solicitud de incorporación de cambios y **merge**, su integración a la rama principal. **Pipeline** es la secuencia automática de pruebas, construcción y despliegue de GitHub Actions. Referencias abreviadas: **E**, enunciado; **T**, PPT “06-Testing”; **F**, PPT “07- Testing Frontend”. La numeración corresponde a la posición de la diapositiva contando la portada.

### 6.a. Distribución del esfuerzo y patrón

Propondría una estrategia orientada a la **pirámide de pruebas**, distribuyendo inicialmente el tiempo de pruebas en **50 % unitarias, 35 % integración, 10 % E2E y 5 % manuales exploratorias**. Son porcentajes del esfuerzo de pruebas, incluida su preparación y mantenimiento, no del tiempo total del proyecto ni de la cantidad de tests.

La mayor inversión en unitarias permite comprobar muchas combinaciones de disponibilidad, retenciones y pagos con rapidez y sin consumir servicios externos. La integración recibe una parte importante porque las unitarias aisladas no detectan todos los errores de comunicación entre capas, almacenamiento y proveedores. Mantendría pocos E2E por su mayor costo de preparación y mantenimiento, y revisión manual de mensajes, navegación y presentación en las pantallas modificadas, para detectar problemas que los tests no anticipen.

Los cuatro ingenieros escribirían pruebas junto al código nuevo y añadirían tests al código existente por etapas, comenzando por pagos, retenciones, disponibilidad y Ficha de cancha, la página más visitada. Esto resta capacidad inicial a funcionalidades nuevas, pero reduce la repetición manual y el riesgo de regresiones. Revisaría la distribución al cierre de cada iteración según errores encontrados y horas necesarias para mantener los tests; los porcentajes no son una regla rígida.

**Respaldo:** T 24, rapidez y aislamiento de unitarias; T 82, dificultades de integración; T 83–84, recursos limitados y pirámide. E, restricciones de equipo/plazo y descripción de Ficha de cancha. Los porcentajes son una propuesta propia.

### 6.b. Cómo probar páginas, endpoints, services y proveedores

#### Enfoques y ambientes

Usaría **London/Mockist en unitarias**: ejecutaría el código del service y reemplazaría sus consultas a otros services, almacenamiento o proveedores por respuestas elegidas en el test, para provocar éxitos y fallas. Un **stub** devuelve datos preparados; un **mock/spy** permite comprobar llamadas y argumentos. En integración seguiría **Detroit/Classical**, ejecutando juntos componentes y almacenamiento reales de prueba, sustituyendo proveedores cuando necesite controlar errores o evitar cobros.

Distinguiría los siguientes grupos de tests automatizados, ejecutados con datos exclusivos de prueba:

- **Unitarias backend:** service real; otros services, almacenamiento y proveedores sustituidos. No se conectan a PostgreSQL, Redis, AWS ni proveedores reales.
- **Integración backend sin proveedores reales:** FastAPI, controllers, services y PostgreSQL/Redis reales de prueba; Webpay, OpenWeather, SQS y SES sustituidos cuando intervengan. PostgreSQL/Redis se ejecutan en el computador del ingeniero o en el runner de CI, sin utilizar RDS ni ElastiCache. No hay llamadas a proveedores reales.
- **Integración backend con proveedores reales:** aplicación desplegada en el ambiente AWS de pruebas, con RDS, ElastiCache, SQS, SES y Webpay de integración reales. OpenWeather se sustituye en esta suite y se verifica separadamente con llamadas reales acotadas, según 6.d.
- **Vistas frontend:** componentes React reales y respuestas HTTP del backend simuladas. El navegador, backend y almacenamiento se conectan realmente en el E2E de 6.c.

En todos los grupos se sustituye Splunk por su mock del SDK; solo se observa localmente el intento de registrar errores. Ningún test envía logs a Splunk real.

El ambiente AWS compartido estará separado de producción. Aceptaría su costo fijo para verificar permisos, conexiones y configuración que PostgreSQL/Redis fuera de AWS no comprueban; limitar ejecuciones reduce cobros por uso, no elimina ese costo fijo. También presupuestaría horas de preparación/mantenimiento; los minutos de Actions están incluidos según E. Configuraría sustitutos de clientes y de la hora consultada por nuestro código para reproducir vencimientos. Las fechas de prueba usarán hora de Chile continental. Antes de cada caso se insertan los datos indicados y se eliminan residuos de ejecuciones anteriores; nunca se usan datos productivos.

#### Páginas: tests automatizados de comportamiento

Escribiría **tests automatizados con React Testing Library**, ejecutables en el computador del ingeniero antes del PR y en el pipeline frontend al abrir/actualizar PR y hacer merge, antes de generar el paquete desplegable y de desplegar. El test simula acciones del jugador y respuestas HTTP, ejecuta los componentes reales y usa aserciones para comprobar la pantalla. Estas pruebas no son manuales: la exploración humana complementaria se describe en 6.e. La comunicación efectiva entre navegador y backend se verifica en el recorrido E2E de 6.c.

- **Buscar canchas.** Los tests automatizados ingresarán, por ejemplo, deporte tenis y una comuna; simularán pulsar buscar y harán responder al backend simulado con una cancha de tenis de esa comuna. Comprobarán que la solicitud incluya esos filtros y que la cancha aparezca en pantalla; otros casos simularán lista vacía o error y verificarán sus mensajes. Así se verifica la interacción sin atribuirle cobertura de la búsqueda real del backend.

- **Ficha de cancha.** Los tests automatizados harán responder al backend simulado con bloques 18:00/19:00 para un día y 20:00/21:00 para el siguiente. Al seleccionar el segundo día, comprobarán que se solicite esa fecha y se muestren solo 20:00/21:00. Simularán selección de bloque y retención aprobada/rechazada, verificando navegación a pago o error. También comprobarán cancha techada sin clima y exterior con/sin pronóstico, manteniendo la ficha utilizable. Es prioritaria por ser la página más visitada.

- **Pago.** Los tests automatizados iniciarán la vista con un bloque retenido por el jugador y un vencimiento definido en el test; simularán pulsar pagar y recibir aprobación, rechazo, falta de respuesta o vencimiento. Las aserciones comprobarán confirmación, error y reintento mientras siga vigente la retención. Esto verifica presentación del resultado, sin realizar pagos reales a Webpay.

**Respaldo:** F 27–31 y 39–46, comportamiento de vistas, backend simulado y Testing Library; E, páginas y reglas.

#### Endpoints: tests automatizados de integración HTTP

Escribiría tests ejecutables en el computador del ingeniero antes del PR y en el pipeline backend al abrir/actualizar PR y hacer merge, antes de generar el paquete desplegable y de desplegar. Enviarán solicitudes a FastAPI con controllers, services y PostgreSQL/Redis reales de prueba (**criterio Detroit**) y comprobarán contenido de la respuesta HTTP y creación, eliminación o conservación de registros en PostgreSQL/Redis. Las reglas detalladas se cubren en los services, evitando repetir todos sus casos aquí.

- **Consultas GET — `/api/v1/canchas` y `/api/v1/canchas/{id}/bloques`.** Los tests crearán previamente canchas de tenis/fútbol, horarios, reservas y retenciones en la base y Redis de prueba. Buscar tenis debe devolver solo las canchas de ese deporte y respetar las fechas permitidas; repetir la búsqueda debe recuperar el resultado guardado en caché. Consultar bloques debe excluir los que el test dejó reservados, retenidos o ya iniciados. Simularán OpenWeather para verificar que una cancha exterior responda con pronóstico cuando esté disponible y siga funcionando sin él cuando falle. Así se comprueba que los filtros de la solicitud produzcan la respuesta esperada al atravesar controller, service y almacenamiento, sin consumir cuota meteorológica.

- **Operaciones POST — `/api/v1/retenciones` y `/api/v1/pagos`.** Con un jugador de prueba autenticado, los tests solicitarán un bloque futuro libre y comprobarán su retención en Redis a nombre del jugador. Repetirán con un bloque ocupado o un jugador con otra retención y verificarán rechazo sin nueva escritura. Para pago aprobado verificarán reserva persistida, retención eliminada y mensaje enviado al sustituto de SQS; sin retención inicial, error sin llamar a Webpay. El test hará que el sustituto de Webpay responda aprobado/rechazado y que el de SQS conserve los mensajes publicados (**aislamiento London**) para evitar cobros y reproducir fallas. Las variantes de vencimiento/rechazo se detallan en los services y la integración real con proveedores se verifica aparte.

**Controllers:** omitiría unitarias de mera delegación, pues estas pruebas HTTP aportan mayor confianza. Si contienen validaciones o transformaciones propias, sí comprobaría sus casos.

**Respaldo:** T 13–18 y 59–71, comportamiento e integración HTTP; E, arquitectura, reglas y supuesto de sesión.

#### Services: tests unitarios automatizados London, agrupados por responsabilidad

Los siguientes tests ejecutarán cada service por separado, sustituyendo sus consultas a almacenamiento, otros services y proveedores. Correrán en el computador del ingeniero antes del PR y en CI backend al abrir/actualizar PR y hacer merge, antes de generar/desplegar el paquete. Se comprobarán respuestas e intentos de escritura/envío; los datos realmente guardados y la comunicación real entre componentes se comprobarán en las integraciones de endpoints y proveedores ya descritas, sin duplicar esas suites.

- **Búsqueda, disponibilidad y clima — BusquedaService, DisponibilidadService y ClimaService.** Los tests simularán almacenamiento, consultas entre services y OpenWeather. Comprobarán filtros, rango de hoy a hoy+7 en hora de Chile continental y exclusión de bloques iniciados, reservados o retenidos. Para las cachés verificarán que un resultado vigente se devuelva sin consultar su fuente y que, al faltar o vencer, se consulte y solicite guardarlo cinco minutos para búsquedas y una hora para clima. Por ejemplo, un pronóstico de hace 30 minutos debe devolverse sin llamar a OpenWeather; uno de una hora debe renovarse. También comprobarán que las canchas techadas no soliciten pronóstico y que la falta de respuesta meteorológica permita continuar sin él. Así se cubren las decisiones de consulta sin consumir proveedores.

- **Retención y pago — RetencionService y PagoService.** Los tests simularán disponibilidad, Redis, PostgreSQL, Webpay y SQS. Comprobarán que solo se concedan retenciones para bloques disponibles y jugadores sin otra vigente, solicitando guardarlas a su nombre por diez minutos y sin consultar clima. Para pagos verificarán: sin retención inicial, error sin llamar a Webpay; aprobado con retención vigente, solicitar creación de reserva, eliminación de retención y publicación de notificación, aunque haya comenzado el bloque; rechazo o falta de respuesta, no reservar ni eliminar la retención, permitiendo reintento hasta vencer; aprobación tras vencimiento, solicitar devolución sin reservar. El test controlará la hora y las respuestas de Redis antes/después del pago para reproducir esos límites sin esperas ni cobros.

- **Notificaciones — NotificacionService.** Los tests entregarán un mensaje de reserva con jugador, recinto y destinatarios definidos por la prueba y simularán aceptaciones/rechazos de SES. Comprobarán los dos destinatarios, el reintento inmediato únicamente del correo fallido hasta cuatro envíos totales contando el primero y la solicitud de registro de error al agotarlos. Un spy observará esa llamada sin enviar logs a Splunk. El plazo de un minuto se comprobará adicionalmente en la integración real con SQS/SES, midiendo desde el registro de la reserva hasta la aceptación de ambos correos, como se indica en proveedores.

**Respaldo:** E, reglas y responsabilidades de los seis services; T 28–39, London/Detroit; T 32, casos negativos y bordes; T 47–55, dobles y comprobación de llamadas. La agrupación organiza la explicación: no significa probar juntos los services en las unitarias.

#### Proveedores: qué probar realmente y qué simular

Los siguientes son **tests automatizados de nuestra integración con proveedores**. Las suites previas a construir el paquete, ejecutadas en PR/merge, simulan proveedores; al hacer merge también se ejecutan pruebas reales AWS/Webpay después de desplegar a pruebas y antes de producción. OpenWeather tiene la ejecución separada de 6.d. Aquí **Detroit** indica conexión real de prueba y **London**, sustitución del proveedor para controlar sus respuestas.

- **RDS y ElastiCache — Detroit selectivo, aceptando cobros AWS.** Guardar/consultar una reserva y crear/consultar/expirar una retención mediante nuestra aplicación permite verificar datos, permisos y conexión reales. Aceptaría ese costo por su impacto directo en reservas y disponibilidad. Para caché comprobaría también escritura y duración del pronóstico. Las pruebas frecuentes usan PostgreSQL/Redis fuera de AWS, que no verifican configuración administrada; las unitarias London simulan ambos almacenamientos.

- **SQS y SES.** Un test automatizado con ambos servicios reales en AWS de pruebas (**Detroit**) creará una reserva mediante un pago aprobado: PagoService publicará el mensaje en SQS y NotificacionService lo leerá y enviará los correos a dos direcciones de prueba. Se comprobará que SES acepte ambos dentro de 60 segundos desde el registro de la reserva. Aceptaría esos cobros para verificar el proceso completo. Para errores, otros tests simularán rechazo de SES (**London**) y verificarán reintento solo del correo fallido, hasta cuatro envíos totales. La simulación controla fallas sin pagar reenvíos, pero no verifica conexión ni permisos AWS; eso lo comprueba el test real.

- **Webpay — Detroit en su ambiente de integración; London para escenarios controlados.** Los tests realizarán pagos y devoluciones con credenciales/tarjetas de integración y verificarán las respuestas. Es crítico para reservas y, según E, no cobra comisiones ni cuenta como reemplazo. Timeout y aprobación tras vencimiento se simularán para controlar sus tiempos. Los dobles no verifican comunicación real; el ambiente de integración no demuestra cargos monetarios productivos.

- **OpenWeather — principalmente London; Detroit ocasional.** Simularía pronóstico, falta de respuesta y renovación de caché. Una llamada real por ejecución separada comprobará credenciales y lectura del pronóstico, sin exigir una temperatura concreta. Como su falla no bloquea reservas, no destinaría cobros recurrentes a estas pruebas: solo haría la llamada con cuota gratuita compartida disponible; de lo contrario, registraría su omisión. La simulación no verifica el contrato ni la disponibilidad reales; una llamada exitosa tampoco garantiza disponibilidad futura.

- **Splunk — London, sin integración real.** Usaría `mock_log_to_splunk` en controllers/services y un spy para comprobar el intento de registrar errores: el mock no conserva mensajes. Omitiría los logs reales por costo y menor prioridad frente a pagos/reservas. Quedan sin verificar recepción efectiva, credenciales y configuración de Splunk.

Se acepta el costo fijo del ambiente AWS compartido, además del uso anterior; reducir ejecuciones no elimina ese costo fijo. Se prueba nuestra conexión y comportamiento, no el funcionamiento interno de React, PostgreSQL ni los SDK.

**Respaldo:** E, tabla 2, reglas de reserva y notificaciones, particularidades de Splunk; T 28–39, enfoques; T 55 y 59–63, mocks e integración. La priorización y frecuencias son decisiones propuestas.

### 6.c. Recorrido completo

Automatizaría con Cypress un recorrido en el ambiente AWS de pruebas: un jugador ya autenticado busca una cancha, abre su ficha, selecciona un bloque futuro y paga con una tarjeta aprobada de **Webpay de integración**. Utilizaría frontend, backend, RDS y ElastiCache reales de ese ambiente, junto con Webpay de integración. SQS y SES seguirían conectados al ambiente AWS de pruebas, pero sus correos no serían condición de éxito del E2E. OpenWeather y Splunk estarían sustituidos. Comprobaría confirmación visible, reserva persistida y eliminación de la retención.

Esto verifica que navegador, endpoints, lógica, almacenamiento y pago funcionen juntos. Su mayor costo se controla manteniendo pocos recorridos completos y probando rechazos, vencimientos y errores en las unitarias e integraciones de 6.b. OpenWeather se sustituye para evitar cuota e inestabilidad; su contrato real se comprueba aparte. Las respuestas de nuestros endpoints no se simulan en este recorrido. Los correos se verifican en la integración de notificaciones, fuera del criterio de éxito E2E solicitado. El test parte de un jugador autenticado y una cancha con un bloque futuro libre, creados previamente en el ambiente de pruebas, sin probar registro, login ni publicación.

**Respaldo:** T 72–73, pruebas E2E; F 31 y 34–35, Cypress; E, P6.c y supuestos de sesión y alcance.

### 6.d. Ejecución en pipelines

Todos los tests anteriores tienen comprobaciones automáticas. Se ejecutarán en los siguientes momentos:

- **Unitarias backend e integración backend sin proveedores reales:** pipeline backend, al abrir/actualizar PR y hacer merge; antes de generar el paquete desplegable. En unitarias se sustituyen las dependencias del service, incluido el almacenamiento; en integración se ejecutan componentes propios y PostgreSQL/Redis de prueba fuera de AWS. En ambas se sustituyen Webpay, OpenWeather, SQS, SES y Splunk cuando intervengan. También pueden ejecutarse en el computador del ingeniero antes del PR. Así se repiten las pruebas sin llamadas cobradas a proveedores.

- **Unitarias frontend y tests de vistas con backend simulado:** pipeline frontend, al abrir/actualizar PR y hacer merge; antes de generar los archivos desplegables. También pueden ejecutarse en el computador del ingeniero. Ninguno conecta con backend ni proveedores reales.

- **Integración backend con AWS y Webpay de integración reales:** pipeline backend, al hacer merge. Orden: generar paquete → desplegar a pruebas → verificar RDS, ElastiCache, SQS, SES y Webpay de integración → E2E → producción si pasan. Incluye el test de reserva, publicación/consumo SQS y aceptación de los dos correos por SES dentro de 60 segundos desde el registro de la reserva. Los rechazos/reintentos se comprueban con sustitutos en las unitarias anteriores, no provocando fallas reales. OpenWeather y Splunk permanecen sustituidos. Se acepta costo AWS por verificar reservas y notificaciones antes de producción; Webpay de integración no cobra comisiones según E.

- **E2E de las tres páginas:** ambos pipelines, al hacer merge, después de generar/desplegar el componente modificado a pruebas y antes de producción. Usa las dependencias indicadas en 6.c, sin simular respuestas de nuestros endpoints. Se ejecuta desde ambos porque frontend o backend pueden romper el recorrido.

- **Integración real con OpenWeather:** una persona iniciará el workflow backend cuando cambie su integración/configuración y una vez por iteración. Con la versión instalada en pruebas, el test hará llamadas reales y verificaciones automáticas antes de desplegar cambios de clima a producción. Se realizará una llamada solo si hay cuota gratuita compartida disponible; en caso contrario se omitirá y se registrará que esa comunicación no fue comprobada. Splunk seguirá sustituido.

El ambiente compartido se valida por turnos para que los pipelines no alteren simultáneamente sus datos o versiones. Se identifica qué frontend y backend se prueban: nueva versión de uno con la versión vigente del otro, ambas instaladas en pruebas; cambios coordinados se validan juntos. Se despliega a producción el mismo paquete comprobado y un test fallido bloquea ese despliegue. Se acepta mayor duración y uso de servicios a cambio de detectar errores antes de producción; Actions no añade cobros de minutos según E.

**Respaldo:** E, pipelines y tabla 2; T 79 y 82, rapidez/costos. El orden y las suites son decisiones propuestas.

### 6.e. Pruebas manuales

Los cuatro ingenieros repartirían las pruebas manuales de las pantallas modificadas: durante el desarrollo, antes de cerrar el PR y, después de las pruebas automatizadas, antes de cada entrega de tres semanas. Revisarían mensajes, carga/errores, navegación y presentación móvil/escritorio en el ambiente de pruebas, con datos preparados y Webpay de integración.

Esta revisión detecta problemas de uso no anticipados por los tests. Para limitar horas, se concentra en cambios recientes y automatiza las comprobaciones recurrentes, sin sustituir las pruebas de pagos y retenciones.

**Respaldo:** T 8 y 84; E, capacidad del equipo e iteraciones.

## 7. Apoyo de IA: complemento de estudio, no pregunta inventada

La introducción menciona orientar LLM para escribir pruebas, pero no hay una pregunta separada sobre ello en el archivo disponible. Esta sección sirve para trabajar; no es necesario convertirla en una respuesta adicional extensa que distraiga de P1–P6.

Una forma de aprovechar IA sin delegarle la decisión sería:

1. Formular tú los comportamientos y resultados esperados desde E.
2. Pedir una matriz de casos positivos, negativos y bordes, indicando alcance y supuestos.
3. Contrastar cada caso con las reglas originales, especialmente aprobación tardía, reintentos y el comienzo exacto del bloque.
4. Si se generara código, ejecutarlo y revisar que falle ante una implementación incorrecta relevante y pase con la correcta; no confiar en que un test que pasa necesariamente compruebe algo útil.
5. Escribir tu argumento y usar IA para detectar omisiones o corregir lenguaje conservando tus ideas y orden.

**Ejemplo de prompt de estudio:**

> A partir de las reglas de PagoService que adjunto, propone una tabla de casos de prueba con precondición, evento, resultado esperado y regla del enunciado que lo respalda. Separa hechos de supuestos y señala información insuficiente. Incluye vencimiento durante el pago y bloque que comienza con retención aún vigente. No inventes endpoints, capacidades de Webpay ni reglas de cancelación. Después indica qué casos no se verifican con un doble del proveedor.

**Evidencia:** IA, páginas del archivo 7 y 10–13 sobre ejemplos/razonamiento y costo; páginas 29–30 sobre instrucciones específicas, estructura y casos de borde. E, “Uso de inteligencia artificial”, delimita el uso permitido y la obligación de declarar herramientas. El flujo de revisión de tests es una propuesta de trabajo, no una transcripción del PDF.

## 8. Lista de revisión antes de redactar y entregar

- [ ] P1 y P4 usan los mismos inicios y la misma ventana de ocio.
- [ ] P2 incluye timebox, término y resultado accionable.
- [ ] P3 desarrolla solo un spike, ajeno a notificaciones push.
- [ ] P4 incluye el spike en los cuatro promedios y muestra dos decimales.
- [ ] P5 nombra el patrón, distingue esfuerzo/cantidad y explica ganancias y pérdidas.
- [ ] P6.a declara si los porcentajes son tiempo o cantidad y suman 100 %.
- [ ] P6.b cubre tres páginas, cuatro endpoints, seis services y siete dependencias externas/administradas: cuatro AWS más Webpay, OpenWeather y Splunk.
- [ ] P6.b explica qué queda sin verificar con cada reemplazo y cómo se prueba la notificación en un minuto.
- [ ] P6.b justifica omitir unitarias de mera delegación, integración real de Splunk y pruebas internas de terceros, conservando cobertura del comportamiento propio.
- [ ] P6.c atraviesa navegador/backend/pago y deja correos fuera del criterio de ese recorrido.
- [ ] P6.d identifica pipeline, evento y posición para cada suite automática; contempla cambios de ambos repositorios.
- [ ] P6.e ubica temporalmente las pruebas manuales.
- [ ] Toda decisión relevante explica beneficio, costo y limitación.
- [ ] No se presentan AWS como gratuito, el margen promedio de OpenWeather como cuota garantizada ni el mock de Splunk como registro de mensajes.
- [ ] No se rediseña la arquitectura ni se añaden funcionalidades fuera del alcance.
- [ ] Se revisaron las fuentes locales y se redactó una posición propia, más allá de reformular esta guía.
- [ ] La sección final declara cada herramienta de IA y el uso efectivo que se le dio; las fuentes externas que tú consultes se citan identificablemente.
- [ ] El PDF final se llama `Interrogacion-Nombre Apellido.pdf`, sin tildes, y respeta destinatario/asunto de E. Esta guía Markdown no sustituye ese PDF.

### Declaración de procedencia de esta guía

Preparada con Codex a partir del enunciado y de los materiales locales listados. Se extrajo texto de PPTX/PDF, se revisó la imagen de Cupcake de T 85 y se verificaron los cálculos de P4. No se ejecutó una plataforma real, no se consultaron proveedores y no se comprobaron precios ni capacidades comerciales fuera de los supuestos de la evaluación.

Para tu declaración final, describe lo que realmente hiciste con esta ayuda: análisis del enunciado, localización de material de clase, contraste de decisiones, revisión de cálculos y/o corrección de tu texto. Declara también otras herramientas utilizadas. La declaración de IA no reemplaza las citas bibliográficas externas.
