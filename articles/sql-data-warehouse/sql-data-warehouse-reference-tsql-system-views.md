<properties
   pageTitle="SQL Data Warehouse Systemansichten | Microsoft Azure"
   description="Links zu System zeigt Inhalte für SQL Data Warehouse."
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
   ms.date="08/22/2016"
   ms.author="barbkess;sonyama"/>

# <a name="system-views"></a>Ansichten

## <a name="sql-data-warehouse-catalog-views"></a>SQL Data Warehouse Katalogsichten

- [Sys.pdw_column_distribution_properties](http://msdn.microsoft.com/library/mt204022.aspx)
- [Sys.pdw_database_mappings](http://msdn.microsoft.com/library/mt203891.aspx)
- [Sys.pdw_distributions](http://msdn.microsoft.com/library/mt203892.aspx)
- [Sys.pdw_index_mappings](http://msdn.microsoft.com/library/mt203912.aspx)
- [Sys.pdw_loader_backup_run_details](https://msdn.microsoft.com/library/mt203877.aspx)
- [Sys.pdw_loader_backup_runs](https://msdn.microsoft.com/library/mt203884.aspx)
- [Sys.pdw_nodes_column_store_dictionaries](http://msdn.microsoft.com/library/mt203902.aspx)
- [Sys.pdw_nodes_column_store_row_groups](http://msdn.microsoft.com/library/mt203880.aspx)
- [Sys.pdw_nodes_column_store_segments](http://msdn.microsoft.com/library/mt203916.aspx)
- [Sys.pdw_nodes_columns](http://msdn.microsoft.com/library/mt203881.aspx)
- [Sys.pdw_nodes_indexes](http://msdn.microsoft.com/library/mt203885.aspx)
- [Sys.pdw_nodes_partitions](http://msdn.microsoft.com/library/mt203908.aspx)
- [Sys.pdw_nodes_pdw_physical_databases](http://msdn.microsoft.com/library/mt203897.aspx)
- [Sys.pdw_nodes_tables](http://msdn.microsoft.com/library/mt203886.aspx)
- [Sys.pdw_table_distribution_properties](http://msdn.microsoft.com/library/mt203896.aspx)
- [Sys.pdw_table_mappings](http://msdn.microsoft.com/library/mt203876.aspx)

## <a name="sql-data-warehouse-dynamic-management-views-dmvs"></a>SQL Data Warehouse dynamische Verwaltungsansichten (DMVs)

- [Sys.dm_pdw_dms_cores](http://msdn.microsoft.com/library/mt203911.aspx)
- [Sys.dm_pdw_dms_external_work](https://msdn.microsoft.com/library/mt204024.aspx)
- [Sys.dm_pdw_dms_workers](http://msdn.microsoft.com/library/mt203878.aspx)
- [Sys.dm_pdw_errors](http://msdn.microsoft.com/library/mt203904.aspx)
- [Sys.dm_pdw_exec_connections](http://msdn.microsoft.com/library/mt203882.aspx)
- [Sys.dm_pdw_exec_requests](http://msdn.microsoft.com/library/mt203887.aspx)
- [Sys.dm_pdw_exec_sessions](http://msdn.microsoft.com/library/mt203883.aspx)
- [Sys.dm_pdw_hadoop_operations](https://msdn.microsoft.com/library/mt204032.aspx)
- [Sys.dm_pdw_lock_waits](http://msdn.microsoft.com/library/mt203901.aspx)
- [Sys.dm_pdw_nodes](http://msdn.microsoft.com/library/mt203907.aspx)
- [Sys.dm_pdw_nodes_database_encryption_keys](http://msdn.microsoft.com/library/mt203922.aspx)
- [Sys.dm_pdw_os_threads](http://msdn.microsoft.com/library/mt203917.aspx)
- [Sys.dm_pdw_request_steps](http://msdn.microsoft.com/library/mt203913.aspx)
- [Sys.dm_pdw_resource_waits](http://msdn.microsoft.com/library/mt203906.aspx)
- [Sys.dm_pdw_sql_requests](http://msdn.microsoft.com/library/mt203889.aspx)
- [Sys.dm_pdw_sys_info](http://msdn.microsoft.com/library/mt203900.aspx)
- [Sys.dm_pdw_wait_stats](http://msdn.microsoft.com/library/mt203909.aspx)
- [Sys.dm_pdw_waits](http://msdn.microsoft.com/library/mt203909.aspx)

## <a name="sql-server-dmvs-applicable-to-sql-data-warehouse"></a>SQL Server DMVs für SQL Data Warehouse

Die folgenden DMVs gelten für SQL Data Warehouse jedoch müssen durch Herstellen einer Verbindung mit der **master** -Datenbank ausgeführt werden.

- [Sys.database_service_objectives](https://msdn.microsoft.com/library/mt712619.aspx)
- [Sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx)
- [Sys.fn_helpcollations()](https://msdn.microsoft.com/library/ms187963.aspx)

## <a name="sql-server-catalog-views"></a>SQL Server-Katalogsichten

- [Sys.ALL_COLUMNS](http://msdn.microsoft.com/library/ms177522.aspx)
- [Sys.all_objects](http://msdn.microsoft.com/library/ms178618.aspx)
- [Sys.all_parameters](http://msdn.microsoft.com/library/ms190340.aspx)
- [Sys.all_sql_modules](http://msdn.microsoft.com/library/ms184389.aspx)
- [Sys.all_views](http://msdn.microsoft.com/library/ms189510.aspx)
- [Sys.Assemblies](http://msdn.microsoft.com/library/ms189790.aspx)
- [Sys.assembly_modules](http://msdn.microsoft.com/library/ms180052.aspx)
- [Sys.assembly_types](http://msdn.microsoft.com/library/ms178020.aspx)
- [Sys.Certificates](http://msdn.microsoft.com/library/ms189774.aspx)
- [Sys.check_constraints](http://msdn.microsoft.com/library/ms187388.aspx)
- [Sys.Columns](http://msdn.microsoft.com/library/ms176106.aspx)
- [Sys.computed_columns](http://msdn.microsoft.com/library/ms188744.aspx)
- [Sys.Credentials](https://msdn.microsoft.com/library/ms189745.aspx)
- [Sys.data_spaces](http://msdn.microsoft.com/library/ms190289.aspx)
- [Sys.database_credentials](https://msdn.microsoft.com/library/mt270282.aspx)
- [database_files](http://msdn.microsoft.com/library/ms174397.aspx)
- [Sys.database_permissions](http://msdn.microsoft.com/library/ms188367.aspx)
- [Kataloganzeige](http://msdn.microsoft.com/library/ms187328.aspx)
- [Sys.database_role_members](http://msdn.microsoft.com/library/ms189780.aspx)
- [Sys.Databases](http://msdn.microsoft.com/library/ms178534.aspx)
- [Sys.default_constraints](http://msdn.microsoft.com/library/ms173758.aspx)
- [Sys.external_data_sources](http://msdn.microsoft.com/library/dn935019.aspx)
- [Sys.external_file_formats](http://msdn.microsoft.com/library/dn935025.aspx)
- [Sys.external_tables](http://msdn.microsoft.com/library/dn935029.aspx)
- [Sys.FileGroups](http://msdn.microsoft.com/library/ms187782.aspx)
- [Sys.foreign_key_columns](http://msdn.microsoft.com/library/ms186306.aspx)
- [Sys.Foreign_Keys](http://msdn.microsoft.com/library/ms189807.aspx)
- [Sys.identity_columns](http://msdn.microsoft.com/library/ms187334.aspx)
- [index_columns](http://msdn.microsoft.com/library/ms175105.aspx)
- [Sys.Indexes](http://msdn.microsoft.com/library/ms173760.aspx)
- [Sys.key_constraints](http://msdn.microsoft.com/library/ms174321.aspx)
- [Sys.numbered_procedures](http://msdn.microsoft.com/library/ms179865.aspx)
- [Sys.Objects](http://msdn.microsoft.com/library/ms190324.aspx)
- [Sys.Parameters](http://msdn.microsoft.com/library/ms176074.aspx)
- [Sys.partition_functions](http://msdn.microsoft.com/library/ms187381.aspx)
- [Sys.partition_parameters](http://msdn.microsoft.com/library/ms175054.aspx)
- [Sys.partition_range_values](http://msdn.microsoft.com/library/ms187780.aspx)
- [Sys.partition_schemes](http://msdn.microsoft.com/library/ms189752.aspx)
- [Sys.Partitions](http://msdn.microsoft.com/library/ms175012.aspx)
- [Sys.Procedures](http://msdn.microsoft.com/library/ms188737.aspx)
- [Sys.Schemas](http://msdn.microsoft.com/library/ms176011.aspx)
- [Sys.securable_classes](http://msdn.microsoft.com/library/ms408301.aspx)
- [Sys.sql_expression_dependencies](http://msdn.microsoft.com/library/bb677315.aspx)
- [Sys.sql_modules](http://msdn.microsoft.com/library/ms175081.aspx)
- [Sys.Stats](http://msdn.microsoft.com/library/ms177623.aspx)
- [Sys.stats_columns](http://msdn.microsoft.com/library/ms187340.aspx)
- [Sys.symmetric_keys](https://msdn.microsoft.com/library/ms189446.aspx)
- [Sys.Synonyms](https://msdn.microsoft.com/library/ms189458.aspx)
- [Sys.syscharsets](https://msdn.microsoft.com/library/ms190300.aspx)
- [Sys.syscolumns](https://msdn.microsoft.com/library/ms186816.aspx)
- [Sys.sysdatabases](https://msdn.microsoft.com/library/ms179900.aspx)
- [Sys.syslanguages](https://msdn.microsoft.com/library/ms190303.aspx)
- [Sys.sysobjects](https://msdn.microsoft.com/library/ms177596.aspx)
- [Sys.sysreferences](https://msdn.microsoft.com/library/ms186900.aspx)
- [Sys.system_columns](http://msdn.microsoft.com/library/ms178596.aspx)
- [Sys.system_objects](http://msdn.microsoft.com/library/ms173551.aspx)
- [Sys.system_parameters](https://msdn.microsoft.com/library/ms174367.aspx)
- [Sys.system_sql_modules](http://msdn.microsoft.com/library/ms188034.aspx)
- [Sys.system_views](http://msdn.microsoft.com/library/ms187764.aspx)
- [Sys.systypes](https://msdn.microsoft.com/library/ms175109.aspx)
- [Sys.sysusers](https://msdn.microsoft.com/library/ms179871.aspx)
- [' sys.Tables '](http://msdn.microsoft.com/library/ms187406.aspx)
- [Sys.Types](http://msdn.microsoft.com/library/ms188021.aspx)
- [Sys.Views](http://msdn.microsoft.com/library/ms190334.aspx)

## <a name="sql-server-dmvs-available-in-sql-data-warehouse"></a>SQL Server DMVs im SQL Data Warehouse verfügbar

SQL Data Warehouse stellt viele der dynamische Verwaltungsansichten (DMVs) von SQL Server. Diese Ansichten, wenn im SQL Data Warehouse abgefragt Berichten den Status von SQL-Datenbanken auf die Verteilung.

Da SQL Data Warehouse auf Microsofts MPP-Technologie basiert, verwenden Sie SQL Data Warehouse und Analytics Plattformsystem Parallel Data Warehouse (PDW) dieselbe Systemansichten.

Deshalb alle diese DMV eine bestimmte Spalte namens Pdw_node_id. Dies ist der Bezeichner für den Compute-Knoten. In PDW Compute ist Knoten eine stärkere Konzept für die Architektur. Im SQL Data Warehouse stützt sich stärker auf die Verteilung die Architektur.

>[AZURE.NOTE] Um diese Ansicht zu verwenden, fügen Sie 'Pdw_nodes_' den Namen wie in der folgenden Tabelle dargestellt.


| DMV-Name im SQL Data Warehouse | Link zum Thema SQL Server Transact-SQL auf MSDN |
| :----------------------------- | :-------------------------------------------- |
| Sys.dm_pdw_nodes_db_file_space_usage | [Sys.dm_db_file_space_usage](http://msdn.microsoft.com/library/ms174412.aspx) |
| Sys.dm_pdw_nodes_db_index_usage_stats | [dm_db_index_usage_stats](http://msdn.microsoft.com/library/ms188755.aspx) |
| Sys.dm_pdw_nodes_db_partition_stats | [Sys.dm_db_partition_stats](http://msdn.microsoft.com/library/ms187737.aspx) |
| Sys.dm_pdw_nodes_db_session_space_usage | [Sys.dm_db_session_space_usage](http://msdn.microsoft.com/library/ms187938.aspx) |
| Sys.dm_pdw_nodes_db_task_space_usage | [Sys.dm_db_task_space_usage](http://msdn.microsoft.com/library/ms190288.aspx) |
| Sys.dm_pdw_nodes_exec_background_job_queue | [Sys.dm_exec_background_job_queue](http://msdn.microsoft.com/library/ms173512.aspx) |
| Sys.dm_pdw_nodes_exec_background_job_queue_stats | [Sys.dm_exec_background_job_queue_stats](http://msdn.microsoft.com/library/ms176059.aspx) |
| Sys.dm_pdw_nodes_exec_cached_plans | [Sys.dm_exec_cached_plans](http://msdn.microsoft.com/library/ms187404.aspx) |
| Sys.dm_pdw_nodes_exec_connections | [Sys.dm_exec_connections](http://msdn.microsoft.com/library/ms181509.aspx) |
| Sys.dm_pdw_nodes_exec_procedure_stats | [Sys.dm_exec_procedure_stats](http://msdn.microsoft.com/library/cc280701.aspx) |
| Sys.dm_pdw_nodes_exec_query_memory_grants | [Sys.dm_exec_query_memory_grants](http://msdn.microsoft.com/library/ms365393.aspx) |
| Sys.dm_pdw_nodes_exec_query_optimizer_info | [Sys.dm_exec_query_optimizer_info](http://msdn.microsoft.com/library/ms175002.aspx) |
| Sys.dm_pdw_nodes_exec_query_resource_semaphores | [Sys.dm_exec_query_resource_semaphores](http://msdn.microsoft.com/library/ms366321.aspx) |
| Sys.dm_pdw_nodes_exec_query_stats | [dm_exec_query_stats](http://msdn.microsoft.com/library/ms189741.aspx) |
| Sys.dm_pdw_nodes_exec_requests | [dm_exec_requests](http://msdn.microsoft.com/library/ms177648.aspx) |
| Sys.dm_pdw_nodes_exec_sessions | [Sys.dm_exec_sessions](https://msdn.microsoft.com/en-us/library/ms176013.aspx) |
| Sys.dm_pdw_nodes_io_pending_io_requests | [Sys.dm_io_pending_io_requests](http://msdn.microsoft.com/library/ms188762.aspx) |
| Sys.dm_pdw_nodes_os_buffer_descriptors | [Sys.dm_os_buffer_descriptors](http://msdn.microsoft.com/library/ms173442.aspx) |
| Sys.dm_pdw_nodes_os_child_instances | [Sys.dm_os_child_instances](http://msdn.microsoft.com/library/ms165698.aspx) |
| Sys.dm_pdw_nodes_os_cluster_nodes | [Sys.dm_os_cluster_nodes](http://msdn.microsoft.com/library/ms187341.aspx) |
| Sys.dm_pdw_nodes_os_dispatcher_pools | [Sys.dm_os_dispatcher_pools](http://msdn.microsoft.com/library/bb630336.aspx) |
| Sys.dm_pdw_nodes_os_dispatchers | Transact-SQL-Dokumentation ist nicht verfügbar. |
| Sys.dm_pdw_nodes_os_hosts | [Sys.dm_os_hosts](http://msdn.microsoft.com/library/ms187800.aspx) |
| Sys.dm_pdw_nodes_os_latch_stats | [Sys.dm_os_latch_stats](http://msdn.microsoft.com/library/ms175066.aspx) |
| Sys.dm_pdw_nodes_os_memory_brokers | [Sys.dm_os_memory_brokers](http://msdn.microsoft.com/library/bb522548.aspx) |
| Sys.dm_pdw_nodes_os_memory_cache_clock_hands | [Sys.dm_os_memory_cache_clock_hands](http://msdn.microsoft.com/library/ms173786.aspx) |
| Sys.dm_pdw_nodes_os_memory_cache_counters | [Sys.dm_os_memory_cache_counters](http://msdn.microsoft.com/library/ms188760.aspx) |
| Sys.dm_pdw_nodes_os_memory_cache_entries | [Sys.dm_os_memory_cache_entries](http://msdn.microsoft.com/library/ms189488.aspx) |
| Sys.dm_pdw_nodes_os_memory_cache_hash_tables | [Sys.dm_os_memory_cache_hash_tables](http://msdn.microsoft.com/library/ms182388.aspx) |
| Sys.dm_pdw_nodes_os_memory_clerks | [Sys.dm_os_memory_clerks](http://msdn.microsoft.com/library/ms175019.aspx) |
| Sys.dm_pdw_nodes_os_memory_node_access_stats | Transact-SQL-Dokumentation ist nicht verfügbar. |
| Sys.dm_pdw_nodes_os_memory_nodes | [Sys.dm_os_memory_nodes](http://msdn.microsoft.com/library/bb510622.aspx) |
| Sys.dm_pdw_nodes_os_memory_objects | [Sys.dm_os_memory_objects](https://msdn.microsoft.com/library/ms179875.aspx) |
| Sys.dm_pdw_nodes_os_memory_pools | [Sys.dm_os_memory_pools](http://msdn.microsoft.com/library/ms175022.aspx) |
| Sys.dm_pdw_nodes_os_nodes | [Sys.dm_os_nodes](http://msdn.microsoft.com/library/bb510628.aspx) |
| Sys.dm_pdw_nodes_os_performance_counters | [Sys.dm_os_performance_counters](http://msdn.microsoft.com/library/ms187743.aspx) |
| Sys.dm_pdw_nodes_os_process_memory | [Sys.dm_os_process_memory](http://msdn.microsoft.com/library/bb510747.aspx) |
| Sys.dm_pdw_nodes_os_schedulers | [Sys.DM_OS_SCHEDULERS](http://msdn.microsoft.com/library/ms177526.aspx) |
| Sys.dm_pdw_nodes_os_spinlock_stats | Transact-SQL-Dokumentation ist nicht verfügbar. |
| Sys.dm_pdw_nodes_os_sys_info | [Sys.dm_os_sys_info](http://msdn.microsoft.com/library/ms175048.aspx) |
| Sys.dm_pdw_nodes_os_sys_memory | [Sys.dm_os_memory_nodes](http://msdn.microsoft.com/library/bb510622.aspx) |
| Sys.dm_pdw_nodes_os_tasks | [Sys.dm_os_tasks](http://msdn.microsoft.com/library/ms174963.aspx) |
| Sys.dm_pdw_nodes_os_threads | [Sys.dm_os_threads](http://msdn.microsoft.com/library/ms187818.aspx) |
| Sys.dm_pdw_nodes_os_virtual_address_dump | [Sys.dm_os_virtual_address_dump](https://msdn.microsoft.com/library/ms186294.aspx) |
| Sys.dm_pdw_nodes_os_wait_stats | [dm_os_wait_stats](https://msdn.microsoft.com/library/ms179984.aspx) |
| Sys.dm_pdw_nodes_os_waiting_tasks | [Sys.dm_os_waiting_tasks](https://msdn.microsoft.com/library/ms188743.aspx) |
| Sys.dm_pdw_nodes_os_workers | [Sys.dm_os_workers](https://msdn.microsoft.com/library/ms178626.aspx) |
| Sys.dm_pdw_nodes_resource_governor_resource_pools | [Sys.dm_resource_governor_resource_pools](http://msdn.microsoft.com/library/bb934023.aspx) |
| Sys.dm_pdw_nodes_resource_governor_workload_groups | [Sys.dm_resource_governor_workload_groups](http://msdn.microsoft.com/library/bb934197.aspx) |
| Sys.dm_pdw_nodes_tran_active_snapshot_database_transactions | [Sys.dm_tran_active_snapshot_database_transactions](http://msdn.microsoft.com/library/ms180023.aspx) |
| Sys.dm_pdw_nodes_tran_active_transactions | [Sys.dm_tran_active_transactions](http://msdn.microsoft.com/library/ms174302.aspx) |
| Sys.dm_pdw_nodes_tran_commit_table | [Sys.dm_tran_commit_table](https://msdn.microsoft.com/library/cc645959.aspx) |
| Sys.dm_pdw_nodes_tran_current_snapshot | [Sys.dm_tran_current_snapshot](http://msdn.microsoft.com/library/ms184390.aspx) |
| Sys.dm_pdw_nodes_tran_current_transaction | [Sys.dm_tran_current_transaction](https://msdn.microsoft.com/library/ms186327.aspx) |
| Sys.dm_pdw_nodes_tran_database_transactions | [Sys.dm_tran_database_transactions](https://msdn.microsoft.com/library/ms186957.aspx) |
| Sys.dm_pdw_nodes_tran_locks | [Sys.dm_tran_locks](http://msdn.microsoft.com/library/ms190345.aspx) |
| Sys.dm_pdw_nodes_tran_session_transactions | [Sys.dm_tran_session_transactions](http://msdn.microsoft.com/library/ms188739.aspx) |
| Sys.dm_pdw_nodes_tran_top_version_generators | [Sys.dm_tran_top_version_generators](http://msdn.microsoft.com/library/ms188778.aspx) |

## <a name="sql-server-2016-polybase-dmvs-available-in-sql-data-warehouse"></a>SQL Server 2016 PolyBase DMVs verfügbar im SQL Data Warehouse

- [Sys.dm_exec_compute_node_errors](http://msdn.microsoft.com/library/mt146380.aspx)
- [Sys.dm_exec_compute_node_status](http://msdn.microsoft.com/library/mt146382.aspx)
- [Sys.dm_exec_compute_nodes](https://msdn.microsoft.com/library/mt130700.aspx)
- [Sys.dm_exec_distributed_request_steps](https://msdn.microsoft.com/library/mt130701.aspx)
- [Sys.dm_exec_distributed_requests](http://msdn.microsoft.com/library/mt146385.aspx)
- [Sys.dm_exec_distributed_sql_requests](http://msdn.microsoft.com/library/mt146390.aspx)
- [Sys.dm_exec_dms_services](https://msdn.microsoft.com/library/mt146374.aspx)
- [Sys.dm_exec_dms_workers](https://msdn.microsoft.com/library/mt146392.aspx)
- [Sys.dm_exec_external_operations](https://msdn.microsoft.com/library/mt146391.aspx)
- [Sys.dm_exec_external_work](https://msdn.microsoft.com/library/mt146375.aspx)

## <a name="sql-server-informationschema-views"></a>SQL Server INFORMATION_SCHEMA-Ansichten

- [CHECK_CONSTRAINTS](http://msdn.microsoft.com/library/ms189772.aspx)
- [SPALTEN](http://msdn.microsoft.com/library/ms188348.aspx)
- [PARAMETER](http://msdn.microsoft.com/library/ms173796.aspx)
- [ROUTINEN](https://msdn.microsoft.com/library/ms188757.aspx)
- [SCHEMATA](https://msdn.microsoft.com/library/ms182642.aspx)
- [TABELLEN](http://msdn.microsoft.com/library/ms186224.aspx)
- [VIEW_COLUMN_USAGE](https://msdn.microsoft.com/library/ms190492.aspx)
- [VIEW_TABLE_USAGE](https://msdn.microsoft.com/library/ms173869.aspx)
- [Ansichten](http://msdn.microsoft.com/library/ms181381.aspx)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [SQL Data Warehouse Referenz (Übersicht)][].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse Referenz (Übersicht)]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->


<!--Other Web references-->
