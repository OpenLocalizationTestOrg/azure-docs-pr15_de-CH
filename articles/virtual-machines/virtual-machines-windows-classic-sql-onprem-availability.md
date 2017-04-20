<properties
    pageTitle="Lokale Verfügbarkeitsgruppen ständig in Azure erweitern | Microsoft Azure"
    description="Dieses Lernprogramm verwendet Ressourcen mit dem klassischen Bereitstellungsmodell erstellt und beschreibt, wie der Assistent hinzufügen in SQL Server Management Studio (SSMS) in Azure ein Replikat immer auf Availability Group hinzu."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Erweitern Sie lokale Verfügbarkeitsgruppen ständig in Azure

Immer auf Verfügbarkeitsgruppen bieten hohe Verfügbarkeit für Gruppen der Datenbank durch Hinzufügen von sekundären Replikaten. Diese Replikate ermöglichen Failover für Datenbanken im Fehlerfall. Darüber hinaus können sie verwendet lesen Arbeitslasten oder backup-Aufgaben abnimmt.

Lokale Verfügbarkeitsgruppen kann Microsoft Azure durch eine oder mehrere Azure virtueller Computer mit SQL Server-Bereitstellung und dann als Replikate der lokalen Verfügbarkeitsgruppen hinzufügen.

Es wird vorausgesetzt, dass Sie über Folgendes verfügen:

- Ein aktives Azure-Abonnement. Sie können [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/).

