<properties
    pageTitle="Vorgehensweise bei einer Azure service-Unterbrechung, der Azure Cloud Services betrifft | Microsoft Azure"
    description="Vorgehensweise, bei Azure Service-Ausfällen, die Azure Cloud Services auswirkt."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Was bei einer Azure service-Unterbrechung, die Azure Cloud Services auswirkt

Bei Microsoft arbeiten wir hart um sicherzustellen, dass unsere Dienste immer verfügbar sind, bei Bedarf. Kräfte Gewalt beeinflussen uns auch entsprechend der ungeplanten Ausfällen führen.

Microsoft stellt ein Service Level Agreement (SLA) für seine Dienste als Verpflichtung für Verfügbarkeit und Konnektivität. Die SLA für einzelne Azure Services finden in [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure bietet bereits viele integrierte Plattform, die hohe Verfügbarkeit unterstützen. Lesen Sie für Weitere Informationen zu [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dieser Artikel behandelt true Systemausfall, wenn eine ganze Region ein Ausfall Naturkatastrophe oder verbreitet Unterbrechung auftritt. Diese sind selten, jedoch müssen die Möglichkeit, dass ein einer ganzen Region Ausfall. Eine ganze Region eine serviceunterbrechung auftritt, wäre lokal redundanten Datenkopien vorübergehend nicht verfügbar. Geo-Replikation aktiviert haben, werden drei zusätzliche Kopien der Azure-Speicher Blobs und Tabellen in einer anderen Region gespeichert. Bei vollständigen regionalen Ausfall oder einer Katastrophe in die primäre Region nicht behoben werden kann, ordnet Azure alle DNS-Einträge in den Bereich Geo repliziert.

>[AZURE.NOTE]Achten Sie haben keine Kontrolle über diesen Prozess und tritt nur für Datacenter-Wide-Ausfällen. Aus diesem Grund müssen Sie auf andere anwendungsspezifische Sicherungsstrategien höchste Verfügbarkeit zu verlassen. Weitere Informationen finden Sie im Abschnitt zur [Daten-Strategien für die Wiederherstellung](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Möchten Sie eigene Failover beeinflussen, empfiehlt es sich, die Verwendung von [Lesezugriff Geo redundanten Speicher (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage)sollten schafft eine schreibgeschützte Kopie der Daten in einer anderen Region.

Als Hilfe bei der Behandlung dieser seltenen bieten wir die folgende Anleitung für Azure virtuelle Maschinen (VMs) bei einer Service-Ausfällen den gesamten Bereich, wo die Azure-VM-Anwendung bereitgestellt wird.

##<a name="option-1-wait-for-recovery"></a>Option 1: Warten auf Wiederherstellung
In diesem Fall ist keine Aktion Ihrerseits erforderlich. Kennen Sie Azure Teams sorgfältig arbeiten, um die Verfügbarkeit des Dienstes wiederherzustellen. Sie sehen den aktuellen Servicestatus unsere [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Dies ist am besten Kunden von Azure Site Recovery wurde nicht festgelegt oder sekundären Bereich in einen anderen Bereich.

Für Kunden, die sofortigen Zugriff auf ihre bereitgestellten Clouddienste, stehen die folgenden Optionen.

>[AZURE.NOTE]Beachten Sie, dass diese Optionen die Möglichkeit von Datenverlusten.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Option 2: Bereitstellen Sie Konfiguration des Cloud-Dienst eine neue Region erneut

Haben Sie den ursprünglichen Code können Sie einfach die Anwendung zugeordnete Konfiguration und zugeordneten Ressourcen einen neuen Cloud-Dienst in einer neuen Region erneut bereitstellen.  

Weitere Informationen zum Erstellen und Bereitstellen einer Cloudanwendung-Dienst finden Sie unter [Erstellen und Bereitstellen eines Cloud-Diensts](./cloud-services-how-to-create-deploy-portal.md).

Je nach Anwendung Datenquellen müssen Sie die Wiederherstellungsverfahren für die Datenquelle der Anwendung überprüfen.
  * Azure Storage-Datenquellen finden Sie unter [Azure Storage Replication](../storage/storage-redundancy.md#read-access-geo-redundant-storage) , überprüfen Sie die verfügbaren Optionen nach Replikationsmodell wählen für Ihre Anwendung.
  * Für SQL-Datenquellen lesen [Übersicht: Cloud Business Continuity und Datenbank-Wiederherstellung mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md) Überprüfung verfügbaren Optionen basierend auf der ausgewählten Replikationsmodell für Ihre Anwendung.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Option 3: Verwenden einer backup-Bereitstellung über Azure Traffic Manager
Diese Option setzt voraus, dass Ihre Anwendungslösung mit regionaler Notfall dabei bereits entworfen haben. Diese Option können eine sekundäre Cloud Services anwendungsbereitstellung bereits in einem anderen Bereich mit, der über einen Kanal Traffic Manager verbunden. In diesem Fall Überprüfen der sekundären Bereitstellung. Wenn er fehlerfrei ist, können Sie Datenverkehr zu über Azure Traffic Manager umleiten. Mit dieser Strategie können Sie Datenverkehr Verteilungsmethode und Reihenfolge Failoverkonfigurationen in Azure Traffic Manager nutzen. Weitere Informationen finden Sie unter [Traffic Manager zu konfigurieren](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Netzwerklastenausgleich Azure Cloud Services in Regionen mit Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Implementierung einer Disaster Recovery und Strategie für hohe Verfügbarkeit finden Sie unter [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Entwickeln Sie ein detailliertes technisches Verständnis der eine Cloud-Funktionen finden Sie unter [Azure Resiliency technische Anleitung](../resiliency/resiliency-technical-guidance.md).

Wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)der Anleitung sind nicht klar, ob Microsoft Vorgänge in Ihrem Auftrag durchführen möchten.
