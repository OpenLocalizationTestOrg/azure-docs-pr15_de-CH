<properties
    pageTitle="Android von Azure Active Directory v2. 0 | Microsoft Azure"
    description="Wie ein Android erstellen, die Benutzer mit persönlichen Microsoft-Konto und Arbeit oder schulkonten und Graph-API aufruft, signiert mit Bibliotheken von Drittanbietern."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

#  <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Ein Graph-API v2. 0-Endpunkt mit einer Drittanbieter-Bibliothek mit Android-in hinzufügen

Microsoft Identitätsplattform verwendet offene Standards wie OAuth2 OpenID verbinden. Jede Bibliothek können Entwickler wollen sie unsere Services integriert. Um unsere Plattform mit anderen Bibliotheken verwenden Entwickler unterstützen, haben wir einige Exemplarische Vorgehensweisen wie diese wie Drittanbieterbibliotheken Verbindung zur Microsoft-Identitätsplattform konfiguriert wird geschrieben. Bibliotheken, die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren können zur Microsoft Identity-Plattform.

Mit der in dieser exemplarischen Vorgehensweise erstellte Anwendung Benutzer ihrer Organisation anmelden und anschließend Suchen sich in ihrer Organisation mithilfe der Graph-API.

Wenn Sie OAuth2 oder OpenID nicht vertraut sind, kann dieses Beispiel-Konfiguration nicht Ihnen sinnvoll. Wir empfehlen [2.0 Protokolle - OAuth 2.0 Authorization Code Flow](active-directory-v2-protocols-oauth-code.md) Hintergrundinformationen lesen.

> [AZURE.NOTE] Einige Features der Plattform, die einen Ausdruck in Standards OAuth2 oder OpenID verbinden Zugangskontrolle wie Intune Richtlinienmanagement erfordern unsere offene Quelle Microsoft Azure Identität Bibliotheken.

V2. 0-Endpunkt unterstützt nicht alle Azure Active Directory Szenarien und Features.

> [AZURE.NOTE] Um festzustellen, ob der v2. 0-Endpunkt verwenden sollten, lesen Sie [v2. 0-Einschränkungen](active-directory-v2-limitations.md).


## <a name="download-the-code-from-github"></a>Laden Sie den Code von GitHub
Der Code für dieses Lernprogramm wird [auf GitHub](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2)verwaltet.  Um folgen, können Sie [die Anwendung Skelett als .zip downloaden](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) oder das Skelett Klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Sie können auch das Beispiel herunterladen und loslegen:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Registrieren einer Anwendung
Erstellen einer neuen Applikation [Anwendung registrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)oder detaillierte Schritte wie [sich eine Anwendung mit dem Endpunkt v2. 0](active-directory-v2-app-registration.md).  Stellen Sie sicher, dass:

- Kopieren Sie die **Id der Anwendung** , die Ihre Anwendung zugewiesen ist, da Sie bald benötigt werden.
- **Mobile** Plattform für Ihre Anwendung hinzufügen.

> Hinweis: Das Anwendung registrierungsportal bietet **Umleiten URI** -Wert. Dabei müssen Sie jedoch den Standardwert verwenden `https://login.microsoftonline.com/common/oauth2/nativeclient`.


## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>NXOAuth2 von Drittanbietern Library herunterladen und einen Arbeitsbereich erstellen

In dieser exemplarischen Vorgehensweise verwenden Sie OIDCAndroidLib von GitHub, eine OAuth2 Bibliothek basierend auf dem Code OpenID Verbinden von Google. Es implementiert das systemeigene Anwendungsprofil und autorisierungsendpunkt des Benutzers unterstützt. Dies sind die Punkte, die Microsoft Identity-Plattform integrieren müssen.

Klonen Sie OIDCAndroidLib Repo Computer.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Richten Sie Ihre Android Studio-Umgebung

