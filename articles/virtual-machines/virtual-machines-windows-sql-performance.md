<properties
	pageTitle="Procedimientos recomendados de rendimiento para SQL Server | Microsoft Azure"
	description="Ofrece prácticas recomendadas para optimizar el rendimiento de SQL Server en máquinas virtuales de Microsoft Azure."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management" />

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="04/22/2016"
	ms.author="jroth" />

# Prácticas recomendadas para mejorar el rendimiento para SQL Server en máquinas virtuales de Azure

## Información general

En este tema se ofrecen prácticas recomendadas para optimizar el rendimiento de SQL Server en máquina virtual de Microsoft Azure. Mientras se ejecuta SQL Server en máquinas virtuales de Azure, se recomienda seguir usando las mismas opciones de ajuste de rendimiento de base de datos que son aplicables a SQL Server en el entorno de servidor local. Sin embargo, el rendimiento de una base de datos relacional en una nube pública depende de muchos factores como el tamaño de una máquina virtual y la configuración de los discos de datos.

Al crear imágenes de SQL Server, [considere la posibilidad de aprovisionar las máquinas virtuales en el portal de Azure](virtual-machines-windows-portal-sql-server-provision.md). Las máquinas virtuales de SQL Server aprovisionadas en el portal con Resource Manager incorporan todas estas prácticas recomendadas, incluida la configuración del almacenamiento.

Este artículo se centra en cómo obtener el *mejor* rendimiento de SQL Server en máquinas virtuales de Azure. Si su carga de trabajo no es menos exigente, podría no necesitar todas las optimizaciones enumeradas a continuación. Tenga en cuenta sus necesidades de rendimiento y patrones de carga de trabajo a medida que evalúe estas recomendaciones.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Lista de comprobación rápida

La siguiente es una lista de comprobación rápida para un rendimiento óptimo de SQL Server en máquinas virtuales de Azure:

