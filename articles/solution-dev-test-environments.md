<properties
   pageTitle="Entwicklungs- und Umgebung | Microsoft Azure"
   description="Erfahren Sie mit Azure Resource Manager Vorlagen schnell und einheitlich erstellen und Löschen von Entwicklung und Umgebung testen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="01/22/2016"
   ms.author="tomfitz"/>

# <a name="development-and-test-environments-in-microsoft-azure"></a>Entwicklungs- und Umgebungen in Microsoft Azure

Andere Programme werden in der Regel auf mehrere Entwicklung und testing vor der Bereitstellung für die Produktion bereitgestellt. Beim Erstellen von Umgebungen vor Computerressourcen beschafft oder für jede Umgebung für jede Anwendung zugeordnet. Die Umgebung enthalten häufig mehrere physische oder virtuelle Computer mit spezifischen Konfigurationen manuell bereitgestellt werden, oder komplexe Automatisierungsskripts. Bereitstellung häufig Stunden dauern und zu inkonsistenten Konfigurationen Umgebungen.

## <a name="scenario"></a>Szenario ##

Bei der Bereitstellung von Entwicklung und Tests Umgebungen in Microsoft Azure bezahlen Sie nur für die Ressourcen.  Erläutert, wie schnell und einheitlich können erstellen, verwalten, löschen Entwicklung und Umgebungen Azure Ressourcenmanager Vorlagen und Parameterdateien zu testen, wie unten dargestellt.

![Szenario](./media/solution-dev-test-environments/scenario.png)

Drei Entwicklung und testing werden oben angezeigt.  Jeder hat Anwendung und SQL Ressourcen, die in einer Vorlagendatei angegeben werden.  Die Namen der Anwendung und der Datenbank in jeder Umgebung unterscheiden und eindeutigen Parameterdateien für jede Umgebung angegeben werden.  

Wenn Sie nicht mit Azure-Ressourcen-Manager vertraut sind, empfiehlt es sich, vor dem Lesen dieses Artikels [Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md) Übersichtsartikel lesen.

Sie sollten zu ersten Schritten in diesem Artikel aufgeführten ohne verlinkten Artikeln Erfahrung mit Azure Ressourcenmanager Vorlagen schnell zu lesen. Nachdem Sie einmal schrittweise war, Sie werden die meisten Fragen beantwortet, die sich Ihre erste Zeit durch Experimentieren weitere Schritte und die referenzierten Artikel.

## <a name="plan-azure-resource-use"></a>Azure Ressourcen planen
Haben Sie hohe Design für die Anwendung können Sie:

- Azure Ressourcen wird Ihre Anwendung enthalten. Sie können die Anwendung und als Azure Web App mit einer Azure SQL-Datenbank bereitstellen.  Sie können virtuelle Maschinen mit PHP und MySQL oder IIS und SQL Server oder andere Komponenten die Anwendung erstellen. Artikel [VMs Vergleich, Cloud Services und Azure App Service]( app-service-web/choose-web-site-cloud-service-vm.md) hilft Ihnen Azure Ressourcen entscheiden Sie für Ihre Anwendung verwenden möchten.
- Welche Service-Leveln wie Verfügbarkeit, Sicherheit und Skala, die die Anwendung erfüllen.

## <a name="download-an-existing-template"></a>Herunterladen einer Vorlage
Eine Azure-Ressourcen-Manager-Vorlage definiert alle Azure Ressourcen die Anwendung verwendet. Mehrere Vorlagen vorhanden, Sie direkt in Azure-Portal bereitstellen oder herunterladen, ändern und speichern in einem Quellcodeverwaltungssystem mit dem Anwendungscode.  Schritte eine vorhandene Vorlage herunterladen.

