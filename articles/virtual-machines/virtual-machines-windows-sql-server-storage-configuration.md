<properties
    pageTitle="Speicherkonfiguration für SQL Server VMs | Microsoft Azure"
    description="Dieses Thema beschreibt, wie Azure Storage für SQL Server VMs konfiguriert, während der Bereitstellung (Ressourcen-Manager-Bereitstellungsmodell). Darüber hinaus konfigurieren Storage für Ihre vorhandenen SQL Server-VMs."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="ninarn"
    manager="jhubbard"    
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/04/2016"
    ms.author="ninarn" />

# <a name="storage-configuration-for-sql-server-vms"></a>Speicherkonfiguration für SQL Server VMs

Beim Konfigurieren einer SQL Server-VM Bild in Azure ermöglicht das Portal die Speicherkonfiguration automatisieren. Dies schließt die VM, zugänglich, SQL Server, Speicher und Konfiguration für Ihre spezifischen Bedürfnisse optimiert Speicher zuweisen.

In diesem Thema wird erläutert, wie Azure Storage für Ihre VMs SQL Server während der Bereitstellung und für vorhandene virtuelle Computer konfiguriert. Diese Konfiguration basiert auf [best Practices zur Performance](virtual-machines-windows-sql-performance.md) für Azure VMs mit SQL Server.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassische Bereitstellungsmodell.

## <a name="prerequisites"></a>Erforderliche Komponenten
Zur Verwendung von automatischen Speicher-Konfigurationen muss virtuellen Computers folgende Merkmale:

