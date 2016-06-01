<properties
   pageTitle="Procedimientos recomendados para Almacenamiento de datos SQL de Azure | Microsoft Azure"
   description="Recomendaciones y procedimientos recomendados que debe saber para desarrollar soluciones de Almacenamiento de datos SQL de Azure. Esto le ayudará a tener éxito."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/02/2016"
   ms.author="sonyama;barbkess"/>

# Procedimientos recomendados para el Almacenamiento de datos SQL

Este artículo es una colección de muchos procedimientos recomendados que le permitirá conseguir la mejor relación precio/rendimiento para su Almacenamiento de datos de SQL de Azure. Algunos de los conceptos son muy básicos y fáciles de explicar, otros son más avanzados y solo se pueden ver por encima en este artículo. El objetivo de este artículo es proporcionarle algunos consejos básicos y mostrarle los aspectos importantes que debe considerar al crear una unidad de almacenamiento de datos. En cada sección se presenta un concepto y se le indican artículos que lo desarrollan más en detalle.

Si acaba de empezar con el Almacenamiento de datos SQL de Azure, no se preocupe por la amplitud del contenido de este artículo. Los temas se van desarrollando básicamente en orden de importancia. Con que se centre en los tres primeros es suficiente para empezar. Según se vaya familiarizando y se sienta cómodo con el Almacenamiento de datos SQL, vuelva y eche un vistazo a los demás temas. No tardará mucho en darle sentido a todo. Puede parecer abrumador al principio, pero con el tiempo irá adquiriendo experiencia y se aprenderá estos temas de memoria.

## Menos costos gracias a la pausa y la escala
Una característica clave del Almacenamiento de datos SQL es la capacidad de pausar cuando no se usa, que detiene la facturación de los recursos de proceso. Otra característica clave es la capacidad de escalar los recursos. La pausa y la escala se pueden usar desde el Portal de Azure o a través de comandos de PowerShell. Familiarizarse con estas características, ya que pueden reducir considerablemente el costo del almacén de datos cuando no se encuentra en uso. Si quiere que el almacén esté siempre disponible, puede reducir la escala al tamaño menor, DW100, en lugar de usar la pausa.

Consulte también cómo [pausar][], [reanudar][] y [aumentar o reducir la escala de][] los recursos de proceso


## Vaciado de las transacciones antes de aplicar la pausa o la escala 
Al aplicar la pausa o la escala al Almacenamiento de datos SQL, instancia de la base de datos se detiene en segundo plano. Esto significa que se cancelan todas las consultas pendientes. Una simple consulta de selección se cancela rápidamente y no afecta casi al tiempo que se tarda en aplicar la pausa o la escala a la instancia. Sin embargo, las consultas sobre transacciones, que modifican los datos o la estructura de estos, no se puede detener rápidamente. **Las consultas sobre transacciones deben completarse íntegramente o deshacerse los cambios**. Deshacer el trabajo realizado con una consulta sobre una transacción puede tardar tanto (o más) que el tiempo necesario para aplicar el cambio original de esta. Por ejemplo, si se cancela una consulta que se eliminaba filas y ya lleva en ejecución una hora, el sistema puede tardar una hora para reinsertar las filas eliminadas. Si se ejecuta la pausa o la escala con transacciones en curso, puede parecer que tardan mucho en aplicarse, ya que deben esperar a que se deshagan todos los cambios para continuar.

Consulte también [Transacciones en el Almacenamiento de datos SQL][] y [Optimización de transacciones para Almacenamiento de datos SQL][]

