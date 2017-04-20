<properties
   pageTitle="Azure Ressource Gruppe Visual Studio-Projekte | Microsoft Azure"
   description="Verwenden Sie Visual Studio Azure Ressource Projekt erstellen und Bereitstellen von Ressourcen in Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Erstellen und Bereitstellen von Azure Ressourcengruppen über Visual Studio

Mit Visual Studio und [Azure SDK](https://azure.microsoft.com/downloads/)können Sie ein Projekt erstellen, die Ihre Infrastruktur und Code in Azure bereitgestellt wird. Sie können z. B. Webhost, Website und Datenbank für die Anwendung definieren und Infrastruktur Code bereitstellen. Oder eines virtuellen Computers, virtuelles Netzwerk und Speicher-Konto definieren und Bereitstellen dieser Infrastruktur mit einem Skript, das auf dem virtuellen Computer ausgeführt wird. Das Bereitstellungsprojekt **Azure-Ressourcengruppe** können Sie die erforderlichen Ressourcen in einer einzigen, wiederholbare Operation bereitstellen. Weitere Informationen zum Bereitstellen und Verwalten von Ressourcen finden Sie unter [Azure-Ressourcen-Manager (Übersicht)](azure-resource-manager/resource-group-overview.md).

Azure-Ressourcengruppe Projekte enthalten Azure Ressourcenmanager JSON-Vorlagen, die die Ressourcen definieren, die in Azure bereitstellen. Zu den Elementen der Ressourcen-Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](resource-group-authoring-templates.md). Visual Studio können Sie Vorlagen bearbeiten und bietet Tools, die Vorlagen erleichtern.

In diesem Thema stellen Sie ein WebApp und die SQL-Datenbank. Die Schritte sind jedoch nahezu identisch für jede Ressource. Sie können als einfach einen virtuellen Computer und die zugehörigen Ressourcen bereitstellen. Visual Studio bietet viele verschiedene startvorlagen für allgemeine Szenarien bereitstellen.

