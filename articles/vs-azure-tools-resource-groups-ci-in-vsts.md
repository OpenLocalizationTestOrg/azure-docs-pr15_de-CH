<properties
    pageTitle="Fortlaufende Integration in Azure-Ressourcengruppe mit VS Team Services | Microsoft Azure"
    description="Beschreibt die fortlaufende Integration in Visual Studio Team Services mithilfe der Azure-Ressourcengruppe Bereitstellungsprojekte in Visual Studio eingerichtet."
    services="visual-studio-online"
    documentationCenter="na"
    authors="mlearned"
    manager="erickson-doug"
    editor="" />

 <tags
    ms.service="azure-resource-manager"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/01/2016"
    ms.author="mlearned" />

# <a name="continuous-integration-in-visual-studio-team-services-using-azure-resource-group-deployment-projects"></a>Fortlaufende Integration in Visual Studio Team Services mit Azure-Ressourcengruppe Bereitstellungsprojekte

Zum Bereitstellen einer Azure Vorlage müssen Sie Aufgaben der verschiedenen Phasen durchlaufen: Builds, Tests in Azure kopieren (auch als "Staging" bezeichnet) und bereitstellen.  Es gibt zwei unterschiedliche Arten im Visual Studio Team Services (VS Team Services). Beide Methoden liefern die gleichen Ergebnisse, so wählen Sie das Ihrem Workflow am besten entspricht.

-   Die Builddefinition, die PowerShell-Skript ausgeführt wird, das im Bereitstellungsprojekt Azure-Ressourcengruppe (Deploy-AzureResourceGroup.ps1) enthalten fügen Sie einen Schritt hinzu. Das Skript kopiert Artefakte und stellt die Vorlage.
-   Fügen Sie mehrerer VS Team Services Schritte jeweils eine Stufe Aufgabe erstellen hinzu.

Dieser Artikel veranschaulicht, wie die erste Option (verwenden Sie eine Builddefinition zum Ausführen des PowerShell-Skripts). Ein Vorteil dieser Option ist, dass das Skript vom Entwickler in Visual Studio verwendet dasselbe Skript VS Team Services verwendet wird. Es wird vorausgesetzt, dass Sie bereits ein Visual Studio-Bereitstellungsprojekt in VS Team Services eingecheckt.

## <a name="copy-artifacts-to-azure"></a>Artefakte in Azure kopieren 

Unabhängig von dem Szenario haben Sie alle Elemente, die für die Bereitstellung der Vorlage müssen Sie Azure-Ressourcen-Manager zugreifen können. Diese Artefakte können Dateien umfassen:

-   Verschachtelte Vorlagen
-   Konfigurationsskripts und DSC-Skripts
-   Binärdateien der Anwendung

