# AsistenteCreativo
Asistente para creativos freelance que busca gestionar contrataciones, desplazamientos, cotizaciones y agendas con el objetivo de maximizar el beneficio.


## Descripción del problema

Soy fotógrafo y videógrafo. Necesito constantemente planificar mi agenda según las necesidades y ubicación de cada cliente, asi como calcular tarifas teniendo en cuenta servicios contratados y tiempo consumido por cada uno, horas necesarias para cada servicio y distancia de desplazamiento entre sesiones consecutivas.<br>

Debido a que resulta dificil dar con la respuesta que optimice y tenga en cuenta todas estas variables, es común acabar con una agenda con horas muertas y rutas ineficientes, limitando el número máximo de clientes atendidos en un determinado período de tiempo y afectando en última instancia a mis ingresos potenciales.<br>


## ¿De dónde provienen los datos?

Los datos utilizados provienen de mi experiencia personal, agendas que haya construido en el pasado, trabajos realizados, tiempo invertido en cada trabajo, disponibilidades de clientes pasados y fechas límites de cada uno. En cuanto a los tiempos de desplazamiento que es una variable importante para la construcción de una agenda óptima, se utilizará una matriz fija con tiempos aproximados de desplazamiento entre ubicaciones (basados en mi experiencia).<br>

Por el nicho al que me dedico las ubicaciones que debo incluir se reducen a un número limitado conocido al igual que los tiempos de desplazamiento. Por ejemplo: sé que si me tengo que desplazar al Circuito de Jerez desde Granada tardo 3h, si me desplazo al circuito de tabernas tardo 1h 45m, si luego tengo que ir a un concesionario en Almería capital, tardo 40 minutos desde el circuito.<br>

Esta forma de afrontar la variable del tiempo de desplazamiento mediante matrices de tiempos predefinidos es utilizada, por ejemplo, por las aerolineas para gestionar las conexiones de tripulación y pasajeros dentro de un aeropuerto (Minimum Connection Times). No se calcula en tiempo real cuanto se tarda en ir de una terminal a otra, sino que se utiliza una matriz predefinida que establece tiempos fijos según la terminal de origen y destino, teniendo en cuenta estos márgenes al emitir los billetes.<br>


## ¿Se puede testear la lógica de negocio?

Si. Para comprobar la lógica de negocio con tests y que efectivamente se resuelve el problema que se tiene, se utilizaran las agendas que haya construido en el pasado, estas se compararán con la agenda propuesta por mi lógica de negocio, de esta forma se puede comprobar que efectivamente se perdió cierta cantidad de horas que podían ser aprovechadas y nos aseguramos de que en el futuro esto no vuelva a ocurrir.


## Juego de rol cliente-desarrollador

[Fotografía de la tarjeta de rol](./juegoRol.jpeg)


## Lista de comprobación

**¿Se trata de un problema real del que se tenga conocimiento personal?** <br>

Si, me he dedicado a la fotografía y videgrafía por lo que el problema que se presenta es uno al que he tenido que hacer frente personalmente.<br>

**¿Se trata de un problema que para solucionar requiera el despliegue de una aplicación en la nube?** <br>

Si, los datos de disponibilidad, ubicacion, etc. tanto del creativo como del cliente deben estar en un mismo sitio para así poder calcular la agenta más rentable para el creativo.<br>

**¿La solución requiere una cierta cantidad de lógica de negocio, en vez de solucionarse sólo almacenando y buscando?** <br>

Si, el calculo de la agenda más rentable no tiene una solución trivial, ya que depende de muchos factores como la ubicación en la que se vaya a hacer el trabajo, distancia entre ubicaciones, servicios contratados por cada cliente, duración de cada sesión y disponibilidades tanto de cada cliente como del creativo. <br>

**¿Se ha incluido la configuración del repositorio y se ha enlazado desde el `README`?** <br>

Toda la información relacionada con la configuración del repositorio se puede encontrar en el directorio [doc](./doc/configuracion.md) del mismo.<br>

**¿Se ha incluido y enlazado correctamente la fotografía de la tarjeta del juego de rol en el `README.md` subiéndola al repositorio?** <br>

La [fotografía de la tarjeta del juego de rol](./juegoRol.jpeg) es vicible en el anterior hipervínculo y más arriba en este mismo fichero. <br>

**¿El estudiante tiene todos los datos necesarios para poder resolver el problema, o va a requerir que el usuario los introduzca?** <br>

Los datos necesarios serían los correspondientes a mi agenda actual. <br>

**¿Se está marcando al buen tuntún todo?** <br>

:no_mouth: <br>