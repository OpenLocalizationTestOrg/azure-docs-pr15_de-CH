
<properties
    pageTitle="Azure Site Recovery: Replizieren Hyper-V virtuelle Computer auf einem einzigen VMM-Server | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie Hyper-V virtuelle Computer replizieren, wenn Sie nur einen einzelnen VMM-Server."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Replizieren Sie Hyper-V virtuelle Computer auf einem einzigen VMM-server

Dieser Artikel Informationen zum Replizieren von Hyper-V virtuelle Computer auf einem Server Hyper-V-Host in VMM-Cloud gespeichert, wenn Sie nur einen einzelnen VMM-Server in der Bereitstellung.

Azure hat zwei verschiedene [Bereitstellungsmodelle](../resource-manager-deployment-model.md) für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Dieser Artikel enthält eine Anleitung zum Einrichten der Replikation in Azure-Portal.


Fragen nach dem Lesen dieses Artikels, stellen Sie sie in den Kommentaren Disqus am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Übersicht

Wenn Sie Hyper-V-VMs am Hyper-V-Hosts in VMM replizieren möchten und Sie nur einen VMM-Server, können Sie [in Azure replizieren](site-recovery-vmm-to-azure.md), oder zwischen auf dem VMM-Server.

Wir empfehlen Sie Azure replizieren, da Failover und Recovery nicht nahtlos beim Replizieren zwischen Wolken und einige manuelle Schritte erforderlich. Wenn Sie mit dem VMM-Server replizieren möchten, können Sie Folgendes:

- Mit einem einzelnen Standalone VMM-Server replizieren
- Replizieren Sie mit einem einzigen VMM-Server in einem gestreckten Windows Cluster bereitgestellt


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Replikation zwischen Standorten mit einem einzelnen Standalone VMM-server

![Eigenständige virtual VMM-server](./media/site-recovery-single-vmm/single-vmm-standalone.png)

In diesem Szenario den VMM-Server als virtueller Computer am primären Standort bereitstellen und diese VM an einem sekundären Standort Site Recovery mit Hyper-V Replica replizieren.

1. Einrichten von VMM auf einem Hyper-V-VM. Wir empfehlen, Installieren der SQL Server-Instanz von VMM auf dem gleichen virtuellen Computer verwendet, um Zeit zu sparen. Möchten Sie verwenden eine Remoteinstanz von SQL Server und einem Ausfall auftritt, müssen Sie die Instanz wiederherstellen, bevor Sie VMM wiederherstellen können.
2. Stellen Sie sicher, dass der VMM-Server mindestens zwei Clouds konfiguriert wurde. Eine Wolke enthält die VMs replizieren möchten und die Cloud dient als sekundären Standort. Die Cloud, die VMs enthält zu müssen:

    - Eine oder mehrere VMM-Hostgruppen, enthält ein oder mehrere Hyper-V-Hostservern in jede Hostgruppe.
    - Mindestens ein Hyper-V-VM auf jedem Hyper-V-Host-Server.

3. Ein Depot Recovery Services erstellen, generieren einen Registrierungsschlüssel Vault herunterladen und Tresor VMM-Server anmelden. Während der Registrierung installiert der Azure Site Recovery-Anbieter auf dem VMM-Server.
4. Einrichten einer oder mehreren Wolken auf VMM VM und Hyper-V-Hosts diese Wolken hinzufügen.
3. Konfigurieren Sie Schutz für Wolken. Als Quell- und Zielspeicherorte Geben Sie den Namen der VMM-Server. Um Netzwerkzuordnung zu konfigurieren, ordnen Sie VM-Netzwerk für die Cloud mit VMs VM-Netzwerk für die Replikation Cloud schützen möchten.
4. Aktivieren Sie erste Replikation für virtuelle Computer über das Netzwerk zu schützen, da beide Wolken auf demselben Server befinden soll.
4. Aktivieren Sie in Hyper-V-Manager-Konsole Hyper-V Replica auf Hyper-V-Host, der die VMM-VM enthält und Replikation des virtuellen Computers. Stellen Sie sicher, dass Sie VMM VM Wolken nicht hinzufügen, die von Site Recovery geschützt sind. Dadurch wird sichergestellt, dass Hyper-V Replica Einstellungen von Site Recovery überschrieben werden nicht.
5. Wenn Sie Pläne erstellen möchten, geben Sie dieselben VMM Server für Quelle und Ziel.

