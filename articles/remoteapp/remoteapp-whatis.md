<properties 
    pageTitle="Was ist Azure RemoteApp? | Microsoft Azure" 
    description="Erfahren Sie, wie apps und Ressourcen auf alle Geräte über Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="what-is-azure-remoteapp"></a>Was ist Azure RemoteApp?

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Azure RemoteApp bringt die Funktionalität der lokalen Microsoft RemoteApp-Programm von Remote Desktop Services in Azure unterstützt. Azure RemoteApp können Sie sicheren remote-Zugriff auf Applikationen über viele verschiedene Geräte ermöglichen. Azure RemoteApp hostet grundsätzlich nicht persistent Terminal Server-Sitzung in der Cloud, und erhalten sie und Ihre Benutzer freigeben.

Azure RemoteApp können Sie apps und Ressourcen für Benutzer von fast jedem Gerät freigeben. Wir hosten Ihre apps in die Cloud bedeutet, dass wir die Hardware kümmern und Benutzer Anforderungen skalieren. Müssen Sie lediglich hochladen apps, die Sie freigeben möchten und dann die Benutzer diese apps verwenden. [Benutzer können ihre eigenen Geräte](remoteapp-clients.md), während Sie alles über das Azure-Portal verwalten. Sie können auch auf die Unternehmensanmeldeinformationen lassen Sie Sicherheit apps und Daten.

Weitere Informationen zu Azure RemoteApp oder, wenn wir Sie [Probieren sie](https://azure.microsoft.com/services/remoteapp/)überzeugt haben.

Fragen zur Azure RemoteApp? Sehen Sie sich unsere [FAQ](remoteapp-faq.md).

Azure RemoteApp ist Teil der [Microsoft Virtual Desktop Infrastructure](http://www.microsoft.com/server-cloud/products/virtual-desktop-infrastructure/explore.aspx).

**Neu!** Weitere Informationen zu Azure RemoteApp möchten? Oder Azure RemoteApp Ebene überprüfen? Verknüpfen Sie unseren wöchentlichen [Experten Webinar](https://azureinfo.microsoft.com/AzureRemoteAppAskTheExperts-Registration-Page.html?ls=Website).

## <a name="azure-remoteapp-collections"></a>Azure RemoteApp-Sammlungen
Es gibt zwei Arten von [Azure RemoteApp-Sammlungen](remoteapp-collections.md):


- **Cloud-Sammlung** wird und Daten für Programme in der Cloud gespeichert. Benutzer können sich mit ihrem Microsoft-Konto oder Unternehmensanmeldeinformationen synchronisiert oder Verbund mit Azure Active Directory apps zugreifen.

    Wählen Sie einer Cloud-Sammlung bei Anwendung freigeben möchten keine Verbindung zu Ressourcen erforderlich private Netzwerk des Unternehmens (z. B. über ein VPN-Gerät). Wenn die Anwendung Ressourcen im Internet, OneDrive oder Azure verwendet, funktionieren eine Cloud-Sammlung. Außerdem ist es am schnellsten erstellen.

- **Hybrid-Sammlung** wird und speichert Daten in der Azure-Cloud aber auch können Benutzer Zugriff auf Daten und Ressourcen im lokalen Netzwerk. Benutzer können sich mit ihren Unternehmensanmeldeinformationen synchronisiert oder Verbund mit Azure Active Directory apps zugreifen.

    Wählen Sie eine Hybrid-Sammlung benötigen Sie eine Verbindung zu Ressourcen im privaten Netzwerk Ihres Unternehmens. Z. B. wenn die Anwendung Zugriff auf die folgenden:

    - Dateiservern im intranet
    - Quicken
    - Datenbanken hinter einer firewall

    Dies ist in der Regel nützlicher für große Unternehmen mit vielen Ressourcen auf ihren privaten Netzwerken in die Cloud verschoben werden können.

Anderen Sammlungen haben verschiedene Optionen, einschließlich Netzwerke, herauszufinden, [welche Auflistung](remoteapp-collections.md) für Sie am besten geeignet. 


### <a name="updating-your-collection"></a>Ihre Sammlung aktualisieren
Die wichtigsten Unterschiede zwischen den Hybriden und Cloud gehört wie Software-Updates behandelt werden. Mit einer Cloud-Auflistung, die vorinstallierte Office 365 ProPlus oder Office 2013-Bild verwendet, müssen Sie keinen Updates kümmern. Der Dienst selbst verwaltet und rollt Updates laufend apps und Betriebssystem.

Hybrid-Sammlungen sowie Cloud-Sammlungen, die eine benutzerdefinierte Vorlage benutzen, sind für Bild und apps erhalten bleiben. Domäne Bilder können Sie mit Tools wie Windows Update, Gruppenrichtlinien und System Center Updates steuern.

Nach dem aktualisieren Ihre benutzerdefinierte Vorlage Bilds das neue Bild in Azure Cloud hochladen und aktualisieren die Auflistung um das neue Bild verwenden. (Sie können die Seite Azure RemoteApp **Schnellstart** oder Dashboard dazu.)

Weitere Informationen finden Sie unter [Aktualisieren die Auflistung](remoteapp-update.md) .

## <a name="supported-remoteapp-clients"></a>Unterstützte RemoteApp-clients
Azure RemoteApp wird auf RemoteApp-Clientanwendungen für Windows und Windows-RT Microsoft Remote Desktop-apps für Mac, iOS und Android unterstützt. Benutzer verwenden diese apps auf ihrem Handy oder berechnen Geräte neue Azure RemoteApp-Programme zugreifen.

Weitere Informationen zu den Clients finden Sie unter [Zugriff auf Ihre apps in Azure RemoteApp](remoteapp-clients.md) .

## <a name="next-steps"></a>Nächste Schritte
Go! Probieren Sie es aus! Diese Artikel helfen Ihnen Azure RemoteApp:

- [Welche Art von Auflistung benötigen Sie für Azure RemoteApp?](remoteapp-collections.md)
- [Erstellen eines Abbildes Azure RemoteApp](remoteapp-imageoptions.md)
- [Erstellen eine cloudsammlung von Azure RemoteApp](remoteapp-create-cloud-deployment.md)
- [Erstellen eine hybridsammlung von Azure RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Funktionsweise Lizenzierung in Azure RemoteApp](remoteapp-licensing.md)
- [Tipps für die Verwendung von Azure RemoteApp](remoteapp-bestpractices.md)
- [Häufig gestellte Fragen zur Azure RemoteApp](remoteapp-faq.md)
 

### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen helfen 
Wussten Sie, dass neben diesen Artikel Bewertung und Kommentare unten unten, Sie Artikel selbst ändern können? Fehlt etwas? Etwas? Habe ich etwas schreiben, die nur verwirrend? Bildlauf nach oben und klicken **auf GitHub bearbeiten** oder **Bearbeiten** ändern - die Überprüfung zu uns kommen und danach, sobald wir sie abzeichnen, sehen Sie Ihre Änderung und Verbesserung hier.