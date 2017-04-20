<properties 
    pageTitle="Authentifizierung mit Active Directory in der Azure-Anwendung lokal | Microsoft Azure" 
    description="Erfahren Sie mehr über die verschiedenen Optionen für Branchen apps in Azure App Service mit lokalen Active Directory authentifizieren" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="08/31/2016" 
    ms.author="cephalin"/>

# <a name="authenticate-with-on-premises-active-directory-in-your-azure-app"></a>Authentifizierung mit lokalen Active Directory in der Azure-Anwendung #

Dieser Artikel beschreibt, wie die Authentifizierung mit Active Directory (AD) in [Azure App Service](../app-service/app-service-value-prop-what-is.md)vor Ort. Eine Azure-Anwendung in der Cloud gehostet wird, aber es gibt Methoden zur Authentifizierung Active Directory-Benutzer sicheren lokalen. 

## <a name="authenticate-through-azure-active-directory"></a>Azure Active Directory authentifizieren
Eine Mieter Azure Active Directory kann mit einem lokalen Verzeichnis synchronisiert AD. Dieser Ansatz ermöglicht AD App aus dem Internet zugreifen und ihre lokale Anmeldeinformationen authentifizieren. Azure App Service bietet außerdem eine [Lösung für diese Methode](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). Mit wenigen Mausklicks eine Schaltfläche können Sie die Authentifizierung mit einem Verzeichnis synchronisiert Mieter für Ihre Azure-Anwendung aktivieren. Diese Vorgehensweise hat folgende Vorteile:

-   Erfordert keine Authentifizierungscode in Ihrer Anwendung. Ermöglichen Sie App-Dienst die Authentifizierung für Sie und Ihre Zeit auf die Funktionalität Ihrer Anwendung.
-   [Azure AD Graph-API](http://msdn.microsoft.com/library/azure/hh974476.aspx) ermöglicht den Zugriff auf Verzeichnisdaten von Azure-Anwendung.
-   Bietet SSO auf [Alle Anträge von Azure Active Directory unterstützt](/marketplace/active-directory/)Office 365, Dynamics CRM Online Windows Intune und Tausende von Drittanbietern Cloudanwendungen. 
-   Azure Active Directory unterstützt die rollenbasierte Zugriffskontrolle. [Authorize(Roles="X")] Muster können mit minimalem Code.

Azure Line-of-Business-Anwendung schreiben, die in Azure Active Directory authentifiziert, finden Sie unter [Erstellen einer Geschäftsbereich Azure-Anwendung mit Azure Active Directory-Authentifizierung](web-sites-dotnet-lob-application-azure-ad.md).

## <a name="authenticate-through-an-on-premises-sts"></a>Authentifizierung über einen lokalen STS
Haben Sie einen lokalen Sicherheitstokendiensts (STS) wie Active Directory Federation Services (AD FS) können, der Sie Authentifizierung für Ihre Azure Föderation. Dieser Ansatz empfiehlt sich, wenn Unternehmensrichtlinie AD-Daten verhindert in Azure. Beachten Sie jedoch Folgendes:

-   STS-Topologie muss bereitgestellten lokal mit und Management-Overhead.
-   Nur AD FS-Administratoren können [relying Party Vertrauensstellungen und Anspruchsregeln](http://technet.microsoft.com/library/dd807108.aspx)konfigurieren, der Entwickler Optionen einschränken. Auf der anderen Seite kann zum Verwalten und Anpassen von [Ansprüchen](http://technet.microsoft.com/library/ee913571.aspx) auf einer Basis pro Anwendung.
-   Zugriff auf lokalen AD-Daten erfordert eine separate Lösung durch die Unternehmensfirewall.

Schreiben eine authentifiziert Azure Line-of-Business-Anwendung mit einem lokalen STS finden Sie unter [Erstellen einer Geschäftsbereich Azure-Anwendung mit AD FS-Authentifizierung](web-sites-dotnet-lob-application-adfs.md).
 
