---
UniqueId: KBuEXNuKlD
Title: "Consejo de diseño #167 Tipos de tablas de hechos complementarias"
Url: 2014/tipos-tablas-de-hecho.html
Date: 2017-04-10T00:06:51.6480000
SecondaryDate: 2014-06-17T00:00:00.0000000
Description: "Hay tres tipos fundamentales de tablas de hechos en el área de presentación del data warehouse: ·Tablas de hechos de transacciones, tablas de hechos de capturas periódicas,· tablas de hechos de capturas acumuladas."
Image: 2014-tipos-tablas-de-hecho.jpg
Author: Bob Becker
Category: "Fundamentos diseño dimensional"
RelatedUrl: http://www.kimballgroup.com/2014/06/design-tip-167-complementary-fact-table-types/
IsDraft: false

---
Hay tres tipos fundamentales de tablas de hechos en el área de presentación del data warehouse:

- [Tablas de hechos de transacciones][1]
- [Tablas de hechos de capturas periódicas][2]
- [Tablas de hechos de capturas acumuladas][3]

La mayoría de equipos de diseño DW/BI están muy familiarizados con las **tablas de hechos de transacciones**. Son el tipo más común de tipos de tablas de hecho y normalmente el caballo de batalla para muchas organizaciones. Muchos equipos también han incorporado las **tablas de hechos de capturas periódicas** en sus áreas de presentación. Pocas organizaciones aprovechan las **tablas de hechos de capturas acumuladas**. Los equipos de diseño normalmente no aprecian como una tabla de hechos de captura acumulada puede complementar tablas de hechos de capturas de transacciones y/o periódicas.

Cada tipo de tabla de hechos es una respuesta de diseño a la amplia variedad de requisitos planteados por la comunidad de negocio. A menudo la mejor respuesta de diseño es una combinación de dos, o incluso de tres, tipos de tabla de hechos. Las tablas de hechos múltiples complementan unas a las otras, cada una apoya una única visión de los procesos de negocio los cuales sería difícil conseguir con sólo un tipo de tabla de hechos.

### Un ejemplo

Una cadena de suministro logística es un escenario excelente para ilustrar los tres tipos de tablas de hecho trabajando a la vez para dar soporte a un rico conjunto de requisitos empresariales. Utilizaremos una vista simplificada de los productos terminados de la cadena logística de un gran fabricante de coches como ayuda para entender las fortalezas y el uso apropiado de cada tabla de hechos.

Nuestro fabricante de coches tiene plantas donde los vehículos se ensamblan. Los vehículos terminados son llevados finalmente a los distribuidores donde se venderán a los usuarios finales. Nuestro fabricante ficticio mantiene el inventario de productos acabados en un gran aparcamiento -un verdadero almacén- situado justo a las puertas de la planta de ensamblaje. Los vehículos (del inventario) se envían desde el almacén de productos acabados mediante tren de carga a uno de los varios aparcamientos de la región. Desde estos almacenes regionales, el inventario se envía mediante camión de carga a la localización del distribuidor. Una vez que el vehículo llega al distribuidor, se prepara y se pone en el lote del distribuidor (inventario del almacén) para la venta final.

Los usuarios del negocio logístico necesitan entender el número de vehículos que fluyen fuera del final de la cadena de ensamblaje, dentro y fuera de cada almacén, y el cliente final demanda varios tipos de vehículos, colores, modelos  y así sucesivamente. La compañía también necesita entender y analizar el nivel de inventario en cada etapa de la cadena logística. La gestión de logística quiere comprender cuanto tiempo lleva mover un vehículo desde la planta de ensamblaje hasta el cliente final, dependiendo del tipo de vehículo, almacenes y transportes. Mover los vehículos más rápidamente y de forma eficiente a través de la cadena logística ayuda a la compañía a minimizar los niveles de inventario y reducir los costes de transporte.

Un diseño robusto para respaldar la cadena logística de productos terminados de nuestro fabricante ilustra los tres tipos de tablas de hechos.

#### Tabla de hechos de transacciones

Un componente clave de la cadena logística es la fluidez del inventario desde una localización a otra. El flujo de vehículos se captura en una serie de inventarios de movimientos de transacciones. El vehículo es entonces enviado mediante tren al almacén regional donde se recibe para su inventario; más tarde se mueve desde el inventario y se envía mediante camión al distribuidor donde este lo recibe en su inventario. Para cada uno de estos movimientos de inventario, se genera un movimiento de inventario o una transacción de envío/recepción. El granulo de esta tabla de hechos es una fila para cada transacción de  movimiento de inventario de cada vehículo. Del mismo modo la venta final del vehículo debería ser capturada en una tabla de hechos de  transacción de ventas con una fila para cada vehículo vendido.

