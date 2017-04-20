<properties 
    pageTitle="Häufig gestellte Fragen zur Azure RemoteApp | Microsoft Azure" 
    description="Enthält Antworten auf häufig gestellte Fragen zur Azure RemoteApp." 
    services="remoteapp" 
    documentationCenter="" 
    authors="lizap" 
    manager="swadhwa" 
    editor=""/>

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="azure-remoteapp-faq"></a>Häufig gestellte Fragen zur Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Wir haben die folgenden Fragen zur Azure RemoteApp gehört. Haben andere? [RemoteApp-Foren](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureRemoteApp) und uns benötigen oder einen Kommentar unten Dropdown.

## <a name="cant-find-what-youre-looking-for-have-a-question-we-didnt-answer"></a>Finden kann nicht Sie suchen? Fragen beantworten wir nicht?
Wenn Sie die Informationen finden müssen, oder weitere Fragen, die wir hier nicht behandelt werden, bitte zum [Azure RemoteApp-Forum](http://aka.ms/araforum) und formulieren Sie Ihre Frage. Wir können jederzeit weitere Antworten hinzufügen.

## <a name="what-is-azure-remoteapp"></a>Was ist Azure RemoteApp? ##


- **Was ist Azure RemoteApp?** RemoteApp ist Azure Service bieten sicheren remote-Zugriff für Clientanwendungen aus vielen unterschiedlichen Geräte hilft. Weitere Informationen über [Azure RemoteApp](remoteapp-whatis.md).
- **Was sind die Bereitstellungsoptionen?** Es gibt zwei Arten von RemoteApp-Sammlungen: Cloud und Hybrid. Die gewünschten hängt einer Reihe von Faktoren wie Sie beitreten zu einer Domäne brauchen. Wir sprechen über die Entscheidungen [hier](remoteapp-collections.md).

## <a name="quick-tips-on-using-azure-remoteapp"></a>Tipps zur Verwendung von Azure RemoteApp ##
- **Wie lange kann ich unterbrochen? Wie lange kann ich im Leerlauf, bevor Sie den Startvorgang mir?** 4 Stunden. Wenn Sie oder ein Benutzer im Leerlauf für 4 Stunden, werden Sie automatisch von Azure RemoteApp angemeldet. Checken Sie die anderen Standardeinstellungen in [Azure-Abonnement und Service Grenzen Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
- **Kann ich diesen Dienst kostenlos testen?** Ja. Gibt eine kostenlose Testversion für 30 Tage. Nach der Testphase können Sie die Umstellung auf ein kostenpflichtiges Konto (die in der Produktion verwenden können) oder den Service. Starten Sie Ihre kostenlose Testversion zu [portal.azure.com](http://portal.azure.com) - Erstellen einer neuen Instanz von RemoteApp. Mit der Testversion können Sie 2 Instanzen der RemoteApp 10 Benutzer pro Instanz erstellen. Beachten Sie, dass diese Testversion nur für 30 Tage lebt.
## <a name="azure-remoteapp-subscription-details"></a>Azure RemoteApp Abonnementdetails ##

- **Was sind die Grenzwerte Service?** Sie können die Standardeinstellungen und Service Grenzen Azure RemoteApp [Azure-Abonnement](../azure-subscription-service-limits.md)und Service Grenzen, Kontingente Einschränkungen kennen. Lassen Sie uns wissen Sie, wenn Sie weitere Fragen haben.
- **Wie viele Benutzer haben müssen?** Es ist mindestens 20 Benutzer. Nochmals, dass super - sind mindestens 20 ist. Sie werden für 20 abgezogen. 
- **Wie viel kostet RemoteApp?** [Azure RemoteApp Preisdetails ](https://azure.microsoft.com/pricing/details/remoteapp/)anzeigen
- **Kostet eine Art von Auflistung über einen anderen?** Ja, es kann, je nach Bedarf Auflistung. Hybrid-Auflistung erfordert eine Verbindung mit dem lokalen Netzwerk von Azure RemoteApp. Verwenden Sie eine vorhandene VNET-Express Route ist kostenlos. Aber bei Verwendung einer neuen Azure-VNET und ein Gateway oder Express Route wird berechnet für [VPN-Gateway](https://azure.microsoft.com/pricing/details/vpn-gateway) oder [Express Route](https://azure.microsoft.com/pricing/details/expressroute/). Diese (Siehe Links) ist auf Ihrem monatlichen Azure RemoteApp Kosten.

## <a name="collections---whats-supported-which-should-you-use-and-others"></a>Sammlungen - was unterstützt die sollten Sie verwenden, usw.
- **Unterstützt benutzerdefinierte Line-of-Business (LOB)-Applikationen werden?** Ja. Um eine benutzerdefinierte Anwendung in Azure RemoteApp verwenden, erstellen Sie eine [benutzerdefinierte Vorlagenbild](remoteapp-create-custom-image.md)und uploaden Sie RemoteApp-Auflistung.
- **Funktioniert Meine benutzerdefinierte LOB-Anwendung in Azure RemoteApp?** Die beste Möglichkeit auf, die Sie testen. [RD Compatibility Center](http://www.rdcompatibility.com/compatibility/default.aspx)anzeigen
- **Bereitstellungsmethode (Cloud oder Hybrid) für mein Unternehmen am besten ist?** Hybrid-Sammlungen bieten die umfassendste Erfahrung vollständige Integration mit einmaliges Anmelden (SSO) und sichere lokale Netzwerkkonnektivität. Cloud-Sammlungen bieten eine Möglichkeit schnell und einfache die Bereitstellung über mehrere Authentifizierungsmethoden isolieren. Weitere Informationen über [Bereitstellungsoptionen](remoteapp-whatis.md).
- **Wir haben SQL Datenbank entweder lokal oder in Azure. Welcher Bereitstellungstyp verwenden wir?** Dies die SQL oder Back-End-Datenbank ist abhängig. Wenn die Datenbank in einem privaten Netzwerk, verwenden Sie Hybrid-Auflistung. Wenn die Datenbank im Internet ausgesetzt und Clients Verbindungen ermöglicht herstellen, können Sie die Cloud-Sammlung.
- **Was Zuordnung zu Laufwerk, USB und serielle gemeinsame Nutzung der Zwischenablage und Clientdruckerumleitung?** Alle diese Funktionen werden in Azure RemoteApp unterstützt. Umleitung über Zwischenablage freigeben und Drucker ist standardmäßig aktiviert. Erfahren Sie mehr über Umleitung [hier](remoteapp-redirection.md). 


## <a name="template-images"></a>Vorlage Bilder
- **Kann ich eine Cloud oder vorhandenen virtuellen Computer als Vorlage für meine RemoteApp-Sammlung verwenden?** Ja! Sie können ein Bild einer Azure VM erstellen, verwenden Sie eines der Bilder in Ihrem Abonnement enthalten oder erstellen Sie ein benutzerdefiniertes Bild. [RemoteApp Bildoptionen](remoteapp-imageoptions.md)anzeigen


## <a name="network-options"></a>Netzwerkoptionen
- **Die Hybrid-Auflistung ist ein VNET erforderlich. Können wir unsere bestehenden VNET?** Sie können vorhandene VNET ist ein Azure-VNET. Siehe "Schritt 1: Richten Sie Ihr virtuelles Netzwerk" [Hybrid Anweisungen](remoteapp-create-hybrid-deployment.md) für Weitere Informationen.
- **Kann ich ein VNET mit Cloud verwenden?** Tatsächlich können Sie. [Erstellen einer cloudsammlung](remoteapp-create-cloud-deployment.md), insbesondere Schritt 1 Weitere Informationen anzeigen

## <a name="authentication-options"></a>Optionen



- **Wie Authentifizierung? Welche Methoden werden unterstützt?** Die Cloud-Sammlung unterstützt Microsoft Konten und Azure Active Directory-Konten, die Office 365-Konten. Hybrid-Auflistung unterstützt nur Azure Active Directory-Konten, die synchronisiert (mit einem Tool wie [Azure Active Directory-Synchronisierung](http://blogs.technet.com/b/ad/archive/2014/09/16/azure-active-directory-sync-is-now-ga.aspx)) aus Windows Server Active Directory-Bereitstellung. insbesondere oder synchronisiert mit Kennwortsynchronisation synchronisiert mit Active Directory-Verbunddienste (AD FS) konfiguriert. Sie können auch [Mehrstufige Authentifizierung (MFA)](https://azure.microsoft.com/services/multi-factor-authentication/)konfigurieren.

>[AZURE.NOTE]Azure Active Directory-Benutzer muss vom Mieter, die Ihrem Abonnement zugeordnet ist. (Sie können anzeigen und Ändern Ihres Abonnements auf **die Registerkarte im Portal** . Siehe [Ändern der Azure Active Directory Mieter von RemoteApp verwendet](remoteapp-changetenant.md) Informationen.)

- **Warum gewähren kann nicht meine Azure Active Directory-Kontozugriff?** Azure Active Directory-Benutzer muss aus dem Verzeichnis, die Ihrem Abonnement zugeordnet ist. Sie können anzeigen oder ändern das Verzeichnis auf der Registerkarte im Portal. Weitere Informationen finden Sie unter [Ändern der Azure Active Directory Mieter von RemoteApp verwendet](remoteapp-changetenant.md) .

## <a name="clients---what-device-can-i-use-to-access-azure-remoteapp"></a>Kunden - Gerät kann auf Azure RemoteApp verwende?
Finden Sie gute Informationen, einschließlich der Schritte zum Installieren von verschiedenen Clients auf [Ihre apps in Azure RemoteApp zugreifen](remoteapp-clients.md).

- **Welche Geräte und Betriebssysteme unterstützen die Clientanwendungen?**
Erste Computer und Tablets: 
    - Windows 10 (Client Vorschau)
    - Windows 8.1 und Windows 8
    - Windows 7 Servicepack 1
    - Mac OS X
    - Windows RT
    - Android tablets
    - iPad und die Telefone:
    - iPhone
    - Android-Telefon
    - Windows Phone
 
    [Download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) jetzt ein RemoteApp-Client.
- **Unterstützt Azure RemoteApp Thin Clients?** Ja, werden die folgenden thin Clients mit Windows Embedded unterstützt:
    - Windows Embedded Standard 7
    - Windows Embedded 8 Standard
    - Windows Embedded 8.1 Industry Pro
    - Windows 10 IoT Enterprise

- **Welche Version von Windows Server für Remote Desktop Session Host (RDSH) unterstützt?** Windows Server 2012 R2.

## <a name="support-and-feedback"></a>Support und feedback


- **Was ist der Plan für RemoteApp?** Abrechnung und Abonnement-Management wird nicht unterstützt. Support ist über die [Azure Service-Pläne](https://azure.microsoft.com/support/plans/). Sie können auch kostenlose communitysupport durch [Azure Diskussionsforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)abrufen. 
- **Wie geben kann ich Feedback?** Besuchen Sie das [Bewertungsportal besucht](https://feedback.azure.com/forums/247748-azure-remoteapp/).
- **Wen kann ich weitere Informationen zu Azure RemoteApp?** Neben unseren [Diskussionsforum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=AzureRemoteApp)ist gut, Fragen begleiten wöchentlich [Ask Webinar Experten](https://azureinfo.microsoft.com/US-Azure-WBNR-FY15-11Nov-AzureRemoteAppAskTheExperts-Registration-Page.html)Sie wo wir alles RemoteApp reden.
- **Was RemoteApp-Dokumentation?** Wir freuen uns, dass Sie Fragen. Neben den Hilfeinhalt in der Schublade Portal-Hilfe (klicken Sie einfach auf **?** die folgenden Artikel stehen vermitteln Sie auf einer beliebigen Seite im Portal) um RemoteApp:
    - **Erste Schritte:**
        - [Was ist RemoteApp?](remoteapp-whatis.md)
        - [Was ist die RemoteApp Vorlage Bilder?](remoteapp-images.md)
        - [Wie funktioniert Lizenzierung?](remoteapp-licensing.md)
        - [Wie Arbeiten RemoteApp und Office zusammen?](remoteapp-o365.md)
        - [Umleitung funktioniert in RemoteApp](remoteapp-redirection.md)?
    - **Bereitstellen:**
        - [Erstellen Sie eine benutzerdefinierte Vorlagenbild](remoteapp-create-custom-image.md)
        - [Erstellen einer Hybrid-Auflistung](remoteapp-create-hybrid-deployment.md)
        - [Erstellen Sie eine cloudsammlung](remoteapp-create-cloud-deployment.md)
        - [Konfigurieren von Azure Active Directory für RemoteApp](remoteapp-ad.md)
        - [Veröffentlichen Sie eine Anwendung in RemoteApp](remoteapp-publish.md)
    - **Verwalten:**
        - [Hinzufügen von Benutzern](remoteapp-user.md)
        - [Best Practices für Konfiguration und Verwendung von RemoteApp](remoteapp-bestpractices.md)  

    Videos! Wir haben auch eine Anzahl von Videos über RemoteApp. Einige stellen Introduction ([Einführung in Azure RemoteApp](https://azure.microsoft.com/documentation/videos/cloud-cover-ep-150-azure-remote-app-with-thomas-willingham-and-nihar-namjoshi/)), während andere Deployment ([Bereitstellung Cloud](https://www.youtube.com/watch?v=3NAv2iwZtGc&feature=youtu.be) und [Hybrid-Bereitstellung man](https://www.youtube.com/watch?v=GCIMxPUvg0c&feature=youtu.be)). Schauen sie sich!

 
### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen helfen 
Wussten Sie, dass neben diesen Artikel Bewertung und Kommentare unten unten, Sie Artikel selbst ändern können? Fehlt etwas? Etwas? Habe ich etwas schreiben, die nur verwirrend? Bildlauf nach oben und auf Änderungen **auf GitHub bearbeiten** - die Überprüfung zu uns kommen und danach, sobald wir sie abzeichnen, sehen Sie Ihre Änderung und Verbesserung hier.