### <a name="nested-templates-and-configuration-scripts"></a>Geschachtelte Vorlagen und Konfigurationsskripts
Verwendung die Vorlagen von Visual Studio oder mit Visual Studio erstellt, PowerShell-Skript nicht nur Stufen Artefakte, auch parametrisiert URI der Ressourcen für verschiedene Deployments. Das Skript dann kopiert Artefakte in einem sicheren Container in Azure erstellt ein SAS-Token für den betreffenden Container und übergibt diese Informationen an die vorlagenbereitstellung. Finden Sie mehr verschachtelte Vorlagen [eine vorlagenbereitstellung zu erstellen](https://msdn.microsoft.com/library/azure/dn790564.aspx) .

## <a name="set-up-continuous-deployment-in-vs-team-services"></a>Einrichten der kontinuierlichen Bereitstellung in VS Team Services

PowerShell-Skript in VS Team Services aufrufen, müssen Sie die Builddefinition aktualisieren. Kurz gesagt, sind die Schritte: 

1.  Bearbeiten Sie die Builddefinition.
1.  Azure Autorisierung in VS Team Services einrichten.
1.  Hinzufügen einer Azure PowerShell Buildschritt, die PowerShell-Skript im Bereitstellungsprojekt Azure-Ressourcengruppe verweist.
1.  Legen Sie den Wert des *ArtifactsStagingDirectory -* Parameters ein Projekt VS Team Services arbeiten.

### <a name="detailed-walkthrough"></a>Ausführliche Anleitung

Die folgenden Schritte führt Sie durch die erforderlichen Schritte zum kontinuierlichen Bereitstellung in VS Team Services konfigurieren 

1.  Bearbeiten Sie VS Team Services Builddefinition und fügen Sie einer Azure PowerShell Buildschritt hinzu. Wählen Sie die Builddefinition Kategorie **Builddefinitionen** und dann den Link **Bearbeiten** .

    ![][0]

1.  Die Builddefinition ein neuer Buildschritt **Azure PowerShell** hinzu, und wählen Sie **Add Buildschritt...** Schaltfläche.

    ![][1]

1.  Wählen Sie die **Bereitstellungsaufgabe** aus, wählen Sie **Azure PowerShell** -Vorgang und wählen Sie die Schaltfläche **Hinzufügen** .

    ![][2]

1.  Wählen Sie **Azure PowerShell** Buildschritt aus und dann die Werte.

    1.  Wenn Sie bereits ein Azure Dienstendpunkt VS Team Services hinzugefügt haben, wählen Sie das Abonnement in der **Azure-Abonnement** Dropdown-Listenfeld und fahren Sie mit dem nächsten Abschnitt. 

        Haben Sie einen Azure-Endpunkt in VS Team Services, müssen Sie einen hinzufügen. Dieser Unterabschnitt führt Sie durch den Prozess. Azure-Konto ein Microsoft-Konto (wie Hotmail) verwendet, müssen Sie einen Dienstprinzipalnamen Authentifizierung zu gehen.

    1.  Wählen Sie der Link neben der **Azure-Abonnement** **Verwalten** Dropdown-Listenfeld.

        ![][3]

    1. Wählen Sie im Dropdownlistenfeld **Neuer Dienstendpunkt** **Azure** .

        ![][4]

    1.  Klicken Sie im Dialogfeld **Azure-Abonnement hinzufügen** die Option **Service Principal** .

        ![][5]

    1.  Fügen Sie Ihre Azure Abonnementinformationen im Dialogfeld **Azure-Abonnement hinzufügen** . Sie müssen Folgendes angeben:
        -   Abonnement-Id
        -   Namen
        -   Dienstprinzipalnamen-Id
        -   Dienstprinzipalnamen Schlüssel
        -   Mandanten-Id

    1.  Feld **Abonnement** einen Namen Ihrer Wahl hinzufügen. Dieser Wert wird später in der Dropdownliste **Azure-Abonnement** in VS Team Services angezeigt. 

    1.  Wenn Sie Ihre Azure-Abonnement-ID nicht kennen, können einen der folgenden Befehle Sie zu.
        
        PowerShell-Skripts verwenden:

        `Get-AzureRmSubscription`

        Azure-Befehlszeilenschnittstelle verwenden:

        `azure account show`
    

    1.  Um einen Dienstprinzipalnamen-ID zu erhalten, gehen Sie Dienstprinzipalnamen Schlüssel und Mandanten-ID [mithilfe von Active Directory erstellen Anwendung und Dienstprinzipalnamen Portal](resource-group-create-service-principal-portal.md) oder [Authentifizierung Dienstprinzipalnamen mit Azure-Ressourcen-Manager](resource-group-authenticate-service-principal.md).

    1.  Das Dialogfeld **Azure-Abonnement hinzufügen** fügen Sie Dienstprinzipalnamen ID Dienstprinzipalnamen Schlüssel und Mandanten-ID Werte hinzu, und klicken Sie dann auf **OK** .

        Sie haben nun einen gültigen Dienstprinzipalnamen Azure PowerShell-Skript ausführen.

1.  Bearbeiten Sie die Builddefinition, und wählen Sie **Azure PowerShell** Buildschritt. Wählen Sie den Dauerauftrag im Dropdownlistenfeld **Azure-Abonnement** . (Wenn das Abonnement nicht angezeigt wird, wählen Sie **Aktualisieren** weiter Link **Verwalten** .) 

    ![][8]

1.  Geben Sie einen Pfad zum Bereitstellen AzureResourceGroup.ps1 PowerShell-Skript. Dazu wählen Sie Auslassungszeichen (...) neben dem Feld **Pfad aus** , navigieren Sie zum Bereitstellen AzureResourceGroup.ps1 PowerShell-Skript im Ordner " **Scripts** " des Projekts wählen Sie aus und wählen Sie dann auf **OK** . 

    ![][9]

1. Nach Auswahl des Skripts aktualisieren Sie den Pfad zum Skript, damit sie von Build.StagingDirectory (gleiches Verzeichnis, das auf *ArtifactsLocation* ) ausgeführt wird. Sie können hierzu hinzufügen "$(Build.StagingDirectory)/"an den Pfad.

    ![][10]

1.  Geben Sie im Feld **Skriptargumente** die folgenden Parameter (in einer einzigen Zeile). Beim Ausführen des Skripts in Visual Studio können Sie sehen, wie VS Parameter im **Ausgabefenster** verwendet. Dies können als Ausgangspunkt die Parameter in der Buildschritt festlegen.

  	| Parameter | Beschreibung|
  	|---|---|
  	| -ResourceGroupLocation           | Der Standort-Wert die Ressourcengruppe, **Eastus** oder **' OST US '**befindet. (Fügen Sie Anführungszeichen befindet sich ein Leerzeichen im Namen.) Weitere Informationen finden Sie in der [Azure-Regionen](https://azure.microsoft.com/en-us/regions/) .|                                                                                                                                                                                                                              |
  	| -ResourceGroupName               | Der Name der Ressourcengruppe für die Bereitstellung verwendet.|                                                                                                                                                                                                                                                                                                                                                                                                                |
  	| -UploadArtifacts                 | Dieser Parameter gibt vorhanden, Artefakte aus dem lokalen System in Azure hochgeladen werden müssen. Sie müssen nur diese Schalter erfordert die vorlagenbereitstellung zusätzliche Elemente, die Sie mithilfe des PowerShell-Skripts (oder Konfigurationsskripts verschachtelte Vorlagen) bereitstellen möchten.                                                                                                                                                                 |
  	| -StorageAccountName              | Der Name des Speicherkontos zur Bühne Artefakte für die Bereitstellung. Dieser Parameter ist erforderlich, wenn Sie Artefakte in Azure kopieren. Dieses Speicherkonto nicht automatisch von der Bereitstellung erstellt, muss bereits vorhanden.|                                                                                                                                                                                                                     |
  	| -StorageAccountResourceGroupName | Der Name der Ressourcengruppe Storage-Konto zugeordnet. Dieser Parameter ist erforderlich, wenn Sie Artefakte in Azure kopieren.|                                                                                                                                                                                                                                                                                                                               |
  	| TemplateFile-                    | Der Pfad der Vorlagendatei in der Azure-Ressourcengruppe Bereitstellungsprojekt. Um die Flexibilität zu erhöhen, verwenden Sie einen Pfad für diesen Parameter ist relativ zum Speicherort des PowerShell-Skripts anstelle absoluter Pfad.|
  	| -TemplateParametersFile          | Pfad zur Parameterdatei im Bereitstellungsprojekt Azure-Ressourcengruppe. Um die Flexibilität zu erhöhen, verwenden Sie einen Pfad für diesen Parameter ist relativ zum Speicherort des PowerShell-Skripts anstelle absoluter Pfad.|
  	| -ArtifactStagingDirectory        | Dieser Parameter ermöglicht die PowerShell Skripts kennen den Ordner aus, in dem das Projekt binäre Dateien kopiert werden sollen. Dieser Wert überschreibt den Standardwert von PowerShell-Skript. Für VS Team Services verwenden, legen Sie den Wert auf: ArtifactStagingDirectory - $(Build.StagingDirectory)                                                                                                                                                                                              |

    Hier ist ein Skript Argumente Beispiel (Zeile zur besseren Lesbarkeit aufgeteilt):

    ``` 
    -ResourceGroupName 'MyGroup' -ResourceGroupLocation 'eastus' -TemplateFile '..\templates\azuredeploy.json' 
    -TemplateParametersFile '..\templates\azuredeploy.parameters.json' -UploadArtifacts -StorageAccountName 'mystorageacct' 
    –StorageAccountResourceGroupName 'Default-Storage-EastUS' -ArtifactStagingDirectory '$(Build.StagingDirectory)' 
    ```

    Wenn Sie fertig sind, sollte das Feld **Skriptargumente** folgendermaßen aussehen.

    ![][11]

1.  Nachdem Sie, dass alle erforderlichen Elemente in Azure PowerShell Buildschritt hinzugefügt haben, wählen Sie die **Warteschlange** erstellen Schaltfläche, um das Projekt zu erstellen. **Build** -Bildschirm zeigt die Ausgabe von PowerShell-Skript.

## <a name="next-steps"></a>Nächste Schritte

[Übersicht über Azure Ressource-Manager](azure-resource-manager/resource-group-overview.md) Weitere Informationen zu Azure Resource Manager und Azure Ressourcengruppen lesen.


[0]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough1.png
[1]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough2.png
[2]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough3.png
[3]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough4.png
[4]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough5.png
[5]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough6.png
[8]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough9.png
[9]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough10.png
[10]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough11b.png
[11]: ./media/vs-azure-tools-resource-groups-ci-in-vsts/walkthrough12.png
