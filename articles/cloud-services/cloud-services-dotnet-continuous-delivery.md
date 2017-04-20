<properties
    pageTitle="Kontinuierliche Bereitstellung Cloud-Dienste mit TFS in Azure | Microsoft Azure"
    description="Informationen Sie zum Einrichten der kontinuierlichen Bereitstellung Azure-Cloud-Apps. Codebeispiele für MSBuild-Befehlszeilen-Anweisungen und PowerShell-Skripts."
    services="cloud-services"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/30/2016"
    ms.author="tarcher"/>

# <a name="continuous-delivery-for-cloud-services-in-azure"></a>Kontinuierliche Bereitstellung für Cloud-Services in Azure

In diesem Artikel beschriebene Verfahren veranschaulicht die kontinuierlichen Bereitstellung Azure-Cloud-Apps einrichten. Dabei können Sie automatisch Pakete erstellen und Bereitstellen des Pakets in Azure jeder Code einchecken. In diesem Artikel beschriebenen Paket-Buildprozess entspricht dem Befehl **Verpacken** in Visual Studio und publishing Schritte entsprechen den Befehl **Veröffentlichen** in Visual Studio.
Artikel umfasst Methoden gewünschte einen Buildserver MSBuild-Befehlszeilen-Anweisungen mit Windows PowerShell-Skripts erstellen und es veranschaulicht, wie Visual Studio Team Foundation Server - Team Build-Definitionen der MSBuild-Befehle und PowerShell-Skripts optional konfigurieren. Für Ihre Buildumgebung und Azure Ziel Umgebung angepasst ist.

Sie können auch Visual Studio Team Services Version von TFS, die in Azure leichter dazu gehostet wird. Weitere Informationen finden Sie in der [Kontinuierlichen Bereitstellung in Azure mithilfe von Visual Studio Team Services][].

Veröffentlichen Sie vor der Anwendung von Visual Studio.
Dadurch wird sichergestellt, dass alle Ressourcen verfügbar und initialisiert werden, wenn Sie versuchen, die Publikation zu automatisieren.

## <a name="1-configure-the-build-server"></a>1: den Buildserver konfigurieren

Bevor Sie Azure Paket mit MSBuild erstellen können, müssen Sie die erforderliche Software und Tools auf dem Buildserver installieren.

Visual Studio ist nicht auf dem Buildserver installiert werden. Wenn Sie Team Foundation Build-Dienst zum Verwalten der Buildserver verwenden möchten, gehen Sie in der [Team Foundation Build Service][] -Dokumentation.