1. Durchsuchen Sie Vorlagen in [Azure Schnellstart Vorlagen](https://github.com/Azure/azure-quickstart-templates/) GitHub Repository. In der Liste sehen Sie einen Ordner "[201-Web-app-Sql-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)". Da viele benutzerdefinierte Clientanwendungen eine Web-Anwendung und SQL-Datenbank, wird diese Vorlage als Beispiel im Rest dieses Artikels, wie Vorlagen verwendet. Den Rahmen dieses Artikels vollständig alles erläutern diese Vorlage erstellt und konfiguriert, aber wenn Sie tatsächliche Umgebung in Ihrer Organisation erstellen möchten, Sie wollen verstehen durch [Bereitstellung einer Webanwendung mit einer SQL-Datenbank](app-service-web/app-service-web-arm-with-sql-database-provision.md) lesen.
Hinweis: Dieser Artikel wurde für Dezember 2015 Version der Vorlage [201-Web-app-Sql-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) geschrieben. Die Links unten, zeigen Sie auf Vorlage und Parameterdateien dieser Version der Vorlage.
2. Klicken Sie auf die Datei [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json) im Ordner 201-Web-app-Sql-Datenbank, um seinen Inhalt anzuzeigen. Dies ist der Azure-Ressourcen-Manager. 
3. Klicken Sie im Ansichtsmodus auf "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.json)". 
4. Markieren Sie mit der Maus den Inhalt dieser Datei und als eine Datei namens "TestApp1 Template.json" auf Ihrem Computer speichern 
5. Überprüfen Sie den Vorlageninhalt, und beachten Sie Folgendes:
 - **Ressourcenabschnitt** : Dieser Abschnitt definiert die Typen von Azure Ressourcen über diese Vorlage erstellt. Diese Vorlage erstellt unter andere Ressourcentypen [SQL](sql-database/sql-database-technical-overview.md) [Azure Web App](app-service-web/app-service-web-overview.md) und Azure-Ressourcen. Wenn ausführen und Web- und SQL Server in virtuelle Computer verwalten möchten, können Sie die "[Iis-2vm-Sql-1vm](https://github.com/Azure/azure-quickstart-templates/tree/master/iis-2vm-sql-1vm)" oder "[Lampe app](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)" Vorlagen der Anleitung in diesem Artikel basieren auf der Vorlage [201-Web-app-Sql-Datenbank](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database) .
 - **Abschnitt** : Dieser Abschnitt definiert die Parameter, die mit jeder Ressource konfiguriert werden kann. Einige Parameter in der Vorlage angegeben haben "DefaultValue" Eigenschaften nicht. Bei der Bereitstellung von Azure-Ressourcen mit einer Vorlage müssen Sie Werte für alle Parameter angeben, die nicht in der Vorlage angegebene Standardwert-Eigenschaften.  Wenn Sie keine Werte für Parameter mit Standardwert angeben, wird der angegebene Wert für die DefaultValue-Parameter in der Vorlage verwendet.

Eine Vorlage definiert Azure Ressourcen erstellt werden und die Parameter jeder Ressource kann mit konfiguriert werden. Erfahren Sie mehr über Vorlagen und eigene durch Lesen der [Best Practices für Ressourcenmanager Azure Vorlagen entwerfen](best-practices-resource-manager-design-templates.md) entwerfen.

## <a name="download-and-customize-an-existing-parameter-file"></a>Downloaden und Anpassen einer vorhandenen Parameterdatei

Wenn Sie wahrscheinlich die *gleichen* Azure wollen Ressourcen in jeder Umgebung erstellt, Sie werden wahrscheinlich die Konfiguration der Ressourcen, die *andere* in jeder Umgebung.  Dies sind Parameterdateien. Erstellen Sie Parameterdateien, die eindeutige Werte in jeder Umgebung durch die nachfolgenden Schritte enthalten.   

1. Den Inhalt der Datei [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/tree/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json) im Ordner 201-Web-app-Sql-Datenbank anzuzeigen. Dies ist der Parameter für die Vorlagendatei, die Sie im vorherigen Abschnitt gespeichert. 
2. Klicken Sie im Ansichtsmodus auf "[Raw](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/3f24f7b7e1e377538d1d548eaa6eab2851a21810/201-web-app-sql-database/azuredeploy.parameters.json)". 
3. Markieren Sie mit der Maus den Inhalt dieser Datei, und speichern sie in drei separaten Dateien auf Ihrem Computer mit den folgenden Namen:
 - TestApp1-Parameter-Development.json
 - TestApp1-Parameter-Test.json
 - TestApp1-Parameter-Pre-Production.json

3. Bearbeiten Sie Text oder JSON-Editor mit der Entwicklung Umgebung Parameter erstellten Datei in Schritt 3 mit den *Werten* der **Parameter** unten rechts neben die Parameterwerte in der Datei aufgeführten Werte ersetzt: 
 - **SiteName**: *TestApp1DevApp*
 - **HostingPlanName**: *TestApp1DevPlan*
 - **SiteLocation**: *USA*
 - **ServerName**: *testapp1devsrv*
 - **geeignetsten**: *USA*
 - **AdministratorLogin**: *testapp1Admin*
 - **AdministratorLoginPassword**: *Ihr Kennwort ersetzen*
 - **Datenbankname**: *testapp1devdb*

4. Mit einem Text- oder JSON-Editor bearbeiten die Umgebung Parameter Testdatei erstellt in Schritt 3 ersetzen die Werte rechts die Parameterwerte in der Datei mit den *Werten* , die rechts der folgenden **Parameter** aufgeführt:
 - **SiteName**: *TestApp1TestApp*
 - **HostingPlanName**: *TestApp1TestPlan*
 - **SiteLocation**: *USA*
 - **ServerName**: *testapp1testsrv*
 - **geeignetsten**: *USA*
 - **AdministratorLogin**: *testapp1Admin*
 - **AdministratorLoginPassword**: *Ihr Kennwort ersetzen*
 - **Datenbankname**: *testapp1testdb*

5. Bearbeiten Sie in Schritt 3 erstellten Parameterdatei Pre-Production-mit Text oder JSON-Editor.  Ersetzen Sie den gesamten Inhalt der Datei mit dem folgenden:

        {
          "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
          "contentVersion" : "1.0.0.0",
          "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "replace with your password"
        },
        "databaseName" : {
          "value" : "testapp1preproddb"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PreProdPlan"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprodsrv"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProdApp"
        },
        "sku" : {
          "value" : "Standard"
        },
            "requestedServiceObjectiveName" : {
              "value" : "S1"
        }
          }
        }

