---
UniqueId: oZTgDLUJsb
Title: "Consejo de diseño #175: No hay magia de base de datos"
Url: 2015/no-hay-magia-en la-base-de-datos.html
Date: 2016-12-27T10:07:04.2353977+01:00
SecondaryDate: 2015-06-08T00:00:00.0000000
Description: "Las tecnologías de almacenamiento de datos ofrecen cada vez mejores ventajas al arquitecto del DWH y llega a parecer mágico. ¿Debes aferrarte a las bases de datos relacionales para hospedar tu data warehouse? ¿Debes usar cubos? ¿O debes dirigirte hacia las últimas y mejores soluciones de almacenamiento de datos como el MPP (massively parallel processing ) o las soluciones columnares? ¿O la nube?"
Author: Joy Mundy
Category: ETL y calidad de datos
RelatedUrl: http://www.kimballgroup.com/2015/06/design-tip-175-there-is-no-database-magic/

---
Las tecnologías de almacenamiento de datos ofrecen cada vez más opciones al arquitecto del DWH y llega a parecer mágico. ¿Cómo puedes saber qué hacer? ¿Debes aferrarte a las bases de datos relacionales, probadas y verificadas, para hospedar tu data warehouse? ¿Debes usar cubos? ¿O debes dirigirte hacia las últimas y mejores soluciones de almacenamiento de datos como el MPP (massively parallel processing ) o las soluciones columnares? ¿O la nube?

Primero, hablemos de la base de datos del data warehouse, el almacén principal donde se mantienen los modelos dimensionales con granularidad atómica. Todo aquel que sigue el método Kimball tendrá uno. Su diseño, cuidado y alimentación son el centro de atención del método Kimball. Adicionalmente, debes tener cubos adicionales u otras estructuras, pero por ahora hablemos de la base de datos del DWH atómico.

En la mayoría de las organizaciones en donde he trabajado utilizan un RDBMS para hospedar las estrellas dimensionales con granularidad  atómica. Observamos muchos servidores SQL Server e implementaciones Oracle, un menor número de DB2, y una minoría creciente de otras plataformas y tecnologías. Y son estas tecnologías alternativas las que están generando la mayor parte de preguntas entre nuestros clientes.

Para la mayoría de organizaciones, el viejo enfoque RDBMS funciona bien. Adquieres un servidor interno o un entorno virtual en la nube, instalas y configuras el RDBMS para cargar el data warehouse, implementas y estableces tus modelos dimensionales, conectas tus herramientas BI y ya estás listo para disfrutar de tu entorno analítico.

Una mayoría aplastante de data warehouses subterabyte usan uno de los tres grandes RDBMS para hospedar el modelo dimensional atómico. Estos sistemas BI, si fallan, no lo harán debido a la elección en la tecnología de la base de datos.

El paisaje es marcadamente diferente para los sistemas data warehouse muy grandes. Sobre los 10 TB y con seguridad para 100 TB o más, es altamente probable que encuentres  tecnologías alternativas.

### Tecnologías alternativas de bases de datos atómicas.

Generalizando, hay dos tipos principales de tecnología de base de datos que concentran mucho interés ahora mismo:

- MPP (Massively parallel processing) que escalan a través de pequeños servidores antes que escalar un único y más grande SMP. Los "appliances"as MPP disponibles incluyen Teradata, IBM’s Netezza, EMC’s Greenplum, Oracle’s Exadata, and Microsoft’s PDW, entre otros. Nosotros incluimos las soluciones de código abierto Hadoop, como Hive y Impala, en esta categoría de implementaciones distribuidas de manera masiva.
- Bases de datos columnares, que invierten tus tablas al almacenar los datos por columna en lugar de por fila. Algunas de las soluciones columnares son también masivamente paralelas. Las bases de datos columnares más populares incluyen SAP Sybase IQ, InfoBright, HP Vertica, ParAccel (que hospeda Amazon’s RedShift), y EXASOL, entre otras. Una estructura de almacenamiento de datos en Hadoop es el "Parquet columnar format", que puede ser implementado de manera efectiva con ambos Hive e Impala, y proporciona beneficios equivalentes a una solución de base de datos columnar que no sea Hadoop.

La mayoría de las tecnologías se encuentran ya fuera de la fase experimental: Teradata lleva alrededor de  35 años, SAP Sybase IQ más de 20 años, y la mayor parte del resto entre 10 y 15 años.

### ¿Qué ofrecen estas “nuevas” tecnologías?

