<properties 
	pageTitle="Crear una colección de base de datos de DocumentDB | Microsoft Azure" 
	description="Aprenda a crear colecciones de documentos JSON mediante el portal de servicios en línea para Azure DocumentDB, una base de datos de documentos NoSQL basada en la nube. Obtenga una versión de evaluación gratuita hoy mismo." 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/16/2016" 
	ms.author="mimig"/>

# Creación de una colección de DocumentDB mediante el Portal de Azure

Para usar Microsoft Azure DocumentDB, debe tener una [cuenta de DocumentDB](documentdb-create-account.md), una [base de datos](documentdb-create-database.md), una colección y documentos. En este tema se describe cómo crear una colección de DocumentDB en el Portal de Azure.

¿No está seguro de lo que es una colección? Vea [¿Qué es una colección de DocumentDB?](#what-is-a-documentdb-collection)

1.  En la barra de accesos directos del [Portal de Azure](https://portal.azure.com/), haga clic en **Cuentas de DocumentDB**. Si **Cuentas de DocumentDB** no está visible, haga clic en **Examinar** y después en **Cuentas de DocumentDB**.

    ![Captura de pantalla con la opción Cuentas de DocumentDB de la barra de accesos directos, la cuenta en la hoja Cuentas de DocumentDB y la base de datos en la hoja de la cuenta de DocumentDB, en la lente Bases de datos, resaltados](./media/documentdb-create-collection/docdb-database-creation-1-2.png)

2.  En la hoja **Cuentas de DocumentDB**, seleccione la cuenta en la que desea agregar una colección. Si no se muestra ninguna cuenta, deberá [crear una cuenta de DocumentDB](documentdb-create-account.md).

3. En la hoja **Cuenta de DocumentDB** de la cuenta seleccionada, desplácese hacia abajo hasta la lente **Bases de datos** y seleccione la base de datos en la que se va a agregar una colección.

    ![Captura de pantalla con la opción Cuentas de DocumentDB de la barra de accesos directos, la cuenta en la hoja Cuentas de DocumentDB y la base de datos en la hoja de la cuenta de DocumentDB, en la lente Bases de datos, resaltados](./media/documentdb-create-collection/docdb-database-creation-3.png)

4. En la hoja **Base de datos**, haga clic en **Agregar colecciones**.

	![Captura de pantalla con el botón Agregar colección de la hoja Base de datos, la configuración de la hoja Agregar colección y el botón Aceptar resaltados - Portal de Azure para DocumentDB - Creador de bases de datos basadas en la nube para bases de datos JSON NoSQL](./media/documentdb-create-collection/docdb-collection-creation-4.png)

5. En la hoja **Agregar colección**, en el cuadro **Identificador**, escriba el identificador de la nueva colección. Los nombres de colección deben tener entre 1 y 255 caracteres y no pueden contener `/ \ # ?` o un espacio al final. Cuando se valida el nombre, aparece una marca de verificación verde en el cuadro Id.

	![Captura de pantalla con el botón Agregar colección de la hoja Base de datos, la configuración de la hoja Agregar colección y el botón Aceptar resaltados - Portal de Azure para DocumentDB - Creador de bases de datos basadas en la nube para bases de datos JSON NoSQL](./media/documentdb-create-collection/docdb-collection-creation-5-8.png)

6. Si va a crear una colección con una sola partición, haga clic en la flecha para seleccionar un plan de tarifa, luego haga clic en Ver todo y seleccione un nivel de rendimiento para la colección. Para obtener más información sobre los niveles de rendimiento disponibles, consulte [Niveles de rendimiento en DocumentDB](documentdb-performance-levels.md). Cada colección creada es una entidad facturable.

	Si va a crear una colección con particiones, vaya al paso 7 ya que no necesita seleccionar un plan de tarifa.

7. Seleccione uno de los valores de **Modo de particionamiento** para la colección: **Partición única** o **Particionada**. Una única partición tiene una capacidad de almacenamiento reservado de 10 GB y puede tener niveles de rendimiento de entre 400 y 10 000 unidades de solicitud por segundo. Una colección particionada se puede escalar para administrar 250 GB de almacenamiento en varias particiones, y puede tener niveles de rendimiento de entre 10 100 y 250 000 unidades de solicitud por segundo. Para obtener información acerca de la solicitud de aumento de estas asignaciones predeterminadas después de crear una colección, consulte [Solicitud de aumento de los límites de la cuenta de DocumentDB](documentdb-increase-limits.md).

8. Seleccione el valor de **Rendimiento** de la colección particionada. Una unidad de solicitud (RU) corresponde al rendimiento de una lectura de un documento de 1 KB. Para más información sobre las unidades de solicitud, consulte [Unidades de solicitud en DocumentDB](documentdb-request-units.md).

9. Si va a crear una colección particionada, seleccione el valor de **Clave de partición** de la colección. La selección de la clave de partición correcta es importante al crear una colección de rendimiento. Para más información sobre cómo seleccionar una clave de partición, consulte [Partición y escalado de datos en DocumentDB](documentdb-partition-data.md).

10. Haga clic en **Aceptar** en la parte inferior de la pantalla para crear la nueva colección.

11. La nueva colección aparece ahora en el modo **Colecciones** en la hoja **Base de datos**.
 
	![Captura de pantalla de la nueva colección en la hoja Base de datos - Portal de Azure para DocumentDB - Creador de bases de datos basadas en la nube para bases de datos JSON NoSQL](./media/documentdb-create-collection/docdb-collection-creation-9.png)

## ¿Qué es una colección de DocumentDB? 

Una colección es un contenedor de documentos JSON asociado a la lógica de aplicación de JavaScript. Una colección es una entidad facturable, en la que el [costo](documentdb-performance-levels.md) viene determinado por el rendimiento aprovisionado de la colección. Las colecciones pueden abarcar una o varias particiones o uno o varios servidores y se pueden escalar para administrar volúmenes prácticamente ilimitados de almacenamiento o rendimiento.

Las colecciones se particionan automáticamente en uno o más servidores físicos mediante DocumentDB. Cuando crea una colección, puede especificar el rendimiento aprovisionado en términos de unidades de solicitud por segundo y una propiedad de clave de partición. El valor de esta propiedad se usará en DocumentDB para distribuir documentos entre las particiones y enrutar solicitudes, como las consultas. El valor de la clave de partición también actúa como límite de la transacción para los procedimientos y los desencadenadores almacenados. Cada colección tiene una cantidad reservada de capacidad de proceso específica para esa colección, que no se comparte con otras colecciones de la misma cuenta. Por lo tanto, puede escalar la aplicación horizontalmente tanto en términos de rendimiento como de almacenamiento.

Las colecciones son distintas de las tablas incluidas en las bases de datos relacionales. Las colecciones no imponen un esquema, de hecho, DocumentDB no impone ningún esquema, sino que es una base de datos sin esquemas. Por lo tanto, se pueden almacenar distintos tipos de documentos con esquemas diferentes en la misma colección. Puede optar por utilizar colecciones para almacenar objetos de un solo tipo, como se haría con las tablas. El mejor modelo depende solo de la forma en que aparezcan los datos juntos en las consultas y las transacciones.

## Otras formas de crear una colección de DocumentDB

No es necesario crear las colecciones con el portal, también puede crearlas con los [SDK de DocumentDB](documentdb-sdk-dotnet.md) y la API de REST.

- Para ver un ejemplo de código de C#, consulte los [ejemplos de colección de C#](documentdb-dotnet-samples.md#collection-examples). 
- Para ver un ejemplo de código Node.js, consulte los [ejemplos de colección de Node.js](documentdb-nodejs-samples.md#collection-examples).
- Para ver un ejemplo de código Python, consulte los [ejemplos de colección de Python](documentdb-python-samples.md#collection-examples).
- Para ver un ejemplo de API de REST, consulte [Crear una colección](https://msdn.microsoft.com/library/azure/mt489078.aspx).

## Solución de problemas

Si la opción **Agregar colección** está deshabilitada en el portal de Azure, significa que la cuenta está deshabilitada, lo que normalmente se produce cuando se utilizan todos los créditos de beneficios del mes.

## Pasos siguientes

Ahora que tiene una colección, el paso siguiente es agregar o importar documentos a la colección. Cuando se trata de agregar documentos a una colección, tiene varias posibilidades:

- Puede [agregar documentos](documentdb-view-json-document-explorer.md) mediante el Explorador de documentos en el Portal.
- También puede [importar documentos y datos](documentdb-import-data.md) mediante la Herramienta de migración de datos de DocumentDB, que le permite importar archivos CSV y JSON, así como datos de SQL Server, MongoDB, almacenamiento de tablas de Azure y otras colecciones de DocumentDB. 
- También puede agregar documentos con uno de los [SDK de DocumentDB](documentdb-sdk-dotnet.md). DocumentDB tiene .NET, Java, Python, Node.js y SDK de la API de JavaScript. Para ver ejemplos de código de C# que muestran cómo trabajar con documentos mediante el SDK de .NET para DocumentDB, consulte la sección [Ejemplos de .NET de DocumentDB](documentdb-dotnet-samples.md#document-examples). Para ver ejemplos de código de Node.js que muestran cómo trabajar con documentos mediante el SDK de Node.js para DocumentDB, consulte la sección [Ejemplos de Node.js de DocumentDB](documentdb-nodejs-samples.md#document-examples).

Cuando tenga documentos en una colección, puede usar [SQL de DocumentDB](documentdb-sql-query.md) para [ejecutar consultas](documentdb-sql-query.md#executing-queries) en sus documentos mediante el [Explorador de consultas](documentdb-query-collections-query-explorer.md) del portal, la [API de REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) o uno de los [SDK](documentdb-sdk-dotnet.md).

<!---HONumber=AcomDC_0518_2016-->