---
UniqueId: BtrVMUvMvT
Title: "Consejo de diseño #179: Principios clave del método Kimball"
Url: 2015/claves-metodo-kimball.html
Date: 2016-12-24T02:34:16.0167205+01:00
SecondaryDate: 2015-11-05T00:00:00.0000000
Description: "Me apasionan varios principios del método Kimball. En este consejo de diseño enumero las cosas que repito una y otra vez, tanto a las audiencias experimentadas como a las nuevas."
Author: Joy Mundy
Category: "Fundamentos diseño dimensional"
RelatedUrl: http://www.kimballgroup.com/2015/11/design-tip-179-key-tenets-of-kimball-method/

---
La mayor parte de la guía del método Kimball para el diseño, desarrollo e  implementación de un sistema DW/BI es simplemente eso: una guía. Existen cientos o miles de reglas en los numerosos libros de Kimball Group y confieso haber quebrantado muchas de esas reglas durante décadas, cuando afrontaba objetivos conflictivos o realidades políticas desagradables.

Me apasionan varios principios del método Kimball. En este consejo de diseño enumero las cosas que repito una y otra vez, tanto a las audiencias experimentadas como a las nuevas. Cualquier lector que haya asistido a una de mis clases, o me haya contratado como consultor, habrá oído mi discurso sobre la mayoría o todos los puntos de esta lista.

### 1. El modelo dimensional es el activo clave.

El método Kimball, tal y como se explica en la segunda edición de The Data Warehouse Lifecycle Toolkit, se centra en el modelo dimensional. Los principios del modelado dimensional representan las contribuciones más conocidas de Ralph Kimball y Kimball Group, en el mundo de la inteligencia de negocios. Es nuestro foco ya que un buen modelado dimensional es absolutamente vital para el éxito de tu proyecto DW/BI. Si obtienes el modelo correcto, y lo completas con integridad, el resto será sencillo.

### 2. El modelado dimensional es una actividad de grupo.

Incluso el mejor modelador dimensional creará un modelo dimensional pobre si trabaja solo. El modelado dimensional no es solamente una actividad en grupo, es una actividad en grupo que debe involucrar a la comunidad de usuarios de negocio. A lo largo de los años, hemos rechazado incontables solicitudes de consultoría para diseñar modelos sin aportación empresarial. O, peor, hemos luchado en proyectos costosos donde no se materializó la participación de los usuarios de negocio prometida.

Es sin duda una enorme solicitud que hace la comunidad de usuarios. Nuestro proceso de diseño requiere normalmente 50-60 horas en sesiones repartidas en un período de entre 4 a 6 semanas (o más, dependiendo de la complejidad). La gente que queremos que participen en las sesiones de diseño son siempre valiosas y exigentes. Pero si no los podemos convencer para que dediquen el tiempo y la energía necesarios, el sistema resultante fallará.

### 3. El modelo dimensional es la mejor concreción para el sistema de DW / BI.

La mayoría de clientes con los que trabajo no tienen una concreción escrita para el sistema DW/BI, ciertamente no tienen ningún documento  que refleje la realidad de manera significativa. El modelo de especificación más común incluye listas confusas de lo que los usuarios quieren filtrar y desglosar, así como la demanda de que los 2.000 informes existentes sean mantenidos en el nuevo sistema. Si todo lo que hemos conseguido con nuestro nuevo sistema DW/BI es resituar los  informes enlatados existentes, hemos fallado.

Al final del proceso de diseño les pedimos a los usuarios de negocio que piensen sobre los análisis que han hecho recientemente, que han intentado hacer, o que nos gustaría que hubiesen hecho, de la información en el ámbito de aplicación del diseño actual. Queremos que nos digan “Sí, este modelo satisface nuestras necesidades”. Al mismo tiempo, la gente IT del equipo han estado supervisando la discusión de las fuentes de datos, transformaciones y otros detalles técnicos. Les pedimos que afirmen “Si, podemos llenar este modelo de datos”. La redacción del diseño del modelo dimensional, es una especificación significativa y factible de los requisitos.

### 4. El modelo dimensional debe añadir valor más allá de la reestructuración.

Algunas de las mejoras más valiosas que puedes implementar en tu sistema DW/BI son añadir descriptores mejorados y grupos de datos usados frecuentemente. Normalmente, estas posibilidades se olvidan por parte del equipo de diseño. Incluso he encontrado equipos con una política explícita de no añadir nada más allá de lo que está en el sistema fuente.

