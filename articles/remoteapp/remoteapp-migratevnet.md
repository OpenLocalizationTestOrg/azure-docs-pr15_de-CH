<properties
    pageTitle="Migrieren von RemoteApp VNET, ein VNET Azure | Microsoft Azure"
    description="Informationen Sie zum Migrieren von RemoteApp VNET zu einem VNET Azure"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="how-to-migrate-a-hybrid-collection-from-a-remoteapp-vnet-to-an-azure-vnet"></a>Migrieren von Hybrid-Sammlung von RemoteApp VNET um eine Azure-VNET

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Gute Nachrichten! Wir konnten Hybrid RemoteApp-Sammlungen direkt in Ihre vorhandene Azure virtuelle Netzwerke (VNETs) anstatt RemoteApp-spezifische VNETs bereitstellen. Auf diese Weise können Sie nutzen die neuesten VNET Funktionen (z. B. ExpressRoute) und Ihre Hybriden Sammlungen direkten Netzwerkzugriff zu anderen Azure Services, VNET eingesetzt.  (Dies wird eine bessere Leistung und vereinfachte Einrichtung als VNET VNET Konfigurationen).


Angenommen, eine hybride RemoteApp Kollektion *OriginalCollection* RemoteApp VNET aufgerufen *RemoteAppVNET*bereits erstellt haben. Hier werden die Schritte zum Migrieren zu einer neuen Azure-VNET namens *AzureVNET*.

1.  Erstellen Sie auf der Registerkarte **Netzwerke** im [Verwaltungsportal](http://manage.windowsazure.com/)ein VNET namens *AzureVNET*, mit dem gleichen Speicherort, DNS-Konfiguration und Adressraum (mindestens *AzureVNET* Subnetze) für *RemoteAppVNET*.
2.  *AzureVNET* oder Host konfigurieren oder Netzwerkkonnektivität und die Active Directory-Bereitstellung, die *OriginalCollection* Domäne verbunden ist.
3.  Erstellen Sie auf der Registerkarte **RemoteApps** RemoteApp Portfolio *Neue Auflistung*aufgerufen. (Verwenden Sie die Option **Erstellen vnet** , nicht **Schnell erstellen**.)
3.  Konfigurieren Sie *NewCollection* in einem Subnet in *AzureVNET*bereitgestellt werden.
4.  Konfigurieren Sie *NewCollection* um dasselbe Bild und Domäneninformationen Join wie für *OriginalCollection*verwenden.
5.  Nach ein paar Stunden werden *NewCollection* in der Liste mit aktiven Status angezeigt.

Jetzt benötigen Sie keine Benutzerinformationen aus der ursprünglichen Sammlung neue Sammlung zu migrieren, führen Sie diese Schritte weiter:

6.  Löschen Sie *OriginalCollection*
7.  Löschen Sie *RemoteAppVNET*

Und Sie sind fertig!

Auch wenn Sie Benutzerinformationen aus der ursprünglichen Sammlung neue Sammlung migrieren müssen, führen Sie diese Schritte weiter:

6.  Senden Sie eine e-Mail an [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20user%20information%20migration) Ihre Azure-Abonnement-ID, den Namen der ursprünglichen Sammlung und den Namen der neuen Auflistung und bitten um Ihre Benutzerinformationen zu migrieren.
7.  Innerhalb von 2 Werktagen verschiebt RemoteApp-Team Benutzerzugriffsliste und alle Benutzerdokumente und Einstellungen aus der ursprünglichen Sammlung neue Sammlung.
8.  Löschen Sie *OriginalCollection*
9.  Löschen Sie *RemoteAppVNET*

Und nun sind Sie fertig!

Wenn Sie Fragen oder benötigen Sie spezielle Hilfe e-Mail- [remoteappforum@microsoft.com](mailto:remoteappforum@microsoft.com?subject=Azure%20RemoteApp%20VNET%20migration%20help).
