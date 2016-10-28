---
UniqueId: dvYbjoFOOP
Title: Dos ideas potentes
Url: article/dos-ideas-potentes.html
Section: "Artículos"
Date: 2016-10-28T00:56:35.8899365+02:00
Description: xxx

---
Hay dos ideas potentes en la creación de los data warehouses de más éxito. Primero, separar tus sistemas. Segundo, construir estrellas y cubos.

En mi columna previa, describí un completo espectro de restricciones de diseño y realidades inevitables a las que debía enfrentarse un diseñador de data warehouse. Era una lista de grandes proporciones de la que me preocupaba que os hiciera coger la puerta y os fuerais. Pero, tal vez, lo que hizo seguir leyendo fue mi promesa de salir de la ciénaga. Aquí es donde entran las dos potentes ideas.

La última vez, indiqué que las restricciones no negociables del diseño de un data warehouse eran el entendimiento por parte del usuario final y la velocidad en la ejecución de la consulta. Un almacén de datos complejo y lento es un fracaso sin importar como de elegante pueda ser el resto del diseño porque la gente para el que fue destinado no querrá utilizarlo.

El resto de restricciones implicaban el reconocimiento de que el diseño del DWH es extremadamente complejo. La fuente de datos, las tecnologías de la base de datos y los entornos empresariales con los que tratamos son increíblemente complicados. Así que, como buenos ingenieros, para buscar nuestro camino hacia fuera de la ciénaga, debemos descomponer el problema en partes separadas manejables y poner énfasis en las técnicas que son predecibles, reutilizables y robustas.

### Separa tus sistemas

El primer paso crucial en el diseño de un almacén de datos en una empresa es separar tus sistemas lógica, física y administrativamente.

Encuentro muy útil pensar en el proyecto como cuatro sistemas distintos y diferentes, de los cuáles el administrador del DWH será el responsable de solo dos. No agobiaré con el típico bloque de diagramas porque estas cuestiones son más simples que eso.

Los cuatro sistemas son:

- Sistema transaccional de producción (ERP origen).
- Sistema data warehouse de almacenamiento
- Sistemas de presentación del DWH, incluyendo las herramientas de query, reporting y análisis, ya sean soluciones cliente/servidor o web.
- Opcionalmente, herramientas analíticas  de alta gama para la minería de datos, forecasting, valoración o asignación.

Como administrador del DWH no debes ser responsable de los sistemas origen que captan y procesan transacciones. Ese es el trabajo de otro. No tienes que verte involucrado en soportar las funciones de auditoría legal y financiera o las funciones de retroceso y recuperación de estos sistemas. Son los sistemas de cajas registradoras de la compañía, y sus prioridades son diferentes que las del data warehouse.

El primer sistema del cuál es responsable el diseñador del DWH es el área de almacenamiento de datos, donde los datos de producción de los distintos orígenes  es traído, limpiado, conformado, combinado y en última instancia entregado  a los sistemas de presentación del almacén de datos. 

Mucho se ha escrito sobre el crucial paso extraer-transformar-cargar (ETL) en el DWH, pero  alejándonos de este detalle, el principal requerimiento del área de almacenamiento es que está fuera de los límites para todos los clientes finales del almacén de datos. El área de almacenamiento es exactamente como la cocina en un restaurante. La cocina es un lugar concurrido, incluso peligroso, lleno de cuchillos afilados y líquidos calientes. Los cocineros están ocupados, centrados en la tarea de preparar la comida. No es apropiado permitir a los comensales entrar en una cocina profesional o permitir que los cocineros se distraigan con las cuestiones separadas de la experiencia culinaria. En términos de almacén de base de datos, prohibiendo el acceso de todos clientes del almacén de datos desde el área de almacenamiento, conseguimos:

1. Garantizar óptimos niveles de servicio para consultar o informar.
2. Cumplir con el nivel de seguridad del cliente.
3. Posibilidad de  construir índices y agregaciones  para el rendimiento de las consultas.
4. Manejar conflictos lógicos y físicos entre los pasos de consulta y limpieza de datos.
5. Garantizar la consistencia entre las distintas fuentes de datos

Las dos estructuras de datos dominantes para el almacenamiento de datos son el archivo de texto plano y el esquema entidad/relación, que son extraídos  directamente del sistema de producción. Casi todo el procesamiento en el área de almacenamiento (DWH) es clasificación o procesamiento secuencial simple.

---



El Segundo sistema más grande bajo el control específico del almacén de datos es el sistema de presentación. Por supuesto, este sistema es análogo al área del comedor de un buen restaurante. El área de comedor está diseñado para ofrecer confort a los comensales. La comida se sirve rápido y de la forma más atractiva, y los problemas y distracciones se evitan tanto como es posible. De la misma manera, el sistema de presentación del almacén de datos está construido con el propósito de mejorar la experiencia de consultar e informar. El sistema de presentación necesita ser simple y rápido y ofrecer los datos correctos que correspondan a las necesidades de análisis de los usuarios finales. También, en el sistema de presentación, podemos manejar con facilidad la lista enumerada con anterioridad sobre los requerimientos que habíamos excluido del área de almacenamiento.

Las estructuras de datos dominantes en el área de presentación son el esquema relacional de estrella y el cubo de datos de procesamiento analítico online (OLAP). El procesamiento en el área de presentación debe responder a una tormenta de grandes y pequeñas consultas procedentes de cualquier ángulo de los datos. A lo largo del tiempo, no habrá un patrón predecible a estas consultas. Algunos diseñadores le llaman el ataque ad hoc.