Führen Sie die Schritte in [diesem Artikel](site-recovery-vmm-to-vmm.md) ein Depot erstellen, registrieren Sie den Server und Schutz eingerichtet.

### <a name="what-to-do-in-an-outage"></a>Aktivitäten in einem Ausfall

Wenn eine vollständige Ausfall und vom sekundären Standort arbeiten, gehen Sie folgendermaßen vor:

1.  Führen Sie in Hyper-V-Manager-Konsole am sekundären Standort ein ungeplantes Failovers VMM VM vom primären zum sekundären Standort Failover.
2.  Überprüfen Sie die VMM VM läuft am sekundären Standort.
3.  Führen Sie im Tresor Recovery Services ein ungeplantes Failovers Arbeitslast VMs vom primären, sekundären Wolken Failover. Zum Abschließen des ungeplanten Failovers der VMs commit Failover oder einen anderen Wiederherstellungspunkt wählen.
4.  Nach Abschluss das ungeplante Failover können Benutzer Aufgaben Ressourcen am sekundären Standort zugreifen.

Wenn der primäre Standort wieder normal ist, führen Sie folgende Schritte aus:

1.  Hyper-V-Manager-Konsole aktivieren Sie umgekehrter Replikation für die VM VMM Starten vom sekundären zum primären replizieren.
2.  In Hyper-V-Manager-Konsole Ausführen eines geplanten Failovers Failback VMM VM vom primären Standort. Übernehmen Sie das Failover zum Abschließen. Aktivieren Sie umgekehrte Replikation zu VMM vom primären auf sekundären repliziert.
3.  Aktivieren Sie im Tresor Recovery Services reverse Replikation für die Arbeitslast VMs Starten vom sekundären zum primären replizieren.
4.  Führen Sie im Tresor Recovery Services Geplantes Failover Failback Arbeitslast VMs auf den primären Standort. Übernehmen Sie das Failover zum Abschließen. Aktivieren Sie umgekehrte Replikation replizieren Arbeitslast VMs vom primären, sekundären gestartet.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Replikation zwischen Standorten mit einem einzigen VMM-Server in einem Cluster gestreckt

![Cluster virtuellen VMM-server](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Bereitstellen eines eigenständigen VMM-Servers als ein virtueller Computer auf einem sekundären Standort repliziert, machen VMM hochverfügbare Sie als virtueller Computer in einem Windows-Failovercluster bereitstellen. Dies bietet Arbeitslast Stabilität und Schutz vor. Site Recovery bereitstellen sollten VMM VM als Stretchcluster geografisch getrennten Standorten bereitgestellt werden. Dazu:

1. Installieren Sie VMM auf einem virtuellen Computer in einem Windows-Failovercluster, und wählen Sie die Option Server als hoher Verfügbarkeit während der Installation ausgeführt.
2. Mit SQL Server AlwaysOn Availability sollte von VMM verwendete SQL Server-Instanz repliziert werden, damit eine der Datenbank in den sekundären Standort Kopie.
3. Führen Sie die Schritte in [diesem Artikel](site-recovery-vmm-to-vmm.md) ein Depot erstellen, registrieren Sie den Server und Schutz eingerichtet. Sie müssen jede VMM-Server im Cluster im Tresor Recovery Services registrieren. Hierzu Anbieter auf einem aktiven Knoten installieren und registrieren Sie den VMM-Server. Installieren Sie dann den Anbieter auf anderen Knoten.

### <a name="what-to-do-in-an-outage"></a>Aktivitäten in einem Ausfall

Bei einem Ausfall der VMM-Server und der entsprechenden SQL Server-Datenbank Failover und vom sekundären Standort zugegriffen.


## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr](site-recovery-vmm-to-vmm.md) über detaillierte Site Recovery-Bereitstellung für VMM VMM-Replikation.
