﻿---
UniqueId: yCgqODiWRH
Title: "Consejo de diseño #158: Dar sentido a la capa semántica"
Url: 2013/dar-sentido-capa-semnatica.html
Image: 2013-dar-sentido-capa-semnatica.jpg
Date: 2016-11-03T00:00:00.0000000
SecondaryDate: 2013-08-05T00:32:00.0000000
Description: "Uno de los componentes clave de la arquitectura de un sistema Business Intelligence es la capa semántica. La capa semántica proporciona la traducción de las estructuras subyacentes de la base de datos en términos y construcciones orientados  al usuario de negocio."
Author: Joy Mundy
Category: Fundamentos Business Intelligence
RelatedUrl: http://www.kimballgroup.com/2013/08/design-tip-158-making-sense-of-the-semantic-layer/

---
Uno de los componentes clave de la arquitectura de un sistema Business Intelligence es la capa semántica. La capa semántica proporciona la traducción de las estructuras subyacentes de la base de datos en términos y construcciones orientados  al usuario de negocio. Normalmente es una parte integrante de la herramienta de consulta e informe. Los sistemas OLAP o las bases de datos multi-dimensionales (cubos) también incluyen una capa semántica.

Algunas sistemas BI proporcionan capas semánticas microscópicamente pequeñas, otras son ricas y robustas. La funcionalidad mínima para poder calificarse como una capa semántica incluye:

- Una organización que presenta los elementos de modo intuitivo para la gente de negocios. En la mayoría de las herramientas organizarás las tablas y columnas en estructuras de carpetas. El método Kimball te aconseja estructurar dimensionalmente tu almacén de datos, lo cuál te sitúa por delante de aquellos que intentan lanzar BI directamente sobre estructuras de bases de datos transaccionales. Pero incluso si rehaces sobre un modelo dimensional limpio, una capa semántica proporcionará oportunidades para mejorar la navegabilidad y la capacidad de encontrar.
- Una oportunidad para renombrar los elementos de datos para que los usuarios de negocios encuentren sentido. Por supuesto, el método Kimball recomienda encarecidamente que las tablas y columnas del almacén de la base de datos sean nombrados como a los usuarios les gustaría verlos, pero esta funcionalidad se implementa a veces en la capa semántica.
- Una interfaz  para mantener las descripciones de los elementos de datos orientadas al negocio. Idealmente, tu almacenas y expones múltiples tipos de metadatos: descripciones de negocios, valores de ejemplo, y la política de cambios en las dimensiones de los atributos. En un mundo perfecto, la capa semántica de la BI expondría al completo el número de líneas de cada elemento: que sistema de transacción de datos era la fuente, que operario de ETL tocó  este atributo, y cuando fue cargado en el almacén de datos. De modo realista, pocas capas semánticas soportan más de una única descripción para cada elemento de datos. En mi experiencia, la mayoría de equipos de DW/BI ni se molestan en enriquecer esa única descripción.
- Un mecanismo para definir reglas de cálculo y agregación. Por ejemplo, debes definir en el modelo semántico que balances de inventario son aditivos a través de la mayoría dimensiones, pero cuando se agregan a través del tiempo, necesitas tomar el periodo final del balance o dividir la suma de balances entre el número de periodos de tiempo.

¿La capa semantica es un complemento obligatorio de la arquitectura DW/BI? La respuesta es sí, si planeas abrir tus puertas  a tu sistema DW/BI para un uso ad hoc. **Cada hora que utilices construyendo una capa semántica rica se verá recompensada con las mejoras adoptadas y el éxito en la comunidad de usuarios.** No ejecutes simplemente el asistente de tu herramienta BI que  genera una capa semántica equivalente a las tablas subyacentes (incluso si son dimensionales). Dedica tiempo para hacerla lo más racional y precisa que puedas.

La desventaja de invertir en una capa semántica es que tendrás que realizar la inversión múltiples veces. La mayoría de organizaciones se encuentran con que necesitan varias herramientas BI para encontrar las diversas necesidades de sus comunidades de usuarios. Cada herramienta tiene su propia capa semántica, que raramente puede ser copiada de una herramienta a otra (incluso en herramientas vendidas por el mismo vendedor). Uno de los muchos retos para los equipos de BI es mantener una apariencia y estructura organizacional similares en todas las herramientas BI. De nuevo, el método Kimball enfatiza que conseguir el modelo relacional correcto de datos dimensionales facilitará considerablemente la tarea de crear la capa semántica (haciendo más fina la capa semántica de la herramienta BI).

Sólo hay un escenario donde me quedaría el argumento de que puedes funcionar sin una capa semántica. Si las puertas de tu sistema DW/BI están cerradas a todos los usuarios ad hoc, y todo el acceso está intermediado por desarrolladores de informes corporativos, puedes hacerla funcionar sin una capa semántica BI. Esta es una recomendación no deseada en diversos frentes. Lo más importante, ¿cómo puedes cerrar las puertas a los usuarios ad hoc? Es una locura. Adicionalmente, los desarrolladores son personas también e incluso pueden escribir SQL a mano y buscar definiciones en un diccionario de datos externo, Pero, ¿por qué torturarlos?

Si te quedas con el argumento de que necesitas una capa semántica, tu próxima pregunta puede ser si necesitas un datawarehouse dimensional. Algunos observadores, especialmente algunos vendedores de herramientas BI, argumentan que puedes omitir el almacén de datos relacional y proporcionar la experiencia dimensional virtualmente. Esto suena atractivo, ya que nadie quiere realmente construir un sistema ETL, pero es una quimera. **Ninguna herramienta de capa semántica proporciona la funcionalidad de transformación e integración de una herramienta ETL.** La mayoría de herramientas BI son estupendas en lo que hacen; no las corrompas por intentar hacer un ETL en la capa semántica. Y no olvides que el mantenimiento del ETL añade valor a los datos limpiando, estandarizando, conformando y des-duplicando. Todos estos pasos una herramienta BI sencillamente no puede hacer.

Lo dicho: He trabajado con clientes que han tenido éxito enganchando una herramienta BI directamente al modelo de datos normalizado (transaccional o ODS). El escenario más común es un prototipo: Enseñémosle a la gente  como sería un universo de objetos de negocios o una interfaz Tableau para generar entusiasmo y apoyo financiero para un proyecto Business Intelligence.

Otro escenario seria conocer los informes de requerimientos operacionales mediante la construcción de una capa semántica sobre el sistema transaccional. Pero, en la mayoría de casos, esta capa semántica operacional es un componente relativamente menor en un entorno analítico de una empresa que incluye un DWH real. Soy muy escéptico con cualquier organización que proclama que sus sistemas transaccionales están lo suficientemente limpios, y sus necesidades analíticas son bastante simples, que no necesitan  instanciar sus modelos dimensionales. No me opongo a la idea en la teoría, pero en la práctica todavía no lo veo funcionando.

No creo que haya visto alguna vez que una organización sobre-invirtiese en una capa semántica, pero he visto muchos data warehouses fallar debido a la falta de inversión. Adquiere una herramienta BI decente (hay docenas), e invierte tiempo desarrollando la capa semántica. De lo contrario estarás subestimando tu sustancial inversión en diseño y desarrollo de un data warehouse técnicamente solido.




[1]: http://www.kimballgroup.com/2013/08/design-tip-158-making-sense-of-the-semantic-layer/