<properties
    pageTitle="BLOB-Speicher Datenübertragung mit importieren/exportieren | Microsoft Azure"
    description="Informationen Sie zum Import erstellen und Aufträge im klassischen Azure-Portal zur Datenübertragung BLOB-Speicher exportieren."
    authors="renashahmsft"
    manager="aungoo"
    editor="tysonn"
    services="storage"
    documentationCenter=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="renash"/>


# <a name="use-the-microsoft-azure-importexport-service-to-transfer-data-to-blob-storage"></a>Verwenden des Import/Export Microsoft Azure Datenübertragung BLOB-Speicher

## <a name="overview"></a>Übersicht

Azure Import/Export-Dienst ermöglicht sichere Datenmengen Azure BLOB-Speicher übertragen von Festplatten ein Azure-Rechenzentrum Versand. Sie können auch diesen Dienst Datenübertragung von Azure BLOB-Speicher, Festplatten und Versenden Ihrer lokalen Site. Dieser Dienst ist in Situationen sollen mehrere TB Daten aus bzw. in Azure, wobei ein Upload oder Download über das Netzwerk nicht möglich aufgrund begrenzter Bandbreite oder hohe Netzwerkkosten ist geeignet.

Der Dienst muss Festplatten BitLocker für die Sicherheit Ihrer Daten verschlüsselt werden soll. Der Dienst unterstützt classic Speicherkonten in allen Bereichen von öffentlichen Azure. Liefern Ihnen Festplatten eines der unterstützten in diesem Artikel angegeben.

In diesem Artikel erfahren Sie mehr über Azure Import/Export-Dienst und Laufwerke Kopieren von Azure BLOB-Speicher Daten liefern.

