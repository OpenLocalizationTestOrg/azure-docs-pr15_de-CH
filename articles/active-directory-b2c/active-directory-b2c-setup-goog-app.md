<properties
    pageTitle="Azure Active Directory B2C: Google Konfiguration | Microsoft Azure"
    description="Gewähren Sie Anmeldung und Anmelden mit Google Konten in Ihrer Anwendung von Azure Active Directory B2C gesichert sind."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Gewähren Sie Azure Active Directory B2C: Anmeldung und Anmelden mit Google Konten

## <a name="create-a-google-application"></a>Google-Anwendung erstellen

Google als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden, müssen Sie eine Google-Anwendung erstellen und die richtigen Parameter angeben. Sie benötigen ein Google Konto. Wenn Sie haben, können Sie es unter [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)abrufen.

1. Rufen Sie [Google Entwickler-Konsole](https://console.developers.google.com/) und die Google-Anmeldeinformationen anmelden.
2. Klicken Sie auf **Projekt erstellen**, geben Sie einen **Projektnamen**, und klicken Sie auf **Erstellen**.

    ![Google - erste Schritte](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google - neues Projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Auf **API-Manager** , und klicken Sie im linken Navigationsbereich auf **Anmeldeinformationen** .
4. Klicken Sie auf die Registerkarte **OAuth Zustimmung Bildschirm** oben.

    ![Google - Anmeldeinformationen](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Wählen Sie oder geben Sie eine gültige **e-Mail-Adresse**, geben Sie einen **Produktnamen**und klicken Sie auf **Speichern**.

    ![Google - OAuth zustimmungsbildschirm](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Klicken Sie auf **neue Anmeldeinformationen** und dann **OAuth-Client-ID**.

    ![Google - OAuth zustimmungsbildschirm](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Wählen Sie unter **Anwendungstyp** **Web Application**.

    ![Google - OAuth zustimmungsbildschirm](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Geben Sie einen **Namen** für Ihre Anwendung, geben Sie `https://login.microsoftonline.com` im Feld **autorisierte JavaScript Herkunft** und `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **autorisierte URIs umleiten** . Name des Mieters (z. B. contosob2c.onmicrosoft.com) **{Tenant}** ersetzen. Der Wert **{Tenant}** beachtet werden. Klicken Sie auf **Erstellen**.

    ![Google - Client-ID erstellen](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Kopieren Sie die Werte der **Client-ID** und **geheimen**. Sie benötigen beide Google als Identitätsanbieter in Ihrem Mandanten konfigurieren. **Clientschlüssel** ist ein wichtiges Sicherheitsfeature.

    ![Google - Clientschlüssel](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigurieren Sie Google als Identitätsanbieter in Ihrem Mandanten

1. Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
2. Klicken Sie auf Features B2C-Blade **Identitätsanbieter**aus.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Geben Sie einen angezeigten **Namen** Anbieterkonfiguration Identität. Geben Sie beispielsweise "G +".
5. Auf **Identität Anbietertyp**, wählen Sie **Google**und auf **OK**.
6. Klicken Sie auf **Identitätsanbieter** und geben Sie die Client-ID und geheimen Google-Anwendung, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** , und klicken Sie auf **Erstellen** , um die Google-Konfiguration zu speichern.
