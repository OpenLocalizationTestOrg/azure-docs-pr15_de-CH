<properties
    pageTitle="Azure AD Android Einstieg | Microsoft Azure"
    description="Wie Android-Anwendung, die in Azure AD Anmelden integriert werden und ruft Azure AD geschützten APIs mit OAuth."
    services="active-directory"
    documentationCenter="android"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-android-app"></a>Android App integrieren Sie Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Wenn Sie eine desktop-Anwendung entwickeln, vereinfacht Azure AD und für Ihre Benutzer authentifizieren anhand ihrer Active Directory-Benutzerkonten.  Außerdem können die Anwendung sicher alle Web API geschützt von Azure AD Azure-API oder die Office 365-APIs verwenden.

Für Android-Clients, die Zugriff auf geschützte Ressourcen bietet Azure AD Active Directory Authentifizierungsbibliothek oder ADAL.  Zweck des ADAL im Leben ist Ihre app Zugriffstoken zu erleichtern.  Um zu veranschaulichen, wie einfach es ist, bauen hier einer Aufgabenliste Android-Anwendung, die wir:

-   Erhält Zugriff auf Token für eine Aufgabe Liste-API mit dem [Authentifizierungsprotokoll OAuth 2.0](https://msdn.microsoft.com/library/azure/dn645545.aspx)aufrufen.
-   Ruft eines Benutzers-Aufgabenliste
-   Benutzer Zeichen.

Zunächst benötigen Sie einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Registrieren einer Anwendung.  Haben Sie bereits [erfahren Sie, wie man](active-directory-howto-tenant.md)Mieter.

> [AZURE.TIP] Testen Sie die Vorschau unsere neue [Entwicklerportal](https://identity.microsoft.com/Docs/Android) , mit denen Sie die laufenden Azure Active Directory in wenigen Minuten abrufen und!  Entwicklerportal führt Sie durch den Prozess der Registrierung einer app und Azure AD in Code.  Wenn Sie fertig sind, haben Sie eine einfache Anwendung, die in Ihrem Mandanten und Backend-Benutzer authentifizieren, die Tokens akzeptieren und Validierung. 

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Schritt 1: Downloaden und ausgeführt Node.js REST API TODO-Beispielserver

In diesem Beispiel steht speziell für unsere vorhandene Beispiel zum Erstellen eines einzelnen Mandanten Aufgabe REST API für Microsoft Azure Active Directory verwenden. Dies ist eine Voraussetzung für den Schnellstart.

Informationen dazu finden Sie auf unseren vorhandenen Beispielen:

* [Microsoft Azure Active Directory REST API Beispieldienst für Node.js](active-directory-devquickstarts-webapi-nodejs.md)

## <a name="step-2-register-your-web-api-with-your-microsoft-azure-ad-tenant"></a>Schritt 2: Registrieren des Webs-API mit der Microsoft Azure AD-Mandanten

**Was mache ich?**

*Microsoft Active Directory unterstützt zwei Arten von Clientanwendungen. Web-APIs, die Dienstleistungen für Anwender und Applications (entweder im Web oder in einer Anwendung auf einem Gerät), die auf diesen Web-APIs zugreifen. In diesem Schritt werden die Web-API Anmeldung lokal zum Testen dieses Beispiels ausführen. Normalerweise wäre dieses Web API REST-Dienst, der Funktionalität bietet eine Anwendung zugreifen möchten. Microsoft Azure Active Directory kann jeder Endpunkt schützen!*

*Hier wird davon ausgegangen, TODO REST API erwähnten registriert, aber dies funktioniert für alle Web API Azure Active Directory schützen möchten.*

Schritte zum Registrieren einer Web-API mit Microsoft Azure AD

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie auf Active Directory links NAV
3. Klicken Sie auf Verzeichnis Mieter, Beispiel registrieren möchten.
4. Klicken Sie auf die Registerkarte Applications.
5. Klicken Sie auf "hinzufügen" in der Schublade.
6. Klicken Sie auf "Von meinem Unternehmen entwickelte Anwendung hinzufügen".
7. Geben Sie einen Anzeigenamen für die Anwendung, z. B. "TodoListService", wählen Sie "Web Application oder Web API", und klicken Sie auf Weiter.
8. URL anmelden, geben Sie den Basis-URL für das Beispiel standardmäßig ist `https://localhost:8080`.
9. URI-ID App Geben Sie `https://<your_tenant_name>/TodoListService`, ersetzt `<your_tenant_name>` mit dem Namen der Azure AD-Mandanten.  Klicken Sie auf OK, um die Registrierung abzuschließen.
10. Klicken Sie auf die Registerkarte Konfigurieren der Anwendung noch in Azure-Portal.
11. **Den Client-ID-Wert suchen und kopieren Sie sie beiseite**, Sie diese später benötigen die Anwendung konfigurieren.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Schritt 3: Registrieren Sie Beispiel Android Native Client-Anwendung

Registrieren Ihre Web-Anwendung ist der erste Schritt. Anschließend müssen Sie die Anwendung sowie Active Directory Azure erzählen. Dadurch wird die Anwendung nur registrierte Web API kommunizieren

**Was mache ich?**  

*Wie bereits erwähnt, unterstützt Microsoft Azure Active Directory Hinzufügen von zwei Anwendungsarten. Web-APIs, die Dienstleistungen für Anwender und Applications (entweder im Web oder in einer Anwendung auf einem Gerät), die auf diesen Web-APIs zugreifen. In diesem Schritt werden Sie die Anwendung in diesem Beispiel registrieren. Sie müssen diese Anwendung können die Anforderung auf die Web-API nur registriert. Azure Active Directory lässt sich selbst kann eine Anwendung anmelden beantragen, wenn er registriert ist. Das ist der Teil des Modells.*

*Hier gehen wir diese obenstehende Anwendung registriert, aber dies gilt für jede Anwendung, die Sie entwickeln.*

**Warum wird eine Anwendung und eine Web-API in einem Mandanten werden setzen?**

*Wie Sie schon erraten haben vielleicht, könnten Sie eine Anwendung erstellen, die externe API zugreift, die von einem anderen Mandanten in Azure Active Directory registriert ist. Wenn Sie dies tun, werden Kunden aufgefordert, die Verwendung der API in der Anwendung. Das Schöne daran ist Active Directory Authentifizierungsbibliothek für iOS übernimmt diese Zustimmung für Sie! Wie wir im erweiterten Funktionen erhalten, sehen Sie ist ein wichtiger Bestandteil der Arbeit Azure Office sowie anderen Service Provider Suite von Microsoft-APIs zugreifen. Jetzt da der Web-API und die Anwendung unter demselben Mandanten registriert keine Aufforderung zur Bestätigung angezeigt. Dies gilt normalerweise Wenn Sie eine Anwendung nur für Ihr eigenes Unternehmen entwickeln verwenden.*

1. Melden Sie sich bei [Azure-Verwaltungsportal](https://manage.windowsazure.com)an.
2. Klicken Sie auf Active Directory links NAV
3. Klicken Sie auf Verzeichnis Mieter, Beispiel registrieren möchten.
4. Klicken Sie auf die Registerkarte Applications.
5. Klicken Sie auf "hinzufügen" in der Schublade.
6. Klicken Sie auf "Von meinem Unternehmen entwickelte Anwendung hinzufügen".
7. Geben Sie einen Anzeigenamen für die Anwendung, beispielsweise "TodoListClient-Android", wählen Sie "Native Client", und klicken Sie auf Weiter.
8. Geben Sie den URI umleiten `http://TodoListClient`.  Klicken Sie auf Fertig stellen.
9. Klicken Sie auf die Registerkarte Konfigurieren der Anwendung.
10. Den Client-ID-Wert suchen und kopieren, Sie benötigen diese später beim Konfigurieren Ihrer Anwendung.
11. Klicken Sie in "Berechtigungen to Other Applications" auf "Anwendung hinzufügen".  Wählen Sie "Andere" in der Dropdownliste "Anzeigen", und klicken Sie oben.  Klicken Sie auf die TodoListService, und klicken Sie auf das Häkchen unten die Anwendung hinzufügen.  Wählen Sie "Zugriff TodoListService" aus "Delegiert Berechtigungen", und speichern Sie die Konfiguration.



Um Maven zu erstellen, können die pom.xml auf oberster Ebene Sie

  * Klonen Sie dieses Repo in ein Verzeichnis Ihrer Wahl:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  

  * Gehen Sie mit [Prerequests die Maven für Android einrichten](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android)
  * Setup-Emulator SDK 19
  * Wechseln Sie in das Stammverzeichnis, in dem das Repo geklont
  * Führen Sie den Befehl: Säubern Mvn installieren
  * Wechseln Sie im Schnellstart-Beispiel: cd Samples\hello
  * Führen Sie den Befehl: Mvn android: Bereitstellen von Android ausführen:
  * Sie sollten sehen app starten
  * Geben Sie die Anmeldeinformationen des Testbenutzers versuchen!

JAR-Paketen werden auch neben Tätigkeitsbericht Paket gesendet.

### <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Schritt 4: Android ADAL downloaden und Eclipse Arbeitsbereich hinzufügen

Wir haben es leicht mehrere Optionen dieser Bibliothek im Android Projekt vorgenommen:

* Den Quellcode können Sie dieser Bibliothek in Eclipse und Verknüpfung der Anwendung importieren.
* Wenn Android Studio, können *Tätigkeitsbericht* Paketformat verwenden und Binärdateien verweisen.

####<a name="option-1-source-zip"></a>Option 1: Quelle Zip

Eine Kopie des Quellcodes, klicken Sie auf der rechten Seite der Seite "Download ZIP" oder klicken Sie [hier](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

####<a name="option-2-source-via-git"></a>Option 2: Quelle über Git

Zu der Quellcode des SDK über Git einfach Folgendes ein:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

####<a name="option-3-binaries-via-gradle"></a>Option 3: Binärdateien über Gradle

Sie erhalten die Binärdateien von Maven central Repo. Tätigkeitsbericht Paket kann wie folgt in Ihrem Projekt AndroidStudio enthalten:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

####<a name="option-4-aar-via-maven"></a>Option 4: Tätigkeitsbericht über Maven

Bei Verwendung von m2e-Plugin in Eclipse können Sie die Abhängigkeit in der Datei pom.xml angeben:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


####<a name="option-5-jar-package-inside-libs-folder"></a>Option 5: JAR-Paket Bibliotheken Ordner
Sie können die JAR-Datei von Maven Repo und *Bibliotheken* Ordner ablegen, in Ihrem Projekt. Sie müssen die erforderlichen Ressourcen zum Projekt sowie kopieren, da JAR-Pakete enthalten nicht.


### <a name="step-5-add-references-to-android-adal-to-your-project"></a>Schritt 5: Dem Projekt Verweise auf Android ADAL hinzufügen


2. Fügen Sie einen Verweis auf das Projekt und Android Bibliothek angeben. Wenn Sie unsicher sind, wie Sie dazu [Weitere Informationen hier klicken] (http://developer.android.com/tools/projects/projects-eclipse.html)

3. Fügen Sie projektabhängigkeit zum Debuggen im projekteinstellungen hinzu

4. Aktualisieren des Projekts AndroidManifest.xml Datei enthalten:

    ```Java
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
      ....
      <application/>
    ```

7. Erstellen Sie eine Instanz des AuthenticationContext an die wichtigsten. Die Details des Aufrufs sind nicht Gegenstand dieser Readme-Datei erhalten Sie einen guten Start [Android Native Beispiel](https://github.com/AzureADSamples/NativeClient-Android)betrachten Es folgt ein Beispiel:

    ```Java
    // Authority is in the form of https://login.windows.net/yourtenant.onmicrosoft.com
    mContext = new AuthenticationContext(MainActivity.this, authority, true); // This will use SharedPreferences as            default cache
    ```
  * Hinweis: mContext ist ein Feld in der Aktivität

8. Kopieren Sie diesen Codeblock Ende AuthenticationActivity behandeln, nachdem Benutzer Anmeldeinformationen eingibt und Autorisierungscode erhält:

    ```Java
     @Override
     protected void onActivityResult(int requestCode, int resultCode, Intent data) {
         super.onActivityResult(requestCode, resultCode, data);
         if (mContext != null) {
             mContext.onActivityResult(requestCode, resultCode, data);
         }
     }
    ```

9. Um ein Token anfordern, definieren Sie einen Rückruf

    ```Java
    private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };
    ```
10. Überzeugen Sie sich für ein Token mit diesem Rückruf:

    ```Java
     mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                    callback);
    ```

Erläuterung der Parameter:

  * Ressource ist erforderlich und Ressourcen Sie zugreifen wollen.
  * ClientID erforderlich und aus dem AzureAD Portal.
  * Sie können RedirectUri wie der Paketname einrichten. Es ist nicht erforderlich für den AcquireToken-Aufruf angegeben werden.
  * PromptBehavior kann Anmeldeinformationen Cache und Cookies überspringen erfragen.
  * Rückruf wird aufgerufen, nachdem Autorisierungscode für ein Token ausgetauscht werden.

  Der Rückruf wird ein Objekt die Zugriffstoken AuthenticationResult, Datum abgelaufen und Idtoken.

Optional: **acquireTokenSilent**

Rufen Sie **AcquireTokenSilent** behandeln Zwischenspeichern und token aktualisieren. Es bietet auch Synchronisierungsversion. Benutzer-ID akzeptiert als Parameter.

    ```java
     mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

11. **Broker**: Windows Intune Unternehmensportal-app bietet die Broker-Komponente. ADAL mit erstellten Broker-Konto ist ein Benutzerkonto Authentifikator und Entwickler wollen nicht überspringen. Entwickler kann den Broker Benutzer überspringen:

    ```java
     AuthenticationSettings.Instance.setSkipBroker(true);
    ```

 Entwickler spezielle RedirectUri für Broker Verwendung registrieren. RedirectUri ist im Format msauth://packagename/Base64UrlencodedSignature. Sie können Ihre Redirecturi für Ihre Anwendung mithilfe des Skripts "brokerRedirectPrint.ps1" Abrufen oder API-Aufruf mContext.getBrokerRedirectUri. Signatur bezieht sich auf die Signaturzertifikate.

 Aktuelle Broker-Modell ist für einen Benutzer. AuthenticationContext enthält die API-Methode, um den Broker-Benutzer.

 ```java
 String brokerAccount =  mContext.getBrokerUser();
 ```
 Broker-Benutzer wird zurückgegeben, wenn Konto gültig ist.

 App-Manifest AccountManager Benutzerkonten Berechtigungen besitzen: http://developer.android.com/reference/android/accounts/AccountManager.html

 * GET_ACCOUNTS
 * USE_CREDENTIALS
 * MANAGE_ACCOUNTS


In dieser exemplarischen Vorgehensweise verwenden, haben Sie erfolgreich in Azure Active Directory integriert werden sollen. Weitere Beispiele für diese Funktion finden Sie auf der AzureADSamples / GitHub-Repository.

## <a name="important-information"></a>Wichtige Informationen

### <a name="customization"></a>Anpassung

Bibliotheksressourcen können durch der Anwendungsressourcen überschrieben werden. Dies geschieht, wenn Ihre Anwendung erstellen. Aus diesem Grund können Sie Authentifizierung Aktivität Layout wie anpassen. Sie müssen sicherstellen, dass die Id der Steuerelemente, ADAL uses(Webview) beibehalten.

### <a name="broker"></a>Broker

Broker-Komponente wird mit Windows Intune Unternehmensportal-app geliefert. Konto wird im Konto-Manager erstellt werden. Kontotyp ist "com.microsoft.workaccount". Es kann nur einzelne SSO-Dienstkonto. Nach Abschluss der Gerät fordert eine apps wird SSO-Cookie für diesen Benutzer erstellt.

### <a name="authority-url-and-adfs"></a>Behörde Url und ADFS

ADFS ist nicht erkannt Produktion STS Instanz Entdeckung und false bei AuthenticationContext-Konstruktor übergeben werden müssen.

Behörde Url benötigt STS-Instanz und Mieter Name: https://login.windows.net/yourtenant.onmicrosoft.com

### <a name="querying-cache-items"></a>Abfragen von Elementen

ADAL bietet Standardcache in SharedPreferences mit einigen einfachen Cache Abfrage. Sie erhalten den aktuellen Cache von AuthenticationContext mit:
```Java
 ITokenCacheStore cache = mContext.getCache();
```
Cacheimplementierung bieten Sie anpassen.
```Java
mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);
```

### <a name="promptbehavior"></a>PromptBehavior

ADAL bietet die Möglichkeit, Fragen Verhalten angeben. PromptBehavior.Auto öffnet Benutzeroberfläche Aktualisierungstoken ist ungültig und Benutzeranmeldeinformationen erforderlich sind. PromptBehavior.Always überspringt die Cache-Nutzung und Benutzeroberfläche immer anzeigen.

### <a name="silent-token-request-from-cache-and-refresh"></a>Automatische token Anforderung aus dem Cache und aktualisieren

Diese Methode verwendet keine Benutzeroberfläche Pop up und keine Aktivität. Aus dem Cache ggf. wird token zurückgegeben. Token abgelaufen ist, versucht er, es zu aktualisieren. Aktualisierungstoken abgelaufen oder Fehler, wird AuthenticationException zurückgegeben.

    ```Java
    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );
    ```

Sie können auch synchronisieren mit dieser Methode aufrufen. Sie können null, Rückruf oder festlegen AcquireTokenSilentSync.

### <a name="diagnostics"></a>Diagnose

Folgende sind die wichtigsten Informationsquellen zur Diagnose von Problemen:

+ Ausnahmen
+ Protokolle
+ Netzwerk-Spuren

Beachten Sie, dass Korrelations-IDs der Diagnose in der Bibliothek sind. Legen Sie die Korrelation IDs auf pro Anfrage soll eine ADAL Korrelation mit anderen Vorgängen im Code anfordern. Wenn Sie Korrelationen Id nicht ADAL zufällig generiert wird, und werden Nachrichten protokollieren und Netzwerkaufrufe mit Korrelations-Id versehen. Selbst generierte Id ändert sich bei jeder Anforderung.

#### <a name="exceptions"></a>Ausnahmen

Dies ist offensichtlich der ersten Diagnose. Wir versuchen, hilfreiche Fehlermeldungen. Wenn Sie eine nicht hilfreich ist ein Problem melden und lassen Sie uns wissen. Geben Sie Informationen wie Model SDK-auch aus.

#### <a name="logs"></a>Protokolle

Sie können die Bibliothek Protokollnachrichten generiert, mit denen Sie Probleme diagnostizieren. Sie Protokollierung durch den folgenden Aufruf einen Rückruf konfigurieren, den ADAL generiert wird, aus jeder Nachricht verwendet wird.


 ```Java
 Logger.getInstance().setExternalLogger(new ILogger() {
     @Override
     public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
      ...
      // You can write this to logfile depending on level or errorcode.
      writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
     }
 }
 ```
Nachrichten können in einer benutzerdefinierten Protokolldatei geschrieben werden, wie unten dargestellt. Leider ist keine standardmäßige Möglichkeit, Protokolle von einem Gerät. Es gibt einige Dienste, die Ihnen dabei helfen können. Sie können auch eigene, wie die Datei an einen Server sendet erfinden.

```Java
private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
}
```

##### <a name="logging-levels"></a>Protokollierungsstufen

+ Error(Exceptions)
+ Warn(Warning)
+ Info (Zwecke)
+ Verbose (Weitere Details)

Die Ebene wie folgt festlegen:
```Java
Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);
 ```

 Alle Lognachrichten werden Logcat neben Alle Rückrufe benutzerdefiniertes Protokoll gesendet.
Protokoll in eine Datei Formular Logcat erhalten als angezeigte verwendeten:

 ```
  adb logcat > "C:\logmsg\logfile.txt"
 ```
 Weitere Beispiele über Adb Befehle: https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat

#### <a name="network-traces"></a>Netzwerk-Spuren

Verschiedene Tools können Sie um den HTTP-Verkehr zu erfassen, den ADAL generiert.  Dies eignet sich am besten OAuth-Protokoll vertraut sind oder wenn Sie diagnostische Informationen an Microsoft oder anderen Supportkanäle.

Fiddler ist das einfachste Tool zum Verfolgen von HTTP.  Verwenden Sie die folgenden Links bis richtig aufzeichnen ADAL Netzwerkverkehr einrichten.  Um ist Fiddler oder ein anderes Programm wie Charles aufzuzeichnende unverschlüsselten SSL-Datenverkehr konfiguriert.  Hinweis: Spuren auf diese Weise können umfangreiche Informationen Zugriffstoken, Benutzernamen und Kennwörter enthalten.  Wenn Sie Produktionskonten verwenden, teilen Sie diese Spuren nicht mit 3.  Benötigen Sie eine Verfolgung einer Person angeben, um Unterstützung zu erhalten, reproduzieren Sie das Problem, mit einem temporären Konto mit Benutzernamen und Kennwörter, die Sie freigeben ausmacht.

+ [Einrichten von Fiddler für Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
+ [Konfigurieren von Fiddler Regeln für ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)


### <a name="dialog-mode"></a>Dialogfeld-Modus
AcquireToken-Methode ohne unterstützt Dialogfeld.

### <a name="encryption"></a>Verschlüsselung

ADAL verschlüsselt Token und standardmäßig in SharedPreferences speichern. Betrachten Sie die StorageHelper-Klasse zu sehen. Android, AndroidKeyStore für sichere Speicherung privater Schlüssel 4.3(API18) eingeführt. ADAL verwendet, API18 und höher. Wenn Sie ADAL für niedrigere SDK-Versionen verwenden möchten, müssen Sie geheime Schlüssel unter AuthenticationSettings.INSTANCE.setSecretKey

### <a name="oauth2-bearer-challenge"></a>Oauth2 Träger Herausforderung

AuthenticationParameters-Klasse stellt Funktionen zum Abrufen der Authorization_uri aus Oauth2 Träger Herausforderung.

### <a name="session-cookies-in-webview"></a>Sitzungscookies in der Webansicht

Android Webansicht deaktivieren Sitzungscookies nicht, nachdem die Anwendung geschlossen wird. Sie können dies mit Beispielcode behandeln:
```java
CookieSyncManager.createInstance(getApplicationContext());
CookieManager cookieManager = CookieManager.getInstance();
cookieManager.removeSessionCookie();
CookieSyncManager.getInstance().sync();
```
Weitere Informationen zu Cookies: http://developer.android.com/reference/android/webkit/CookieSyncManager.html

### <a name="resource-overrides"></a>Ressource überschreibt

ADAL Bibliothek umfasst englische Zeichenfolgen für die beiden folgenden ProgressDialog Nachrichten.

Die Anwendung sollte ggf. lokalisierte Zeichenfolgen überschrieben.

```Java
<string name="app_loading">Loading...</string>
<string name="broker_processing">Broker is processing</string>
<string name="http_auth_dialog_username">Username</string>
<string name="http_auth_dialog_password">Password</string>
<string name="http_auth_dialog_title">Sign In</string>
<string name="http_auth_dialog_login">Login</string>
<string name="http_auth_dialog_cancel">Cancel</string>
```

=======

### <a name="ntlm-dialog"></a>NTLM-Dialogfeld
Adal Version 1.1.0 unterstützt NTLM-Dialogfeld, das durch WebViewClient OnReceivedHttpAuthRequest Ereignis verarbeitet wird. Layout des Dialogfelds und Zeichenfolgen können angepasst werden.

### <a name="cross-app-sso"></a>Cross-app SSO
Aktivieren Sie [Cross-app SSO-Funktionen für Android mit ADAL](active-directory-sso-android.md)  


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