Dieser Artikel zeigt Visual Studio 2015 Update 2 und Microsoft Azure SDK für .NET 2.9. Wenn Sie Visual Studio 2013 Azure SDK 2.9 verwenden, ist Ihre Erfahrung größtenteils identisch. Sie können Versionen von Azure SDK von 2.6 oder höher. jedoch kann Ihre Erfahrung der Benutzeroberfläche die Benutzeroberfläche in diesem Artikel abweichen. Es wird dringend empfohlen, dass Sie die neueste Version von [Azure SDK](https://azure.microsoft.com/downloads/) installieren, bevor Sie die Schritte. 

## <a name="create-azure-resource-group-project"></a>Azure-Ressourcengruppe erstellen Projekt

In diesem Verfahren erstellen Sie eine Ressourcengruppe Azure-Projekt mit einer Vorlage **WebApp + SQL** .

1. In Visual Studio wählen **Datei**, **Neues Projekt**, **C#** oder **Visual Basic**. Wählen Sie die **Cloud**, und wählen Sie **Ressourcengruppe Azure** -Projekt.

    ![Cloud-Bereitstellungsprojekt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Wählen Sie die Vorlage, die Sie zum Azure-Ressourcen-Manager bereitstellen möchten. Beachten Sie, dass sind verschiedene Optionen basierend auf dem Typ des Projekts, das Sie bereitstellen möchten. Wählen Sie für dieses Thema die Vorlage **WebApp + SQL** .

    ![Wählen Sie eine Vorlage](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Die Vorlage, die Sie auswählen, ist nur ein Ausgangspunkt. Sie können hinzufügen und Entfernen von Ressourcen, um das Szenario zu erfüllen.

    >[AZURE.NOTE] Visual Studio ruft eine Liste der verfügbaren Vorlagen online. Die Liste möglicherweise ändern.

    Visual Studio erstellt eine Ressource Gruppe Bereitstellungsprojekt für WebApp und SQL-Datenbank.

1. Wenn Sie erstellt, erweitern Sie die Knoten im Bereitstellungsprojekt.

    ![Knoten anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Da wir WebApp + SQL-Vorlage für dieses Beispiel haben, sehen Sie die folgenden Dateien: 

  	|Dateiname|Beschreibung|
  	|---|---|
  	|Bereitstellen von AzureResourceGroup.ps1|Ein Powershellskript, das PowerShell-Befehlen, Azure-Ressourcen-Manager bereitgestellt wird.<br />**Hinweis** Visual Studio verwendet das PowerShell-Skript die Vorlage bereitstellen. Jede Änderung an diesem Skript beeinflussen die Bereitstellung in Visual Studio, achten.|
  	|WebSiteSQLDatabase.json|Der Ressourcen-Manager-Vorlage, die die Infrastruktur definiert soll Azure und während der Bereitstellung bieten Parameter bereitstellen. Es definiert auch Abhängigkeiten Ressourcen damit Ressourcenmanager die Ressourcen in der richtigen Reihenfolge verwendet werden.|
  	|WebSiteSQLDatabase.parameters.json|Eine Datei, die Werte von der Vorlage enthält. Sie übergeben Sie Parameterwerte jede Bereitstellung anpassen.|

    Alle Ressourcen Gruppe Bereitstellungsprojekte enthalten die folgenden grundlegenden Dateien. Andere Projekte enthalten zusätzliche Dateien andere Funktionen unterstützen.

## <a name="customize-the-resource-manager-template"></a>Anpassen der Ressourcen-Manager-Vorlage

Einem Bereitstellungsprojekt können durch Ändern der JSON-Vorlagen, die die Ressourcen beschreiben, die Sie bereitstellen möchten. JSON steht für JavaScript Objektnotation und ist eine serialisierte Daten problemlos. Die JSON-Dateien verwenden ein Schema, das Sie am Anfang jeder Datei verweisen. Ggf. das Schema verstehen können herunterladen und analysieren. Das Schema definiert, welche Elemente gültig sind die Typen und Formate der Felder, die möglichen Werte der Enumerationswerte usw.. Zu den Elementen der Ressourcen-Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](resource-group-authoring-templates.md).

Um Ihre Vorlage zu bearbeiten, öffnen Sie **WebSiteSQLDatabase.json**.

Der Visual Studio-Editor bietet Tools zur Unterstützung bei der Bearbeitung der Vorlage Ressourcenmanager. **JSON** Übersichtsfenster erleichtert die Elemente in der Vorlage definiert.

![JSON Gliederung anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Wählen eines der Elemente in der Gliederung gelangen Sie zu dem entsprechenden Teil der Vorlage und hebt die entsprechende JSON.

![JSON navigieren](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Sie können eine Ressource wählen Sie **Ressource hinzufügen** -Schaltfläche am oberen Rand des Übersichtsfensters JSON oder **Ressourcen** und wählen **Neue Ressource hinzufügen**hinzufügen.

![Ressource hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Für dieses Lernprogramm **Speicherkonto** auswählen und einen Namen. Geben Sie einen Namen, die mehr als 11 Zeichen enthält nur Zahlen und Kleinbuchstaben.

![Hinzufügen von Speicher](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Die Ressource war nicht nur beachten, sondern auch einen Parameter für das Speicherkonto Typ und eine Variable für den Namen des Speicherkontos.

![Gliederung anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Der Parameter **StorageType** ist standardmäßig mit zulässigen Typen. Sie können diese Werte oder für Ihr Szenario bearbeiten. Wenn Sie nicht alle ein Speicherkonto **Premium_LRS** durch diese Vorlage bereitstellen möchten, trennen sie den zulässigen Typen. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio bietet außerdem Intellisense, um besser zu verstehen, welche Eigenschaften verfügbar sind, wenn die Vorlage bearbeiten. Bearbeiten Sie die Eigenschaften für Ihren Dienstplan App **Vorlagen** Ressource navigieren Sie, und fügen Sie **Eigenschaften**aus. Beachten Sie, dass Intellisense zeigt die verfügbaren Werte sowie eine Beschreibung des Werts.

![Intellisense anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

Sie können **NumberOfWorkers** auf 1 festlegen.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Ressourcengruppe in Azure bereitstellen

Sie können nun das Projekt bereitstellen. Wenn eine Ressourcengruppe Azure-Projekt bereitstellen, stellen Sie es Azure Ressourcengruppe. Die Ressourcengruppe ist eine logische Gruppierung von Ressourcen, die einen gemeinsamen Lebenszyklus aufweisen.

1. Wählen Sie im Kontextmenü des Knotens Projekt Bereitstellung **Bereitstellen** > **Neue Bereitstellung**.

    ![Neue Bereitstellung im Menüelement bereitstellen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Das Dialogfeld **Ressourcengruppe bereitstellen** .

    ![Das Dialogfeld Ressourcengruppe bereitstellen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. In der Dropdownliste **Ressourcengruppe** eine vorhandene Ressourcengruppe auswählen oder einen neuen erstellen. Erstellen Sie eine Ressourcengruppe aus öffnen Sie der Dropdownliste **Ressourcengruppe** , und wählen Sie **Neu erstellen**.

    ![Das Dialogfeld Ressourcengruppe bereitstellen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Das Dialogfeld **Ressourcengruppe erstellen** . Geben Sie einen Namen und Speicherort Ihrer Gruppe, und wählen Sie die Schaltfläche **Erstellen** .

    ![Ressourcengruppe erstellen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Bearbeiten Sie die Parameter für die Bereitstellung durch Auswählen der Schaltfläche **Parameter bearbeiten** .

    ![Schaltfläche Parameter bearbeiten](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Geben Sie Werte für die Parameter leer, und wählen Sie die Schaltfläche **Speichern** . Die leeren Parameter sind **HostingPlanName**, **AdministratorLogin**, **AdministratorLoginPassword**und **Datenbankname**.

    **HostingPlanName** gibt einen Namen für die [App Service-Plan](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) erstellen. 
    
    **AdministratorLogin** gibt den Namen der SQL Server-Administrator. Verwenden Sie allgemeine Admin Namen wie **sa** oder **Admin**. 
    
    **AdministratorLoginPassword** gibt ein Kennwort für den SQL Server-Administrator. Die Option **Speichern Kennwörter als Klartext in der Datei** ist nicht sicher. Daher wählen Sie diese Option nicht. Da das Kennwort nicht als Klartext gespeichert wird, müssen Sie dieses Kennwort während der Bereitstellung erneut bereitstellen. 
    
    **Datenbankname** gibt einen Namen für die Datenbank erstellen. 

    ![Im Dialogfeld Parameter bearbeiten](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Wählen Sie das Projekt in Azure bereitgestellt **Bereitstellen** . PowerShell-Konsole wird außerhalb der Visual Studio-Instanz geöffnet. Geben Sie das Administratorkennwort für SQL Server in der PowerShell-Konsole Aufforderung. **PowerShell-Konsole durch andere Objekte verdeckt oder in der Taskleiste minimiert.** Diese Konsole suchen Sie und wählen Sie aus, das Kennwort einzugeben.

    >[AZURE.NOTE] Visual Studio bitten Sie Azure PowerShell-Cmdlets installieren. Sie benötigen den Azure PowerShell-Cmdlets zur erfolgreichen Bereitstellung von Ressourcengruppen. Wenn Sie aufgefordert werden, installieren Sie sie.
    
1. Die Bereitstellung kann einige Minuten dauern. Im Fenster **Ausgabe** wird der Status der Bereitstellung angezeigt. Nach Abschluss die Bereitstellung gibt die letzte Meldung eine erfolgreiche Bereitstellung mit ähnlich:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. In einem Browser öffnen [Azure-Portal](https://portal.azure.com/) , und melden Sie sich bei Ihrem Konto. Wählen Sie die Ressourcengruppe finden **Ressourcengruppen** und die Ressourcengruppe, der Sie bereitgestellt.

    ![Wählen Sie aus](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Sie können die bereitgestellten Ressourcen. Beachten Sie, dass das Speicherkonto nicht ist was Sie beim Hinzufügen der Ressource angegeben. Das Speicherkonto muss eindeutig sein. Die Vorlage fügt automatisch eine Zeichenfolge hinzu das angegebene Name einen eindeutigen Namen zu. 

    ![Ressourcen anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Im Kontextmenü der Azure-Ressource Projekt ändern und das Projekt erneut bereitstellen möchten wählen Sie vorhandene Ressourcengruppe aus. Klicken Sie im Kontextmenü wählen Sie **Bereitstellen**, und dann die Ressourcengruppe, die Sie bereitgestellt haben.

    ![Azure-Ressourcengruppe bereitgestellt](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Code in Ihre Infrastruktur bereitstellen

An dieser Stelle die Infrastruktur für Ihre Anwendung bereitgestellt haben, aber keinen tatsächlichen Code mit dem Projekt bereitgestellt. Dieses Thema zeigt, wie Web app und SQL-Datenbanktabellen während der Bereitstellung bereitstellen. Bei der Bereitstellung einer virtuellen Maschine Web App Code auf dem Computer als Teil der Bereitstellung ausgeführt werden soll. Der Prozess für die Bereitstellung von Code für eine Webanwendung oder eine virtuelle Maschine ist nahezu identisch.

1. Der Visual Studio-Projektmappe ein Projekt hinzufügen. Maustaste auf die Projektmappe und wählen **Hinzufügen** > **Neues Projekt**.

    ![Projekt hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Hinzufügen einer **ASP.NET Web-Anwendung**. 

    ![Hinzufügen einer Web-Applikation](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Aktivieren Sie **MVC** und deaktivieren Sie das Feld für **Host in der Cloud** , da Projektgruppe Ressource dem Vorgang ausgeführt.

    ![Wählen Sie MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Nachdem Visual Studio Ihrer Anwendung erstellt wurde, sehen Sie beide Projekte in der Projektmappe.

    ![Projekte anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Nun müssen Sie sicherstellen, dass die Ressource Gruppenprojekt des neuen Projekts bekannt ist. Zurück zur Ressource Gruppenprojekt (AzureResourceGroup1). Auf **Verweise** und wählen Sie **Verweis hinzufügen**.

    ![Verweis hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Wählen Sie Web-app-Projekt, das Sie erstellt haben.

    ![Verweis hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Durch Hinzufügen eines Verweises Web-app-Projekt, Projektgruppe Ressource verknüpfen und drei Haupteigenschaften automatisch festgelegt. Sie sehen diese Eigenschaften im **Eigenschaftenfenster** für den Verweis.

      ![finden Sie unter Referenz](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Die Eigenschaften sind:

    - Die **Zusätzliche Eigenschaften** enthält Webbereitstellungspaket staging Speicherort auf den Azure-Speicher abgelegt. Beachten Sie die Ordner (ExampleApp) und (package.zip). Sie werden diese Werte als Parameter angeben die Anwendung bereitstellen. 
    - Der **Dateipfad enthalten** enthält den Pfad, in dem das Paket erstellt wird. Die **Ziele umfassen** enthält den Befehl, den Bereitstellung ausgeführt wird. 
    - Der Standardwert von **Erstellen; Paket** ermöglicht die Bereitstellung zu erstellen, und erstellen ein Webbereitstellungspaket (package.zip).  
    
    Sie brauchen kein Veröffentlichungsprofil, wie die Bereitstellung von Eigenschaften zum Erstellen des Pakets erforderlichen Informationen erhält.
      
1. Eine Ressource zur Vorlage hinzuzufügen.

    ![Ressource hinzufügen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Wählen Sie dieses Mal **Web Deploy Web Apps**. 

    ![Hinzufügen von Web bereitstellen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Erneutes Bereitstellen der Projektgruppe Ressourcen der Ressourcengruppe. Diesmal gibt es einige neue Parameter. Sie müssen keine Werte für **_artifactsLocation** oder **_artifactsLocationSasToken** bereitstellen, da Visual Studio automatisch die Werte generiert. Sie müssen jedoch die Ordner- und Dateinamen den Pfad festzulegen, das Bereitstellungspaket (als **ExampleAppPackageFolder** und **ExampleAppPackageFileName** in der folgenden Abbildung dargestellt) enthält. Geben Sie Werte in die Referenz-Eigenschaften (**ExampleApp** und **package.zip**) sah.

    ![Hinzufügen von Web bereitstellen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    Wählen Sie für das **Speicherkonto Artefakt**diese Ressourcengruppe bereitgestellt.
    
1. Nachdem die Bereitstellung abgeschlossen ist, wählen Sie Ihrer Anwendung im Portal. Wählen Sie den URL der Website durchsuchen.

    ![Website durchsuchen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Beachten Sie, dass die standardmäßige ASP.NET Anwendung erfolgreich bereitgestellt haben.

    ![bereitgestellte Anwendung anzeigen](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten von Ressourcen über das Portal finden Sie unter [Verwenden des Azure-Portals Azure Ressourcen verwalten](./azure-portal/resource-group-portal.md).
- Über Vorlagen finden Sie unter [Erstellen von Azure Resource Manager Vorlagen](resource-group-authoring-templates.md).
