<properties
    pageTitle="Bereitstellen einer Web app Hostname und Ssl-Zertifikat mit MSDeploy"
    description="Verwenden einer Vorlage Ressourcenmanager Azure eine Web app MSDeploy und Festlegen von benutzerdefinierten Hostname und ein SSL-Zertifikat bereitstellen"
    services="app-service\web"
    manager="erikre"
    documentationCenter=""
    authors="jodehavi"
    />

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="john.dehavilland"/>

# <a name="deploy-a-web-app-with-msdeploy-custom-hostname-and-ssl-certificate"></a>Bereitstellen einer Web app MSDeploy benutzerdefinierte Hostname und SSL-Zertifikat

Dieses Handbuch führt durch Erstellen einer End-to-End-Bereitstellung für eine Azure Web App MSDeploy Nutzung sowie ARM-Vorlage benutzerdefinierten Hostname und ein SSL-Zertifikat hinzufügen.

Weitere Informationen zum Erstellen von Vorlagen finden Sie unter [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md).

###<a name="create-sample-application"></a>Beispiel-Anwendung erstellen

Sie können eine ASP.NET Web-Anwendung bereitstellen. Der erste Schritt ist eine einfache Anwendung erstellen (oder Sie können eine vorhandene verwenden - in diesem Fall können Sie diesen Schritt überspringen.)

Öffnen Sie Visual Studio 2015 und wählen Sie Datei > Neues Projekt. Wählen Sie im daraufhin angezeigten Dialogfeld Web > ASP.NET Web Application. Klicken Sie unter Vorlagen wählen Sie Web und die MVC-Vorlage. Wählen Sie _Keine Authentifizierung_ _Authentifizierungstyp ändern_ . Wird nur die beispielanwendung so einfach wie möglich.

Zu diesem Zeitpunkt haben Sie eine einfache ASP.Net Web app als Teil der Bereitstellung verwenden.

###<a name="create-msdeploy-package"></a>MSDeploy-Paket erstellen

Nächste Schritt ist die Erstellung des Pakets zum Web app in Azure bereitstellen. Dazu speichern Sie das Projekt, und führen Sie Folgendes über die Befehlszeile:

    msbuild yourwebapp.csproj /t:Package /p:PackageLocation="path\to\package.zip"

Dadurch wird ein ZIP-Paket im Ordner PackageLocation erstellt. Die Anwendung kann jetzt bereitgestellt werden, die können Sie jetzt ein Ressourcenmanager Azure Vorlage dafür erstellen.

###<a name="create-arm-template"></a>ARM-Vorlage erstellen
Zunächst beginnen wir mit einer grundlegenden ARM-Vorlage, die eine Anwendung und eine Hosting-Plan (Beachten Sie die Parameter und Variablen aus Platzgründen nicht angezeigt werden) erstellt.

    {
        "name": "[parameters('appServicePlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "apiVersion": "2014-06-01",
        "dependsOn": [ ],
        "tags": {
            "displayName": "appServicePlan"
        },
        "properties": {
            "name": "[parameters('appServicePlanName')]",
            "sku": "[parameters('appServicePlanSKU')]",
            "workerSize": "[parameters('appServicePlanWorkerSize')]",
            "numberOfWorkers": 1
        }
    },
    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        }
    }

Als Nächstes müssen Sie app Webressource geschachtelten MSDeploy-Ressource zu ändern. Damit können Sie auf die zuvor erstellten und sagen Azure Ressourcenmanager MSDeploy Programmeigenschaften WebApp Azure verwenden. Im folgenden wird die Microsoft.Web/sites Ressource mit geschachtelten MSDeploy-Ressource:

    {
        "name": "[variables('webAppName')]",
        "type": "Microsoft.Web/sites",
        "location": "[resourceGroup().location]",
        "apiVersion": "2015-08-01",
        "dependsOn": [
            "[concat('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        ],
        "tags": {
            "[concat('hidden-related:', resourceGroup().id, '/providers/Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]": "Resource",
            "displayName": "webApp"
        },
        "properties": {
            "name": "[variables('webAppName')]",
            "serverFarmId": "[resourceId('Microsoft.Web/serverfarms/', parameters('appServicePlanName'))]"
        },
        "resources": [
            {
                "name": "MSDeploy",
                "type": "extensions",
                "location": "[resourceGroup().location]",
                "apiVersion": "2015-08-01",
                "dependsOn": [
                    "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
                ],
                "tags": {
                    "displayName": "webDeploy"
                },
                "properties": {
                    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]",
                    "dbType": "None",
                    "connectionString": "",
                    "setParameters": {
                        "IIS Web Application Name": "[variables('webAppName')]"
                    }
                }
            }
        ]
    }

Jetzt sehen Sie, dass die Ressource MSDeploy **PackageUri** -Eigenschaft wird wie folgt definiert:

    "packageUri": "[concat(parameters('_artifactsLocation'), '/', parameters('webDeployPackageFolder'), '/', parameters('webDeployPackageFileName'), parameters('_artifactsLocationSasToken'))]"

