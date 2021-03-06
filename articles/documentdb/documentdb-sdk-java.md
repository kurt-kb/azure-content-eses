<properties 
	pageTitle="SDK para Java de DocumentDB | Microsoft Azure" 
	description="Obtenga toda la información sobre el SDK para Java como, por ejemplo, fechas de lanzamiento, fechas de retirada y cambios de una versión a otra del SDK para Java de DocumentDB." 
	services="documentdb" 
	documentationCenter="java" 
	authors="aliuy" 
	manager="jhubbard" 
	editor="cgronlun"/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="java" 
	ms.topic="article" 
	ms.date="06/14/2016" 
	ms.author="andrl"/>

# SDK de DocumentDB

> [AZURE.SELECTOR]
- [.NET SDK](documentdb-sdk-dotnet.md)
- [SDK de Node.js](documentdb-sdk-node.md)
- [SDK de Java](documentdb-sdk-java.md)
- [SDK de Python](documentdb-sdk-python.md)

##SDK para Java de DocumentDB

<table>
<tr><td>**Descargar**</td><td>[Maven](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb)</td></tr>
<tr><td>**Contribuciones**</td><td>[GitHub] (https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Documentación**</td><td>[Documentación de referencia SDK de Java] (http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Introducción**</td><td>[Introducción al SDK de Java] (documentdb-java-application.md)</td></tr>
<tr><td>**Tiempo de ejecución admitido actualmente**</td><td>[JDK 7] (http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## Notas de la versión

### <a name="1.8.0"/>[1\.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Se ha agregado compatibilidad con cuentas de base de datos de varias regiones.
  - Se ha agregado compatibilidad con el reintento automático en solicitudes limitadas, con opciones para personalizar el número máximo de reintentos y el tiempo de espera máximo de reintento. Consulte RetryOptions y ConnectionPolicy.getRetryOptions(). 
  - Se ha dejado de utilizar el código de creación de particiones personalizado basado en IPartitionResolver. Utilice colecciones con particiones para conseguir un almacenamiento y un rendimiento más elevados. 

### <a name="1.7.1"/>[1\.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Se ha agregado compatibilidad con la directiva de reintentos de la limitación.  

### <a name="1.7.0"/>[1\.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Se ha agregado compatibilidad con período de vida (TTL) para los documentos. 

### <a name="1.6.0"/>[1\.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Se han implementado [colecciones con particiones](documentdb-partition-data.md) y [niveles de rendimiento definidos por el usuario](documentdb-performance-levels.md). 

### <a name="1.5.1"/>[1\.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Se ha corregido un error en HashPartitionResolver para generar valores hash en little endian que sean consistentes con otros SDK.

### <a name="1.5.0"/>[1\.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Se han agregado solucionadores de particiones de hash e intervalo para ayudar con el particionamiento de las aplicaciones entre varias particiones.

### <a name="1.4.0"/>[1\.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Implementación de Upsert. Se han agregado nuevos métodos upsertXXX para admitir la característica Upsert.
- Se implementa el enrutamiento por identificador. Sin cambios en la API pública, todos los cambios son internos.

### <a name="1.3.0"/>1.3.0
- Versión omitida para alinear el número de versión con otros SDK

### <a name="1.2.0"/>[1\.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Compatible con índice geoespacial.
- Valida la propiedad id para todos los recursos. Los identificadores de recursos no pueden contener los caracteres ?, /, #, \\, ni terminar con un espacio.
- Agrega el nuevo encabezado "progreso de transformación de índices" a ResourceResponse.

### <a name="1.1.0"/>[1\.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Implementación de la directiva de indexación V2

### <a name="1.0.0"/>[1\.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- SDK de GA

## Fechas de lanzamiento y de retirada
Microsoft notificará la retirada de un SDK con al menos **12 meses** de antelación para facilitar la transición a una versión compatible o más reciente.

Solo se agregan nuevas características, funcionalidad y optimizaciones al SDK actual, por lo que se recomienda actualizar siempre a la última versión del SDK tan pronto como sea posible.

El servicio rechazará cualquier solicitud realizada en DocumentDB mediante un SDK retirado.

> [AZURE.WARNING]
Todas las versiones del SDK de Azure DocumentDB para Java anteriores a la versión **1.0.0** se retirarán el **29 de febrero de 2016**.

<br/>

| Versión | Fecha de lanzamiento | Fecha de retirada 
| ---	  | ---	         | ---
| [1\.8.0](#1.8.0) | 14 de junio, 2016 |--- 
| [1\.7.1](#1.7.1) | 30 de abril, 2016 |--- 
| [1\.7.0](#1.7.0) | 27 de abril, 2016 |--- 
| [1\.6.0](#1.6.0) | 29 de marzo, 2016 |--- 
| [1\.5.1](#1.5.1) | 31 de diciembre, 2015 |--- 
| [1\.5.0](#1.5.0) | 04 de diciembre, 2015 |--- 
| [1\.4.0](#1.4.0) | 05 de octubre, 2015 |--- 
| [1\.3.0](#1.3.0) | 05 de octubre, 2015 |--- 
| [1\.2.0](#1.2.0) | 05 de agosto, 2015 |--- 
| [1\.1.0](#1.1.0) | 09 de julio, 2015 |--- 
| [1\.0.1](#1.0.1) | 12 de mayo, 2015 |--- 
| [1\.0.0](#1.0.0) | 07 de abril, 2015 |--- 
| 0.9.5-versión preliminar | 09 de marzo, 2015 | 29 de febrero, 2016 
| 0.9.4-versión preliminar | 17 de febrero, 2015 | 29 de febrero, 2016 
| 0.9.3-versión preliminar | 13 de enero, 2015 | 29 de febrero, 2016 
| 0.9.2-versión preliminar | 19 de diciembre, 2014 | 29 de febrero, 2016 
| 0.9.1-versión preliminar | 19 de diciembre, 2014 | 29 de febrero, 2016 
| 0.9.0-versión preliminar | 10 de diciembre, 2014 | 29 de febrero, 2016

## P+F
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## Otras referencias

Para más información sobre DocumentDB, consulte la página del servicio [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/).

<!---HONumber=AcomDC_0615_2016-->