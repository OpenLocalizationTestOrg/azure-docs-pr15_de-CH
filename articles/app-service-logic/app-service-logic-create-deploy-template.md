<properties
   pageTitle="Erstellen Sie eine Logik app Bereitstellungsvorlage | Microsoft Azure"
   description="So erstellen Sie eine Logik app Bereitstellungsvorlage und für releasemanagement"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="create-a-logic-app-deployment-template"></a>Erstellen Sie eine Logik app Bereitstellungsvorlage

Nach dem Erstellen eine Anwendung Logik möchten Sie als Azure-Ressourcen-Manager-Vorlage erstellen. Auf diese Weise können Sie leicht bereitstellen Logik app Umgebung oder Ressourcengruppe, wo Sie sie benötigen. Eine Einführung in Ressourcen-Manager Vorlagen unbedingt Artikeln auf [Azure Resource Manager Vorlagen erstellen](../resource-group-authoring-templates.md) und [Bereitstellen von Ressourcen mithilfe von Azure-Ressourcen-Manager Vorlagen](../resource-group-template-deploy.md)Auschecken.

## <a name="logic-app-deployment-template"></a>Vorlage für die Bereitstellung von Logik app

Logik-app besteht aus drei Hauptkomponenten:

* **Logik app Ressource**. Diese Ressource enthält Informationen wie Preis, Plan, Standort und der Workflow-Definition.
* **Workflow-Definition**. Dies ist wie in der Codeansicht. Sie enthält die Definition der Schritte und die Ausführung der Engine. Dies ist die `definition` -Eigenschaft der Logik app Ressource.
* **Verbindung**. Separate Ressourcen, in denen Metadaten Verbindungskabel Connector eine Verbindungszeichenfolge wie ein Zugriffstoken sicher gespeichert sind. Verweisen Sie in einer Anwendung Logik in die `parameters` Abschnitt der Logik app Ressource.

