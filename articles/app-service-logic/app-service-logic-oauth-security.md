<properties
    pageTitle="Sicherheit in SaaS-Connectors und API-Apps OAUTH | Azure"
    description="Lesen Sie zur Sicherheit von Connectors und API-Apps in Azure App Service OAUTH; Microservices-Architektur; SaaS"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="dwrede"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/23/2016"
    ms.author="mandia"/>


# <a name="learn-about-oauth-security-in-saas-connectors"></a>Erfahren Sie mehr über OAUTH Sicherheit in SaaS-connectors

>[AZURE.NOTE] Diese Version des Artikels gilt für Logik apps 2014-12-01-Vorschau-Schemaversion.

Viele der Software als ein Service (SaaS) Anschlüsse wie Facebook, Twitter, Ablage usw. benötigen Benutzer authentifizieren mit OAUTH-Protokoll.  Bei Verwendung dieser SaaS-Connectors von Logik Apps bieten wir eine vereinfachte Benutzeroberfläche, wo Sie im Designer Logik Apps "Autorisieren" klicken. Wenn Sie **Autorisieren**Sie aufgefordert werden zu signieren in (wenn nicht bereits) Zustimmung Verbindung mit SaaS-Dienst in Ihrem Namen. Nachdem Sie Zustimmung eine und autorisieren, können Logik-Apps dann diese SaaS-Dienste zugreifen.

## <a name="create-your-own-saas-app"></a>SaaS-app erstellen
Diese vereinfachte Erfahrung ist möglich, da wir zuvor erstellt und die Anwendung diese SaaS-Services registriert.  In bestimmten Fällen möchten Sie registrieren und Ihre eigene Anwendung.  Dies wird erforderlich, beispielsweise diese SaaS-Connectors in Ihrer benutzerdefinierten Anwendung verwenden möchten. In diesem Beispiel DropBox-Connector verwendet, aber der Prozess gilt für alle Connectors auf OAUTH.

Auch im Kontext von Logik-Apps können Sie Ihre eigene Anwendung anstelle der standardanwendung, die wir bereitstellen. Wenn die Schaltfläche "Autorisieren" keine Verbindung herstellen, können Sie versuchen, Ihre Anwendung erstellen. Im folgenden werden Schritte für Twitter-Connector:

1. Öffnen des Twitter-Connectors in Azure vorschauportal. **Wechseln Sie**zu > **API-Apps**. Wählen Sie Ihr Twitter-Connector:  
    ![][1]

2. **Wählen Sie in** > **Authentifizierung**:  
    ![][2]

3. Kopieren Sie den Wert **URI umleiten** :  
    ![][3]

4. Zur [Twitter](http://apps.twitter.com) und **eine neue Anwendung erstellen**. Fügen Sie im **Callback-URL** -Eigenschaft den Wert **URI umleiten** von Twitter-Connector: ![][4]  
5. Beim Erstellen der Twitter-Anwendung wählen Sie **Schlüssel und Zugriffstoken**. Kopieren Sie diese Werte.
6. Fügen Sie diese Werte in der Twitter-Authentifizierung Connectoreinstellungen in die **Client-ID** und **Geheimen** :   
    ![][5]  
7. Speichern der Connectoreinstellungen.  

Jetzt sollten Sie den Connector von Logik Apps verwenden können. Wenn Sie diesen Connector von Logik-Apps verwenden, verwendet die Anwendung anstelle der standardanwendung.  

> [AZURE.NOTE] Wenn Sie bereits eine Anwendung autorisiert haben, müssen Sie die Anwendung erneut zu autorisieren.


<!--Image references-->
[1]: ./media/app-service-logic-oauth-security/TwitterConnector.png
[2]: ./media/app-service-logic-oauth-security/Authentication.png
[3]: ./media/app-service-logic-oauth-security/RedirectURI.png
[4]: ./media/app-service-logic-oauth-security/TwitterApp.png
[5]: ./media/app-service-logic-oauth-security/TwitterKeys.png
