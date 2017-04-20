<properties
    pageTitle="Verwalten von Azure Redis Cache Azure PowerShell | Microsoft Azure"
    description="Erfahren Sie, wie Verwaltungsaufgaben mithilfe von Azure PowerShell Azure Redis Cache."
    services="redis-cache"
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Verwalten von Azure Redis Cache Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](cache-howto-manage-redis-cache-powershell.md)
- [Azure CLI](cache-manage-cli.md)

Dieses Thema zeigt Ihnen wie auszuführenden Aufgaben wie erstellen, aktualisieren und Skalierung Azure Redis Cache Instanzen, wie Zugriffstasten Regenerieren und Informationen über Ihre Caches anzeigen. Eine vollständige Liste der Azure Redis Cache PowerShell-Cmdlets finden Sie unter [Azure Redis Cache-Cmdlets](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)][Klassisch](../resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Erforderliche Komponenten

Wenn Sie Azure PowerShell installiert haben, müssen Sie Azure PowerShell Version 1.0.0 oder höher. Die Version von Azure PowerShell finden, die Sie mit diesem Befehl an der Befehlszeile Azure PowerShell installiert haben.

    Get-Module azure | format-table version

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

Zunächst müssen Sie in Azure mit diesem Befehl anmelden.

    Login-AzureRmAccount

Geben Sie die e-Mail-Adresse der Azure-Konto und das Kennwort in das Microsoft Azure-Dialogfeld.

Als Nächstes müssen Sie haben mehrere Azure-Abonnements Azure Abonnement festgelegt. Führen Sie diesen Befehl, um eine Liste Ihrer aktuellen Abonnements anzuzeigen.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Führen Sie den folgenden Befehl, um die Subskription anzugeben. Im folgenden Beispiel wird der Namen `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Bevor Sie Windows PowerShell mit Azure-Ressourcen-Manager verwenden können, benötigen Sie Folgendes:

- Windows PowerShell 3.0 oder 4.0. Geben Sie zum Ermitteln der Version von Windows PowerShell:`$PSVersionTable` und überprüfen Sie den Wert `PSVersion` 3.0 oder 4.0 ist. Eine kompatible Version finden Sie unter [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) oder [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Erhalten Sie detaillierte Hilfe zu jeder Cmdlet sehen Sie in diesem Lernprogramm verwenden Sie das Cmdlet "Get-Help".

    Get-Help <cmdlet-name> -Detailed

Beispiel für Hilfe für die `New-AzureRmRedisCache` Cmdlet Typ:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-azure-government-cloud-or-azure-china-cloud"></a>Verbindung zu Azure Government Cloud oder Azure China Cloud

Standardmäßig der Azure-Umgebung ist `AzureCloud`, die globale Azure-Cloud-Instanz darstellt. Verbindung zu einer anderen Instanz verwenden die `Add-AzureRmAccount` Befehl mit der `-Environment` -`EnvironmentName` Befehlszeilenoption und den gewünschten Umgebungsnamen.

Um die Liste der verfügbaren Umgebungen anzuzeigen, führen die `Get-AzureRmEnvironment` Cmdlet.

### <a name="to-connect-to-the-azure-government-cloud"></a>Verbindung zu Azure Government Cloud

Verbindung zu Azure Government Cloud verwenden Sie einen der folgenden Befehle.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

oder

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Um einen Cache in Azure Government Cloud zu erstellen, verwenden Sie einen der folgenden Speicherorte.

-   USGov Virginia
-   USGov Iowa

Weitere Informationen zu Azure Government Cloud finden Sie unter [Microsoft Azure Regierung](https://azure.microsoft.com/features/gov/) und [Microsoft Azure Regierung Developer Guide](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Verbindung zu Azure China Cloud

Um in Azure China Cloud zu verbinden, verwenden Sie einen der folgenden Befehle.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

oder

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Um einen Cache in Azure China Cloud zu erstellen, verwenden Sie einen der folgenden Speicherorte.

-   China-OST
-   China North

Weitere Informationen zu Azure China Cloud finden Sie unter [AzureChinaCloud für Azure von 21Vianet in China betrieben wird](http://www.windowsazure.cn/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Eigenschaften für Azure Redis Cache PowerShell

Die folgende Tabelle enthält die Eigenschaften und eine Beschreibung der häufig verwendeten Parameter beim Erstellen und Verwalten von Azure Redis Cache Instanzen Azure PowerShell.

| Parameter          | Beschreibung                                                                                                                                                                                                        | Standard  |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Name               | Name des Caches                                                                                                                                                                                                  |          |
| Speicherort           | Speicherort des Caches                                                                                                                                                                                              |          |
| ResourceGroupName  | Gruppe Ressourcenname, Cache erstellen                                                                                                                                                                   |          |
| Größe               | Die Größe des Caches. Gültige Werte sind: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB                                                                     | 1GB      |
| ShardCount         | Die Anzahl der Splitter Premium-Cache erstellen mit Clusterunterstützung. Gültige Werte sind: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10                                                                                                      |          |
| SKU                | Gibt die SKU des Caches. Gültige Werte sind: Basic, Standard, Premium                                                                                                                                         | Standard |
| RedisConfiguration | Gibt Redis-Konfigurationen. Details zu jeder Einstellung finden Sie in der Eigenschaftentabelle von [RedisConfiguration](#redisconfiguration-properties) . |          |
| EnableNonSslPort   | Gibt an, ob nicht-SSL-Port aktiviert ist.                                                                                                                                                                     | False    |
| MaxMemoryPolicy    | Dieser Parameter ist veraltet - RedisConfiguration verwenden.                                                                                                                                              |          |
| StaticIP           | Wenn den Cache in einem VNET, gibt eine eindeutige IP-Adresse im Subnetz für den Cache. Wenn nicht angegeben, wird eine aus dem Subnetz für Sie ausgewählt.                                                                                                                     |          |
| Subnetz             | Wenn den Cache in einem VNET, gibt den Namen des Subnetzes, Cache bereitstellen.                                                                                                                  |          |
| VirtualNetwork     | Wenn den Cache in einem VNET, gibt die Ressourcen-ID des VNET, den Cache bereitstellen.                                                                                                             |          |
| Schlüsseltyp            | Gibt die Zugriffstaste zu generieren, wenn Sie Zugriffstasten zu erneuern. Gültige Werte sind: primäre, sekundäre |  |                                                                                                                                                                                                              |          |


### <a name="redisconfiguration-properties"></a>RedisConfiguration Eigenschaften

| Eigenschaft                      | Beschreibung                                                                                                          | Tarifen       |
|-------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------|
| der RDB-Sicherung aktiviert            | Ob die [Dauerhaftigkeit von Benutzerdaten Redis](cache-how-to-premium-persistence.md) aktiviert ist                                     | Nur Premium        |
| der RDB-Speicher-Verbindungszeichenfolge | Die Verbindungszeichenfolge für das Speicherkonto für [Dauerhaftigkeit von Daten Redis](cache-how-to-premium-persistence.md)       | Nur Premium        |
| der RDB-Backup-Häufigkeit          | Backup Häufigkeit für [Dauerhaftigkeit von Daten Redis](cache-how-to-premium-persistence.md)                               | Nur Premium        |
| Maxmemory reserviert            | Konfiguriert den [reservierten Speicher](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) für Cache-Prozesse | Standard- und Premium |
| Maxmemory-Richtlinie              | Konfigurieren für den Cache der [Entfernungsrichtlinie](cache-configure.md#maxmemory-policy-and-maxmemory-reserved)           | Alle Tarifen   |
| Benachrichtigen erledigten-Ereignisse        | Konfiguriert die [erledigten Anträge](cache-configure.md#keyspace-notifications-advanced-settings)                     | Standard- und Premium |
| Hash-Max-Ziplist-Einträge      | Konfiguriert die [Optimierung des Speicher](http://redis.io/topics/memory-optimization) für kleine aggregate von Datentypen          | Standard- und Premium |
| Hash-Ziplist-Maximalwert        | Konfiguriert die [Optimierung des Speicher](http://redis.io/topics/memory-optimization) für kleine aggregate von Datentypen          | Standard- und Premium |
| Set-Max-Intset-Einträge        | Konfiguriert die [Optimierung des Speicher](http://redis.io/topics/memory-optimization) für kleine aggregate von Datentypen          | Standard- und Premium |
| Zset-Max-Ziplist-Einträge      | Konfiguriert die [Optimierung des Speicher](http://redis.io/topics/memory-optimization) für kleine aggregate von Datentypen          | Standard- und Premium |
| Zset-Ziplist-Maximalwert        | Konfiguriert die [Optimierung des Speicher](http://redis.io/topics/memory-optimization) für kleine aggregate von Datentypen          | Standard- und Premium |
| Datenbanken                     | Die Anzahl der Datenbanken konfiguriert. Diese Eigenschaft kann nur zur Erstellung des Caches konfiguriert werden.                          | Standard- und Premium |

## <a name="to-create-a-redis-cache"></a>Redis Cache erstellen

Neue Azure Redis Cache-Instanzen werden mithilfe des [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) -Cmdlet erstellt.

>[AZURE.IMPORTANT] Beim ersten erstellen einen Redis Cache in einem Abonnement mit Azure-Portal im Portal registriert die `Microsoft.Cache` Namespace für dieses Abonnement. Beim ersten Redis Cache in einem Abonnement mit PowerShell erstellen, müssen Sie diesen Namespace mit dem folgenden Befehl registrieren; andernfalls Cmdlets wie `New-AzureRmRedisCache` und `Get-AzureRmRedisCache` fehl.
>
>`Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `New-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed
    
    NAME
        New-AzureRmRedisCache
    
    SYNOPSIS
        Creates a new redis cache.
    
    
    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]
    
    
    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.
    
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to create.
    
        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.
    
        -Location <String>
            Location in which to create the redis cache.
    
        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}
    
        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Führen Sie den folgenden Befehl, um einen Cache mit erstellen.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, und `Location` erforderlichen Parameter sind jedoch die übrigen sind optional und haben Standardwerte. Mit dem vorherigen Befehl erstellt eine Standard SKU Azure Redis Cache-Instanz mit dem angegebenen Namen, Speicherort und Ressourcengruppe 1 GB mit nicht-SSL-Anschluss deaktiviert ist.

Einen Premium-Cache erstellen, geben Sie eine Größe von P1 (6 GB - 60 GB), P2 (13 GB - 130 GB) (26 GB - 260 GB), P3 und P4 (53 bis 530 GB). Zum Aktivieren des clustering geben Splitter Anzahl mit dem `ShardCount` Parameter. Im folgende Beispiel wird ein Cache P1 Premium mit 3 erstellt. Ein P1 Premium-Cache ist 6 GB Größe, und da wir drei Splitter angegeben beträgt 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Werte für die `RedisConfiguration` Parameter, setzen die Werte in `{}` Schlüssel-Wert-Paaren wie `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Das folgende Beispiel erstellt einen standard 1 GB Cache mit `allkeys-random` Maxmemory-Richtlinie und erledigten Benachrichtigungen konfiguriert `KEA`. Weitere Informationen finden Sie unter [erledigten Anträge (Erweitert)](cache-configure.md#keyspace-notifications-advanced-settings) und [Maxmemory-Policy und Maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>
## <a name="to-configure-the-databases-setting-during-cache-creation"></a>So konfigurieren Sie die Datenbanken während der Erstellung des Caches festlegen

Die `databases` Einstellung kann nur während der Erstellung des Caches. Das folgende Beispiel erstellt eine Prämie P3 (26 GB) Cache mit 48 Datenbanken [Neu AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) -Cmdlet.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Weitere Informationen zu den `databases` -Eigenschaft finden Sie unter [Default Azure Redis Cache-Serverkonfiguration](cache-configure.md#default-redis-server-configuration). Weitere Informationen zum Erstellen eines Caches mit dem [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) -Cmdlet finden Sie in vorherigen Abschnitt [Redis Cache erstellen](#to-create-a-redis-cache) .

## <a name="to-update-a-redis-cache"></a>Redis Cache aktualisieren

Azure Redis Cache-Instanzen werden mithilfe des [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) -Cmdlet.

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Set-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed
    
    NAME
        Set-AzureRmRedisCache
    
    SYNOPSIS
        Set redis cache updatable parameters.
    
    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]
    
    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to update.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.
    
        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.
    
        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}
    
        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.
    
        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.
    
        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Die `Set-AzureRmRedisCache` Cmdlet wird aktualisieren Eigenschaften wie `Size`, `Sku`, `EnableNonSslPort`, und `RedisConfiguration` Werte. 

Der folgende Befehl aktualisiert Maxmemory-Richtlinie für die Redis mit dem Namen MyCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>
## <a name="to-scale-a-redis-cache"></a>Redis Cache skalieren

`Set-AzureRmRedisCache`wird einen Azure Redis Cache skaliert Instanz, wenn die `Size`, `Sku`, oder `ShardCount` geändert. 

>[AZURE.NOTE]Skalierung einen Cache mit PowerShell unterliegt den gleichen Grenzen Richtlinien Skalierung einen Cache von Azure-Portal. Sie können einen anderen Tarif mit folgender Einschränkung skalieren.
>
>-  Sie können nicht an eine niedrigere Preise von höheren Tarif skalieren.
>    -    Ein **Premium** -Cache auf **Standard** oder **einfache** Cache kann nicht skaliert werden.
>    -    Von **Standard** -Cache auf **einfache** Cache kann nicht skaliert werden.
>-  Sie können aus einem **einfachen** Cache ein **Standard** -Cache skalieren jedoch können Sie gleichzeitig die Größe ändern. Wenn Sie eine andere Größe benötigen, möglich nachfolgende Skalierungsoperation auf die gewünschte Größe.
>-  Von einer **grundlegenden** Cache kann nicht direkt auf ein **Premium** -Cache skaliert werden. Sie müssen **grundlegende** **Standard** auf einmal Skalierung und von **Standard** auf **Premium** in nachfolgenden Skalierungsvorgang skalieren.
>-  Skalieren kann nicht von einer großen auf der **C0 (250 MB)** Größe.
>
>Weitere Informationen finden Sie unter [Skalierung Azure Redis Cache](cache-how-to-scale.md).

Im folgenden Beispiel wird veranschaulicht, wie einen Cache mit dem Namen `myCache` 2,5 GB Cache. Beachten Sie, dass dieser Befehl ein oder ein Standard-Cache.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Nachdem dieser Befehl ausgegeben wird, wird der Status des Caches zurückgegeben (ähnlich wie das Aufrufen `Get-AzureRmRedisCache`). Beachten Sie, dass die `ProvisioningState` ist `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB
    
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Nach Abschluss der Skalierung der `ProvisioningState` ändert sich in `Succeeded`. Wenn Sie eine nachfolgende Skalierungsoperation von grundlegenden Standard ändern und Ändern der Größe müssen müssen Sie warten, bis der vorherige abgeschlossen ist oder eine ähnlich der folgenden Fehlermeldung.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Informationen zu Redis-cache

Sie können Informationen über einen Cache mit dem Cmdlet [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) abrufen.

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Get-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed
    
    NAME
        Get-AzureRmRedisCache
    
    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.
    
    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.
    
        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.
    
        If no parameters are given than it will return details about all caches the current subscription.
    
    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Um Informationen über alle Caches in das aktuelle Abonnement zurückzukehren, führen `Get-AzureRmRedisCache` ohne Parameter.

    Get-AzureRmRedisCache

Um Informationen über alle Caches in einer Ressourcengruppe zurückzukehren, führen `Get-AzureRmRedisCache` mit dem `ResourceGroupName` Parameter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Zur Rückgabe von Informationen über einen bestimmten Cache ausgeführt `Get-AzureRmRedisCache` mit der `Name` Parameter mit dem Namen des Caches, und die `ResourceGroupName` Parameter die Ressourcengruppe mit Cache.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Die Zugriffstasten Redis Cache abrufen

Rufen Sie die Zugriffstasten für den Cache können Sie das Cmdlet " [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) ".

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Get-AzureRmRedisCacheKey`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed
    
    NAME
        Get-AzureRmRedisCacheKey
    
    SYNOPSIS
        Gets the accesskeys for the specified redis cache.
    
    
    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]
    
    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Rufen Sie zum Abrufen der Schlüssel für den Cache der `Get-AzureRmRedisCacheKey` Cmdlet und übergeben Sie den Namen der Ressource, die der Cache enthält der Cache.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Zugriffstasten für den Redis-Cache neu erstellen