Diese Teile für vorhandene Logik apps können Sie mit einem Tool wie [Azure-Ressourcen-Explorer](http://resources.azure.com)anzeigen.

Um eine Vorlage für eine Anwendung Logik mit Ressource Gruppe Bereitstellung, müssen Sie die Ressourcen definieren und Parametrisieren Bedarf. Beispielsweise wenn Sie Entwicklung, Test und Produktion bereitstellen, werden Sie wahrscheinlich verschiedene Verbindungszeichenfolgen mit einer SQL-Datenbank in jeder Umgebung verwenden möchten. Oder Sie in andere Abonnements oder Ressourcengruppen bereitstellen.  

## <a name="create-a-logic-app-deployment-template"></a>Erstellen Sie eine Logik app Bereitstellungsvorlage

Die einfachste Möglichkeit, eine gültige Logik app Bereitstellung Vorlage ist der [Visual Studio-Tools für Logik-Apps](./app-service-logic-deploy-from-vs.md).  Visual Studio Tools gültige Bereitstellungsvorlage generieren, die über keine Abonnement- oder Speicherort verwendet werden kann.

Einige Tools können Ihnen helfen, eine Bereitstellungsvorlage Logik app erstellen. Manuell, können also erstellen mithilfe von Ressourcen bereits hier behandelten Parameter nach Bedarf erstellen. Eine weitere Option ist ein [Ersteller von Filtervorlagen Logik app](https://github.com/jeffhollan/LogicAppTemplateCreator) PowerShell-Modul verwenden. Diese Open Source-Modul wertet zuerst app Logik und Verbindung ist und Ressourcen mit den notwendigen Parametern für die Bereitstellung generiert. Haben Sie eine Logik-app, die eine Meldung von einer Warteschlange Azure Service Bus und Daten mit einer Azure SQL-Datenbank hinzugefügt, wird das Tool beispielsweise Orchestrierung Geschäftslogik beibehalten und die Verbindungszeichenfolgen SQL und Service Bus parametrisieren, so dass sie bei der Bereitstellung festgelegt werden können.

>[AZURE.NOTE] Innerhalb der gleichen Ressourcengruppe als Anwendung Logik müssen sein.

### <a name="install-the-logic-app-template-powershell-module"></a>Installieren der Logik app Vorlage PowerShell

Installation des Moduls am einfachsten [PowerShell Katalog](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1)mithilfe des Befehls `Install-Module -Name LogicAppTemplate`.  

Sie können auch das PowerShell-Modul manuell installieren:

1. Herunterladen der neuesten Version von [Logik app Ersteller von Filtervorlagen](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).  
1. Extrahieren Sie den Ordner im Ordner PowerShell-Modul (in der Regel `%UserProfile%\Documents\WindowsPowerShell\Modules`).

Für das Modul mit Mieter und Abonnement token empfohlen, dass Sie mit dem Tool [ARMClient](https://github.com/projectkudu/ARMClient) verwenden.  Dieser [Blogbeitrag](http://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) erläutert ARMClient im Detail.

### <a name="generate-a-logic-app-template-by-using-powershell"></a>Mithilfe von PowerShell generieren Sie Logik app Vorlage

Wenn PowerShell installiert ist, können Sie eine Vorlage mit dem folgenden Befehl generieren:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId`ID der Azure-Abonnement. Diese Zeile zuerst erhält einen Zugriff über ARMClient, token und pipes über PowerShell-Skript und erstellt dann die Vorlage in einer JSON-Datei.

## <a name="add-parameters-to-a-logic-app-template"></a>Logik app Vorlage Parameter hinzufügen

Nach dem Erstellen der Logik app-Vorlage können Sie weiterhin möglicherweise Parameter hinzufügen oder ändern. Enthält die Definition eine Ressourcen-ID eine Azure-Funktion oder verschachtelte Workflow in eine einzelne Bereitstellung bereitgestellt, können Sie z. B. die Vorlage Weitere Ressourcen hinzufügen und Parametrisieren IDs Bedarf. Das gleiche gilt für Verweise auf benutzerdefinierte APIs oder stolz Endpunkte jede Ressourcengruppe bereitgestellt werden soll.

## <a name="deploy-a-logic-app-template"></a>Bereitstellen einer Logik app-Vorlage

Sie können die Vorlage eine beliebige Anzahl von Tools, einschließlich PowerShell, REST API Visual Studio Release Management und Bereitstellung Azure Portal Vorlage bereitstellen. Dieser Artikel Weitere Informationen [Bereitstellen von Ressourcen mithilfe von Azure-Ressourcen-Manager-Vorlagen](../resource-group-template-deploy.md) anzeigen Wir empfehlen auch, erstellen Sie eine [Datei](../resource-group-template-deploy.md#parameter-file) zum Speichern der Werte für den Parameter.

### <a name="authorize-oauth-connections"></a>OAuth-Verbindungen zulassen

Nach der Bereitstellung ist die Logik app End to End mit gültigen Parametern. OAuth-Verbindungen müssen allerdings weiterhin berechtigt, ein gültiges Zugangstoken zu generieren. Hierzu die Logik-app im Designer und Autorisieren von Verbindungen. Oder wenn Sie automatisieren möchten, können Sie ein Skript für jede Verbindung OAuth zuzustimmen. Gibt ein Beispielskript GitHub unter dem [LogicAppConnectionAuth](https://github.com/logicappsio/LogicAppConnectionAuth) -Projekt.

## <a name="visual-studio-release-management"></a>Visual Studio Releasemanagement

Ein häufiges Szenario für die Bereitstellung und Verwaltung einer Umgebung ist eine Bereitstellungsvorlage Logik app Tools wie Visual Studio Release Management mit. Visual Studio Team Services umfasst eine [Azure-Ressourcengruppe bereitstellen](https://github.com/Microsoft/vsts-tasks/tree/master/Tasks/DeployAzureResourceGroup) Aufgabe für einen Build hinzufügen oder freigeben Pipeline. Benötigen Sie einen [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/) Autorisierung bereitstellen und Sie können dann die Release-Definition.

1. Release Management erstellen Sie eine neue Definition wählen Sie **leere** mit einer leeren Definition gestartet.

    ![Eine neue, leere erstellen][1]   

1. Wählen Sie für diese Ressourcen. Diese werden wahrscheinlich die Logik app Vorlage manuell oder als Teil des Buildprozesses erstellt.
1. Hinzufügen einer Aufgabe **Azure Ressource bereitstellen** .
1. [Dienstprinzipal](https://blogs.msdn.microsoft.com/visualstudioalm/2015/10/04/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/)konfigurieren und die Vorlage und Vorlagenparameter Dateien verweisen.
1. Weiterhin Schritte in der Freigabe für andere Umgebung, automatisierten Test oder genehmigenden Personen nach Bedarf erstellen.

<!-- Image References -->
[1]: ./media/app-service-logic-create-deploy-template/emptyReleaseDefinition.PNG
