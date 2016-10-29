---
UniqueId: mneNSHknqU
Title: "Consejo de diseño #135. Las dimensiones conformadas como fundamento de un data warehouse ágil"
Url: 2011/135-dimensiones-conformadas-fundamento-datawarehouse-agil.html
Section: "Artículos"
Date: 2011-06-01T00:00:00.0000000+02:00
Author: Margy Ross

---
## Consejo de diseño \#135.

Algunos clientes y estudiantes lamentan que cuando quieren entregar y compartir dimensiones maestras definidas consistentemente en el entorno DWH/BI, simplemente, "no se puede". Explican que lo harían si pudiesen, pero con una alta dirección centrada en utilizar técnicas de desarrollo ágil para ofrecer soluciones DW/BI, es "imposible" tener tiempo para llegar a un acuerdo en toda la organización sobre dimensiones conformadas. Quiero llevar a cabo este argumento al revés afirmando que **las dimensiones conformadas permiten el desarrollo ágil de DW/BI  y agiliza la toma de decisiones**.

Seáis o no un especialista Kimball, muchos de vosotros estáis familiarizados con las dimensiones conformadas. Una dimensión conformada es un dato maestro descriptivo que está referenciado en múltiples modelos dimensionales. Las dimensiones conformadas son fundamentales dentro del enfoque Kimball. Las dimensiones conformadas permiten a  los usuarios del DW/BI cortar y fragmentar constantemente las métricas de rendimiento de los procesos de fuentes de datos procedentes de múltiples empresas. Las dimensiones conformadas permiten que datos de diferentes fuentes sean integrados basándose en atributos de dimensiones comunes y unificados. Finalmente, las dimensiones conformadas permiten construir y mantener una tabla de datos una sola vez en lugar de recrear versiones ligeramente diferentes durante cada ciclo de desarrollo.

Reutilizando dimensiones conformadas entre proyectos es  donde se obtiene un impulso para obtener más agilidad en el desarrollo  DW/BI. A medida que desarrollas tu portfolio de dimensiones maestras conformadas, la manivela de desarrollo del proyecto empieza a girar más rápido.

El *time-to-market* de una nueva fuente de datos empresarial se reduce a medida que los desarrolladores reutilizan dimensiones conformadas existentes. Finalmente, los nuevos desarrollos ETL se centrarán casi exclusivamente en entregar más tablas de hechos a medida que las tablas de dimensiones asociadas preparadas en su estantería listas para ser utilizadas.

**Definir una dimensión conformada requiere un consenso organizacional y compromiso en la organización.** No es necesario que todos estén de acuerdo con cada atributo en cada tabla dimensional. Como mínimo, deberías identificar un subconjunto de atributos que tenga significado en la empresa. Estas características descriptivas comunes son el punto de partida de los atributos conformados.

A lo largo del tiempo, puedes expandir este sencillo punto de partida de manera iterativa. Ninguno de los atributos dimensionales especiales procedentes de un solo proceso empresarial no deben ser descartados durante el proceso de conformación (asumimos muy pocos compromisos definitivos). Y finalmente, si tu organización ya ha implementado la capacidad de *master data managment* (MDM) para apoyar el sistema de fuente transaccional, el trabajo de crear dimensiones conformadas para el almacén de datos de la empresa, será más sencillo.

**Si fallas en concentrarte en las dimensiones conformadas porque estás bajo presión de entregar alguna cosa, los distintos departamentos analíticos de silos de datos probablemente tendrán categorías y etiquetas inconsistentes**. O peor: Los conjuntos de datos ofrecerán la apariencia de poder ser comparados y integrados debido a la similitud de sus etiquetas, pero las reglas empresariales subyacentes serán ligeramente diferentes. Los usuarios de negocio malgastarán cantidades exageradas de tiempo intentando componer y resolver estas inconsistencias en los datos que influirán negativamente en su agilidad para la toma de decisiones.

Los altos directivos de IT que demandan desarrollo de sistemas ágiles, si están interesados en conseguir eficiencia en el desarrollo a largo plazo y quieren una toma de decisiones efectiva en la empresa a largo plazo, deberán ejercer incluso una mayor presión a la organización para definir dimensiones conformadas consistentes .

Artículo original: [http://www.kimballgroup.com/2011/06/design-tip-135-conformed-dimensions-as-the-foundation-for-agile-data-warehousing/][1]





[1]: http://www.kimballgroup.com/2011/06/design-tip-135-conformed-dimensions-as-the-foundation-for-agile-data-warehousing/