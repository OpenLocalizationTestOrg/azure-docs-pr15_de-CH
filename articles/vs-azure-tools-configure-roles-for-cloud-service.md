<properties
   pageTitle="Konfigurieren von Rollen für eine Azure-Cloud-Dienst mit Visual Studio | Microsoft Azure"
   description="Informationen Sie zum Einrichten und Konfigurieren von Rollen für Azure-Cloud-Diensten mit Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configure-the-roles-for-an-azure-cloud-service-with-visual-studio"></a>Konfigurieren von Rollen für eine Azure-Cloud-Dienst mit Visual Studio

Ein Azure-Cloud-Dienst können Arbeitskraft oder Webrollen. Für jede Rolle müssen Sie definieren, wie die Rolle eingerichtet ist und auch konfigurieren, wie diese Funktion ausgeführt wird. Um weitere Informationen zu Rollen in Cloud-Dienste finden Sie unter video- [Einführung in Azure Cloud Services](https://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Introduction-to-Windows-Azure-Cloud-Services). Die Informationen für den Clouddienst ist in den folgenden Dateien gespeichert:

- **ServiceDefinition.csdef**

    Service-Definitionsdatei definiert die Runtime Einstellungen für Cloud-Dienst einschließlich Rollen erforderlich sind, Endpunkte und Größe des virtuellen Computers. In dieser Datei gespeicherten Daten können geändert werden, wenn Ihre Rolle ausgeführt wird.

- **ServiceConfiguration.cscfg**

    Wie viele Instanzen einer Rolle ausgeführt werden und die Werte für eine Rolle definierten Einstellungen konfiguriert Dienstkonfigurationsdatei. In dieser Datei gespeicherten Daten können geändert werden, während Ihre Rolle ausgeführt wird.

Um für die Ausführung Ihrer Rolle unterschiedliche Werte für diese Einstellung speichern, haben Sie mehrere Dienstkonfigurationen. Eine andere Konfiguration können Sie für jede bereitstellungsumgebung. Beispielsweise können Sie Storage-Konto Zeichenfolge zu einer Dienstleistung lokalen Azure Speicheremulator verwenden einen anderen Dienstkonfiguration den Azure-Speicher in der Cloud festlegen.

Beim Erstellen eines neuen Azure-Cloud-Diensts in Visual Studio werden standardmäßig zwei Service-Konfigurationen erstellt. Diese Konfigurationen werden Azure-Projekts hinzugefügt. Die Konfigurationen lauten:

- ServiceConfiguration.Cloud.cscfg

- ServiceConfiguration.Local.cscfg

## <a name="configure-an-azure-cloud-service"></a>Einen Azure-Cloud-Dienst konfigurieren

Sie können einen Azure-Cloud-Dienst im Projektmappen-Explorer in Visual Studio konfigurieren, wie in der folgenden Abbildung dargestellt.

![Cloud-Dienst konfigurieren](./media/vs-azure-tools-configure-roles-for-cloud-service/IC713462.png)

### <a name="to-configure-an-azure-cloud-service"></a>Einen Azure-Cloud-Dienst konfigurieren

1. Um jede Rolle in Azure Projekt im **Projektmappen-Explorer**zu konfigurieren, öffnen Sie das Kontextmenü für die Rolle in Azure-Projekt und wählen Sie dann **Eigenschaften**.

    Eine Seite mit dem Namen der Rolle wird im Visual Studio-Editor angezeigt. Die Seite zeigt die Felder der Registerkarte **Konfiguration** .

1. Wählen Sie in der Liste **Konfiguration** der Name der Konfiguration, die Sie bearbeiten möchten.

    Wenn Sie alle Dienstkonfigurationen für diese Rolle ändern möchten, können Sie **Alle Konfigurationen**.

    >[AZURE.IMPORTANT] Wählt eine bestimmte Konfiguration sind einige Eigenschaften deaktiviert, da sie nur für alle Konfigurationen festgelegt werden. Um diese Eigenschaften zu bearbeiten, müssen Sie alle Konfigurationen auswählen.

    Sie können jetzt eine Registerkarte, um alle aktivierten Eigenschaften für Ansicht aktualisieren.

## <a name="change-the-number-of-role-instances"></a>Ändern der Anzahl der Rolleninstanzen

Zum Verbessern der Leistung der Cloud-Dienst können Sie die Anzahl der Instanzen einer Rolle ändern, die ausgeführt werden, basierend auf der Anzahl der Benutzer oder der erwarteten Auslastung für eine bestimmte Rolle. Ein virtuellen Computer wird für jede Instanz der Rolle erstellt, wenn in Azure Cloud-Dienst ausgeführt wird. Dies betrifft die Rechnung für die Bereitstellung von Cloud-Dienst. Weitere Informationen zur Abrechnung finden Sie [Ihre Rechnung für Microsoft Azure verstehen](billing/billing-understand-your-bill.md).

### <a name="to-change-the-number-of-instances-for-a-role"></a>So ändern Sie die Anzahl der Instanzen für eine Rolle

1. Wählen Sie die Registerkarte **Konfiguration** .

1. Wählen Sie in der Liste **Konfiguration** die Konfiguration, die Sie aktualisieren möchten.

    >[AZURE.NOTE] Sie können die Anzahl der Instanzen für eine bestimmte Konfiguration oder alle Dienstkonfigurationen festlegen.

1. Geben Sie im Feld **Anzahl der Instanzen** die Anzahl der Instanzen dieser Rolle beginnen soll.

    >[AZURE.NOTE] Jede Instanz wird auf einem virtuellen Computer ausgeführt, wenn den Cloud-Dienst in Azure veröffentlichen.

1. Wählen Sie die Schaltfläche **Speichern** auf der Symbolleiste auf die Dienstkonfigurationsdatei speichern.

## <a name="manage-connection-strings-for-storage-accounts"></a>Verbindungszeichenfolgen für Speicherkonten verwalten

Sie können hinzufügen, entfernen oder ändern Verbindungszeichenfolgen für Ihre Dienstkonfigurationen. Sie sollten unterschiedliche Verbindungszeichenfolgen für verschiedene Dienstkonfigurationen. Beispielsweise empfiehlt eine lokale Verbindungszeichenfolge für eine lokale Konfiguration mit dem Wert der `UseDevelopmentStorage=true`. Sie sollten auch eine Cloud-Dienstkonfiguration konfigurieren, die ein Speicherkonto in Azure verwendet.

>[AZURE.WARNING] Bei der Eingabe der Azure-Speicher wichtige Kontoinformationen für eine Verbindungszeichenfolge Storage-Konto wird diese Informationen in der Konfigurationsdatei Service lokal gespeichert. Dazu ist jedoch derzeit nicht als verschlüsselter Text gespeichert.

Einen anderen Wert für jede Konfiguration verwenden, müssen Sie keinen verwenden unterschiedliche Verbindungszeichenfolgen in der Cloud-Dienst oder den Code ändern, wenn Sie Azure Cloud-Dienst veröffentlichen. Können Sie im Code den gleichen Namen für die Verbindungszeichenfolge und der Wert unterscheiden, basierend auf der Konfiguration, die Sie auswählen, wenn Sie Cloud-Dienst erstellen oder beim Veröffentlichen.

### <a name="to-manage-connection-strings-for-storage-accounts"></a>Die Verbindungszeichenfolgen für Speicherkonten verwalten

1. Wählen Sie **die Registerkarte** .

1. Wählen Sie in der Liste **Konfiguration** die Konfiguration, die Sie aktualisieren möchten.

    >[AZURE.NOTE] Verbindungszeichenfolgen für eine bestimmte Konfiguration aktualisieren, aber ggf. hinzufügen oder Löschen einer Verbindungszeichenfolge müssen Sie alle Konfigurationen auswählen.

1. Wählen Sie zum Hinzufügen einer Verbindungszeichenfolge die **Einstellung hinzufügen** . Ein neuer Eintrag wird der Liste hinzugefügt.

1. Geben Sie im Feld **Name** den Namen für die Verbindungszeichenfolge verwenden möchten.

1. Wählen Sie in der Dropdown-Liste **Typ** **-Verbindungszeichenfolge**.

1. Wählen Sie zum Ändern des Wertes für die Verbindungszeichenfolge das Auslassungszeichen (...). Das Dialogfeld **Storage-Verbindungszeichenfolge erstellen** .

1. Um das lokale Konto Speicheremulator verwenden, wählen Sie das Optionsfeld **Microsoft Azure Storage Emulator** , und klicken Sie dann auf **OK** .

1. Um ein Speicherkonto in Azure verwenden, wählen Sie die Option " **Abonnements** ", und wählen Sie das gewünschte Speicher.

1. Um benutzerdefinierte Anmeldeinformationen verwenden, wählen Sie **manuell eingegebenen Anmeldeinformationen** Optionen. Geben Sie speicherkontonamen und Primärschlüssel bzw. zweite Schlüssel. Informationen wie ein Speicherkonto erstellen und geben Sie die Details für das Speicherkonto im Dialogfeld **Storage-Verbindungszeichenfolge erstellen** finden Sie unter [Vorbereitung zu veröffentlichen oder eine Azure-Anwendung aus Visual Studio bereitstellen](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Um eine Verbindungszeichenfolge zu löschen, wählen Sie die Verbindungszeichenfolge, und klicken Sie dann auf **Einstellung entfernen** .

1. Wählen Sie das Symbol **Speichern** auf der Symbolleiste auf die Dienstkonfigurationsdatei speichern.

1. Um die Verbindungszeichenfolge in der Konfigurationsdatei Service zuzugreifen, müssen Sie den Wert der Einstellung abrufen. Der folgende Code zeigt ein Beispiel, in dem BLOB-Speicher erstellt, und Daten mit einer Verbindungszeichenfolge hochgeladen `MyConnectionString` aus der Dienstkonfigurationsdatei, wenn ein Benutzer auf die Seite Default.aspx in der Webrolle Azure-Clouddienst **Button1** auswählt. Fügen Sie den folgenden Anweisungen Default.aspx.cs verwenden:

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Öffnen Sie Default.aspx.cs in der Entwurfsansicht, und fügen Sie eine Schaltfläche aus der Toolbox. Fügen Sie den folgenden Code in die `Button1_Click` Methode. Dieser Code verwendet `GetConfigurationSettingValue` Nutzen aus der Service-Konfigurationsdatei für die Verbindungszeichenfolge. Ein Blob im Speicherkonto erstellt, die in der Verbindungszeichenfolge auf `MyConnectionString` und schließlich die Anwendung Text hinzugefügt Blob.

    ```
    protected void Button1_Click(object sender, EventArgs e)
    {
        // Setup the connection to Azure Storage
        var storageAccount = CloudStorageAccount.Parse(RoleEnvironment.GetConfigurationSettingValue("MyConnectionString"));
        var blobClient = storageAccount.CreateCloudBlobClient();
        // Get and create the container
        var blobContainer = blobClient.GetContainerReference("quicklap");
        blobContainer.CreateIfNotExists();
        // upload a text blob
        var blob = blobContainer.GetBlockBlobReference(Guid.NewGuid().ToString());
        blob.UploadText("Hello Azure");

    }
    ```

## <a name="add-custom-settings-to-use-in-your-azure-cloud-service"></a>Hinzufügen von benutzerdefinierten Einstellungen in der Azure-Cloud-Dienst

Benutzerdefinierte Einstellung in der Konfigurationsdatei Service können Sie einen Namen und einen Wert für eine Zeichenfolge für eine bestimmte Konfiguration. Sie können diese Einstellung verwenden, um eine in Ihrem Cloud-Dienst konfigurieren, indem Sie den Wert der Einstellung lesen und diesen Wert zum Steuern der Logik im Code verwenden. Ändern dieser Dienst Werte ohne rebuild Servicepaket oder bei Ihrem Cloud-Dienst ausgeführt wird. Code kann bei Änderung einer Einstellung für Benachrichtigungen überprüfen. Siehe [RoleEnvironment.Changing-Ereignis](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx).

Sie können hinzufügen, entfernen oder ändern Standardeinstellungen für Ihre Dienstkonfigurationen. Sie sollten unterschiedliche Werte für diese Zeichenfolgen für verschiedene Dienstkonfigurationen.

Einen anderen Wert für jede Konfiguration verwenden, müssen Sie keinen andere Zeichenfolgen in Ihrem Cloud-Dienst oder den Code ändern, wenn Sie Azure Cloud-Dienst veröffentlichen. Können Sie denselben Namen für die Zeichenfolge im Code und der Wert unterscheiden, basierend auf der Konfiguration, die Sie auswählen, wenn Sie Cloud-Dienst erstellen oder beim Veröffentlichen.

### <a name="to-add-custom-settings-to-use-in-your-azure-cloud-service"></a>Benutzerdefinierte Einstellungen in der Azure-Cloud-Dienst hinzufügen

1. Wählen Sie **die Registerkarte** .

1. Wählen Sie in der Liste **Konfiguration** die Konfiguration, die Sie aktualisieren möchten.

    >[AZURE.NOTE] Sie können Zeichenfolgen für eine bestimmte Konfiguration, aber Sie ggf. hinzufügen oder Löschen einer Zeichenfolge müssen Sie **Alle Konfigurationen**auswählen.

1. Wählen Sie zum Hinzufügen einer **Einstellung hinzufügen** . Ein neuer Eintrag wird der Liste hinzugefügt.

1. Geben Sie im Feld **Name** den Namen für die Zeichenfolge verwendet werden soll.

1. Wählen Sie in der Dropdown-Liste **Typ** **Zeichenfolge**.

1. Geben Sie zum Hinzufügen oder ändern Sie den Wert für die Zeichenfolge in das Textfeld **Wert** den neuen Wert ein.

1. Zum Löschen einer Zeichenfolge wählen Sie die Zeichenfolge aus, und klicken Sie dann auf **Einstellung entfernen** .

1. Wählen Sie die Schaltfläche **Speichern** auf der Symbolleiste auf die Dienstkonfigurationsdatei speichern.

1. Um die Zeichenfolge in der Konfigurationsdatei Service zuzugreifen, müssen Sie den Wert der Einstellung abrufen.

    Müssen Sie sicherstellen, dass die folgende using Statements sind bereits hinzugefügt Default.aspx.cs, wie im vorherigen Verfahren.

    ```
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.ServiceRuntime;
    ```

1. Fügen Sie den folgenden Code in die `Button1_Click` Methode, um diese Zeichenfolge auf die gleiche Weise eine Verbindungszeichenfolge zugreifen. Code kann dann basierend auf dem Wert der einstellungszeichenfolge für Dienstkonfigurationsdatei verwendet bestimmte Code ausführen.

    ```
    var settingValue = RoleEnvironment.GetConfigurationSettingValue("MySetting");
    if (settingValue == “ThisValue”)
    {
    // Perform these lines of code
    }
    ```

## <a name="manage-local-storage-for-each-role-instance"></a>Verwalten von lokalen Speicher für jede Instanz der Rolle

Sie können lokale Dateisystem-Speicherung für jede Instanz der Rolle hinzufügen. Sie können lokale Daten speichern, die nicht von anderen Rollen zugegriffen werden. Alle Daten nicht in Tabelle Blob oder SQL-Datenbankspeicher speichern müssen können hier gespeichert werden. Beispielsweise können Sie diese lokalen Speicher zum Zwischenspeichern von Daten, die wieder verwendet werden müssen. Diese Daten können nicht von anderen Instanzen der Rolle zugegriffen werden. 

Lokale Einstellungen gelten für alle Dienstkonfigurationen. Sie können nur hinzufügen, entfernen Sie oder ändern Sie lokalen Speicher für alle Dienstkonfigurationen.

### <a name="to-manage-local-storage-for-each-role-instance"></a>Lokalen Speicher für jede Rolleninstanz verwalten

1. Wählen Sie die Registerkarte **Lokale Speicher** .

1. Wählen Sie in der Liste **Konfiguration** **Alle Konfigurationen**.

1. Um einen Eintrag Lokaler Speicher hinzufügen, wählen Sie **Lokalen Speicher hinzufügen** . Ein neuer Eintrag wird der Liste hinzugefügt.

1. Geben Sie im Feld **Name** den Namen für diese lokalen Speicher verwenden möchten.

1. Geben Sie im Feld **Größe** die Größe in MB, die für diese lokalen Speicher benötigen.

1. Entfernen die Daten im lokalen Speicher beim virtuellen Computer für diese Rolle wiederverwendet wird, aktivieren Sie das Kontrollkästchen **säubern Rolle wiederverwenden** .

1. Bearbeiten Sie einen vorhandenen lokalen Speicher Eintrag wählen Sie die Zeile, die Sie aktualisieren möchten. Anschließend können Sie die Felder bearbeiten, wie in den vorherigen Schritten beschrieben.

1. Um einen lokalen Speicher Eintrag zu löschen, wählen Sie Storage-Eintrag in der Liste, und klicken Sie dann auf **Lokaler Speicher entfernen** .

1. Diese Service-Konfigurationsdateien speichern, wählen Sie auf der Symbolleiste auf das Symbol **Speichern** .

1. Um den lokalen Speicher zuzugreifen, den in der Konfigurationsdatei Service hinzugefügt haben, müssen Sie den Wert der Einstellung lokale Ressource abrufen. Verwenden Sie die folgenden Codezeilen dieser Wert eine Datei namens **MyStorageTest.txt** und eine Zeile von Daten in die Datei schreiben. Fügen Sie diesen Code in der `Button_Click` Methode, die Sie im vorherigen Verfahren verwendet:

1. Müssen Sie sicherstellen, dass die folgende using-Anweisung Default.aspx.cs hinzugefügt:

    ```
    using System.IO;
    using System.Text;
    ```

1. Fügen Sie den folgenden Code in die `Button1_Click` Methode. Dies erstellt die Datei im lokalen Speicher und schreibt Daten in die Datei.

    ```
    // Retrieve an object that points to the local storage resource
    LocalResource localResource = RoleEnvironment.GetLocalResource("LocalStorage1");

    //Define the file name and path
    string[] paths = { localResource.RootPath, "MyStorageTest.txt" };
    String filePath = Path.Combine(paths);

    using (FileStream writeStream = File.Create(filePath))
    {
          Byte[] textToWrite = new UTF8Encoding(true).GetBytes("Testing Web role storage");
          writeStream.Write(textToWrite, 0, textToWrite.Length);
    }
    ```

1. (Optional) Um diese Datei anzuzeigen, die Sie erstellt, wenn Sie den Cloud-Dienst lokal ausführen, gehen Sie folgendermaßen vor:

  1. Führen Sie die Web-Rolle und wählen **Button1** sicherstellen, dass der Code in `Button1_Click` aufgerufen wird.

  1. Infobereich öffnen Sie des Kontextmenüs für Azure Symbol und wählen Sie **Berechnen Emulator-Benutzeroberfläche angezeigt**. **Azure-Serveremulator** -Dialogfeld wird angezeigt.

  1. Wählen Sie die Web-Rolle.

  1. Wählen Sie auf der Menüleiste **Extras** **Öffnen lokalen Speicher**. Windows Explorer-Fenster wird angezeigt.

  1. Geben Sie auf der Menüleiste **MyStorageTest.txt** in das Textfeld **Suchen ein** und dann **EINGABETASTE** zu suchen.

    Die Datei wird in den Suchergebnissen angezeigt.

  1. Um den Inhalt der Datei anzuzeigen, öffnen Sie das Kontextmenü für die Datei, und wählen Sie **Öffnen**.

## <a name="collect-cloud-service-diagnostics"></a>Cloud-Dienst Diagnoseinformationen sammeln

Sie können für Ihren Azure-Cloud-Dienst Diagnosedaten sammeln. Ein Speicherkonto Daten hinzugefügt. Sie sollten unterschiedliche Verbindungszeichenfolgen für verschiedene Dienstkonfigurationen. Beispielsweise empfiehlt ein Konto Lokaler Speicher für eine lokale Konfiguration mit dem Wert von UseDevelopmentStorage = True. Sie sollten auch eine Cloud-Dienstkonfiguration konfigurieren, die ein Speicherkonto in Azure verwendet. Weitere Informationen zu Azure-Diagnose finden Sie unter protokollieren Daten sammeln von Azure-Diagnose verwenden.

>[AZURE.NOTE] Konfiguration des lokalen Dienstes ist bereits konfiguriert lokale Ressourcen. Verwenden Sie die Cloud-Dienstkonfiguration den Azure-Cloud-Dienst veröffentlichen, wird die Verbindungszeichenfolge angeben, wenn Sie veröffentlichen auch für Diagnose-Verbindungszeichenfolge verwendet, wenn eine Verbindungszeichenfolge angegeben haben. Verpacken der Cloud-Dienst mit Visual Studio wird die Verbindungszeichenfolge in der Konfiguration des Dienstes nicht geändert.

### <a name="to-collect-cloud-service-diagnostics"></a>Cloud-Dienst Diagnose sammeln

1. Wählen Sie die Registerkarte **Konfiguration** .

1. Wählen Sie in der Liste **Konfiguration** die Dienstkonfiguration zu aktualisieren, oder wählen **Alle Konfigurationen**.

    >[AZURE.NOTE] Das Speicherkonto für eine bestimmte Konfiguration aktualisieren, aber möchten Sie aktivieren oder Deaktivieren der Diagnose müssen Sie alle Konfigurationen auswählen.

1. Um die Diagnose zu aktivieren, aktivieren Sie das Kontrollkästchen **Diagnose aktivieren** .

1. Wählen Sie zum Ändern des Wertes für das Speicherkonto Auslassungszeichen (...).

    Das Dialogfeld **Storage-Verbindungszeichenfolge erstellen** .

1. Um eine lokale Verbindungszeichenfolge verwenden, wählen Sie Emulator Azure-Speicher, und klicken Sie dann auf **OK** .

1. Um ein Speicherkonto zugeordneten Azure-Abonnement verwenden, wählen Sie **Ihr Abonnement** .

1. Um ein Speicherkonto für die lokale Verbindungszeichenfolge verwenden, wählen Sie **manuell eingegebenen Anmeldeinformationen** .

    Weitere Informationen wie ein Speicherkonto erstellen und geben Sie die Details für das Speicherkonto im Dialogfeld **Storage-Verbindungszeichenfolge erstellen** finden Sie unter [Vorbereitung zu veröffentlichen oder eine Azure-Anwendung aus Visual Studio bereitstellen](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Wählen Sie das Speicherkonto **Kontonamen**verwenden möchten.

    Sind Sie manuell eingeben Ihrer Anmeldeinformationen Speicher kopieren oder Primärschlüssel **kontoschlüssel**geben. Dieser Schlüssel kann aus dem [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)kopiert werden. Dieser Schlüssel in [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)Ansicht **Speicherkonten** folgendermaßen kopieren:
    
  1. Wählen Sie das Speicherkonto für den Clouddienst verwenden möchten.

  1. Wählen Sie **Zugriffstasten verwalten** am unteren Rand des Bildschirms. Das Dialogfeld **Zugriff auf Schlüssel verwalten** .

  1. Wählen Sie die Zugriffstaste kopieren **in die Zwischenablage kopieren** . Sie können nun diesen Schlüssel in das Feld **Konto** einfügen.

1. Das Speicherkonto verwenden, das Sie, wie der Verbindungszeichenfolge bereitstellen für Diagnose (und Zwischenspeichern) Wenn Sie Azure Cloud-Dienst veröffentlichen das Kontrollkästchen **Update Entwicklung Speicher Verbindungszeichenfolgen für Diagnose und Zwischenspeichern mit Azure-Speicher Anmeldeinformationen beim Veröffentlichen in Azure Konto** auswählen.

1. Wählen Sie die Schaltfläche **Speichern** auf der Symbolleiste auf die Dienstkonfigurationsdatei speichern.

## <a name="change-the-size-of-the-virtual-machine-used-for-each-role"></a>Ändern der Größe der virtuellen Computer für jede Rolle verwendet

Sie können die Größe des virtuellen Computers für jede Rolle festlegen. Sie können nur diese Größe für alle Dienstkonfigurationen festlegen. Bei Auswahl der Maschine kleiner ist weniger CPU-Kerne, Arbeitsspeicher und Festplatte Speicher zugewiesen. Zugewiesene Bandbreite ist auch kleiner. Weitere Informationen zu diesen Größen und die anzeigen Sie [Größen für Cloud-Dienste](cloud-services/cloud-services-sizes-specs.md)

Die erforderlichen Ressourcen für jeden virtuellen Computer in Azure wirkt sich auf die Kosten für den Clouddienst in Azure. Weitere Informationen zu Azure Abrechnung finden Sie [Ihre Rechnung für Microsoft Azure verstehen](billing/billing-understand-your-bill.md).

### <a name="to-change-the-size-of-the-virtual-machine"></a>Ändern die Größe des virtuellen Computers

1. Wählen Sie die Registerkarte **Konfiguration** .

1. Wählen Sie in der Liste **Konfiguration** **Alle Konfigurationen**.

1. Um die Größe für den virtuellen Computer für diese Rolle auswählen, wählen Sie die entsprechende aus **virtueller Speicher** .

1. Wählen Sie die Schaltfläche **Speichern** auf der Symbolleiste auf die Dienstkonfigurationsdatei speichern.

## <a name="manage-endpoints-and-certificates-for-your-roles"></a>Endpunkte und Zertifikate für Ihre Rollen verwalten

Konfigurieren Sie Netzwerke Endpunkte durch das Protokoll die Port-Nummer angeben und für HTTPS und SSL-Zertifikatsinformationen. Versionen vor Juni 2012 unterstützt TCP, HTTP und HTTPS. Version vom Juni 2012 unterstützt die Protokolle und UDP. UDP können keine Eingabe Endpunkte Serveremulator. Sie können dieses Protokoll nur für interne Endpunkte verwenden.

Zur Verbesserung der Sicherheit von Azure-Cloud-Dienst können Sie Endpunkte erstellen, die das HTTPS-Protokoll verwenden. Beispielsweise haben Sie einen Cloud-Dienst, Kunden mit der Bestellung, möchten Sie sicherstellen, dass ihre Informationen sicher über SSL.

Sie können auch Endpunkte hinzufügen, die intern verwendet oder extern. Externer Endpunkte heißen input Endpunkte. Anliegenden Eingabe ermöglicht einem anderen Zugriffspunkt für Benutzer zu Ihrem Clouddienst. Haben Sie einen WCF-Dienst möchten Sie eines internen Endpunkts für eine Webrolle auf diesen Dienst zugreifen.

>[AZURE.IMPORTANT] Sie können nur Endpunkte für alle Dienstkonfigurationen.

Wenn Sie HTTPS Endpunkte hinzufügen, müssen Sie ein SSL-Zertifikat verwenden. Dazu können Sie Ihre Rolle für alle Dienstkonfigurationen Zertifikate zuordnen und diese Endpunkte verwenden.

>[AZURE.IMPORTANT] Diese Zertifikate werden mit dem Dienst nicht gepackt. Sie müssen Ihre Zertifikate separat in Azure [Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen.

Der Dienstkonfigurationen zuordnen Management-Zertifikate anwenden nur, wenn der Cloud-Dienst in Azure ausgeführt wird. Bei der Cloud-Dienst in der lokalen Ausführung wird ein Azure-Serveremulator verwaltet Standardzertifikat verwendet.

### <a name="to-add-a-certificate-to-a-role"></a>Hinzufügen ein Zertifikats zu einer Rolle

1. Wählen Sie die Registerkarte **Zertifikate** .

1. Wählen Sie in der Liste **Konfiguration** **Alle Konfigurationen**.

    >[AZURE.NOTE] Hinzufügen oder Entfernen von Zertifikaten müssen Sie alle Konfigurationen auswählen. Sie können den Namen und den Fingerabdruck für eine bestimmte Konfiguration aktualisieren, falls erforderlich.

1. Wählen Sie zum Hinzufügen eines Zertifikats für diese Rolle **Zertifikat hinzufügen** . Ein neuer Eintrag wird der Liste hinzugefügt.

1. Geben Sie im Feld **Name** den Namen für das Zertifikat.

1. Wählen Sie in der Liste **Speicherort** den Speicherort für das Zertifikat, das Sie hinzufügen möchten.

1. Wählen Sie in der Liste **Speichern** Speicher zu verwenden, um das Zertifikat auszuwählen.

1. Um das Zertifikat hinzuzufügen, wählen Sie das Auslassungszeichen (...). Das Dialogfeld **Windows-Sicherheit** .

1. Wählen Sie das Zertifikat zu verwenden, aus der Liste und klicken Sie dann auf **OK** .

    >[AZURE.NOTE] Wenn Sie ein Zertifikat aus dem Zertifikatspeicher hinzufügen, werden alle Zwischenzertifikate Konfigurationsinformationen für Sie automatisch hinzugefügt. Diese Zwischenzertifikate müssen auch in Azure hochgeladen werden, um den Dienst für SSL ordnungsgemäß konfigurieren.

1. Um ein Zertifikat zu löschen, wählen Sie das Zertifikat, und klicken Sie dann auf **Zertifikat entfernen** .

1. Wählen Sie das Symbol **Speichern** in der Symbolleiste, um diese Service-Konfigurationsdateien speichern.

### <a name="to-manage-endpoints-for-a-role"></a>Endpunkte für eine Rolle verwalten

1. Wählen Sie **aus** .

1. Wählen Sie in der Liste **Konfiguration** **Alle Konfigurationen**.

1. Wählen Sie dazu einen Endpunkt **Endpunkt hinzufügen** . Ein neuer Eintrag wird der Liste hinzugefügt.

1. Geben Sie im Feld **Name** den Namen für diesen Endpunkt verwendet werden soll.

1. Wählen Sie den gewünschten Endpunkt aus **der Liste** .

1. Wählen Sie das Protokoll für den gewünschten Endpunkt **Protokoll** aus der Liste.

1. Input Endpunkt ist im Feld **Öffentlicher Port** Geben Sie öffentlichen Port verwenden.

1. Geben Sie im Textfeld **Port Private** privaten Port verwenden.

1. Wenn der Endpunkt das Https-Protokoll erfordert, in der Liste **Name des SSL-Zertifikats** Auswählen eines Zertifikats

    >[AZURE.NOTE] Diese Liste enthält die Zertifikate, die für diese Rolle in der Registerkarte **Zertifikate** hinzugefügt wurden.

1. Wählen Sie die Schaltfläche **Speichern** auf der Symbolleiste auf diese Service-Konfigurationsdateien speichern.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr über Azure-Projekten in Visual Studio anhand der [Konfiguration von Azure-Projekt](vs-azure-tools-configuring-an-azure-project.md). Erfahren Sie mehr zum Cloud-Dienstschema [Schema Reference](https://msdn.microsoft.com/library/azure/dd179398)lesen.
