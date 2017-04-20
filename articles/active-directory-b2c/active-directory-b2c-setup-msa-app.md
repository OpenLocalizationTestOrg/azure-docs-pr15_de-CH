<properties
    pageTitle="Azure Active Directory B2C: Konfiguration Microsoft-Konto | Microsoft Azure"
    description="Gewähren Sie Anmeldung und Anmelden mit Microsoft-Konten in der Anwendung von Azure Active Directory B2C gesichert sind."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Ermöglichen Sie Anmeldung und melden Sie sich mit Microsoft-Konten

## <a name="create-a-microsoft-account-application"></a>Erstellen Sie eine Microsoft-Konto-Anwendung

Microsoft-Konto als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden, müssen Sie eine Microsoft-Konto-Anwendung erstellen und die richtigen Parameter angeben. Sie benötigen ein microsoftkonto. Wenn Sie haben, erhalten Sie es unter [https://www.live.com/](https://www.live.com/).

1. Rufen Sie [Microsoft Anwendung Registrierungsportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) und Microsoft-Anmeldeinformationen anmelden.
2. Klicken Sie auf **eine Anwendung**.

    ![Microsoft Konto - Hinzufügen einer neuen Applikation](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Geben Sie einen **Namen** für Ihre Anwendung, und klicken Sie auf **Anwendung erstellen**.

    ![Microsoft-Konto - Anwendungsname](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Kopieren Sie den Wert der **Id der Anwendung**. Sie benötigen sie Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren.

    ![Microsoft-Konto - Id der Anwendung](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Klicken Sie auf **Add Platform** und wählen Sie **Web**.

    ![Microsoft Konto - Plattform hinzufügen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoft-Konto - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Geben Sie `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **URIs umleiten** . Name des Mieters (z. B. contosob2c.onmicrosoft.com) **{Tenant}** ersetzen.

    ![Microsoft-Konto - URL umleiten](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Klicken Sie auf **Neues Kennwort generieren** **Anwendung** Geheimnissen. Kopieren Sie das neue Kennwort auf dem Bildschirm angezeigt. Sie benötigen sie Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Dieses Kennwort ist ein wichtiges Sicherheitsfeature.

    ![Microsoft Konto - neues Kennwort generieren](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoft-Konto - Passwort](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Aktivieren Sie das Kontrollkästchen **Live SDK unterstützen** unter **Erweiterte Optionen** . Klicken Sie auf **Speichern**.

    ![Microsoft-Konto - Live SDK-Unterstützung](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Konfigurieren Sie Microsoft-Konto als Identitätsanbieter in Ihrem Mandanten

1. Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
2. Klicken Sie auf Features B2C-Blade **Identitätsanbieter**aus.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Geben Sie einen angezeigten **Namen** Anbieterkonfiguration Identität. Geben Sie beispielsweise "MSA".
5. Klicken Sie auf **Identität Provider**, wählen Sie **Microsoft-Konto aus**und auf **OK**.
6. Klicken Sie auf **Identitätsanbieter** und Anwendung-Id und Kennwort der Microsoft-Konto-Anwendung, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** , und klicken Sie auf **Erstellen** , um Ihre Microsoft-Konto speichern.
