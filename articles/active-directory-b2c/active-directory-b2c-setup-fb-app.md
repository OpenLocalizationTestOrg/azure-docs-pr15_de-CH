<properties
    pageTitle="Azure Active Directory B2C: Konfiguration Facebook | Microsoft Azure"
    description="Gewähren Sie Anmeldung und Anmelden mit Facebook-Konten in der Anwendung von Azure Active Directory B2C gesichert sind."
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Gewähren Sie Azure Active Directory B2C: Anmeldung und Anmelden mit Facebook-Konten

## <a name="create-a-facebook-application"></a>Erstellen einer Facebook-Anwendung

Facebook als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden, müssen Sie eine Facebook-Anwendung erstellen und die richtigen Parameter angeben. Sie benötigen dazu eine Facebook-Konto. Wenn Sie haben, erhalten Sie es unter [https://www.facebook.com/](https://www.facebook.com/).

1. Besuchen Sie die Website [für Entwickler Facebook](https://developers.facebook.com/) und Ihre Facebook-Anmeldeinformationen anmelden.
2. Wenn nicht bereits geschehen, müssen Sie als Entwickler Facebook registrieren. Dazu klicken Sie auf **Registrieren** (in der oberen rechten Ecke der Seite) akzeptieren Facebook Richtlinien und Registrierung Schritte.
3. Klicken Sie auf **Meine Apps** und klicken Sie **Hinzufügen einer neuen Applikation**. **Website** als Plattform wählen Sie und klicken Sie auf **überspringen und App-ID erstellen**.

    ![Facebook - Hinzufügen einer neuen Applikation](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook - Hinzufügen einer neuen Applikation - Website](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook - App-ID erstellen](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Das Formular einen **Anzeigenamen**, eine gültige **Kontakt E-Mail**, eine passende **Kategorie**und dann auf **App-ID erstellen**. Dazu müssen Sie Richtlinien Facebook-Plattform und führen Sie eine Überprüfung online-Sicherheit.

    ![Facebook - Erstellen einer neuen App-ID](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. **Klicken Sie in der linken Navigationsleiste.**
6. Klicken Sie auf **+ Plattform hinzufügen** , und wählen Sie die **Website**.

    ![Facebook - Einstellungen](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebook - Einstellungen - Website](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Geben Sie [https://login.microsoftonline.com/](https://login.microsoftonline.com/) im Feld **Website-URL** , und klicken Sie dann auf **Speichern**.

    ![Facebook - Website-URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Kopieren Sie den Wert der **App-ID** Klicken Sie auf **Anzeigen** und kopieren Sie den Wert der **App-Schlüssel**. Sie benötigen beide Facebook als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. **App-Schlüssel** ist ein wichtiges Sicherheitsfeature.

    ![Facebook - App-ID & App Geheimnis](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Klicken Sie auf **+ Hinzufügen Produkt** auf der Navigationsleiste und dann **Fangen Sie** neben **Facebook anmelden**.

    ![Facebook - Facebook anmelden](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **gültige OAuth umleiten URIs** im Abschnitt **OAuth-Clienteinstellungen** . Name des Mieters (z. B. contosob2c.onmicrosoft.com) **{Tenant}** ersetzen. Klicken Sie auf **Speichern** am unteren Rand der Seite.

    ![Facebook - URI OAuth-Umleitung](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Um Facebook Anwendung von Azure AD B2C, müssen öffentlich zugänglich gemacht werden. **App-Überprüfung** auf der Navigationsleiste und den Schalter am oberen Rand der Seite auf **Ja** und **bestätigen**auf dabei.

    ![Facebook - App öffentliche](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Konfigurieren Sie Facebook als Identitätsanbieter in Ihrem Mandanten

1. Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
2. Klicken Sie auf Features B2C-Blade **Identitätsanbieter**aus.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Geben Sie einen angezeigten **Namen** Anbieterkonfiguration Identität. Geben Sie beispielsweise "FB".
5. Auf **Identität Anbietertyp**, wählen Sie **Facebook**und auf **OK**.
6. Klicken Sie auf **Identitätsanbieter** und app-ID und app-Schlüssel (der Facebook-Anwendung, die Sie zuvor erstellt haben) die **Client-ID** und **geheimen** Felder bzw..
7. Klicken Sie auf **OK**, und klicken Sie auf **Erstellen** , um Ihre Facebook-Konfiguration zu speichern.