Diese **PackageUri** nimmt Storage Konto Uri die auf das Speicherkonto, lädt Sie Ihr Paket Zip. Der Azure-Ressourcen-Manager nutzen [Gemeinsame Zugriff Signaturen](../storage/storage-dotnet-shared-access-signature-part-1.md) das Paket lokal aus dem Speicherkonto ziehen Sie beim Bereitstellen der Vorlage. Dieser Prozess wird automatisiert werden über PowerShell Skript, laden Sie das Paket und Aufrufen der Azure-API zum Erstellen der Schlüssel, erforderlich und in der Vorlage als Parameter (*_artifactsLocation* und *_artifactsLocationSasToken*) übergeben. Sie müssen Parameter für die Ordner definieren und Dateiname das Paket unter der Speichercontainer hochgeladen.

Anschließend müssen Sie eine andere geschachtelte Ressource Hostname Bindungen nutzen eine benutzerdefinierte Domäne einrichten hinzufügen. Müssen Sie zuerst, dass Hostname besitzen und Einrichten von Azure überprüft werden, dass-Besitzer finden Sie unter [Konfigurieren einer benutzerdefinierten Domänennamen in Azure App Service](web-sites-custom-domain-name.md). Danach können Sie die Vorlage im Abschnitt Ressource Microsoft.Web/sites Folgendes hinzufügen:

    {
        "apiVersion": "2015-08-01",
        "type": "hostNameBindings",
        "name": "www.yourcustomdomain.com",
        "dependsOn": [
            "[concat('Microsoft.Web/sites/', variables('webAppName'))]"
        ],
        "properties": {
            "domainId": null,
            "hostNameType": "Verified",
            "siteName": "variables('webAppName')"
        }
    }

Schließlich müssen Sie weitere Top Level Ressource Microsoft.Web/certificates hinzufügen. Diese Ressource enthält das SSL-Zertifikat und Ebene als WebApp und Hosting-Plan vorhanden ist.

    {
        "name": "[parameters('certificateName')]",
        "apiVersion": "2014-04-01",
        "type": "Microsoft.Web/certificates",
        "location": "[resourceGroup().location]",
        "properties": {
            "pfxBlob": "pfx base64 blob",
            "password": "some pass"
        }
    }

Sie müssen ein gültiges SSL-Zertifikat verfügen, um diese Ressource einzurichten. Haben, gültige Zertifikat müssen Sie die Pfx-Bytes als base64-Zeichenfolge extrahieren. Eine Möglichkeit zum Extrahieren dieser ist mit dem folgenden PowerShell-Befehl:

    $fileContentBytes = get-content 'C:\path\to\cert.pfx' -Encoding Byte

    [System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-bytes.txt'

Sie konnte dies dann als Parameter an die ARM-Bereitstellungsvorlage übergeben.

An diesem Punkt kann die ARM-Vorlage.

###<a name="deploy-template"></a>Vorlage bereitstellen

Die letzten Schritte sind dies alle in eine vollständige End-to-End-Bereitstellung zusammen. Erleichtert Bereitstellung **Bereitstellen AzureResourceGroup.ps1** PowerShell-Skript nutzen können, das beim Erstellen einer Ressourcengruppe Azure-Projekt in Visual Studio, bei der alle Elemente in der Vorlage erforderlich hochladen hinzugefügt wird. Sie müssen ein Speicherkonto erstellt haben, die vorzeitig verwenden möchten. In diesem Beispiel habe ich freigegebenen Speicherkonto für die package.zip übertragen werden. Das Skript wird AzCopy zum Hochladen des Pakets auf das Speicherkonto nutzen. In Ihrem Ordner Artefakt übergeben und das Skript lädt automatisch alle Dateien in diesem Verzeichnis zu benannten Behälter. Nach dem Aufruf von AzureResourceGroup.ps1 bereitstellen müssen Sie SSL-Bindungen zum Zuordnen der benutzerdefinierten Hostname mit dem SSL-Zertifikat aktualisieren.

Die folgende PowerShell zeigt die vollständige Bereitstellung bereitstellen aufrufen-AzureResourceGroup.ps1:

    #Set resource group name
    $rgName = "Name-of-resource-group"

    #call deploy-azureresourcegroup script to deploy web app

    .\Deploy-AzureResourceGroup.ps1 -ResourceGroupLocation "East US" `
                                    -ResourceGroupName $rgName `
                                    -UploadArtifacts `
                                    -StorageAccountName "name-of-storage-acct-for-package" `
                                    -StorageAccountResourceGroupName "resource-group-name-storage-acct" `
                                    -TemplateFile "web-app-deploy.json" `
                                    -TemplateParametersFile "web-app-deploy-parameters.json" `
                                    -ArtifactStagingDirectory "C:\path\to\packagefolder\"

    #update web app to bind ssl certificate to hostname. This has to be done after creation above.

    $cert = Get-PfxCertificate -FilePath C:\path\to\certificate.pfx

    $ar = Get-AzureRmResource -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -ApiVersion 2014-11-01

    $props = $ar.Properties

    $props.HostNameSslStates[2].'SslState' = 1
    $props.HostNameSslStates[2].'thumbprint' = $cert.Thumbprint
    $props.hostNameSslStates[2].'toUpdate' = $true

    Set-AzureRmResource -ApiVersion 2014-11-01 -Name nameofwebsite -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites -PropertyObject $props

An diesem Punkt die Anwendung sollte wurden bereitgestellt haben und sollte man wechseln über https://www.yourcustomdomain.com