> [AZURE.IMPORTANT] Erstellen und verwalten importieren, und Aufträge für klassische Speicher mit klassischen Portal oder [Import/Export-Dienst REST-APIs](http://go.microsoft.com/fwlink/?LinkID=329099)zu exportieren. Ressourcenmanager Speicherkonten werden zurzeit nicht unterstützt.

## <a name="when-should-i-use-the-azure-importexport-service"></a>Wann sollte den Azure Import/Export-Dienst verwenden?

Sie können in Betracht Azure Import/Export-Dienst beim Hochladen oder Herunterladen von Daten über das Netzwerk zu langsam ist oder zusätzliche Netzwerkbandbreite ist kostspielig.

Dieser Dienst können in Szenarien wie:

- Migrieren von Daten in der Cloud: große Datenmengen in Azure schnell und kostengünstig.
- Content-Verteilung: Daten schnell an Ihre Kunden senden.
- : Dauern Sicherungskopien Ihrer lokalen Daten in Azure BLOB-Speicher.
- Daten-Recovery: Wiederherstellen große Datenmengen im BLOB-Speicher gespeichert, bis auf Ihrem lokalen Standort.

## <a name="pre-requisites"></a>Erforderliche Komponenten

In diesem Abschnitt haben wir die erforderlichen Komponenten zum Verwenden dieses Diensts erforderlich aufgeführt. Überprüfen sie sorgfältig vor der Auslieferung Ihrer Laufwerke.

### <a name="storage-account"></a>Konto

Ein Azure-Abonnement und mindestens eine **klassische** Speicherkonten mit dem Import/Export-Dienst benötigt. Jeder Auftrag verwendet Daten aus nur einem klassischen Speicherkonto übertragen. Anders gesagt kann kein einzelnen Import-Export Auftrag mehrere Storage-Konten umfassen. Informationen zum Erstellen eines neuen Speicherkontos finden Sie [ein Speicherkonto erstellen](storage-create-storage-account.md#create-a-storage-account).

### <a name="blob-types"></a>BLOB-Typen

Azure Import/Export-Dienst können Sie Daten auf **Block** -Blobs oder **Seite** Blobs kopieren. Umgekehrt können Sie nur **Block** -Blobs, **Seite** Blobs oder **Append** Blobs von Azure-Speicher mit diesem Dienst exportieren.

### <a name="job"></a>Auftrag

Zum Importieren oder Exportieren von BLOB-Speicher zu beginnen, erstellen Sie zunächst ein Projekt. Ein Auftrag kann ein Importauftrag oder einen Auftrag exportieren:

- Erstellen Sie einen Importauftrag, wenn Daten lokal BLOBs in Ihrem Konto Azure-Speicher übertragen.
- Erstellen Sie ein Projekt exportieren, wenn übertragen Daten als Blobs in Ihrem Speicherkonto, Festplatten, die an Sie geliefert werden.

Beim Erstellen eines Auftrags benachrichtigt Sie, dass Sie Festplatten Azure Rechenzentrum Versand dem Import/Export-Dienst.

- Für einen Importauftrag Versand Sie Festplatten mit den Daten.
- Für ein Projekt exportieren werden Sie leere Festplatten ausgeliefert.
- Sie können bis zu 10 Festplatten pro Auftrag versenden.

Erstellen ein Imports oder Auftrag mit dem [Classic-Portal](https://manage.windowsazure.com/) oder [REST-API von Azure Storage importieren/exportieren](http://go.microsoft.com/fwlink/?LinkID=329099)exportieren.

### <a name="client-tool"></a>Client-tool

Der erste Schritt beim Erstellen eines Auftrags **Importieren** ist Laufwerk geliefert werden für den Import vorbereiten. Vorbereitung auf das Laufwerk müssen auf einem lokalen Server herstellen und Azure Import/Export-Clienttools auf dem lokalen Server ausführen. Dieses Clienttool ermöglicht Kopieren von Daten auf das Laufwerk und die Daten auf dem Laufwerk mit BitLocker verschlüsselt Dateien der Buch.-Blatt generiert.

Die Protokolldateien speichern grundlegende Informationen zum Auftrag und Laufwerk wie Seriennummer und Storage-Kontoname. Diese Erfassung ist nicht auf dem Laufwerk gespeichert. Sie wird beim Import Arbeitsplätzen verwendet. Details zu Arbeitsplätzen werden Schritt für Schritt in diesem Artikel bereitgestellt.

Das Clienttool ist nur kompatibel mit 64-Bit-Windows-Betriebssystem. Siehe Abschnitt [Betriebssystem](#operating-system) für bestimmte Betriebssystemversionen unterstützt.

Downloaden Sie die neueste Version von [Azure Import/Export-Clienttools](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409). Weitere Informationen zur Verwendung von Azure Import/Export Tool finden Sie unter [Referenz Azure importieren/exportieren](http://go.microsoft.com/fwlink/?LinkId=329032).

### <a name="hard-disk-drives"></a>Festplatten

3,5-Zoll SATA II/III interne Festplatten für Import/Export-Dienst unterstützt. Festplatten können bis zu 10TB.
Für Importaufträge wird nur das erste Datenvolume auf dem Laufwerk verarbeitet werden. Das Datenvolume muss mit NTFS formatiert.
Beim Kopieren von Daten auf die Festplatte verbinden direkt mit SATA-Anschluss oder Verbinden mit extern externe SATA-II-III-USB-Adapter. Wir empfehlen eine der folgenden externe SATA-II-III-USB-Adapter:

- Anker 68UPSATAA - 02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Sharkoon QuickPort XT HC

Haben Sie einen Konverter, die oben nicht aufgeführt ist, können Sie versuchen, Azure Import/Export Tool mit der Konverter das Laufwerk vorbereiten und vor dem Kauf unterstützten Konverters funktioniert.

> [AZURE.IMPORTANT] Externer Festplattenlaufwerke, die mit einem integrierten USB-Adapter werden von diesem Dienst nicht unterstützt. Außerdem kann die Festplatte im Gehäuse eine externe Festplatte verwendet werden. Bitte senden Sie externe Festplatten.

### <a name="encryption"></a>Verschlüsselung

Die Daten auf dem Laufwerk müssen mit BitLocker Drive Encryption verschlüsselt werden. Dies schützt Ihre Daten während der Übertragung.

Importaufträge stehen zwei Methoden zur Verschlüsselung. Die erste Möglichkeit ist die Verwendung der / Parameter beim Ausführen des Clienttools während der laufwerksvorbereitung zu verschlüsseln. Die zweite Möglichkeit ist das BitLocker-Verschlüsselung auf dem Laufwerk manuell aktivieren und Chiffrierschlüssel in der Befehlszeile des Client-Tool bei der Aufbereitung Laufwerk angeben.

Exportaufträge nachdem die Daten auf den Laufwerken wird der Dienst das Laufwerk mit BitLocker vor der Auslieferung an Sie verschlüsseln. Der Verschlüsselungsschlüssel erhalten Sie Portal Classic.  

### <a name="operating-system"></a>Betriebssystem

Einer der 64-Bit-Betriebssysteme können Sie die Festplatte vor der Lieferung des Laufwerks in Azure Azure Import/Export Tool mit vorbereiten:

Windows 7 Enterprise, Windows 7 Ultimate Windows 8 Pro Windows 8 Enterprise Windows 8.1 Pro Windows 8.1 Enterprise Windows 10<sup>1</sup>, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2. Diese Betriebssysteme unterstützen BitLocker Drive Encryption.

> [AZURE.IMPORTANT] <sup>1</sup> Wenn Sie Windows 10 Computer Festplatte vorbereiten verwenden, downloaden Sie die neueste Version von Azure Import/Export Tool.

### <a name="locations"></a>Standorte

Kopieren von Daten in und aus allen öffentlichen Azure-Speicherkonten Azure Import/Export-Dienst unterstützt. Sie können Festplatten an folgenden Speicherorten liefern. Ist das Speicherkonto an einem öffentlichen Ort Azure hier nicht angegeben ist, wird beim Erstellen des Auftrags Classic-Portal oder die Import/Export-REST-API eine alternative Versandort bereitgestellt.

Versandorte unterstützt:

- Osten der USA

- Westen der USA

- USA 2 OST

- USA

- Norden der USA – zentral

- Südlichen zentralen USA

- Nordeuropa

- Westeuropa

- Ostasien

- Südostasien

- Australien OST

- Australien Südost

- Japan West

- Japan OST

- Zentrale Indien


### <a name="shipping"></a>Versandkosten

**Versand Laufwerke im Rechenzentrum:**

Beim Erstellen eines Auftrags importieren oder exportieren Sie eine Versandadresse eines der unterstützten Speicherorte, liefern Laufwerke erhalten. Der Lieferanschrift hängt der Speicherort Ihres Speicherkontos kann, es jedoch nicht Ihr Konto Speicherort identisch.

Netzbetreiber wie FedEx, DHL, UPS und US Postal Service können Sie Ihren Laufwerken Lieferadresse geliefert.

**Versand Laufwerke im Rechenzentrum:**

Beim Erstellen eines Auftrags Import- oder Exportvorgang erforderlich Absenderadresse für Microsoft Laufwerke liefern nach Ihrem Projekt abgeschlossen ist. Stellen Sie sicher, dass eine gültige Absenderadresse um Verarbeitung zu vermeiden.

Geben Sie eine gültige Spediteur DHL oder FedEx auch Kontonummer von Microsoft für den Versand der Laufwerke wieder verwendet werden. FedEx Kundennummer ist erforderlich für den Versand von Laufwerken aus den USA und Europa Speicherorten. Eine Kontonummer DHL ist erforderlich für den Versand von Laufwerken von Standorten in Asien und Australien. Ein kann [FedEx](http://www.fedex.com/us/oadr/) (für USA und Europa) [DHL](http://www.dhl.com/) (Asien und Australien) Carrier Konto erstellen oder wenn Sie nicht besitzen. Wenn bereits eine Kontonummer des Spediteurs überprüfen Sie gültig ist.

Versand Pakete, befolgen Sie die Begriffe zu [Microsoft Azure Service](https://azure.microsoft.com/support/legal/services-terms/).

> [AZURE.IMPORTANT] Beachten Sie, dass das physische Medium, die Sie versenden grenzüberschreitenden benötigen. Sie sind dafür verantwortlich, dass Ihre physische Medien und Daten importiert oder den geltenden Gesetzen exportiert. Überprüfen Sie vor der Lieferung das physische Medium, mit Ihrem Berater zu überprüfen, ob Ihre Medien und Daten rechtmäßig identifizierten Rechenzentrum versendet werden können. Dies hilft sicherzustellen, dass Microsoft rechtzeitig erreicht. Beispielsweise benötigt ein Paket, die grenzüberschreitenden wird eine Rechnung mit dem Paket (außer wenn Grenzen innerhalb der Europäischen Union) begleitet werden. Sie können eine Rechnung von Webseite gefüllt drucken. Beispiel für kommerzielle Rechnung sind [DHL Rechnung] (http://invoice-template.com/wp-content/uploads/dhl-commercial-invoice-template.pdf) oder [FedEx Rechnung](http://images.fedex.com/downloads/shared/shipdocuments/blankforms/commercialinvoice.pdf). Stellen Sie sicher, dass Microsoft nicht als Ausführer angegeben wurde.

## <a name="how-does-the-azure-importexport-service-work"></a>Wie funktioniert der Azure Import/Export-Dienst?

Sie können Daten zwischen dem lokalen Standort und Arbeitsplätze und Versand Festplatten Azure-Rechenzentrum Azure Import/Export-Service durch Azure BLOB-Speicher übertragen. Jede Festplatte, die Sie liefern ist einem Projekt zugeordnet. Jedes Projekt ist ein einzelnes Speicherkonto zugeordnet. Überprüfen der [erforderlichen Komponenten Abschnitt](#pre-requisites) sorgfältig über die Besonderheiten dieser Diensttypen unterstützt Blob Datenträgertypen, Speicherorte und Versand.

In diesem Abschnitt wird auf hoher Ebene beschrieben, die Schritte zum Importieren und Exportieren von Aufträgen. Weiter unten im [Abschnitt "Schnellstart"](#quick-start)bieten wir Anleitung zu einer Auftrag exportieren.

### <a name="inside-an-import-job"></a>In ein Projekt importieren

Auf einer hohen Ebene Schritten ein Importauftrag folgenden:

- Bestimmen Sie die zu importierenden Daten und die Anzahl der Laufwerke, die Sie benötigen.
- Identifizieren Sie die Blobs Ziel für die Daten im BLOB-Speicher.
- Verwenden der Azure Import/Export Tool zu Daten eine oder mehrere Festplatten mit BitLocker verschlüsselt.  
- Erstellen Sie einen Importauftrag in Ihrem Ziel klassische Speicherkonto Classic-Portal oder die Import/Export-REST-API verwenden. Verwenden das klassischen Portal hochladen Sie Dateien der Erfassung.
- Geben Sie Adresse und Carrier Kontonummer für Laufwerke an Sie ausgeliefert.
- Die Festplatten während Arbeitsplätzen Lieferadresse geliefert.
- Nachverfolgungsnummer in den Auftragsdetails importieren Lieferung aktualisieren Sie und übermitteln Sie des Importauftrags.
- Laufwerke empfangen und verarbeitet der Azure-Rechenzentrum.
- Laufwerke werden mit Ihrem Konto Carrier gemäß den Importauftrag Absenderadresse versendet.

    ![Abbildung 1:Import auftragsfluss](./media/storage-import-export-service/importjob.png)


### <a name="inside-an-export-job"></a>In einem Projekt exportieren

Auf einer hohen Ebene umfasst ein Auftrag exportieren folgende Schritte:

- Bestimmen Sie die zu exportierenden Daten und die Anzahl der Laufwerke, die Sie benötigen.
- Identifizieren der Quelle Blobs oder Container Pfade der Daten im BLOB-Speicher.
- Erstellen eines exportauftrags im Konto Speicher über Classic-Portal oder die Import/Export-REST-API.
- Quelle Blobs oder Container Pfade Daten exportieren Projekt angeben.
- Bieten Sie die Absenderadresse und Carrier Kontonummer für Laufwerke an Sie ausgeliefert.
- Die Festplatten während Arbeitsplätzen Lieferadresse geliefert.
- Nachverfolgungsnummer in Auftragsdetails Export Lieferung aktualisieren Sie und senden Sie des Exportvorgangs.
- Die Laufwerke empfangen und verarbeitet der Azure-Rechenzentrum.
- Die Laufwerke werden mit BitLocker verschlüsselt. die Schlüssel sind Portal Classic verfügbar.  
- Laufwerke werden mit Ihrem Konto Carrier gemäß den Importauftrag Absenderadresse versendet.

    ![Abbildung 2:Export auftragsfluss](./media/storage-import-export-service/exportjob.png)

### <a name="viewing-your-job-status"></a>Anzeigen des Jobstatus

Sie können den Status des Importvorgangs oder Aufträge aus dem Classic-Portal exportieren. Das Speicherkonto im klassischen Portal und Sie auf dem Register **Import/Export** . Eine Liste von Aufträgen wird auf der Seite angezeigt. Sie können die Liste auf Auftragsstatus, Namen, Einzelvorgangstyp oder Nachverfolgungsnummer filtern.

Sie sehen einen der folgenden Status je nachdem, wo Ihr Laufwerk im Prozess.

| Auftragsstatus   | Beschreibung                                                                                                                                                                                                                                                                                                                                                                        |
|:-------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Erstellen     | Der Auftrag wurde erstellt, aber Ihre Versandinformationen noch nicht bereitgestellt.                                                                                                                                                                                                                                                                                                |
| Versandkosten     | Der Auftrag wurde erstellt und Sie haben Ihre Versandinformationen. **Hinweis**: Wenn das Laufwerk in Azure Rechenzentrum übermittelt wird, kann der Status weiterhin "Shipping" für einige Zeit angezeigt. Nach dem Start des Dienstes Kopieren von Daten ändert sich der Status in "Übertragen". Wenn Sie spezifischere Status der Festplatte anzeigen möchten, können Sie die Import/Export-REST-API. |
| Übertragen von | Ihre Daten werden von der Festplatte (für einen Importauftrag) oder auf der Festplatte (für einen Auftrag exportieren).                                                                                                                                                                                                                                                                 |
| Verpackung    | Die Datenübertragung abgeschlossen ist und die Festplatte ist für den Versand für Sie vorbereitet.                                                                                                                                                                                                                                                                             |
| Abschließen     | Die Festplatte wurde an Sie geliefert.                                                                                                                                                                                                                                                                                                                                      |

### <a name="time-to-process-job"></a>Zeit-Einzelvorgang

Die Zeitspanne dauert ein Import/Export-Auftrag hängt von Faktoren wie der Versand Prozess job Typ, Typ und Größe der kopierten Daten und die Größe der Festplatten bereitgestellt. Der Import/Export-Dienst hat keine SLA. Die REST-API können Sie Arbeit besser verfolgen. Gibt Prozent abgeschlossen Parameter der Liste Aufträge der Kopiervorgang anzeigt. Erreichen Sie uns benötigen Sie geschätzte Zeit wichtige importieren/exportieren abgeschlossen.

### <a name="pricing"></a>Preisgestaltung

**Laufwerk Bearbeitungsgebühr**

Fällt ein Laufwerk Behandlung für jedes Laufwerk als Teil des Importvorgangs verarbeitet oder Auftrag exportieren. Einzelheiten Sie zum [Azure Import/Export-Preise](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Versandkosten**

Wenn Sie Laufwerke in Azure liefern, bezahlen Sie Versandkosten Spediteur. Microsoft gibt die Laufwerke Sie ist die Versandkosten Carrier Rechnung bei Arbeitsplätzen vorgesehen.

**Transaktionskosten**

Es gibt keine Kosten beim Importieren von Daten in den BLOB-Speicher. Standard-Ausgang Gebühren werden Daten aus dem BLOB-Speicher. Weitere Informationen zu Transaktionen finden Sie unter [Daten Verrechnungspreise.](https://azure.microsoft.com/pricing/details/data-transfers/)

## <a name="quick-start"></a>Schnellstart

In diesem Abschnitt stellen wir eine schrittweise Anleitung zum Erstellen ein Import und ein Exportauftrag. Stellen Sie sicher, dass Sie alle [erforderlichen Komponenten](#pre-requisites) vor voran.

## <a name="how-to-create-an-import-job"></a>Wie ein Importauftrag erstellt?

Erstellen Sie einen Importauftrag, um Daten zu Ihrem Konto Azure-Speicher von Festplatten kopieren Versand ein oder mehrere Laufwerke mit Daten im angegebenen Rechenzentrum. Importauftrag vermittelt Informationen Festplattenlaufwerke Daten kopiert, Ziel Speicherkonto und Versandinformationen Azure Import/Export-Dienst. Erstellen eines Importauftrags ist drei Schritte. Zuerst bereiten Sie Ihre Laufwerke Azure Import/Export-Clienttool verwenden. Zweitens senden Sie einen Importauftrag Classic-Portal verwenden. Dritte die Laufwerke während Arbeitsplätzen Lieferadresse geliefert und Versandinformationen in der Auftragsdetails aktualisiert.   

> [AZURE.IMPORTANT] Sie können nur ein Projekt pro Konto senden. Jedes Laufwerk Sie liefern kann ein Speicherkonto importiert werden. Angenommen zwei Speicherkonten Daten importieren möchten. Getrennte Festplatten für jedes Storage-Konto verwenden, und erstellen Sie separate Aufträge pro Konto.

### <a name="prepare-your-drives"></a>Bereiten Sie Ihrer Laufwerke vor

Der erste Schritt beim Importieren von Daten mit dem Azure Import/Export-Dienst ist die Laufwerke mit Azure Import/Export-Clienttool vorbereiten. Gehen Sie Ihre Laufwerke vorbereiten.

1.  Identifizieren Sie die Daten importiert werden sollen. Verzeichnisse und Dateien auf dem lokalen Server oder einer Netzwerkfreigabe eigenständige möglicherweise.  

2.  Bestimmen Sie die Anzahl der Laufwerke je nach Größe der Daten benötigen. Beschaffen Sie die erforderliche Anzahl von Festplatten für 3,5-Zoll-SATA-II/III.

3.  Ziel Speicherkonto, Container, virtuelle Verzeichnisse und Blobs zu identifizieren.

4.  Bestimmen Sie die Verzeichnisse und eigenständige Dateien, die auf jeder Festplatte kopiert werden.

5.  Verwenden Sie [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409) die Daten in eine oder mehrere Festplatten kopieren.

    - Azure Import/Export Tool erstellt Kopiervorgänge um Daten vom Quell-auf Festplatten zu kopieren. In einer einzelnen Kopie kann das Tool ein einzelnes Verzeichnis mit Unterverzeichnissen oder eine einzelne Datei kopieren.

    - Kopieren mehrerer Sitzungen möglicherweise Quelldaten umfasst viele Verzeichnisse.

    - Jede Festplatte bereiten Sie benötigen mindestens einen Kopiervorgang.

6.  Sie können die / Parameter aktivieren Sie Bitlocker-Verschlüsselung auf der Festplatte zu verschlüsseln. Alternativ kann auch Bitlocker-Verschlüsselung auf die Festplatte manuell aktivieren und den Schlüssel während der Ausführung des Tools angeben.

7.  Azure Import/Export Tool generiert eine Journaldatei Laufwerk für jedes Laufwerk bereit ist. Die Journaldatei Laufwerk ist auf dem lokalen Computer nicht auf der Festplatte gespeichert. Sie werden die Journal-Datei hochladen, bei der Erstellung des Importauftrags. Eine Journaldatei Laufwerk enthält die Laufwerk-ID und den BitLocker-Schlüssel sowie andere Informationen über das Laufwerk.
**Wichtig**: jede Festplatte, die Sie vorbereiten führt eine Journaldatei. Beim Erstellen des Importauftrags mit klassischen Portal müssen Sie Journaldateien Laufwerke hochladen, Importauftrag gehören. Laufwerke ohne Erfassung Dateien werden nicht verarbeitet.

8.  Daten auf Festplatten oder die Journal-Datei nach Abschluss der Vorbereitung der Festplatte geändert.

Unten sind die Befehle und Beispiele für die Vorbereitung der Festplatte Azure Import/Export-Clienttool verwenden.

Azure Import/Export Tool PrepImport Clientbefehl für die erste kopiersitzung ein Verzeichnis zu kopieren:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Beispiel:**

Im folgenden Beispiel werden alle Dateien kopiert und Unterverzeichnisse von H:\Video Festplatte auf X: bereitgestellt. Die Daten werden von Speicherschlüssel Konto und in den Speichercontainer namens Video Storage Konto importiert. Wenn die Behälter nicht vorhanden ist, wird sie erstellt. Dieser Befehl wird auch formatieren und die Ziel-Festplatte verschlüsseln.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Video1 /logdir:c:\logs /sk:storageaccountkey /t:x /format /encrypt /srcdir:H:\Video1 /dstdir:video/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Azure Import/Export Tool PrepImport Clientbefehl für nachfolgende Kopiervorgänge auf ein Verzeichnis zu kopieren:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Geben Sie für nachfolgende Kopiervorgänge auf derselben Festplatte dasselbe Journal-Datei und geben Sie, eine neue Sitzung. Es ist nicht erforderlich die Schlüssel und Ziel Festplatte von Konto erneut bereitstellen, formatieren oder das Laufwerk verschlüsselt. In diesem Beispiel werden wir H:\Photo Ordner und seine Unterverzeichnisse zu demselben Ziellaufwerk in der Speichercontainer Namen Foto kopiert.

    WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:Photo /srcdir:H:\Photo /dstdir:photo/ /MetadataFile:c:\WAImportExport\SampleMetadata.txt

Der Azure Import/Export-Clienttool PrepImport Befehl für die erste kopiersitzung eine Datei kopieren:

    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

Azure Import/Export Clienttool PrepImport Befehl für nachfolgende Kopiervorgänge, eine Datei zu kopieren:

    WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /srcfile:<SourceFile> /dstblob:<DestinationBlobPath> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>]

**Denken Sie daran**: Standardmäßig werden die Daten als Block-Blobs importiert. Den /BlobType-Parameter können Sie Daten als Blobs eine Seite importieren. Wenn Sie VHD-Dateien importieren, die als Datenträger auf einer VM Azure bereitgestellt werden, müssen Sie sie beispielsweise als Blobs Seite importieren. Wenn Sie unsicher sind, welche Blob Geben Sie verwenden, können Sie /blobType:auto und wir helfen den richtigen Typ zu bestimmen. In diesem Fall alle Vhd und Vhdx als Seitenblobs importiert und der Rest als Block-Blobs importiert.

Einzelheiten Sie weitere zur Verwendung von Azure Import/Export-Clienttool in [Vorbereiten von Festplatten für den Import](https://msdn.microsoft.com/library/dn529089.aspx).

Lesen Sie auch den [Beispielworkflow vorbereiten Festplatten für einen Importauftrag](https://msdn.microsoft.com/library/dn529097.aspx) ausführliche schrittweise Anleitung.  

### <a name="create-the-import-job"></a>Erstellen des Importauftrags

1.  Sobald Sie Ihr Laufwerk vorbereitet haben, zu navigieren Sie das Speicherkonto im [klassischen Portal](https://manage.windowsazure.com) und das Dashboard. Klicken Sie unter **Kurzer Blick**auf **ein Importauftrag erstellen**. Gehen Sie die Schritte und aktivieren Sie das Kontrollkästchen Festplatte vorbereitet haben und sich die Journaldatei Laufwerk verfügbar.

2.  Geben Sie in Schritt 1 Kontaktinformationen für diesen Importvorgang und eine gültige Absenderadresse Verantwortlichen. Wenn Sie ausführliche Daten für den Importauftrag speichern möchten, das Kontrollkästchen Sie **ausführliche Protokoll in meinem 'Waimportexport' BLOB-Container zu speichern**.

3.  Laden Sie in Schritt2 der Buch.-Dateien, die während der Vorbereitungsschritt abgerufen. Sie müssen für jedes Laufwerk eine Datei hochladen, die Sie erstellt haben.

    ![Erstellen Sie Importauftrags - Schritt 3](./media/storage-import-export-service/import-job-03.png)

4.  Geben Sie in Schritt 3 einen aussagekräftigen Namen für den Importauftrag. Der eingegebene Name enthält möglicherweise nur Kleinbuchstaben, Zahlen, Bindestriche und Unterstriche, muss mit einem Buchstaben beginnen und darf keine Leerzeichen enthalten. Sie verwenden den Namen können Sie Ihre Aufträge verfolgen, während sie ausgeführt und nach Abschluss.

    Wählen Sie anschließend Ihr Data Center Region aus der Liste. Der Datenbereich Center zeigt Rechenzentrum und Adresse, Ihr Paket geliefert werden müssen. Siehe FAQ unter Weitere Informationen.

5.  Schritt 4 Wählen Sie zurückgeben Anbieter aus, und geben Sie die Kontonummer des Spediteurs. Microsoft verwendet dieses Konto liefern Sie Laufwerke nach Abschluss der Importauftrag.

    Haben Sie die laufende Nummer wählen Sie der Spediteurs aus, und geben Sie die laufende Nummer.

    Wenn Sie keine Nachverfolgungsnummer noch, wählen Sie **bieten ich meine Versandinformationen für diesen Importvorgang, nachdem ich mein Paket geliefert haben**, führen Sie den Importvorgang.

6. Um die laufende Nummer einzugeben, nachdem Ihr Paket zurück zu der Seite **Import/Export** für das Speicherkonto im Portal Classic geliefert haben Ihre Aufgabe aus der Liste auswählen und wählen Sie " **Lieferung"**. Navigieren Sie durch den Assistenten, und geben Sie die laufende Nummer in Schritt2.

    Wenn die laufende Nummer nicht innerhalb von 2 Wochen erstellen den Auftrag aktualisiert wird, läuft der Auftrag.

    Ist der Auftrag erstellen, Lieferung oder Übertragung können Sie Ihre Kontonummer Netzbetreiber in Schritt2 des Assistenten aktualisieren. Sobald der Auftrag in der Verpackung Zustand befindet, können Sie die Kontonummer des Spediteurs für dieses Projekt nicht aktualisieren.

7. Sie können den Job-Status auf dem Portal Dashboard verfolgen. Bedeutung jeder Auftragsstatus im vorherigen Abschnitt durch [Anzeigen des Jobstatus](#viewing-your-job-status)anzeigen

## <a name="how-to-create-an-export-job"></a>Erstellen Sie ein Projekt exportieren

Erstellen Sie einen Auftrag exportieren, um dem Import/Export-Dienst benachrichtigen, dass Sie eine oder mehrere leere Laufwerke im Rechenzentrum ausgeliefert werden, damit Daten aus Ihrem Speicherkonto Laufwerke und Laufwerke exportiert dann an Sie geliefert.

### <a name="prepare-your-drives"></a>Bereiten Sie Ihrer Laufwerke vor

Vorab überprüft werden zur Vorbereitung einer Exportvorgangs Laufwerke empfohlen:

1. Überprüfen Sie die Anzahl der Disketten Azure Import/Export Tool PreviewExport Befehl erforderlich. Weitere Informationen finden Sie unter [Vorschau Laufwerk Verbrauch für ein Projekt exportieren](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sie können Laufwerk Verwendung Vorschau für Blobs und die Größe der Laufwerke verwendet werden.

2. Achten Sie auf die Festplatte Schreib-/können, die für den Exportauftrag geliefert wird.

### <a name="create-the-export-job"></a>Erstellen des Exportvorgangs

1.  Erstellen Sie ein Projekt exportieren, das Speicherkonto im [klassischen Portal](https://manage.windowsazure.com)navigieren und das Dashboard anzeigen Klicken Sie unter **Kurzer Blick**auf **Erstellen eines Auftrags exportieren** und mit dem Assistenten fortfahren.

2.  Geben Sie in Schritt2 Kontaktinformationen Verantwortlichen für dieses Projekt exportieren. Wenn Sie ausführliche Daten für den Exportauftrag speichern möchten, das Kontrollkästchen Sie **ausführliche Protokoll in meinem 'Waimportexport' BLOB-Container zu speichern**.

3.  Geben Sie in Schritt 3 die BLOB-Daten aus das Speicherkonto in dem leeren Laufwerk oder Laufwerke exportieren möchten. Können alle BLOB-Daten im Speicherkonto exportieren, oder Sie können angeben, welche blobs oder Sätze von Blobs exportieren.

    Einen Blob exportieren, verwenden Sie **Gleich** Auswahl und die relative Pfadangabe Blob mit den Containernamen angeben. Verwenden Sie *$root* , um den Stammcontainer anzugeben.

    Um alle Blobs mit Präfix angeben, **Beginnt mit** Auswahl verwenden, und geben Sie das Präfix beginnen mit einem Schrägstrich '/'. Das Präfix kann das Präfix den Containernamen, den kompletten Namen oder den kompletten Namen gefolgt von Präfix mit dem blobnamen sein.

    Die folgende Tabelle zeigt Beispiele für gültige Blob-Pfade:

    Auswahl|BLOB-Pfad|Beschreibung
    ---|---|---
    Beginnt mit|/|Exportiert alle Blobs im Speicherkonto
    Beginnt mit|/$root /|Exportiert alle Blobs im Stammcontainer
    Beginnt mit|/Book|Exportiert alle Blobs in jedem Container mit Präfix **Buch**
    Beginnt mit|/Music/|Exportiert alle Blobs im Container **Musik**
    Beginnt mit|Liebe/Musik|Exportiert alle Blobs im Container **Musik** mit Präfix **Liebe**
    Gleich|$root/logo.bmp|Export- **logo.bmp** im Stammcontainer blob
    Gleich|Videos/Story.mp4|Exporte blob **story.mp4** Container **videos**

    Geben Sie wie in dieser Abbildung Blob Pfade in gültige Formate Fehler während der Verarbeitung zu vermeiden.

    ![Exportauftrag - Schritt 3 erstellen](./media/storage-import-export-service/export-job-03.png)


4.  Geben Sie in Schritt 4 einen aussagekräftigen Namen für das Projekt exportieren. Der Namen dürfen nur Kleinbuchstaben, Zahlen, Bindestriche und Unterstriche, muss mit einem Buchstaben beginnen und darf keine Leerzeichen enthalten.

    Center Datenbereich zeigt Rechenzentrum, das Paket geliefert werden müssen. Siehe FAQ unter Weitere Informationen.

5.  Wählen Sie in Schritt 5 zurück Anbieter aus, und geben Sie die Kontonummer des Spediteurs. Microsoft verwendet dieses Konto liefern Sie Laufwerke nach Ihrem Projekt exportieren abgeschlossen ist.

    Haben Sie die laufende Nummer wählen Sie der Spediteurs aus, und geben Sie die laufende Nummer.

    Wenn Sie keine Nachverfolgungsnummer noch, wählen Sie **bieten ich meine Versandinformationen für dieses Projekt exportieren, nachdem ich mein Paket geliefert haben**, führen Sie den Exportvorgang.

6. Um die laufende Nummer einzugeben, nachdem Ihr Paket zurück zu der Seite **Import/Export** für das Speicherkonto im Portal Classic geliefert haben Ihre Aufgabe aus der Liste auswählen und wählen Sie " **Lieferung"**. Navigieren Sie durch den Assistenten, und geben Sie die laufende Nummer in Schritt2.

    Wenn die laufende Nummer nicht innerhalb von 2 Wochen erstellen den Auftrag aktualisiert wird, läuft der Auftrag.

    Ist der Auftrag erstellen, Lieferung oder Übertragung können Sie Ihre Kontonummer Netzbetreiber in Schritt2 des Assistenten aktualisieren. Sobald der Auftrag in der Verpackung Zustand befindet, können Sie die Kontonummer des Spediteurs für dieses Projekt nicht aktualisieren.

    > [AZURE.NOTE] Ist das Blob exportiert werden zum Zeitpunkt des Kopierens von Festplatte, Azure Import/Export-Dienst eine Momentaufnahme des BLOBs und kopieren Sie den Snapshot.

7.  Sie können den Job-Status auf dem Dashboard im klassischen Portal verfolgen. Was jeder Auftragsstatus im vorherigen Abschnitt bedeutet auf "Anzeigen des Jobstatus".

8.  Nach der Laufwerke mit den exportierten Daten Erhalt können Sie anzeigen und kopieren Sie die BitLocker-Schlüssel generiert, durch den Dienst für das Laufwerk. Das Speicherkonto im klassischen Portal und Sie auf dem Register Import/Export. Wählen Sie aus Ihrem Projekt exportieren, und klicken Sie auf Schlüssel anzeigen. Die BitLocker-Schlüssel werden wie folgt angezeigt:

    ![BitLocker-Schlüssel für Exportauftrag anzeigen](.\media\storage-import-export-service\export-job-bitlocker-keys.png)

Finden Sie in den FAQ-Abschnitt wie Fragen, die Kunden bei der Verwendung dieser Dienst auftreten.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen ##


**Wie lange dauert es Daten kopieren die Laufwerken des Rechenzentrums erreicht?**

Das Kopieren variiert abhängig von Faktoren wie Auftragstyp, Typ und Größe der Daten kopiert, Größe der Festplatten bereitgestellt und vorhandene Arbeitslast. Es kann von ein paar Tage ein paar Wochen diese Faktoren variieren. Daher ist es schwierig, eine allgemeine Schätzung. Der Dienst versucht, Ihre Arbeit optimieren, indem Sie mehrere Laufwerke nach Möglichkeit parallel kopieren. Haben Sie eine wichtige Import/Export-Einzelvorgang, erreichen Sie uns für eine Schätzung.

**Wann sollte Azure Import/Export-Dienst verwenden?**
Sollten mit Azure Import Export Upload oder Download über Netzwerk ungefähr abschätzen mehr als 7 Tage dauert. Sie können ausrechnen, wie lange online-Rechner oder im Azure importieren exportieren REST API-Beispiel in Azure Beispiele Repository an [Daten übertragen Geschwindigkeit Rechner](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) [herunterladen](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/archive/master.zip) wird. Dies ist keine exakte Berechnung jedoch nur eine ungefähre Angabe.

**Kann ich ein Speicherkonto Ressourcenmanager Azure Import/Export-Dienst verwenden?**

Nein, nicht möglich Daten zu oder von einem Ressourcenmanager Speicherkonto direkt mit dem Azure Import/Export-Dienst kopieren. Der Dienst unterstützt nur klassische Speicherkonten. Ressourcenmanager Speicher Kundensupport wird bald kommen. Alternativ können Daten in einem klassischen Speicherkonto und Migrieren in der Ressourcen-Manager Speicherkonto mit [AzCopy](storage-use-azcopy.md) oder CopyBlob.

**Kann Azure Dateien Azure Import/Export-Dienst kopieren?**

Nein, unterstützt den Azure Import/Export-Dienst nur Block Blobs und Seitenblobs. Alle Speichertypen einschließlich Azure Dateien, Tabellen und Warteschlangen werden nicht unterstützt.

**Ist der Azure Import/Export-Dienst für CSP-Abonnements?**

Nein, unterstützt der Azure Import/Export-Dienst CSP Abonnements nicht. Die Unterstützung wird in Zukunft hinzugefügt werden.

**Können den Vorbereitung für einen Importauftrag Schritt überspringen oder kann ein Laufwerk ohne vorbereiten?**

Liefern gewünschte für den Datenimport Laufwerk muss vorbereitet werden Azure Import/Export-Clienttools. Das Clienttool verwenden Daten auf das Laufwerk kopieren.

**Muss ich Datenträger Vorbereitung führen Sie beim Erstellen eines Auftrags exportieren?**

Nein, aber einige Vorabprüfungen werden empfohlen. Überprüfen Sie die Anzahl der Disketten Azure Import/Export Tool PreviewExport Befehl erforderlich. Weitere Informationen finden Sie unter [Vorschau Laufwerk Verbrauch für ein Projekt exportieren](https://msdn.microsoft.com/library/azure/dn722414.aspx). Sie können Laufwerk Verwendung Vorschau für Blobs und die Größe der Laufwerke verwendet werden. Überprüfen Sie auch lesen und Schreiben auf die Festplatte, die für den Exportauftrag geliefert wird.

**Was geschieht, wenn ich versehentlich eine Festplatte nicht unterstützten Anforderungen senden?**

Azure-Rechenzentrum zurück Laufwerk, der nicht unterstützten an Sie. Wenn nur einige der Laufwerke im Paket Anforderungen auf Unterstützung dieser Laufwerke verarbeitet und Laufwerke, die nicht die Anforderungen an Sie zurückgesendet.

**Kann ich meinen Auftrag stornieren?**

Sie können einen Auftrag abbrechen, wenn der Status erstellen oder Versand.

**Wie lange kann ich den Status der abgeschlossenen Aufträge im klassischen Portal anzeigen?**

Sie können den Status für abgeschlossene Projekte bis zu 90 Tage anzeigen. Abgeschlossene Aufträge werden nach 90 Tagen gelöscht.

**Was tun kann ich, wenn importieren oder Exportieren von mehr als 10 Laufwerken möchte?**

Eine Import- oder Exportvorgangs kann nur 10 Laufwerken in Druckauftrag für den Import/Export-Dienst. Wenn mehr als 10 Festplatten ausgeliefert werden soll, können Sie mehrere Aufträge erstellen. Laufwerke, die mit demselben Auftrag müssen im gleichen Paket zusammen ausgeliefert werden.

**Kann ich SATA-USB-Adapter verwenden, die nicht in der empfohlenen Liste?**

Haben Sie einen Konverter, die oben nicht aufgeführt ist, können Sie versuchen, Azure Import/Export Tool mit der Konverter das Laufwerk vorbereiten und vor dem Kauf unterstützten Konverters funktioniert.

**Formatieren der Dienst die Laufwerke vor der Rückgabe?**

Nein. Alle Laufwerke werden mit BitLocker verschlüsselt.

**Können Laufwerke für Projekte Importieren/Exportieren von Microsoft werden kaufen?**

Nein. Sie müssen Ihre eigenen Laufwerke für Import und export Jobs.

**Nach Abschluss des Importauftrags ist wie meine Daten im Speicherkonto aussehen? Erhalten Meine Verzeichnishierarchie?**

Beim Vorbereiten einer Festplatte für einen Importauftrag wird durch die /dstdir Ziel angegeben: Parameter. Dies ist der Zielcontainer im Speicherkonto, Daten von der Festplatte kopiert werden. In diesem Zielcontainer virtuelle Verzeichnisse für Ordner von der Festplatte und Blobs für Dateien erstellt werden.

**Verfügt das Laufwerk Dateien, die in meinem Speicherkonto vorhanden, wird der Dienst vorhandenen Blobs in meinem Speicherkonto überschreiben?**

Das Laufwerk vorbereiten, Sie können angeben, ob die Zieldateien überschrieben werden sollen oder ignorierten mit dem Parameter /Disposition aufgerufen: < umbenennen | Nein überschreiben | überschreiben >. Standardmäßig wird der Dienst die neue Dateien anstatt überschreiben vorhandene Blobs.

**Ist Azure Import/Export-Clienttools mit 32-Bit-Betriebssystemen kompatibel?**
Nein. Das Clienttool ist nur mit 64-Bit-Windows-Betriebssystemen kompatibel. Finden Sie eine vollständige Liste der [erforderlichen Komponenten](#pre-requisites) unterstützten Betriebssystemversionen im Abschnitt Operating Systems.

**Sollte nicht auf der Festplatte in meinem Paket enthalten?**

Versenden Sie nur Festplatten. Enthalten Sie Stromkabel oder USB-Kabel nicht.

**Muss ich meine Laufwerke DHL oder FedEx versenden?**

Sie können Laufwerke im Rechenzentrum mit jedem bekannten Träger wie FedEx, DHL, UPS und US Postal Service liefern. Jedoch müssen Sie für den Versand vom Rechenzentrum die Laufwerke zurück, FedEx-Kundennummer in den USA und die EU oder eine Kontonummer DHL in Asien und Australien angeben.

**Gibt es Einschränkungen mit meinem Laufwerk International?**

Beachten Sie, dass das physische Medium, die Sie versenden grenzüberschreitenden benötigen. Sie sind dafür verantwortlich, dass Ihre physische Medien und Daten importiert oder den geltenden Gesetzen exportiert. Überprüfen Sie vor der Lieferung der Speichermedien, mit Ihr zu überprüfen, ob Ihre Medien und Daten rechtmäßig identifizierten Rechenzentrum versendet werden können. Dies hilft sicherzustellen, dass Microsoft rechtzeitig erreicht.

**Beim Erstellen eines Auftrags ist die Adresse sehr Mein Konto Speicherort unterscheidet. Was soll ich tun?**

Alternative Versandorte sind einige Speicherorte Konto zugeordnet. Zuvor können verfügbare Versandorte auch vorübergehend Alternative Speicherorte zugeordnet werden. Überprüfen Sie die Versandadresse während Arbeitsplätzen vor der Auslieferung Ihrer Laufwerke.

**Warum wird mein Auftragsstatus in der klassischen Portal sagen "Shipping" beim Spediteur Website Meine zeigt erfolgt?**

Im klassischen Portal Statuswechsel Versand, wenn die Verarbeitung beginnt Laufwerk übertragen. Wenn Laufwerk erreicht die Anlage wurde nicht verarbeitet, wird der Auftragsstatus als Lieferung angezeigt.

**Bei meinem Laufwerk, fordert der Spediteur Data Center Kontaktname und Telefonnummer. Was soll ich?**

Die Nummer wird bei Arbeitsplätzen bereitgestellt. Benötigen Sie den Namen ein Kontakts, wenden Sie sich an uns unter waimportexport@microsoft.com , und wir werden Ihnen mit diesen Informationen.

**Können ich Azure Import/Export-Dienst in Office 365 PST Postfächer und SharePoint-Daten kopieren?**

Siehe [Importieren PST-Dateien oder Daten zu Office 365 SharePoint](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

**Können ich Azure Import/Export-Dienst Meine Backups offline in Azure Backup Service kopieren?**

Siehe [Offline Backup Workflow in Azure Backup](../backup/backup-azure-backup-import-export.md).

## <a name="see-also"></a>Siehe auch:

- [Einrichten von Azure Import/Export-Clienttool](https://msdn.microsoft.com/library/dn529112.aspx)

- [Datenübertragung mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md)

- [Azure Import Export REST API-Beispiel](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)
