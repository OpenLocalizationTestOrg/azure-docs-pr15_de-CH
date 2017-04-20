<properties 
    pageTitle="QuickBooks in Azure RemoteApp bereitstellen | Microsoft Azure" 
    description="Informationen Sie zum Azure RemoteApp QuickBooks freigeben." 
    services="remoteapp" 
    documentationCenter="" 
    authors="ericorman" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="how-do-you-deploy-quickbooks-in-azure-remoteapp"></a>Wie bereitstellen Sie QuickBooks in Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Verwenden Sie die folgende Informationen QuickBooks als Anwendung in Azure RemoteApp freigeben.


Sie können Azure RemoteApp Hybrid oder Cloud Auflistung QuickBooks 2015 Unternehmen freigeben. Die Datei muss auf einem virtuellen Computer, die QuickBooks-Datenbankserver ausgeführt wird, unabhängig von Azure RemoteApp-Server befinden. Nie Unternehmen auf Speichern das Bild Azure RemoteApp - Datenverlust ist dabei erwartet. Nur QuickBooks Enterprise unterstützt QuickBooks-Datei auf eine externe Freigabe mit QuickBooks über standard-Windows-Netzwerk gehostet.   

> [AZURE.IMPORTANT] QuickBooks-Datenbankserver, der die Datei hostet muss auf einem separaten virtuellen Computer innerhalb derselben VNET Azure RemoteApp-Sammlung befinden.  

## <a name="steps-to-deploy-quickbooks"></a>Schritte zum Bereitstellen von QuickBooks

1. Azure-VM erstellen und QuickBooks QuickBooks-Datenbankserver installieren und die Unternehmensdatei Azure VM.  Stellen Sie Firewallregeln konfigurieren.
2. Installieren Sie QuickBooks für ein [benutzerdefiniertes Bild](remoteapp-imageoptions.md) und erstellen Sie eine [Azure RemoteApp-Sammlung](remoteapp-collections.md), Cloud oder Hybrid in den genau gleichen VNET VM Hosten des Datenbankservers QuickBooks mit Unternehmen sich befindet. 
3.  [Veröffentlichen](remoteapp-publish.md) QuickBooks app für Benutzer
4.  Azure RemoteApp gehosteten QuickBooks-Client zu starten, Navigieren mit standardmäßigen Windows-Netzwerke Hosten des Datenbankservers QuickBooks VM und öffnen Sie die Datei. 

## <a name="documentation-references"></a>Dokumentation Verweise

- QuickBooks [unterstützte Konfigurationen](http://enterprisesuite.intuit.com/products/enterprise-solutions/technical/#top)
- QuickBooks- [Bereitstellungsoptionen](http://enterprisesuite.intuit.com/everythingenterprise/launchpad/new-user/)

Sie können der entzünden Präsentation [Grundlagen von Microsoft Azure RemoteApp Management und Administration](https://channel9.msdn.com/Events/Ignite/2015/BRK3868) - vorspulen, 1:02:45 zu QuickBooks Teil auschecken.

## <a name="deployment-architecture"></a>Bereitstellungsarchitektur

![QuickBooks + Azure RemoteApp-Bereitstellung](./media/remoteapp-quickbooks/ra-quickbooks.png)