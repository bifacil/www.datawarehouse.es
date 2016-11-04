---
UniqueId: KahpyQlsoo
Title: "Consejo de diseño #115: El ciclo de vida Kimball resumido."
Url: 2009/ciclo-de-vida-kimball.html
Section: "Artículos"
Date: 2016-11-03T00:00:00.0000000
SecondaryDate: 2009-08-04T01:30:00.0000000
Description: "Nuestro enfoque para el diseño, desarrollando una y otra vez soluciones DW/BI, está probado y verificado. Ha sido utilizados por miles de equipos de proyectos virtualmente en todas las industrias, en el área de aplicación, en las funciones de negocio y en las plataformas de negocio. Una y otra vez se ha comprobado que el enfoque del Lifecylce Kimball funciona."
Author: Margy Ross
Category: "Planificación y gestión de proyectos"
RelatedUrl: http://www.kimballgroup.com/2009/08/design-tip-115-kimball-lifecycle-in-a-nutshell/

---
Recientemente, en una clase sobre el Lifecycle del Data Warehouse un estudiante me preguntó a cerca de una visión resumida del enfoque Kimball para compartirlo con su director. Estaba contento de que me preguntara porque asumía que habíamos publicado un resumen ejecutivo sobre esto. Para mi sorpresa, nuestra única publicación sobre la visión del ciclo de vida era un capítulo en un libro de  herramientas de ayuda. Este consejo de diseño incluye el vacío de contenido inesperado en nuestros archivos.

El enfoque Kimball sobre el ciclo de vida lleva décadas siendo utilizado. Los conceptos fueron inicialmente establecidos en 1980 por miembros de Kimball Group y diversos colegas de *Metaphor Computer Systems*. Cuando publicamos la metodología por primera vez en The Data Warehouse Lifecycle Toolkit en 1998, lo  llamamos *"Business Dimensional Lifecycle"* porque este título reforzaba tres conceptos fundamentales:

- Se centraba en añadir valor empresarial en la organización
- Estructuraba  dimensionalmente los datos proporcionados a Negocio vía reportes y consultas.
- Desarrollaba iterativamente la solución en incrementos manejables antes que facilitar un Big Bang que se pudiese entregar.

Rebobinando a 1990, nuestra metodología fue una de las pocas que recalcaban este conjunto de principios centrales, por tanto el título El “Business Dimensional Lifecycle” diferenciaba nuestro enfoque de otros existentes en la industria.

En un avance rápido hacia 2008 cuando publicamos la segunda edición de “*The Data Warehouse Lifecycle Toolkit*”, todavía creemos absolutamente en estos conceptos, pero la industria ha evolucionado. Nuestros principios habían llegado a ser la corriente principal en las mejores prácticas ofrecidas por muchos, así que reducimos el nombre oficial de la metodología a simplemente "*Kimball Lifecylce*".

A pesar de  los avances dramáticos en tecnología y entendimiento durante las dos últimas décadas, las construcciones básicas del ciclo de vida de Kimball han permanecido sorprendentemente constantes.

Nuestro enfoque para el diseño, desarrollando una y otra vez soluciones DW/BI, está probado y verificado. Ha sido utilizados por miles de equipos de proyectos virtualmente en todas las industrias, en el área de aplicación, en las funciones de negocio y en las plataformas de negocio. Una y otra vez se ha comprobado que el enfoque del *Lifecylce Kimball* funciona.

El enfoque Kimball del Ciclo de vida se ilustra en la Figura 1. Una implementación exitosa del DW/BI depende de una combinación apropiada de numerosas tareas y componentes.  No es suficiente tener un modelo de datos perfecto o la mejor tecnología desarrollada. El diagrama del ciclo de vida es la hoja de ruta general que describe las tareas requeridas para un diseño efectivo, desarrollo y despliegue.

