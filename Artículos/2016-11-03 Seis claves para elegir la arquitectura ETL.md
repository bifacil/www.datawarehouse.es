---
UniqueId: GYCVZOyZzN
Title: Seis claves para elegir la arquitectura ETL
Url: 2009/seis-claves-para-elegir-arquitectura-etl.html
Image: 2009-seis-claves-para-elegir-arquitectura-2.jpg
Date: 2016-11-03T00:00:00.0000000
SecondaryDate: 2009-10-09T22:08:00.0000000
Description: "Este artículo describe seis decisiones clave que deben incluirse durante la elaboración de la arquitectura ETL para un DWH dimensional. Estas decisiones tienen un impacto importante en el coste inicial y coste corriente y en la complejidad de la solución ETL y, finalmente, en el éxito de toda la solución BI/DW en global."
Author: Bob Becker
Category: ETL y calidad de datos
RelatedUrl: http://www.kimballgroup.com/2009/10/six-key-decisions-for-etl-architectures/

---
Este artículo describe seis decisiones clave que deben incluirse durante la elaboración de la arquitectura ETL para un DWH dimensional. Estas decisiones tienen un impacto importante en el coste inicial y coste corriente y en la complejidad de la solución ETL y, finalmente, en el éxito de toda la solución BI/DW en global.



## 1. ¿Debemos utilizar una herramienta ETL?

Una de las primeras y más fundamentales decisiones que debes tomar es si codificar tu proceso ETL a mano desde cero, o si debes utilizar un software específico de un proveedor. Dejando de lado  las cuestiones técnicas y los costes de licencia, no debes adoptar un método que a tus empleados y directores no les sea familiar sin considerar seriamente las implicaciones a largo plazo de estas decisiones. Esta decisión tendrá un mayor impacto en el contexto del ETL, guiando decisiones respecto al personal, diseñando enfoques, estrategias de metadatos, y implementando cronologías a largo plazo.

**En el contexto actual, la mayoría de organizaciones deben usar un software suministrado por un proveedor.** Sin embargo, esta decisión debe tomarse en base a los recursos disponibles para construir y dirigir el sistema. Las herramientas ETL construyen entornos que usan iconos, flechas y propiedades en lugar de escribir código para construir la solución ETL. Ten cuidado: Si tu equipo de desarrollo del ETL está integrado por cierto número de programadores de la vieja escuela, es posible que no se adapten bien a una herramienta ETL. Sólo por esta razón, algunas organizaciones piensan que un ETL personalizado es una solución razonable.

Si decides utilizar una herramienta ETL, no esperes una gran amortización en tu primera iteración. Las ventajas aparecerán a medida que realices iteraciones adicionales y empieces a aprovechar las ventajas de utilizar la herramienta en las siguientes implementaciones. También experimentarás los beneficios en la capacidad de mantenimiento, documentación más completa y soporte mejorado para los metadatos.

## 2. ¿Dónde y cómo debe tener lugar la integración de datos?

La integración de datos es un tema importantísimo para IT porque, básicamente, se trata de hacer que todos los sistemas trabajen juntos eficientemente. La "visión de 360 de la empresa" es un objetivo comúnmente discutido que realmente implica la integración de datos. En muchos casos, la integración de datos debe realizarse en los sistemas de transacciones primarios de la organización antes de que los datos lleguen al data warehouse. Sin embargo, antes que enfrentarse a la integración en el entorno operacional, estos requisitos son normalmente encargados al DWH y al sistema ETL.

La mayoría de nosotros entendemos el concepto principal de que la integración permite que bases de datos dispares funcionen juntas de una manera útil. Todos sabemos que lo necesitamos, simplemente no tenemos una idea clara de cómo dividirlo en partes manejables.

¿Integración significa que todas las partes de grandes organizaciones estén de acuerdo con cada dato o sólo con alguno de ellos? Este es el punto crucial de la decisión que se debe tomar. ¿En qué datos la dirección empresarial está de acuerdo/insiste que suceda? ¿Están dispuestos a establecer definiciones comunes entre la organización y atenerse a esas definiciones?