1.  Installieren Sie auf dem Buildserver [.NET Framework 4.5.2][]MSBuild enthält.
2.  Installieren Sie die neuesten [Azure Authoring-Tools für .NET](https://azure.microsoft.com/develop/net/).
3.  Installieren der [Azure-Bibliotheken für .NET](http://go.microsoft.com/fwlink/?LinkId=623519).
4.  Kopieren Sie die Datei Microsoft.WebApplication.targets aus einer Visual Studio-Installation auf den Buildserver.

    Auf einem Computer mit Visual Studio installiert diese Datei befindet sich im Verzeichnis C:\\Programm Files(x86)\\MSBuild\\Microsoft\\Visual Studio\\v14.0\\Webapplikationen. Sie sollten das in das gleiche Verzeichnis auf dem Buildserver kopieren.
5.  [Azure Tools für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx)installieren.

## <a name="2-build-a-package-using-msbuild-commands"></a>2: erstellen ein Pakets mithilfe von MSBuild-Befehle

Dieser Abschnitt beschreibt einen MSBuild-Befehl erstellen, der ein Azure-Paket erstellt. Führen Sie diesen Schritt auf dem Buildserver, um sicherzustellen, dass alles richtig konfiguriert ist und der MSBuild-Befehl mit dem gewünschten. Sie können diese Befehlszeile zu vorhandenen Buildskripts auf dem Buildserver hinzufügen oder der Befehlszeile in eine TFS-Builddefinition können wie im nächsten Abschnitt beschrieben. Weitere Informationen zu Befehlszeilenparametern und MSBuild finden Sie unter [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

1.  Wenn Visual Studio auf dem Buildserver installiert ist, suchen Sie und wählen Sie im Ordner **Visual Studio Tools** Windows **Visual Studio Befehl Prompt** .

    Wenn Visual Studio nicht auf dem Buildserver installiert ist, öffnen Sie ein Eingabeaufforderungsfenster, und sicherstellen Sie, dass MSBuild.exe im Pfad zugegriffen werden kann. MSBuild mit.NET Framework im Pfad %WINDIR% installiert\\Microsoft.NET\\Framework\\*Version*. Um MSBuild.exe Umgebungsvariable PATH hinzuzufügen, wenn Sie.NET Framework 4 installiert haben, beispielsweise den folgenden Befehl in die Befehlszeile eingeben:

        set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"

2.  Navigieren Sie in der Befehlszeile zu dem Ordner mit der Azure-Projektdatei, die Sie erstellen möchten.

3.  Führen Sie MSBuild mit der/target: Option wie folgt veröffentlichen:

        MSBuild /target:Publish

    Diese Option kann abgekürzt t: veröffentlichen. Die /t:Publish-Option in MSBuild nicht verwechseln mit in Visual Studio verfügbaren Befehle veröffentlichen bei Azure SDK installiert. / T: Option nur Builds Azure Pakete veröffentlichen. Es stellt die Pakete nicht wie Veröffentlichen Befehle in Visual Studio bereit.

    Optional können Sie den Projektnamen als MSBuild-Parameter angeben. Wenn nicht angegeben, wird das aktuelle Verzeichnis verwendet. Weitere Informationen zu MSBuild-Befehlszeilenoptionen finden Sie unter [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).

4.  Suchen Sie die Ausgabe. Dieser Befehl erstellt ein Verzeichnis in Bezug auf den Stammordner des Projekts, wie *ProjectDir*\\Bin\\*Konfiguration*\\app.publish\\. Wenn ein Azure-Projekt erstellen, erstellen Sie zwei Dateien, die Paketdatei selbst und der zugehörigen Konfigurationsdatei:

    -   Project.cspkg
    -   ServiceConfiguration. *TargetProfile*.cscfg

    Standardmäßig jede Azure-Projekt enthält eine Dienstkonfigurationsdatei (.cscfg-Datei) für lokale (debugging) Builds und Cloud (Staging- oder Produktionsserver) Builds hinzufügen oder entfernen werden nach Bedarf Beim Erstellen eines Pakets in Visual Studio werden die Dienstkonfigurationsdatei neben das Paket aufgefordert.

5.  Geben Sie die Dienstkonfigurationsdatei. Beim Erstellen eines Pakets mit MSBuild ist die Konfigurationsdatei Dienstleistung standardmäßig enthalten. Um verschiedene Dienstkonfigurationsdatei einzubeziehen, legen Sie die TargetProfile-Eigenschaft des MSBuild-Befehls wie im folgenden Beispiel:

        MSBuild /t:Publish /p:TargetProfile=Cloud

6.  Geben Sie den Speicherort für die Ausgabe. Legen Sie den Pfad mit der /p:PublishDir =*Verzeichnis* \\ Option, darunter nachgestellten umgekehrten Schrägstrich Trennzeichen, wie im folgenden Beispiel:

        MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

    Sobald Sie erstellt und eine entsprechende MSBuild-Befehlszeile zu-Projekte in Azure Paket kombiniert getestet haben, können Sie Ihre Buildskripts Befehlszeile hinzufügen. Wenn der Buildserver benutzerdefinierte Skripts verwendet, hängt dabei die Besonderheiten des benutzerdefinierten Buildprozesses. Bei Verwendung von TFS als Build-Umgebung können Sie der Anleitung im nächsten Schritt des Buildprozesses Azure Paket-Builds hinzufügen folgen.

## <a name="3-build-a-package-using-tfs-team-build"></a>3: erstellen ein Pakets mit TFS Team Build

Haben Sie Team Foundation Server (TFS) als einen Buildcontroller und Build-Server als Buildcomputer TFS eingerichtet und können optional festlegen einen automatisierten Build-für Ihre Azure-Paket. Informationen zum Einrichten und Verwenden von Team Foundation Server als ein Buildsystem finden Sie unter [Skalierung des Buildsystems][]. Insbesondere nimmt folgendermaßen konfiguriert der Buildserver unter [Bereitstellen und einen Buildserver konfigurieren][], und ein Teamprojekt erstellt haben ein Cloud-Dienstprojekt im Teamprojekt erstellt.

Konfigurieren Sie TFS zum Azure-Pakete erstellen, führen Sie die folgenden Schritte:

1.  In Visual Studio auf dem Entwicklungscomputer im Menü Ansicht wählen Sie **Team Explorer**, oder drücken Sie STRG +\\, STRG + M. Im Fenster Team Explorer-Knoten **erstellt** oder wählen Sie die Seite **erstellt** , und **Neue Builddefinition**.

    ![Neue Builddefinition option][0]

2.  Wählen Sie die Registerkarte **Auslöser** und geben Sie gewünschten Optionen für an, wenn das Paket erstellt werden soll. Beispielsweise **Fortlaufende Integration** , um das Paket zu erstellen, wenn ein Datenquellen-Steuerelement einchecken.

3.  Wählen Sie die Registerkarte **Einstellungen** und sicherstellen Sie Projektordner in der Spalte " **Quellcodeverwaltungsordner"** aufgeführt ist, und der Status ist **aktiv**.

4.  Wählen Sie die Registerkarte **Build-Standardwerte** und unter Buildcontroller, überprüfen Sie den Namen des Servers erstellen.  Wählen Sie die Option **Kopie Buildausgabe in folgenden Ablageordner** auch anzugeben Sie den gewünschten Speicherort.

5.  Wählen Sie die Registerkarte **Prozess** . Wählen Sie auf der Registerkarte Prozess Standardvorlage unter **Build aus**, wählen Sie das Projekt aus, falls es nicht bereits ausgewählt, und erweitern Sie den Abschnitt **Erweitert** im Abschnitt **Erstellen** des Rasters.

6.  Wählen Sie **MSBuild-Argumente**und festlegen Sie die entsprechenden MSBuild-Befehlszeilenargumente, wie in Schritt2 oben beschrieben. Geben Sie beispielsweise **t: Veröffentlichen /p:PublishDir =\\\\Myserver\\löscht\\ ** ein Paket erstellen und kopieren die Paketdateien an den Speicherort \\ \\Myserver\\löscht\\:

    ![MSBuild-Argumente][2]

    **Hinweis:** Kopieren der Dateien in einem freigegebenen Verzeichnis erleichtert die Pakete vom Entwicklungscomputer manuell bereitstellen.

5.  Testen des Erfolgs der Buildschritt eine Änderung an Ihrem Projekt einchecken oder einen neuen Build in die Warteschlange. Um einen neuen Build in Team Explorer in die Warteschlange mit der rechten Maustaste **Alle Builddefinitionen** und dann **Neuen Build in Warteschlange**.

## <a name="4-publish-a-package-using-a-powershell-script"></a>4: Veröffentlichen eines Pakets mithilfe eines PowerShell-Skripts

Dieser Abschnitt beschreibt, wie ein Windows PowerShell-Skript zu erstellen, die mit optionalen Parametern Azure Cloud app Paket Ausgabe veröffentlicht werden. Dieses Skript kann nach dem Build Schritt in die benutzerdefinierte Automatisierung aufgerufen werden. Es kann auch von Workflowaktivitäten in Visual Studio TFS Team Build Process Template aufgerufen.

1.  Installieren der [Azure-PowerShell-Cmdlets][] (v0.6.1 oder höher).
    Wählen Sie während der Installationsphase Cmdlet als Snap-in installieren. Beachten Sie, dass diese offiziell unterstützte Version angeboten CodePlex, ältere ersetzt jedoch früheren Versionen 2.x.x nummeriert.

2.  Starten über das Startmenü Azure PowerShell oder Seite. Wenn Sie auf diese Weise starten, wird der Azure-PowerShell-Cmdlets geladen.

3.  PowerShell, überprüfen die PowerShell-Cmdlets geladen sind teilweise den Befehl `Get-Azure` und drücken die Tab-Taste für den Anweisungsabschluss.

    Wenn Sie die Tab-Taste drücken, sollte verschiedene Azure PowerShell Befehle angezeigt werden.

4.  Überprüfen Sie, ob Sie Ihre Azure-Abonnement Verbindung Ihre Abonnementinformationen aus PUBLISHSETTINGS-Datei importieren.

    `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

    Geben Sie den Befehl

    `Get-AzureSubscription`

    Zeigt Informationen zu Ihrem Abonnement. Stellen Sie sicher, dass alles in Ordnung ist.

4.  Skript am Ende dieses Artikels Ordner Skripts als c: Speichern\\Skripts\\Azure\\**PublishCloudService.ps1**.

5.  Lesen Sie den Parameterabschnitt des Skripts. Hinzufügen oder Ändern von Standardwerten. Diese Werte können durch explizite Parameterübergabe immer überschrieben.

6.  Sicherstellen Sie gültiger Cloud Service und Speicher in Ihrem Abonnement erstellt werden, die das Veröffentlichen Skript verwendet werden kann. Das Speicherkonto (BLOB-Speicher) verwendet, vorübergehend speichern Deployment Package und Config-Dateien während die Bereitstellung erstellt wird.

    -   Erstellen Sie einen neuen Clouddienst können Sie dieses Skript aufrufen oder das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885). Cloud Service-Namen als Präfix in einem vollqualifizierten Domänennamen verwendet und muss daher eindeutig sein.

            New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

    -   Erstellen Sie ein neues Speicherkonto kann dieses Skript aufrufen oder das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885). Speicher-Kontonamen als Präfix in einem vollqualifizierten Domänennamen verwendet und muss daher eindeutig sein. Sie können versuchen, mit dem gleichen Namen als Cloud-Dienst.

            New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"

7.  Rufen Sie das Skript direkt Azure PowerShell und verbinden Sie dieses Skript auf Ihrem Host Buildautomatisierung nach Paket-Builds auftreten.

    >[AZURE.IMPORTANT] Das Skript wird immer löschen oder ggf. vorhandenen Installationen standardmäßig erkannt werden. Dies muss kontinuierliche Bereitstellung von Automation, ist kein Benutzer möglich.

    **Szenario 1:** kontinuierlichen Bereitstellung in die Stagingumgebung eines Dienstes:

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    In der Regel ist Testlauf Überprüfung und VIP-Swap danach. VIP-Swap kann über das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) oder mithilfe des Cmdlets verschieben-Bereitstellung erfolgen.

    **Szenario 2:** kontinuierlichen Bereitstellung Produktionsumfeld eines dedizierten Test-Dienstes

        PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

    **Remote Desktop:**

    Wenn Remotedesktop in Azure-Projekts durchführen müssen aktiviert werden weitere einmalige Maßnahmen Zertifikat des richtigen Cloud auf alle Clouddienste dieses Ziel hochgeladen.

    Suchen Sie nach Zertifikat Fingerabdruck Werte erwartet Ihre Rollen. Fingerabdruck-Werte werden im Abschnitt Zertifikate der Cloud-Konfigurationsdatei (d. h. ServiceConfiguration.Cloud.cscfg) angezeigt. Wird auch bei der Konfiguration des Remotedesktop-Dialogfeld in Visual Studio angezeigt und das ausgewählte Zertifikat anzeigen.

        <Certificates>
              <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
        </Certificates>

    Hochladen Sie Remotedesktop Zertifikate als einmalige Einrichtung Schritt mit dem folgenden Cmdlet Skript:

        Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

    Zum Beispiel:

        Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

    Sie exportieren die Zertifikatdatei PFX mit privaten Schlüssel und Zertifikate auf jedes Ziel Cloud-Dienst über das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885)hochladen. 
    Die folgenden Artikel weitere: [http://msdn.microsoft.com/library/windowsazure/gg443832.aspx] [].

    **Bereitstellung Bereitstellung im Vergleich zu löschen - Upgrade\> neue Bereitstellung**

    Das Skript standardmäßig führt eine Bereitstellung aktualisieren ($enableDeploymentUpgrade = 1) Wenn kein Parameter übergeben wird oder der Wert 1 wird explizit übergeben. Für einzelne Instanzen hat dies den Vorteil weniger Zeit als eine vollständige Bereitstellung. Für Instanzen, die hohen Verfügbarkeit auch den Vorteil erfordern hat, dass einige andere Instanzen (gehen Ihre Domäne aktualisieren) aktualisiert und die VIP nicht gelöscht werden.

    Aktualisierung im Skript deaktiviert werden ($enableDeploymentUpgrade = 0) oder *- EnableDeploymentUpgrade 0* als Parameter übergeben, ändert das Skriptverhalten vorhandenen Bereitstellung löschen und eine neue Bereitstellung erstellen.

    >[AZURE.IMPORTANT] Das Skript wird immer löschen oder ggf. vorhandenen Installationen standardmäßig erkannt werden. Dies ist erforderlich, kontinuierliche Bereitstellung von Automation, ist keine Benutzer-Operator kann.

## <a name="5-publish-a-package-using-tfs-team-build"></a>5: Veröffentlichen eines Pakets mithilfe von TFS Team Build

Mit diesem optionale Schritt verbindet TFS Team Build das Skript in Schritt 4 erstellte die Veröffentlichung des Paket-Builds in Azure behandelt. Dazu ändern die Prozessvorlage durch die Builddefinition verwendet werden, sodass eine Aktivität veröffentlichen am Ende des Workflows ausgeführt. Die Aktivität veröffentlichen führt den PowerShell-Befehl übergeben Parameter aus dem Build. Ausgabe des MSBuild-Ziele und veröffentlichen Skript wird in die Buildausgabe standard geleitet.

1.  Verantwortlich für die Builddefinition bearbeiten kontinuierlichen bereitstellen.

2.  Wählen Sie die Registerkarte **Prozess** .

3.  Befolgen Sie [Diese Anleitung](http://msdn.microsoft.com/library/dd647551.aspx) um Aktivität Projekt für die Buildprozessvorlage, downloaden die Standardvorlage zum Projekt hinzufügen und einchecken. Geben Sie der Buildprozessvorlage einen neuen Namen wie AzureBuildProcessTemplate.

3.  Auf der Registerkarte **Prozess** zurück, und verwenden Sie zum Anzeigen einer Liste der verfügbaren Buildprozessvorlagen **Details anzeigen** . Wählen Sie die Schaltfläche **neu...** und navigieren zu dem Projekt nur hinzugefügt und eingecheckt. Suchen Sie die Vorlage nur erstellt und klicken auf **OK**.

4.  Öffnen Sie zum Bearbeiten der ausgewählten Prozessvorlage. Sie können direkt im Workflow-Designer oder im XML-Editor XAML zu öffnen.

5.  Fügen Sie folgende neue Argumente als separate Positionen im Workflow-Designer auf der Registerkarte Argumente. Alle Argumente müssen Richtung = In, und geben Sie = Zeichenfolge. Diese werden zum Datenstromparameter von der Builddefinition in den Workflow, der anschließend veröffentlichen Skript abrufen.

        SubscriptionName
        StorageAccountName
        CloudConfigLocation
        PackageLocation
        Environment
        SubscriptionDataFileLocation
        PublishScriptLocation
        ServiceName

    ![Liste der Argumente][3]

    Der entsprechende XAML-Code sieht folgendermaßen aus:

        <Activity  _ />
          <x:Members>
            <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
            <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
            <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
            <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
            <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
            <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
            <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
            <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
            <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
            <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
            <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
            <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
            <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
            <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
            <x:Property Name="GetVersion" Type="InArgument(x:String)" />
            <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
            <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
            <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
            <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
            <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
            <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
            <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
            <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
            <x:Property Name="Environment" Type="InArgument(x:String)" />
            <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
            <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
            <x:Property Name="ServiceName" Type="InArgument(x:String)" />
          </x:Members>

          <this:Process.MSBuildArguments>

6.  Hinzufügen einer neuen Sequenz am Ende auf Agent ausführen:

    1.  Zunächst eine If-Anweisung Aktivität zu prüfen, ob eine gültige Datei hinzufügen. Die Bedingung wird auf diesen Wert festgelegt:

            Not String.IsNullOrEmpty(PublishScriptLocation)

    2.  Anschließend Fall der If-Anweisung eine Sequence-Aktivität hinzufügen. Legen Sie den Anzeigenamen an"Start"

    3.  Veröffentlichen Sie mit Sequenz ausgewählt, fügen Sie folgende neue Variablen als separate Positionen im Workflow-Designer auf der Registerkarte Variablen. Alle Variablen müssen Variablentyp = Zeichenfolge und Umfang = Start veröffentlichen. Diese werden zum Datenstromparameter von der Builddefinition in den Workflow, der anschließend veröffentlichen Skript abrufen.

        -   SubscriptionDataFilePath vom Typ String

        -   PublishScriptFilePath vom Typ String

            ![Neue Variablen][4]

    4.  Bei Verwendung von TFS 2012 oder früher fügen Sie ConvertWorkspaceItem Aktivitäten am Anfang der neuen Sequenz hinzu. Bei Verwendung von TFS 2013 oder höher fügen Sie GetLocalPath Aktivitäten am Anfang der neuen Sequenz hinzu. Ein ConvertWorkspaceItem legen die Eigenschaften wie folgt fest: Richtung = ServerToLocal, DisplayName = 'Convert veröffentlichen Skript Dateiname' Eingabe = PublishScriptLocation, Ergebnis = PublishScriptFilePath, Arbeitsbereich = "Arbeitsbereich". Eine Aktivität GetLocalPath legen Sie die Eigenschaft IncomingPath auf 'PublishScriptLocation' und 'PublishScriptFilePath' das Ergebnis. Diese Aktivität konvertiert den Pfad aus TFS-Server (falls zutreffend) einen standardmäßigen lokalen Datenträgerpfad Skript veröffentlichen.

    5.  Bei Verwendung von TFS 2012 oder früher fügen Sie anderen ConvertWorkspaceItem-Aktivität am Ende der neuen Sequenz. Richtung = ServerToLocal, DisplayName = 'Konvertieren Abonnement Dateiname' Eingabe = SubscriptionDataFileLocation, Ergebnis = SubscriptionDataFilePath, Arbeitsbereich = "Arbeitsbereich". Wenn Sie TFS 2013 oder höher verwenden, fügen Sie einen anderen GetLocalPath. IncomingPath = 'SubscriptionDataFileLocation' und 'SubscriptionDataFilePath' =

    6.  Eine InvokeProcess-Aktivität am Ende der neuen Sequenz hinzufügen.
        Diese Aktivität ruft PowerShell.exe mit der Builddefinition übergebenen Argumente.

        1.  Argumente = String.Format ("-Datei""{0}'" - Dienstname {1} - StorageAccountName {2} - PackageLocation "" {3}' "- CloudConfigLocation""{4}'" - SubscriptionDataFile "" {5}' "SelectedSubscription - {6}-Umgebung""{7}'" ", PublishScriptFilePath, Dienstname, StorageAccountName, PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, Umgebung)

        2.  DisplayName = Ausführung Skript veröffentlichen

        3.  FileName = "PowerShell" (einschließlich Anführungszeichen)

        4.  OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)

    7.  Im Abschnitt **Standardausgabe** Textbox die InvokeProcess eingestellt Textbox 'Daten'. Dies ist eine Variable zum Speichern der Daten für die Standardausgabe.

    8.  Hinzufügen einer WriteBuildMessage-Aktivität nur unten die **Standardausgabe** . Legen Sie die Wichtigkeit = "Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High" und die Meldung = 'Daten'. Dadurch Standardausgabe des Skripts in die Ausgabe geschrieben wird.

    9.  Im Abschnitt **Fehlerausgabe** Textbox die InvokeProcess eingestellt Textbox 'Daten'. Dies ist eine Variable zum Speichern der Daten Standardfehler.

    10. WriteBuildError-Aktivität nur unten **Fehlerausgabe** hinzufügen. Die Nachricht = 'Daten'. Dadurch Standardfehler des Skripts in der Buildausgabe Fehler geschrieben wird.

    11. Beheben Sie alle Fehler blauen Ausrufezeichen gekennzeichnet. Mauszeiger Ausrufezeichen einen Hinweis zum Fehler abrufen. Speichern Sie den Workflow, um Fehler zu löschen.

    Das Endergebnis der Workflowaktivitäten veröffentlichen wird im Designer aussehen:

    ![Workflow-Aktivitäten][5]

    Das Endergebnis der Workflowaktivitäten veröffentlichen wird in XAML aussehen:

        <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
            <If.Then>
              <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
                <Sequence.Variables>
                  <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                  <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
                </Sequence.Variables>
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
                <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
                <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                  <mtbwa:InvokeProcess.ErrorDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.ErrorDataReceived>
                  <mtbwa:InvokeProcess.OutputDataReceived>
                    <ActivityAction x:TypeArguments="x:String">
                      <ActivityAction.Argument>
                        <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                      </ActivityAction.Argument>
                      <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                    </ActivityAction>
                  </mtbwa:InvokeProcess.OutputDataReceived>
                </mtbwa:InvokeProcess>
              </Sequence>
            </If.Then>
          </If>
        </Sequence>


7.  Speichern Sie Workflow des Vorlage erstellen und einchecken.

8.  Bearbeiten Sie die Builddefinition (schließen, wenn er bereits geöffnet ist), und wählen Sie die Schaltfläche **neu** , wenn die neue Vorlage in der Liste der Prozessvorlagen noch nicht angezeigt wird.

9.  Festlegen des Parameters Eigenschaftswerte im Abschnitt Sonstiges wie folgt:

    1.  CloudConfigLocation = "c:\\löscht\\app.publish\\ServiceConfiguration.Cloud.cscfg" *dieser Wert abgeleitet: ($PublishDir)ServiceConfiguration.Cloud.cscfg*

    2.  PackageLocation = "c:\\löscht\\app.publish\\ContactManager.Azure.cspkg" *dieser Wert abgeleitet: ($PublishDir)($ProjectName) .cspkg*

    3.  PublishScriptLocation = "c:\\Skripts\\Azure\\PublishCloudService.ps1"

    4.  ServiceName = 'Mycloudservicename' *mit der entsprechenden Cloud-Dienstname*

    5.  Umgebung = "Staging"

    6.  StorageAccountName = "Mystorageaccountname" *mit den entsprechenden Speicher-Kontonamen*

    7.  SubscriptionDataFileLocation = "c:\\Skripts\\Azure\\Subscription.xml"

    8.  SubscriptionName = "Default"

    ![Parameterwerte-Eigenschaft][6]

10. Speichern der Definition erstellen.

11. Warteschlange einen Build des Paket-Builds und veröffentlichen. Wenn Sie einen Trigger für die fortlaufende Integration festgelegt haben, wird dieses Verhalten auf alle ausgeführt werden.

### <a name="publishcloudserviceps1-script-template"></a>PublishCloudService.ps1-Skriptvorlage

```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy to $servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according to $alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress to activity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a>Nächste Schritte

Remotedebuggen Verwendung der kontinuierlichen Bereitstellung finden Sie [remote Debuggen kontinuierlichen Bereitstellung Azure veröffentlichen aktivieren](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).

  [Kontinuierliche Bereitstellung in Azure mithilfe von Visual Studio Team Services]: cloud-services-continuous-delivery-use-vso.md  
  [Team Foundation Build-Diensts]: https://msdn.microsoft.com/library/ee259687.aspx
  [.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
  [.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
  [.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
  [Das Buildsystem skalieren]: https://msdn.microsoft.com/library/dd793166.aspx
  [Bereitstellen und einen Buildserver konfigurieren]: https://msdn.microsoft.com/library/ms181712.aspx
  [Azure PowerShell-cmdlets]: powershell-install-configure.md
  [the .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
  [0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
  [2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
  [3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
  [4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
  [5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
  [6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