Um Zugriffstasten für den Cache zu regenerieren, können Sie das [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) -Cmdlet.

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `New-AzureRmRedisCacheKey`, den folgenden Befehl ausführen.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed
    
    NAME
        New-AzureRmRedisCacheKey
    
    SYNOPSIS
        Regenerates the access key of a redis cache.
    
    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]
    
    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache.
    
        -ResourceGroupName <String>
            Name of the resource group for the cache.
    
        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.
    
        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    
Um den primären oder sekundären Schlüssel für den Cache neu zu generieren, rufen die `New-AzureRmRedisCacheKey` Cmdlet und übergeben Sie den Namen der Ressourcengruppe, und geben Sie entweder `Primary` oder `Secondary` für die `KeyType` Parameter. Im folgenden Beispiel wird die sekundäre Zugriffstaste für einen Cache neu erstellt.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary
    
    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y
    
    
    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Redis-Cache löschen

Um Redis Cache zu löschen, verwenden Sie das Cmdlet " [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) ".

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Remove-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed
    
    NAME
        Remove-AzureRmRedisCache
    
    SYNOPSIS
        Remove redis cache if exists.
    
    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>
    
    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.
    
    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.
    
        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.
    
        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.
    
        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Im folgenden Beispiel der Cache mit dem Namen `myCache` entfernt.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup
    
    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Redis Cache importieren

