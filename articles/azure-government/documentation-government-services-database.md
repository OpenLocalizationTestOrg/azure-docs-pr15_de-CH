<properties
    pageTitle="Azure Regierung Dokumentation | Microsoft Azure"
    description="Dies bietet einen Vergleich der Features und Hinweise auf die Anwendungsentwicklung für Azure"
    services="Azure-Government"
    cloud="gov"
    documentationCenter=""
    authors="ryansoc"
    manager="zakramer"
    editor=""/>

<tags
    ms.service="multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="azure-government"
    ms.date="10/18/2016"
    ms.author="ryansoc"/>


#  <a name="azure-government-databases"></a>Azure Government Datenbanken

##  <a name="sql-database"></a>SQL-Datenbank

Weitere<a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx"> Microsoft Security Center für SQL-Datenbank-Engine</a> und [Azure SQL Datenbank öffentliche Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/) zusätzliche Anleitung Metadatenkonfiguration und optimalen Schutz.

### <a name="variations"></a>Variationen

SQL V12 Datenbank ist in Azure Government erhältlich.

Die Adresse für SQL Azure-Server in Azure Government unterscheidet:

Diensttyp|Azure Public|Azure Government
---|---|---
SQL-Datenbank|*. database.windows.net|*. database.usgovcloudapi.net

### <a name="considerations"></a>Hinweise

Die folgenden Informationen identifizieren Azure Government Grenze für SQL Azure:

| Reguliert/gesteuert Daten erlaubt | Reguliert/gesteuert Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten gespeichert und verarbeitet Microsoft Azure SQL können Azure Regierung regulierte Daten enthalten. Verwenden Sie für die Datenübertragung von Azure Regierung regulierte Daten Datenbanktools. | Azure SQL-Metadaten darf nicht Exportkontrolle Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten beim Erstellen und verwalten Ihre Speicherprodukt eingegeben.  Reguliert/gesteuert Daten nicht in die folgenden Felder eingeben: Datenbank namens Abonnementname Ressourcengruppen Servername Server administratoranmeldung Bereitstellung Namen Ressourcennamen, Resource-Tags

## <a name="azure-redis-cache"></a>Azure Redis Cache

Ausführliche Informationen über diesen Dienst und dessen Verwendung Dokumentation [Azure Redis Cache public](https://azure.microsoft.com/documentation/services/redis-cache/).

### <a name="variations"></a>Variationen

Die URLs für den Zugriff auf und Verwaltung von Azure Redis Cache in Azure Government unterscheiden:

Diensttyp|Azure Public|Azure Government
---|---|---
Cacheendpunkt|*. redis.cache.windows.net|*. redis.cache.usgovcloudapi.net
Azure-portal|https://Portal.Azure.com|https://Portal.Azure.US

>[AZURE.NOTE] Alle Skripts und Code muss die entsprechende Endpunkte und Umgebung. Weitere Informationen finden Sie unter [Verbindung zu Azure Government Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).


### <a name="considerations"></a>Hinweise

Die folgenden Informationen identifizieren Azure Government Grenze für Azure Redis Cache:

| Reguliert/gesteuert Daten erlaubt | Reguliert/gesteuert Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle Daten in Azure Redis Cache gespeichert und verarbeitet können Azure Regierung regulierte Daten enthalten. | Azure Redis Cache Metadaten darf nicht Exportkontrolle Daten enthalten. Reguliert/gesteuert Daten nicht in die folgenden Felder eingeben: Cache, VNET, Namen, Ressourcengruppen, Ressourcen-Tags, Redis Eigenschaften.  

##  <a name="next-steps"></a>Nächste Schritte

Für zusätzliche Informationen und Updates Abonnieren der <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Regierung Blog.</a>
