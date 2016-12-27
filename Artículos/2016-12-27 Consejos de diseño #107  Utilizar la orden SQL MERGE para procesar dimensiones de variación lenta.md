---
UniqueId: QeKNyyRcLR
Title: "Consejos de diseño #107: Utilizar la orden SQL MERGE para procesar dimensiones de variación lenta"
Url: 2008/sql-merge-para-la-carga-de-dimensiones-variacino.lenta.html
Date: 2016-12-27T11:22:36.7711455+01:00
SecondaryDate: 2008-11-06T00:00:00.0000000
Description: "La gran ventaja de la orden MERGE es que es capaz de llevar a cabo múltiples acciones en una sola transferencia de grupos de datos, sin requerir múltiples transferencias con inserciones y actualizaciones separadas. Un optimizador bien preparado podría llevar a cabo esto con extrema eficiencia."
Author: Warren Thornthwaite
Category: ETL y calidad de datos
RelatedUrl: http://www.kimballgroup.com/2008/11/design-tip-107-using-the-sql-merge-statement-for-slowly-changing-dimension-processing/

---
La mayoría de las herramientas ETL proporcionan cierta funcionalidad para poder manipular dimensiones de variación lenta (Slowly Changing Dimensions).

De vez en cuando, cuando la herramienta no trabaja como es requerido, el desarrollador de la ETL utiliza la base de datos para identificar filas nuevas o que hayan cambiado, y así aplicar las inserciones y actualizaciones apropiadas. Mostré ejemplos de este tipo de código en la clase sobre [“Data Warehouse Lifecycle in Depth”][1] utilizando códigos estándar de inserción y actualización. Pocos meses después, mi amigo Stuart Ozer me sugirió que el nuevo comando MERGE en SQL Server 2008 sería más eficiente, ambos desde una perspectiva de ejecución y código. Su mención del blog de Chad Boy MSSQLTips.com me dio algunas indicaciones de cómo funciona.

**MERGE** es una combinación de **INSERT** (insertar), **UPDATE** (actualizar) y **DELETE** (borrar) y permite un control significativo sobre lo que pasa en cada cláusula.

Este ejemplo contiene una dimensión con un solo cliente con dos atributos: nombre y apellido. Identificaremos el nombre como atributo *Tipo 1* y el apellido como *Tipo 2*. Recordemos que en el *Tipo 1* es donde se encuentra el cambio en la dimensión del atributo sobrescribiendo el valor existente con el nuevo valor; el *Tipo 2* es donde podremos hacer el seguimiento de la evolución, añadiendo una nueva fila que será efectiva cuando el nuevo valor aparezca.

### Paso 1: Sobrescribir los cambios en el Tipo 1

Intenté conseguir el ejemplo completo trabajando sobre una única orden MERGE, pero la función es determinista y sólo permite una orden de actualización, así que tuve que utilizar MERGE separadamente  para actualizar el Tipo1. Esto se podría haber llevado a cabo también con una instrucción  UPDATEya que el Tipo 1 es una actualización por definición.

```
MERGE INTO dbo.Customer\\\\\\\_Master AS CM
USING Customer\\\\\\\_Source AS CS ON (CM.Source\\\\\\\_Cust\\\\\\\_ID = CS.Source\\\\\\\_Cust\\\\\\\_ID)
WHEN MATCHED AND CM.First\\\\\\\_Name \\\\\\\<\\\\\\\> CS.First\\\\\\\_Name — Update all existing rows for Type 1 changes
THEN UPDATE SET CM.First\\\\\\\_Name = CS.First\\\\\\\_Name
```

Esta es una versión simple de la sintaxis del comando MERGE para combinar la tabla Customer\_Source con la dimensión Customer\_Master utilizando la clave de negocios, y actualizar todas las filas asignadas donde el nombre en la tabla maestra (Master table) no es igual que el nombre en la tabla de origen (Source table).

### Paso 2: Realizar los cambios del Tipo 2

Ahora, hacemos una segunda orden MERGE para llevar a cabo los cambios del tipo 2.

Aquí es donde las cosas se vuelven un poco complicadas ya que implica realizar diversos pasos para el seguimiento de los cambios en el Tipo 2. Nuestro código necesita:

- Insertar nuevas filas de clientes con las fechas finales  efectivas y apropiadas.
- Eliminar las filas antiguas cambiándolas por aquellas que tienen un cambio en el atributo Tipo 2, determinando la fecha de finalización apropiada y el indicador de fila actual (current\_row\_flag).
- Insertar las filas de Tipo 2 cambiadas con la apropiada y efectivas fechas de finalización y el indicador de fila actual (current\_row flag).