In der Pre-Production-Parameter über wurden Parameter **Sku** und **RequestedServiceObjectiveName** *hinzugefügt*, während sie Entwicklungs- und Parameter-Dateien wurden nicht hinzugefügt. Ist Standard Werte für diese Parameter in der Vorlage angegeben und Umgebung Entwicklungs- und die Standardwerte verwendet, aber in der Vorproduktion Umgebung nicht standardmäßige Werte für diese Parameter werden verwendet.

Nicht standardmäßige Werte für diese Parameter in der Pre-Production-Umgebung verwendet werden deshalb Werte für diese Parameter testen, die Sie für Ihre produktionsumgebung lieber, so kann auch getestet werden.  Diese Parameter beziehen sich auf Azure [Web App hosting Pläne](https://azure.microsoft.com/pricing/details/app-service/)oder **Sku** und Azure [SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/)oder **RequestedServiceObjectiveName** , die von der Anwendung verwendet werden.  Verschiedene Skus und objektive Dienstnamen unterschiedliche Kosten und Funktionen unterstützen verschiedene Service Level Metriken.

Die Tabelle enthält die Standardwerte für diese Parameter angegebenen Vorlage und Werte, die anstatt die Standardwerte in der Datei vor Produktionsparameter verwendet werden.

| Parameter | Standardwert | Parameterwert |
|---|---|---|
| **SKU** | Frei | Standard |
| **requestedServiceObjectiveName** | S0 | S1 |

## <a name="create-environments"></a>Erstellen der Umgebung
Alle Azure-Ressourcen müssen in einer [Azure-Ressourcengruppe](azure-resource-manager/resource-group-overview.md)erstellt werden. Ressourcengruppen können Sie Azure Ressourcen gruppieren, damit sie gemeinsam verwaltet werden können.  Ressourcengruppen können [Berechtigungen](./active-directory/role-based-access-built-in-roles.md) zugewiesen werden, bestimmte Personen in Ihrer Organisation erstellen, ändern, löschen oder sie und Ressourcen darin anzeigen.  Alarme und Abrechnungsinformationen für Ressourcen in der Ressourcengruppe können in [Azure-Portal](https://portal.azure.com)angezeigt werden. Ressourcengruppen werden in einer Azure- [Region](https://azure.microsoft.com/regions/)erstellt.  In diesem Artikel werden alle Ressourcen in der Region USA erstellt. Beginn der eigentlichen Umgebung erstellen wählen Sie den Bereich, der Ihren Bedürfnissen am besten entspricht. 

Erstellen Sie Ressourcengruppen für jede Umgebung mit einer der folgenden Methoden.  Alle Methoden werden das gleiche Ergebnis erzielen.

###<a name="azure-command-line-interface-cli"></a>Azure-Befehlszeilenschnittstelle (CLI)

Stellen Sie sicher, dass Sie die CLI [installiert](xplat-cli-install.md) auf Windows, OS X oder Linux-Computer, und Sie [verbunden](xplat-cli-connect.md) Ihr [Azure AD-Konto haben](./active-directory/active-directory-how-subscriptions-associated-directory.md) (auch Arbeits- oder Schulcomputer Konto genannt) zu Ihrem Azure-Abonnement. Geben Sie über die CLI-Befehlszeile folgenden Befehl zum Erstellen der Ressourcengruppe Development Environment.

    azure group create "TestApp1-Development" "Central US"

Der Befehl wird erfolgreich Folgendes zurück:

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

Um die Ressourcengruppe für die Umgebung zu erstellen, geben Sie folgenden Befehl:

    azure group create "TestApp1-Test" "Central US"

Um der Ressourcengruppe für die Pre-Production-Umgebung zu erstellen, geben Sie folgenden Befehl:

    azure group create "TestApp1-Pre-Production" "Central US"

###<a name="powershell"></a>PowerShell

Stellen Sie sicher, dass Azure PowerShell 1.01 oder höher auf einem windowscomputer installiert und Ihr [Azure AD-Konto](./active-directory/active-directory-how-subscriptions-associated-directory.md) (auch Arbeits- oder Schulcomputer Konto genannt) zu Ihrem Abonnement gemäß Artikel [zum Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md) verbunden haben. Geben Sie ein PowerShell-Befehlszeile folgenden Befehl zum Erstellen der Ressourcengruppe Development Environment.

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

Der Befehl wird erfolgreich Folgendes zurück:

    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

Um die Ressourcengruppe für die Umgebung zu erstellen, geben Sie folgenden Befehl:

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

Um der Ressourcengruppe für die Pre-Production-Umgebung zu erstellen, geben Sie folgenden Befehl:

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

###<a name="azure-portal"></a>Azure-portal

1. Melden Sie sich bei [Azure-Portal](https://portal.azure.com) mit einem Konto [Azure AD](./active-directory/active-directory-how-subscriptions-associated-directory.md) (auch als Arbeit oder Schule). Klicken Sie auf neue--> Management--> Ressourcengruppe und geben Sie in das Feld Ressource "TestApp1 Development", wählen Sie Ihr Abonnement und "Zentralen USA" in Feld Ort Ressource wie in der folgenden Abbildung dargestellt.
   ![Portal](./media/solution-dev-test-environments/rgcreate.png)
2. Klicken Sie auf erstellen, um die Ressourcengruppe erstellen.
3. Klicken Sie auf Durchsuchen, der Liste Ressourcengruppen und Ressourcengruppen auf, wie unten dargestellt.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png) 
4. Nach dem Klicken auf Ressourcengruppen sehen Sie Ressource Gruppen Blade mit der neuen Ressource.
   ![Portal](./media/solution-dev-test-environments/rgview.png)
5. Erstellen des TestApp1-Tests und TestApp1 der Vorproduktion Ressourcengruppen die TestApp1 Entwicklung Ressourcengruppe erstellt genauso.

##<a name="deploy-resources-to-environments"></a>Bereitstellen von Ressourcen auf

Bereitstellen Sie Azure-Ressourcen der Ressourcengruppen für jede Umgebung die Vorlagendatei für die Lösung und Parameterdateien für jede Umgebung mit einer der folgenden Methoden.  Beide Methoden werden das gleiche Ergebnis erzielen.

###<a name="azure-command-line-interface-cli"></a>Azure-Befehlszeilenschnittstelle (CLI)

Geben Sie über die CLI-Befehlszeile folgenden Befehl zum Bereitstellen von Ressourcen der Ressourcengruppe Diskette(n) Development Environment [Pfad] ersetzen den Pfad zu den Dateien, die Sie zuvor gespeichert.

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

Nach sehen eine Meldung "Warten Bereitstellung abgeschlossen" für Minuten, der Befehl zurück Folgendes erfolgreich:

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1DevApp
    data:    hostingPlanName                String        TestApp1DevPlan
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1devsrv
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1devdb
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

Wenn der Befehl nicht erfolgreich ist, beheben Sie Fehlermeldungen, und versuchen Sie es erneut.  Probleme werden Parameterwerte verwenden, die nicht Azure Ressourcengründen Benennungskonventionen entsprechen. Weitere Tipps zur Problembehandlung finden im Artikel [Problembehandlung Ressource Gruppe Bereitstellung in Azure](./resource-manager-troubleshoot-deployments-cli.md) .

Geben Sie über die CLI-Befehlszeile folgenden Befehl zum Bereitstellen von Ressourcen der Ressourcengruppe erstellte Umgebung Test [Pfad] ersetzen den Pfad zu den Dateien, die Sie zuvor gespeichert.

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

Geben Sie über die CLI-Befehlszeile folgenden Befehl zum Bereitstellen von Ressourcen der Ressourcengruppe erstellte Umgebung Pre-Production [Pfad] ersetzen den Pfad zu den Dateien, die Sie zuvor gespeichert.

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
###<a name="powershell"></a>PowerShell

Geben Sie eine Befehlszeile Azure PowerShell (Version 1.01 oder höher) folgenden Befehl zum Bereitstellen von Ressourcen der Ressourcengruppe Diskette(n) Development Environment [Pfad] ersetzen den Pfad zu den Dateien, die Sie zuvor gespeichert.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

Nach einem Blick auf einen blinkenden Cursor für einen Moment, der Befehl zurück Folgendes erfolgreich:

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1DevApp
                        hostingPlanName  String                     TestApp1DevPlan
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1devsrv
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1devdb
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  Wenn der Befehl nicht erfolgreich ist, beheben Sie Fehlermeldungen, und versuchen Sie es erneut.  Probleme werden Parameterwerte verwenden, die nicht Azure Ressourcengründen Benennungskonventionen entsprechen. Weitere Tipps zur Problembehandlung finden im Artikel [Problembehandlung Ressource Gruppe Bereitstellung in Azure](./resource-manager-troubleshoot-deployments-powershell.md) .

  Geben Sie ein PowerShell-Befehlszeile folgenden Befehl zum Bereitstellen von Ressourcen der Ressourcengruppe erstellte Umgebung Test [Pfad] ersetzen den Pfad zu den Dateien, die Sie zuvor gespeichert.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  Geben Sie PowerShell-Befehlszeile folgenden Befehl zum Bereitstellen von Ressourcen der Ressourcengruppe erstellte Umgebung Pre-Production [Pfad] ersetzen den Pfad zu den Dateien, die Sie zuvor gespeichert.

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

Vorlage und Parameter-Dateien kann versioniert und verwaltet mit dem Anwendungscode in ein Quellcodeverwaltungssystem.  Sie können auch die Befehle oben Skriptdateien und mit Ihrem Code speichern.

> [AZURE.NOTE] Sie können die Vorlage in Azure bereitstellen direkt auf die Schaltfläche "Nach Azure bereitstellen" Artikel [Bereitstellung Web App mit einer SQL-Datenbank](https://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) .  Möglicherweise hilfreich diese Informationen Vorlagen, aber dadurch nicht Version bearbeiten können und Ihre Vorlage und Parameter Werte mit dem Anwendungscode speichern, damit weitere Informationen zu dieser Methode nicht in diesem Artikel behandelt wird.

## <a name="maintain-environments"></a>Umgebung verwalten
Während der Entwicklung werden Konfiguration von Azure Ressourcen in unterschiedlichen Umgebungen inkonsistent absichtlich oder versehentlich geändert.  Dies kann während der Anwendungsentwicklung Problembehandlung und Lösung von Problemen führen.

1. Ändern Sie die Umgebung durch Öffnen der [Azure-Portal](https://portal.azure.com). 
2. Melden Sie sich es mit demselben Konto, das Sie verwendet, um die oben aufgeführten Schritte. 
3. Wie in der Abbildung unten auf Durchsuchen--> Ressourcengruppen (möglicherweise müssen Scrollen Ressourcengruppen anzeigen) angezeigt.
   ![Portal](./media/solution-dev-test-environments/rgbrowse.png)
4. Nach dem Klicken auf Ressourcengruppen in der obigen Abbildung sehen Sie Ressource Gruppen Blade und die drei Ressourcengruppen, die Sie in einem früheren Schritt erstellt haben, wie in der folgenden Abbildung dargestellt. Klicken Sie auf die Ressourcengruppe TestApp1 Entwicklung und sehen Sie das Blade, das von der Vorlage in der Bereitstellung für zuvor abgeschlossenen TestApp1 Development-Gruppe erstellten Ressourcen auflistet.  Löschen der Ressource TestApp1DevApp Web App auf TestApp1DevApp in der TestApp1 Entwicklung Ressource Blatt und TestApp1DevApp Web app Blatt löschen klicken.
   ![Portal](./media/solution-dev-test-environments/portal2.png)
5. Klicken Sie auf "Ja", wenn das Portal gefragt werden, ob Sie wirklich die Ressource gelöscht werden soll. Das TestApp1 Entwicklung Ressource Blatt schließen und erneut öffnen zeigt sie Web App nun nur gelöscht.  Der Inhalt der Ressourcengruppe unterscheiden sich jetzt als sie sein sollte. Sie können weiter experimentieren, indem mehrere Ressourcen aus mehreren Ressourcengruppen löschen oder sogar Einstellungen Konfiguration für einige Ressourcen. Anstelle des Azure-Portals löschen eine Ressource aus einer Ressourcengruppe konnte Befehl PowerShell [AzureResource entfernen](https://msdn.microsoft.com/library/azure/dn757676.aspx) oder oder "Azure Ressource löschen" über die Befehlszeile zum Durchführen derselben Aufgabe.
6. Zum Abrufen aller Ressourcen und Konfiguration, die in den Ressourcengruppen auf den Zustand sein, sie in bereitstellen Sie Umgebung [Bereitstellen auf](#deploy-resources-to-environments) Ressourcenabschnitt dieselben Befehle verwendeten bei Ressourcengruppen erneut, aber ersetzen Sie "Deployment1" mit "Einrichtung2."
7.  Siehe Abschnitt "Zusammenfassung" TestApp1 Development-Blade in das Bild in Schritt 4, sehen Sie Web app im Portal im vorherigen Schritt gelöscht gibt wieder, wie andere Ressourcen möglicherweise soll löschen. Wenn Sie die Konfiguration der Ressourcen geändert, können Sie auch überprüfen, dass sie zurück auf die Werte in Parameterdateien zu eingerichtet haben. Einer der Vorteile der Bereitstellung Ihrer Umgebung mit Azure Ressourcenmanager Vorlagen ist, dass Sie problemlos die Umgebung in einen bekannten Zustand jederzeit erneut bereitstellen können.
8. Wenn auf den Text unter "Letzte Bereitstellung" in der folgenden Abbildung sehen Sie eine-Blade Verlauf der Bereitstellung für die Ressourcengruppe.  Da Sie den Namen "Deployment1" für die erste Bereitstellung und "Einrichtung2" für die zweite Bereitstellung verwendet, müssen Sie zwei Einträge.  Bereitstellung auf zeigt eine-Blade, die mit den Ergebnissen für jede Bereitstellung angezeigt.
  ![Portal](./media/solution-dev-test-environments/portal3.png)



## <a name="delete-environments"></a>Umgebung löschen
Wenn Sie in einer Umgebung fertig sind, sollten Sie sie löschen, damit Sie Verwendung für Azure Ressourcen Gebühren nicht mehr verwendeten.  Löschen von Umgebungen ist noch einfacher als erstellen.  In den vorherigen Schritten erstellten einzelnen Azure-Ressourcengruppen für jede Umgebung und dann Ressourcen in den Ressourcengruppen bereitgestellt wurden. 

Löschen der Umgebung mit einer der folgenden Methoden.  Alle Methoden werden das gleiche Ergebnis erzielen.

### <a name="azure-cli"></a>Azure CLI

Aus einem CLI Folgendes ein:

    azure group delete "TestApp1-Development"

Wenn Sie aufgefordert werden, geben Sie y und dann drücken Development Environment und alle Ressourcen. Nach einigen Minuten wird der Befehl Folgendes zurück:

    info:    group delete command OK

CLI-Aufforderung geben Sie Folgendes ein, um die verbleibenden Umgebung löschen:

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
### <a name="powershell"></a>PowerShell

Geben Sie eine Befehlszeile Azure PowerShell (Version 1.01 oder höher) folgenden Befehl der Ressourcengruppe und seinen gesamten Inhalt löschen.    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

Wenn Sie aufgefordert werden, wenn Sie sicher sind, dass Sie die Ressourcengruppe entfernen möchten, geben Sie y, gefolgt von der EINGABETASTE.

Geben Sie Folgendes ein, um die verbleibenden Umgebung löschen:

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

### <a name="azure-portal"></a>Azure-portal

1. Wechseln Sie in Azure-Portal Ressourcengruppen, wie in den vorherigen Schritten. 
2. Wählen Sie die Ressourcengruppe TestApp1 Entwicklung und in der TestApp1 Entwicklung Ressource Blatt klicken Sie löschen. Ein neues Blatt wird angezeigt. Gruppennamen Sie Ressource, und klicken Sie auf die Schaltfläche löschen.
![Portal](./media/solution-dev-test-environments/rgdelete.png)
3. Löschen Sie TestApp1-Test und TestApp1 der Vorproduktion Ressourcengruppen dasselbe TestApp1 Entwicklung Ressourcengruppe gelöscht.

Unabhängig von der verwendeten Methode Ressourcengruppen und alle enthaltenen Ressourcen nicht mehr vorhanden und Abrechnung Ausgaben für Ressourcen werden nicht mehr entstehen.  

Minimierung die Azure Ressource Auslastung Kosten, die entstehen bei der Anwendungsentwicklung [Azure Automation](automation/automation-intro.md) Verwendung planen Aufträge, die:

- Beenden Sie virtuelle Computer am Ende des jeweiligen Tages und starten sie zu Beginn jeden Tag.
- Gesamte Umgebung am Ende des jeweiligen Tages löschen und neu zu Beginn jeden Tag erstellen.
 
Da Sie erlebt, wie einfach es ist, erstellen, verwalten, löschen Entwicklung und test Umgebung weitere haben, was nur Sie weiter mit den Schritten und Lesen in diesem Artikel enthaltenen Verweise.

## <a name="next-steps"></a>Nächste Schritte

- [Delegieren von Verwaltungsfunktionalität](./active-directory/role-based-access-control-configure.md) für unterschiedliche Ressourcen in jeder Umgebung durch bestimmte Funktionen, die eine Teilmenge der Betrieb auf Azure Ressourcen ausführen können, Microsoft Azure AD-Gruppen oder Benutzern zuweisen.
- [Zuweisen von Tags](resource-group-using-tags.md) zu Ressourcengruppen für jede Umgebung bzw. der einzelnen Ressourcen. Sie können die Ressourcengruppen ein Tag "Umgebung" hinzufügen und Wert entsprechen den Umgebungsnamen. Tags können besonders hilfreich sein, wenn Sie Ressourcen für Abrechnung oder Management organisieren müssen.
- Alarme überwachen und Abrechnung für Ressourcen Gruppenressourcen in [Azure-Portal](https://portal.azure.com).
