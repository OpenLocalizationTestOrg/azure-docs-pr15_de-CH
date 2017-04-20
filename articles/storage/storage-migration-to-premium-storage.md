<properties
    pageTitle="Migrieren zu Azure Premium Speicher | Microsoft Azure"
    description="Die vorhandenen virtuellen Computer in Azure Premium-Speicher migriert. Premium-Speicher unterstützt hohe Leistung, niedriger Latenz Disk I/O-Intensive Arbeitslasten auf Azure virtuellen Computer ausgeführt."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="yuemlu"/>


# <a name="migrating-to-azure-premium-storage"></a>Migrieren von Azure Premium-Speicher

## <a name="overview"></a>Übersicht

Azure Storage Premium bietet Unterstützung für virtuelle Maschinen, I/O-Intensive Arbeitslasten hohe Leistung, niedriger Latenz Datenträger. Der Geschwindigkeitsvorteil und Leistung dieser Festplatten können durch die Anwendung VM Festplatten Azure Premium Speicher migrieren.

Dieses Handbuch soll neue Benutzer von Azure Premium Speicher besser einen reibungslosen Übergang vom aktuellen System Premium Speicher vorbereiten. Das Handbuch behandelt drei Hauptkomponenten in diesem Prozess: 

  - [Planen der Migrations zu Premium-Speicher](#plan-the-migration-to-premium-storage)
  - [Vorbereiten und virtuelle Festplatten (VHDs) Premium-Speicher kopieren](#prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage)
  - [Erstellen Sie Azure Virtual Machine Premium Speicher](#create-azure-virtual-machine-using-premium-storage)

Migration von VMs von anderen Plattformen zu Azure Premium Storage oder Migrieren vorhandener Azure VMs von Standardspeicher Premium-Speicher. Dieser Leitfaden deckt Schritte für beide zwei Szenarien. Gehen Sie im entsprechenden Abschnitt je nach Szenario angegeben.

>[AZURE.NOTE] Finden Sie eine Übersicht über die Features und Preise für Premium-Speicher in Storage Premium: [Hochleistungsspeicher für Azure Virtual Machine Arbeitslasten](storage-premium-storage.md). Migrieren von virtuellen Datenträger, hohe IOPS Azure Premium Speicher für die optimale Leistung für Ihre Anwendung wird empfohlen. Wenn Festplatte hohe IOPS nicht benötigen, können Sie Kosten Standardspeicher der virtuellen Festplattendaten auf Festplattenlaufwerken (HDDs) statt SSDs gespeichert beibehalten beschränken.

Abschließen des Migrationsprozesses insgesamt benötigen zusätzliche Aktionen vor und nach den Schritten in diesem Handbuch. Beispiele sind virtuelle Netzwerke oder Endpunkte konfigurieren oder Code Änderungen in selbst die gewisse Ausfallzeit der Anwendung erfordern. Diese Aktionen sind für jede Anwendung eindeutig und sollten sie mit den Schritten in diesem Handbuch zu den Übergang Premium Speicher so reibungslos wie möglich durchgeführt.


## <a name="plan-the-migration-to-premium-storage"></a>Planen der Migrations zu Premium-Speicher

In diesem Abschnitt wird sichergestellt, dass der Schritte in diesem Artikel die Migration bereit sind und hilft Ihnen die beste Entscheidung zu VM und Datenträger.

### <a name="prerequisites"></a>Erforderliche Komponenten
- Sie benötigen ein Azure-Abonnement. Sie haben, Sie Erstellen eines einmonatigen [Testversion](https://azure.microsoft.com/pricing/free-trial/) Abonnements oder besuchen Sie für weitere Optionen [Azure Preise](https://azure.microsoft.com/pricing/) .
- PowerShell-Cmdlets ausführen zu können, benötigen Sie Microsoft Azure PowerShell-Modul. Finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) Installationshinweise zeigen und Installation.
- Wenn Sie Azure VMs auf Premium-Speicher verwenden möchten, müssen Sie Premium Speicher kann VMs verwenden. Standard- und Premium-Speicher Festplatten können mit Premium-Speicher kann VMs. Premium-Datenträger werden in Zukunft mit mehr VM. Weitere Informationen zu allen verfügbaren Azure VM Laufwerkstypen und-Größen finden Sie [Größen für virtuelle Maschinen](../virtual-machines/virtual-machines-windows-sizes.md) und [-Größen für Cloud-Services](../cloud-services/cloud-services-sizes-specs.md).

### <a name="considerations"></a>Hinweise

Ein Azure-VM unterstützt mehrere Premium-Datenträger anhängen, so dass die Anwendung bis zu 64 TB pro VM. Mit Storage Premium erreichen Ihre Anwendung 80.000 IOPS (Input/Output-Operationen pro Sekunde) pro VM und 2000 MB Datendurchsatz pro Sekunde Datenträger pro VM mit extrem niedriger Latenz für Lesevorgänge. Sie haben Optionen an VMs und Festplatten. In diesem Abschnitt wird eine Möglichkeit finden, die Ihre Arbeit am besten.

#### <a name="vm-sizes"></a>VM-Größen
[Größen für virtuelle Maschinen](../virtual-machines/virtual-machines-windows-sizes.md)die Azure-VM Angaben entnehmen. Überprüfen Sie die Leistungsmerkmale von virtuellen Computern, die mit Premium und wählen Sie das am besten geeignete VM, die Ihre Arbeit am besten. Stellen Sie sicher, dass auf der VM auf dem Datenträger Verkehr ausreichende Bandbreite vorhanden ist.


#### <a name="disk-sizes"></a>Datenträger
Es gibt drei Typen von Datenträgern, die mit Ihrer VM und jeweils spezifische IOPs und Durchsatz Grenzen. Berücksichtigen Sie diese Grenzen als Typ für die VM Auswahl basierend auf den Bedürfnissen Ihrer Anwendung Kapazität, Leistung, Skalierbarkeit, Spitzenlasten.

|Premium-Speichertyp für Datenträger|P10|P20|P30|
|:---:|:---:|:---:|:---:|
|Festplattengröße|128 GB|512 GB|1024 GB (1 TB)|
|IOPS pro Datenträger|500|2300|5000|
|Durchsatz pro Datenträger|100 MB pro Sekunde|150 MB pro Sekunde|200 MB pro Sekunde|

Je nach der Auslastung zu bestimmen, ob zusätzliche Datenträger für die VM erforderlich sind. Sie können die VM permanente Datenträger zuordnen. Bei Bedarf können Sie auf Datenträger die Kapazität und Leistung des Volumes zu verteilen. (Disk Striping sehen [hier](storage-premium-storage-performance.md#disk-striping).) Wenn Streifen Storage Premium-Datenträger [Storage Spaces]mit[4], Sie sollten mit einer Spalte für jedes Laufwerk, das verwendet wird, konfigurieren. Andernfalls kann die allgemeine Leistung von Stripesetvolumes erwarteten durch ungleichmäßige Verteilung des Datenverkehrs auf die Laufwerke vorangehen. *Mdadm* -Dienstprogramm können Sie für Linux VMs auch. Siehe [Konfigurieren des Software-RAID auf Linux](../virtual-machines/virtual-machines-linux-configure-raid.md) für Details.

#### <a name="storage-account-scalability-targets"></a>Konto Skalierbarkeitsziele
Premium-Speicherkonten verfügen folgenden Skalierbarkeitsziele neben [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md). Ihre Anforderungen der Skalierbarkeit ein einzelnes Speicherkonto übertreffen, erstellen Sie die Anwendung für mehrere Speicherkonten und Partitionieren der Daten über die Speicherkonten.

|Total Kapazität|Gesamte Bandbreite für ein Speicherkonto lokal redundanter|
|:--|:---|
|Festplattenkapazität: 35TB<br />Snapshot-Kapazität: 10 TB|Bis zu 50 Gigabit pro Sekunde für eingehenden und ausgehenden|

Informationen auf Premium Speicher Spezifikationen checken Sie [Skalierbarkeits- und Performance-Ziele bei Premium Speicherung aus](storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage).

#### <a name="disk-caching-policy"></a>Datenträger Cacherichtlinie
Cachingrichtlinie Datenträger ist *Schreibgeschützt* für alle Premium-Daten Festplatten und *Lese-/ Schreibzugriff* für die Premium-Betriebssystem-CD an der VM. Diese Einstellung wird empfohlen, zum Erreichen optimaler Leistung für Ihre Anwendung IOs. Schreibzugriff Heavy oder nur-schreiben-Datenträger (z. B. SQL Server-Protokolldateien) deaktivieren Sie Datenträgerspeicher, Anwendungsleistung verbessert. Cache RasterX vorhandenen Datenträger können über [Azure-Portal](https://portal.azure.com) oder *HostCaching -* Parameter des Cmdlets *Set AzureDataDisk* aktualisiert werden.

#### <a name="location"></a>Speicherort
Wählen Sie einen Speicherort, Azure Premium Speicher verfügbar ist. Aktuelle Informationen über die verfügbaren Standorte finden Sie unter [Azure Services nach Region](https://azure.microsoft.com/regions/#services) . Virtuelle Computer befindet sich in der gleichen Region wie das Speicherkonto, daß speichert die Datenträger für die VM wesentlich leistungsfähiger ist als wenn sie in separaten Bereichen.

#### <a name="other-azure-vm-configuration-settings"></a>Azure VM her
Beim Erstellen einer VM Azure müssen Sie bestimmte VM konfigurieren. Beachten Sie, dass einige Einstellungen behoben werden während der Lebensdauer der VM ändern oder andere hinzufügen. Überprüfen Sie diese Konfigurationen Azure VM und sicherstellen Sie, dass diese entsprechend konfiguriert sind, um Ihre Aufgaben erfüllen.

### <a name="optimization"></a>Optimierung

[Azure Premium Storage: Entwurf für hohe Leistung](storage-premium-storage-performance.md) Leitlinien zum Erstellen von leistungsstarken Azure Premium Speicher. Sie können die Richtlinien zusammen mit best Practices zur Performance für von der Anwendung verwendeten Technologien folgen.


## <a name="prepare-and-copy-virtual-hard-disks-VHDs-to-premium-storage"></a>Vorbereiten und virtuellen Festplatten (VHDs) Premium-Speicher kopieren

Der folgende Abschnitt enthält Richtlinien für das Vorbereiten von VHDs von VM und VHDs in Azure-Speicher kopiert.

- [Szenario 1: "Ich bin vorhandene Azure VMs Azure Premium-Speicher migrieren."](#scenario1)
- [Szenario 2: "Ich bin VMs von anderen Plattformen Azure Premium-Speicher migrieren."](#scenario2)

### <a name="prerequisites"></a>Erforderliche Komponenten

Um die VHDs Vorbereitung der Migration müssen Sie:

- Ein Azure-Abonnement ein Speicherkonto und Container in diesem Storage-Konto, Ihre VHD kopieren können. Beachten Sie, dass Speicherkonto ein Konto Standard oder Premium Speicher je nach Bedarf.
- Ein Tool VHD verallgemeinern Sie mehrere VM-Instanzen erstellen möchten. Sysprep für Windows oder Virt Sysprep für Ubuntu.
- Ein Werkzeug, das Speicherkonto VHD-Datei hinzufügen. Siehe [Datenübertragung mit AzCopy-Befehlszeilenprogramm](storage-use-azcopy.md) oder eine [Azure-Speicher-Explorer](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx)verwenden. Dieses Handbuch beschreibt Ihre VHD Werkzeug AzCopy kopieren.

> [AZURE.NOTE] Wenn Sie synchrone Kopie mit AzCopy, Option für optimale Leistung kopieren Sie Ihre VHD mit einem dieser Tools von Azure-VM, die im Bereich Storage Konto Wenn Sie eine virtuelle Festplatte aus einer Azure VM in einen anderen Bereich kopieren, kann die Leistung verlangsamt werden.
>
> Kopieren Sie eine große Datenmenge über begrenzte Bandbreite, verwenden [des Azure Import/Export-Dienstes Daten-BLOB-Speicher übertragen](storage-import-export-service.md); Dadurch können Sie für den Datentransfer durch Versand Festplatten, Azure-Rechenzentrum. Azure Import/Export-Dienst können Sie Daten in einem standardmäßigen Speicher-Konto kopieren. Sobald in Ihrem Konto Standardspeicher Daten können die [Kopie Blob-API](https://msdn.microsoft.com/library/azure/dd894037.aspx) oder AzCopy die Datenübertragung an das Speicherkonto Premium.
>
> Beachten Sie, dass Microsoft Azure nur feste Größe VHD-Dateien unterstützt. VHDX Dateien oder dynamische Festplatten werden nicht unterstützt. Haben Sie eine dynamische VHD können Sie es [Konvertieren VHD](http://technet.microsoft.com/library/hh848454.aspx) -Cmdlet mit fester Größe konvertieren.

### <a name="scenario1"></a>Szenario 1: "Ich bin vorhandene Azure VMs Azure Premium-Speicher migrieren."

Bei der Migration von vorhandenen Azure VMs beenden Sie die VM bereiten Sie VHDs entsprechend dem gewünschten VHD vor und kopieren Sie die VHD-Datei mit AzCopy oder PowerShell.

Die VM muss vollständig auf einen sauberen Zustand zu migrieren. Nach Abschluss die Migration wird eine Ausfallzeit sein.

#### <a name="step-1-prepare-vhds-for-migration"></a>Schritt 1. Vorbereiten von Festplatten für die migration

Wenn Sie vorhandene Azure VMs Premium Speicher migrieren, möglicherweise Ihre VHD:

- Allgemeine Betriebssystem-image
- Eine eindeutige Betriebssystem-CD
- Datenträger

Im folgenden gehen wir diese 3 Szenarien für die virtuelle Festplatte vorbereiten.

##### <a name="use-a-generalized-operating-system-vhd-to-create-multiple-vm-instances"></a>Verwenden Sie ein generalisiertes Betriebssystem VHD mehrere VM-Instanzen erstellen

Wenn Sie eine virtuelle Festplatte hochladen, mit der mehrere generische Azure VM-Instanzen erstellen, müssen Sie zuerst VHD mithilfe eines Dienstprogramms Sysprep verallgemeinern. Dies gilt für eine virtuelle Festplatte, die lokal oder in der Cloud. Sysprep entfernt computerspezifische Informationen aus der virtuellen Festplatte.

>[AZURE.IMPORTANT] Eine Momentaufnahme oder VM vor verallgemeinern sie sichern. Ausführen von Sysprep wird beendet und die VM-Instanz freigeben. Befolgen Sie die nachfolgenden Schritte Sysprep eine virtuelle Festplatte Windows-Betriebssystem. Beachten Sie, dass den Befehl Sysprep ausführen Sie den virtuellen Computer herunterfahren muss. Weitere Informationen zu Sysprep finden Sie unter [Übersicht Sysprep](http://technet.microsoft.com/library/hh825209.aspx) oder [Technische Referenz zu Sysprep](http://technet.microsoft.com/library/cc766049.aspx).

1. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator.
2. Geben Sie den folgenden Befehl Sysprep öffnen:

        %windir%\system32\sysprep\sysprep.exe

3. In das Systemvorbereitungsprogramm select System geben Sie Out-of-Box-Experience (OOBE) verallgemeinern das Kontrollkästchen wählen Sie **Herunterfahren,**und klicken Sie auf **OK**, wie in der folgenden Abbildung dargestellt. Sysprep generalize Betriebssystem und Herunterfahren des Systems.

    ![][1]

Verwenden Sie für eine VM Ubuntu Virt Sysprep zu identisch. [Sysprep Virt](http://manpages.ubuntu.com/manpages/precise/man1/virt-sysprep.1.html) Weitere Details anzeigen Siehe auch einige open Source [Linux Server Provisioning-Software](http://www.cyberciti.biz/tips/server-provisioning-software.html) für Linux-Betriebssystemen.

##### <a name="use-a-unique-operating-system-vhd-to-create-a-single-vm-instance"></a>Verwenden Sie eindeutige Betriebssystem VHD eine VM-Instanz erstellen

Haben Sie eine Anwendung auf dem virtuellen Computer erfordert bestimmte Computerdaten verallgemeinern Sie die virtuelle Festplatte nicht. Eine virtuelle Festplatte nicht verallgemeinert kann zum Erstellen einer eindeutigen Azure VM-Instanz verwendet werden. Beispielsweise haben Sie Domänencontroller auf Ihre VHD machen Sysprep Ausführen als Domänencontroller unwirksam es. Überprüfen Sie auf der VM und die Auswirkung von Sysprep vor der Verallgemeinerung könnte die virtuelle Festplatte ausgeführt Applications.

##### <a name="register-data-disk-vhd"></a>Datenträger VHD registrieren

Haben Sie Datenträger in Azure migriert werden, müssen Sie sicherstellen, dass Herunterfahren der VMs, die diesen Datenträger verwendet.

Folgen Sie den Schritten unten VHD in Azure Premium-Speicher kopieren und als bereitgestellte Datenträger registrieren.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Schritt 2. Das Ziel für die virtuelle Festplatte erstellen

Erstellen Sie ein Speicherkonto für die Verwaltung der virtuellen Festplatten. Speicherort die virtuellen Festplatten Planung sollten berücksichtigen Sie Folgendes:

- Das Speicherkonto Premium-Ziel.
- Konto Speicherort muss wie Storage Premium kann Azure VMs in der letzten Phase erstellten. Sie können neue Speicherkonto oder verwenden das gleiche Speicherkonto Ihren Bedürfnissen entsprechend zu kopieren.
- Kopieren und Speichern von Konto Speicherschlüssel Ziel Speicherkonto für die nächste Phase.

Für Datenträger können zu einigen Datenträger im standardspeicherkonto (z. B. Festplatten, die Kühler Storage) empfehlen wir Ihnen alle Daten für die Arbeitslast Premium-Speicher verwendet.

#### <a name="copy-vhd-with-azcopy-or-powershell"></a>Schritt 3. Virtuelle Festplatte mit AzCopy oder PowerShell kopieren

Sie müssen Ihre Container Pfad und Storage-Konto Schlüssel zu diesen beiden Optionen. Container Pfad und Storage-kontoschlüssel finden Sie in **Azure-Portal** > **Speicher**. Container-URL wird "https://myaccount.blob.core.windows.net/mycontainer/".

##### <a name="option-1-copy-a-vhd-with-azcopy-asynchronous-copy"></a>Option 1: Kopieren einer virtuellen Festplatte mit AzCopy (asynchrone Kopie)

AzCopy verwenden, können Sie einfach die VHD-Datei über das Internet hochladen. Je nach Größe der virtuellen Festplatten kann dies länger dauern. Denken Sie daran, die Speichergrenzwerte Konto Ingress-Ausgang bei Verwendung dieser Option. Einzelheiten finden Sie in [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md) .

1. Herunterladen und Installieren von AzCopy hier: [neueste Version AzCopy](http://aka.ms/downloadazcopy)
2. Öffnen Sie Azure PowerShell, und wechseln Sie zum Ordner, in dem AzCopy installiert ist.
3. Verwenden Sie den folgenden Befehl, kopieren Sie die VHD-Datei "Quelle", "Ziel".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Beispiel:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /Pattern:abc.vhd

    Hier werden eine Beschreibung der Parameter im Befehl AzCopy verwendet:

 - * */Source: * &lt;Quelle&gt;:*** Speicherort des Ordners oder der Speicher Container URL, die VHD-Datei enthält.
 - * */SourceKey: * &lt;Konto Quellschlüssel&gt;:*** Konto Speicherschlüssel des Quellkontos Speicher.
 - * */Dest: * &lt;Ziel&gt;:*** Storage Container URL die virtuellen Festplatte zu kopieren.
 - * */DestKey: * &lt;Dest Konto Schlüssel&gt;:*** Konto Speicherschlüssel Ziel Speicherkontos.
 - * *-Block: * &lt;Dateinamen&gt;:*** Geben Sie den Namen der virtuellen Festplatte kopieren.

Ausführliche AzCopy Werkzeug finden Sie unter [Übertragen von Daten mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

##### <a name="option-2-copy-a-vhd-with-powershell-synchronized-copy"></a>Option 2: Kopieren einer virtuellen Festplatte mit PowerShell (synchronisierte Kopie)

Sie können auch die VHD-Datei mithilfe des PowerShell-Cmdlets Start AzureStorageBlobCopy. Verwenden Sie folgenden Befehl auf Azure PowerShell VHD-Datei kopieren. Ersetzen Sie die Werte in <> mit den entsprechenden Werten aus dem Quell- und Storage-Konto. Zum Verwenden dieses Befehls müssen Sie Behälter Vhds in Ihrem Speicherkonto verfügen. Wenn der Container nicht vorhanden ist, erstellen Sie vor dem Ausführen des Befehls.

    $sourceBlobUri = <source-vhd-uri>

    $sourceContext = New-AzureStorageContext  –StorageAccountName <source-account> -StorageAccountKey <source-account-key>

    $destinationContext = New-AzureStorageContext  –StorageAccountName <dest-account> -StorageAccountKey <dest-account-key>

    Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer <dest-container> -DestBlob <dest-disk-name> -DestContext $destinationContext

Beispiel:

    C:\PS> $sourceBlobUri = "https://sourceaccount.blob.core.windows.net/vhds/myvhd.vhd"

    C:\PS> $sourceContext = New-AzureStorageContext  –StorageAccountName "sourceaccount" -StorageAccountKey "J4zUI9T5b8gvHohkiRg"

    C:\PS> $destinationContext = New-AzureStorageContext  –StorageAccountName "destaccount" -StorageAccountKey "XZTmqSGKUYFSh7zB5"

    C:\PS> Start-AzureStorageBlobCopy -srcUri $sourceBlobUri -SrcContext $sourceContext -DestContainer "vhds" -DestBlob "myvhd.vhd" -DestContext $destinationContext

### <a name="scenario2"></a>Szenario 2: "Ich bin VMs von anderen Plattformen Azure Premium-Speicher migrieren."

Wenn Sie virtuelle Festplatte aus nicht - Azure Cloud-Speicher in Azure migrieren, müssen Sie zuerst die VHD-Datei in einem lokalen Verzeichnis exportieren. Der vollständige Pfad des lokalen Verzeichnisses VHD praktischen Speicherort, und verwenden Sie AzCopy zum Azure-Speicher hochgeladen.

#### <a name="step-1-export-vhd-to-a-local-directory"></a>Schritt 1. Exportieren von VHD-Datei in ein lokales Verzeichnis

##### <a name="copy-a-vhd-from-aws"></a>Eine virtuelle Festplatte aus AWS kopieren

1. Bei Verwendung von AWS exportieren Sie EC2 Instanz in eine virtuelle Festplatte in eine Amazon S3-Bucket. Befolgen Sie die Schritte in der Dokumentation Amazon exportieren Amazon EC2 Instanzen zu Amazon EC2 Befehlszeilenschnittstelle (CLI) Tool-Instanz-Export-Task erstellen Befehl EC2 Instanz in eine VHD-Datei exportieren. Achten Sie darauf, dass Sie **virtuelle Festplatte** für den Datenträger #95, Bild & #95 verwenden. FORMAT Variable beim Ausführen des Befehls **Instanz-Export-Task erstellen** . Die exportierte VHD-Datei wird im Amazon S3-Bucket gespeichert werden dabei.

        aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT \
        --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX

2. S3-Bucket VHD-Datei herunterladen. Wählen Sie die VHD-Datei dann **Aktionen** > **herunterladen**.

    ![][3]

##### <a name="copy-a-vhd-from-other-non-azure-cloud"></a>Kopieren einer virtuellen Festplatte aus anderen nicht Azure cloud

Wenn Sie virtuelle Festplatte aus nicht - Azure Cloud-Speicher in Azure migrieren, müssen Sie zuerst die VHD-Datei in einem lokalen Verzeichnis exportieren. Kopieren Sie den vollständigen Quellpfad des lokalen Verzeichnisses VHD Speicherort.

##### <a name="copy-a-vhd-from-on-premise"></a>Eine virtuelle Festplatte aus lokalen kopieren

Wenn Sie virtuelle Festplatte aus einer lokalen Umgebung migrieren, benötigen Sie den vollständigen Pfad der virtuellen Festplatte Speicherort. Der Quellpfad könnte eine Serverfreigabe Position oder Datei.

#### <a name="step-2-create-the-destination-for-your-vhd"></a>Schritt 2. Das Ziel für die virtuelle Festplatte erstellen

Erstellen Sie ein Speicherkonto für die Verwaltung der virtuellen Festplatten. Speicherort die virtuellen Festplatten Planung sollten berücksichtigen Sie Folgendes:

- Das Zielkonto Speicher konnte standard oder Premium je nach anwendungsanforderung werden.
- Bereich Konto Speicher muss wie Storage Premium kann Azure VMs in der letzten Phase erstellten. Sie können neue Speicherkonto oder verwenden das gleiche Speicherkonto Ihren Bedürfnissen entsprechend zu kopieren.
- Kopieren und Speichern von Konto Speicherschlüssel Ziel Speicherkonto für die nächste Phase.

Wir empfehlen Sie alle Daten für die Arbeitslast Premium-Speicher verwendet.

#### <a name="step-3-upload-the-vhd-to-azure-storage"></a>Schritt 3. Die virtuelle Festplatte Azure-Speicher hochgeladen

Jetzt haben Sie die virtuelle Festplatte im lokalen Verzeichnis können AzCopy oder AzurePowerShell Sie die VHD-Datei Azure-Speicher hochgeladen. Beide Optionen werden hier bereitgestellt.

##### <a name="option-1-using-azure-powershell-add-azurevhd-to-upload-the-vhd-file"></a>Option 1: Verwendung von Azure PowerShell Add-AzureVhd die VHD-Datei

    Add-AzureVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo>

Beispiel <Uri> möglicherweise **_"https://storagesample.blob.core.windows.net/mycontainer/blob1.vhd"_**. Beispiel <FileInfo> möglicherweise **_"C:\path\to\upload.vhd"_**.

##### <a name="option-2-using-azcopy-to-upload-the-vhd-file"></a>Option 2: Mithilfe von AzCopy die VHD-Datei

AzCopy verwenden, können Sie einfach die VHD-Datei über das Internet hochladen. Je nach Größe der virtuellen Festplatten kann dies länger dauern. Denken Sie daran, die Speichergrenzwerte Konto Ingress-Ausgang bei Verwendung dieser Option. Einzelheiten finden Sie in [Azure Storage Skalierbarkeits- und Performance-Ziele](storage-scalability-targets.md) .

1. Herunterladen und Installieren von AzCopy hier: [neueste Version AzCopy](http://aka.ms/downloadazcopy)
2. Öffnen Sie Azure PowerShell, und wechseln Sie zum Ordner, in dem AzCopy installiert ist.
3. Verwenden Sie den folgenden Befehl, kopieren Sie die VHD-Datei "Quelle", "Ziel".

        AzCopy /Source: <source> /SourceKey: <source-account-key> /Dest: <destination> /DestKey: <dest-account-key> /BlobType:page /Pattern: <file-name>

    Beispiel:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /SourceKey:key1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /DestKey:key2 /BlobType:page /Pattern:abc.vhd

    Hier werden eine Beschreibung der Parameter im Befehl AzCopy verwendet:

 - * */Source: * &lt;Quelle&gt;:*** Speicherort des Ordners oder der Speicher Container URL, die VHD-Datei enthält.
 - * */SourceKey: * &lt;Konto Quellschlüssel&gt;:*** Konto Speicherschlüssel des Quellkontos Speicher.
 - * */Dest: * &lt;Ziel&gt;:*** Storage Container URL die virtuellen Festplatte zu kopieren.
 - * */DestKey: * &lt;Dest Konto Schlüssel&gt;:*** Konto Speicherschlüssel Ziel Speicherkontos.
 - **/BlobType: Seite:** Gibt an, dass das Ziel ein Seitenblob.
 - * *-Block: * &lt;Dateinamen&gt;:*** Geben Sie den Namen der virtuellen Festplatte kopieren.

Ausführliche AzCopy Werkzeug finden Sie unter [Übertragen von Daten mit AzCopy Befehlszeilenprogramm](storage-use-azcopy.md).

##### <a name="other-options-for-uploading-a-vhd"></a>Weitere Optionen für das Hochladen von einer virtuellen Festplatte

Außerdem können Sie eine virtuelle Festplatte an das Speicherkonto mithilfe einer der folgenden Arten:

- [Azure-Speicher kopieren Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure-Speicher-Explorer hochladen Blobs](https://azurestorageexplorer.codeplex.com/)
- [Storage Import/Export Service REST-API-Referenz](https://msdn.microsoft.com/library/dn529096.aspx)

>[AZURE.NOTE] Wir empfehlen Import/Export-Dienst vorkalkulierten Uploaden Zeit länger als 7 Tage ist. [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) können Sie die Zeit von Größe und übertragen Daten schätzen.
>
> Import/Export kann ein standard Speicherkonto kopieren verwendet werden. Sie müssen aus Standardspeicher Premium Storage-Konto mit einem Tool wie AzCopy kopieren.


## <a name="create-azure-virtual-machine-using-premium-storage"></a>Erstellen von Azure VMs mit Premium-Speicher

Die VHD-Datei hochgeladen oder in die gewünschte Speicherkonto kopiert, führen Sie die Schritte in diesem Abschnitt die virtuelle Festplatte als Betriebssystemabbild oder Betriebssystemdatenträger je nach Szenario registrieren und erstellen Sie eine VM-Instanz aus. Datenträger VHD kann die VM zugeordnet werden, erstellt werden. Ein Beispielskript Migration erfolgt am Ende dieses Abschnitts. Dieses einfache Skript entspricht nicht alle Szenarien. Sie müssen das Skript mit Szenario aktualisieren. Wenn dieses Szenario gilt, finden Sie unter [Ein Beispielskript Migration](#a-sample-migration-script).

### <a name="checklist"></a>Prüfliste

1.  Warten Sie, bis alle VHD Datenträger kopieren ist abgeschlossen.
2.  Stellen Sie sicher, dass Premium-Speicher verfügbar ist, zu dem Sie migrieren.
3.  Entscheiden Sie, die neue VM-Serie, die Sie verwenden. Sollte ein Premium-Speicher kann und die Größe sollte je nach Verfügbarkeit in der Region und Ihren Bedürfnissen entsprechend.
4.  Entscheiden Sie die genaue Größe VM verwenden Sie. Virtueller Speicher muss groß genug für die Anzahl der Datenträger unterstützen Sie. Z.b. haben Sie 4 Datenträger muss die VM 2 oder mehr Kerne. Bedenken Sie auch, Leistung, Arbeitsspeicher und Netzwerkbandbreite erforderlich.
5.  Erstellen Sie ein Speicherkonto Premium in den Zielbereich. Dies ist das Konto, das Sie für den neuen virtuellen Computer.
6.  Haben Sie die aktuelle VM Details praktisch, einschließlich der Liste der Datenträger und die zugehörigen VHD-Blobs.

Bereiten Sie Ihre Anwendung für Ausfallzeiten. Hierzu eine saubere Migration müssen Sie die Verarbeitung im aktuellen System beenden. Nur erhalten sie konsistenten Zustand Sie die neue Plattform zu migrieren. Ausfallzeit hängt die Datenmenge Laufwerke migrieren.

>[AZURE.NOTE] Beim Erstellen einer Azure Ressourcenmanager VM aus einer speziellen VHD Festplatten finden Sie in [dieser Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) für Ressourcenmanager VM mit vorhandenen Datenträger bereitstellen.

### <a name="register-your-vhd"></a>Registrieren der virtuellen Festplatte

Eine VM von OS VHD erstellen oder einen neuen virtuellen Computer einen Datenträger hinzufügen, müssen Sie sie registrieren. Befolgen Sie die nachfolgenden Schritte, je nach der virtuellen Festplatte.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Allgemeine Betriebssystem VHD mehrere Azure VM-Instanzen erstellen

Nach allgemeine Betriebssystemabbild VHD das Speicherkonto hochgeladen wird, registrieren Sie als **Azure VM Image** , damit Sie eine oder mehrere VM-Instanzen erstellen können. Verwenden Sie die folgenden PowerShell-Cmdlets Ihre VHD als eine Azure VM-Betriebssystemabbild. Geben Sie die kompletten URL in die virtuelle Festplatte kopiert wurde an.

    Add-AzureVMImage -ImageName "OSImageName" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osimage.vhd" -OS Windows

Kopieren Sie und speichern Sie den Namen dieses neuen Azure VM-Image. Im obigen Beispiel ist es *OSImageName*.

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Eindeutige Betriebssystem VHD eine Azure VM-Instanz erstellen

Nach der eindeutigen OS VHD Storage-Konto hochgeladen, als **Azure Betriebssystemdatenträger** registrieren, damit eine VM-Instanz davon erstellen können. Verwenden Sie die PowerShell-Cmdlets Ihre VHD als Azure OS Festplatte. Geben Sie die kompletten URL in die virtuelle Festplatte kopiert wurde an.

    Add-AzureDisk -DiskName "OSDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd" -Label "My OS Disk" -OS "Windows"

Kopieren Sie und speichern Sie diese OS Azure. Im obigen Beispiel ist es *OSDisk*.

#### <a name="data-disk-vhd-to-be-attached-to-new-azure-vm-instances"></a>Daten Datenträger VHD neue Azure VM-Instanzen an

Nachdem der Datenträger VHD Speicherkonto hochgeladen wurde, als registrieren Sie ein Azure-Datenträger, damit der neuen DS-Serie DSv2 Serie oder GS-Serie Azure VM-Instanz angefügt werden können.

Verwenden Sie die PowerShell-Cmdlets Ihre VHD als eine Azure-Datenträger. Geben Sie die kompletten URL in die virtuelle Festplatte kopiert wurde an.

    Add-AzureDisk -DiskName "DataDisk" -MediaLocation "https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk.vhd" -Label "My Data Disk"

Kopieren Sie und speichern Sie diese Daten Azure. Im obigen Beispiel ist es *DataDisk*.

### <a name="create-a-premium-storage-capable-vm"></a>Erstellen einer Premium-Speicher kann VM

Einmal das Betriebssystemabbild oder Betriebssystemdatenträger registriert, neue DS-Serie, DSv2-Serie und GS-Serie VM erstellen. Sie werden Betriebssystemabbild oder Datenträger Betriebssystemname, die Registrierung verwenden. Wählen Sie die VM aus der Premium-Speicherebene. Im folgenden Beispiel verwenden wir die *Standard_DS2* VM-Größe.

>[AZURE.NOTE] Aktualisiert die Größe um sicherzustellen, dass Ihre Kapazität und Leistung und verfügbaren Azure Datenträgergrößen entspricht.

Führen Sie die Schritt-PowerShell-Cmdlets um den neuen virtuellen Computer erstellen. Zunächst richten Sie die allgemeinen Parameter

    $serviceName = "yourVM"
    $location = "location-name" (e.g., West US)
    $vmSize ="Standard_DS2"
    $adminUser = "youradmin"
    $adminPassword = "yourpassword"
    $vmName ="yourVM"
    $vmSize = "Standard_DS2"

Erstellen Sie zunächst einen Cloud-Dienst, in dem Sie neue VMs hosten.

    New-AzureService -ServiceName $serviceName -Location $location

Erstellen Sie dann je nach Szenario Azure VM-Instanz vom Betriebssystemabbild oder Betriebssystem-Datenträger, die Sie registriert.

#### <a name="generalized-operating-system-vhd-to-create-multiple-azure-vm-instances"></a>Allgemeine Betriebssystem VHD mehrere Azure VM-Instanzen erstellen

Erstellen Sie die eine oder mehrere neuen DS Serie Azure VM Instanzen mithilfe des **Betriebssystemabbilds Azure** , die Sie registriert. Namen dieses Betriebssystemabbild in die VM-Konfiguration beim neuen virtuellen Computer erstellen, wie unten dargestellt.

    $OSImage = Get-AzureVMImage –ImageName "OSImageName"

    $vm = New-AzureVMConfig -Name $vmName –InstanceSize $vmSize -ImageName $OSImage.ImageName

    Add-AzureProvisioningConfig -Windows –AdminUserName $adminUser -Password $adminPassword –VM $vm

    New-AzureVM -ServiceName $serviceName -VM $vm

#### <a name="unique-operating-system-vhd-to-create-a-single-azure-vm-instance"></a>Eindeutige Betriebssystem VHD eine Azure VM-Instanz erstellen

Erstellen Sie eine neue DS Serie Azure VM-Instanz die **Betriebssystemdatenträger Azure** , die Sie registriert. Namen dieses Betriebssystem-Datenträger in der VM-Konfiguration beim neuen VM erstellen, wie unten dargestellt.

    $OSDisk = Get-AzureDisk –DiskName "OSDisk"

    $vm = New-AzureVMConfig -Name $vmName -InstanceSize $vmSize -DiskName $OSDisk.DiskName

    New-AzureVM -ServiceName $serviceName –VM $vm

Geben Sie andere Informationen Azure VM Clouddienst, Region, Speicherkonto, Verfügbarkeit festlegen und Cacherichtlinie. Beachten Sie, dass die VM-Instanz muss mit Betriebssystems oder Datenträger, das ausgewählten Cloud Service, Region und Storage-Konto in demselben Speicherort wie die zugrunde liegende VHDs dieser Datenträger werden muss.

### <a name="attach-data-disk"></a>Legen Sie Datenträger

Schließlich Wenn Registrierung Datenträger VHDs fügen Sie die neue Premium-Speicher kann Azure VM bei.

Verwenden Sie folgende PowerShell-Cmdlet Datenträger zur neuen VM und die Cacherichtlinie angeben. Im folgenden Beispiel wird die Cacherichtlinie *ReadOnly*festgelegt.

    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName

    Add-AzureDataDisk -ImportFrom -DiskName "DataDisk" -LUN 0 –HostCaching ReadOnly –VM $vm

    Update-AzureVM  -VM $vm

>[AZURE.NOTE] Möglicherweise zusätzliche Schritte erforderlich, die Anwendung unterstützen nicht Gegenstand dieses Handbuchs.

### <a name="checking-and-plan-backup"></a>Überprüfen und Planen der Sicherung

Sobald die neue VM ausgeführt wird, Zugriff, mit demselben Benutzernamen und Kennwort als die ursprünglichen virtuellen Computer und überprüfen, ob alles wie erwartet funktioniert. Alle Einstellungen, einschließlich der Stripesetdatenträger wäre in neuen VM vorhanden.

Der letzte Schritt ist Sicherungsplan und Wartungszeitplan für den neuen virtuellen Computer basierend auf der Anwendung benötigt.

### <a name="a-sample-migration-script"></a>Ein Beispielskript für die migration

Haben Sie mehrere VMs migrieren, werden Automatisierung über PowerShell-Skripts. Es folgt ein Beispielskript, das die Migration einer VM automatisiert. Hinweis unten Skript ist nur ein Beispiel, und es gibt einige Annahmen über die aktuelle VM-Datenträger. Sie müssen das Skript mit Szenario aktualisieren.

Annahmen sind:

- Klassische Azure VMs erstellen.
- Der Quelldatenträger OS und Quelldatenträger werden demselben Speicherkonto und demselben Container. Wenn die Betriebssystem-Datenträger und Datenträger nicht in der gleichen Stelle, können AzCopy oder Azure PowerShell Sie VHDs Speicherkonten und Container kopieren. Siehe vorherigen Schritt: [Kopieren virtuelle Festplatte mit AzCopy oder PowerShell](#copy-vhd-with-azcopy-or-powershell). Dieses Szenario zu bearbeiten ist eine andere Auswahl empfehlen wir AzCopy oder PowerShell ist einfacher und schneller

Automatisierungsskript finden Sie weiter unten. Ersetzen Sie Text durch eigene Informationen und aktualisieren Sie das Skript auf Ihr spezielles Szenario entsprechen.

>[AZURE.NOTE] Das vorhandene Skript wird die Netzwerkkonfiguration Ihrer Datenquelle VM nicht beibehalten. Sie benötigen Re-Konfiguration die Einstellungen für die migrierten VMs.

    <#
    .Synopsis
    This script is provided as an EXAMPLE to show how to migrate a VM from a standard storage account to a premium storage account. You can customize it according to your specific requirements.

    .Description
    The script will copy the vhds (page blobs) of the source VM to the new storage account.
    And then it will create a new VM from these copied vhds based on the inputs that you specified for the new VM.
    You can modify the script to satisfy your specific requirement, but please be aware of the items specified
    in the Terms of Use section.

    .Terms of Use
    Copyright © 2015 Microsoft Corporation.  All rights reserved.

    THIS CODE AND ANY ASSOCIATED INFORMATION ARE PROVIDED “AS IS” WITHOUT WARRANTY OF ANY KIND,
    EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY
    AND/OR FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE RISK OF USE, INABILITY TO USE, OR
    RESULTS FROM THE USE OF THIS CODE REMAINS WITH THE USER.

    .Example (Save this script as Migrate-AzureVM.ps1)

    .\Migrate-AzureVM.ps1 -SourceServiceName CurrentServiceName -SourceVMName CurrentVMName –DestStorageAccount newpremiumstorageaccount -DestServiceName NewServiceName -DestVMName NewDSVMName -DestVMSize "Standard_DS2" –Location “Southeast Asia”

    .Link
    To find more information about how to set up Azure PowerShell, refer to the following links.
    http://azure.microsoft.com/documentation/articles/powershell-install-configure/
    http://azure.microsoft.com/documentation/articles/storage-powershell-guide-full/
    http://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/

    #>

    Param(
    # the cloud service name of the VM.
    [Parameter(Mandatory = $true)]
    [string] $SourceServiceName,

    # The VM name to copy.
    [Parameter(Mandatory = $true)]
    [String] $SourceVMName,

    # The destination storage account name.
    [Parameter(Mandatory = $true)]
    [String] $DestStorageAccount,

    # The destination cloud service name
    [Parameter(Mandatory = $true)]
    [String] $DestServiceName,

    # the destination vm name
    [Parameter(Mandatory = $true)]
    [String] $DestVMName,

    # the destination vm size
    [Parameter(Mandatory = $true)]
    [String] $DestVMSize,

    # the location of destination VM.
    [Parameter(Mandatory = $true)]
    [string] $Location,

    # whether or not to copy the os disk, the default is only copy data disks
    [Parameter(Mandatory = $false)]
    [Bool] $DataDiskOnly = $true,

    # how frequently to report the copy status in sceconds
    [Parameter(Mandatory = $false)]
    [Int32] $CopyStatusReportInterval = 15,

    # the name suffix to add to new created disks to avoid conflict with source disk names
    [Parameter(Mandatory = $false)]
    [String]$DiskNameSuffix = "-prem"

    ) #end param

    #######################################################################
    #  Verify Azure PowerShell module and version
    #######################################################################

    #import the Azure PowerShell module
    Write-Host "`n[WORKITEM] - Importing Azure PowerShell module" -ForegroundColor Yellow
    $azureModule = Import-Module Azure -PassThru

    if ($azureModule -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    else
    {
        #show module not found interaction and bail out
        Write-Host "[ERROR] - PowerShell module not found. Exiting." -ForegroundColor Red
        Exit
    }


    #Check the Azure PowerShell module version
    Write-Host "`n[WORKITEM] - Checking Azure PowerShell module verion" -ForegroundColor Yellow
    If ($azureModule.Version -ge (New-Object System.Version -ArgumentList "0.8.14"))
    {
        Write-Host "`tSuccess" -ForegroundColor Green
    }
    Else
    {
        Write-Host "[ERROR] - Azure PowerShell module must be version 0.8.14 or higher. Exiting." -ForegroundColor Red
        Exit
    }

    #Check if there is an azure subscription set up in PowerShell
    Write-Host "`n[WORKITEM] - Checking Azure Subscription" -ForegroundColor Yellow
    $currentSubs = Get-AzureSubscription -Current
    if ($currentSubs -ne $null)
    {
        Write-Host "`tSuccess" -ForegroundColor Green
        Write-Host "`tYour current azure subscription in PowerShell is $($currentSubs.SubscriptionName)." -ForegroundColor Green
    }
    else
    {
        Write-Host "[ERROR] - There is no valid Azure subscription found in PowerShell. Please refer to this article http://azure.microsoft.com/documentation/articles/powershell-install-configure/ to connect an Azure subscription. Exiting." -ForegroundColor Red
        Exit
    }


    #######################################################################
    #  Check if the VM is shut down
    #  Stopping the VM is a required step so that the file system is consistent when you do the copy operation.
    #  Azure does not support live migration at this time..
    #######################################################################

    if (($sourceVM = Get-AzureVM –ServiceName $SourceServiceName –Name $SourceVMName) -eq $null)
    {
        Write-Host "[ERROR] - The source VM doesn't exist in the current subscription. Exiting." -ForegroundColor Red
        Exit
    }

    # check if VM is shut down
    if ( $sourceVM.Status -notmatch "Stopped" )
    {
        Write-Host "[Warning] - Stopping the VM is a required step so that the file system is consistent when you do the copy operation. Azure does not support live migration at this time. If you’d like to create a VM from a generalized image, sys-prep the Virtual Machine before stopping it." -ForegroundColor Yellow
        $ContinueAnswer = Read-Host "`n`tDo you wish to stop $SourceVMName now? Input 'N' if you want to shut down the VM manually and come back later.(Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $sourceVM | Stop-AzureVM

        # wait until the VM is shut down
        $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        while ($VMStatus -notmatch "Stopped")
        {
            Write-Host "`n[Status] - Waiting VM $vmName to shut down" -ForegroundColor Green
            Sleep -Seconds 5
            $VMStatus = (Get-AzureVM –ServiceName $SourceServiceName –Name $vmName).Status
        }
    }

    # exporting the sourve vm to a configuration file, you can restore the original VM by importing this config file
    # see more information for Import-AzureVM
    $workingDir = (Get-Location).Path
    $vmConfigurationPath = $env:HOMEPATH + "\VM-" + $SourceVMName + ".xml"
    Write-Host "`n[WORKITEM] - Exporting VM configuration to $vmConfigurationPath" -ForegroundColor Yellow
    $exportRe = $sourceVM | Export-AzureVM -Path $vmConfigurationPath


    #######################################################################
    #  Copy the vhds of the source vm
    #  You can choose to copy all disks including os and data disks by specifying the
    #  parameter -DataDiskOnly to be $false. The default is to copy only data disk vhds
    #  and the new VM will boot from the original os disk.
    #######################################################################

    $sourceOSDisk = $sourceVM.VM.OSVirtualHardDisk
    $sourceDataDisks = $sourceVM.VM.DataVirtualHardDisks

    # Get source storage account information, not considering the data disks and os disks are in different accounts
    $sourceStorageAccountName = $sourceOSDisk.MediaLink.Host -split "\." | select -First 1
    $sourceStorageKey = (Get-AzureStorageKey -StorageAccountName $sourceStorageAccountName).Primary
    $sourceContext = New-AzureStorageContext –StorageAccountName $sourceStorageAccountName -StorageAccountKey $sourceStorageKey

    # Create destination context
    $destStorageKey = (Get-AzureStorageKey -StorageAccountName $DestStorageAccount).Primary
    $destContext = New-AzureStorageContext –StorageAccountName $DestStorageAccount -StorageAccountKey $destStorageKey

    # Create a container of vhds if it doesn't exist
    if ((Get-AzureStorageContainer -Context $destContext -Name vhds -ErrorAction SilentlyContinue) -eq $null)
    {
        Write-Host "`n[WORKITEM] - Creating a container vhds in the destination storage account." -ForegroundColor Yellow
        New-AzureStorageContainer -Context $destContext -Name vhds
    }


    $allDisksToCopy = $sourceDataDisks
    # check if need to copy os disk
    $sourceOSVHD = $sourceOSDisk.MediaLink.Segments[2]
    if ($DataDiskOnly)
    {
        # copy data disks only, this option requires deleting the source VM so that dest VM can boot
        # from the same vhd blob.
        $ContinueAnswer = Read-Host "`n`t[Warning] You chose to copy data disks only. Moving VM requires removing the original VM (the disks and backing vhd files will NOT be deleted) so that the new VM can boot from the same vhd. This is an irreversible action. Do you wish to proceed right now? (Y/N)"
        If ($ContinueAnswer -ne "Y") { Write-Host "`n Exiting." -ForegroundColor Red;Exit }
        $destOSVHD = Get-AzureStorageBlob -Blob $sourceOSVHD -Container vhds -Context $sourceContext
        Write-Host "`n[WORKITEM] - Removing the original VM (the vhd files are NOT deleted)." -ForegroundColor Yellow
        Remove-AzureVM -Name $SourceVMName -ServiceName $SourceServiceName

        Write-Host "`n[WORKITEM] - Waiting utill the OS disk is released by source VM. This may take up to several minutes."
        $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        while ($diskAttachedTo -ne $null)
        {
            Start-Sleep -Seconds 10
            $diskAttachedTo = (Get-AzureDisk -DiskName $sourceOSDisk.DiskName).AttachedTo
        }

    }
    else
    {
        # copy the os disk vhd
        Write-Host "`n[WORKITEM] - Starting copying os disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $allDisksToCopy += @($sourceOSDisk)
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $sourceOSVHD -DestContainer vhds -DestBlob $sourceOSVHD -Context $sourceContext -DestContext $destContext -Force
        $destOSVHD = $targetBlob
    }


    # Copy all data disk vhds
    # Start all async copy requests in parallel.
    foreach($disk in $sourceDataDisks)
    {
        $blobName = $disk.MediaLink.Segments[2]
        # copy all data disks
        Write-Host "`n[WORKITEM] - Starting copying data disk $($disk.DiskName) at $(get-date)." -ForegroundColor Yellow
        $targetBlob = Start-AzureStorageBlobCopy -SrcContainer vhds -SrcBlob $blobName -DestContainer vhds -DestBlob $blobName -Context $sourceContext -DestContext $destContext -Force
        # update the media link to point to the target blob link
        $disk.MediaLink = $targetBlob.ICloudBlob.Uri.AbsoluteUri
    }

    # Wait until all vhd files are copied.
    $diskComplete = @()
    do
    {
        Write-Host "`n[WORKITEM] - Waiting for all disk copy to complete. Checking status every $CopyStatusReportInterval seconds." -ForegroundColor Yellow
        # check status every 30 seconds
        Sleep -Seconds $CopyStatusReportInterval
        foreach ( $disk in $allDisksToCopy)
        {
            if ($diskComplete -contains $disk)
            {
                Continue
            }
            $blobName = $disk.MediaLink.Segments[2]
            $copyState = Get-AzureStorageBlobCopyState -Blob $blobName -Container vhds -Context $destContext
            if ($copyState.Status -eq "Success")
            {
                Write-Host "`n[Status] - Success for disk copy $($disk.DiskName) at $($copyState.CompletionTime)" -ForegroundColor Green
                $diskComplete += $disk
            }
            else
            {
                if ($copyState.TotalBytes -gt 0)
                {
                    $percent = ($copyState.BytesCopied / $copyState.TotalBytes) * 100
                    Write-Host "`n[Status] - $('{0:N2}' -f $percent)% Complete for disk copy $($disk.DiskName)" -ForegroundColor Green
                }
            }
        }
    }
    while($diskComplete.Count -lt $allDisksToCopy.Count)

    #######################################################################
    #  Create a new vm
    #  the new VM can be created from the copied disks or the original os disk.
    #  You can ddd your own logic here to satisfy your specific requirements of the vm.
    #######################################################################

    # Create a VM from the existing os disk
    if ($DataDiskOnly)
    {
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $sourceOSDisk.DiskName
    }
    else
    {
        $newOSDisk = Add-AzureDisk -OS $sourceOSDisk.OS -DiskName ($sourceOSDisk.DiskName + $DiskNameSuffix) -MediaLocation $destOSVHD.ICloudBlob.Uri.AbsoluteUri
        $vm = New-AzureVMConfig -Name $DestVMName -InstanceSize $DestVMSize -DiskName $newOSDisk.DiskName
    }
    # Attached the copied data disks to the new VM
    foreach ($dataDisk in $sourceDataDisks)
    {
        # add -DiskLabel $dataDisk.DiskLabel if there are labels for disks of the source vm
        $diskLabel = "drive" + $dataDisk.Lun
        $vm | Add-AzureDataDisk -ImportFrom -DiskLabel $diskLabel -LUN $dataDisk.Lun -MediaLocation $dataDisk.MediaLink
    }

    # Edit this if you want to add more custimization to the new VM
    # $vm | Add-AzureEndpoint -Protocol tcp -LocalPort 443 -PublicPort 443 -Name 'HTTPs'
    # $vm | Set-AzureSubnet "PubSubnet","PrivSubnet"

    New-AzureVM -ServiceName $DestServiceName -VMs $vm -Location $Location

#### <a name="optimization"></a>Optimierung

Aktuelle VM-Konfiguration kann angepasst werden ausdrücklich mit standardmäßigen Festplatten arbeiten. Beispielsweise zur Leistungssteigerung mit vielen Datenträger in einem Stripesetvolume. Z. B. anstatt 4 Datenträger separat auf Premium-Speicher, die Kosten optimieren mit einer Festplatte möglicherweise. Optimierungen wie diese müssen regelmäßig von Fall zu Fall behandelt werden und benutzerdefinierte Schritte nach der Migration. Beachten Sie, dass dieser Vorgang nicht gut funktionieren für Datenbanken und Programme, die das Layout in der Einrichtung definiert abhängig.

##### <a name="preparation"></a>Vorbereitung

1.  Führen Sie die einfache Migration wie im vorigen Abschnitt beschrieben. Optimierungen werden nach der Migration auf dem neuen virtuellen Computer ausgeführt.
2.  Definieren Sie die neue Datenträgergröße für optimierte Konfiguration erforderlich.
3.  Festlegen Sie Zuordnung aktuelle Datenträger/Volumes der neuen Datenträger Spezifikationen

##### <a name="execution-steps"></a>Ausführungsschritte

1.  Erstellen Sie neue Datenträger die richtigen Größen Premium Speicher VM.
2.  Melden Sie die VM und die Daten vom aktuellen Volume auf die neue Festplatte, die das Volume zugeordnet ist. Hierzu für alle aktuellen Volumes, die auf einer neuen Festplatte zuordnen.
3.  Ändern der Einstellungen auf neuen Festplatten wechseln, und Trennen der alten Volumes.

Die Anwendung Festplatte die Leistung optimieren, finden Sie [Anwendungsleistung optimieren](storage-premium-storage-performance.md#optimizing-application-performance).

### <a name="application-migrations"></a>Anwendungsmigrationen

Datenbanken und andere komplexe Programme erfordern spezielle Schritte gemäß der Anwendungsanbieter für die Migration. Finden Sie in der Dokumentation zur jeweiligen Anwendung. Z.b. Normalerweise können Datenbanken migriert werden, durch Sicherung und Wiederherstellung.

## <a name="next-steps"></a>Nächste Schritte

Finden Sie in folgenden Ressourcen für bestimmte Szenarien für die Migration von virtuellen Maschinen:

- [Migrieren von Azure virtuellen Maschinen zwischen Speicherkonten](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
- [Erstellen Sie und Hochladen Sie der virtuellen Festplatte Server Windows Azure.](../virtual-machines/virtual-machines-windows-classic-createupload-vhd.md)
- [Erstellen und Hochladen einer virtuellen Festplatte mit Linux-Betriebssystem](../virtual-machines/virtual-machines-linux-classic-create-upload-vhd.md)
- [Migrieren von virtuellen Maschinen von Amazon AWS Microsoft Azure](http://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Beachten Sie auch die folgenden Ressourcen Weitere Informationen zu Azure Storage und Azure virtuelle Computer:

- [Azure-Speicher](https://azure.microsoft.com/documentation/services/storage/)
- [Azure Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
- [Premium-Speicher: Hochleistungsspeicher für Azure Virtual Machine-Arbeitslasten](storage-premium-storage.md)

[1]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[2]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-1.png
[3]:./media/storage-migration-to-premium-storage/migration-to-premium-storage-3.png
[4]: http://technet.microsoft.com/library/hh831739.aspx