![Imagen 1](https://datawarehouse.es/images/dt-115-lifecycle-kimball.png)

Figura 1. El diagrama del Ciclo de vida Kimball.

### Dirección y planificación del programa/proyecto

El primer recuadro en la ruta se centra en conseguir lanzar el programa/proyecto, incluyendo la determinación del alcance, la justificación y la dotación de personal. A lo largo del ciclo de vida, la planificación y la dirección de las tareas del proyecto mantiene las actividades en marcha.

### Requisitos del negocio

Identificar las necesidades del negocio es una tarea clave en el ciclo de vida Kimball ya que estos descubrimientos dirigen la mayoría de decisiones ascendentes y descendentes. Las necesidades se recogen para determinar los factores clave que repercuten en el negocio centrándose en lo que los usuarios de negocio hacen hoy (o lo que quieren hacer en el futuro) en lugar de preguntar “¿Qué quieres almacenar en el almacén de datos?”, se identifican las oportunidades más significativas en la empresa, basándose en el valor del negocio y su viabilidad. Posteriormente las necesidades detalladas son recopiladas en la primera iteración del sistema de desarrollo DW/BI. Tres rutas paralelas del ciclo de vida siguen la definición de las necesidades.

### Ruta de tecnología

Los entornos DW/BI se encargan de la integración de numerosas tecnologías, almacenes de datos y metadatos asociados. La ruta de la tecnología empieza con el diseño del sistema de arquitectura para establecer una lista de la compra de las capacidades necesarias, seguidas de la selección e instalación de productos que satisfagan esas necesidades de la arquitectura.

### Ruta de datos

La ruta de datos empieza con el diseño de un modelo dimensional como objetivo para enfrentarse a los requisitos del negocio, mientras consideramos las realidades de datos subyacentes. El mundo Kimball es sinónimo del modelo dimensional donde los datos se dividen en medidas o dimensiones descriptivas. Los modelos dimensionales pueden estar instanciados en bases de datos relacionales, referidas como esquemas de estrella, o bases de datos multidimensionales, conocidas como los cubos OLAP. Independientemente de la plataforma, los modelos dimensionales intentar dirigirse a dos objetivos simultáneos: facilidad de uso desde la perspectiva de los usuarios y rápidez en la realización de la consulta.

La matriz empresarial (EDW Bus Matrix) es un entregable clave ya que representa  el núcleo de los procesos empresariales de la organización y las dimensiones conformadas comunes asociadas. Es el prototipo de datos para asegurar la integración de la empresa de arriba abajo con un intercambio manejable desde abajo hacia arriba centrándose en un único proceso de negocio a la vez. La matriz en bus  es tremendamente importante porque sirve simultáneamente como una guía técnica, una guía para gestionar, y un fórum para comunicarse con ejecutivos.

El modelo dimensional se convierte en un diseño físico donde las estrategias de optimización del rendimiento son tomadas en cuenta. De esta manera, se aborda el diseño del sistema ETL y los desafíos para el desarrollo. El ciclo de vida describe 34 subsistemas del proceso de extracción transformación y carga (ETL). Estos subsistemas  se agrupan en 4 operaciones mayores: (1) extraer los datos de la fuente, (2) llevar a cabo la limpieza y el ajuste de las transformaciones; (3) proporcionar los datos en la capa de presentación y (4) dirigir el proceso ETL de mantenimiento y entorno.

### Ruta de la inteligencia de negocios

Mientras que algunos miembros de proyectos están inmersos en la tecnología y los datos, otros se centran en identificar y construir un amplio rango de  aplicaciones BI, incluyendo informes estandarizados, consultas parametrizadas, tableros, cuadros de mando, modelos analíticos, aplicaciones de extracción de datos junto con las interfaces de navegación asociadas.

### Implementación, mantenimiento y crecimiento:

Las tres rutas de ciclos de vida convergen en la implementación, reuniendo la tecnología, los datos y las aplicaciones BI. La implementación iterada entra en fase de mantenimiento, mientras que el crecimiento vuelve atrás hacia la planificación del proyecto para la próxima iteración del sistema DW/BI. Recuerda que el sistema DW/BI es un proceso a largo plazo, ¡no un proyecto que se hace una sola vez!.

A lo largo del ciclo de vida Kimball, hay un tema recurrente reconociendo que los profesionales del DW/BI deben combinar los requerimientos de los usuarios de negocio con las realidades subyacentes de los datos fuente, tecnología y recursos relacionados. Los equipos de proyecto que se centran exclusivamente en las necesidades (o realidades) de un modo aislado, inevitablemente tendrán que hacer frente a riesgos en forma de retrasos en las entregas y/o adopción por parte de los usuarios.

Finalmente, como hemos dicho en otras ocasiones, y seguro que repetiremos en el  futuro: Independientemente de los objetivos específicos DW/BI de tu organización, creemos que tu objetivo general debe ser la aceptación por parte de los usuarios de negocio del DW/BI para el apoyar la toma de decisiones. Este objetivo debe permanecer en el ojo del huracán durante el diseño, desarrollo e implementación del ciclo de vida de cualquier sistema data warehouse o business intelligence.

Artículo original: [http://www.kimballgroup.com/2009/08/design-tip-115-kimball-lifecycle-in-a-nutshell/][1]





[1]: http://www.kimballgroup.com/2009/08/design-tip-115-kimball-lifecycle-in-a-nutshell/