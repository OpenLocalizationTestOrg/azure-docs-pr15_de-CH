<properties
   pageTitle="Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer | Microsoft Azure"
   description="Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="tarcher" />

# <a name="browsing-and-managing-storage-resources-with-server-explorer"></a>Durchsuchen und Verwalten von Speicherressourcen mit Server-Explorer

[AZURE.INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Übersicht
Wenn der Azure-Tools für Microsoft Visual Studio installiert haben, können Sie Blob, Warteschlange und Tabellendaten aus Speicherkonten für Azure anzeigen. Azure Storage Node in Server-Explorer zeigt Daten in das lokale Emulator Speicherkonto und Ihre anderen Konten der Azure-Speicher.

Wählen Sie zum Anzeigen von Server-Explorer in Visual Studio im Menü **Ansicht**, **Server-Explorer**. Storage Node enthält Speicherkonten jede Azure-Abonnement-Zertifikat, mit denen Sie verbunden sind. Das Speicherkonto angezeigt wird, können Sie die Informationen [in diesem Thema](#add-storage-accounts-by-using-server-explorer)hinzufügen.

Azure SDK 2.7 ab, können Sie auch neue Cloud Explorer anzeigen und Verwalten der Azure-Ressourcen. Weitere Informationen finden Sie unter [Verwalten von Azure Ressourcen mit Cloud Explorer](./vs-azure-tools-resources-managing-with-cloud-explorer.md) .


## <a name="view-and-manage-storage-resources-in-visual-studio"></a>Anzeigen und Verwalten von Speicherressourcen in Visual Studio

Server-Explorer zeigt automatisch eine Liste der Blobs, Queues und Tabellen in das Speicherkonto Emulator. Emulator Speicherkonto ist im Server-Explorer unter dem Knoten Speicher als Knoten **Development** aufgeführt.

Wenn Speicher Emulator Konto Ressourcen, erweitern Sie den Knoten **Development** . Wenn der Speicheremulator noch nicht gestartet wurde, erweitern Sie den Knoten **Development** , wird automatisch gestartet. Dies kann einige Sekunden dauern. Sie können weiterhin in anderen Bereichen von Visual Studio Speicheremulator wird gestartet.

Zum Anzeigen von Ressourcen in ein Speicherkonto Knoten Sie das Speicherkonto im Server-Explorer. Die folgenden untergeordneten Knoten angezeigt:

- BLOBs

- Warteschlangen

- Tabellen

## <a name="work-with-blob-resources"></a>BLOB-Ressourcen

Blobs Knoten zeigt eine Liste der Container für das ausgewählte Speicherkonto. BLOB-Container BLOB-Dateien enthalten, und diese Blobs in Ordnern und Unterordnern organisieren. Weitere Informationen finden Sie unter [wie BLOB-Speicher von .NET](./storage/storage-dotnet-how-to-use-blobs.md) .

### <a name="to-create-a-blob-container"></a>Einen BLOB-Container erstellen

1. Öffnen Sie das Kontextmenü für den Knoten **Blobs** , und wählen Sie die **Blob-Container erstellen**.

1. Geben Sie den Namen des neuen Containers im Dialogfeld **Blob-Container erstellen** , und wählen Sie dann **Ok**

    >[AZURE.NOTE] Der Containername Blob muss mit einer Zahl (0-9) oder Kleinbuchstaben (a-Z) beginnen.

### <a name="to-delete-a-blob-container"></a>Einen BLOB-Container löschen

- Öffnen Sie das Kontextmenü für den blobcontainer zu entfernen und dann **Löschen**.

### <a name="to-display-a-list-of-the-items-contained-in-a-blob-container"></a>Eine Liste der Elemente in einem BLOB-Container anzeigen

- Öffnen Sie das Kontextmenü für ein BLOB-Container in der Liste, und wählen Sie **Ansicht BLOB-Container**.

    Wenn Sie einen BLOB-Container anzuzeigen, wird es auf einer Registerkarte als BLOB-Container anzeigen.

    ![VST_SE_BlobDesigner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC749016.png)

    Die folgenden Operationen mit Blobs möglich mithilfe der Schaltflächen oben rechts BLOB-Container anzeigen:

    - Geben Sie einen Filterwert und anwenden

    - Aktualisiert die Liste der Blobs im container

    - Hochladen einer Datei

    - Löschen eines BLOBs

      >[AZURE.NOTE] Löschen einer Datei aus einem BLOB-Container nicht die zugrunde liegende Datei gelöscht; Er wird nur aus dem BLOB-Container entfernt.

    - Öffnen Sie einen blob

    - Einen Blob auf dem lokalen Computer speichern

### <a name="to-create-a-folder-or-subfolder-in-a-blob-container"></a>Erstellen Sie einen Ordner oder Unterordner in einem BLOB-container

1. Wählen Sie den BLOB-Container im Server-Explorer. Wählen Sie im Containerfenster **Blob hochladen** .

    ![Hochladen einer Datei in einem BLOB-Ordner](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766037.png)

1. Klicken Sie im Dialogfeld **Neue Datei hochladen** wählen Sie die Schaltfläche **Durchsuchen** , um die Datei anzugeben, die Sie hochladen möchten, und geben Sie einen Ordnernamen im **Ordner (optional)** .

    Nach demselben Verfahren können Sie Unterordner im Container Ordner hinzufügen. Wenn Sie einen Ordnernamen angeben, wird die Datei auf der obersten Ebene der BLOB-Container hochgeladen. Die Datei wird im angegebenen Ordner im Container.

    ![Ordner mit einem BLOB-Container hinzugefügt](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766038.png)

1. Doppelklicken Sie auf den Ordner, oder drücken Sie die EINGABETASTE, um den Inhalt des Ordners anzuzeigen. Wenn Sie den Container Ordner haben, navigieren Sie das **Übergeordnete Verzeichnis öffnen** (oben) klicken Sie in das Stammverzeichnis des Containers.

### <a name="to-delete-a-container-folder"></a>Um einen Containerordner löschen

 - Löschen Sie alle Dateien im Ordner

    >[AZURE.NOTE] Da Blob-Container Ordner virtuelle Ordner sind, einen leeren Ordner kann nicht erstellt und einen Ordner, löschen Sie dessen Inhalt löschen. Sie müssen den gesamten Inhalt eines Ordners den Ordner löschen Löschen.

### <a name="to-filter-blobs-in-a-container"></a>Blobs in einem Container filtern

Sie können die Blobs filtern, die angezeigt werden, indem Sie ein gemeinsames Präfix angeben.

Beispielsweise geben Sie das Präfix `hello` in Filtertext und dann wählen Sie **Ausführen** (**!**) angezeigt, nur Blobs, die mit 'Hello' beginnen.

![VST_SE_FilterBlobs](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC519076.png)


>[AZURE.NOTE] Filterfeld Groß- und Filtern mit Platzhalterzeichen unterstützt. BLOBs können nur durch Präfix gefiltert werden. Das Präfix kann ein Trennzeichen enthalten, verwenden Sie ein Trennzeichen Blobs in eine virtuelle Hierarchie organisieren. Z. B. das Präfix HelloFabric filtern oder gibt alle Blobs, die mit dieser Zeichenfolge beginnen.

### <a name="to-download-blob-data"></a>BLOB-Daten

- Öffnen Sie im **Server-Explorer**das Kontextmenü für eine oder mehrere Blobs und dann Sie **Öffnen**, wählen Sie Blob und dann **Öffnen** wählen Sie oder doppelklicken Sie Blob.

    Der Fortschritt eines Downloads Blob in **Azure Activity Log** -Fenster wird angezeigt.

    Das Blob wird im Standardeditor für diesen Dateityp geöffnet. Wenn das Betriebssystem den Dateityp erkennt, öffnet die Datei in einer lokal installierten Anwendung; Andernfalls werden Sie aufgefordert, eine Anwendung auswählen, die für den Dateityp des BLOBs geeignet ist. Beim Herunterladen eines BLOBs erstellten lokale Datei ist als schreibgeschützt markiert.

    BLOB-Daten lokal zwischengespeichert und das Blob zuletzt geändert im Blob-Dienst geprüft. Das Blob aktualisiert wurde, seit es zuletzt heruntergeladen wird erneut heruntergeladen. Andernfalls wird das Blob von der Festplatte geladen. Standardmäßig wird ein Blob in ein temporäres Verzeichnis heruntergeladen. Blobs in ein bestimmtes Verzeichnis herunterladen, öffnen Sie das Kontextmenü für die blobnamen der ausgewählten und wählen Sie **Speichern unter**. Beim Speichern eines BLOBs auf diese Weise BLOB-Datei nicht geöffnet, und die lokale Datei mit Lese-/ Schreibzugriff erstellt.

### <a name="to-upload-blobs"></a>Blobs hochladen

- Wählen Sie **Blob hochladen** , wenn der Container in der BLOB-Container geöffnet ist.

    Eine oder mehrere Dateien zum Hochladen auswählen und Laden Sie Dateien aller Typen. **Azure-Aktivitätsprotokoll** zeigt den Fortschritt des Uploads. Weitere Informationen zum Arbeiten mit BLOB-Daten finden Sie unter [Azure Blob Storage Service in .NET verwenden](http://go.microsoft.com/fwlink/p/?LinkId=267911).

### <a name="to-view-logs-transferred-to-blobs"></a>Übertragen von Blobs Protokolle anzeigen

- Wenn Sie Azure-Diagnose zum Protokollieren von Daten aus der Azure-Anwendung verwenden und Protokolle wurden an das Speicherkonto übertragen, sehen Sie Container, die von Azure für diese Protokolle erstellt wurden. Anzeigen von Ereignisprotokollen im Server-Explorer ist eine einfache Möglichkeit, Probleme mit der Anwendung, besonders wenn sie in Azure bereitgestellt wurde. Weitere Informationen zu Azure-Diagnose finden Sie unter [Protokollieren Daten sammeln von Azure-Diagnose verwenden](https://msdn.microsoft.com/library/azure/gg433048.aspx).

### <a name="to-get-the-url-for-a-blob"></a>Die URL für ein Blob zu

- Öffnen Sie der Blob-Kontextmenü zu, und wählen Sie die **URL kopieren**.

### <a name="to-edit-a-blob"></a>So bearbeiten Sie einen blob

- Wählen Sie das Blob, und klicken Sie dann auf **Öffnen-Blob** .

    Die Datei wird an einem temporären Speicherort heruntergeladen und auf dem lokalen Computer öffnen. Blob müssen erneut hochladen, nachdem Sie geändert haben.

## <a name="work-with-queue-resources"></a>Warteschlange Ressourcen

Storage Services Warteschlangen werden in Azure Storage-Konto und können sie Ihre Cloud Rollen durch eine Nachricht Übergabemechanismus untereinander und mit anderen Diensten kommunizieren können. Sie können die Warteschlange programmgesteuert durch einen Cloud-Dienst und über einen Webdienst für externe Clients zugreifen. Die Warteschlange möglich direkt mithilfe von Server-Explorer in Visual Studio.

Bei der Entwicklung eines Cloud-Diensts, der Warteschlangen verwendet werden, empfiehlt es sich Warteschlangen erstellen und arbeiten sie interaktiv beim Entwickeln und Testen Sie den Code mit Visual Studio.

Im Server-Explorer können Sie Warteschlangen in ein Speicherkonto anzeigen, erstellen und Löschen von Warteschlangen, öffnen eine Warteschlange die Nachrichten anzeigen und Nachrichten in einer Warteschlange hinzufügen. Beim Öffnen einer Warteschlange für die Anzeige einzelnen Nachrichten anzeigen und möglich die folgenden Aktionen in der Warteschlange mithilfe der Schaltflächen in der oberen linken Ecke:

- Aktualisieren Sie die Ansicht der Warteschlange

- Hinzufügen einer Meldung in der Warteschlange

- Entfernen Sie die oberste angezeigt.

- Die gesamte Warteschlange löschen

Die folgende Abbildung zeigt eine Warteschlange, die zwei Nachrichten enthält.

![Anzeigen einer Warteschlange](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC651470.png)

Weitere Informationen zum Speicher Warteschlangen services, finden Sie unter [wie: Verwenden der Warteschlangendienst Speicher](http://go.microsoft.com/fwlink/?LinkID=264702). Informationen über den Webdienst für Storage Services Warteschlangen finden Sie in der [Warteschlange Konzepte](http://go.microsoft.com/fwlink/?LinkId=264788). Informationen zum Senden von Nachrichten an eine Warteschlange Storage Services mithilfe von Visual Studio finden Sie unter [Senden von Nachrichten an eine Warteschlange Storage Services](https://msdn.microsoft.com/library/azure/jj649344.aspx).

>[AZURE.NOTE] Storage Services Warteschlangen unterscheiden sich von Service Bus-Warteschlangen. Weitere Informationen zu Service Bus-Warteschlangen finden Sie unter Service Bus Warteschlangen, Themen und Abonnements.

## <a name="work-with-table-resources"></a>Tabelle Ressourcen

Der Speicherdienst Azure Tabelle speichert große Mengen von strukturierten Daten. Der Dienst ist ein NoSQL-Datenspeicher die akzeptiert Aufrufe von innerhalb und außerhalb der Azure-Cloud authentifiziert. Azure-Tabellen eignen sich zum Speichern von strukturierten, nicht-relationalen Daten.

### <a name="to-create-a-table"></a>Zum Erstellen einer Tabelle

1. Wählen Sie im Server-Explorer den Knoten **Tabellen** das Speicherkonto, und wählen Sie **Tabelle erstellen**.

1. Geben Sie einen Namen für die Tabelle, klicken Sie im Dialogfeld **Tabelle erstellen** .

### <a name="to-view-table-data"></a>Daten anzeigen

1. Knoten Sie im Server-Explorer **Azure** und öffnen Sie den Knoten **Speicher** .

1. Öffnen Sie der Konto-Speicherknoten, die Sie interessieren, und dann den Knoten **Tabellen** , um eine Liste der Tabellen für das Speicherkonto.

1. Öffnen Sie das Kontextmenü für eine Tabelle, und wählen Sie die **Tabelle**.

    ![Ein Azure-Tabelle im Projektmappen-Explorer](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744165.png)

Die Tabelle organisiert (siehe Zeilen) Entitäten und Eigenschaften (in Spalten). Die folgende Abbildung zeigt z. B. Entitäten im **Tabellen-Designer**aufgelistet:

### <a name="to-edit-table-data"></a>Tabellendaten bearbeiten

1. Öffnen Sie im **Tabellen-Designer**das Kontextmenü für eine Entität (einer Zeile) oder Eigenschaft (eine einzelne Zelle), und wählen Sie dann **Bearbeiten**.

    ![Hinzufügen oder Bearbeiten einer Tabellenentität](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC656238.png)

    Elemente in einer Tabelle nicht müssen dieselben Eigenschaften (Spalten). Dabei die folgenden Beschränkungen anzeigen und Bearbeiten von Daten.
    - Nicht anzeigen oder Bearbeiten von Binärdaten (Typ byte[]), aber Sie können sie in einer Tabelle speichern.

    - Azure-Tabellenspeicher unterstützt diese Operation kann nicht die Werte **PartitionKey** oder **RowKey** bearbeitet.

    - Sie können eine Eigenschaft namens Timestamp, Azure-Speicherdienste verwenden Sie eine Eigenschaft mit diesem Namen.

    - Wenn Sie einen DateTime-Wert eingeben, befolgen Sie ein Format, das auf die Region und Sprache des Computers geeignet ist (z. B. MM/TT/JJJJ hh: mm: [AM | PM] für US-Englisch).

### <a name="to-add-entities"></a>Hinzufügen von Entitäten

1. Wählen Sie im **Tabellen-Designer**die **Entität hinzufügen** Schaltfläche in der Ecke oben rechts der Tabellenansicht.

    ![Entität hinzufügen](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655336.png)

1. Geben Sie im Dialogfeld **Entität hinzufügen** die Werte der Eigenschaften **PartitionKey** und **RowKey** .

    ![Fügen Sie im Dialogfeld Entität](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655335.png)

    Geben Sie die Werte sorgfältig, da Sie sie ändern können, nachdem das Dialogfeld schließen, wenn die Entität löschen und erneut hinzufügen.

### <a name="to-filter-entities"></a>Elemente filtern

Sie können die Gruppe von Entitäten, die in einer Tabelle angezeigt werden, wenn Sie den Abfrage-Generator verwenden.

1. Um den Abfrage-Generator zu öffnen, öffnen Sie eine Tabelle für die Anzeige.

1. Wählen Sie die Schaltfläche ganz rechts auf der Symbolleiste der Tabelle.

    Der **Abfrage-Generator** wird angezeigt. Die folgende Abbildung zeigt im Abfrage-Generator eine Abfrage erstellt wird.

    ![Abfrage-Generator](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC652231.png)

1. Sie erstellen die Abfrage schließen. Der resultierende Textform der Abfrage wird als WCF-Datendienste Filter in einem Textfeld angezeigt.

1. Wählen Sie zum Ausführen der Abfrage das grüne Dreieck.

    Sie können auch Entitätsdaten filtern, die im **Tabellen-Designer** wird angezeigt, wenn Sie geben eine Filterzeichenfolge WCF-Datendienste direkt in das Feld "Filter". Diese Art von Zeichenfolge ähnelt einer SQL WHERE-Klausel jedoch als eine HTTP-Anforderung an den Server gesendet. Informationen zum Filtern von Zeichenfolgen erstellen finden Sie unter [Filtern von Zeichenfolgen für den Tabellen-Designer erstellen](https://msdn.microsoft.com/library/azure/ff683669.aspx).

    Die folgende Abbildung zeigt ein Beispiel für eine gültige Zeichenfolge:

    ![VST_SE_TableFilter](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC655337.png)

### <a name="refresh-storage-data"></a>Daten aktualisieren

Server-Explorer eine Verbindung mit oder ruft Daten aus einem Speicherkonto, kann es sich um eine Minute nach Abschluss des Vorgangs dauern. Wenn die Verbindung nicht, kann der Vorgang Zeit. Während Daten abgerufen werden, können Sie weiterhin in anderen Teilen des Visual Studio. Um den Vorgang zu lange dauert, wählen Sie den Server-Explorer-Symbolleiste auf die Schaltfläche **Aktualisierung abbrechen** .

#### <a name="to-refresh-blob-container-data"></a>BLOB-Containerdaten aktualisieren

- Den **Blobs** darunter **Speicher** Knoten und wählen Sie Server-Explorer-Symbolleiste auf die Schaltfläche **Aktualisieren** .

- Zum Aktualisieren der Liste von Blobs, die angezeigt wird, wählen Sie **Ausführen** .

#### <a name="to-refresh-table-data"></a>Daten aktualisieren

- Wählen Sie den Knoten **Tabellen** unter **Speichern** und **Aktualisieren** .

- Zum Aktualisieren der Liste der Entitäten, die in den **Tabellen-Designer**angezeigt wird, wählen Sie im **Tabellen-Designer** **Ausführen** .

#### <a name="to-refresh-queue-data"></a>Warteschlangendaten aktualisieren

- Wählen Sie den Knoten **Warteschlangen** , und wählen Sie dann die Schaltfläche **Aktualisieren** .

#### <a name="to-refresh-all-items-in-a-storage-account"></a>Alle Elemente in ein Speicherkonto aktualisiert

- Wählen Sie den Kontonamen und anschließend auf der Symbolleiste auf die Schaltfläche **Aktualisieren** für Server-Explorer.

### <a name="add-storage-accounts-by-using-server-explorer"></a>Fügen Sie Speicherkonten mithilfe von Server-Explorer hinzu

Es gibt zwei Möglichkeiten Speicherkonten mithilfe von Server-Explorer hinzugefügt. Erstellen eines neuen Speicherkontos in Azure-Abonnement oder ein vorhandenes Speicherkonto anfügen.

#### <a name="to-create-a-new-storage-account-by-using-server-explorer"></a>Ein neues Speicherkonto erstellen mithilfe von Server-Explorer

1. Öffnen Sie im Server-Explorer im Kontextmenü für den Knoten Speicher, und wählen Sie Storage-Konto erstellen.

    ![Erstellen eines neuen Kontos Azure-Speicher](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC744166.png)

1. Wählen Sie oder geben Sie die folgende Informationen für das neue Speicherkonto im Dialogfeld **Storage-Konto erstellen** .

    - Der Azure-Abonnement Sie das Speicherkonto hinzufügen möchten.

    - Der Name für das neue Speicherkonto verwenden möchten.

    - Region oder Gruppe (z. B. Westen der USA oder Asien).

    - Der Typ der Replikation für das Speicherkonto wie Geo-Redundant verwenden möchten.

1. Wählen Sie **Erstellen**.

    Neue Speicher-Konto wird in **der Speicherliste im Projektmappen-Explorer** angezeigt.

#### <a name="to-attach-an-existing-storage-account-by-using-server-explorer"></a>An ein vorhandenes Speicherkonto mit Server-Explorer

1. Öffnen Sie im Server-Explorer im Kontextmenü für den Azure-Speicherknoten, und wählen Sie **Externspeicher anfügen**.

    ![Hinzufügen eines vorhandenen Speicher](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766039.png)

1. Wählen Sie oder geben Sie die folgende Informationen für das neue Speicherkonto im Dialogfeld **Storage-Konto erstellen** .

    - Der Name des Speicherkonto zu schließen. Geben Sie einen Namen oder aus der Liste auswählen können.

    - Der Schlüssel für das ausgewählte Speicherkonto. Wählen Sie ein Speicherkonto wird dieser Wert in der Regel für Sie bereitgestellt. Soll Visual Studio Speicherschlüssel Konto an, wählen Sie speichern Konto Feld.

    - Das Protokoll mit Speicherkonto wie HTTP, HTTPS oder ein benutzerdefiniertes Endpunktverhalten herstellen. Weitere Informationen zu benutzerdefinierten Endpunkten finden Sie unter [wie Verbindungszeichenfolgen konfigurieren](https://msdn.microsoft.com/library/azure/ee758697.aspx) .

### <a name="to-view-the-secondary-endpoints"></a>Sekundäre Endpunkte anzeigen

- Erstellt ein Speicherkonto mit **Lesezugriff Geo redundante** Replikationsoption sekundäre Endpunkte angezeigt. Öffnen Sie das Kontextmenü für den Kontonamen, und wählen Sie dann **Eigenschaften**.

    ![Sekundäre Speicher-Endpunkte](./media/vs-azure-tools-storage-resources-server-explorer-browse-manage/IC766040.png)

### <a name="to-remove-a-storage-account-from-server-explorer"></a>Ein Speicherkonto im Server-Explorer entfernen

- Öffnen Sie im Server-Explorer des Kontextmenüs für den Kontonamen, und wählen Sie dann **Löschen**. Wenn Sie ein Speicherkonto löschen, wird gespeicherte Schlüsselinformationen für dieses Konto auch entfernt.

    >[AZURE.NOTE] Wenn Sie ein Speicherkonto im Server-Explorer löschen, Einfluss auf nicht das Speicherkonto oder die darin enthaltenen Daten. Es entfernt nur den Verweis im Server-Explorer. Verwenden Sie zum Löschen ein Speicherkonto [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen über Azure-Speicherdienste verwenden finden Sie unter [Zugriff auf Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/ee405490.aspx).
