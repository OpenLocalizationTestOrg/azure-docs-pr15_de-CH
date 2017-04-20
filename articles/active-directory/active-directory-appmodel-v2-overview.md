<properties
    pageTitle="v2. 0-Endpunkt Übersicht | Microsoft Azure"
    description="Eine Einführung zum Erstellen von apps mit Microsoft Account und Azure Active Directory anmelden."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>-In Microsoft Account & Azure AD-Benutzer in einer einzigen Anwendung

In der Vergangenheit musste ein app-Entwickler, die Microsoft-Konten und Azure Active Directory unterstützen zwei separate Systeme integrieren.  Wir haben jetzt eine neue Authentifizierung API-Version eingeführt, die Benutzer mit beide Konten Azure AD-System anmelden können.  Diese konvergierte Authentifizierungssystem wird als **v2. 0-Endpunkt**bezeichnet.  Mit dem Endpunkt v2. 0 ermöglicht eine einfache Integration Publikum, das Millionen von Benutzern mit persönlichen und Arbeit oder Schule umfasst.

Apps, die v2. 0-Endpunkt können auch REST-APIs von [Microsoft Graph](https://graph.microsoft.io) und [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) Einsatz des Kontos verwenden.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Erste Schritte
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Wählen Sie Ihre bevorzugte Plattform aus der folgenden Liste eine Anwendung mit unseren open-Source-Bibliotheken und Frameworks erstellt.  Alternativ können Sie Dokumentation Protokoll OAuth 2.0 & OpenID Connect senden und Empfangen von Nachrichten ohne eine Auth-Bibliothek.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Neuigkeiten
Die grundlegende Informationen werden sich was und was nicht mit der Version 2.0 möglich.

- Informationen Sie zu den [Typen von apps erstellen mit der v2. 0](active-directory-v2-flows.md)
- Verstehen der [Grenzen, Einschränkungen und Einschränkungen](active-directory-v2-limitations.md) mit der v2. 0.
- Wir haben vor kurzem [Admin eingeschränkte Bereiche](active-directory-v2-scopes.md) und [Anmeldeinformationen gewährt OAuth2 Client](active-directory-v2-protocols-oauth-client-creds.md)unterstützt.  Ausprobieren!

## <a name="reference"></a>Referenz
Diese Links werden die Plattform eingehend untersuchen:

- Build 2016: [Erste Schritte mit Microsoft: Enterprise Klasse anmelden für Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Hilfe mit dem [Azure Active Directory](http://stackoverflow.com/questions/tagged/azure-active-directory) Stapelüberlauf oder [adal](http://stackoverflow.com/questions/tagged/adal) Tags.
- [v2. 0 Protokoll Referenz](active-directory-v2-protocols.md)
- [Tokenverweis v2. 0](active-directory-v2-tokens.md)
- [Bibliotheksverweis v2. 0](active-directory-v2-libraries.md)
- [Bereiche und Zustimmung in V2. 0-Endpunkt](active-directory-v2-scopes.md)
- [Das Portal Microsoft App-Registrierung](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST-API-Referenz](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Microsoft Graph](https://graph.microsoft.io)