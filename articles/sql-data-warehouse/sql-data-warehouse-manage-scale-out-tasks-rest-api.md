<properties
   pageTitle="Administración de tareas de escalabilidad de Almacenamiento de datos SQL de Azure (REST) | Microsoft Azure"
   description="Tareas de la API de REST para escalar horizontalmente el rendimiento de Almacenamiento de datos SQL de Azure Cambiar recursos de proceso ajustando DWU. Pausar y reanudar recursos de proceso para ahorrar costos."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/28/2016"
   ms.author="barbkess;sonyama"/>

# Administración de tareas de escalabilidad de Almacenamiento de datos SQL de Azure (REST)

> [AZURE.SELECTOR]
- [Información general](sql-data-warehouse-overview-scalability.md)
- [Portal](sql-data-warehouse-manage-scale-out-tasks.md)
- [PowerShell](sql-data-warehouse-manage-scale-out-tasks-powershell.md)
- [REST](sql-data-warehouse-manage-scale-out-tasks-rest-api.md)
- [TSQL](sql-data-warehouse-manage-scale-out-tasks-tsql.md)

Escale horizontalmente con total flexibilidad memoria y recursos de proceso para satisfacer las demandas de cambio de su carga de trabajo y ahorre costos reduciendo recursos durante las horas de poca actividad.

Esta colección de tareas utiliza las API de REST para:

- Escalar el rendimiento mediante el ajuste de DWU
- Pausar recursos de proceso
- Reanudar recursos de proceso

Para obtener más información al respecto, consulte [Información general sobre la escalabilidad de rendimiento][].

<a name="scale-performance-bk"></a>

## Rendimiento a escala

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description (Descripción de escalado de DWU de Almacenamiento de datos SQL)](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Para cambiar las DWU, utilice la API de REST [Crear o actualizar base de datos][]. En el ejemplo siguiente se establece el objetivo de nivel de servicio en DW1000 para la base de datos MySQLDW que se hospeda en el servidor MyServer. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/MyServer/databases/MySQLDW?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

<a name="pause-compute-bk"></a>

## Pausa del proceso

[AZURE.INCLUDE [SQL Data Warehouse pause description (Descripción de pausa de Almacenamiento de datos SQL)](../../includes/sql-data-warehouse-pause-description.md)]

Para pausar una base de datos, utilice la API de REST [Pausar la base de datos][]. El siguiente ejemplo pausa una base de datos denominada Database02 que está hospedada en un servidor llamado Server01. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/pause?api-version=2014-04-01-preview HTTP/1.1
```

<a name="resume-compute-bk"></a>

## Reanudación del proceso

[AZURE.INCLUDE [SQL Data Warehouse resume description (Descripción de reanudación de Almacenamiento de datos SQL)](../../includes/sql-data-warehouse-resume-description.md)]

Para iniciar una base de datos, utilice la API de REST [Reanudar la base de datos][]. El siguiente ejemplo inicia una base de datos denominada Database02 que está hospedada en un servidor llamado Server01. El servidor está en un grupo de recursos de Azure denominado ResourceGroup1.

```
POST https://management.azure.com/subscriptions{subscription-id}/resourceGroups/ResourceGroup1/providers/Microsoft.Sql/servers/Server01/databases/Database02/resume?api-version=2014-04-01-preview HTTP/1.1
```

<a name="next-steps-bk"></a>

## Pasos siguientes

Para otras tareas de administración, consulte [Información general de administración][].

<!--Image references-->

<!--Article references-->
[Información general de administración]: ./sql-data-warehouse-overview-manage.md
[Información general sobre la escalabilidad de rendimiento]: ./sql-data-warehouse-overview-scalability.md

<!--MSDN references-->
[Pausar la base de datos]: https://msdn.microsoft.com/library/azure/mt718817.aspx
[Reanudar la base de datos]: https://msdn.microsoft.com/library/azure/mt718820.aspx
[Crear o actualizar base de datos]: https://msdn.microsoft.com/library/azure/mt163685.aspx

<!--Other Web references-->

[Azure portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0504_2016-->