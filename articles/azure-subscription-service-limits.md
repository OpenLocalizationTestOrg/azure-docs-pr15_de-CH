<properties
    pageTitle="Microsoft Azure-Abonnement und Service Grenzen, Kontingente und Integritätsregeln"
    description="Enthält eine Liste häufig Azure-Abonnement und Service-Grenzwerte Kontingente und Einschränkungen. Diese enthält Informationen und maximalen Limits zu erhöhen."
    services=""
    documentationCenter=""
    authors="rothja"
    manager="jeffreyg"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="btardif"/>

# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-Abonnement und Service Grenzen, Kontingente und Integritätsregeln

## <a name="overview"></a>Übersicht

Dieses Dokument gibt einige der am häufigsten verwendeten Microsoft Azure Grenzwerte. Dies deckt derzeit nicht alle Azure-Dienste. Im Laufe der Zeit werden diese Grenzwerte erweitert und aktualisiert, um mehr von der Plattform.

Besuchen Sie [Preisübersicht Azure](https://azure.microsoft.com/pricing/) erfahren Sie mehr über Azure-Preisen. Sie können Vorkalkulation der mit dem [Rechner Preise](https://azure.microsoft.com/pricing/calculator/) oder Preise Detailseite für einen Service (z. B. [Windows-VMs](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)).

> [AZURE.NOTE] Ggf. die Grenze über **Standardmäßige Begrenzung**können Sie [Öffnen eine Anfrage kostenlos](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Die Grenzwerte können der **Maximale** Wert in den folgenden Tabellen heraufgestuft werden. Ist keine **Maximale** Spalte, muss die angegebene Ressource nicht veränderbare Grenzen.

## <a name="limits-and-the-azure-resource-manager"></a>Grenzwerte und Azure Resource Manager

Jetzt ist es möglich, mehrere Azure Ressourcen in einer einzelnen Azure-Ressourcengruppe. Verwendung der Ressourcengruppen werden globale wurden Grenzwerte auf regionaler Ebene mit der Azure-Ressourcen-Manager verwaltet. Weitere Informationen zu Ressourcengruppen Azure Übersicht [Azure-Ressourcen-Manager](azure-resource-manager/resource-group-overview.md).

In folgenden Grenzen hat eine neue Tabelle hinzugefügt Unterschiede in Grenzen wiederzugeben, wenn der Azure-Ressourcen-Manager verwenden. Beispielsweise ist ein **Abonnement Grenzen** und **Grenzen der Abonnement - Azure-Ressourcen-Manager** . Wenn ein Limit für beide Szenarien gilt, wird sie nur in der ersten Tabelle angezeigt. Sofern nicht anders angegeben, sind Grenzwerte global in allen Regionen.

> [AZURE.NOTE] Es ist wichtig zu betonen, dass Kontingente für Ressourcen in Azure-Ressourcengruppen pro Region durch Ihr Abonnement verfügbar sind, und nicht pro Abonnement Service Management Kontingente sind. Nehmen wir als Beispiel Core Kontingente. Benötigen Sie ein Kontingent erhöhen mit Unterstützung für Kerne anfordern, müssen Sie wie viele Kerne sollen in die Bereiche verwenden, und stellen eine Anfrage für Ressourcengruppe Azure Core Kontingente für die Beträge und die gewünschten Bereiche. Daher muss 30 Kerne in Westeuropa verwenden, um die Anwendung auszuführen. Sie sollten insbesondere 30 Kerne in Westeuropa anfordern. Aber Sie keinen zentralen Kontingent in keiner anderen Region vergrößern – nur Westeuropa haben 30 Core Kontingent.
<!-- -->
Sie können daher hilfreich sollten entscheiden, was Ihre Azure-Ressourcengruppe Kontingente müssen für Ihre Arbeit in einer Region und diesen Betrag in jeder Region in die Bereitstellung erwägen anfordern. [Problembehandlung bei der Bereitstellung](./resource-manager-common-deployment-errors.md) mehr entdecken die aktuellen Kontingente für bestimmte Regionen anzeigen

## <a name="service-specific-limits"></a>Dienstspezifische Grenzwerte

- [Active Directory](#active-directory-limits)
- [API-Management](#api-management-limits)
- [App Service](#app-service-limits)
- [Application Gateway](#application-gateway-limits)
- [Anwendung Einblicke](#application-insights-limits)
- [Automatisierung](#automation-limits)
- [Azure Redis Cache](#azure-redis-cache-limits)
- [Azure RemoteApp](#azure-remoteapp-limits)
- [Sicherung](#backup-limits)
- [Stapelverarbeitung](#batch-limits)
- [BizTalk-Dienste](#biztalk-services-limits)
- [CDN](#cdn-limits)
- [Cloud-Dienste](#cloud-services-limits)
- [Data Factory](#data-factory-limits)
- [Datenanalyse See](#data-lake-analytics-limits)
- [DNS](#dns-limits)
- [DocumentDB](#documentdb-limits)
- [Ereignis-Hubs](#event-hubs-limits)
- [IoT Hub](#iot-hub-limits)
- [Key Vault](#key-vault-limits)
- [Media Services](#media-services-limits)
- [Mobile Engagement](#mobile-engagement-limits)
- [Mobile Dienste](#mobile-services-limits)
- [Überwachung](#monitoring-limits)
- [Mehrstufige Authentifizierung](#multi-factor-authentication)
- [Netzwerk](#networking-limits)
- [Hub-Benachrichtigungsdienst](#notification-hub-service-limits)
- [Betriebliche Informationen](#operational-insights-limits)
- [Ressourcengruppe](#resource-group-limits)
- [Planer](#scheduler-limits)
- [Suche](#search-limits)
- [Servicebus](#service-bus-limits)
- [Site Recovery](#site-recovery-limits)
- [SQL-Datenbank](#sql-database-limits)
- [Speicher](#storage-limits)
- [StorSimple System](#storsimple-system-limits)
- [Stream Analytics](#stream-analytics-limits)
- [Abonnement](#subscription-limits)
- [Traffic Manager](#traffic-manager-limits)
- [Virtuelle Computer](#virtual-machines-limits)
- [Virtual Machine Maßstab legt](#virtual-machine-scale-sets-limits)


### <a name="subscription-limits"></a>Abonnement-Grenzwerte
#### <a name="subscription-limits"></a>Abonnement-Grenzwerte
[AZURE.INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Abonnement Grenzen - Azure-Ressourcen-Manager

Es gelten folgende Grenzwerte der Azure-Ressourcen-Manager und Azure-Ressourcengruppen. Grenzwerte, die nicht mit der Azure-Ressourcen-Manager geändert wurden nicht sind. Finden Sie in der vorherigen Tabelle Grenzwerte.

Informationen zum Behandeln von Grenzen Ressourcenmanager Anfragen anzeigen Sie [Ressourcenmanager Drosselung Anfragen](resource-manager-request-limits.md)

[AZURE.INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]


### <a name="resource-group-limits"></a>Ressourcengruppe begrenzt

[AZURE.INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Grenzwerte für virtuelle Computer
#### <a name="virtual-machine-limits"></a>Virtual Machine-Grenzwerte
[AZURE.INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]


#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuelle Computer Grenzen - Azure-Ressourcen-Manager

Es gelten folgende Grenzwerte der Azure-Ressourcen-Manager und Azure-Ressourcengruppen. Grenzwerte, die nicht mit der Azure-Ressourcen-Manager geändert wurden nicht sind. Finden Sie in der vorherigen Tabelle Grenzwerte.

[AZURE.INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Virtual Machine Maßstab legt Grenzwerte

[AZURE.INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="networking-limits"></a>Netzwerk-Grenzwerte

[AZURE.INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Netzwerk-Grenzwerte
[AZURE.INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Application Gateway begrenzt

[AZURE.INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="traffic-manager-limits"></a>Traffic Manager beschränkt

[AZURE.INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-Grenzwerte

[AZURE.INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Speichergrenzwerte

Finden Sie weitere Informationen über Konto Speichergrenzwerte [Azure Storage Skalierbarkeits- und Performance-Ziele](../articles/storage/storage-scalability-targets.md).

#### <a name="storage-service-limits"></a>Speichergrenzwerte Service

[AZURE.INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

#### <a name="virtual-machine-disk-limits"></a>Virtual Machine Grenzen

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Weitere Informationen finden Sie in der [Größe der virtuellen Computer](../articles/virtual-machines/virtual-machines-linux-sizes.md) .

**Standardspeicherkonten**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

**Premium-Speicherkonten**

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Speichergrenzwerte Ressourcenanbieter

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]


### <a name="cloud-services-limits"></a>Grenzwerte für Cloud-Services

[AZURE.INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]


### <a name="app-service-limits"></a>App-Dienstes beschränkt
Die folgenden Grenzwerte App Service gehören Grenzwerte für Web Apps, Mobile Apps, API-Apps und Logik Apps.

[AZURE.INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Planer-Grenzwerte

[AZURE.INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch-Grenzwerte

[AZURE.INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

###<a name="biztalk-services-limits"></a>BizTalk-Dienste Grenzen
Die folgende Tabelle gibt die Grenzwerte für Azure Biztalk-Dienste.

[AZURE.INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]


### <a name="documentdb-limits"></a>DocumentDB-Grenzwerte

[AZURE.INCLUDE [azure-documentdb-limits](../includes/azure-documentdb-limits.md)]

Kontingente, hinter der ein Sternchen (*) [bei Azure-Support angepasst werden](./documentdb/documentdb-increase-limits.md).

### <a name="mobile-engagement-limits"></a>Mobile Engagement Grenzen

[AZURE.INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]


### <a name="search-limits"></a>Suche-Grenzwerte

Tarifen bestimmen die Kapazität und Grenzen der Suchdienst. Tier umfassen:

- *Frei* Multi-Tenant-Dienst gemeinsam mit anderen Abonnenten Azure vorgesehenen Bewertung und kleinen Projekten.
- *Grundlegende* bietet dedizierte Ressourcen für Produktionsarbeitslasten in kleinerem Maßstab für hohe Verfügbarkeit Abfrage Arbeitslasten mit bis zu drei.
- *Standard (S1, S2, S3, S3 HD)* ist für größere Produktions-Workloads. Mehrere Ebenen sind in der standard-Stufe, damit Ressourcenkonfiguration für bestimmte Szenarien können.

**Grenzwerte pro Abonnement**

[AZURE.INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Die einzelnen Suchdienst**

[AZURE.INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

Detailliertere Informationen über andere, einschließlich Dokumentgröße Abfragen pro Sekunde, Schlüssel, Anfragen und Antworten finden Sie unter [Service Grenzwerte in Azure suchen](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services Grenzwerte

[AZURE.INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-Grenzwerte

[AZURE.INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Mobile Dienste Grenzen

[AZURE.INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitoring-limits"></a>Überwachung der Grenzen

[AZURE.INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Hub-Benachrichtigungsdienst beschränkt

[AZURE.INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Ereignis-Hubs Grenzwerte

[AZURE.INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus-Grenzwerte

[AZURE.INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT Hub begrenzt

[AZURE.INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Data Factory beschränkt

[AZURE.INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Datenanalyse See begrenzt
[AZURE.INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics Grenzwerte
[AZURE.INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory begrenzt

[AZURE.INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]


### <a name="azure-remoteapp-limits"></a>Azure RemoteApp-Grenzwerte

[AZURE.INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple Systemgrenzen

[AZURE.INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]


### <a name="operational-insights-limits"></a>Einblicke Einschränkungen

[AZURE.INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Backup-Grenzwerte

[AZURE.INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Site Recovery beschränkt

[AZURE.INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Anwendung Einblicke Grenzwerte

[AZURE.INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management Grenzen

[AZURE.INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis Cache-Grenzwerte

[AZURE.INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Key Vault-Grenzwerte

[AZURE.INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Mehrstufige Authentifizierung
[AZURE.INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatisierung Grenzwerte
[AZURE.INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL Datenbank beschränkt

SQL Datenbank Grenzen finden Sie unter [SQL Datenbank Ressourcengrenzen](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Siehe auch

[Grundlegendes zu Azure Grenzen und erhöht](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtueller Computer und Azure Cloud Service Größe](virtual-machines/virtual-machines-linux-sizes.md)

[Größen für Cloud-Services](cloud-services/cloud-services-sizes-specs.md)