El cuarto sistema en nuestra lista es una capa opcional de herramientas analíticas específicas de alta gama que normalmente consume datos del almacén de datos en grandes lotes. Frecuentemente estas herramientas de extracción, predicción, valoración y distribución de datos utilizan algoritmos especializados fuera de la experiencia normal de un diseñador de almacén de datos.

Y, honestamente, muchos de estos procesos tienen un componente interpretativo o político que es sabiamente segregado del almacén de datos. Por ejemplo, la extracción de datos como disciplina es una tarea interpretativa compleja que involucra una completa colección de potentes técnicas analíticas, muchas de las cuales no son entendidas o creídas del todo por la comunidad de usuarios. La extracción propia de datos requiere un profesional experto en extracción de datos cualificado para utilizar las herramientas de manera efectiva y presentar los resultados de la extracción de datos a la comunidad.

Además, como he mencionado frecuentemente, hay un desajuste fundamental en la impedancia entre la extracción de datos y el almacén de datos. La extracción de datos frecuentemente necesita  examinar miles o millones de observaciones una y otra vez, a velocidades extremas de datos. No es fácil de sostener directamente desde el almacén. Es mejor manejar el set de observación desde un equipo de extracción de datos, sólo una vez.

Otro ejemplo de herramienta analítica de alta gama que el almacén de datos debe evitar es el sistema de  distribución para asignar costes a las distintas líneas de negocios en tu organización para calcular la rentabilidad global. No sólo puede ser un paso complejo de procesar fuera de las posibilidades de la mayoría de las herramientas de consulta e informe, también es un asunto delicado en política. Dejemos que el departamento financiero haga las asignaciones, y tu (el almacén de datos) estarás encantado de almacenar los resultados.



Estrellas y cubos simétricos 



La mayor parte de las áreas de representación hoy en día están dominadas por esquemas relacionales de esquemas de estrella y cubos de datos multidimensionales OLAP. Estas estructuras de datos han probado durante los  últimos 30 años ser las que entienden los usuarios finales. Recordad que el entendimiento es una de las dos restricciones no negociables en el diseño.

La simplicidad de los esquemas de estrella y cubos OLAP ha permitido a los brillantes diseñadores de software centrarse en los algoritmos más potentes para ofrecer un rendimiento rápido en las consultas. Recordad que la velocidad es la otra restricción no negociable en el diseño.

La simetría de ambas, el esquema de estrella y el cubo OLAP también permiten:

•	Interfaz de usuario predictiva que puede “aprender” qué hacer en el momento de la consulta.

•	Escenarios administrativos predictivos en el área de almacenamiento porque toda la estructura de datos tienen aspecto y sensación parecidos.

•	Respuestas de implementación predecibles cuando nuevos tipos de datos están disponibles.



Por supuesto, el esquema de estrella y el cubo OLAP están estrechamente relacionados. Los esquemas de estrellas son los más apropiados para los conjuntos grandes de datos, con muchos millones o billones de mediciones numéricas o muchos millones de miembros en una entidad de cliente o de producto.

Los cubos OLAP son los más apropiados para conjuntos de datos más pequeños donde las herramientas analíticas pueden realizar comparaciones de datos complejas y cálculos. En casi todos los entornos de cubos OLAP, se recomienda que introduzcas originalmente los datos en un esquema de estructura de estrella, y después utilices asistentes para transformar los datos en el cubo de OLAP. De esa manera, toda las complejas herramientas ETL del área de almacenamiento que tratan con los archivos planos y esquemas entidades/relaciones pueden ser parte de las fuentes de información de datos OLAP.  Y, por supuesto, el sistema de esquema híbrido estrella-OLAP permite que los grandes conjuntos de datos en formato de esquema estrella sean fácilmente desglosados en cubos de datos OLAP , todos bajo una única interfaz de usuario.











La gran recompensa

La gran recompensa final por construir  el sistema de representación en el almacén de datos sobre esquemas simétricos de estrella y cubos OLAP es el conjunto predecible de puntos comunes para encajar juntos datos desde toda la empresa.

En mi próxima columna, pondré al descubierto las técnicas de conformar las definiciones de las dimensiones y hechos en todas las dispares fuentes de datos de tu gran empresa.  Estas dimensiones conformadas y hechos serán la base de un almacén de datos de aquitectura de bus--- un conjunto de puntos de conexiones estándar que proporcionan poder a todas las líneas de transmisiones, y justo como el bus en tu ordenador proporciona datos a todos los periféricos. 

¿Qué hemos realizado?

Hasta ahora hemos implementado dos ideas potentes. Primero, hemos separado lógicamente, físicamente y administrativamente los sistemas de nuestro entorno en 4 distritos distintos. Realmente necesitas 4 sistemas informáticos diferentes, pero ¡eres responsable solo de dos de ellos! Nuestros dos sistemas de almacenes de datos también nos permiten separar las responsabilidades incompatibles del almacenamiento de datos y las consultas de los usuarios finales. Segundo, hemos llenado nuestra área de presentación del almacén de datos con esquemas de estrellas y cubos de datos OLAP, las únicas estructuras entendibles, y que pueden soportar el ataque ad hoc.

Aunque claramente, no hemos direccionado todas las  complejas restricciones de diseño y realidades inevitables, hemos neutralizado en gran parte el desafío global, solamente impulsando estas dos potentes ideas. Vuelve a mi columna previa y revisa la lista. Hemos dirigido grandes partes comprensibilidad, rapidez en las consultas, los tres costes mencionados en esa columna, los riesgos de la centralización inapropiada, la necesidad de desarrollo incremental, manejar continuos cambios consistentes en pequeñas y grandes sorpresas y como pensar en el papel de los data marts. Quizá hay esperanza.