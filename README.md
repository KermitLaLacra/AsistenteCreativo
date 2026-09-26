# AsistenteCreativo
Asistente para creativos freelance que busca gestionar contrataciones, desplazamientos y agendas con el objetivo de minimizar "horas muertas" y maximizar el beneficio.


## Problema a tratar

La gestión y asignación de citas para servicios creativos como la fotografía y la videografía, sobre todo durante periodos de alta demanda, genera una agenda altamente fragmentada y con muchas horas muertas, resultando en una pérdida significativa de horas productivas.


## Origen y desarrollo del problema

Como fotógrafo y videógrafo, mi trabajo se desarrolla tanto en mi estudio como en ciertas localizaciones recurrentes (principalmente circuitos de velocidad y ciudades cercanas a estos). Cuando un cliente solicita un servicio, la planificación va mucho más allá de anotar una cita en el calendario. La asignación manual (por orden de llegada de la petición) resulta ineficiente debido a tres factores concurrentes:

1. Disponibilidad cruzada: Los clientes no suelen exigir una hora exacta, sino que ofrecen ventanas de disponibilidad (por ejemplo, "cualquier tarde de esta semana" o "lunes y miércoles de 8:00 am a 12:00 pm").

2. Tiempos de tránsito: Las sesiones requieren desplazamientos. Agendar a dos clientes en ciudades distintas el mismo día sin agruparlos por zonas genera una pérdida drástica de tiempo.

3. Post-producción: Cada servicio tiene asociado un tiempo de trabajo post-producción asociado. Una sesión de grabación de 2 horas en un circuito puede requerir obligatoriamente 5 horas adicionales que abarcan: selección de materiañ, transferencia del material y preparación para su edición y edición. Todo esto debe realizarse en el estudio antes de una fecha límite de entrega acordada.

4. Imprevistos y cancelaciones: En el modelo manual, cuando un cliente cancela una sesión con poco margen, ese bloque de tiempo suele perderse por completo. La incapacidad de reaccionar rápidamente para reestructurar la agenda e insertar otras tareas pendientes si fuera posible en ese hueco genera una pérdida de rentabilidad. Si bien se necesita flexibilidad para reubicar citas, en la práctica profesional no se pueden alterar los horarios de otros clientes a pocos días de su sesión. Es necesario un sistema que permita reajustes dinámicos, pero garantizando un periodo de bloqueo (14 días) para evitar inconvenientes a corto plazo.

Organizar esto de forma manual provoca que la jornada laboral acabe llena de "huecos muertos" (por ejemplo, franjas de 45 a 60 minutos de inactividad entre sesiones en las que no hay tiempo suficiente ni para iniciar un desplazamiento a otra ciudad ni para avanzar en la edición de un proyecto). El resultado es una limitación en el número de clientes que se pueden atender a la semana y, por tanto, una pérdida de ingresos potenciales.


## Datos disponibles

Para modelar y resolver este problema de optimización, el sistema se alimentará de dos fuentes de datos:

- Peticiones de entrada: Tipo de servicio (determina la duración base de grabación y la cantidad de horas de post-producción requeridas), ubicación y ventanas de disponibilidad del cliente.

- Matriz de desplazamientos: Para aislar el problema y no depender de APIs de tráfico externas que introduzcan inestabilidad en las pruebas, se utilizará una matriz interna que define el tiempo de tránsito en minutos entre los puntos de trabajo (Estudio, Circuito A, Circuito B, Ciudad C). Estos tiempos aproximados están basados en la experiencia real.


## Lógica de negocio

El sistema no actuará como un simple calendario, sino como un motor de optimización. Para resolver el problema de la fragmentación, el algoritmo deberá:

- Calcular múltiples permutaciones de asignación teniendo en cuenta las ventanas de disponibilidad de todos los clientes pendientes.

- Agrupar automáticamente las sesiones por proximidad geográfica consultando la matriz de desplazamientos, minimizando el tiempo total de tránsito diario.

- Calcular e insertar de forma automática los bloques de tiempo necesarios para la post-producción, asegurando que se agenden dentro de la disponibilidad del estudio y antes de las fechas límite, sin entrar en conflicto con los horarios de rodaje.

- Replanificación dinámica: Ante la cancelación inesperada de una sesión, el sistema debe ser capaz de reevaluar el cuadrante en tiempo real para rellenar ese nuevo hueco muerto, adelantando automáticamente bloques de post-producción pendientes o reasignando sesiones de clientes con flexibilidad horaria que encajen en esa zona geográfica. Sin embargo, el algoritmo debe respetar una regla estricta: las reservas ya existentes solo podrán ser ajustadas si están agendadas para dentro de más de 2 semanas. Toda cita a menos de 14 días se considera bloqueada para el motor de optimización.

## Cómo se probará automáticamente

El problema está planteado para ser comprobable de forma totalmente automática. Se considerará que la lógica ha resuelto el problema si, al introducir un conjunto de peticiones potencialmente conflictivas, el sistema devuelve un cuadrante semanal que cumpla con los siguientes criterios:

1.  Cero colisiones: Ninguna restricción de tiempo, desplazamiento o fecha de entrega es violada.

2.  Reducción de fragmentación: Al comparar la agenda generada por el algoritmo contra una agenda generada secuencialmente, el sistema debe demostrar una reducción porcentual medible en minutos de los "huecos muertos", maximizando la tasa de ocupación de las horas laborables totales.

3. Tolerancia ante cancelaciones: Al simular la cancelación de un evento en mitad de la semana, el algoritmo debe regenerar el cuadrante absorbiendo el hueco generado con tareas en cola (como horas de edición pendientes), si esto reduce efectivamente el total de "horas muertas". La prueba validará que la ocupación se maximiza y que ninguna cita de otro cliente programada a menos de 14 días de distancia ha visto alterada.



## Juego de rol cliente-desarrollador

[Fotografía de la tarjeta de rol](./juegoRol.jpeg)