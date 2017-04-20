<properties
    pageTitle="Erste Schritte mit Speicher-Explorer (Vorschau) | Microsoft Azure"
    description="Ressourcen Sie Azure-Speicher-mit dem Speicher-Explorer (Vorschau)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Erste Schritte mit Speicher-Explorer (Vorschau)

## <a name="overview"></a>Übersicht 

Microsoft Azure Storage Explorer (Vorschau) ist eine eigenständige Anwendung, die Ihnen ermöglicht, problemlos mit Azure-Speicher unter Windows, OS X und Linux. In diesem Artikel erfahren Sie verschiedene Methoden zum Verbinden und Verwalten der Azure-Speicher.

![Microsoft Azure-Speicher-Explorer (Vorschau)][15]

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Herunterladen und Installieren von Speicher-Explorer (Vorschau)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Verbinden Sie mit einem Speicher oder

Speicher-Explorer (Vorschau) ermöglicht eine Vielzahl Speicherkonten herstellen. Dazu gehören mit Speicherkonten Azure Abonnements mit Speicherkonten und andere Azure-Abonnements freigegebene Dienste zugeordnet und sogar mit und lokale Speicherung mit Speicheremulator Azure verwalten:

- [Mit Azure-Abonnement](#connect-to-an-azure-subscription) - Speicherressourcen zur Azure-Abonnement verwalten.
- [Arbeiten mit lokalen Entwicklung](#work-with-local-development-storage) - verwalten lokale Speicherung mit Speicheremulator Azure. 
- [An externe Speicher](#attach-or-detach-an-external-storage-account) - Management von Speicherressourcen zur anderen Azure-Abonnement des Speicherkontos Kontonamen und Schlüssel.
- [Attach Speicherkonto mithilfe SAS](#attach-storage-account-using-sas) - Management von Speicherressourcen zur anderen Azure-Abonnement mithilfe SAS.
- [Attach Service mithilfe SAS](#attach-service-using-sas) - verwalten einen bestimmten Speicher-Dienst (BLOB-Container, Warteschlange oder Tabelle) zur anderen Azure-Abonnement mithilfe SAS.

## <a name="connect-to-an-azure-subscription"></a>Verbinden Sie mit Azure-Abonnement

> [AZURE.NOTE] Wenn ein Azure-Konto haben, können Sie [sich für eine kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [die Visual Studio-Abonnementvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. Wählen Sie im Speicher-Explorer (Vorschau) **Azure-Konto**. 

    ![Azure-Konto][0]

1. Im linken Bereich zeigt jetzt alle Microsoft-Konten, denen Sie sich angemeldet haben. Verbindung mit einem anderen Konto wählen Sie **ein Konto hinzufügen**und folgen Sie Dialogfelder mit einem Microsoft-Konto anmelden, die mindestens eine aktive Azure-Abonnement zugeordnet ist.

    ![Konto hinzufügen][1]

1. Wenn Sie ein Microsoft-Konto erfolgreich anmelden, wird links mit diesem Konto zugeordneten Azure-Abonnements aufgefüllt. Wählen Sie den Azure-Abonnements mit denen Sie arbeiten möchten, und dann **Übernehmen**. ( **Alle Abonnements** schaltet alle oder keinen der aufgeführten Azure-Abonnements auswählen auswählen)

    ![Wählen Sie Azure-Abonnements][3]

1. Im linken Bereich zeigt jetzt Speicherkonten ausgewählten Azure Abonnements zugeordnet.

    ![Ausgewählte Azure-Abonnements][4]

## <a name="work-with-local-development-storage"></a>Arbeiten Sie mit lokalen Entwicklung

Speicher-Explorer (Preview) können Sie lokale Speicherung mit Azure Speicheremulator verwenden. Dadurch können Sie Code schreiben und Testen von Speicher ohne ein Speicherkonto (da das Speicherkonto von Azure Speicheremulator emuliert) auf Azure bereitgestellt.

>[AZURE.NOTE] Azure Storage-Emulator wird derzeit nur für Windows unterstützt. 

1. Erweitern Sie im linken Bereich des Speicher-Explorers (Preview) der **(lokale und zugeordnete** > **Speicherkonten** > Knoten**(Entwicklung)** .

    ![Lokale Entwicklung Knoten][21]

1. Wenn der Speicheremulator Azure noch nicht installiert haben, werden Sie per Infoleiste aufgefordert. Wenn die Infoleiste angezeigt wird, wählen Sie **die neueste Version herunterladen**und installieren Sie den Emulator. 

    ![Azure Speicheremulator Aufforderung herunterladen][22]

1. Sobald der Emulator installiert ist, müssen Sie die Fähigkeit zum Erstellen und Arbeiten mit lokalen Blobs, Queues und Tabellen. Informationen zum Arbeiten mit jeder Firma Speichertyp wählen Sie auf den entsprechenden Link unten:

    - [Azure BLOB-Speicher Ressourcen](./vs-azure-tools-storage-explorer-blobs.md)
    - Verwalten von Azure Storage Dateifreigaberessourcen - *Vorbereitung*
    - Management von Speicherressourcen Azure-Warteschlange - *Vorbereitung*
    - Management von Speicherressourcen Azure Tabelle - *Vorbereitung*

## <a name="attach-or-detach-an-external-storage-account"></a>Fügen Sie an oder trennen Sie einer externen Speicher-Konto

Speicher-Explorer (Preview) bietet die Möglichkeit, externe Speicherkonten zuordnen, damit Speicherkonten problemlos gemeinsam genutzt werden können. Dieser Abschnitt erklärt das zuordnen (und trennen) externe Speicherkonten.

### <a name="get-the-storage-account-credentials"></a>Anmeldeinformationen für das Speicherkonto abrufen

Um einen externen Speicherkonto gemeinsam nutzen, muss der Besitzer des Kontos zunächst Anmeldeinformationen - Kontonamen und Schlüssel - für das Konto und Person, (extern) Konto zuordnen wollen, Informationen freigeben. Abrufen von Anmeldeinformationen für das Speicherkonto Azure-Portal erfolgt über folgende Schritte: 

1.  Mit der [Azure-Portal](https://portal.azure.com)anmelden.
1.  Wählen Sie **Durchsuchen**.
1.  **Speicherkonten**auswählen
1.  Wählen Sie Blatt **Speicherkonten** gewünschten Speicherkontos.
1.  Wählen Sie im Blatt **Einstellungen** für das ausgewählte Speicherkonto **Zugriffstasten**.

    ![Schlüssel anzuzeigen][5]
    
1.  **Zugriffstasten** Blatt kopieren Sie **SPEICHERKONTONAMEN** und **Schlüssel 1** Werte für die Verwendung beim Anschließen an das Speicherkonto. 

    ![Zugriffstasten][6]

### <a name="attach-to-an-external-storage-account"></a>Ein externer Speicherkonto zuordnen
Anfügen an einen externen Speicher-Konto benötigen Sie Namen und den Schlüssel des Kontos. *Abrufen von Anmeldeinformationen für das Speicherkonto* erläutert: wie man diese Werte von Azure-Portal. Beachten Sie, dass im Portal kontoschlüssel genannt "key 1" so bei der Speicher-Explorer (Vorschau) ein kontoschlüssel anfordert, werden eingeben (oder einfügen) den Wert "key 1". 
 
1.  Wählen Sie im Speicher-Explorer (Vorschau) **mit Azure-Speicher**.

    ![Verbinden Sie mit Azure Storage-option][23]

1.  Klicken Sie im Dialogfeld **Verbindung mit Azure Storage** das Konto Schlüssel (Wert von Azure-Portal "key 1") und anschließend auf **Weiter**.

    ![Azure-Speicher Dialogfeld Verbinden][24] 

1.  Geben Sie im Dialogfeld **Anfügen Externspeicher** Speicher Konto in das Feld **Kontoname** , geben Sie andere Einstellungen und wählen Sie **Weiter** ausgeführt. 

    ![Externer Speicher Dialogfeld Anfügen][8]

1.  Überprüfen Sie im Dialogfeld **Verbindung Zusammenfassung** der Informationen. Wenn Sie ändern möchten, wählen Sie **zurück** und geben Sie die gewünschte Einstellung. Nachdem, wählen Sie **Verbinden**.

1.  Sobald verbunden, externe Speicher-Konto mit Text **(extern)** angehängten Storage-Konto erscheint. 

    ![Herstellen einer Verbindung mit einem externen Speicher-Konto aus][9]

### <a name="detach-from-an-external-storage-account"></a>Trennen von einem externen Speicher-Konto

1.  Maustaste externen Speicher-Konto zu trennen, und wählen Sie aus dem Kontextmenü - **Trennen**.

    ![Speicheroption trennen][10]

1.  Im Bestätigungsfeld angezeigt, wählen Sie **Ja** , die Trennung von externen Speicher-Konto zu bestätigen.

## <a name="attach-storage-account-using-sas"></a>Mit SAS Speicherkonto Anfügen

[SAS (SAS)](storage/storage-dotnet-shared-access-signature-part-1.md) gibt der Administrator der Azure-Abonnement auf ein Speicherkonto vorübergehend zu erteilen, ohne ihre Anmeldeinformationen Azure-Abonnement. 

Zur Veranschaulichung angenommen UserA ist Administrator Azure-Abonnement und BenutzerA BenutzerB auf ein Speicherkonto für einen begrenzten Zeitraum mit bestimmten Berechtigungen zu:

1. UserA generiert SAS (die Verbindungszeichenfolge für das Speicherkonto bestehend) für einen bestimmten Zeitraum und die gewünschten Berechtigungen.
1. UserA teilt die SAS mit der Person, die Zugriff auf das Speicherkonto - BenutzerB, in unserem Beispiel wollen.  
1. BenutzerB verwendet Storage Explorer (Vorschau) an das Konto BenutzerA mit angegebenen SAS. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Holen Sie SAS für das Konto, das Sie freigeben möchten

1.  Im Speicher-Explorer (Vorschau) mit der rechten Maustaste des Speicherkontos freigeben möchten und wählen Sie aus dem Kontextmenü - **SAS erhalten**.

    ![Abrufen von SAS-Kontextmenü][13]

1. Im Dialogfeld **SAS** Geben Sie den Zeitrahmen und Berechtigungen für das Konto ein, und **Erstellen**.

    ![SAS angezeigt][14]
 
1. Ein zweites Dialogfeld **SAS** wird die SAS angezeigt. Wählen Sie **Kopieren** die **Verbindungszeichenfolge** in die Zwischenablage kopieren. Wählen Sie **Schließen** das Dialogfeld zu schließen.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Das gemeinsame Konto mithilfe SAS zuordnen

1.  Wählen Sie im Speicher-Explorer (Vorschau) **mit Azure-Speicher**.

    ![Verbinden Sie mit Azure Storage-option][23]

1.  Klicken Sie im Dialogfeld **Verbindung mit Azure-Speicher** Geben Sie die Verbindungszeichenfolge an und anschließend auf **Weiter**.

    ![Azure-Speicher Dialogfeld Verbinden][24] 

1.  Überprüfen Sie im Dialogfeld **Verbindung Zusammenfassung** der Informationen. Wenn Sie ändern möchten, wählen Sie **zurück** und geben Sie die gewünschte Einstellung. Nachdem, wählen Sie **Verbinden**.

1.  Nach das Speicherkonto erscheint mit dem angehängten Konto eingegebene Text (SAS).

    ![Ergebnis einer Firma mithilfe SAS verbunden][17]

## <a name="attach-service-using-sas"></a>Anfügen mit SAS-service

Abschnitt [Anhängen Speicherkonto mithilfe SAS](#attach-storage-account-using-sas) werden wie Administrator Azure-Abonnement temporäre ein Speicherkonto generiert gewähren kann (und) SAS für das Speicherkonto. Ebenso kann für einen bestimmten Dienst (BLOB-Container, Warteschlange oder Tabelle) in ein Speicherkonto SAS generiert werden.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Generieren einer SAS für den Dienst, den Sie freigeben möchten

In diesem Kontext kann ein Dienst ein BLOB-Container, Warteschlange oder Tabelle. In den folgenden Abschnitten erläutern wie SAS für die aufgeführten Dienst generiert:

- [SAS für BLOB-Container abrufen](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- SAS für Dateifreigabe - *bald* abrufen
- Erhalten Sie SAS für eine Warteschlange - *bald*
- SAS für eine Tabelle - *bald* abrufen

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Fügen Sie an den freigegebenen Konto mithilfe SAS an

1.  Wählen Sie im Speicher-Explorer (Vorschau) **mit Azure-Speicher**.

    ![Verbinden Sie mit Azure Storage-option][23]

1.  Klicken Sie im Dialogfeld **Verbindung mit Azure Storage** Geben Sie SAS-URI an und anschließend auf **Weiter**.

    ![Azure-Speicher Dialogfeld Verbinden][24] 

1.  Überprüfen Sie im Dialogfeld **Verbindung Zusammenfassung** der Informationen. Wenn Sie ändern möchten, wählen Sie **zurück** und geben Sie die gewünschte Einstellung. Nachdem, wählen Sie **Verbinden**.

1.  Nach der neu angefügte Dienst unter dem Knoten **(Service SAS)** erscheint.

    ![Ergebnis an einen freigegebenen Dienst mithilfe SAS][20]

## <a name="search-for-storage-accounts"></a>Speicherkonten suchen

Haben Sie eine lange Liste von Speicherkonten werden schnell zu einem bestimmten Speicherkonto verwenden Sie das Suchfeld oben im linken. 

Eingabe im Suchfeld zeigt Links nur die Speicherkonten, die den zu suchenden Wert entsprechen bis eingegebene zeigen. Der folgende Screenshot zeigt ein Beispiel, in dem ich alle Speicherkonten gesucht haben speicherkontoname den Text "ml" enthält.

![Storage-Konto suchen][11]
    
Suche zu deaktivieren, wählen Sie die Schaltfläche **X** im Suchfeld.

## <a name="next-steps"></a>Nächste Schritte
- [Management von Speicherressourcen Azure BLOB-Speicher-Explorer (Vorschau)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