## Mantenimiento de las estadísticas
A diferencia de SQL Server, que automáticamente detecta y crea o actualiza las estadísticas en columnas que se beneficiarían, el Almacenamiento de datos SQL requiere el mantenimiento manual de las estadísticas. Aunque nuestra intención es cambiar esto, por ahora deberá realizar el mantenimiento de las estadísticas para garantizar que los planes del Almacenamiento de datos SQL son óptimos. Los planes que crea el optimizador son igual de buenos que las estadísticas disponibles. **Crear estadísticas de muestra en cada columna es una forma sencilla de empezar a trabajar con las estadísticas**. Es igualmente importante actualizar las estadísticas cuando se produzcan cambios significativos en los datos. Un enfoque conservador puede ser actualizar las estadísticas diariamente o después de cada carga. Existen inconvenientes entre el rendimiento y el costo de crear y actualizar las estadísticas. Si cree que tarda demasiado en realizar el mantenimiento de todas las estadísticas, puede intentar ser más selectivo acerca de las columnas con estadísticas o las que necesitan actualizarse con frecuencia. Por ejemplo, puede actualizar las columnas de fecha, donde se añadan valores todos los días. **Sacará el máximo provecho con las estadísticas en columnas implicadas en combinaciones, las columnas que se usan en la cláusula WHERE y las columnas de GROUP BY.**

Consulte también [Administración de estadísticas en el Almacenamiento de datos SQL][], [CREATE STATISTICS (Transact-SQL)][] y [UPDATE STATISTICS (Transact-SQL)][]

## Agrupación de instrucciones INSERT en lotes
Una tabla pequeña se puede cargar perfectamente una sola vez con una instrucción INSERT o incluso con una recarga periódica que le funcione bien con una instrucción del tipo `INSERT INTO MyLookup VALUES (1, 'Type 1')`. Sin embargo, si necesita cargar miles o millones de filas a lo largo del día, es posible que las instrucciones INSERT sencillas no sean suficientes. En su lugar, desarrolle sus procesos para que escriban en un archivo y otro proceso que periódicamente se ejecute y lo cargue.

Consulte también [Insert (Transact-SQL)][]
 
## Carga y exportación de datos rápidas con PolyBase
Almacenamiento de datos SQL admite la carga y exportación de datos con varias herramientas, como Data Factory de Azure, PolyBase y BCP. Para pequeñas cantidades de datos donde el rendimiento no es clave, cualquier herramienta le sirve. Sin embargo, para cargar o exportar grandes volúmenes de datos o si se necesita un rendimiento rápido, PolyBase es la mejor opción. PolyBase está diseñado para aprovechar la estructura MPP (procesamiento masivo en paralelo) del Almacenamiento de datos SQL y, por tanto, carga y exporta grandes cantidades de datos más rápido que cualquier otra herramienta. Lo que haya cargado con PolyBase se ejecuta con la consulta CTAS o de selección. **CTAS reduce el registro de transacciones y es la manera más rápida de cargar datos**. Data Factory de Azure también admite datos cargados con PolyBase. PolyBase admite distintos de formatos de archivo, como Gzip. **Con el fin de conseguir un mayor rendimiento al usar archivos de texto gzip, divídalos en 60 o varios archivos para aumentar el paralelismo de la carga**. Para conseguir un rendimiento total más rápido, cargue los datos simultáneamente.

Consulte también [Carga de datos en Almacenamiento de datos SQL][], [Guía para el uso de PolyBase en Almacenamiento de datos SQL][], [Azure SQL Data Warehouse loading patterns and strategies][] (estrategias y patrones de carga de Almacenamiento de datos SQL de Azure), [Carga de datos con Data Factory de Azure][], [Movimiento de datos hacia y desde Almacenamiento de datos SQL de Azure mediante Data Factory de Azure][], [CREATE EXTERNAL FILE FORMAT (Transact-SQL)][] (creación de formatos de archivo externos [Transact-SQL]), [CREATE TABLE AS SELECT (CTAS) en Almacenamiento de datos SQL][]

## Distribución Hash para tablas grandes
De forma predeterminada, las tablas se distribuyen según el patrón Round Robin. Esto facilita a los usuarios empezar a crear tablas sin tener que decidir sobre la distribución. Las tablas Round Robin son eficaces para algunas cargas de trabajo, pero a menudo es mucho mejor seleccionar una columna de distribución. El ejemplo más común de tabla distribuida por una columna que supera con creces a una Round Robin es al combinarse dos tablas grandes de hechos. Por ejemplo, si tiene una tabla de pedidos, que se distribuye por order\_id, y una tabla de transacciones, que también se distribuye por order\_id, al unir la tabla de pedidos a la de transacciones en order\_id, esta consulta se convierte en una consulta de paso a través, lo que significa que se eliminan las operaciones de movimiento de datos. Menos pasos suponen mayor rapidez de consulta. Menos movimiento de datos también se traduce en consultas más rápidas. Esta explicación es muy general. Al cargar una tabla con distribución, asegúrese de que no se ordenan los datos entrantes en la clave de distribución, ya que esto ralentizará la carga. En los siguientes vínculos se muestra con mucho más detalle cómo seleccionar una columna de distribución mejora el rendimiento y cómo definir una tabla distribuida en la cláusula WITH de la instrucción CREATE TABLES.