Sie können Daten in einer Azure Redis Cache-Instanz mit der `Import-AzureRmRedisCache` Cmdlet.

>[AZURE.IMPORTANT] Import/Export steht nur für [Premium-Tier-](cache-premium-tier-intro.md) Caches. Weitere Informationen zum Importieren/Exportieren finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Import-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed
    
    NAME
        Import-AzureRmRedisCache
    
    SYNOPSIS
        Import data from blobs to Azure Redis Cache.
    
    
    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.
    
        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Der folgende Befehl importiert Daten aus dem Blob in Azure Redis Cache SAS-URI angegeben.


    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Redis Cache exportieren

Exportieren von Daten aus einer Azure Redis Cache-Instanz mit der `Export-AzureRmRedisCache` Cmdlet.

>[AZURE.IMPORTANT] Import/Export steht nur für [Premium-Tier-](cache-premium-tier-intro.md) Caches. Weitere Informationen zum Importieren/Exportieren finden Sie unter [Importieren und Exportieren von Daten in Azure Redis Cache](cache-how-to-import-export-data.md).

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Export-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed
    
    NAME
        Export-AzureRmRedisCache
    
    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.
    
    
    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -Prefix <String>
            Prefix to use for blob names.
    
        -Container <String>
            SAS url of container where data should be exported.
    
        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.
    
        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Der folgende Befehl exportiert Daten aus einer Azure Redis Cache-Instanz in der SAS-URI angegebene Container.


        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Redis-Cache neu starten