Ejemplos de incorporaciones valiosas al modelo de datos que incluyen: 

- Permitir que los usuarios puedan filtrar códigos de transacción que se usan pocas veces.
- Proporcionar columnas de clasificación para listas de selección e informes que sean atractivas.
- Bandas pre-calculadas, como rangos de edad o medidas de calidad para las transacciones.
- Dar soporte a diferentes jerarquías, o más profundas, de las que son administradas en los sistemas de origen, como marketing frente a las visiones de fabricación de la dimensión del producto.

### 5. Los sistemas de gestión de datos maestros son una gran fuente.

La de-duplicación es una de las tareas más difíciles a las que se enfrenta el equipo de data warehouse. Desde los inicios del data warehousing, el equipo de mantenimiento ha luchado para diseñar los procesos ETL que deduplicaran entidades como la dimensión "cliente". La popularidad creciente y la funcionalidad de la tecnología y programas del master data management (MDM) proporcionan una mejor solución que el flujo ETL. ¡Y no solo porque es duro!. El ritmo del proceso ETL,  donde queremos que la base en funcionamiento sea a prueba de balas, es  un inconveniente con el proceso de deduplicación.

No importa lo buenas que sean nuestras herramientas, como de inteligente sea nuestro código, como de completas sean nuestras reglas de negocio, el proceso automatizado de deduplicación no puede alcanzar el 100% de precisión. Se requiere una persona para juzgar los casos cuestionables. Funciona mucho mejor si esto es una responsabilidad profesional durante los días laborables, más que esperar a la carga del ETL.

Un simple sistema MDM se puede utilizar para construir datos con valor añadido debatidos previamente en este consejo de diseño. Las dimensiones jerárquicas se hacen notar más cuando son estructuradas desde los escritorios de los usuarios; las herramientas MDM proporcionan una plataforma simple para gestionar esta información.

### 6. Que no se excluya el data warehouse relacional.

Diseñar y llenar el data warehouse conformado de una empresa es duro. A todo el mundo le gustaría saltarse este paso. A lo largo de los más de 23 años de carrera en data warehousing, he observado  numerosos intentos para simplificar el proceso, desde construir la capa BI directamente desde el sistema de transacciones, hasta los llamados data warehouses virtuales, volviendo al ciclo actual de scripts de creación de  herramientas de visualización. 

Como comenté en el [Consejo de diseño #162][1], aprovecha las herramientas de visualización de datos, pero evita la anarquía. A menos que controles completamente toda tus datos de origen, debes dejar el ETL para la herramienta ETL, dejar el almacenamiento de datos y la gestión a un motor de database relacional, y dejar que las herramientas BI sean brillantes en lo que mejor hacen: grandes visualizaciones y experiencia de usuario.



### 7. Todo se trata del negocio.

Digo esto muchas veces durante mis clases y consultas. Es la característica más importante del método Kimball Lifecycle: Mantén el foco incuestionablemente en el negocio. Afecta a todo lo que hacemos y es el mensaje más importante que transmitir hacia el futuro.

## Looking Forward

Me siento increíblemente afortunado de haber formado parte del Kimball Group durante los últimos 12 años. Ha sido un honor trabajar no solo con Ralph, Margy, Bob y Warren, sino también con los cientos de estudiantes, clientes y colegas con los que he aprendido tanto.  Me honra lo que hemos conseguido juntos, y me siento bendecida por lo que considero el mejor trabajo del mundo.

Me siento particularmente bendecida en comparación con Warren, falleció el año pasado de cáncer cerebral y su muerte me afectó profundamente tanto a mí como al toda la gente del Kimball Group. En 2016, homenajearé a Warren y su pasión por el fitness haciendo ciclismo por todo Estados Unidos para recaudar fondos para la investigación del cáncer cerebral. Me honrará si sigues nuestro blog que narrará este viaje ([https://itinerantphilosopher.org/][2]) y me emocionaré si consideras hacer una donación por nuestros esfuerzos para recaudar fondos de investigación para el cáncer cerebral.











[1]: http://www.kimballgroup.com/2014/01/design-tip-162-leverage-data-visualization-tools-but-avoid-anarchy/
[2]: https://itinerantphilosopher.org/