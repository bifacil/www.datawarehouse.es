---
UniqueId: vQSJbZiWGv
Title: Las 10 reglas esenciales del modelado dimensional
Url: 2009/los-10-mandamientos-de-kimball.html
Date: 2016-11-03T00:00:00.0000000
SecondaryDate: 2009-05-29T00:07:00.0000000
Description: "Los 10 mandamientos de Kimball. Estas reglas incluyen recomendaciones que se deben seguir necesariamente junto a otras buenas prácticas que conviene tener en cuenta."
Author: Margy Ross
Category: "Fundamentos diseño dimensional"
RelatedUrl: http://www.kimballgroup.com/2009/05/the-10-essential-rules-of-dimensional-modeling/

---
Un estudiante que asistió recientemente a una de las clases de modelado dimensional de *Kimball Gruop* me pidió una lista de "10 Mandamientos de Kimball" para el modelado dimensional. Nos abstendremos de utilizar la terminología religiosa, pero debemos decir que estas reglas incluyen recomendaciones que se deben seguir necesariamente junto a otras buenas prácticas que conviene tener en cuenta.

### Regla \#1: Cargar los datos atómicos en estructuras dimensionales	

Los modelos dimensionales deben estar cargados con el máximo detalle para poder dar respuesta a los filtros impredecibles y agrupaciones requeridas en las consultas de los usuarios de negocio. Normalmente los usuarios no necesitan ver un único registro cada vez, pero no puedes predecir de que manera arbitraria querrán visualizar y consultar estos detalles.

Si sólo están disponibles los datos agregados, habrás hecho conjeturas sobre el patrón de utilización de la base de datos que ocasionará que los usuarios se den contra una pared cuando intenten indagar en profundidad en los detalles Por supuesto, los detalles atómicos se pueden completar con resúmenes de los modelos dimensionales que proporcionan ventajas en el rendimiento de consultas comunes, pero los usuarios de negocio no pueden trabajar sólo con resúmenes de datos; necesitan los detalles específicos para responder sus preguntas cambiantes.

### Regla \#2: Estructura de los modelos dimensionales en función de los procesos de negocio

Los procesos empresariales son las actividades llevadas a cabo por tu organización; representan sucesos, como tomar un pedido o facturar a un cliente.

Normalmente, los procesos empresariales recopilan o generan una única métrica asociada con cada suceso. Estas métricas se traducen en hechos, con cada proceso empresarial representado por un única tabla atómica de hechos. Además del proceso único de tablas de hechos, las tablas de hechos consolidadas se crean a veces para combinar métricas de múltiples procesos en una tabla de hechos con un mismo nivel de detalles. De nuevo, las tablas de hecho consolidadas son un complemento a los detalles, no un sustituto de ellas.

### Regla \#3: Asegurarse de que cada tabla de hechos tiene una tabla de dimensión tiempo asociada

Los sucesos de medición descritos en la *Regla \#2* siempre tienen una fecha asociada, tanto si es un balance mensual o una transferencia bancaria capturada en una centésima de segundo. Cada tabla de hechos debería tener al menos una clave externa asociada a la tabla de dimensión de tiempo (con granularidad de día) con atributos para calendario y con características no estándar sobre la fecha, tales como el mes fiscal o el indicador de las vacaciones corporativas. A veces, una tabla de hechos contiene varias claves foráneas de fecha.

### Regla \#4: Asegurarse de que todos los hechos de la tabla de hechos tienen el mismo nivel de detalle

Hay tres tipos fundamentales para categorizar todas las tablas de hecho: transacciones, capturas periódicas o capturas acumuladas. Independientemente del tipo de tabla de hechos, cada medición dentro de una tabla de hechos debe presentar el mismo nivel de detalle. Cuando mezclamos hechos que presentan diferentes niveles de detalle (granularidad) en la misma tabla de hechos, estamos creando confusiones para el usuario de negocio y haremos que la aplicación BI sea vulnerable a ofrecer resultados erróneos.

### Regla \#5: Resolver correspondencias muchos a muchos en tablas de hechos

Partiendo de que una tabla de hechos recopila los resultados de un proceso empresarial, naturalmente hay una relación muchos a muchos (M:M) entre sus claves foráneas (múltiples productos vendidos en múltiples tiendas en múltiples días). Estas claves foráneas nunca serán nulas. A veces las dimensiones pueden tomar múltiples valores para una sola medición, como los múltiples diagnósticos asociados en una consulta de salud o múltiples clientes con una cuenta bancaria. En estos casos, no es razonable resolver estas dimensiones multivalor en la misma tabla de hechos, ya que esto alteraría la granularidad del suceso de medición. Por lo tanto, deberemos utilizar una tabla puente many-to-many entre la tabla de hechos y la dimensión.

### Rule \#6: Resolver correspondencias muchos a muchos en tablas de dimensiones

Las jerarquías, o relaciones muchos a uno (M:1),  entre atributos son normalmente desnormalizadas en una tabla de dimensión plana. Si te has pasado la mayor parte de tu carrera diseñando modelos entidad-relación para sistemas de procesamiento de transacciones, tendrás que resistirte a tu propia tendencia a normalizar o fragmentar la relación M:1 en subdimensiones más pequeñas. La desnormalización de dimensiones es la regla del modelado dimensional.

