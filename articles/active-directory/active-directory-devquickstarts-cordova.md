<properties
    pageTitle="Azure AD Cordova erste Schritte | Microsoft Azure"
    description="Wie eine Cordova Anwendung, die Integration mit Azure anmelden und ruft Azure AD geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter=""
    authors="vibronet"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="vittorib"/>

# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integrieren eines Apache Azure AD Cordova-app

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Apache Cordova ermöglicht Ihnen HTML5 und JavaScript-Anwendungsentwicklung die Mobilgeräte als vollwertige systemeigene Anwendung ausführen können.
Azure AD können Sie Enterprise Grade Authentifizierungsfunktionen Cordova Anwendung hinzufügen. Dank Cordova Plug-in umbrochen Azure AD systemeigene SDKs iOS, Android, Windows Store und Windows Phone können Sie Ihre Anwendung, Unterstützung Zeichen auf Ihre Benutzer AD-Konten Zugriff auf Office 365 und Azure-API, und sogar eigene benutzerdefinierte Web-API-Aufrufe erhöhen.

In diesem Tutorial verwenden wir Apache Cordova Plugin für Active Directory Authentifizierung Library (ADAL) zu einer einfachen Anwendung mit folgenden Funktionen:

-   Mit wenigen Codezeilen einen AD-Benutzer zu authentifizieren und ein Token für den Aufruf von Azure AD Graph-API abrufen.
-   Verwendung des Tokens die Graph-API, um das Verzeichnis Abfragen und die Ergebnisse aufrufen  
-   Nutzung des ADAL token Caches minimieren Authentifizierungsanfragen für den Benutzer.

Dazu benötigen Sie:

2. Registrieren einer Anwendung in Azure AD
2. Fügen Sie Code für Ihre Anwendung Tokens anfordern
3. Fügen Sie Code hinzu, um das Token für die Abfrage der Graph-API verwenden und anzuzeigen Sie Ergebnisse.
4. Das Weitergabeprojekt Cordova mit allen Plattformen abzielen möchten und Cordova ADAL-Plug-in erstellen und Testen der Lösung in Emulatoren.

## <a name="0--prerequisites"></a>*0. erforderliche Komponenten*

Zum Bearbeiten dieses Lernprogramms benötigen Sie:

- Ein Azure AD-Mandanten, in dem Sie ein Konto mit app-Entwicklung haben
- Einer Umgebung Apache Cordova konfiguriert  

Wenn Sie beide bereits eingerichtet, fahren Sie direkt mit Schritt 1.

Wenn Sie Azure AD-Mandanten haben, finden Sie [wie man hier](active-directory-howto-tenant.md).

Wenn Sie Apache Cordova auf Ihrem Computer eingerichtet haben, installieren Sie Folgendes:

- [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [NodeJS](https://nodejs.org/download/)
- [Cordova CLI](https://cordova.apache.org/) (NPM-Paket-Manager kann einfach installiert werden: `npm install -g cordova`)

Beachten Sie, dass die auf dem PC und Mac sollte

Jede Zielplattform hat verschiedene Komponenten.

- Erstellen und Ausführen von Windows Tablet-PC oder Telefon app version
    - [Visual Studio 2013 für Windows Update 2 oder höher](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express oder einer anderen Version).
- Erstellen und führen für iOS
    -   Xcode 5.x oder höher. Http://developer.apple.com/downloads oder [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12) herunterladen
    -   [Ios-Sim](https://www.npmjs.org/package/ios-sim) – können Sie iOS-apps in iOS Simulator von der Befehlszeile aus starten (problemlos über die Terminaldienste installiert werden: `npm install -g ios-sim`)

- Erstellen und Ausführen der Anwendung für Android
    - Installieren Sie [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher. Stellen Sie sicher, dass `JAVA_HOME` (Umgebungsvariable) wird nach JDK-Installationspfad (z.B. C:\Program Files\Java\jdk1.7.0_75) korrekt eingestellt.
    - [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) installieren und hinzufügen `<android-sdk-location>\tools` Speicherort (z. B. C:\tools\Android\android-sdk\tools), die `PATH` Umgebungsvariablen.
    - Android SDK Manager zu öffnen (z. B. über Terminal: `android`) und installieren
    - Plattform-SDK für *Android 5.0.1 (API-21)*
    - *Android SDK Buildtools* Version 19.1.0 oder höher
    - *Android unterstützt Repository* (Extras)

  Standardinstanz Emulator zur Android Sdk nicht Verfügung. Erstellen eines neuen mit `android avd` von Terminalserver auswählen und dann auf *Create* Android Emulator ausgeführt werden soll. Empfohlene *API-Ebene* ist 19 oder höher, AVD Manager (http://developer.android.com/tools/help/avd-manager.html) für Weitere Informationen über Android-Emulator und erstellen.


## <a name="1--register-an-application-with-azure-ad"></a>*1. Registrieren einer Anwendung in Azure AD*

Hinweis: dieser __Schritt ist optional__. Das Lernprogramm bereitgestellt vorab bereitgestellte Werte, mit denen Sie das Beispiel in Aktion sehen, ohne jede Bereitstellung in Ihrem eigenen Mandanten. Jedoch wird empfohlen, dass Sie diesen Schritt ausführen und mit dem Prozess vertraut werden, wie es erforderlich sein, wenn Ihre eigene Anwendung erstellen.

Azure AD werden nur bekannte Anwendung Token ausstellen. Vor der Verwendung von Azure AD aus Ihrer Anwendung müssen Sie einen Eintrag in Ihrem Mandanten erstellen.  Registriert eine neue Anwendung in Ihrem Mandanten

-   Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)
-   Klicken Sie im linken Navigationsbereich auf **Active Directory**
-   Wählen Sie Mieter wo Sie registrieren möchten.
-   Klicken Sie auf die Registerkarte **Applications** , und klicken Sie auf **Hinzufügen** in der unteren Schublade.
-   Befolgen, und erstellen Sie eine neue **Systemeigene Clientanwendung** (trotz der Tatsache, dass Cordova apps HTML-basierte, schaffen wir hier systemeigene Clientanwendung so `Native Client Application` Option muss ausgewählt werden, anderenfalls die Anwendung funktioniert nicht).
    -   Der **Name** der Anwendung wird die Anwendung an Endbenutzer beschrieben.
    -   **Umleitung URI** ist der URI Token an Ihre Anwendung zurückgegeben. Geben Sie `http://MyDirectorySearcherApp`.

Nach Abschluss Anmeldung zuweisen AAD app eine eindeutige Client-ID.  Sie benötigen diesen Wert in den nächsten Abschnitten: in das Register **Konfigurieren** der neu erstellten Anwendung finden.

Ausführen `DirSearchClient Sample`, die neu erstellte Anwendung Berechtigung _Azure AD Graph-API_abgefragt:
-   Suchen Sie im Abschnitt "Berechtigungen to Other Applications" in Registerkarte **Konfigurieren** .  Fügen Sie für die Anwendung "Azure Active Directory" die Berechtigung **das Verzeichnis der angemeldeten Benutzer** **Delegiert**Berechtigungen hinzu  Dadurch kann die Anwendung die Graph-API für Benutzer Abfragen.

## <a name="2-clone-the-sample-app-repository-required-for-the-tutorial"></a>*2. Klonen Sie Beispiel app Repository erforderlich für das Lernprogramm*

Geben Sie den folgenden Befehl aus der Shell oder Befehlszeile:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="3-create-the-cordova-app"></a>*3. Cordova-Anwendung erstellen*

Es gibt unterschiedliche Cordova Anwendung erstellen. In diesem Lernprogramm wird die Cordova-Befehlszeilenschnittstelle (CLI) verwendet.
Geben Sie den folgenden Befehl aus der Shell oder Befehlszeile:


     cordova create DirSearchClient --copy-from="NativeClient-MultiTarget-Cordova/DirSearchClient"

Die Ordnerstruktur und Gerüstbau für das Cordova-Projekt kopieren den Inhalt des Startprojekt im Www Unterordner erstellt.
Verschieben Sie in den neuen Ordner DirSearchClient.

    cd .\DirSearchClient

Hinzufügen des weißen-Plugins für die Graph-API aufrufen.

     cordova plugin add cordova-plugin-whitelist

Fügen Sie alle Plattformen zu unterstützen. Um ein funktionierendes Beispiel, müssen Sie mindestens die folgenden Befehle ausführen. Beachten Sie, dass Sie nicht unter Windows oder Windows-Windows Phone Mac iOS emulieren

    cordova platform add android
    cordova platform add ios
    cordova platform add windows

Schließlich können Sie die ADAL für Cordova Plugin zum Projekt hinzufügen.

    cordova plugin add cordova-plugin-ms-adal

## <a name="3-add-code-to-authenticate-users-and-obtain-tokens-from-aad"></a>*3. Fügen Sie Code zum Authentifizieren von Benutzern und Token aus AAD*

Die Anwendung, die Sie in diesem Lernprogramm entwickeln bietet eine Suchfunktion Barebone-Verzeichnis, in dem der Benutzer geben Sie den Alias eines Benutzers im Verzeichnis und einige grundlegende Attribute visualisieren können.  Das Startprojekt enthält die Definition die grundlegende Benutzeroberfläche der Anwendung (in www/index.html) und die grundlegenden app Ereignis Drähte Gerüstbau Zyklen User Interface Bindings und Ergebnisse Anzeigelogik (www/js/index.js). Links für Sie lediglich die Logik implementieren Identität Aufgaben hinzufügen.

Die ersten müssen Sie ist im Code die Protokollwerte vor, die von AAD Ihrer app und die Ressourcen, die Sie als Ziel verwendet werden. Diese Werte dienen token Anfragen später erstellen. Einfügen des Ausschnitts unten oben in der Datei index.js.

```javascript
    var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Die `redirectUri` und `clientId` Werte sollten Ihre app AAD beschreiben Werte übereinstimmen. Die Registerkarte Konfigurieren finden im Azure-Portal Sie unter Schritt 1 weiter oben in diesem Lernprogramm.
Hinweis: Wenn Sie sich für eine neue Anwendung in Ihrem eigenen Mandanten nicht registriert, können einfach einfügen die vordefinierten Werte oben ist - können Sie die Ausführung Beispiel finden Sie unter für Ihre apps für Produktion sollte immer einen eigenen Eintrag erstellen sollten.

Als Nächstes müssen wir den tatsächliche token Anforderungscode hinzufügen. Fügen Sie den folgenden Codeausschnitt zwischen dem `search `und `renderdata `Definitionen.

```javascript
    // Shows user authentication dialog if required.
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Betrachten wir diese Funktion durch die Zerlegung in zwei Hauptteilen.
In diesem Beispiel soll alle Mandanten arbeiten und nicht um eine verbunden werden. Es verwendet den Endpunkt "/ common", der den Benutzer ein Konto zum Zeitpunkt der Authentifizierung eingeben, und leitet die Anforderung an den Mieter gehört.
Im ersten Teil der Methode überprüft ADAL Cache ist bereits gespeicherte Token - und wird verwendet, wenn Mieter kam es aus für die erneute Initialisierung ADAL. Ist zusätzliche Abfragen vermeiden wie bei Verwendung von "/ common" immer den Benutzer auffordert wird, geben Sie ein neues Konto
```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Der zweite Teil der Methode führt die richtigen Tokewn-Anforderung.
Die `acquireTokenSilentAsync` -Methode fragt ADAL, ein Token für die angegebene Ressource zurück, ohne alle UX. Das kann passieren, wenn Cache ein geeignetes Zugriffstoken gespeichert bereits, oder ein Aktualisierungstoken, mit der einem Zugriffstoken ohne Shwoing Aufforderung angezeigt.
Wenn dieser Versuch fehlschlägt, wir zurückgreifen auf `acquireTokenAsync` -die sichtbar fordert den Benutzer zu authentifizieren.
```javascript
            // Attempt to authorize user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials so triggers authentication dialog
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Jetzt haben wir das Token wir schließlich die Graph-API aufrufen und Ausführen die gewünschten Abfrage. Die folgende Ausschnitt einfügen unter der `authenticate` Definition.

```javascript
// Makes Api call to receive user list.
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Start Point-Dateien geliefert Barebone UX Alias eines Benutzers in ein Textfeld eingeben. Diese Methode verwendet den Wert Abfrage erstellen, mit dem Zugriffstoken kombinieren, dem Diagramm senden und Analysieren der Ergebnisse. RenderData-Methode bereits im ersten Punktdatei übernimmt die Ergebnisse visuell darstellen.

## <a name="4-run"></a>*4. ausführen*
Ihre Anwendung wird schließlich ausgeführt! Es ist sehr einfach: nach dem Starten der Anwendung geben Sie im Textfeld den Alias des Benutzers zu suchen - klicken. Sie werden zur Authentifizierung aufgefordert. Nach erfolgreicher Authentifizierung und die Suche erfolgreich werden die Attribute des gesuchten Benutzers angezeigt. Nachfolgende Testläufe führt die Suche ohne jede Aufforderung Dank im Cache des zuvor erworbenen Tokens.
Die konkreten Schritte zum Ausführen der Anwendung variieren je nach Plattform.

####<a name="windows-10"></a>Windows 10:

   Stift:`cordova run windows --archs=x64 -- --appx=uap`

   Mobile (Windows10 Mobile Geräte PC erforderlich):`cordova run windows --archs=arm -- --appx=uap --phone`

   __Hinweis__: während der ersten Ausführung Sie möglicherweise eine Entwicklerlizenz Anmelden aufgefordert. Weitere Informationen finden Sie unter [Entwicklerlizenz](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-81-tabletpc"></a>Windows 8.1 Tablet-PC:

   `cordova run windows`

   __Hinweis__: während der ersten Ausführung Sie möglicherweise eine Entwicklerlizenz Anmelden aufgefordert. Weitere Informationen finden Sie unter [Entwicklerlizenz](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx) .

####<a name="windows-phone-81"></a>Windows Phone 8.1:

   Auf Gerät ausgeführt:`cordova run windows --device -- --phone`

   Standard-Emulator ausführen:`cordova emulate windows -- --phone`

   Mit `cordova run windows --list -- --phone` an alle verfügbaren Ziele und `cordova run windows --target=<target_name> -- --phone` Anwendung auf dem Gerät oder Emulator auszuführen (z. B. `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

####<a name="android"></a>Android:

   Auf Gerät ausgeführt:`cordova run android --device`

   Standard-Emulator ausführen:`cordova emulate android`

   __Hinweis__: Achten erstellte Emulatorinstanz *AVD-Manager* verwenden, wie es im Abschnitt *erforderliche* ist.

   Mit `cordova run android --list` an alle verfügbaren Ziele und `cordova run android --target=<target_name>` Anwendung auf dem Gerät oder Emulator auszuführen (z. B. `cordova run android --target="Nexus4_emulator"`).

####<a name="ios"></a>iOS:

   Auf Gerät ausgeführt:`cordova run ios --device`

   Standard-Emulator ausführen:`cordova emulate ios`

   __Hinweis__: Stellen Sie sicher, dass `ios-sim` auf Emulator installiert. *Komponenten* Siehe für Weitere Informationen.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run application on specific device or emulator (for example,  `cordova run android --target="iPhone-6"`).

Mit `cordova run --help` zu zusätzliche Optionen ausführen.

Zu Referenzzwecken vollständiges Beispiel (ohne die Konfigurationswerte) bereitgestellt [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).  Sie können für erweiterte Szenarien (ok und interessanter) nun.  Möglicherweise möchten versuchen:

[Sichere Node.js Web API Azure AD >>](active-directory-devquickstarts-webapi-nodejs.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