Fundamentalmente, integración significa llegar a un acuerdo en el sentido de los datos desde la perspectiva de dos o más bases de datos. Con la integración, los resultados de dos bases de datos pueden ser combinados en un solo análisis del DWH. Sin un acuerdo, las bases de datos permanecerán aisladas y no se podrán enlazar en una aplicación. En el contexto de nuestro entorno ETL, la integración de datos toma la forma de las dimensiones acordadas y de los hechos establecidos en el almacén de datos. Las dimensiones acordadas hacen referencia a establecer atributos dimensionales comunes entre tablas de hechos separadas de forma que informes puedan ser generados utilizando estos atributos. Los hechos establecidos hacen referencia a acuerdos en métricas comunes de negocios como los indicadores de actividad (KPIs) entre bases de datos separadas, para que estos números puedas ser comparados matemáticamente para calcular diferencias y ratios.

## 3. ¿Qué mecanismo de recopilación de cambios de datos debemos escoger?

Durante la carga inicial del histórico de datos del almacén de datos, recopilar cambios contenidos en las fuentes de datos no es importante ya que estás cargando datos en un punto futuro. Sin embargo, la mayoría de tablas del almacén de datos son tan extensas que no pueden ser actualizadas durante cada ciclo ETL. Debes tener la capacidad de transferir sólo los cambios relevantes para la fuente de datos  desde la última actualización. Aislar la última fuente de datos se denomina recopilación de cambio de datos (*change data capture*, CDC). **La idea detrás de recopilar el cambio de datos es bastante simple: transferir solamente los datos que han sido cambiados desde la última actualización.**

Pero construir un buen sistema de recopilación de cambio de datos no es tan fácil como suena. Asumamos  que el mecanismo seleccionado debe ser totalmente fiable y a prueba de fallos. Todos los datos cambiados deben ser identificados. Encontrar la estrategia más completa puede ser evasivo, muchas veces la actualización de los sistemas de tablas de la fuente puede ocurrir fuera de la misma aplicación. Un error aquí, provocará resultados que no podrán ser explicados fácilmente; frecuentemente esto deriva en la recuperación de datos para identificar el culpable.

Los problemas pueden significar elevados costes en términos de rehacer el trabajo-sin mencionar la situación embarazosa. En resumen, **recopilar los cambios de datos está lejos de ser una tarea trivial, y debes entender muy bien los sistemas de fuente de datos.** Este conocimiento ayudará al equipo ETL a evaluar las fuentes de datos, identificar problemas en  la recopilación de los cambios de datos y determinar la estrategia más apropiada.

## 4. ¿Cuando debemos almacenar los datos?

En el entorno actual de almacenamiento de datos, es bastante posible para las herramientas ETL establecer una conexión directa con la fuente de la base de datos, extraer los datos, aplicar cualquier transformación requerida en la memoria y, finalmente, escribirla- sólo una vez- en la tabla objetivo del DWH. Desde el punto de vista de rendimiento, esta es una gran capacidad ya que el coste de escritura es elevado. Minimizar el número de escrituras en un buen objetivo de diseño.

Sin embargo, a pesar de las ventajas de rendimiento, este puede no ser el mejor planteamiento. Hay diversas razones por las que una organización puede decidir para almacenar los datos físicamente  (por ejemplo, escribirlos en un disco) durante el proceso ETL:

- **El método CDC más apropiado requiere comparar la copia actual de la tabla fuente con la copia precedente de la misma tabla.**
- La organización ha elegido almacenar los datos inmediatamente después de la extracción con el propósito de archivar (posiblemente para cumplir los requerimientos de auditoria y control).
- A veces se requiere un punto de recuperación/reinicio cuando el ETL falla a mitad del trabajo.
- Los procesos ETL que se desarrollan durante largos períodos pueden abrir una conexión con el sistema fuente que crea problemas con bloqueos de la base de datos y sobrecarga el sistema de transacciones.

## 5. ¿Dónde debemos corregir los datos?

Los usuarios de negocios son conscientes de que la calidad de los datos es un problema serio y caro. Por este motivo, a la mayoría de organizaciones les gusta apoyar iniciativas para mejorar la calidad de los datos. Pero la mayoría de los usuarios probablemente no tienen ni idea de dónde se originan los problemas de calidad de los datos o que se debe hacer para mejorar la calidad de los datos. Deben pensar que la calidad de los datos es un simple problema de ejecución del equipo ETL. En este entorno, el equipo ETL necesita ser ágil y proactivo:

1. **La calidad de los datos no puede ser mejorada solamente por el ETL.**
2. **La clave está en que el equipo de ETL colabore con la empresa y los equipos de soporte del sistema fuente del IT.**

La decisión clave es dónde corregir los datos. Claramente, la mejor solución es recopilar correctamente los datos en el momento que se generan. Por supuesto, este no es siempre el caso, pero **en la mayoría de casos los datos deben ser corregidos en sistema fuente.**

