---
UniqueId: dfNcjZyjvC
Title: "Consejo de diseño #171: Desatascar la cadena de claves en la carga de las tabla de hechos"
Url: 2015/desatacagar-la-cargas-hechos-claves-subrogadas.html
Date: 2016-12-27T00:25:24.2605620+01:00
SecondaryDate: 2015-01-05T00:00:00.0000000
Author: Joy Mundy
Category: ETL y calidad de datos
RelatedUrl: http://www.kimballgroup.com/2015/01/design-tip-171-unclogging-fact-table-surrogate-key-pipeline/
IsDraft: true

---
Definimos el sistema ETL como una actividad “back room” que los usuarios nunca verán o tocarán. De todas maneras, el sistema ETL debe diseñarse según los requisitos del usuario. En este "consejo de diseño" miramos el diseño de una pieza de la ETL desde la perspectiva de las necesidades del usuario de negocio.

La **cadena de clave subrogada** es la 14 de 34 sistemas ETL descritos por Ralph Kimball en [la tercera edición del The Data Warehouse Toolkit][1]. Hay una [reseña de los subsistemas ETL en nuestra web][2], junto con un [artículo][3] de Bob Becker. La cadena de clave subrogada es normalmente el último paso del procesamiento de la tabla de hechos donde el sistema ETL intercambia claves del sistema fuente por claves subrogadas del data warehouse.

Para la mayor parte, la implementación de la cadena es un detalle dejado para el equipo ETL. Ellos decidirán si cargar los datos de hechos primero y si usar un widget de herramienta ETL, búsquedas o  combinaciones de bases de datos para hacer la tarea de intercambiar claves de sistema fuente por las claves subrogadas del DWH. en cualquier caso, sin embargo, el equipo de diseño ETL necesita información de la comunidad de usuarios de negocio sobre cómo manejar las filas problemáticas descritas en los escenarios siguientes.

### Falta de la clave del sistema fuente

La situación más simple se da cuando una fila de hechos de entrada pierde por completo una clave para una de las dimensiones. A veces una dimensión simplemente no se refiere a alguna de las filas de hecho. Por ejemplo, considera el escenario de una tabla de hechos que describe la venta de productos en las tiendas. Este modelo dimensional debe incluir una dimensión de promoción que describa las promociones de marketing y cupones asociados con la venta de un artículo. Pero muchas ventas no están asociadas con ninguna promoción: el código de promoción del sistema fuente está en blanco o nulo.

**Es absolutamente imperativo que cada fila de la tabla de hechos tenga una fila correspondiente en cada tabla dimensional.** Sería un error manejar este problema de falta de clave insertando una fila de hechos con una clave de promoción nula. Una clave foránea nula en la tabla de hechos violaría automáticamente la integridad referencial, y ninguna solución que implique OUTER JOINS tendría sentido para el negocio.

El problema de una clave ausente en el sistema de origen tiene una solución simple: poner una fila específica en la tabla dimensional para tratar esta circunstancia. Es común utilizar una clave subrogada de -1, y después rellenar las otras columnas de dimensiones con una descripción adecuada (ausente, desconocido, no disponible o no informado). Asegúrate de preguntar a los usuarios de negocio como quieren que se vean los atributos visibles en las filas de dimensiones ausentes.

### Mala fuente de clave de sistema

Un problema de diseño más importante es manejar una clave de sistema de entrada que el data warehouse no ha visto nunca antes. Las dimensiones deberían ser procesadas antes que los datos. De esta manera, bajo circunstancias normales, debería haber una fila en cada tabla de dimensiones para cada clave en una fila de hechos. Pero el proceso ETL debe contar con lo inesperado. Hemos visto muchas soluciones, ninguna de ellas es perfecta.

***Opción 1: Elimina la fila de hechos ausente.***

Hay pocos escenarios en los cuáles tendría sentido para los usuarios de negocio que el sistema eliminara filas de hechos entrantes. Eliminando las filas se distorsionarían los totales y recuentos, y podría existir contenido valioso en la fila.

***Opción 2: Registra la fila de hechos negativos con una tabla de error.***

Probablemente, esta es la solución más común. Es fácil de implementar y mantiene limpias la tabla de hecho y las tablas de dimensiones. Sin embargo, si tu sistema ETL simplemente escribe las filas de hechos problemáticas en una tabla de error y nunca son revisadas, el resultado es funcionalmente equivalente a eliminar la fila de hechos. El diseño correcto del sistema debe incluir dos procesos:

- Un proceso automatizado que revisa las filas con errores para ver si la dimensión ha aparecido y ya puede asignarse una clave subrogada.
- Un proceso manual mediante el cual una persona evalúa los errores antiguos y se comunica con los sistemas operacionales para solucionarlos en origen.

La mayor desventaja de este diseño es que los datos de hechos  no entran en la tabla de hechos. Esto estaría bien en algunos escenarios, pero sería muy problemático en otros casos (por ejemplo, en temas financieros, que necesitan cuadrar cifras).

***Opción 3: Asigna todas las filas de errores a una fila de dimensiones***