Consulte también [La distribución hash y su efecto sobre el rendimiento de las consultas en Almacenamiento de datos SQL][], [Choosing hash distributed table vs. round-robin distributed table][] (elegir una tabla Hash en lugar de Round Robin), [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][] (CREATE TABLE [Almacenamiento de datos SQL de Azure, almacenamiento de datos paralelos]), [CREATE TABLE AS SELECT (Azure SQL Data Warehouse)][] (CREATE TABLE AS SELECT [Almacenamiento de datos SQL de Azure])

## Sin particiones excesivas
Crear particiones de datos puede ser muy eficaz para el mantenimiento de los datos mediante la modificación de particiones o exámenes de optimización, pero el exceso de particiones puede ralentizar las consultas. A menudo una estrategia de división con granularidad alta que puede funcionar bien en SQL Server no funciona correctamente en el Almacenamiento de datos SQL. El exceso de particiones también puede reducir la eficacia de los índices de almacén de columnas agrupadas si cada partición tiene menos de 1 millón de filas. Tenga en cuenta que, en segundo plano, Almacenamiento de datos SQL divide los datos automáticamente en 60 bases de datos, por lo que si crea una tabla con 100 particiones, se generan realmente 6000 particiones. Cada carga de trabajo es diferente, por lo mejor es probar con las particiones para ver qué funciona mejor para la suya. Considere la posibilidad de reducir la granularidad respecto a lo que le funcionaba en SQL Server. Por ejemplo, puede usar particiones semanales o mensuales, en lugar de diarias.

Consulte también [Particiones de tabla en el Almacenamiento de datos SQL][]

## Reducción del tamaño de las transacciones
Las instrucciones INSERT, UPDATE Y DELETE se ejecutan en las transacciones y, cuando fallan, deben deshacerse. Para que no se tarde tanto en deshacer, reduzca el tamaño de las transacciones siempre que pueda. Puede hacerlo si divide las instrucciones INSERT, UPDATE y DELETE en partes. Por ejemplo, si tiene una instrucción INSERT que se suele tardar de 1 hora, si puede, divídala en 4 partes de 15 minutos cada una. Aproveche los casos de registro mínimo, como CTAS, TRUNCATE, DROP TABLE o INSERT para vaciar las tablas y así reducir el riesgo de reversión. Otra manera de eliminar reversiones es usar funciones de solo metadatos, como la modificación de particiones para la administración de datos. Por ejemplo, en lugar de ejecutar una instrucción DELETE para eliminar todas las filas de una tabla cuyo order\_date fuera octubre de 2001, podría dividir los datos mensualmente y desactivar la división con los datos de una partición vacía de otra tabla (consulte los ejemplos de ALTER TABLE). Para tablas sin divisiones, puede usar CTAS en lugar de DELETE para escribir los datos que quiera mantener en una tabla. Si CTAS tarda lo mismo, es una operación mucho más segura, ya que su registro de transacciones es mínimo y se puede cancelar rápidamente si es necesario.

Consulte también [Transacciones en el Almacenamiento de datos SQL][], [Optimización de transacciones para Almacenamiento de datos SQL][], [Particiones de tabla en el Almacenamiento de datos SQL][], [TRUNCATE TABLE (Transact-SQL)][], [ALTER TABLE (Transact-SQL)][], [Create Table As Select (CTAS) en Almacenamiento de datos SQL][]

