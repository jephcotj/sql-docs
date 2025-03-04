---
description: "sys.elastic_pool_resource_stats (Azure SQL Database)"
title: "sys.elastic_pool_resource_stats"
titleSuffix: Azure SQL Database
ms.date: "01/28/2019"
ms.service: sql-database
ms.prod_service: "sql-database"
ms.reviewer: ""
ms.topic: "reference"
dev_langs: 
  - "TSQL"
f1_keywords: 
  - "sys.elastic_pool_resource_stats catalog view"
helpviewer_keywords: 
  - "sys.elastic_pool_resource_stats_TSQL"
  - "sys.elastic_pool_resource_stats"
  - "elastic_pool_resource_stats_TSQL"
  - "elastic_pool_resource_stats"
ms.assetid: f242c1bd-3cc8-4c8b-8aaf-c79b6a8a0329
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: "= azuresqldb-current"
---
# sys.elastic_pool_resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Returns resource usage statistics for all the elastic pools in a SQL Database server. For each elastic pool, there is one row for each 15 second reporting window (four rows per minute). This includes CPU, IO, Log, storage consumption and concurrent request/session utilization by all databases in the pool. This data is retained for 14 days. 
  
||  
|-|  
|**Applies to**:  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] V12.|  
  
|Column name|Data type|Description|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime2**|UTC time indicating the start of the 15 second reporting interval.|  
|**end_time**|**datetime2**|UTC time indicating the end of the 15 second reporting interval.|  
|**elastic_pool_name**|**nvarchar(128)**|Name of the elastic database pool.|  
|**avg_cpu_percent**|**decimal(5,2)**|Average compute utilization in percentage of the limit of the pool.|  
|**avg_data_io_percent**|**decimal(5,2)**|Average I/O utilization in percentage based on the limit of the pool.|  
|**avg_log_write_percent**|**decimal(5,2)**|Average write resource utilization in percentage of the limit of the pool.|  
|**avg_storage_percent**|**decimal(5,2)**|Average storage utilization in percentage of the storage limit of the pool.|  
|**max_worker_percent**|**decimal(5,2)**|Maximum concurrent workers (requests) in percentage based on the limit of the pool.|  
|**max_session_percent**|**decimal(5,2)**|Maximum concurrent sessions in percentage based on the limit of the pool.|  
|**elastic_pool_dtu_limit**|**int**|Current max elastic pool DTU setting for this elastic pool during this interval.|  
|**elastic_pool_storage_limit_mb**|**bigint**|Current max elastic pool storage limit setting for this elastic pool in megabytes during this interval.|
|**avg_allocated_storage_percent**|**decimal(5,2)**|The percentage of data space allocated by all databases in the elastic pool.  This is the ratio of data space allocated to data max size for the elastic pool.  For more information see: [File space management in SQL Database](/azure/sql-database/sql-database-file-space-management)|  
  
## Remarks

 This view exists in the master database of the SQL Database server. You must be connected to the master database to query **sys.elastic_pool_resource_stats**.  
  
## Permissions

 Requires membership in the **dbmanager** role.  
  
## Examples

 The following example returns resource utilization data ordered by the most recent time for all the elastic database pools in the current SQL Database server.  
  
```sql
SELECT * FROM sys.elastic_pool_resource_stats
ORDER BY end_time DESC;  
```

 The following example calculates the average DTU percentage consumption for a given pool.  

```sql
SELECT start_time, end_time,
  (SELECT Max(v)
FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS value(v)) AS [avg_DTU_percent]
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = '<your pool name>'
ORDER BY end_time DESC;  
```

## See Also

 [Tame explosive growth with elastic databases](/azure/azure-sql/database/elastic-pool-overview)   
 [Create and manage a SQL Database elastic database pool](/azure/azure-sql/database/elastic-pool-overview)   
 [sys.resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [sys.dm_db_resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)  
  
