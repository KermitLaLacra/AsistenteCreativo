# AsistenteCreativo
Asistente para creativos freelance que busca gestionar contrataciones y agendas con el objetivo de minimizar "horas muertas" y maximizar el beneficio.


## Problema a tratar

La gestión y asignación de citas para servicios creativos como la fotografía y la videografía, sobre todo durante periodos de alta demanda, genera una agenda altamente fragmentada y con muchas horas muertas, resultando en una pérdida significativa de horas productivas.


## Origen y desarrollo del problema

Como fotógrafo y videógrafo, la planificación de la carga de trabajo no consiste en anotar eventos simples. Cuando un cliente solicita un servicio, la planificación va mucho más allá de anotar una cita en el calendario. La asignación manual (por orden de llegada de la petición) resulta ineficiente debido a tres factores:

1. Disponibilidad cruzada: Los clientes no suelen exigir una hora exacta, sino que ofrecen ventanas de disponibilidad (por ejemplo, "cualquier tarde de esta semana" o "lunes y miércoles de 8:00 am a 12:00 pm").

2. Post-producción: Cada servicio tiene asociado un tiempo de trabajo post-producción asociado. Una sesión de grabación de 2 horas en un circuito puede requerir obligatoriamente 5 horas adicionales que abarcan: selección de material, transferencia del material y preparación para su edición y edición. Estas horas de edición no tienen que ejecutarse seguidas, pero deben encajarse obligatoriamente en los huecos libres de la agenda de esa misma semana, sin pisar otros rodajes, y estrictamente antes de la fecha límite de entrega pactada.

3. Imprevistos y cancelaciones: En el modelo manual, cuando un cliente cancela una sesión con poco margen, ese bloque de tiempo suele perderse por completo. La incapacidad de reaccionar rápidamente para reestructurar la agenda e insertar otras tareas pendientes si fuera posible en ese hueco genera una pérdida de rentabilidad. Si bien se necesita flexibilidad para reubicar citas, en la práctica profesional no se pueden alterar los horarios de otros clientes a pocos días de su sesión. Es necesario un sistema que permita reajustes dinámicos, pero garantizando un periodo de bloqueo (14 días) para evitar inconvenientes a corto plazo.

Organizar esto de forma manual provoca que la jornada laboral acabe llena de "huecos muertos". El resultado es una limitación en el número de clientes que se pueden atender a la semana. Esta ineficiencia se traduce en una pérdida directa de ingresos al tener que rechazar en algunos casos hasta el 15% de peticiones entrantes durante la temporada alta, a pesar de tener simultáneamente hasta un 10% de tiempo libre fragmentado."


## Datos disponibles

Para modelar y resolver este problema de optimización el sistema extraerá la información de un fichero local basado en mi experiencia y trabajos pasados. Puedo exportar mi calendario de Google en un fichero .ics y luego convertirlo a un fichero .csv. En este fichero se encuentran datos como: cliente, servicios contratados, disponibilidad del cliente, duración de la sesión, horas de post-producción y fecha límite de entrega.

Este histórico de calendario abarca meses completos de trabajo real, por lo que los registros del fichero CSV reflejan y demuestran la densidad de eventos y la fragmentación generada durante los periodos de alta demanda.


## Lógica de negocio

El sistema no actuará como un simple calendario, sino como un motor de optimización. Para resolver el problema de la fragmentación, el algoritmo deberá:

- Calcular múltiples permutaciones de asignación teniendo en cuenta las ventanas de disponibilidad de todos los clientes pendientes.

- Calcular e insertar de forma automática los bloques de tiempo necesarios para la post-producción, asegurando que se agenden dentro de la disponibilidad del estudio y antes de las fechas límite, sin entrar en conflicto con los horarios de rodaje.

- Replanificación dinámica: Ante la cancelación inesperada de una sesión, el sistema debe ser capaz de reevaluar el cuadrante en tiempo real para rellenar ese nuevo hueco muerto, adelantando automáticamente bloques de post-producción pendientes o reasignando sesiones de clientes con flexibilidad horaria que encajen en ese hueco disponible. Sin embargo, el algoritmo debe respetar una regla estricta: las reservas ya existentes solo podrán ser ajustadas si están agendadas para dentro de más de 2 semanas. Toda cita a menos de 14 días se considera bloqueada para el motor de optimización.


## Heurística de optimización y asignación

Para resolver el problema de forma eficiente, el algoritmo no evalúa las peticiones a ciegas, sino que aplica un conjunto de reglas heurísticas (pasos lógicos) diseñadas para maximizar los ingresos sin comprometer las entregas:

1.  **Priorización por rentabilidad (Maximizador de ingresos):** Ante un volumen alto de peticiones, el sistema ordena las solicitudes en cola de forma descendente en función del beneficio económico que genera cada servicio. El algoritmo intentará encajar primero los proyectos que reporten mayores ingresos.

2.  **Inserción segura de nuevos proyectos:** La heurística da preferencia absoluta a aceptar nuevos trabajos para maximizar la ocupación, pero con una condición. Al intentar agendar un nuevo rodaje, el sistema simula el cuadrante resultante, el nuevo proyecto solo se acepta si, tras colocar su rodaje y sus horas de post-producción, el resto de tareas de post-producción ya existentes siguen teniendo espacio para completarse antes de sus respectivas fechas límite. Si aceptarlo provoca el retraso de un proyecto previo, se busca otra ventana de disponibilidad o se rechaza.

3.  **Relleno de huecos por urgencia de entrega:** Una vez fijados los bloques inamovibles de rodaje, el algoritmo fragmenta las horas de post-producción y las usa como "cemento" para rellenar los huecos muertos. En caso de conflicto (dos proyectos compiten por el mismo hueco para ser editados), la heurística siempre asigna el bloque al proyecto con la fecha límite de entrega más próxima.

4.  **Arrastre dinámico ante cancelaciones:** Cuando una cancelación libera un bloque de tiempo, la heurística prioriza rellenarlo rescatando un nuevo proyecto de la cola, evaluando siempre primero a los clientes de mayor rentabilidad. Solo en el caso de que ningún nuevo rodaje encaje en esa franja horaria específica, el sistema aprovechará ese tiempo liberado para adelantar horas de post-producción pendientes, evitando así que el hueco se desperdicie.



## Cómo se probará automáticamente

El problema está planteado para ser comprobable de forma totalmente automática. Se considerará que la lógica ha resuelto el problema si, al introducir un conjunto de peticiones potencialmente conflictivas, el sistema devuelve un cuadrante semanal que cumpla con los siguientes criterios:

1.  Cero colisiones: Ninguna restricción de tiempo o fecha de entrega es violada.

2.  Reducción de fragmentación: Al comparar la agenda generada por el algoritmo contra una agenda generada secuencialmente, el sistema debe demostrar una reducción porcentual medible en minutos de los "huecos muertos", maximizando la tasa de ocupación de las horas laborables totales.

3. Tolerancia ante cancelaciones: Al simular la cancelación de un evento en mitad de la semana, el algoritmo debe regenerar el cuadrante absorbiendo el hueco generado con tareas en cola (como horas de edición pendientes), si esto reduce efectivamente el total de "horas muertas". La prueba validará que la ocupación se maximiza y que ninguna cita de otro cliente programada a menos de 14 días de distancia ha visto alterada.



## Juego de rol cliente-desarrollador

[Fotografía de la tarjeta de rol](./juegoRol.jpeg)