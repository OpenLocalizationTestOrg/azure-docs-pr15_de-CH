<properties
    pageTitle="Azure Active Directory B2C: Konfiguration Amazon | Microsoft Azure"
    description="Gewähren Sie Anmeldung und Anmelden mit Amazon-Konten in der Anwendung von Azure Active Directory B2C gesichert sind."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Gewähren Sie Azure Active Directory B2C: Anmeldung und Anmelden mit Amazon-Konten

## <a name="create-an-amazon-application"></a>Erstellen Sie eine Amazon-Anwendung

Amazon als Identitätsanbieter in Azure Active Directory (Azure AD) B2C verwenden, müssen Sie eine Amazon-Anwendung erstellen und die richtigen Parameter angeben. Sie benötigen dazu eine Amazon-Konto. Wenn Sie haben, erhalten Sie es unter [http://www.amazon.com/](http://www.amazon.com/).

1. Besuchen Sie das [Developer Center Amazon](https://login.amazon.com/) und Ihre Amazon-Anmeldeinformationen anmelden.
2. Wenn nicht bereits geschehen, klicken Sie auf **Registrieren**gehen Sie Entwickler Registrierung und übernehmen Sie die Richtlinie.
3. Klicken Sie auf **neue Anwendung registrieren**.

    ![Registrieren eine neue Anwendung auf Amazon-website](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Informationen Sie Anwendung (**Name**, **Beschreibung**und **Datenschutz beachten URL**), und klicken Sie auf **Speichern**.

    ![Die Informationen zur Registrierung einer neuen Anwendung bei Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. Kopieren Sie in **Web** -Einstellungen die Werte der **Client-ID** und **Geheimen** (Sie müssen hierzu **Schlüssel anzeigen** klicken.) Sie brauchen beide Amazon als Identitätsanbieter in Ihrem Mandanten zu konfigurieren. Klicken Sie unten im Abschnitt **Bearbeiten** . **Clientschlüssel** ist ein wichtiges Sicherheitsfeature.

    ![Die Client-ID und geheimen für Ihre neue Anwendung bei Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Geben Sie `https://login.microsoftonline.com` im Feld **Zulässige JavaScript Herkunft** und `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` im Feld **Zurückgeben URLs zulässig** . Name des Mieters (z. B. contoso.onmicrosoft.com) **{Tenant}** ersetzen. Klicken Sie auf **Speichern**. Der Wert **{Tenant}** beachtet werden.

    ![Die neue Anwendung bei Amazon zur JavaScript Ursprünge und URLs zurück](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Konfigurieren von Amazon als Identitätsanbieter in Ihrem Mandanten

1. Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
2. Klicken Sie auf Features B2C-Blade **Identitätsanbieter**aus.
3. Klicken Sie auf **+ Hinzufügen** oben das Blade.
4. Geben Sie einen angezeigten **Namen** Anbieterkonfiguration Identität. Geben Sie beispielsweise "Amzn".
5. Auf **Identität Anbietertyp**, wählen Sie **Amazon**und auf **OK**.
6. Klicken Sie auf **Identitätsanbieter** und geben Sie die Client-ID und geheimen Amazon-Anwendung, die Sie zuvor erstellt haben.
7. Klicken Sie auf **OK** , und klicken Sie dann auf **Erstellen** , um die Amazon-Konfiguration zu speichern.
