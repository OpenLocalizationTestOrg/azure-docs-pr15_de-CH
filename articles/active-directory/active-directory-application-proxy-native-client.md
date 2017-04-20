<properties
    pageTitle="Veröffentlichung von systemeigenen Clientanwendungen mit Proxy aktivieren | Microsoft Azure"
    description="Erläutert, wie systemeigene Clientanwendungen mit Azure AD Anwendung Proxy sicheren Remotezugriff auf Ihre lokalen apps zu aktivieren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="how-to-enable-native-client-apps-to-interact-with-proxy-applications"></a>Systemeigene Clientanwendungen interagieren Proxy Applikationen aktivieren

Azure Active Directory Anwendungsproxy diverse Browseranwendungen wie SharePoint, Outlook Web Access und benutzerdefinierte branchenanwendungen veröffentlicht. Es kann auch verwendet werden, einheitlichen Clientanwendungen Veröffentlichen von webapps unterscheiden, da sie auf einem Gerät installiert. Dies geschieht durch die Unterstützung von Azure AD ausgestellte Token standard autorisieren HTTP-Header gesendet werden.

![Beziehung zwischen Benutzer und Azure Active Directory veröffentlichte Programme](./media/active-directory-application-proxy-native-client/richclientflow.png)

Veröffentlichen von Anträgen empfiehlt es sich, die Authentifizierung Ärger und fördert viele Clientumgebungen Azure AD-Authentifizierung-Bibliothek verwenden. Anwendungsproxy passt in die [Systemeigene Anwendung Web API-Szenario](active-directory-authentication-scenarios.md#native-application-to-web-api). Der Prozess für die hierfür lautet wie folgt:

## <a name="step-1-publish-your-application"></a>Schritt 1: Veröffentlichen der Anwendung

Veröffentlichen Sie die Proxyanwendung wie jede andere Anwendung, weisen Sie Benutzer zu und Ihnen Sie Premium oder basic-Lizenzen. Weitere Informationen finden Sie unter [Veröffentlichen Anwendung Anwendungsproxy](active-directory-application-proxy-publish.md).

## <a name="step-2-configure-your-application"></a>Schritt 2: Konfigurieren der Anwendung

Konfigurieren Sie Ihre systemeigene Anwendung wie folgt:

1. Mit klassischen Azure-Portal anmelden.
2. Wählen Sie das Symbol Active Directory im linken Menü auf, und wählen Sie das Verzeichnis.
3. Klicken Sie im oberen Menü auf **Programme**. Wenn keine apps zu Ihrem Verzeichnis hinzugefügt haben, zeigt diese Seite nur den Link zum **Hinzufügen einer Anwendung** . Klicken Sie auf den Link oder Sie können auf der Befehlsleiste auf **Hinzufügen** klicken.
4. Klicken Sie auf den Link **Mein Unternehmen entwickelten Anwendung hinzufügen**auf der Seite **Was möchten Sie tun** .
5. Geben Sie auf der Seite **Angaben über Ihre Anwendung** einen Namen für Ihre Anwendung, und wählen Sie **Native Client-Anwendung**. Klicken Sie auf das Pfeilsymbol weiter.
6. Auf der Seite **Informationen** **URI umleiten** der systemeigene Clientanwendung bereitstellen Sie und dann auf das Kontrollkästchen abgeschlossen.

Die Anwendung hinzugefügt wurde, und Sie gelangen zur Seite Schnellstart für Ihre Anwendung.

## <a name="step-3-grant-access-to-other-applications"></a>Schritt 3: Zugriff auf andere Programme gewähren

Aktivieren Sie die systemeigene Anwendung, andere Programme im Verzeichnis angezeigt werden soll:

1. Klicken Sie im Hauptmenü auf **Applikationen**, wählen Sie die neue systemeigene Anwendung und klicken Sie auf **Konfigurieren**.
2. Bildlauf im Abschnitt **Berechtigungen auf andere Programme** . Klicken Sie auf **Anwendung hinzufügen** wählen Sie Proxyanwendung, der die systemeigene Anwendung gewähren möchten, und klicken Sie auf das Häkchen in der rechten unteren Ecke. Wählen Sie im Dropdownmenü **Berechtigungen delegiert** die neue Berechtigung.

![Berechtigungen für andere Applikationen Screenshot - hinzufügen Anwendung](./media/active-directory-application-proxy-native-client/delegate_native_app.png)

## <a name="step-4-edit-the-active-directory-authentication-library"></a>Schritt 4: Bearbeiten Sie Active Directory-Authentifizierungsbibliothek

Bearbeiten Sie den anwendungsspezifischen Code im Bereich Authentifizierung von der Active Directory Authentifizierung Library (ADAL) auf folgende:

        // Acquire Access Token from AAD for Proxy Application
        AuthenticationContext authContext = new AuthenticationContext("https://login.microsoftonline.com/<TenantId>");
        AuthenticationResult result = authContext.AcquireToken("< Frontend Url of Proxy App >",
                                                        "< Client Id of the Native app>",
                                                        new Uri("< Redirect Uri of the Native App>"),
                                                        PromptBehavior.Never);

        //Use the Access Token to access the Proxy Application
        HttpClient httpClient = new HttpClient();
        httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
        HttpResponseMessage response = await httpClient.GetAsync("< Proxy App API Url >");

Die Variablen sollten wie folgt ersetzt:

- **TenantId** können nach "/ Verzeichnis /" die GUID in der URL der Seite **Konfiguration** der Anwendung gefunden werden.
- **Front-End-URL** ist der URL-front-End in der Proxy-Anwendung eingegeben und finden Sie auf der Seite **Konfiguration** des Proxy-Anwendung.
- **Client-Id** der systemeigene Anwendung finden auf der Seite **Konfigurieren** der einheitlichen Anwendung.
- **Umleitung URI der einheitlichen Anwendung** finden auf der Seite **Konfigurieren** der einheitlichen Anwendung.

![Neue systemeigene Anwendung konfigurieren Seite screenshot](./media/active-directory-application-proxy-native-client/new_native_app.png)

Weitere Informationen über die systemeigene Anwendung finden Sie unter [Native Anwendung Web API](active-directory-authentication-scenarios.md#native-application-to-web-api).


## <a name="see-also"></a>Siehe auch

- [Veröffentlichen Sie mit Ihren eigenen Domänennamen Applikationen](active-directory-application-proxy-custom-domains.md)
- [Bedingter Zugriff](active-directory-application-proxy-conditional-access.md)
- [Arbeiten mit Ansprüchen Beachten](active-directory-application-proxy-claims-aware-apps.md)
- [Einmalige Anmeldung aktivieren](active-directory-application-proxy-sso-using-kcd.md)

Überprüfen Sie die aktuellsten Neuigkeiten und Updates [Anwendungsproxy blog](http://blogs.technet.com/b/applicationproxyblog/)