Es relativamente común tener múltiples relaciones M:1  representadas en una tabla dimensional. Las relaciones uno-a-uno, como descripción única de un producto asociado con un código de producto, también están contenidos en una tabla dimensional. Ocasionalmente, las relaciones muchos-a-uno se resuelven en la tabla de hechos, como en el caso de que la tabla dimensional detallada tenga millones de filas y sus atributos estén cambiando frecuentemente. Sin embargo, utilizar la tabla de hechos para resolver relaciones M:1 debe hacerse con moderación.

### Regla \#7: Almacenar las descripciones en las tablas de dimensión

Los códigos y, más importante, las decodificaciones asociadas y las descripciones usadas para filtrar las consultas deben incluirse en las tablas dimensionales. Evitaremos almacenar campos de códigos crípticos o campos descriptivos voluminosos en la misma tabla de hechos. Dicho de otro modo: No sólo guardaremos el código en la tabla de dimensión asumiendo que los usuarios no necesitan decodificaciones descriptivas o que estas decodificaciones serán llevadas a cabo por la aplicación de inteligencia de negocios (BI). Si es un título de fila o columna o si aparece en un menú desplegable, entonces debe ser considerado como un atributo dimensional.

Aunque en la *Regla \#5* establecimos que la tabla de hechos de las claves foráneas no debía ser nunca nula, es también recomendable evitar tener nulidades en los campos de atributo de las tablas dimensionales. Para ello, sustituiremos el valor nulo por “NA” (no aplicable) u otro valor por defecto, determinado por el administrador de datos, para reducir las confusiones en la medida de lo posible.

### Regla \#8: Asegurarse de que las tablas dimensionales usan claves subrogadas

Asignar claves subrogadas secuencialmente, claves sin sentido de negocio, proporciona ciertas ventajas operacionales  (excepto para la dimensión fecha donde se asignan cronológicamente). Algunas de estas ventajas son: claves más pequeñas, lo que implica en tablas de hecho menores, menores índices y mejor rendimiento general.

Las claves subrogadas son totalmente necesarias si estás haciendo un seguimiento de los cambios en un atributo dimensional (con un nuevo registro para cada cambio en la descripción...). Incluso si los usuarios de tu empresa no visualizan inicialmente el valor de poder seguir los cambios en los atributos, utilizar claves subrogadas hará que una política de cambios posterior sea menos costosa.

Las claves subrogadas también nos permiten relacionar múltiples claves operacionales distintas en un perfil común. Además nos protege de actividades operacionales inesperadas, como el reciclado de un número obsoleto de producto o la adquisición de otra empresa con sus propios esquemas de código.

### Regla \#9: Crear dimensiones conformadas para integrar los datos de toda la empresa

Las dimensiones conformadas (también conocidas como comunes, maestros, estándar o dimensiones de referencia) son esenciales para el data warehouse en la empresa. Se gestiona una vez en el sistema ETL y se reutiliza posteriormente en múltiples tablas de hechos. Las dimensiones conformadas proporcionan atributos descriptivos consistentes sobre modelos dimensionales y facilitan la habilidad para navegar y integrar datos desde múltiples procesos empresariales. La matriz empresarial (Enterprise Bus Matrix) es el modelo clave para representar los procesos centrales del negocio y su  dimensionalidad asociada.

Reutilizar las dimensiones conformadas reduce el tiempo de comercialización eliminando el diseño redundante y los esfuerzos de desarrollo. Sin embargo, las dimensiones conformadas requieren esfuerzo y inversión en la administración y dirección de datos, incluso si no necesitas que todo el mundo esté de acuerdo en cuanto a todos los atributos de dimensiones para impulsar la conformidad.

### Regla \#10: Valora constantemente los requerimientos y las realidades para proporcionar una solución DW/BI que sea aceptada por los usuarios de negocios y que apoye su proceso de decisiones

Los modeladores dimensionales constantemente deben combinar las demandas de los usuarios de negocio con las realidades subyacentes de las fuentes de datos asociadas para proporcionar un diseño que pueda ser implementado y, más importante, exista una posibilidad razonable de ser aceptado por el negocio.

La confrontación *requerimientos-vs-realidad* es parte del día a día de los profesionales DW/BI, tanto si se centran en el modelo dimensional, en la estrategia de proyecto, en la parte técnica ETL/BI, o en la planificación de la arquitectura de implementación o mantenimiento.

Si has leído regularmente nuestro Kit de artículos, o los consejos de diseño, estas reglas no deberían ser nuevas para ti. En todo caso, aquí las hemos refundido en un simple artículo de reglas que puedes consultar cuando tengas que diseñar o revisar tus modelos.

¡Buena suerte!

Artículo original: [http://www.kimballgroup.com/2009/05/the-10-essential-rules-of-dimensional-modeling/][1]





[1]: http://www.kimballgroup.com/2009/05/the-10-essential-rules-of-dimensional-modeling/