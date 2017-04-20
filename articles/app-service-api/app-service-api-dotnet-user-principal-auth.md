<properties
    pageTitle="Benutzerauthentifizierung für API-Apps in Azure App Service | Microsoft Azure"
    description="Informationen Sie zu einer API-app in Azure App Service von Zugriff nur für authentifizierte Benutzer."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Benutzerauthentifizierung für API-Apps in Azure App Service

## <a name="overview"></a>Übersicht

Dieser Artikel veranschaulicht, wie eine Azure-API-Anwendung zu schützen, sodass nur authentifizierte Benutzern aufgerufen werden können. Es wird vorausgesetzt, dass Sie [Azure App Service-Authentifizierung Overview](../app-service/app-service-authentication-overview.md)gelesen haben.

Sie erfahren:

* Einen Authentifizierungsanbieter mit Azure Active Directory (Azure AD) konfigurieren
* Wie Sie eine geschützte API-app mit [Active Directory Authentifizierung Library (ADAL) für JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Der Artikel enthält zwei Abschnitte:

* Im Abschnitt [Benutzerauthentifizierung in Azure App Service konfigurieren](#authconfig) im Allgemeinen erläutert Benutzerauthentifizierung für jede API-Anwendung konfiguriert und gilt für alle Frameworks von App Service, einschließlich .NET, Node.js und Java unterstützt.

* Beginnend mit [.NET API-Apps Lernprogramme weiterhin](#tutorialstart) Abschnitt Artikel Hilfslinien durch Konfigurieren einer beispielanwendung mit einem back-End und ein AngularJS-Frontend, dass Azure Active Directory für die Authentifizierung verwendet. 

## <a id="authconfig"></a>Konfigurieren der Benutzerauthentifizierung in Azure App Service

Dieser Abschnitt enthält allgemeine Informationen, die für jede API-Anwendung gelten. Schritte werden Liste .NET Beispiel Anwendung weiterhin [.NET Einsteiger - Lernprogramme](#tutorialstart)finden.

1. Navigieren Sie in [Azure-Portal](https://portal.azure.com/)zu Blatt **Einstellungen** der API-app die zu schützen, finden im Abschnitt **Features** und dann auf **Authentifizierung / Autorisierung**.

    ![Azure-Portal Authentifizierung](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In der **Authentifizierung / Autorisierung** Blade, klicken Sie **auf**.

4. Wählen Sie eine der Optionen aus der Dropdown-Liste **Aktion Anforderung nicht authentifiziert ist** .

    * Soll nur authentifizierte Aufrufe zu API-app wählen Sie eine der Optionen **Anmelden mit...** . Diese Option ermöglicht das Schützen der API-app ohne Schreiben von Code, der es ausgeführt wird.

    * Wählen Sie alle API-app erreichen, **Anforderung zulassen (keine Aktion)**. Diese Option können nicht authentifizierte Aufrufer Authentifizierungsanbieter zu leiten. Mit dieser Option müssen Sie Code schreiben, um die Autorisierung zu behandeln.

    Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung für API-Apps in Azure App Service](app-service-api-authentication.md#multiple-protection-options).

5. Wählen Sie eine oder mehrere der **Authentifizierungsanbieter**.

    Das Bild zeigt Optionen an, die alle Aufrufer von Azure AD authentifiziert werden müssen.
 
    ![Azure Portal Authentifizierung blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Wenn Sie Authentifizierungsanbieter auswählen, zeigt das Portal Konfiguration Blade-Anbieters. 

    Informationen, die die Authentifizierung Anbieter Konfiguration Blades konfigurieren erklären finden Sie unter [Ihre App mit Azure Active Directory-Konto konfigurieren](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Der Link führt zu einem Artikel über Azure AD, aber der Artikel selbst enthält Registerkarten für die anderen Authentifizierungsanbieter mit Artikeln verknüpfen.)

7. Klicken Sie mit Authentifizierung Anbieter Konfiguration fertig sind auf **OK**.

7. In der **Authentifizierung / Autorisierung** Blade, klicken Sie auf **Speichern**.

Wenn dies geschieht, authentifiziert App Service alle API-Aufrufe vor der API-app. Die Authentifizierungsdienste funktioniert für alle Sprachen, die App Service unterstützt, einschließlich .NET, Node.js und Java. 

Authentifizierte API-Anrufe, enthält der Aufrufer den Authentifizierungsanbieter OAuth 2.0 trägertoken in der Authorization-Header der HTTP-Anfragen. Das Token kann mithilfe der Authentifizierungsanbieter SDK erworben werden.

## <a id="tutorialstart"></a>Fortsetzen der .NET API-Apps-Lernprogramme

Wenn Sie die Lernprogramme Node.js oder Java API-Apps sind, fahren Sie mit nächsten Artikel [principal Authentifizierung für API-apps](app-service-api-dotnet-service-principal-auth.md). 

Wenn nachfolgende .NET Tutorial Reihe für API-apps und die beispielanwendung bereits haben wie im [ersten](app-service-api-dotnet-get-started.md) und [zweiten](app-service-api-cors-consume-javascript.md) Lernprogramme springen Sie zum Abschnitt [App Service und Azure AD-Authentifizierung einrichten](#azureauth) .

Ggf. Lernprogramms ohne Lernprogramme und gehen Sie folgendermaßen vor die erklären, wie mithilfe eines automatisierten Prozesses Sample-Anwendung bereitstellen.

>[AZURE.NOTE] Die folgenden Schritte können Sie demselben Ausgangspunkt wie würden die ersten beiden Lernprogramme, mit einer Ausnahme: Visual Studio wird nicht wissen, welche Web oder API-app, die für jedes Projekt bereitgestellt wird. Das bedeutet, dass Sie genaue Anleitung in diesem Lernprogramm nicht, die erklären, wie die richtigen Ziele bereitgestellt. Wenn Sie nicht mit herauszufinden, wie die Bereitstellungsschritte auf eigene, empfiehlt das [erste Lernprogramm](app-service-api-dotnet-get-started.md) als mit dieser automatisierte Bereitstellungsprozess zu beginnen die Tutorials folgen.

1. Stellen Sie sicher, dass alle erforderlichen Komponenten in das [erste Lernprogramm](app-service-api-dotnet-get-started.md)aufgeführt. Neben den aufgeführten Komponenten vorausgesetzt diese Authentifizierung Lernprogramme App Service webapps und API-apps in Visual Studio und Azure-Portal gearbeitet haben.

2. Klicken Sie **Bereitstellen in Azure** in der [Liste Beispiel Repository-Infodatei](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) API-apps und Web app. Notieren Sie sich die erstellt wird, wie Sie diese später WebApp und API-app Namen nachschlagen, Azure-Ressourcengruppe.
 
3. Herunterladen Sie oder Klonen Sie [Liste Beispiel Repository](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) zu Sie mit lokal in arbeiten Visual Studio den Code.

## <a id="azureauth"></a>In App Service und Azure AD-Authentifizierung einrichten

Sie haben jetzt die Anwendung in Azure App Service ohne Benutzer authentifiziert werden. In diesem Abschnitt fügen Sie Authentifizierung, führen Sie die folgenden Aufgaben:

* Konfigurieren Sie App Dienst Azure Active Directory (Azure AD) Authentifizierung zum Aufrufen der Zwischenebene API-Anwendung.
* Erstellen einer Azure AD-Anwendung.
* Konfigurieren Sie die Anwendung Azure AD AngularJS Frontend trägertoken nach Anmeldung an. 

Wenn Sie Probleme beim Lernprogramm Richtung ausführen, finden Sie im Abschnitt [Problembehandlung](#troubleshooting) am Ende des Lernprogramms. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Konfigurieren der Authentifizierung für die mittlere Ebene API-app

1. Navigieren Sie in [Azure-Portal](https://portal.azure.com/)zu Blatt **Einstellungen** der API-Anwendung, die Sie erstellt für das Projekt ToDoListAPI, finden Sie im Abschnitt **Features** und dann auf **Authentifizierung / Autorisierung**.

    ![Azure-Portal Authentifizierung](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. In der **Authentifizierung / Autorisierung** Blade, klicken Sie **auf**.

4. Wählen Sie in der Dropdownliste **Aktion Anforderung nicht authentifiziert** **Azure Active Directory einloggen**.

    Dadurch wird sichergestellt, dass keine Anfragen Unauathenticated API-app erreichen. 

5. Klicken Sie unter **Authentifizierungsanbieter**auf **Azure Active Directory**.

    ![Azure Portal Authentifizierung blade](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. Klicken Sie im Blatt **Azure Active Directory Einstellungen** auf **Express**

    ![Azure Portal Authentifizierung Express-option](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    Mit der Option **Express** können App Service automatisch eine Azure AD-Anwendung in Ihrem Azure AD- [Mandanten](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)erstellen. 

    Sie müssen einen Mandanten erstellen, da jede Azure-Konto automatisch hat.

7. Klicken Sie im **Verwaltungsmodus**auf **Neue AD App erstellen** , wenn sie nicht bereits ausgewählt ist, und beachten Sie den Wert im Textfeld **App erstellen** ; Sie sehen diese AAD Anwendung in Azure-Verwaltungsportal später.

    ![Azure-Portal Azure AD-Optionen](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure erstellt automatisch eine Azure AD-Anwendung mit dem angegebenen Namen in Azure AD-Mandanten. Standardmäßig heißt Azure AD-Anwendung die API-app identisch. Alternativ können Sie einen anderen Namen eingeben.
 
7. Klicken Sie auf **OK**.

7. In der **Authentifizierung / Autorisierung** Blade, klicken Sie auf **Speichern**.

    ![Klicken Sie auf Speichern](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Jetzt können nur Benutzer in Ihrem Mandanten Azure AD app API aufrufen.

### <a name="optional-test-the-api-app"></a>Optional: Test der API-app

1. Wechseln Sie im Browser auf die URL der API-app: Blatt **API-app** im Azure-Portal klicken Sie unter **URL**.  

    Sie werden eine Anmeldeseite umgeleitet, da nicht authentifizierte Anfragen dürfen die API-app erreichen.

    Wenn Ihr Browser "erfolgreich erstellt" Seite geht, Browser möglicherweise bereits angemeldet-in diesem Fall öffnen ein Fensters InPrivate oder Inkognito und API-app-URL.

2. Melden Sie sich unter Verwendung der Anmeldeinformationen eines Benutzers in Azure AD-Mandanten.

    Wenn Sie angemeldet sind, wird die "erfolgreich" Seite im Browser angezeigt.

9. Schließen Sie den Browser.

### <a name="configure-the-azure-ad-application"></a>Konfigurieren Sie die Azure AD-Anwendung

Bei der Konfiguration von Azure AD-Authentifizierung erstellt App Service Azure AD-Anwendung. Standardmäßig neue Azure AD wurde Anwendung konfiguriert trägertoken API-app-URL bereitstellen. In diesem Abschnitt Konfigurieren Sie die Anwendung Azure AD trägertoken zum AngularJS front-End anstatt direkt an die Zwischenebene API-Anwendung bereitstellen. AngularJS-front-End sendet das Token API-app beim Aufrufen der API-app.

>[AZURE.NOTE] Zu den Prozess als einfach wie möglich, dieses Lernprogramms verwendet eine einzelne Azure AD-Anwendung für das front-End und die Mitte tier API-app. Eine weitere Option ist mit zwei Azure AD-Applikationen. In diesem Fall müssten Sie front-End Azure AD-Anwendung aufrufen, Azure AD-Anwendung der mittleren Ebene zustimmen. Dieser multifunktionale Ansatz Geben Sie genauere Steuerung der Berechtigungen für jede Ebene.

11. Wechseln Sie in [Azure-Verwaltungsportal](https://manage.windowsazure.com/)zu **Azure Active Directory**.

    Sie müssen das Verwaltungsportal verwenden, da bestimmte Azure Active Directory-Einstellung, die Sie auf noch nicht im aktuellen Azure-Portal verfügbar sind.

12. Klicken Sie auf der Registerkarte **Verzeichnis** AAD-Mandanten.

    ![Azure AD im Verwaltungsportal](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Klicken Sie auf **Applikationen > Applications Mein Unternehmen**, und klicken Sie dann auf das Häkchen.

    Sie müssen die Seite, um die neue Anwendung aktualisieren.

15. Klicken Sie in der Liste Programme auf Azure für Sie erstellt, wenn Sie für Ihre API-Authentifizierung aktiviert.

    ![Azure AD Registerkarte](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Klicken Sie auf **Konfigurieren**.

    ![Registerkarte Azure AD konfigurieren](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. Die URL für Ihre AngularJS Web app keine nachgestellten Schrägstrich **Anmelden URL** soll.

    Beispiel: https://todolistangular.azurewebsites.net

    ![URL anmelden](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. Legen Sie **Antwort-URL** den URL für Ihre Webanwendung ohne nachstehenden Schrägstrich.

    Beispiel: https://todolistsangular.azurewebsites.net

16. Klicken Sie auf **Speichern**.

    ![Antwort-URL](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. Klicken Sie am unteren Rand der Seite auf **Verwalten Manifest > Download Manifest**.

    ![Manifest herunterladen](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Downloaden Sie die Datei an, in dem Sie sie bearbeiten können.

16. Suchen in der heruntergeladenen manifest-Datei der `oauth2AllowImplicitFlow` Eigenschaft. Ändern Sie den Wert dieser Eigenschaft von `false` , `true`, und speichern Sie die Datei.

    Diese Einstellung ist für den Zugriff aus einer einseitigen JavaScript-Anwendung erforderlich. Das Token Oauth 2.0 Inhaber in das URL-Fragment zurückgegeben werden können.

16. Klicken Sie auf **Verwalten Manifest > Upload Manifest**, und Laden Sie die Datei, die Sie im vorherigen Schritt aktualisiert.

    ![Manifest hochladen](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Kopieren Sie den **Client-ID** -Wert und speichern Sie einem können Sie es später.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Konfigurieren Sie das Projekt ToDoListAngular Authentifizierung

In diesem Abschnitt ändern Sie AngularJS-front-End, dass Active Directory Authentifizierung Library (ADAL) für JS zu produzierende Token für angemeldete Benutzer von Azure AD verwendet. Code wird in HTTP-Anfragen an die mittlere Schicht Token enthalten, wie im folgenden Diagramm dargestellt. 

![Benutzer Authentifizierung Diagramm](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Ändern der folgenden Dateien im ToDoListAngular-Projekt.

1. Öffnen Sie die Datei *index.html* .

2. Kommentieren Sie die Zeilen, die das Active Directory Authentifizierung Library (ADAL) für JS-Skripts verweisen.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Öffnen Sie die Datei *app/scripts/app.js* .

2. Kommentieren Sie den Codeblock für "ohne Authentifizierung" markiert, und kommentieren Sie den Codeblock für "mit Authentifizierung" markiert.

    Diese Änderung verweist auf ADAL JS Authentifizierungsanbieter und bietet Werte zu. In den folgenden Schritten legen Sie die Konfigurationswerte für Ihre API-app und Azure AD-Anwendung.

8. Im Code wird die `endpoints` , gesetzt den API-URL auf die URL der API-app für das ToDoListAPI-Projekt erstellt und die Client-ID, die von der Azure-Verwaltungsportal kopiert die Azure AD-ID gesetzt.

    Der Code ist nun wie im folgenden Beispiel.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. Aufruf von `adalProvider.init`, legen `tenant` , der Name des Mieters und `clientId` , im vorherigen Schritt verwendeten, Wert übereinstimmen.

    Der Code ist nun wie im folgenden Beispiel.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Diese Änderungen `app.js` angeben, dass der aufrufende Code und die aufgerufene API in derselben Azure AD-Anwendung.

1. Öffnen Sie die Datei *app/scripts/homeCtrl.js* .

2. Kommentieren Sie den Codeblock für "ohne Authentifizierung" markiert, und kommentieren Sie den Codeblock für "mit Authentifizierung" markiert.

1. Öffnen Sie die Datei *app/scripts/indexCtrl.js* .

2. Kommentieren Sie den Codeblock für "ohne Authentifizierung" markiert, und kommentieren Sie den Codeblock für "mit Authentifizierung" markiert.

### <a name="deploy-the-todolistangular-project-to-azure"></a>ToDoListAngular in Azure bereitstellen

8. Im **Projektmappen-Explorer**mit der rechten Maustaste des ToDoListAngular Projekts und dann auf **Veröffentlichen**.

9. Klicken Sie auf **Veröffentlichen**.

    Visual Studio stellt das Projekt und öffnet einen Browser Web app-Basis-URL. Dies zeigt eine Fehlerseite 403 normal Versuch zu einem Web API-Basis-URL in einem Browser.

    Sie haben noch ändern Zwischenebene API-app vor dem Testen der Anwendung.

10. Schließen Sie den Browser.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Konfigurieren Sie das Projekt ToDoListAPI Authentifizierung

ToDoListAPI Projekt derzeit sendet "*" als das `owner` zu ToDoListDataAPI Wert. In diesem Abschnitt ändern Sie den Code des angemeldeten Benutzers gesendet.

Änderungen Sie die folgenden im Projekt ToDoListAPI.

1. Öffnen Sie die Datei *Controllers/ToDoListController.cs* , und kommentieren Sie die Zeile in jede Aktionsmethode legt `owner` Azure AD `NameIdentifier` Anspruchswert. Zum Beispiel:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Wichtig**: nicht kommentieren Sie Code in der `ToDoListDataAPI` -Methode. haben Sie Gelegenheit, die später Service principal Authentifizierung Tutorial.

### <a name="deploy-the-todolistapi-project-to-azure"></a>ToDoListAPI in Azure bereitstellen

8. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts ToDoListAPI und klicken Sie auf **Veröffentlichen**.

9. Klicken Sie auf **Veröffentlichen**.

    Visual Studio stellt das Projekt und öffnet einen Browser auf Basis der API-app-URL.

10. Schließen Sie den Browser.

### <a name="test-the-application"></a>Testen der Anwendung

9. Gehen Sie auf die URL des Web app **Verwenden Sie HTTPS und nicht HTTP**.

8. Klicken Sie auf **Liste** .

    Sie werden aufgefordert, anmelden.

9. Melden Sie sich mit den Anmeldeinformationen eines Benutzers in Ihrem Mandanten AAD.

10. Die **Liste** wird angezeigt.

    ![Seite Aufgabenliste](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Keine Aufgaben werden angezeigt, da sie alle Besitzer bisher "*". Nun fordert der mittleren Ebene Elemente für den angemeldeten Benutzer und keine noch erstellt wurden.

11. Fügen Sie neue Aufgaben, um sicherzustellen, dass die Anwendung funktioniert.

12. In einem anderen Browserfenster Swagger UI URL für die ToDoListDataAPI-API-Anwendung und klicken Sie auf **Aufgabenliste > erhalten**. Geben Sie ein Sternchen für die `owner` Parameter, und klicken Sie dann auf **ausprobieren**.

    Die Antwort zeigt, dass die neuen Aufgaben der Azure AD-Benutzer-ID in der Eigenschaft Besitzer.

    ![Besitzer-ID in JSON-Antwort](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Erstellen von Projekten neu

Zwei Web API Projekte wurden unter Verwendung der Projektvorlage **Azure API-App** und ersetzt den Standard-Domänencontroller-Werte mit einer Aufgabenliste erstellt. 

Weitere Informationen zum Erstellen einer einseitigen Anwendung AngularJS mit Web API 2 Back-End [Hände auf Lab: Erstellen Sie eine einzelne Seite Anwendung (SPA) Angular.js mit ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informationen zum Azure AD Authentifizierungscode hinzufügen finden Sie in folgenden Ressourcen:

* [AngularJS einzelne Seite Apps mit Azure sichern](../active-directory/active-directory-devquickstarts-angular.md).
* [Einführung ADAL JS v1](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Problembehandlung

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Stellen Sie sicher, dass ToDoListAPI (mittlere Stufe) und ToDoListDataAPI (Datenebene) verwechseln. Beispielsweise überprüfen Sie, ob die Zwischenebene API-Anwendung nicht die Datenebene Authentifizierung hinzugefügt. 
* Sicherstellen, dass AngularJS Quellcode Zwischenebene API-app-URL (ToDoListAPI, nicht ToDoListDataAPI) und die richtige Azure verweist auf AD Client-ID. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm haben Sie wie App-Authentifizierung für eine API-app verwendet und wie die API-app mit ADAL JS-Bibliothek. In der nächsten Übung erfahren wie Sie [sicheren Zugriff auf die API-app für Dienst - Szenarien](app-service-api-dotnet-service-principal-auth.md).

