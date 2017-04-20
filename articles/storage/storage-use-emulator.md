<properties 
    pageTitle="Verwenden Sie den Azure-Speicheremulator für Entwicklung und Testing | Microsoft Azure" 
    description="Azure Speicheremulator bietet eine kostenlose lokale Entwicklungsumgebung für die Entwicklung und Tests mit Azure-Speicher. Erfahren Sie mehr über Speicheremulator einschließlich wie Anfragen authentifiziert werden, mit der Anwendung eine Verbindung zu den Emulator und zum Verwenden des Befehlszeilenprogramms." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Verwenden Sie den Azure-Speicheremulator für Entwicklung und Testing

## <a name="overview"></a>Übersicht

Der Speicheremulator Microsoft Azure bietet eine Umgebung, die Dienste Azure Blob, Warteschlange und Tabelle zu Entwicklungszwecken emuliert. Speicheremulator verwenden, können Sie Ihre Anwendung mit der Speicherdienste lokal testen, ohne Azure-Abonnement erstellen oder Kosten. Wenn Sie zufrieden, wie die Anwendung im Emulator arbeitet, können Sie mit einem Konto Azure-Speicher in der Cloud.

> [AZURE.NOTE] Der Speicheremulator steht als Teil des [Microsoft Azure SDK](https://azure.microsoft.com/downloads/). Sie können auch mit dem [eigenständigen Installer](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)Speicheremulator. Konfigurieren den Speicheremulator benötigen Sie Administratorrechte auf dem Computer.
> 
> Der Speicheremulator wird derzeit nur unter Windows ausgeführt.
>  
> Beachten Sie, dass Daten in einer Speicheremulator nicht unbedingt verfügbar sein, wenn eine andere Version. Möchten Sie Ihre Daten langfristig beizubehalten, sollten speichern Daten in Azure Storage-Konto nicht in der Speicheremulator.

## <a name="how-the-storage-emulator-works"></a>Funktionsweise der Speicheremulator
 
Der Speicheremulator verwendet eine lokale Instanz von Microsoft SQL Server und im lokalen Dateisystem Azure-Speicherdienste zu emulieren. Der Speicheremulator verwendet eine Datenbank in Microsoft SQL Server 2012 Express LocalDB.  Sie können den Speicheremulator Zugriff auf eine lokale Instanz von SQL Server nicht LocalDB-Instanz konfigurieren. Weitere Informationen finden Sie unter [Starten und Initialisieren der Speicheremulator](#start-and-initialize-the-storage-emulator) .

Sie können SQL Server Management Studio Express LocalDB-Installation verwalten. Speicheremulator herstellt LocalDB mit Windows-Authentifizierung zu SQL Server. 

Einige Funktionen Unterschiede zwischen Speicheremulator und Azure Storage Services. Weitere Informationen zu diesen Unterschieden finden Sie unter [Unterschiede zwischen der Speicheremulator und Azure-Speicher](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Authentifizieren gegen Speicheremulator

Wie bei Azure-Speicher in der Cloud, muss jede Anforderung, die Sie gegen Speicheremulator stellen authentifiziert werden, sei eine anonyme Anforderung. Sie können mit der Shared Key-Authentifizierung Speicheremulator oder mit SAS (SAS) authentifizieren.

### <a name="authentication-with-shared-key-credentials"></a>Authentifizierung mit gemeinsamem Schlüssel

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [Azure Storage-Verbindungszeichenfolgen konfigurieren](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Authentifizierung mit einem SAS 

Einige Clientbibliotheken Azure-Speicher wie Xamarin Bibliothek unterstützen nur Authentifizierung mit einem gemeinsamen Zugriff Signaturtoken (SAS). Sie müssen zum Erstellen dieses SAS-Tokens ein Tool oder eine Anwendung, die Shared Key-Authentifizierung unterstützt. Eine einfache Möglichkeit zum Generieren des SAS-Tokens ist über Azure PowerShell:

1. Installieren Sie Azure PowerShell, wenn nicht bereits geschehen. Die neueste Version von Azure PowerShell-Cmdlets wird empfohlen. Informationen Sie [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md#Install) Installationshinweise.

2. Öffnen Sie Azure PowerShell zu, und führen Sie die folgenden Befehle. *ACCOUNT_NAME* ersetzen merken und *ACCOUNT_KEY ==* mit Ihren eigenen Anmeldeinformationen. Ersetzen Sie *CONTAINER_NAME* durch einen Namen Ihrer Wahl.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Die resultierende SAS URI für den neuen Container sollte ähnlich der folgenden:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Mit diesem Beispiel erstellte SAS beträgt einen Tag. Die Signatur gewährt Vollzugriff (Lesen, schreiben, löschen, Liste) BLOBs innerhalb des Containers.

Weitere Informationen zu SAS finden Sie [Mithilfe von gemeinsamen Zugriff Signaturen (SAS)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Starten und Initialisieren des Speicheremulators

Um den Azure-Speicher-Emulator zu starten, klicken Sie auf Start oder drücken Sie Windows. Geben Sie **Azure Speicheremulator**, und wählen Sie den Emulator aus der Liste der Programme. 

Wenn der Emulator ausgeführt wird, sehen Sie ein Symbol im Infobereich Windows-Taskleiste.

Beginn der Speicheremulator wird ein Befehlszeilenfenster angezeigt. Sie können diese Befehlszeilenfenster zu starten und Beenden des Speicheremulators Daten verwenden, aktuellen Status und initialisieren Emulator. Weitere Informationen finden Sie unter [Speicher Emulator Line Tool Reference](#storage-emulator-command-line-tool-reference).

Wenn die Befehlszeile geschlossen wird Speicheremulator weiterhin ausgeführt. Um die Befehlszeile erneut aufzurufen, die obigen Schritte wie wenn Speicheremulator.

Beim ersten Ausführen den Speicheremulator wird zur lokalen Storage-Umgebung initialisiert. Der Initialisierungsprozess erstellt eine Datenbank in LocalDB und HTTP-Ports für jeden Dienst lokaler Speicher reserviert. 

Der Speicheremulator ist standardmäßig in das Verzeichnis C:\Program Files (x86) \Microsoft SDKs\Azure\Storage Emulator installiert. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Initialisieren des Speicheremulators um eine andere SQL-Datenbank verwenden

Speicher-Emulator-Befehlszeilentool können Sie Speicheremulator auf einer SQL-Datenbankinstanz als Standardinstanz LocalDB initialisieren. Beachten Sie, dass Sie das Befehlszeilenprogramm mit Administratorrechten Back-End-Datenbank für Speicheremulator initialisieren müssen ausgeführt werden:

1. Klicken Sie auf die Schaltfläche **Start** oder drücken Sie die **Windows** -Taste. Geben Sie `Azure Storage Emulator` , und wählen sie erscheint es Storage Emulator-Befehlszeilentool bringen.
2. Geben Sie im Eingabeaufforderungsfenster den folgenden Befehl ein, wobei `<SQLServerInstance>` ist der Name der SQL Server-Instanz. LocalDb verwenden, geben `(localdb)\v11.0` als SQL Server-Instanz.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Sie können auch folgenden Befehl leitet den Emulator, SQL Server-Standardinstanz verwenden:

        AzureStorageEmulator init /server .\\ 

    Oder den folgenden Befehl erneut die Datenbank mit der Standardinstanz LocalDB initialisiert verwenden:

        AzureStorageEmulator init /forceCreate 

Weitere Informationen zu diesen Befehlen finden Sie unter [Speicher Emulator Line Tool Reference](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Ressourcen in der Speicheremulator Adressierung

Die Dienstendpunkte Speicheremulator unterscheiden sich von denen eines Kontos Azure-Speicher. Der Unterschied besteht, der lokale Computer Auflösung des Domänennamens, Endpunkte Emulator Storage benötigen Sie eine lokale Adresse statt eines Domänennamens nicht durchgeführt wird.

Beim Adressieren einer Ressource in einem Konto Azure-Speicher verwenden Sie das folgende Schema, in dem der Kontoname Teil der URI-Hostname und Ressourcen behandelt Teil des URL-Pfads:

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Die folgende URI ist z. B. eine gültige Adresse für ein Blob in Azure Storage-Konto:

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Da der lokale Computer keine Auflösung des Domänennamens, ausführen ist der Kontoname in der Speicheremulator Teil des URL-Pfads anstelle des Hostnamens. Das folgende Schema wird für eine Ressource im Speicheremulator verwenden:

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Beispielsweise kann die folgende Adresse für den Zugriff auf ein Blob in Speicheremulator verwendet werden:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Die Dienstendpunkte Speicheremulator sind:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Adressierung mit RA-GRS sekundären Konto

Ab Version 3.1 unterstützt das Emulator Speicherkonto Lesezugriff Geo redundante Replikation (RA-GRS). Für Speicherressourcen in der Cloud und im lokalen Emulator können Sie den sekundären Standort zugreifen von Anhängen - auf den Kontonamen. Beispielsweise kann die folgende Adresse für den Zugriff auf ein Blob mit Hilfe des sekundären schreibgeschützt in Speicheremulator verwendet werden:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Verwenden Sie für den programmgesteuerten Zugriff auf die sekundäre Speicheremulator Speicher-Clientbibliothek für .NET Version 3.2 oder höher. [Microsoft Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) Details anzeigen

## <a name="storage-emulator-command-line-tool-reference"></a>Speicher-Emulator Befehlszeilentool Verweis

Ab Version 3.0 Starten des Speicheremulators, wird ein Befehlszeilenfenster Popup angezeigt. Verwenden Sie das Befehlszeilenfenster starten und beenden sowie den Emulator Status Abfragen und andere Vorgänge ausführen.

> [AZURE.NOTE] Microsoft Azure compute Emulator installiert haben, erscheint ein Symbol im Systemfeld Starten des Speicheremulators. Mit der rechten Maustaste auf das Symbol, um ein Menü bereit zum Starten und Beenden der Speicheremulator grafisch anzuzeigen.

### <a name="command-line-syntax"></a>Befehlszeilensyntax

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Optionen

Geben Sie zum Anzeigen der Liste der Optionen `/help` in der Befehlszeile.

| Option | Beschreibung                                                    | Befehl                                                                                                 | Argumente                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Starten**  | Der Speicheremulator startet.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-Produktion*: Emulator im aktuellen Prozess anstatt einen neuen Prozess starten.                          |
| **Beenden**   | Den Speicheremulator beendet.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Status** | Gibt den Status der Speicheremulator.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Löschen**  | Löscht die Daten in alle Dienste in der Befehlszeile angegeben. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *BLOB*: Löscht BLOB-Daten. <br/>*Warteschlange*: Daten aus der Warteschlange gelöscht. <br/>*Tabelle*: Daten löscht. <br/>*Alle*: alle Daten aller Dienste. |
| **Init**   | Führt die einmalige Initialisierung Emulator einrichten.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-Server ServerName\instanceName*: Gibt den Server an, die SQL-Instanz hostet. <br/>*Sqlinstance - Instanzname*: Gibt den Namen der Instanz SQL Server-Standardinstanz verwendet werden. <br/>*-Forcecreate*: Erzwingt das Erstellen der SQL-Datenbank, auch wenn er bereits vorhanden ist. <br/>*-Produktion*: führt die Initialisierung im aktuellen Prozess statt einen neuen Prozess starten. Sie müssen den aktuellen Prozess mit erhöhten Berechtigungen um Initialisierung starten.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Unterschiede zwischen der Speicheremulator und Azure-Speicher

Da Speicheremulator einer emulierten Umgebung in einer lokalen SQL-Instanz ausgeführt wird, sind einige Unterschiede bezüglich Funktionalität der Emulator und Azure Speicherkonto in der Cloud:

- Der Speicheremulator unterstützt nur einen festen Konto und bekannten Authentifizierungsschlüssel.

- Der Speicheremulator ist kein Dienst skalierbaren Speicher und unterstützen eine große Anzahl von Clients gleichzeitig.

- Wie beschrieben in den [Ressourcen der Speicheremulator Adressierung](#addressing-resources-in-the-storage-emulator)werden Ressourcen anders Speicheremulator im Vergleich zu Azure Speicherkonto behandelt. Dieser Unterschied ist darauf zurückzuführen, dass Auflösung des Domänennamens steht in der Cloud, aber nicht auf dem lokalen Computer.

- Ab Version 3.1 unterstützt das Emulator Speicherkonto Lesezugriff Geo redundante Replikation (RA-GRS). Im Emulator alle Konten RA-GRS aktiviert haben, und gibt nie Verzögerung zwischen primären und sekundären Replikaten. Operationen Get Blob Service Stats, Get Warteschlange Service Stats und Get Tabelle Service Stats auf sekundären Konto unterstützt und gibt immer den Wert der `LastSyncTime` Antwortelement als die aktuelle Zeit nach der SQL-Datenbank.

    Verwenden Sie für den programmgesteuerten Zugriff auf die sekundäre Speicheremulator Speicher-Clientbibliothek für .NET Version 3.2 oder höher. [Microsoft Azure Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) Details anzeigen

- File Service und SMB-Protokoll-Endpunkte werden in der Speicheremulator derzeit nicht unterstützt.

- Der Speicheremulator Fehlermeldung einen VersionNotSupportedByEmulator (HTTP-Statuscode 400 - Bad Request) verwenden Sie eine Version von Storage-Services, die noch nicht von der Version des Emulators unterstützt wird, das Sie verwenden.

### <a name="differences-for-blob-storage"></a>Unterschiede für BLOB-Speicher 

BLOB-Speicher im Emulator gelten die folgenden Unterschiede:

- Speicher-Emulator nur unterstützt Blob Größe bis zu 2 GB.

- Speichern von BLOB-Vorgang möglicherweise gegen ein Blob, das in der Speicheremulator und verfügt über eine aktive Lease erfolgreich, auch wenn Lease-ID als Teil der Anforderung angegeben wurde. 

- Fügen Sie Blob vom Emulator auszuführen. Versucht, einen Vorgang für ein Blob Append Fehlermeldung einen FeatureNotSupportedByEmulator (HTTP-Statuscode 400 - Bad Request).

### <a name="differences-for-table-storage"></a>Unterschiede für Tabellenspeicher 

Tabellenspeicher im Emulator gelten die folgenden Unterschiede:

- Datumseigenschaften in der Tabelle Service im Speicheremulator unterstützen nur die von SQL Server 2005 unterstützt (*d.*h. sie müssen nach dem 1. Januar 1753 liegen). Alle Datumsangaben vor January 1, 1753 werden in diesen Wert geändert. Die Genauigkeit der Daten ist auf die Genauigkeit des SQL Server 2005, d.h. Daten auf 1/300 Sekunden.

- Der Speicheremulator unterstützt Partition Schlüssel und Zeile Haupteigenschaftswerte mit weniger als 512 Bytes. Außerdem darf die Gesamtgröße der Kontoname, Tabellennamen und Schlüsseleigenschaft Namen zusammen 900 Byte nicht überschreiten.

- Die Größe einer Zeile in einer Tabelle in der Speicheremulator beträgt weniger als 1 MB.

- Der Speicheremulator Eigenschaften geben `Edm.Guid` oder `Edm.Binary` unterstützen nur die `Equal (eq)` und `NotEqual (ne)` Vergleichsoperatoren in Abfrage filtern Zeichenfolgen.

### <a name="differences-for-queue-storage"></a>Unterschiede für Warteschlangenspeicher

Es bestehen keine Unterschiede für Warteschlangenspeicher im Emulator.

## <a name="storage-emulator-release-notes"></a>Speicher-Emulator-Versionsinformationen

### <a name="version-45"></a>Version 4.5

- Ein Fehler, der Initialisierung und Installation der Speicheremulator fehlschlägt, wenn die zugrunde liegende Datenbank umbenannt verursacht.

### <a name="version-44"></a>Version 4.4

- Speicheremulator unterstützt nun Version 2015 12 11 von Speicher-Services auf BLOB-, Warteschlange, und Endpunkte.

- Der Speicheremulator Garbagecollection von BLOB-Daten ist nun effizienter Umgang mit Blobs.

- Behoben, die verursacht Container ACL XML anders aus wie der Speicherdienst wird validiert werden.

- Ein Fehler, der Max und min DateTime-Werte zurückgegeben werden in falsche Zeitzone manchmal.

### <a name="version-43"></a>Version 4.3

- Speicheremulator unterstützt nun Version 2015-07-08 von Speicher-Services auf BLOB-, Warteschlange, und Endpunkte.

### <a name="version-42"></a>Version 4.2

- Speicheremulator unterstützt nun Version 2015-04-05 von Speicher-Services auf BLOB-, Warteschlange, und Endpunkte.

### <a name="version-41"></a>Version 4.1

- Der Speicheremulator unterstützt nun Version 2015 02 21 Speicherdienste Dienstendpunkte BLOBs, Warteschlangen und Tabelle mit Ausnahme der neuen Anhängen BLOB-Funktionen. 

- Der Speicheremulator gibt jetzt eine aussagekräftige Fehlermeldung verwenden Sie eine Version von Storage-Services, die von dieser Version des Emulators noch nicht unterstützt wird. Es wird empfohlen, die neueste Version des Emulators. Wenn Sie ein VersionNotSupportedByEmulator (HTTP-Statuscode 400 - Bad Request) Fehler, laden Sie die neueste Version der Speicheremulator.

- Ein Fehler bei einer Race verursachte Entitätsdaten zu falschen bei gleichzeitiger Zusammenführungsoperationen.

### <a name="version-40"></a>Version 4.0

- Ausführbare Speicheremulator wurde in *AzureStorageEmulator.exe*umbenannt.

### <a name="version-32"></a>Version 3.2

- Speicheremulator unterstützt nun Version 2014-02-14 von Speicher-Services auf BLOB-, Warteschlange, und Endpunkte. Beachten Sie, dass die Datei Dienstendpunkte Speicheremulator derzeit nicht unterstützt werden. Informationen zu Version 2014-02-14 finden Sie unter [Versionskontrolle für Azure Storage Services](https://msdn.microsoft.com/library/azure/dd894041.aspx) .

### <a name="version-31"></a>Version 3.1

- Lesezugriff Geo redundanten Speicher (RA-GRS) ist jetzt Speicheremulator unterstützt. Die Get Blob Service Stats, Get Warteschlange Service Stats erhalten Tabelle Service Stats APIs für sekundäre Konto unterstützt und gibt immer den Wert der LastSyncTime-Antwortelement so nach SQL-Datenbank zurück. Verwenden Sie für den programmgesteuerten Zugriff auf die sekundäre Speicheremulator Speicher-Clientbibliothek für .NET Version 3.2 oder höher. Einzelheiten finden Sie unter Microsoft Azure Storage-Clientbibliothek for .NET Reference.

### <a name="version-30"></a>Version 3.0

- Azure Speicheremulator wird nicht mehr im gleichen Paket wie Serveremulator geliefert.

- Speicher-Emulator-Benutzeroberfläche wird für skriptfähige Befehlszeilenschnittstelle veraltet. Ausführliche Informationen über die Befehlszeile finden Sie unter Speicher Emulator Line Tool Reference. Die Benutzeroberfläche wird weiterhin in Version 3.0 vorhanden, aber es kann nur zugegriffen werden, wenn der Serveremulator installiert ist, indem Sie mit der rechten Maustaste auf das Taskleistensymbol und Storage-Emulator UI anzeigen auswählen.

- Version 2013-08-15 der Azure-Speicherdienste ist jetzt vollständig unterstützt. (Zuvor wurde diese Version nur unterstützt von Speicheremulator Version 2.2.1 Vorschau.)