## Tamaño de columna mínimo
Al definir el DDL, usar el tipo de datos mínimo compatible con los datos mejorará el rendimiento de la consulta. Esto tiene especial importancia para las columnas CHAR y VARCHAR. Si el valor mayor máximo de una columna es 25 caracteres, defina la columna como VARCHAR(25). Evite definir todas las columnas de caracteres con una longitud predeterminada de gran tamaño. Defina las columnas como VARCHAR en lugar de NVARCHAR cuando no se necesite nada más.

Consulte también [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][] (CREATE TABLE [Almacenamiento de datos SQL de Azure, almacenamiento de datos paralelos])

## Tablas de apilamiento temporal para los datos transitorios
Cuando almacene datos temporalmente en Almacenamiento de datos SQL, las tablas de apilamiento pueden agilizar el proceso global. Si solo carga datos para transformarlos después, cargar la tabla de apilamiento será mucho más rápido que cargar los datos en una tabla de almacén de columnas agrupadas. Además, los datos de una tabla temporal también se cargarán mucho más rápido que las tablas de almacenamiento permanente. Las tablas temporales empiezan por "#" y solo se puede acceder a ellas desde la sesión en la que se crearan, por lo que pueden no funcionar en algunas situaciones. Las tablas de apilamiento se definen en la cláusula WITH de CREATE TABLE. Si usa una tabla temporal, no olvide crear estadísticas en ella también.

Consulte también [Tablas temporales en el Almacenamiento de datos SQL][], [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][] (CREATE TABLE [Almacenamiento de datos SQL de Azure, almacenamiento de datos paralelos]), [CREATE TABLE AS SELECT (Azure SQL Data Warehouse)][] (CREATE TABLE AS SELECT [Almacenamiento de datos SQL de Azure])

## Optimización de las tablas de almacén de columnas agrupadas
Los índices de almacén de columnas agrupadas son una de las maneras más eficaces para almacenar datos en Almacenamiento de datos SQL de Azure. De forma predeterminada, las tablas de Almacenamiento de datos SQL se crean como almacén de columnas agrupadas. Para conseguir el máximo rendimiento de las consultas en las tablas de almacén de columnas, es importante la calidad de los segmentos. Escriben filas en las tablas de almacén de columnas bajo presión de memoria afecta a la calidad de segmento. La calidad de segmento se puede medir por el número de filas de un grupo de filas comprimido. Consulte la sección **Clustered Columnstore Segment Quality** (Calidad de segmento del almacén de columnas agrupadas) de [Solución de problemas][] para obtener instrucciones paso a paso sobre la detección y mejora de la calidad de segmento para las tablas de almacén de columnas agrupadas. La calidad de los segmentos del almacén de columnas es bastante importante, por lo que, en general, resulta útil crear identificadores de usuario especiales de carga exclusivos con recursos de clase intermedia o grande. Cuanto menos DWU use, mayor será la clase de recurso que deberá asignar al usuario de carga.

Dado que las tablas de almacén de columnas generalmente no insertan datos en un segmento del almacén de columnas comprimido hasta que hay más de 1 millón de filas por tabla y cada tabla del Almacenamiento de datos SQL se divide en 60 partes, como norma general, las tablas de almacén de columnas no serán útiles para las consultas a menos que la tabla tenga más de 60 millones de filas. Para las tablas con menos de 60 millones de filas, podría no tener sentido el índice de almacén de columnas. Pero tampoco molesta. Además, si divide los datos, recuerde que cada parte deberá tener 1 millón de filas para beneficiarse de un índice de almacén de columnas agrupadas. Si una tabla tiene 100 particiones, deberá tener al menos 6 mil millones de filas para beneficiarse del almacén de columnas agrupadas (60 distribuciones * 100 particiones * 1 millón de filas). Si la tabla no tiene 6 mil millones de filas en este ejemplo, reduzca el número de particiones o considere la posibilidad de usar una tabla de apilamiento en su lugar. También puede experimentar para ver si consigue un mejor rendimiento con una tabla de apilamiento con índices secundarios, en lugar de con una tabla de almacén de columnas. Las tablas de almacén de columnas aún no admiten índices secundarios.

Al consultar una tabla de almacén de columnas, las consultas se ejecutarán más rápido si selecciona solo las que necesita.

