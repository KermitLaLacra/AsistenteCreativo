# AsmaTrain
Sistema de asistencia para la planificación de entrenamientos al aire libre para deportistas con asma, distinguiendo los factores que afectan a cada perfil y teniendo en cuenta la exigencia respiratoria del entrenamiento.


## Problema a tratar

Las personas asmáticas que practican deportes al aire libre como ciclismo o running, se enfrentan a una incertidumbre constante sobre cuándo es seguro entrenar. Las métricas públicas que tenemos mas a mano (como las aplicaciones del pronóstico del tiempo) son muy generales y no reflejan el impacto real que podría tener las condiciones del aire sobre cada individuo.

Esta falta de personalización provoca que el deportista acabe entrenando en momentos de alto riesgo, sufriendo crisis asmáticas o, que por precaución, que cancele entrenamientos en momentos que realmente eran seguros para él. El problema a resolver es eliminar esta incertidumbre mediante el cálculo de un riesgo personalizado, evitando la exposición de cada persona a los factores que son mas propensos a perjudicarle sin sacrificar la constancia deportiva.


## Origen y desarrollo del problema

Mi hermano y yo hemos practicado deporte desde una edad bastante temprana, mi hermano ha sido asmático desde muy pequeño y yo he desarrollado en los últimos años asma alérgico. A mi hermano suelen causarle crisis asmáticas los ambientes con mucho polvo y poca humedad, mientras que a mí me suelen afectar los momentos del año en los que la cantidad de polen en el aire es alta.

A cada asmático le afectan cosas distintas: a uno el polen, a otro la temperatura, la humedad, partículas como PM2.5 y PM10, ozono, NO₂ o el polvo. Actualmente, las herramientas meteorológicas y de calidad del aire ofrecen datos demasiado generales, por lo que tomar la decisión de entrenar basándose en una simple alerta de "Calidad del aire moderada" es insuficiente.

La intensidad del ejercicio es un factor que también aumenta drásticamente el riesgo ya que con una alta intensidad se multiplica el volumen de aire inhalado y también la cantidad de contaminantes o alérgenos que llegan a nuestros pulmones. Unas condiciones podrían ser seguras para un entrenamiento suave de recuperación pueden ser peligrosas para un entrenamiento de alta intensidad.

Sumado a todo lo anterior, todos los factores mencionados varían a lo largo del día y en muchos casos, en las aplicaciones de meteorología solo es visible la calidad del aire en el momento actual, por lo que un deportista podria iniciar una ruta segura que empeorará drásticamente a mitad del camino.


## Datos disponibles

El sistema procesará la información a partir de ficheros locales estructurados (JSON/XML). Las fuentes de datos son las siguientes:

- **Perfil de usuario:** Se almacena en fichero JSON que se construirá a partir de las sensibilidades del usuario, otorgandole un peso del 0 al 10 a cada factor que podría ser de riesgo para un asmático, se incluirá también el nivel de gravedad de su asma, la duración del entrenamiento, el nivel de intensidad y sus franjas de disponibilidad horaria.

- **Datos ambientales:** Los encontramos ficheros JSON o XML que contienen las previsiones ambientales. Estos se obtendrán principalmente de Open-Meteo, como se puede ver en el siguiente enlace de ejemplo [Open-Meteo](https://open-meteo.com/en/docs/air-quality-api?latitude=37.1773&longitude=-3.5986&hourly=pm10,pm2_5,carbon_monoxide,nitrogen_dioxide,sulphur_dioxide,ozone,dust&timezone=Europe%2FMadrid&bounding_box=-90,-180,90,180), se nos muestra el pronostico para partículas PM10 y PM2.5, CO, NO2, SO2, O3 y polvo. Desde aquí también podemos obtener datos sobre el clima como la temperatura o la humedad, además se tendrá como segunda opción los proporcionados por [AEMET](https://www.aemet.es/xml/municipios_h/localidad_h_18087.xml).


## Lógica de negocio

La lógica de negocio engloba lo siguiente:

- Ponderación personalizada del riesgo: El algoritmo relaciona las mediciones ambientales de cada hora con los multiplicadores del perfil del usuario, obteniendo un puntaje de riesgo para cada hora. Además se aplica un multiplicador de intensidad.

- Entrenamientos continuos: Para un entrenamiento de N horas el sistema no busca horas sueltas, sino que evalúa bloques continuos, calculando el riesgo acumulado del bloque.

- Busqueda de alternativas: En el caso de que ninguna de las franjas horarias evaluadas sea lo suficientemente segura, el sistema buscará alternativas como reducir la intensidad o acortar la duración del entrenamiento si esto hiciera que encajara en una franja segura.


## Juego de rol cliente-desarrollador

[Fotografía de la tarjeta de rol](./doc/juegoRol.jpeg)