---
UniqueId: SllVObxbVe
Title: "Consejo de dieño #134: Recomendaciones para las pruebas del Data Warehouse"
Url: 2011/recomendaciones-testing-datawarehouse.html
Section: ETL y calidad de datos
Date: 2011-05-04T00:03:27.9860753+02:00
SecondaryDate: 2011-05-04T00:03:00.0000000
Description: "Aquí están mis 5 recomendaciones principales para construir y ejecutar un test en el entorno de tu proyecto DW/BI."
Author: Joy Mundy
Category: ETL y calidad de datos
RelatedUrl: http://www.kimballgroup.com/2011/05/design-tip-134-data-warehouse-testing-recommendations/

---
La validación de un entorno datawarehouse/BI es un gran reto. La metodología de validación estándar valida sólo una pequeña cosa a la vez. Sin embargo, la naturaleza de un sistema DW/BI versa sobre integraciones complejas, sin mencionar el elevado volumen de datos. Aquí están mis 5 recomendaciones principales para construir y ejecutar un test en el entorno de tu proyecto DW/BI.

## 1. Crea un pequeño test estático de la base de datos, derivado de los datos reales.

Necesitas que sea pequeño para que el test se realice rápidamente. Necesitas que sea estático para conocer de antemano los resultados esperados. Y necesitas que derive de datos reales porque no hay nada como los datos reales para ofrecer una combinación realista de escenarios buenos y malos. Necesitarás añadir filas en el test de la base de datos para probar que cada una de las instrucciones de tu código ETL cubre un escenario no incluido en el test de datos original.

## 2. Comprueba pronto y con regularidad.

Empieza comprobando tan pronto como escribas una línea de código (o conectas dos cajas en el UI de tu herramienta ETL). Los desarrolladores hacen esto todo el tiempo, por supuesto. Desarrollar implica poner en marcha tests unitarios para asegurar que su código funciona como se espera. Muchos desarrolladores no son tan buenos siguiendo la evolución de esas pruebas y realizándolas frecuentemente. Diariamente. Cada vez que se realiza un commit en tu código. Si realizas el test cada día, y priorizas en mejorar cada test que falló ayer, será más fácil determinar aquello que no funcionó.

La prueba unitaria asegura que el código desarrollado funciona tal y como se ha diseñado. La prueba del sistema asegura que el sistema en completo funciona, de acuerdo con las especificaciones. La prueba del sistema también se debería realizar pronto.

Hay una "fase de test" oficial antes de la puesta en funcionamiento del DWH. Esta fase de tests es para llevar a cabo las pruebas y reconocer problemas, no para identificar como deberían ser los tests y como llevarlos a cabo. **Se debe  empezar a probar el sistema al inicio del proceso de desarrollo**, de esta manera todos los detalles se resuelven mucho antes de empezar con la "fase de test" oficial, que supone mayor presión para el funcionamiento correcto del sistema.

## 3. Utilizar herramientas de prueba y automatizar el entorno de prueba.

La recomendación de hacer el test pronto y regularmente es práctica solamente si automatizas el proceso. Ningún desarrollador va a utilizar hasta la última de su día de trabajo haciendo de niñera de la prueba unitaria. Y pocos equipos pueden permitirse un controlador a tiempo completo para hacer ese trabajo en lugar del desarrollador. Para automatizar la prueba, necesitas herramientas. Muchas organizaciones ya disponen “in situ” de herramientas para comprobar la calidad de las pruebas. Si tu no las tienes, o estás convencido de que las herramientas de las que dispones no responden a las necesidades de control del sistema DW/BI, intenta buscar en google “herramientas de software para asegurar la calidad” para disponer de una inmensa lista de productos y metodologías disponibles a una extensa gama de precios.

Todos los software comerciales de herramientas de prueba te permitirán formular tests, ejecutar tests, registrar los resultados de los tests realizados y informar de esos resultados. Para la prueba de la unidad y prueba de la calidad de los datos, define los tests para llevar a cabo una consulta en la fuente y el objetivo del almacén de datos. **Tienes que buscar el recuento de filas y cantidades para hacerlas coincidir.**

Una herramienta de testing utilizada para probar el DW/BI debe ser capaz de ejecutar un escenario que establezca el entorno de prueba antes de que las pruebas se lleven a cabo. Las tareas que necesitará ejecutar incluyen:

- Restaurar un entorno de máquina virtual con un test de datos limpio.
- Modificar el test estático de datos con filas especiales para probar condiciones inusuales.
- Ejecutar tu programa ETL

Después de ejecutar y registrar los tests, termina con un *script* de limpieza, que debería ser tan simple como eliminar el entorno VM.

La metodología de prueba estándar cambia una cosa, ejecuta un test y registra los resultados. En el mundo del DW/BI, debes agrupar juntos muchos tests en un grupo de tests. Incluso con una base de datos pequeña, no tienes que ejecutar tu código ETL para cada una de los centenares de unidades que deberían estar funcionando.

## 4. Haz una lista de los usuarios de negocios para definir los tests del sistema.

Necesitamos la experiencia de los usuarios para definir buenos tests para el sistema. ¿Cómo sabemos que los datos son correctos? ¿Cómo sabemos que el rendimiento de las consultas cumple sus expectativas? Hacer una lista de los usuarios de negocios durante el proceso de especifiación del test asegurará una mejor prueba que si simplemente el equipo DW/BI construye tests basados en lo que creen que es interesante. Contar con los usuarios de negocios clave en el proceso de aseguramiento de la calidad también proporciona un impulso enorme en la credibilidad.

## 5. El test del entorno debe ser tan similar como sea posible al entorno de producción.

Es de vital importancia que el entorno de test sea similar al de producción. Sería ideal que fuese el mismo hardware, software y configuración. En el mundo real, relativamente pocas organizaciones tienen presupuesto para tener dos servidores DW grandes. Pero cualquier organización puede, y debe, hacer coincidir los siguientes elementos:

1. Configuración de la unidad (nombres relativos de las unidades). El disco es barato y debes ser capaz de duplicar tu disco para su test. Pero si no puedes, al menos haz que los escritos de la unidad y el archivo de la base de datos tengan la misma disposición. Mucha gente se queja de que esto significa mucho trabajo para cambiar el entorno . ¡Lo es! Y es mucho mejor hacerlo ahora que en la fase final del test de tu proyecto.
2. Las versiones de software del sistema operativo de la base de datos con la base de datos de los escritorios de los usuarios, y todo aquello que los relacione.
3. Diseño del servidor. Si el software de reporting está en su propio servidor de producción, pruébalo de esa manera.
4. Roles de seguridad y privilegios para las cuentas de mantenimiento. Tu despliegue está virtualmente relegado al fracaso si no compruebas los roles de seguridad primero. No sé porque, pero siempre parece que va a ir mal.

**Si sigues estas sugerencias, especialmente la sugerencia de probar continuamente, probablemente tengas una fase de test agradable y sin crisis y cumplirás el hito de pase a producción en  la fecha programada.**  Si no, estás corriendo un serio riesgo de retrasar interminablemente un fabuloso proyecto en el infierno QA, con los usuarios de negocios y la dirección llamando a tu puerta.

Artículo original: [http://www.kimballgroup.com/2011/05/design-tip-134-data-warehouse-testing-recommendations/][1]





[1]: http://www.kimballgroup.com/2011/05/design-tip-134-data-warehouse-testing-recommendations/