Consulte también [Solución de problemas][], [Administración de índices de almacén de columnas en Almacenamiento de datos SQL de Azure][] y [Descripción de los índices de almacén de columnas][]

## Mayor clase de recursos para mejorar el rendimiento de las consultas
Almacenamiento de datos SQL usa grupos de recursos para asignar memoria a las consultas. De manera predeterminada, todos los usuarios se asignan a los recursos de clase pequeña, que concede a 100 MB de memoria por distribución. Dado que siempre hay 60 distribuciones y cada distribución tiene un mínimo de 100 MB, la asignación de memoria total del sistema es de 6 000 MB o justo por debajo de 6 GB. Algunas consultas, como las combinaciones de gran tamaño o las cargas a las tablas de almacén de columnas agrupadas, se beneficiarán de las mayores asignaciones de memoria. Algunas consultas, como los exámenes puros, no sufrirán cambios. Por otro lado, usar las clases de recursos mayores afecta la simultaneidad, por lo que deberá tener esto en cuenta antes de cambiar todos los usuarios a una clase de recursos grande.
 
Consulte también [Simultaneidad y administración de cargas de trabajo en Almacenamiento de datos SQL][]

## Menor clase de recursos para aumentar la simultaneidad
Si observa que las consultas de usuario se retrasan bastante, es posible que los usuarios se ejecutan en clases de recursos mayores y consuman muchas ranuras de simultaneidad, lo que pone en cola otras consultas. Para ver si hay consultas de usuarios en cola, ejecute `SELECT * FROM sys.dm_pdw_waits` para ver si se devuelven filas.

Consulte también [Simultaneidad y administración de cargas de trabajo en Almacenamiento de datos SQL][] y [sys.dm\_pdw\_waits (Transact-SQL)][]

## Vistas de administración dinámica (DMV) para supervisar y optimizar las consultas
Almacenamiento de datos SQL tiene varias DMV que sirven para supervisar la ejecución de la consulta. El siguiente artículo de supervisión le guía con instrucciones paso a paso acerca de cómo ver los detalles de una consulta en curso. Usar la opción LABEL con las consultas puede ayudar a encontrar rápidamente las consultas en estas DMV.

Consulte también [Supervisión de la carga de trabajo mediante DMV][], [Uso de etiquetas para instrumentar consultas en Almacenamiento de datos SQL][], [OPTION (cláusula de Transact-SQL)][], [sys.dm\_exec\_sessions (Transact-SQL)][], [sys.dm\_pdw\_exec\_requests (Transact-SQL)][], [sys.dm\_pdw\_request\_steps (Transact-SQL)][], [sys.dm\_pdw\_sql\_requests (Transact-SQL)][], [sys.dm\_pdw\_dms\_workers (Transact-SQL)], [DBCC PDW\_SHOWEXECUTIONPLAN (Transact-SQL)][], [sys.dm\_pdw\_waits (Transact-SQL)][]

## Otros recursos
Hay muchos lugares para buscar información sobre cómo usar el Almacenamiento de datos SQL de Azure. Este artículo forma parte de la documentación de Azure y tiene numerosos vínculos a otros artículos de Azure, así como a artículos de MSDN. Supervisamos todos sus comentarios sobre los artículos para actualizarlos con frecuencia. Si encuentra que un artículo es útil, díganoslo respondiendo a "¿Le resulta útil esta página?". Puede proporcionar comentarios tanto si responde Sí como No. Si encuentra un artículo útil, pero tiene algún comentario, haga clic en Sí y comente sobre cualquier aspecto que podamos mejorar del artículo. Si no se le formula esta pregunta, la encontrará siempre al final del artículo de Azure; para los artículos de MSDN, existe el vínculo "Sugerencias" en la esquina superior derecha de las páginas de MSDN. Valoramos sus comentarios y tomamos medidas para la mayoría de ellos.

Si tiene **sugerencias para características** de Almacenamiento de datos SQL, use la página de [comentarios de Almacenamiento de datos SQL Azure][]. Añadir sus solicitudes o valoraciones positivas sobre otras solicitudes nos ayuda a priorizar las características.