Esta solución es muy similar a la sugerencia de manejar un sistema de clave fuente ausente: todas las filas de hechos problemáticas se asignan a una única fila de dimensiones (llamémosla fila -2). Como el valor ausente, esta dimensión de fila desconocida tiene valores por defecto reconocible para todos los atributos, clarificando el propósito de la fila.

Hay dos ventajas para este enfoque:

1. Es extremadamente simple de implementar.
2. Devuelve la fila de hechos errónea a la tabla de hechos. En informes, los hechos erróneos se muestran asociados a la dimensión desconocida.

Sin embargo, hay dos problemas significantes en este enfoque: 1) diversas filas de hechos erróneos se agruparán en una única súper fila que no tendrá sentido empresarial; y 2) es difícil corregir los datos si mañana el ETL recibe los detalles correctos de la dimensión. Si es posible recibir posteriormente la información sobre la dimensión, los usuarios de negocio querrán que el ETL corrija la fila de hechos errónea para señalar la dimensión correcta.

Si los usuarios de negocio quieren que las filas de hechos sean re-enlazadas, necesitarás localizar el identificador de la dimensión del sistema fuente en algún sitio. Puedes:

1. Poner los identificadores del sistema origen en la tabla de hechos. Esta estrategia añadirá una amplitud significativa a la tabla de hechos y por lo tanto degradarán el rendimiento en la consulta.
2. Poner una clave primaria subrogada en la tabla de hechos, y registrar la condición de error en una tabla separada que contenga al mismo tiempo  los identificadores del sistema fuente y la tabla de hechos de la clave primaria. Utiliza la tabla de hechos de la clave primaria para encontrar las filas de hechos a actualizar cuando descubras una dimensión que ha llegado más tarde.

En cualquier caso, necesitarás emitir una sentencia UPDATE contra las filas de hecho apropiadas, re-situándolas desde -2 a la nueva clave de dimensión correcta. El  principal argumento a favor de este enfoque era su simplicidad de implementación, pero si necesitas corregir los datos de hechos, este enfoque no es simple.

***Opción 4: Crea una fila en la tabla de dimensión (técnica recomendada)***

La solución final al problema de una mala clave del sistema fuente es crear un "hueco" en la dimensión para acomodar el miembro misterioso. Si el ETL encuentra una mala clave del sistema fuente durante el procesado de la tabla de hechos, deberá parar, recorrer la tabla dimensional, crear una fila, obtener su clave subrogada, y devolver esa clave a la tabla de hechos de clave subrogada.

Los atributos de estas filas serán similares a los atributos de filas ausentes o desconocidas, pero no son exactamente lo mismo. Sabemos un dato sobre estos atributos: El código en el sistema origen.

Con esta técnica, el sistema ETL está en una posición ventajosa para arreglar el problema cuando recibe información de este miembro dimensional. El procesamiento normal de la dimensión actualizará los valores "desconocidos" a sus valores verdaderos y conocidos. La tabla de hecho no requerirá ningún tratamiento especial porque los hechos ya estarán correctamente asignados a la clave subrogada adecuada.

Esta técnica es arriesgada si existe un ratio elevado de ruido-señal. Si hay muchas filas de hechos problemáticas, se crearán muchos registros de dimensión dudosos, pero solo unos pocos se corregirán en futuras cargas. Esta es una de las razones por las que en sistemas ETL en tiempo real (donde datos sucios llegan cada pocos minutos o segundos), los datos en tiempo real son a menudo sustituidos por un lote de carga convencional al final del día (el cual tendrá mejor calidad, incluyendo el detalle completo de la transacción).

### Implicaciones de los requisitos empresariales

Tus decisiones de diseño sobre qué técnica utilizar para manejar filas de hechos problemáticas deben estar dirigidas por las siguientes necesidades de los usuarios de negocio:

1. ¿Quieren los usuarios de negocio que las filas de hecho negativas estén en la tabla de hechos para cuadrar las cifras? en caso negativo, redirige las filas con problemas hacia una tabla de error. Asegúrate que construyes procesos para sacarlos del apuro.
2. ¿Quieren los usuarios de negocio tratar las filas de hecho con correcciones de dimensiones que hayan llegado tarde? Si no, una posible solución es enlazar todos los hechos negativos al miembro desconocido (-2).
3. Si los usuarios de negocio quieren los hechos problemáticos en la tabla de hechos, y ellos quieren actualizar las dimensiones tan pronto como llegue mejor información, entonces el enfoque del "placeholder" es la apuesta mejor.
4. En todos los casos, alguien de la comunidad de usuarios de negocio debe intervenir sobre cómo tratar los errores y las filas falsas. También debe considerarse la ordenación de la dimensión (normalmente los usuarios de negocio quieren ver estas filas falsas en la parte superior de cualquier lista).

Este ejemplo sobre la "cadena de la clave subrogada" es una muestra de como el mejor diseño del sistema ETL sigue el mantra Kimball: **It’s all about the business.**





[1]: https://datawarehouse.es/ralph-kimball-books.html
[2]: http://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/etl-architecture-34-subsystems/
[3]: http://www.kimballgroup.com/2007/10/subsystems-of-etl-revisited/