El problema en este caso es que hay demasiados pasos a llevar a cabo por la sintaxis del comando MERGE. Afortunadamente, la orden MERGE puede transmitir sus resultados a un proceso posterior. Lo utilizaremos para hacer la última inserción de los cambios en las filas del Tipo 2 INSERTando en la tabla de  Customer\_Master realizando un SELECT de los resultados obtenidos en MERGE. Esto puede sonar como una manera complicada de resolver el problema, pero tiene la ventaja de que solo necesitamos encontrar una vez los cambios realizados en las filas del Tipo 2, y después las podremos utilizar muchas veces.

El código empieza con la cláusula INSERT. Esto se debe realizar primero porque la orden MERGE está incluida en INSERT. El código incluye varias referencias a conseguir fecha; el código asume que el cambio fue efectivo ayer (getdate()-1) lo que significa que la versión previa expiraría el dia anterior (getdate()-2).

```
1 INSERT INTO Customer\\\\\\\_Master
2 SELECT Source\\\\\\\_Cust\\\\\\\_ID, First\\\\\\\_Name, Last\\\\\\\_Name, Eff\\\\\\\_Date, End\\\\\\\_Date, Current\\\\\\\_Flag
3 FROM
4 ( MERGE Customer\\\\\\\_Master CM
5 USING Customer\\\\\\\_Source CS
6 ON (CM.Source\\\\\\\_Cust\\\\\\\_ID = CS.Source\\\\\\\_Cust\\\\\\\_ID)
7 WHEN NOT MATCHED THEN
8 INSERT VALUES (CS.Source\\\\\\\_Cust\\\\\\\_ID, CS.First\\\\\\\_Name, CS.Last\\\\\\\_Name, convert(char(10), getdate()-1, 101), ’12/31/2199′, ‘y’)
9 WHEN MATCHED AND CM.Current\\\\\\\_Flag = ‘y’
10 AND (CM.Last\\\\\\\_Name \\\\\\\<\\\\\\\> CS.Last\\\\\\\_Name ) THEN
11 UPDATE SET CM.Current\\\\\\\_Flag = ‘n’, CM.End\\\\\\\_date = convert(char(10), getdate()- 2, 101)
12 OUTPUT $Action Action\\\\\\\_Out, CS.Source\\\\\\\_Cust\\\\\\\_ID, CS.First\\\\\\\_Name, CS.Last\\\\\\\_Name, convert(char(10), getdate()-1, 101) Eff\\\\\\\_Date, ’12/31/2199′ End\\\\\\\_Date, ‘y’Current\\\\\\\_Flag
13 ) AS MERGE\\\\\\\_OUT
14 WHERE MERGE\\\\\\\_OUT.Action\\\\\\\_Out = ‘UPDATE’;
```

#### Comentarios sobre el código

1. Las **líneas 1-3**  son una orden normal INSERT. Lo que terminaremos insertando son los nuevos valores de las filas del Tipo 2 que han cambiado.
2. La **línea 4** es el principio de  la orden MERG E que termina en la línea 13. La orden MERGE tiene una cláusula OUTPUT que transmitirá los resultados obtenidos en MERGE a la función receptora. Esta sintaxis define una tabla de expresión común, básicamente una tabla temporal en la clásula FROM, llamada MERGE\_OUT.
3. Las **líneas 4-6** dan instrucciones a MERGE para cargar la Customer\_Source en la dimensión Customer\_Master, tabla del fichero maestro de clientes.
4. En la **línea 7**, cuando no hay correspondencia en la clave de negocios, debemos crear un nuevo cliente, en **la línea 8** se hace la inserción INSERT. Puedes tomar como parámetro la fecha efectiva en lugar asumir el día anterior como fecha.
5. Las **líneas 9 y 10** identifican un subconjunto de filas combinando claves de negocio, concretamente, donde está la fila actual en el Customer\_Master AND y uno de las columnas de Tipo 2 es diferente.
6. La **línea 11** elimina la fila inicial en el Customer\_Master identificando la fecha de finalización y el indicador de fila actual con “n”.
7. La **línea 12** es la clausula OUTPUT que identifica que atributos serán mostrados desde el MERGE, si existen.
8. Esto es lo que repercutiremos en la orden externa INSERT. La acción es una función MERGE que nos dice de que parte del MERGRE proviene cada fila. Debemos darnos cuenta de que el OUTPUT puede  extraer de ambos de la fuente y del fichero maestro. En este caso, estamos dando salida a los atributos de la fuente ya que son los que contienen los valores del nuevo Tipo 2.
9. La **línea 14** limita el conjunto de filas de salida a las filas que fueron actualizadas en el Customer\_Master. Estas corresponden a las líneas cambiadas en la línea 11, pero obtendremos los valores actuales del Customer\_Source en la línea 12.

### Resumen

La gran ventaja de la orden MERGE es que es capaz de llevar a cabo múltiples acciones en una sola transferencia de grupos de datos, sin requerir múltiples transferencias con inserciones y actualizaciones separadas. Un optimizador bien preparado podría llevar a cabo esto con extrema eficiencia.





[1]: https://datawarehouse.es/ralph-kimball-books.html