En comparacio´n con los RDBMS estándar, estas tecnologías ofrecen una escalabilidad sustancial y mejoras en el rendimiento de  las consultas para el trabajo analítico. Lo que no ofrecen es magia. ¡No hay magia! Pagas por la mejora en las consultas y la escalabilidad utilizando el dinero: estos productos suelen ser más caros que la correspondiente licencia RDBMS estándar más el coste del servidor. Pagas por los beneficios a través de un sistema ETL algo más complejo. He visto a mucha gente quejarse de estas tecnologías y a otra tanta situarse a favor de ellas. Cada tecnología tiene sus propias debilidades, las actualizaciones o borrados parecen particularmente problemáticos para bastantes de ellas. No obstante, si tienes un sistema grande – medido tanto por los volúmenes de datos como por la complejidad en su uso- lo entenderás y conseguirás que funcione. Esta es una más de la larga serie de decisiones donde ponemos carga en el ETL para proporcionar una magnífica experiencia para el usuario.

Para las data warehouses muy grandes, estos MPP y/o tecnologías columnares son muy recomendables. Sin embargo, para la gran mayoría de  entornos BI, pequeños o medianos, estas tecnologías son simplemente demasiado costosas (en términos económicos, experiencia, complejidad del sistema, ...) para siquiera considerarlos. La decisión se vuelve más difícil en sistemas entre medianos y grandes (desde 750 GB hasta 10 TB). Los factores que apuntan hacía estas nuevas tecnologías uncluyen:

1. Gran volumen de datos.
2. Un crecimiento esperado  muy elevado (los sistemas MPP escalan con mayor facilidad)
3. Requisitos ETL más simples con actualizaciones mínimas de las tablas de hechos (los cuales tienden a ser más problemáticos en estas tecnologías)
4. Una arquitectura que utiliza poco o ninguna OLAP o tecnología de cubos, y  que por lo tanto tiene una gran y diversa carga de consultas directamente en la base de datos del data warehouse.

### ¿Qué hay de los Cubos?

Las decisiones de arquitectura no se pueden tomar nunca de manera aislada. Tu decisión sobre la base de datos para el data warehouse influye y está influenciada por tu arquitectura de descarga y por como utilizas los cubos. Cuanto más preveas utilizar cubos (o si existen cubos OLAP antiguos o estructuras de memoria más recientes), menos acceso directo ad hoc necesitarás para la base de datos atómica del data warehouse. En una posición extrema inusual, donde el 100% del acceso del usuario se da a través de cubos, la base de datos DW se relega básicamente a la existencia de una back room.

A tus usuarios de negocio  suelen encantarles los cubos. Ofrecen una experiencia de usuario agradable para las consultas ad hoc porque están básicamente diseñadas para datos dimensionales. Proporcionan un lugar para definir y compartir de manera centralizada cálculos complejos. Y normalmente proporcionan un buen rendimiento en las consultas.

Tal y como comentamos en el [Consejo de diseño 162][1], la base de datos relacional atómica del data warehouse es una pieza requerida de la arquitectura. Los cubos, por mucho que nos encanten, son opcionales. Sin embargo, la inclusión de los cubos en tu arquitectura afecta tu decisión sobre la tecnología para la base de datos del data warehouse.

### ¿Y qué hay sobre la nube?

Este consejo de diseño menciona la nube en el primer párrafo, así que también acabaremos allí. Los vendedores de la herramienta y la base de datos han estado haciendo inversiones sustanciales en el hosting de la nube de sus herramientas y de sus servicios. Pero entre las implementaciones dela data warehouse/BI con las que trabajamos hemos observado poca aceptación. Primero, se da la inevitable decepción cuando la gente se da cuenta de que una herramienta en la nube es sólo eso: casi la misma vieja herramienta, ahora en la nube. No hay ninguna magia, todavía necesitas escribir el ETL.

Los primeros en adoptar son las organizaciones que ya han hecho una inversión en el hosting de sus sistemas operacionales en la nube. Asumir volúmenes de datos razonables, utilizando la nube para hospedar sus entornos DW/BI, es razonable y probablemente supone un coste rentable.

Algunas organizaciones e industrias no lo adoptarán enseguida, porque tienen prohibido por ley o política mover los datos a un entorno de nube. Pero estoy seguro de que es un goteo que llegará a convertirse en un torrente. Los modelos de precios se volverán más atractivos, y la noción de mantener los servidores y bases de datos in house pasará de moda. Pero no tengo una bola mágica que me diga cuándo sucederá este cambio.





[1]: http://www.kimballgroup.com/2014/01/design-tip-162-leverage-data-visualization-tools-but-avoid-anarchy/