Neustart kann den Azure Redis Cache-Instanz mit der `Reset-AzureRmRedisCache` Cmdlet.

>[AZURE.IMPORTANT] Neustart ist nur für [Premium-Tier-](cache-premium-tier-intro.md) Caches. Weitere Informationen über Neustart Ihren Cache finden Sie unter [Cache-Verwaltung - Neustart](cache-administration.md#reboot).

Um eine Liste der verfügbaren Parameter und deren Beschreibung finden Sie unter `Reset-AzureRmRedisCache`, den folgenden Befehl ausführen.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed
    
    NAME
        Reset-AzureRmRedisCache
    
    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.
    
    
    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]
    
    
    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.
    
    
    PARAMETERS
        -Name <String>
            The name of the cache.
    
        -ResourceGroupName <String>
            The name of the resource group that contains the cache.
    
        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".
    
        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.
    
        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.
    
        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.
    
        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).
    

Der folgende Befehl startet beide Knoten des angegebenen Caches.

    
        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force
    

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung von Windows PowerShell mit Azure finden Sie unter den folgenden Ressourcen:

- [Azure Redis Cache-Cmdlet-Dokumentation auf MSDN](https://msdn.microsoft.com/library/azure/mt634513.aspx)
- [Azure-Ressourcen-Manager-Cmdlets](http://go.microsoft.com/fwlink/?LinkID=394765): Cmdlets im Modul AzureResourceManager Verwendung.
- [Mithilfe von Ressourcengruppen Azure Ressourcen verwalten](../resource-group-template-deploy-portal.md): Informationen zum Erstellen und Verwalten von Ressourcengruppen in Azure-Portal.
- [Azure-Blog](http://blogs.msdn.com/windowsazure): Informationen zu neuen Funktionen in Azure.
- [Windows PowerShell-Blog](http://blogs.msdn.com/powershell): Informationen zu neuen Funktionen in Windows PowerShell.
- ["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): reale Tipps und Tricks von Windows PowerShell-Community erhalten.
