<properties
	pageTitle="¿Qué es R en HDInsight? Introducción al servidor de R en HDInsight (versión preliminar) | Microsoft Azure"
	description="Qué es R Server en HDInsight (versión preliminar) y cómo usarlo con el objetivo de crear aplicaciones para realizar análisis de macrodatos."
	services="hdinsight"
	documentationCenter=""
	authors="jeffstokes72"
	manager="paulettm"
	editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/01/2016"
   ms.author="jeffstok"/>


# Información general de R Server en HDInsight (versión preliminar)

Gracias a HDInsight Premium de Microsoft Azure, Microsoft R Server ahora está disponible como opción al crear clústeres de HDInsight en Azure. Esta nueva característica provee a los científicos de datos, estadísticos y programadores de R de acceso a petición a métodos escalables y distribuidos para realizar análisis en HDInsight.

Los clústeres se pueden ajustar a los proyectos y las tareas en cuestión, así como desactivarse cuando ya no se requieran. Como forman parte de HDInsight de Azure, estos clústeres incluyen soporte ininterrumpido de nivel empresarial, un Acuerdo de Nivel de Servicio con un tiempo de actividad del 99,9 % y la flexibilidad suficiente como para integrarse con otros componentes del ecosistema de Azure.

>[AZURE.NOTE] El servidor de R solo está disponible con HDInsight Premium. Actualmente, HDInsight Premium solo está disponible para los clústeres de Hadoop y Spark. Por tanto, en este momento el servidor de R solo se puede usar con clústeres de Hadoop y Spark en HDInsight. Para obtener más información, consulte [¿Cuáles son los diferentes componentes de Hadoop disponibles con HDInsight?](hdinsight-component-versioning.md)

R Server en HDInsight proporciona las funcionalidades más recientes para realizar análisis basados en R en grandes conjuntos de datos cargados en el Almacenamiento de blobs de Azure. Puesto que R Server se basa en el lenguaje R de código abierto, las aplicaciones basadas en R que compile podrán usar cualquiera de los más de 8000 paquetes de R de código abierto, así como las rutinas de ScaleR: el paquete de análisis de macrodatos de Microsoft incluido en R Server.

El nodo perimetral de los clústeres Premium proporciona una ubicación muy útil para conectarse al clúster y ejecutar los scripts de R. Con un nodo perimetral, tiene la opción de ejecutar funciones distribuidas paralelizadas de ScaleR en los diferentes núcleos del servidor de nodo perimetral. También tiene la opción de ejecutarlas en estos nodos del clúster utilizando los contextos de proceso de Spark o Hadoop MapReduce de ScaleR.

