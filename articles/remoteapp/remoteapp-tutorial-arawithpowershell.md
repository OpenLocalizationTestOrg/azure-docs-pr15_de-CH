<properties
   pageTitle="Verwenden von PowerShell-Cmdlets Azure RemoteApp | Microsoft Azure"
   description="Erfahren Sie, wie Windows PowerShell-Cmdlets in Azure RemoteApp verwenden."
   services="remoteapp"
   documentationCenter=""
   authors="guscatalano"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>



# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Verwenden von Windows PowerShell-Cmdlets Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

 Sie können Azure RemoteApp-PowerShell-Cmdlets verwalten Ihre Sammlungen. Verwenden Sie folgende Informationen für den Einstieg.

## <a name="get-the-cmdlets"></a>Die Cmdlets abrufen 
-------------
Zuerst herunterladen Azure Powershell-Cmdlets [hier](http://go.microsoft.com/?linkid=9811175)RemoteApp Cmdlets enthalten sind. 

Checken Sie [Azure RemoteApp Cmdlet helfen](https://msdn.microsoft.com/library/mt428031.aspx).

## <a name="configure-azure-cmdlets-to-use-your-subscription"></a>Konfigurieren von Azure Cmdlets um Ihr Abonnement zu verwenden
------------------
Folgen Sie [dieser Anleitung](../powershell-install-configure.md) , Verwendung der Cmdlets für Ihre Azure-Abonnement.

Folgendermaßen können Sie schnell loslegen:

1.  Downloaden Sie und installieren Sie der [Azure-PowerShell-Cmdlets](http://go.microsoft.com/?linkid=9811175).
2.  Starten Sie Microsoft Azure PowerShell.
3.  Führen Sie **AzureAccount hinzufügen** zu Ihrem Azure-Abonnement zu authentifizieren. Wenn Sie aufgefordert werden, geben Sie Benutzernamen und Kennwort für die Anmeldung bei Azure-Portal.  
4.  Führen Sie **Get-AzureSubscription** um Ihr Benutzerkonto zugeordneten Abonnements anzuzeigen. 
5.  Führen Sie **Wählen Sie AzureSubscription aus** und geben Sie den Namen oder die ID in der PowerShell-Konsole verwenden.

Herzlichen Glückwunsch, Ihr Azure PowerShell-Konsole konfiguriert und einsatzbereit. Beachten Sie, dass wiederholen Sie Schritte 2 bis 5 bei jedem müssen Sie zuerst die Azure PowerShell-Konsole.  

## <a name="create-a-cloud-collection"></a>Erstellen Sie eine cloudsammlung
--------------------
Es ist einfach, führen Sie folgenden Befehl:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Der obige Befehl veröffentlicht automatisch Microsoft Office 365-Programme (Excel, OneNote, Outlook, PowerPoint, Visio und Word).

Erstellung dauert 30 Minuten oder länger. Daher gibt dieser Befehl eine Tracking-ID, die Sie verwenden können:


    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Nach Abschluss die Auflistung können Sie Benutzer der Auflistung mit dem folgenden Befehl hinzufügen:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Und Sie sind fertig! Benutzer sollen die Anwendung gefunden Azure RemoteApp-Client herstellen [hier](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Verfügbaren cmdlets
Es gibt viele andere Befehle, die wir im zu kurz kommt:

Grundlegende RemoteApp-Sammlung Cmdlets: 

- Neue AzureRemoteAppCollection
- AzureRemoteAppCollection abrufen
- AzureRemoteAppCollection festlegen
- AzureRemoteAppCollection aktualisieren
- AzureRemoteAppCollection entfernen
- Hinzufügen AzureRemoteAppUser
- AzureRemoteAppUser abrufen
- AzureRemoteAppUser entfernen
- AzureRemoteAppSession abrufen
- Trennen AzureRemoteAppSession
- Rufen Sie AzureRemoteAppSessionLogoff
- AzureRemoteAppSessionMessage senden
- AzureRemoteAppProgram abrufen
- AzureRemoteAppStartMenuProgram abrufen
- Veröffentlichen AzureRemoteAppProgram
- Veröffentlichung AzureRemoteAppProgram
- AzureRemoteAppCollectionUsageDetails abrufen
- AzureRemoteAppCollectionUsageSummary abrufen
- AzureRemoteAppPlan abrufen

RemoteApp virtuelles Netzwerk Cmdlets:

- Neue AzureRemoteAppVNet
- AzureRemoteAppVNet abrufen
- AzureRemoteAppVNet festlegen
- AzureRemoteAppVNet entfernen
- AzureRemoteAppVpnDevice abrufen
- Get - AzureRemoteAppVpnDeviceConfigScript
- AzureRemoteAppVpnSharedKey zurücksetzen

RemoteApp Vorlage Bild Cmdlets:

- Neue AzureRemoteAppTemplateImage
- AzureRemoteAppTemplateImage abrufen
- AzureRemoteAppTemplateImage umbenennen
- AzureRemoteAppTemplateImage entfernen

Andere Cmdlets RemoteApp:

- AzureRemoteAppLocation abrufen
- AzureRemoteAppWorkspace abrufen
- AzureRemoteAppWorkspace festlegen
- AzureRemoteAppOperationResult abrufen
 
