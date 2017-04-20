<properties
    pageTitle="Was tun, wenn eine Azure Service-Unterbrechung Azure virtuelle Computer auswirkt | Microsoft Azure"
    description="Vorgehensweise, Azure Service-Ausfällen Azure virtuelle Computer auswirkt."
    services="virtual-machines"
    documentationCenter=""
    authors="kmouss"
    manager="timlt"
    editor=""/>

<tags
    ms.service="virtual-machines"
    ms.workload="virtual-machines"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-virtual-machines"></a>Was tun, wenn eine Azure Service-Unterbrechung Azure virtuelle Computer auswirkt

Bei Microsoft arbeiten wir hart um sicherzustellen, dass unsere Dienste immer verfügbar sind, bei Bedarf. Kräfte Gewalt beeinflussen uns auch entsprechend der ungeplanten Ausfällen führen.

Microsoft stellt ein Service Level Agreement (SLA) für seine Dienste als Verpflichtung für Verfügbarkeit und Konnektivität. Die SLA für einzelne Azure Services finden in [Azure Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

Azure bietet bereits viele integrierte Plattform, die hohe Verfügbarkeit unterstützen. Lesen Sie für Weitere Informationen zu [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dieser Artikel behandelt true Systemausfall, wenn eine ganze Region ein Ausfall Naturkatastrophe oder verbreitet Unterbrechung auftritt. Diese sind selten, jedoch müssen die Möglichkeit, dass ein einer ganzen Region Ausfall. Eine ganze Region eine serviceunterbrechung auftritt, wäre lokal redundanten Datenkopien vorübergehend nicht verfügbar. Geo-Replikation aktiviert haben, werden drei zusätzliche Kopien der Azure-Speicher Blobs und Tabellen in einer anderen Region gespeichert. Bei vollständigen regionalen Ausfall oder einer Katastrophe in die primäre Region nicht behoben werden kann, ordnet Azure alle DNS-Einträge in den Bereich Geo repliziert.

>[AZURE.NOTE]Beachten Sie, dass Ihnen keine Kontrolle über diesen Prozess und nur bei Ausfällen flächendeckende tritt. Aus diesem Grund müssen Sie auf andere anwendungsspezifische Sicherungsstrategien höchste Verfügbarkeit zu verlassen. Weitere Informationen finden Sie im Abschnitt [Daten](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery)Strategien für die Wiederherstellung.

Als Hilfe bei der Behandlung dieser seltenen bieten wir die folgende Anleitung für Azure virtuelle Computer bei einer Service-Ausfällen den gesamten Bereich, wo die Azure Virtual Machine-Anwendung bereitgestellt wird.

##<a name="option-1-wait-for-recovery"></a>Option 1: Warten auf Wiederherstellung
In diesem Fall ist keine Aktion Ihrerseits erforderlich. Wissen Sie, dass wir zum Betrieb wiederherstellen fleißig werden. Sie sehen den aktuellen Servicestatus unsere [Azure Service Health Dashboard](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Dies ist die beste Option nicht Azure Site Recovery, Backups virtueller Maschinen, Lesezugriff Geo redundanten Speicher oder Geo-redundanten Speicher vor der Unterbrechung eingerichtet. Sie eingerichtet Geo redundanten Speicher oder Lesezugriff Geo redundanten Speicher für das Speicherkonto, VM virtuellen Festplatten (VHDs) gespeichert, können Sie suchen das Basisabbild VHD wiederherstellen und Bereitstellung eine neue VM aus. Dies ist keine bevorzugte Option, da es gibt keine Garantie der Synchronisation von Daten, wenn Azure VM Backup oder Azure Site Recovery verwendet werden. Daher ist diese Option nicht unbedingt funktionieren.

Für Kunden, die sofortigen Zugriff auf virtuelle Maschinen, stehen zwei Optionen zur Verfügung.  

>[AZURE.NOTE]Beachten Sie, dass beide der folgenden Optionen die Möglichkeit von Datenverlusten.     

##<a name="option-2-restore-a-vm-from-a-backup"></a>Option 2: Wiederherstellen Sie einen virtueller Computer aus einer Sicherung
Kunden, die eine VM-Sicherung konfiguriert haben, können Sie die VM aus der Backup- und Recovery-Point wiederherstellen.

Um einen neuen virtuellen Computer von Azure Sicherung wiederherzustellen, finden Sie unter [virtuelle Computer in Azure wiederherstellen](../backup/backup-azure-restore-vms.md).

Planen für die backup-Infrastruktur Azure virtuelle Computer finden Sie im [Plan die VM-backup-Infrastruktur in Azure](../backup/backup-azure-vms-introduction.md).

##<a name="option-3-initiate-a-failover-by-using-azure-site-recovery"></a>Option 3: Ein Failover initiiert mit Azure Site Recovery
Wenn Sie Azure Site Recovery arbeiten die betroffenen Azure virtuellen Computer konfiguriert haben, können Sie die VMs aus ihrer Replikate wiederherstellen. Diese Replikationen können Azure oder lokal befinden. In diesem Fall können Sie einen neuen virtuellen Computer aus vorhandenen Replikat erstellen. Zum Wiederherstellen Ihrer virtuellen Computer aus ein Replikat Azure Site Recovery anzeigen Sie [Azure IaaS Migration virtueller Maschinen zwischen Azure Regionen mit Azure Site Recovery](../site-recovery/site-recovery-migrate-azure-to-azure.md)

>[AZURE.NOTE]Obwohl Azure virtuellen Betriebssystem und Datenträger auf einen sekundären virtuellen Festplatte replizierter sind in einem Geo-redundanten Speicher oder Lesezugriff Geo redundante Speicherkonto wird jeder VHD unabhängig repliziert. Diese Replikation nicht Konsistenz für replizierte VHDs gewährleisten. Haben Ihre Anwendung oder Datenbanken, die diesen Datenträger verwenden Abhängigkeiten auf, ist nicht garantiert, dass alle Festplatten als einen Snapshot repliziert werden. Es ist auch nicht garantiert, VHD Replikat Geo redundanten Speicher oder Lesezugriff Geo redundanten Speicher einer Anwendung konsistenten Snapshot die VM starten führt.

##<a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Implementierung einer Disaster Recovery und Strategie für hohe Verfügbarkeit finden Sie unter [Disaster Recovery und hohe Verfügbarkeit für Azure Applications](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Entwickeln Sie ein detailliertes technisches Verständnis der eine Cloud-Funktionen finden Sie unter [Azure Resiliency technische Anleitung](../resiliency/resiliency-technical-guidance.md).

Sichern von virtuellen Computern finden Sie unter [Sichern von Azure virtuelle Computer](../backup/backup-azure-vms.md).

Informationen Sie zum Verwenden von Azure Site Recovery zu orchestrieren Schutz physischen (und virtuellen) Windows- und Linux-Computer, die auf VMWare und Hyper-V virtuelle Computer ausführen, finden Sie unter [Azure Site Recovery](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)zu automatisieren.

Wenn die Anweisung nicht deaktivieren oder Microsoft Vorgänge in Ihrem Auftrag durchführen möchten, wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
