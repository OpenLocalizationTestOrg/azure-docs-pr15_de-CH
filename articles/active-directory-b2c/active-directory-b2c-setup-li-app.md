<properties
    pageTitle="Azure Active Directory B2C: Konfiguration LinkedIn | Microsoft Azure"
    description="Mit LinkedIn Konten in Ihrer Anwendung durch Azure Active Directory B2C gesichert ermöglichen Sie Anmeldung und Anmeldung"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Gewähren Sie Azure Active Directory B2C: Anmeldung und Anmelden mit LinkedIn Konten

## <a name="create-a-linkedin-application"></a>Erstellen Sie eine Anwendung LinkedIn

LinkedIn als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden, müssen Sie eine LinkedIn Anwendung erstellen und die richtigen Parameter angeben. Sie benötigen ein LinkedIn Konto. Wenn Sie haben, erhalten Sie es unter [https://www.linkedin.com/](https://www.linkedin.com/).

1. Zur [LinkedIn Entwickler-Website](https://www.developer.linkedin.com/) und Ihre LinkedIn-Anmeldeinformationen anmelden.
2. Klicken Sie in der oberen Menüleiste auf **Meine Apps** und dann auf **Anwendung erstellen**.

    ![LinkedIn - neue Anwendung](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Füllen Sie im Formular **eine neue Anwendung erstellen** Informationen (**Firmenname**, **Name**, **Beschreibung**, **Anwendung Logo-URL**, **Anwendung**, **Website-URL**, **Geschäftlichen E-Mails** und **Telefon**).
4. **LinkedIn API Geschäftsbedingungen** stimmen Sie zu, und klicken Sie auf **Senden**.

    ![LinkedIn - Register app](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Kopieren Sie die Werte der **Client-ID** und **Geheimen**. (Sie können sie unter **Authentifizierung**finden.) Sie benötigen beide LinkedIn als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

    >[AZURE.NOTE] **Clientschlüssel** ist ein wichtiges Sicherheitsfeature.

6. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **Umleiten URLs autorisiert** (unter **OAuth 2.0**). Name des Mieters (z. B. contoso.onmicrosoft.com) **{Tenant}** ersetzen. Klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Aktualisieren**. Der Wert **{Tenant}** beachtet werden.

    ![LinkedIn - Setup-Anwendung](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von LinkedIn als Identitätsanbieter in Ihrem Mandanten

1. Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
2. Klicken Sie auf Features B2C-Blade **Identitätsanbieter**aus.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Geben Sie einen angezeigten **Namen** Anbieterkonfiguration Identität. Geben Sie beispielsweise "LI".
5. Auf **Identität Anbietertyp**, wählen Sie **LinkedIn**und auf **OK**.
6. Klicken Sie auf **Identitätsanbieter** und geben Sie die Client-ID und geheimen LinkedIn-Anwendung, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** , und klicken Sie auf **Erstellen** , um die LinkedIn-Konfiguration zu speichern.