|Ámbito|Optimizaciones|
|---|---|
|[Tamaño de VM](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) o superior para la edición SQL Enterprise.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) o superior para las ediciones SQL Standard y Web.|
|[Almacenamiento](#storage-guidance)|Use [Almacenamiento premium](../storage/storage-premium-storage.md). Solo se recomienda el almacenamiento estándar en fases de desarrollo o pruebas.<br/><br/>Mantenga la [cuenta de almacenamiento](../storage/storage-create-storage-account.md) y la máquina virtual de SQL Server en la misma región.<br/><br/>Deshabilite el [almacenamiento con redundancia geográfica](../storage/storage-redundancy.md) (replicación geográfica) de Azure en la cuenta de almacenamiento.|
|[Discos](#disks-guidance)|Utilice como mínimo 2 [discos P30](../storage/storage-premium-storage.md#scalability-and-performance-targets-whes-ESing-premium-storage) (1 para los archivos de registro y 1 para los archivos de datos y TempDB).<br/><br/>Evite usar el sistema operativo o discos temporales para el registro o el almacenamiento de bases de datos.<br/><br/>Habilite la lectura del almacenamiento en caché en los discos en los que se hospedan los archivos de datos y TempDB.<br/><br/>No habilite el almacenamiento en caché en los discos que hospeden el archivo de registro.<br/><br/>Coloque en franjas varios discos de datos de Azure para obtener un mayor rendimiento de E/S.<br/><br/>Formato con tamaños de asignación documentados.|
|[E/S](#io-guidance)|Habilite la compresión de páginas de bases de datos.<br/><br/>Habilite la inicialización instantánea de archivos para archivos de datos.<br/><br/>Limite o deshabilite el crecimiento automático de la base de datos.<br/><br/>Deshabilite la reducción automática de la base de datos.<br/><br/>Mueva todas las bases de datos a discos de datos, incluidas bases de datos del sistema.<br/><br/>Mueva los directorios de archivos de seguimiento y registros de errores de SQL Server a discos de datos.<br/><br/>Configure ubicaciones predeterminadas de archivo de copia de seguridad y base de datos.<br/><br/>Habilite páginas bloqueadas.<br/><br/>Aplique correcciones de rendimiento de SQL Server.|
|[Características específicas](#feature-specific-guidance)|Realice una copia de seguridad directamente en el almacenamiento de blobs.|

Para más información sobre *cómo* y *por qué* llevar a cabo estas optimizaciones, repase los detalles y las instrucciones proporcionadas en las siguientes secciones.

## Orientación sobre el tamaño de máquina virtual

En aplicaciones sensibles al rendimiento, se recomienda usar los siguientes [tamaños de máquinas virtuales](virtual-machines-windows-sizes.md):

- **SQL Server Enterprise Edition**: DS3 o versiones posteriores

- **Ediciones de SQL Server Standard y Web**: DS2 o versiones posteriores


## Orientación sobre el almacenamiento

Las máquinas virtuales de la serie DS (además de las series DSv2 y GS) admiten el [Almacenamiento premium](../storage/storage-premium-storage.md). Almacenamiento premium es aconsejable para cargas de trabajo de producción.

> [AZURE.WARNING] El almacenamiento estándar presenta un ancho de banda y unas latencias variables, de modo que solo se recomienda para cargas de trabajo de desarrollo o de prueba. En las cargas de trabajo de producción conviene usar Almacenamiento premium.

Además, se recomienda crear su cuenta de almacenamiento de Azure en el mismo centro de datos que sus máquinas virtuales de SQL Server para reducir los retrasos en la transferencia. Al crear una cuenta de almacenamiento, deshabilite la replicación geográfica ya que no se garantiza el orden de escritura coherente entre varios discos. En su lugar, considere la posibilidad de configurar una tecnología de recuperación ante desastres de SQL Server entre dos centros de datos de Azure. Para obtener más información, vea [Alta disponibilidad y recuperación ante desastres para SQL Server en máquinas virtuales de Azure](virtual-machines-windows-sql-high-availability-dr.md).

## Orientación sobre los discos

En una máquina virtual de Azure hay tres tipos de disco principales:

- **Disco del SO**: al crear una máquina virtual de Azure, la plataforma conectará al menos un disco (etiquetado como unidad **C**) a la máquina virtual como disco del sistema operativo. El disco es un VHD almacenado como blob en páginas en almacenamiento.
- **Disco temporal**: las máquinas virtuales de Azure contienen otro disco denominado disco temporal (etiquetado como unidad **D:**). Se trata de un disco en el nodo que se puede usar para el espacio de desecho.
- **Discos de datos**: también puede conectar más discos a la máquina virtual como discos de datos. Estos se guardarán en el almacenamiento como blobs en páginas.

En las siguientes secciones se incluyen recomendaciones sobre cómo usar cada uno de estos discos.

### Disco del sistema operativo

Un disco del sistema operativo es un VHD que se puede arrancar y montar como una versión en ejecución de un sistema operativo y está etiquetado como la unidad **C**.

La directiva del almacenamiento en caché predeterminada en el disco del sistema operativo es **Lectura/escritura**. Para aplicaciones sensibles al rendimiento, se recomienda usar discos de datos en lugar del disco del sistema operativo. Vea la sección sobre Discos de datos a continuación.

### Disco temporal

La unidad de almacenamiento temporal, etiquetada como la unidad **D**:, no se conserva en el almacenamiento de blobs de Azure. No almacene archivos de base de datos de usuarios ni archivos de registro de transacciones de usuarios en la unidad **D:**.

En máquinas virtuales de las series D, Dv2 y G, la unidad temporal está ubicada en discos SSD. Si la carga de trabajo implica un uso intensivo de TempDB (por ejemplo, con objetos temporales o combinaciones complejas), el almacenamiento de TempDB en la unidad **D** podría dar lugar a un mayor rendimiento de TempDB y reducir su latencia.

En las máquinas virtuales que admiten Almacenamiento premium (series DS, DSv2 y GS), se recomienda almacenar TempDB o las extensiones de grupo de búferes en un disco que admita Almacenamiento premium y que tenga habilitado el almacenamiento en caché de lectura. Esta recomendación tiene una excepción: si se realiza un uso de TempDB intensivo en cuanto a escritura, podrá lograr un mayor rendimiento si almacena esta base de datos en la unidad **D** local, que en estos tamaños de equipos también está ubicada en un disco SSD

### Discos de datos

- **Use discos de datos para los archivos de datos y de registro**: como mínimo, use 2 [discos P30](../storage/storage-premium-storage.md#scalability-and-performance-targets-whes-ESing-premium-storage) de Almacenamiento premium, uno para contener los archivos de registro y otro para contener los archivos de datos y TempDB.

- **Seccionamiento de discos**: para disfrutar de un mayor rendimiento, puede agregar más discos de datos y usar el seccionamiento de discos. Para determinar el número de discos de datos, debe analizar el número de IOPS disponibles para los discos de datos y de registro. Para ver esta información, vea las tablas de E/S por segundo por [tamaño de máquina virtual](virtual-machines-windows-sizes.md) y tamaño del disco en el siguiente artículo: [Uso de almacenamiento Premium para discos](../storage/storage-premium-storage.md). Use estas directrices:

	- Para Windows 8/Windows Server 2012 o versiones posteriores, use [Espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx). Configure el tamaño de franja en 64 KB para cargas de trabajo de OLTP y 256 KB para cargas de trabajo de almacenamiento de datos para evitar el impacto del rendimiento debido a la falta de alineación de particiones. Además, establezca el número de columnas = número de discos físicos. Para configurar un espacio de almacenamiento con más de 8 discos, debe usar PowerShell (no la interfaz de usuario del Administrador del servidor) para establecer explícitamente el número de columnas para que coincida con el número de discos. Para obtener más información sobre cómo configurar [Espacios de almacenamiento](https://technet.microsoft.com/library/hh831739.aspx), vea [Cmdlets de espacios de almacenamiento en Windows PowerShell](https://technet.microsoft.com/library/jj851254.aspx).

	- Para Windows 2008 R2 o versiones anteriores, puede usar discos dinámicos (volúmenes seccionados del SO) y el tamaño de la franja siempre es 64 KB. Tenga en cuenta que esta opción es obsoleta a partir de Windows 8/Windows Server 2012. Para obtener información, vea la declaración de soporte técnico en [Servicio de disco virtual está realizando la transición a la API de administración de almacenamiento de Windows](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

	- Si su carga de trabajo no es intensiva de registro y no necesita IOP dedicadas, puede configurar solo un grupo de almacenamiento. De lo contrario, cree dos grupos de almacenamiento, uno para los archivos de registro y otro grupo de almacenamiento para los archivos de datos y TempDB. Determine el número de discos asociados a cada grupo de almacenamiento en función de sus expectativas de carga. Tenga en cuenta que diferentes tamaños de máquina virtual permiten diferentes números de discos de datos conectados. Para obtener más información, consulte [Tamaños de máquinas virtuales](virtual-machines-windows-sizes.md).

	- Si no usa Almacenamiento premium (escenarios de desarrollo o de prueba), se recomienda agregar el número máximo de discos de datos admitidos por el [tamaño de la VM](virtual-machines-windows-sizes.md) y usar el seccionamiento de discos.

- **Directiva de almacenamiento en caché**: en los discos de datos de Almacenamiento premium, habilite el almacenamiento en caché de lectura en los discos de datos que hospedan solo sus archivos de datos y TempDB. Si no usa el Almacenamiento premium, no habilite el almacenamiento en caché en los discos de datos. Para obtener instrucciones sobre la configuración del almacenamiento en caché de disco, vea los siguientes temas: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) y [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

- **Tamaño de unidad de asignación de NTFS**: al formatear el disco de datos, se recomienda usar un tamaño de unidad de asignación de 64 KB para los archivos de datos y de registro, además de TempDB.

## Orientación sobre E/S

- Los mejores resultados con el Almacenamiento premium se logran al establecer paralelismos de su aplicación y solicitudes. El almacenamiento premium está diseñado para escenarios donde la profundidad de la cola de E/S es mayor que 1, por lo que verá poco o ningún aumento del rendimiento para las solicitudes de serie de subproceso único (incluso si son intensivas de almacenamiento). Por ejemplo, esto podría afectar a los resultados de pruebas de subproceso único de herramientas de análisis de rendimiento, como SQLIO.

- Recuerde que también puede usar la [compresión de páginas de bases de datos](https://msdn.microsoft.com/library/cc280449.aspx), ya que puede ayudarle a mejorar el rendimiento de las cargas de trabajo intensivas de E/S. Sin embargo, la compresión de datos puede aumentar el consumo de la CPU en el servidor de bases de datos.

- Considere la posibilidad de habilitar la inicialización instantánea de archivos para reducir el tiempo necesario para la asignación inicial de archivos. Para aprovechar la inicialización instantánea de archivos, se concede la cuenta de servicio de SQL Server (MSSQLSERVER) con SE\_MANAGE\_VOLUME\_NAME y se agrega a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**. Si usa una imagen de la plataforma de SQL Server para Azure, la cuenta de servicio predeterminada (NT Service\\MSSQLSERVER) no se agrega a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**. En otras palabras, la inicialización instantánea de archivos no se habilita en una imagen de plataforma de SQL Server Azure. Después de agregar la cuenta de servicio de SQL Server a la directiva de seguridad **Realizar tareas de mantenimiento del volumen**, reinicie el servicio de SQL Server. Puede haber consideraciones de seguridad para usar esta característica. Para obtener más información, consulte [Inicialización de archivos de base de datos](https://msdn.microsoft.com/library/ms175935.aspx).

- El **crecimiento automático** se considera apenas una contingencia para el crecimiento inesperado. No administre su crecimiento de datos y registros a diario con el crecimiento automático. Si se usa el crecimiento automático, haga crecer previamente el archivo mediante el modificador Tamaño.

- Asegúrese de que la **reducción automática** está deshabilitada para evitar una sobrecarga innecesaria que puede afectar negativamente al rendimiento.

- Mueva todas las bases de datos a discos de datos, incluidas bases de datos del sistema. Para más información, consulte [Mover bases de datos del sistema](https://msdn.microsoft.com/library/ms345408.aspx).

- Mueva los directorios de archivos de seguimiento y registros de errores de SQL Server a discos de datos. Para llevar esto a cabo en el Administrador de configuración de SQL Server, haga clic con el botón secundario en la instancia de SQL Server y seleccione Propiedades. La configuración de los archivos de registro y de seguimiento de errores se puede cambiar en la pestaña **Parámetros de inicio**. El directorio de volcado se especifica en la pestaña **Opciones avanzadas**. En la siguiente captura de pantalla se muestra dónde buscar el parámetro de inicio del registro de errores.

	![Captura de pantalla de registro de errores de SQL](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- Configure ubicaciones predeterminadas para los archivos de base de datos y de copia de seguridad. Use las recomendaciones de este tema y realice los cambios necesarios en la ventana Propiedades del servidor. Para obtener instrucciones, consulte [Ver o cambiar las ubicaciones predeterminadas de los archivos de datos y registro (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). En la siguiente captura de pantalla se muestra dónde realizar estos cambios.

	![Archivos de copia de seguridad y de registro de datos de SQL](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Habilite páginas bloqueadas para reducir la E/S y las actividades de paginación. Para más información, consulte [Habilitar la opción Bloquear páginas en la memoria (Windows)](https://msdn.microsoft.com/library/ms190730.aspx).

- Si está ejecutando SQL Server 2012, instale la actualización acumulativa 10 del Service Pack 1. Esta actualización contiene la corrección para el rendimiento deficiente en la E/S al ejecutar select en la instrucción de tabla temporal en SQL Server 2012. Para obtener información, consulte este [artículo de Knowledge Base](http://support.microsoft.com/kb/2958012).

- Considere la posibilidad de comprimir los archivos de datos cuando se transfieren dentro y fuera de Azure.

## Orientación sobre las características específicas

Algunas implementaciones pueden lograr ventajas de rendimiento adicionales mediante técnicas de configuración más avanzadas. En la siguiente lista se destacan algunas características de SQL Server que pueden ayudarle a lograr un mejor rendimiento:

- **Copia de seguridad para el almacenamiento de Azure**: al realizar copias de seguridad para SQL Server que se ejecutan en máquinas virtuales de Azure, puede usar [Copia de seguridad de SQL Server a URL](https://msdn.microsoft.com/library/dn435916.aspx). Esta característica está disponible a partir de SQL Server 2012 SP1 CU2 y se recomienda para las copias de seguridad en los discos de datos conectados. Al realizar copias de seguridad o restaurar en o desde el almacenamiento de Azure, siga las recomendaciones que se ofrecen en [Solución de problemas y prácticas recomendadas de copia de seguridad de SQL Server en URL y restauración a partir de copias de seguridad almacenadas en el Almacenamiento de Azure](https://msdn.microsoft.com/library/jj919149.aspx). También puede automatizar estas copias de seguridad con [Copia de seguridad automatizada para SQL Server en Máquinas virtuales de Azure](virtual-machines-windows-classic-sql-automated-backup.md).

	Antes de SQL Server 2012, puede usar la [Herramienta de copia de seguridad de SQL Server a Azure](https://www.microsoft.com/download/details.aspx?id=40740). Esta herramienta puede ayudar a aumentar el rendimiento de la copia de seguridad con varios destinos de franjas de copia de seguridad.

- **Archivos de datos de SQL Server en Azure**: esta nueva característica, [Archivos de datos de SQL Server en Azure](https://msdn.microsoft.com/library/dn385720.aspx), está disponible a partir de SQL Server 2014. La ejecución de SQL Server con archivos de datos en Azure muestra características de rendimiento comparables con el uso de discos de datos de Azure.

## Pasos siguientes

Si está interesado en explorar SQL Server y el Almacenamiento premium en mayor profundidad, vea el artículo [Uso del Almacenamiento premium de Azure con SQL Server en máquinas virtuales](virtual-machines-windows-classic-sql-server-premium-storage.md).

Para ver prácticas recomendadas de seguridad, vea [Consideraciones de seguridad para SQL Server en máquinas virtuales de Azure](virtual-machines-windows-sql-security.md).

Revise otros temas de la máquina virtual de SQL Server en [Información general sobre SQL Server en Máquinas virtuales de Azure](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0518_2016-->