Las tablas de hechos transaccionales son una respuesta de diseño apropiada para los requerimientos del negocio que buscan entender la intensidad o cantidad del proceso de un negocio. Las tablas de hechos transaccionales ayudan a responder la pregunta ¿cuántos?. Por ejemplo ¿cuántos vehículos deportivos blancos (SUVs) se vendieron la semana pasada? ¿Cuántas fueron las vendas en dólares? ¿Cuántos vehículos de tracción total se lanzaron para ensamblarse en inventario comercial? ¿Cuántos vehículos enviamos con un transportista pactado? ¿Cuántos vehículos han recibido nuestros distribuidores este mes? ¿Y comparado con el último mes, el último trimestre, o el último año?  Hay una razón por la cual las tablas de datos transaccionales son el caballo de batalla de los tipos de tablas de hechos: Soportan importanteS requerimientos críticos del negocio. Por otra parte, las tablas de hechos transaccionales son menos efectivas respondiendo preguntas sobre el estado de los niveles nuestro inventario o la rapidez/eficiencia de la cadena logística. Para dar apoyo a estos requerimientos del negocio, buscamos otras tablas de hechos para complementar las tablas de hechos.

#### Tablas de hechos de capturas periódicas

El segundo requisito es conocer la cantidad total de inventario en cualquier punto de la cadena. El apoyo del análisis del nivel de inventario es una tarea adecuada para una tabla de hechos de captura periódica. En cualquier momento del tiempo, cada vehículo está en una sola localización física como por ejemplo el inventario de productos terminados en la planta, en un centro de distribución regional, en el parking de un distribuidor, o en tránsito en un tren o camión. Para apoyar el análisis de inventario, la tabla de hechos de captura periódica tiene un gránulo de una fila por vehículo por día. Una dimensión de localización soportará el análisis de inventario en cada punto de la cadena.

La tabla de hechos de captura periódica hace un excelente trabajo ayudando a comprender el volumen de vehículos en nuestra cadena. Contesta a la pregunta ¿cuánto? .¿Cuánto inventario tenemos? ¿Cuánto inventario de vehículos blancos hay disponible? ¿SUVs? ¿cuatro puertas? ¿modelos deportivos? ¿en California? ¿en los lotes de distribuidor? ¿Comparados a meses anteriores, trimestres o años?

La captura periódica respalda la tendencia de niveles de inventario a lo largo del tiempo. Las tablas de transacciones de movimiento y las capturas periódicas de inventario juntas dan soporte a una amplia gama de requerimientos de negocio alrededor de la cadena logística. Sin embargo, incluso con dos de estas tablas de hechos, será difícil atender los requisitos de eficiencia de la cadena. Para completar la fotografía, una tabla de hechos de captura acumulada complementará la tabla de hechos transaccional y de captura periódica.

#### Tabla de hechos de captura acumulada

El tercer conjunto de requisitos para la cadena logística es soportar la velocidad de análisis a la que los vehículos viajan en la cadena logística. Cada vehículo recorrerá una serie de etapas durante su viaje al cliente final. Para cumplir los requisitos analíticos de medir y entender las eficiencias de nuestra cadena logística, una tabla de hechos de captura acumulada se llenará con una fila por vehículo. A medida que cada vehículo se mueve por la cadena, la captura acumulada se actualizará con la fecha de cada movimiento y la localización actual del vehículo. La captura acumulada tendrá numerosas fechas clave, como por ejemplo fecha de entrega por ensamblaje, fecha de envío al centro de distribución, fecha de llegada al centro de distribución y así hasta la venta final. Las métricas de tablas de hechos incluirán una serie de retrasos en las fechas que miden el tiempo que cuesta mover un vehículo entre las diferentes etapas de la cadena.

La tabla de datos de captura acumulada respalda las principales medidas de eficiencia de velocidad. ¿Con que rapidez se mueven los vehículos a través de la cadena? ¿Cuál es la media de tiempo desde la salida de montaje hasta el consumidor final? ¿Es diferente para los coches que para los SUVs? ¿Híbridos versus No-híbridos? ¿Coches blancos versus coches rojos? ¿Cuáles son los transportistas/vías más efectivos? Las capturas acumuladas  se actualizarán diariamente para los vehículos que se encuentren actualmente en la cadena logística.

Consecuentemente, la captura acumulada también se usa para comprobar el estado actual de la cadena e identificar vehículos atascados, como por ejemplo encontrar todos los vehículos que han estado en un centro regional de distribución o en un distribuidor durante más de “x” días. ¿Cuántos vehículos han estado en tránsito vía tren o camión durante más de “x” días? ¿Dónde están todos los SUVs? Esta tabla de hechos puede ayudar al equipo logístico a identificar y mover los vehículos de mayor demanda, identificar las oportunidades de mejora, e identificar los socios de transporte preferidos.

### Conclusión

En el caso de nuestro fabricante de coches, está claro que los tres tipos de tabla de hechos se complementan. Implementar las tres tablas de hechos es una respuesta apropiada al rico conjunto de requisitos empresariales. Implementar solo una o incluso solo dos de los tipos de tablas de hechos hubiesen hecho muy difícil, si no imposible, respaldar todos los requerimientos.





[1]: http://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/transaction-fact-table/
[2]: http://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/periodic-snapshot-fact-table/
[3]: http://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/accumulating-snapshot-fact-table/