<properties
    pageTitle="Migrieren von virtuellen Windows-Maschinen von Amazon Web Services in Azure mit Site Recovery | Microsoft Azure"
    description="Dieser Artikel beschreibt, wie virtuelle Windows Maschinen in Amazon Web Services (AWA) in Azure mithilfe von Azure Site Recovery migrieren."
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
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrieren von virtuellen Windows-Maschinen Amazon Web Services (AWS) in Azure mit Azure Site Recovery

## <a name="overview"></a>Übersicht

Willkommen bei Azure Site Recovery. Anhand dieses Artikels Windows Instanzen in AWS in Azure mit Website migrieren. Bevor Sie beginnen, beachten Sie Folgendes:

- Azure hat zwei verschiedene Bereitstellungsmodelle für erstellen und Verwenden von Ressourcen: Azure Resource Manager und Classic. Azure verfügt außerdem über zwei Portale – klassischen Azure-Portal, das klassische Bereitstellungsmodell unterstützt und Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Die grundlegenden Schritte für die Migration sind identisch, ob Sie Site Recovery im Ressourcenmanager oder Classic konfigurieren. UI-Informationen und Screenshots in diesem Artikel sind jedoch für Azure-Portal.
- **Derzeit können Sie nur von AWS in Azure migrieren. VMs AWS Azure Failover, aber Sie können nicht sie wieder erneut. Es gibt keine Replikation.**
- Migration Anleitung in diesem Artikel basieren auf den für die Replikation von eines physischen Computers in Azure. Sie enthält Links zu den Schritten in [virtuelle VMware-Computer replizieren oder physische Server in Azure](site-recovery-vmware-to-azure.md)beschreibt einen physischen Server in Azure-Portal zu replizieren.
- Wenn Site Recovery im klassischen Portal einrichten, führen Sie die detaillierte Anleitung in [diesem Artikel](site-recovery-vmware-to-azure-classic.md). **Verwenden Sie nicht** dieses [ältere Artikel](site-recovery-vmware-to-azure-classic-legacy.md).

Buchen Sie Kommentare oder Fragen am Ende dieses Artikels oder [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen für die Bereitstellung

- **Konfigurationsserver**: eine lokale VM unter Windows Server 2012 R2 Konfigurationsserver fungiert. Installation von anderen Site Recovery Komponenten (einschließlich Prozessserver und master Zielserver) auf diesem virtuellen Computer zu. Weitere [Szenario Architektur](site-recovery-vmware-to-azure.md#scenario-architecture) und [Konfiguration Server erforderliche Komponenten](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **EC2 VM-Instanzen**: Instanzen Windows migrieren möchten.

## <a name="deployment-steps"></a>Bereitstellungsschritte

Dieser Abschnitt beschreibt die Schritte im neuen Azure-Portal. Finden Sie diese Bereitstellungsschritte für Site Recovery im klassischen Portal, in [diesem Artikel](site-recovery-vmware-to-azure-classic.md).

1. [Erstellen eines Depots](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Konfigurationsserver bereitstellen](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Nach dem Konfigurationsserver bereitgestellt haben, überprüfen Sie, ob er kommunizieren kann mit VMs, die Sie migrieren möchten.
4. [Einrichten von Replikationseigenschaften](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Erstellen Sie einer Replikationsrichtlinie und dem Konfigurationsserver zuweisen.
5. [Mobility-Dienst installieren](site-recovery-vmware-to-azure.md#step-6-replication-application). Jede VM zu schützen braucht den Mobility-Dienst installiert. Dieser Dienst sendet Daten an den Prozess-Server. Mobility-Dienst kann manuell installiert oder abgelegt und installiert automatisch den Prozess-Server bei aktiviertem Schutz für die VM. Firewallregeln EC2-Instanzen, die Sie migrieren möchten sollten für Push-Installation dieses Service konfiguriert werden. Die Sicherheitsgruppe EC2 Instanzen müssen die folgenden Regeln:

    ![Firewall-Regeln](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Aktivieren der Replikation](site-recovery-vmware-to-azure.md#enable-replication). Aktivieren der Replikation für die virtuellen Computer migrieren möchten. Entdecken Sie die EC2 Instanzen mit privaten IP-Adressen aus der Konsole EC2 zu erhalten.
7. [Ein ungeplantes Failover ausgeführt](site-recovery-failover.md#run-an-unplanned-failover). Nach Abschluss der ersten Replikation kann ein ungeplantes Failovers von AWS in Azure für jeden VM ausführen. Optional können Sie einen Wiederherstellungsplan erstellen und Ausführen ein ungeplanten Failover zum Migrieren mehrerer virtueller Maschinen von AWS in Azure. [Weitere Informationen](site-recovery-create-recovery-plans.md) zu Recovery-Pläne.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Replikationsszenarien in [Neuigkeiten Azure Site Recovery?](site-recovery-overview.md)