- Mit [SQL Server Bild](virtual-machines-windows-sql-server-iaas-overview.md#option-1-deploy-a-sql-vm-per-minute-licensing)bereitgestellt.
- Das [Modell zur Bereitstellung von Ressourcen-Manager](../resource-manager-deployment-model.md)verwendet.
- [Premium-Speicher](../storage/storage-premium-storage.md)verwendet.

## <a name="new-vms"></a>Neuer VMs
In den folgenden Abschnitten wird beschrieben, wie Speicher für neue virtuelle SQL Server-Computer konfigurieren.

### <a name="azure-portal"></a>Azure-Portal
Beim Bereitstellen einer Azure VM mit SQL Server-Bild können Sie den Speicher für neue VM automatisch konfigurieren. Geben Sie die Speichergröße, Leistungsgrenzwerte und Art der Auslastung. Der folgende Screenshot zeigt während SQL VM-Konfiguration-Blade für Speicher bereitstellen.

![SQL Server-VM-Speicher-Konfiguration während der Bereitstellung](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-provisioning.png)

Anhand Ihrer Auswahl führt Azure Konfigurationsaufgaben von Speicher nach dem Erstellen des virtuellen Computer:

- Erstellt und fügt Premium-Datenträger Daten auf dem virtuellen Computer.
- Der Datenträger für SQL Server zu konfigurieren.
- Konfiguriert die Datenträger in einem Speicher-Pool basierend auf den angegebenen Größe (IOPS und Durchsatz).
- Ein neues Laufwerk auf dem virtuellen Computer den Speicherpool zugeordnet.
- Dieses neue Laufwerk anhand der angegebenen Arbeitsauslastungsdatei (Transactional processing Datawarehousing oder Allgemein) optimiert.

Weitere Detailinformationen wie Azure Einstellungen konfiguriert finden Sie im [Konfigurationsabschnitt Speicher](#storage-configuration). Eine Anleitung zum Erstellen einer SQL Server-VM in Azure-Portal finden Sie unter [Bereitstellung Tutorial](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="resource-manage-templates"></a>Ressourcenvorlagen verwalten
Verwenden Sie die folgenden Ressourcen-Manager-Vorlagen werden standardmäßig ohne Speicher-Pool-Konfiguration zwei Premium-Datenträger angefügt. Allerdings können diese Vorlagen zum Ändern der Anzahl der Premium-Datenträger, die den virtuellen Computer zugeordnet sind.

- [Erstellen virtueller Computer mit automatisiertem Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autobackup)
- [VM mit automatisierten Patches erstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-autopatching)
- [VM AKV Integration erstellen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-sql-full-keyvault)

## <a name="existing-vms"></a>Vorhandene VMs
Für vorhandene virtuelle Computer von SQL Server können Sie einige Einstellungen im Azure-Portal. Wählen Sie die VM Einstellungen Bereich und wählen Sie SQL Server-Konfiguration. Die Konfiguration der SQL Server-Blade zeigt aktuelle Speicher Verwendung Ihrer VM. In diesem Diagramm werden alle Laufwerke, die vorhanden auf Ihrem virtuellen Computer angezeigt. Für jedes Laufwerk zeigt der Speicherplatz in vier Abschnitte:

- SQL-Daten
- SQL-Protokoll
- Andere (nicht SQL-Speicher)
- Verfügbar

![Konfigurieren von Speicher für bestehende SQL Server-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-existing.png)

Konfigurieren von Speicher, um ein neues Laufwerk hinzufügen oder ein vorhandenes Laufwerk erweitern klicken Sie oberhalb des Diagramms bearbeiten.

Die Konfigurationsoptionen, die Sie sehen, ob je haben Sie diese Funktion vor verwendet. Wenn Sie zum ersten Mal verwenden, können Sie Ihr Speicherbedarf für ein neues Laufwerk angeben. Wenn Sie diese Funktion zuvor verwendet haben, ein Laufwerk, können Sie das Laufwerk Speicherplatz erweitern.

### <a name="use-for-the-first-time"></a>Die erstmalige Verwendung
Ist es mit dieser Funktion zum ersten Mal können Sie die Speichergrenzwerte Größe für ein neues Laufwerk angeben. Dies ähnelt der angezeigt zur Bereitstellungszeit. Der wichtigste Unterschied ist, dass Sie nicht berechtigt sind, geben Sie die Arbeitslast. Diese Einschränkung verhindert die Unterbrechung alle vorhandenen SQL Server-Konfigurationen auf dem virtuellen Computer.

![Konfigurieren von SQL Server Speicher Schieberegler](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-usage-sliders.png)

Azure erstellt ein neues Laufwerk basierend auf Ihren Spezifikationen. In diesem Szenario führt Azure Storage Konfigurationsaufgaben aus:

- Erstellt und fügt Premium-Datenträger Daten auf dem virtuellen Computer.
- Der Datenträger für SQL Server zu konfigurieren.
- Konfiguriert die Datenträger in einem Speicher-Pool basierend auf den angegebenen Größe (IOPS und Durchsatz).
- Ein neues Laufwerk auf dem virtuellen Computer den Speicherpool zugeordnet.

Weitere Detailinformationen wie Azure Einstellungen konfiguriert finden Sie im [Konfigurationsabschnitt Speicher](#storage-configuration).

### <a name="add-a-new-drive"></a>Hinzufügen eines neuen Laufwerks
Wenn Speicher auf Ihrem SQL Server-VM bereits konfiguriert haben, zeigt zwei neue Optionen Erweiterung des Speichers. Die erste Option ist ein neues Laufwerk hinzufügen, das die Leistung Ihrer VM erhöhen können.

![Fügen Sie ein neues Laufwerk einer SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-configuration-add-new-drive.png)

Hinzugefügte Laufwerk müssen Sie jedoch einige zusätzliche manuelle Konfiguration zum Steigern der Leistung ausführen.

### <a name="extend-the-drive"></a>Das Laufwerk
Die andere Option für die Erweiterung des Speichers wird auf das vorhandene Laufwerk erweitern. Diese Option erhöht den verfügbaren Speicherplatz für das Laufwerk, aber es erhöht nicht die Leistung. Speicher-Pools können Sie die Anzahl der Spalten ändern, nachdem Speicherpool erstellt wird. Die Anzahl der Spalten bestimmt die Anzahl der parallel Schreibvorgänge auf den Datenträgern verteilt werden können. Daher können keine hinzugefügten Datenträger Leistung erhöhen. Sie können nur Speicherplatz für die Daten geschrieben werden. Diese Einschränkung bedeutet auch, dass beim Erweitern des Laufwerks die Anzahl der Spalten mit die Mindestanzahl der Datenträger bestimmt, die Sie hinzufügen können. Einen Speicher-Pool mit vier Festplatten erstellen, also die Anzahl der Spalten auch vier. Jedes Mal, wenn Sie den Speicher erweitern müssen mindestens vier Laufwerke hinzufügen.

![Erweitern Sie ein Laufwerk für einen SQL-VM](./media/virtual-machines-windows-sql-storage-configuration/sql-vm-storage-extend-a-drive.png)

## <a name="storage-configuration"></a>Speicherkonfiguration
Dieser Abschnitt enthält eine Referenz für Speicher-Neukonfiguration, die Azure automatisch während SQL VM-Bereitstellung und Konfiguration in Azure-Portal durchführt.

- Wenn weniger als zwei TB Speicher für Ihren virtuellen Computer ausgewählt haben, erstellt Azure keine Speicher-Pool.
- Wenn mindestens zwei TB Speicherplatz für die virtuellen Computer ausgewählt haben, konfiguriert Azure einen Speicher-Pool. Der nächste Abschnitt dieses Themas erläutert die Pools-Konfiguration.
- Automatische Speicherung Konfiguration immer verwendet [Storage Premium](../storage/storage-premium-storage.md) P30 Datenträger. Daher besteht eine 1:1-Zuordnung zwischen der ausgewählten Terabyte und die Anzahl der Datenträger, der VM zugeordnet.

Preisinformationen, finden Sie unter [Speicher Preisgestaltung](https://azure.microsoft.com/pricing/details/storage) -Seite auf der Registerkarte **Datenträger** .

### <a name="creation-of-the-storage-pool"></a>Erstellung eines Speicher-Pools
Azure verwendet Folgendes Speicherpool auf SQL Server VMs zu erstellen.

| Einstellung | Wert |
|-----|-----|
| Stripe-Größe  | 256 KB (Datawarehousing); 64 KB (Transaktion) |
| Datenträger | 1 TB |
| Cache | Lesen |
| Zuordnung | 64 KB NTFS die Größe der Zuordnungseinheiten |
| Instant initialisieren | Aktiviert |
| Sperren von Seiten im Speicher | Aktiviert |
| Wiederherstellung | Einfache Wiederherstellung (keine Flexibilität) |
| Anzahl der Spalten | Anzahl der Datenträger<sup>1</sup> |
| Speicherort der temporären Datenbank | Gespeicherte Daten Datenträger<sup>2</sup> |

<sup>1</sup> nach der Speicherpool erstellt wird, können nicht Sie die Anzahl der Spalten im Speicherpool ändern.

<sup>2</sup> diese Einstellung gilt nur für das erste Laufwerk mit der Speicher Konfiguration erstellten.

## <a name="workload-optimization-settings"></a>Aufgaben Optimierung settings
Die folgende Tabelle beschreibt die drei Workload Typ verfügbaren Optionen und ihre entsprechenden Optimierungen:

| Art der Auslastung | Beschreibung | Optimierungen |
|-----|-----|-----|
| **Allgemein** | Standardeinstellung, unterstützt die meisten Arbeitslasten | Keine |
| **Transaktionale Verarbeitung** | Optimiert den Speicher für herkömmlichen OLTP-Arbeitslasten | Das Ablaufverfolgungsflag 1117<br/>Das Ablaufverfolgungsflag 1118 |
| **Data warehousing** | Optimiert den Speicher für analytische und Berichtstools Arbeitslasten | Das Ablaufverfolgungsflag 610<br/>Das Ablaufverfolgungsflag 1117 |

>[AZURE.NOTE] Der Arbeitsauslastungstyp können nur angeben, bei der Bereitstellung einer virtuellen Maschine SQL im Speicher Konfigurationsschritt auswählen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Themen zum Ausführen von SQL Server in Azure VMs finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).