- Ein immer auf Verfügbarkeit bestehendes lokal. Weitere Informationen zu Verfügbarkeitsgruppen anzeigen Sie [Immer auf Verfügbarkeitsgruppen](https://msdn.microsoft.com/library/hh510230.aspx)

- Die Verbindung zwischen dem lokalen Netzwerk und Azure virtuelle Netzwerk. Weitere Informationen zum Erstellen dieses virtuellen Netzwerks finden Sie unter [Konfigurieren einer Standort-zu-Standort-VPN im klassischen Azure-Portal](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Azure-Assistent hinzufügen

Dieser Abschnitt veranschaulicht die **Azure-Assistent hinzufügen** verwenden, um Ihre Lösung stets auf Availability Group auf Azure Replikate erweitern.

1. In SQL Server Management Studio, erweitern Sie **Immer hohe Verfügbarkeit** > **Verfügbarkeitsgruppen** > **[Name der Availability Group]**.

1. Maustaste auf **Verfügbarkeitsreplikate**Sie **Replikat hinzufügen**.

1. Standardmäßig wird das **Replikat auf Verfügbarkeit Assistenten hinzufügen** . Klicken Sie auf **Weiter**.  Wenn Sie die Option **nicht anzeigen dieser Seite** am unteren Rand der Seite beim vorherigen Start des Assistenten ausgewählt haben, wird dieser Bildschirm nicht angezeigt.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. Sie müssen alle vorhandenen sekundären Replikate herstellen. Klicken Sie auf **verbinden...** Jedes Replikat oder Sie können **Alle verbinden...** klicken am unteren Bildschirmrand. Klicken Sie nach der Authentifizierung auf **Weiter** zum nächsten Bildschirm wechseln.

1. Auf der Seite **Geben Sie Replikate** werden mehrere Registerkarten oben aufgeführt: **Replikate**, **Endpunkte** **Backup Voreinstellungen**und **Listener**. Klicken Sie auf der Registerkarte **Replikate** auf **Azure Replikat hinzufügen...** um den Assistenten zum Hinzufügen von Azure Replikat zu starten.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Auswählen einer vorhandenen Azure-Verwaltungszertifikat aus der lokalen Windows-Zertifikatspeicher vor installiert haben Wählen Sie oder geben Sie die Kennung der Azure-Abonnement, wenn Sie zuvor verwendet haben. Klicken Sie zum Herunterladen und Installieren von Azure-Verwaltungszertifikat und die Liste der Abonnements ein Azure-Konto herunterladen.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Die füllt der Feldes auf der Seite mit den Werten zum Azure Virtual Machine (VM) erstellen, der das Replikat hostet.

  	|Einstellung|Beschreibung|
|---|---|
|**Bild**|Wählen Sie die gewünschte Kombination von Betriebssystem und SQL Server|
|**Virtueller Speicher**|Wählen Sie die Größe der VM, die am besten geeigneten business|
|**Name des virtuellen Computers**|Geben Sie einen eindeutigen Namen für den neuen virtuellen Computer. Der Name muss zwischen 3 und 15 Zeichen enthalten, können nur Buchstaben, Zahlen und Bindestriche enthalten mit einem Buchstaben beginnen und mit einem Buchstaben oder einer Zahl enden.|
|**VM-Benutzername**|Geben Sie einen Benutzernamen auf der VM werden|
|**VM-Administratorkennwort**|Geben Sie ein Kennwort für das neue Konto|
|**Kennwort bestätigen**|Bestätigen Sie das Kennwort des neuen Kontos|
|**Virtuelles Netzwerk**|Geben Sie Azure virtuelle Netzwerk, das die neue VM verwenden soll. Weitere Informationen zu virtuellen Netzwerken finden Sie unter [Übersicht über Virtual Network](../virtual-network/virtual-networks-overview.md).|
|**Virtuelle Netzwerk-Subnetz**|Geben Sie virtuelles Netzwerk-Subnetz an, das neue VM sollte|
|**Domäne**|Bestätigen Sie der voreingestellte Wert für die Domäne|
|**Domänenbenutzername**|Geben Sie ein Konto in der Gruppe der lokalen Administratoren auf dem lokalen Clusterknoten|
|**Kennwort**|Geben Sie das Kennwort für den Domänenbenutzernamen|

1. Klicken Sie auf **OK** , um die Bereitstellungseigenschaften überprüfen.

1. Vertragsbedingungen werden weiter angezeigt. Lesen Sie und klicken Sie auf **OK** , wenn Geschäftsbedingungen zustimmen.

1. **Geben Sie Replikate** wird erneut angezeigt. Überprüfen Sie die Einstellungen für das neue Azure Replikat auf den Registerkarten **Replikate**, **Endpunkte**und **Backup-Voreinstellungen** . Ändern Sie Ihre geschäftlichen Bedürfnisse zugeschnitten.  Weitere Informationen zu den auf diesen Registerkarten enthaltenen Parameter finden Sie in [Geben Sie Replikate Seite (Verfügbarkeit Assistent-Add Replikat Assistenten)](https://msdn.microsoft.com/library/hh213088.aspx). Beachten Sie, dass mithilfe der Registerkarte Listener für Verfügbarkeitsgruppen, Azure Replikate enthalten, Listener erstellt werden können. Darüber hinaus ein Listener bereits angelegt vor dem Starten des Assistenten, erhalten Sie eine Meldung in Azure nicht unterstützt wird. Wir sehen im Abschnitt **Erstellen einer Verfügbarkeitsgruppenlistener** Listener erstellen.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Klicken Sie auf **Weiter**.

1. Wählen Sie die Daten-Synchronisierung zu auf **Ursprüngliche Datensynchronisation auswählen** und auf **Weiter**. In den meisten Fällen wählen Sie **Vollständige Daten-Synchronisierung**. Weitere Informationen zu Methoden für die Synchronisierung finden Sie unter [Wählen Sie anfängliche Synchronisierung Datenseite (immer auf Verfügbarkeit Gruppe Assistenten)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Überprüfen Sie die Ergebnisse auf **der Überprüfungsseite** . Beheben Sie Probleme und führen Sie die Validierung erneut aus, falls erforderlich. Klicken Sie auf **Weiter**.

    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Überprüfen Sie auf der Seite **Zusammenfassung** , und klicken Sie auf **Fertig stellen**.

1. Der Bereitstellungsprozess beginnt. Nach erfolgreichem des Assistenten Abschluss klicken Sie auf **Schließen** um den Assistenten zu beenden.

>[AZURE.NOTE] Den Assistenten zum Hinzufügen von Azure Replikat erstellt eine Protokolldatei in Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Problembehandlung bei fehlgeschlagenen Azure Replikat Bereitstellung kann diese Protokolldatei verwendet werden. Schlägt der Assistent Ausführen jeder Aktion werden alle vorhergehenden Operationen zurückgesetzt einschließlich den bereitgestellten virtuellen Computer löschen.

## <a name="create-an-availability-group-listener"></a>Erstellen einer verfügbarkeitsgruppenlistener

Nach dem Erstellen die verfügbarkeitsgruppe sollten Sie einen Listener für Clients für Replikate erstellen. Listener leiten eingehende Verbindungen primären oder einer sekundären schreibgeschützt. Weitere Informationen zum Listener finden Sie unter [Konfigurieren einer ILB Listener für immer auf Verfügbarkeit in Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Nächste Schritte

Neben der **Azure-Assistent hinzufügen** Ihre immer auf Availability Group in Azure erweitern, können Sie auch einige SQL Server-Arbeitslasten vollständig in Azure verschieben. Zum Einstieg finden Sie unter [Bereitstellen einer SQL Server-VM in Azure](virtual-machines-windows-portal-sql-server-provision.md).

Weitere Themen zum Ausführen von SQL Server in Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