1. Erstellen eines neuen Projekts Android Studio und übernehmen Sie die Standardeinstellungen des Assistenten.

    ![Neues Projekt in Android Studio erstellen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

    ![Zielgeräte](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

    ![Hinzufügen einer Aktivität zu mobile](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Verschieben Sie zum Einrichten der Projektmodulen geklonte Repo Projekt an. Sie können auch das Projekt erstellen und Klonen Sie es direkt an das Projekt.

    ![Projektmodule](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Öffnen Sie die eingestellte Module mithilfe des Kontextmenüs oder mithilfe der Tastenkombination Strg + Alt + Mai + S.

    ![Module Einstellungen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Entfernen Sie das Standardmodul app nur die eingestellte Container möchten.

    ![Standard-app-Modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Importieren Sie Module aus der geklonten Repo zum aktuellen Projekt.

    ![Gradle-Projekt importieren](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG)
    ![neue Modulseite erstellen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Wiederholen Sie diese Schritte für die `oidlib-sample` Modul.

7. Oidclib-Abhängigkeit Aktivieren der `oidlib-sample` Modul.

    ![Oidclib Oidlib Beispiel-Modul Abhängigkeit](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Klicken Sie auf **OK** und Gradle Synchronisierung warten.

    Ihre settings.gradle aussehen sollte:

    ![Screenshot des settings.gradle](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Erstellen Sie die Beispiel-app, um sicherzustellen, dass das Beispiel ordnungsgemäß ausgeführt werden.

    Sie wird nicht noch Azure Active Directory verwenden können. Wir benötigen einige Endpunkte konfiguriert. Dies ist sicher, dass Sie Android Studio Probleme haben, bevor Sie beginnen die Beispiel-app anpassen.

10. Erstellen und ausführen `oidlib-sample` als Ziel in Android.

    ![Status des Oidlib Beispiel erstellen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Löschen der `app ` Verzeichnis, das Sie das Modul aus dem Projekt entfernt, Android Studio Sicherheit löschen nicht ausgefüllt wurde.

    ![Struktur, die das Anwendungsverzeichnis](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Öffnen Sie das Menü **Bearbeiten Konfigurationen** die Testlaufkonfiguration entfernen, die auch beibehalten wurde, wenn Sie das Modul aus dem Projekt entfernt.

    ![Menü Konfigurationen bearbeiten](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![Testlaufkonfiguration App](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Konfigurieren Sie die Endpunkte des Beispiels

Jetzt haben Sie die `oidlib-sample` erfolgreich ausgeführt, wir einige Endpunkte funktioniert Azure Active Directory zu bearbeiten.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Konfigurieren Sie den Client durch Bearbeiten der Datei oidc_clientconf.xml

1. Da Sie nur für ein Token und rufen die Graph-API OAuth2 Flows verwenden, richten Sie nach OAuth2. OIDC kommen im Beispiel weiter unten.

    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```

2. Konfigurieren Sie die Client-ID, die von der registrierungsportal erhalten.

    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```

3. Konfigurieren Sie die URI-Umleitung der unten.

    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```

4. Konfigurieren Sie die Bereiche, die Sie benötigen, um die Graph-API zugreifen.

    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

Die `User.Read` Wert in `oidc_scopes` basic Profile die Benutzer lesen können.
Erfahren Sie mehr über die verfügbaren Bereiche [Microsoft Graph Berechtigung](https://graph.microsoft.io/docs/authorization/permission_scopes)Bereiche.

Möchten Sie Erklärung über `openid` oder `offline_access` Bereiche OpenID Connect Siehe [2.0 Protokolle - OAuth 2.0 Authorization Code übertragen](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Konfigurieren Sie die Clientendpunkte durch Bearbeiten der Datei oidc_endpoints.xml

- Öffnen der `oidc_endpoints.xml` -Datei und ändern Sie folgende:

    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

Diese Endpunkte sollten nicht ändern, wenn Sie OAuth2 als Protokoll verwenden.

> [AZURE.NOTE]
Die Endpunkte für `userInfoEndpoint` und `revocationEndpoint` von Azure Active Directory derzeit nicht unterstützt. Wenn Sie mit example.com Standardwert lassen, werden Sie darauf sind sie nicht in die Stichprobe :-)


## <a name="configure-a-graph-api-call"></a>Konfigurieren Sie einen Graph-API-Aufruf

- Öffnen der `HomeActivity.java` -Datei und ändern Sie folgende:

    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Ein einfacher Diagramm API-Aufruf gibt hier Informationen.

Sind alle Änderungen, die Sie verwenden müssen. Führen Sie die `oidlib-sample` Anwendung, und klicken Sie auf **Anmelden**.

Nachdem Sie erfolgreich authentifiziert haben, wählen Sie **Geschützte Ressourcen anfordern** Aufruf der Graph-API zu testen.

## <a name="get-security-updates-for-our-product"></a>Abrufen von Sicherheitsupdates für das Produkt

Wir empfehlen Ihnen Benachrichtigungen zu Sicherheitsvorfällen [Sicherheits-TechCenter](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