El [foro de MSDN de Almacenamiento de datos SQL de Azure][] se creó como un lugar para formular preguntas a otros usuarios y al grupo de productos de Almacenamiento de datos SQL. Supervisamos este foro para garantizar de que sus preguntas las responde otro usuario o alguno de nosotros. Si prefiere preguntar acerca de Stack Overflow, también tenemos un [foro de Stack Overflow para Almacenamiento de datos SQL de Azure][].

<!--Image references-->

<!--Article references-->
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Simultaneidad y administración de cargas de trabajo en Almacenamiento de datos SQL]: sql-data-warehouse-develop-concurrency.md
[CREATE TABLE AS SELECT (CTAS) en Almacenamiento de datos SQL]: sql-data-warehouse-develop-ctas.md
[Guía para el uso de PolyBase en Almacenamiento de datos SQL]: sql-data-warehouse-load-polybase-guide.md
[La distribución hash y su efecto sobre el rendimiento de las consultas en Almacenamiento de datos SQL]: sql-data-warehouse-develop-hash-distribution-key.md
[Carga de datos en Almacenamiento de datos SQL]: sql-data-warehouse-overview-load.md
[Carga de datos con Data Factory de Azure]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase in SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md
[Administración de índices de almacén de columnas en Almacenamiento de datos SQL de Azure]: sql-data-warehouse-manage-columnstore-indexes.md
[Administración de estadísticas en el Almacenamiento de datos SQL]: sql-data-warehouse-develop-statistics.md
[Supervisión de la carga de trabajo mediante DMV]: sql-data-warehouse-manage-monitor.md
[Movimiento de datos hacia y desde Almacenamiento de datos SQL de Azure mediante Data Factory de Azure]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Optimización de transacciones para Almacenamiento de datos SQL]: sql-data-warehouse-develop-best-practices-transactions.md
[pausar]: sql-data-warehouse-overview-scalability.md#pause-compute-bk
[reanudar]: sql-data-warehouse-overview-scalability.md#resume-compute-bk
[aumentar o reducir la escala de]: sql-data-warehouse-overview-scalability.md#scale-performance-bk
[Table design in SQL Data Warehouse]: sql-data-warehouse-develop-table-design.md
[Particiones de tabla en el Almacenamiento de datos SQL]: sql-data-warehouse-develop-table-partitions.md
[Tablas temporales en el Almacenamiento de datos SQL]: sql-data-warehouse-develop-temporary-tables.md
[Transacciones en el Almacenamiento de datos SQL]: sql-data-warehouse-develop-transactions.md
[Solución de problemas]: sql-data-warehouse-troubleshoot.md
[Uso de etiquetas para instrumentar consultas en Almacenamiento de datos SQL]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/ms190273.aspx
[Descripción de los índices de almacén de columnas]: https://msdn.microsoft.com/library/gg492088.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204041.aspx
[CREATE STATISTICS (Transact-SQL)]: https://msdn.microsoft.com/library/ms188038.aspx
[DBCC PDW\_SHOWEXECUTIONPLAN (Transact-SQL)]: https://msdn.microsoft.com/library/mt204017.aspx
[Insert (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION (cláusula de Transact-SQL)]: https://msdn.microsoft.com/library/ms190322.aspx
[sys.dm\_exec\_sessions (Transact-SQL)]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm\_pdw\_exec\_requests (Transact-SQL)]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm\_pdw\_request\_steps (Transact-SQL)]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm\_pdw\_sql\_requests (Transact-SQL)]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm\_pdw\_dms\_workers (Transact-SQL)]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm\_pdw\_waits (Transact-SQL)]: https://msdn.microsoft.com/library/mt203893.aspx
[TRUNCATE TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS (Transact-SQL)]: https://msdn.microsoft.com/library/ms187348.aspx

<!--Other Web references-->
[Choosing hash distributed table vs. round-robin distributed table]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[comentarios de Almacenamiento de datos SQL Azure]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[foro de MSDN de Almacenamiento de datos SQL de Azure]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[foro de Stack Overflow para Almacenamiento de datos SQL de Azure]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies

<!---HONumber=AcomDC_0518_2016-->