Los modelos o las predicciones que se generan a partir de los análisis pueden descargarse para utilizarse en un entorno local. También pueden emplearse en otro lugar de Azure; por ejemplo, a través de un [servicio web](../machine-learning/machine-learning-publish-a-machine-learning-web-service.md) de [Estudio de aprendizaje automático de Microsoft Azure](http://studio.azureml.net).

## Introducción a R en HDInsight

Para incluir R Server en un clúster de HDInsight, debe crear un clúster de Hadoop o de Spark con la opción Premium al crear un clúster mediante el Portal de Azure. Los dos tipos de clúster usan la misma configuración que incluye R Server en los nodos de datos de un clúster, así como un nodo perimetral como zona de aterrizaje para los análisis basados en este producto. Consulte [Introducción al uso de R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-get-started.md) para ver un tutorial detallado sobre cómo crear un clúster.

## Información sobre las opciones de almacenamiento de datos

El almacenamiento predeterminado de los clústeres de HDInsight es el servicio Almacenamiento de blobs con el sistema de archivos HDFS asignado a un contenedor de blobs. De este modo, se garantiza que cualquier dato cargado en el almacenamiento de clúster (o escrito en él durante el transcurso del análisis) se convierta en persistente. Puede usar la utilidad [AzCopy](../storage/storage-use-azcopy.md) para copiar datos con el blob como origen y destino.

Además de utilizar Almacenamiento de blobs, tiene la opción de usar [Almacén de Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) con el clúster. Si utiliza Data Lake, podrá emplear el Almacenamiento de blobs y Data Lake para el almacenamiento HDFS.

También se puede usar [Archivos de Azure](../storage/storage-how-to-use-files-linux.md) como opción de almacenamiento para emplearse en el nodo perimetral. Gracias a Archivos de Azure, podrá montar en el sistema de archivos Linux un recurso compartido de archivos creado en el servicio Almacenamiento de Azure. Para obtener más información sobre las opciones de almacenamiento de datos de R Server en un clúster de HDInsight, consulte [Opciones de almacenamiento de Azure del servidor de R en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-storage.md).

## Acceso a R Server en el clúster

Después de crear un clúster con R Server, puede conectarse a la consola de R en el nodo perimetral del clúster mediante SSH o PuTTY. También puede conectarse a través de un explorador si opta por instalar el IDE de RStudio Server opcional en el nodo perimetral. Para obtener más información sobre cómo instalar RStudio Server, consulte [Instalación de RStudio con R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-install-r-studio.md).

## Desarrollo y ejecución de scripts de R

Los scripts de R que se crean y ejecutan pueden usar cualquiera de los más de 8000 paquetes de R de código abierto, además de las rutinas distribuidas y paralelizadas de la biblioteca ScaleR. En general, el script que se ejecuta en el nodo perimetral de R Server lo hace dentro del intérprete de R de ese nodo. Hay una excepción: los pasos que llaman a una función ScaleR con un contexto de proceso establecido en Hadoop MapReduce (RxHadoopMR) o Spark (RxSpark).

En estos casos, la función se ejecuta de forma distribuida en esos nodos de datos (tarea) del clúster asociado a los datos a los que se hacen referencia. Para obtener más información sobre las distintas opciones de contexto de proceso, consulte [Opciones de contexto de proceso para R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-compute-contexts.md).

## Uso de modelos

Cuando se complete el modelado de datos, podrá utilizar el modelo para realizar predicciones de nuevos datos tanto en Azure como en un entorno local. Este proceso se conoce como "puntuación". Estos son algunos ejemplos.

### Puntuación en HDInsight

Para realizar puntuaciones en HDInsight, escriba una función de R que llame al modelo para realizar predicciones de un nuevo archivo de datos que se haya cargado en la cuenta de almacenamiento. Después, guarde las predicciones en la cuenta de almacenamiento. Puede ejecutar la rutina a petición en el nodo perimetral del clúster o mediante un trabajo programado.

### Puntuación en Aprendizaje automático de Microsoft Azure

Para puntuar mediante un servicio web de Aprendizaje automático de Microsoft Azure, use el [paquete de R de Aprendizaje automático de Azure de código abierto](http://www.inside-r.org/blogs/2015/11/18/enhancements-azureml-package-connect-r-azureml-studio) para [publicar el modelo como servicio web de Azure](http://www.r-bloggers.com/deploying-a-car-price-model-using-r-and-azureml/). Después, utilice los recursos de Aprendizaje automático con el fin de crear una interfaz de usuario para el servicio web y, luego, según proceda, realice una llamada a dicho servicio web para efectuar puntuaciones.

Si elige esta opción, debe convertir los objetos de modelo de ScaleR en objetos de modelo de código abierto equivalentes para poder usarlos en el servicio web. Para ello, se pueden usar las funciones de conversión de ScaleR, como `as.randomForest()`, para los modelos basados en conjuntos.

### Puntuación en un entorno local

Para puntuar de forma local después de crear el modelo, este se puede serializar en R, descargarlo, anular la serialización y, después, usarlo para puntuar nuevos datos. Puede puntuar nuevos datos adoptando el enfoque descrito anteriormente en [Puntuación en HDInsight](#scoring-in-hdinsight) o mediante [DeployR](https://deployr.revolutionanalytics.com/).

## Mantenimiento del clúster

### Instalación y mantenimiento de paquetes de R

La mayoría de los paquetes de R que utiliza se necesitan en el nodo perimetral, puesto que es donde se ejecutan la mayoría de los scripts de R. Para instalar más paquetes de R en el nodo perimetral, puede usar el método `install.packages()` habitual en R.

En la mayoría de los casos, no hará falta instalar más paquetes de R en los nodos de datos si simplemente usa rutinas de la biblioteca ScaleR en el clúster. Sin embargo, puede que necesite más paquetes con el fin de poder utilizar **rxExec** o **RxDataStep** en los nodos de datos.

En tales casos, estos paquetes adicionales deben especificarse utilizando una acción de script después de crear el clúster. Para obtener más información, consulte [Introducción al uso de R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-get-started.md).

### Cambio de la configuración de memoria de Hadoop MapReduce

Un clúster puede modificarse para cambiar la cantidad de memoria disponible en R Server cuando se ejecuta un trabajo de MapReduce. Para ello, use la IU de Apache Ambari, que se encuentra disponible a través de la hoja del Portal de Azure de su clúster. Para ver instrucciones sobre cómo acceder a la IU de Ambari de su clúster, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Ambari](hdinsight-hadoop-manage-ambari.md).

También se puede cambiar la cantidad de memoria disponible en R Server utilizando los modificadores de Hadoop en la llamada a **RxHadoopMR**, tal y como se muestra a continuación:

	hadoopSwitches = "-libjars /etc/hadoop/conf -Dmapred.job.map.memory.mb=6656"  

### Escalado del clúster

Un clúster existente puede escalarse o reducirse verticalmente a través del Portal. Gracias al escalado, puede conseguir la capacidad adicional que podría necesitar para albergar tareas de procesamiento de mayor envergadura, o bien puede volver a escalar un clúster cuando esté inactivo. Para ver instrucciones sobre cómo escalar un clúster, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Ambari](hdinsight-administer-use-portal-linux.md).

### Mantenimiento del sistema

El mantenimiento se realiza en las máquinas virtuales Linux subyacentes de un clúster de HDInsight durante las horas de poca actividad con el objetivo de aplicar revisiones de sistema operativo y otras actualizaciones. Normalmente, el mantenimiento se realiza a las 3:30 (en la hora local de la máquina virtual) todos los lunes y jueves. Las actualizaciones se llevan a cabo de forma que no afecten a más de un cuarto del clúster a la vez.

Como los nodos principales son redundantes y no todos los nodos de datos se ven afectados, es posible que se ralenticen los trabajos que se estén ejecutando durante ese tiempo. No obstante, deben seguir ejecutándose hasta que finalicen. Cualquier software personalizado o dato local que haya instalado se conserva a través de estos eventos de mantenimiento, salvo que se produzca un error irrecuperable que requiera recompilar el clúster.

## Más información sobre las opciones del IDE de R Server en el clúster de HDInsight

El nodo perimetral de Linux de un clúster de HDInsight Premium es la zona de aterrizaje de los análisis basados en R. Después de conectarse al clúster, puede iniciar la interfaz de la consola en R Server escribiendo **R** en la línea de comandos de Linux. El uso de la interfaz de la consola será mejor si ejecuta un editor de texto para desarrollar scripts de R en otra ventana y corta y pega las secciones del script en la consola de R según lo vaya necesitando.

Una herramienta más sofisticada para desarrollar scripts de R es el IDE basado en este lenguaje, que puede utilizarse en entornos de escritorio, como [R Tools para Visual Studio](https://www.visualstudio.com/es-ES/features/rtvs-vs.aspx) (RTVS), que anunció recientemente Microsoft. Se trata de una familia de herramientas de escritorio y servidor de [RStudio](https://www.rstudio.com/products/rstudio-server/). También puede utilizar el IDE [StatET](http://www.walware.de/goto/statet) basado en Eclipse de Walware.

Otra opción consiste en instalar un IDE en el propio nodo perimetral de Linux. [RStudio Server](https://www.rstudio.com/products/rstudio-server/) es una popular alternativa, que proporciona un IDE basado en explorador que pueden usar los clientes remotos. Si RStudio Server se instala en el nodo perimetral de un clúster de HDInsight Premium, ofrece una experiencia de IDE completa para desarrollar y ejecutar scripts de R con R Server en el clúster. Además, puede ser bastante más productivo que utilizar la consola de R. Si quiere instalar RStudio Server, consulte [Instalación de RStudio con R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-install-r-studio.md).

## Más información sobre los precios

Las tarifas asociadas a un clúster de HDInsight Premium con R Server tienen una estructura similar a la de los clústeres de HDInsight estándar. Se basan en el tamaño de las máquinas virtuales subyacentes en los nodos de nombre, de datos y perimetral, con el extra de un aumento de la hora de núcleo para Premium. Para obtener más información sobre los precios de HDInsight Premium, incluido el precio de la versión preliminar pública y la disponibilidad de una prueba gratuita de 30 días, consulte [Precios de HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## Pasos siguientes

Haga clic en los siguientes vínculos para obtener más información sobre cómo usar R Server con clústeres de HDInsight.

- [Introducción al uso de R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-get-started.md)

- [Add RStudio Server to HDInsight Premium (Agregar un servidor de RStudio a HDInsight Premium)](hdinsight-hadoop-r-server-install-r-studio.md)

- [Opciones de contexto de proceso para R Server en HDInsight (versión preliminar)](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Storage options for R Server on HDInsight Premium (Opciones de almacenamiento de Azure del servidor de R en HDInsight Premium)](hdinsight-hadoop-r-server-storage.md)

<!---HONumber=AcomDC_0622_2016-->