Sin embargo, desafortunadamente, es inevitable que datos de poca calidad lleguen al sistema ETL. En este caso, hay tres opciones:

1. Detener el proceso de carga.
2. Enviar los registros dañados a un archivo temporal para su posterior procesamiento.
3. Únicamente etiquetar los datos y continuar

La tercera opción es por mucho la mejor (siempre que sea posible). Detener el proceso es obviamente doloroso porque requiere intervención manual para diagnosticar el problema, reiniciar o reanudar el trabajo, o abortar completamente. Enviar registros a un archivo temporal siempre es una solución pobre porque no está claro cuándo estos registros serán revisados e introducidos en el DWH. Hasta que los registros son restaurados al flujo de datos, la integridad global de la base de datos es cuestionable ya que estos registros están perdidos. Recomiendo no utilizar el archivo temporal para infracciones menores de datos.

La tercera opción de etiquetar los datos con la condición de error normalmente funciona bien. La tabla de hechos errónea puede ser etiquetada con una dimensión de control que describe la condición de calidad global de la fila del hecho problemático. Los datos mal dimensionados también pueden ser etiquetados con un valores únicos de error en el mismo campo. En este punto, las soluciones de *data quality* pueden ayudar aidentificar el hecho problemático y las filas de dimensión.

## 6. ¿Con que rapidez debe estar disponible la información de origen en el sistema DW/BI?

La latencia de  los datos  describe con qué frecuencia los datos fuente deben ser proporcionados a los usuarios de negocio vía el sistema DW/BI. La latencia de los datos obviamente tiene un gran efecto en el coste y la complejidad de tu entorno ETL. Algoritmos de procesamiento inteligentes, paralelización,  y hardware potente pueden acelerar el flujo de carga (tradicionalmente orientado por lotes). Pero en algún momento, si el requerimiento de la latencia de datos es suficientemente urgente, el proceso ETL debe pasar de estar orientado a lotes a estar orientado a flujos. Este cambio no es gradual ni evolutivo. Es un cambio de paradigma que puede implicar la re-implementación completa del sistema ETL.

Habitualmente, el proceso ETL requiere una latencia de datos acorde al ritmo natural del negocio. **En la mayoría de organizaciones esto se traduce en actualizaciones diarias para la mayoría de procesos  ETL y actualizaciones semanales o mensuales para otros procesos ETL.**

Sin embargo, en algunas circunstancias, el ritmo del negocio requiere actualizaciones más frecuentes o incluso actualizaciones en tiempo real. La clave es reconocer que sólo un puñado de procesos empresariales dentro de una organización son apropiados para realizar actualizaciones en tiempo real. **No hay ninguna razón de peso para convertir todo el proceso ETL a tiempo real.** El ritmo de la mayoría de procesos empresariales simplemente no demandan un tratamiento en tiempo real.

Hay que tener cuidado: Preguntar a los usuarios de negocio si quieren la entrega de datos en tiempo real es una invitación abierta para los problemas. Por supuesto, la mayoría de usuarios de negocios responderán positivamente a tener los datos actualizados más frecuentemente, independientemente de si entienden el impacto de su petición. Claramente este tipo de requerimiento de latencia de datos puede ser peligroso. Recomendamos dividir la dificultad del tiempo real en tres categorías: diaria, frecuente e instantánea. Necesitas que los usuarios finales describan sus requerimientos en cuanto a latencia de datos en estos términos y después diseñar tu solución ETL apropiadamente para respaldar cada set de requerimientos:

1. **"Instántaneo"** significa que los datos visibles en la pantalla del usuario final representan el estado real del sistema de fuente de transacción en cada instante. Cuando el estado del sistema de la fuente cambia, la pantalla debe también responder instantáneamente.
2. **"Frecuentemente"** significa que los datos visibles por el usuario final, se actualizan muchas veces al día pero no se garantiza que representen el  valor correcto absoluto en cada instante.
3. **"Diariamente"** significa que los datos visibles en la pantalla corresponden a la información del sistema fuente al final del día anterior laborable.

En este artículo hemos discutido unas cuentas claves que deben ser evaluadas durante la creación de la arquitectura ETL para mantener DWH dimensional.

Pocas veces hay una sola elección correcta para cualquiera de estas decisiones. Como siempre, la decisión correcta dependerá de las necesidades y características de tu empresa.




[1]: http://www.kimballgroup.com/2009/10/six-key-decisions-for-etl-architectures/