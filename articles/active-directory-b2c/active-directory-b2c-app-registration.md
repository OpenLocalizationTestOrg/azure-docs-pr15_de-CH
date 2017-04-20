<properties
    pageTitle="Azure Active Directory B2C: Registrierung Anwendung | Microsoft Azure"
    description="Wie Sie Ihre Anwendung mit Azure Active Directory B2C registrieren"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Registrieren Sie die Anwendung

## <a name="prerequisite"></a>Voraussetzung

Zum Erstellen einer Anwendung, die Verbraucher Anmeldung anmelden akzeptiert, müssen Sie die Anwendung mit einer Azure Active Directory B2C Mieter registrieren. Rufen Sie eigene Mieter mithilfe der Schritte in [Erstellen einer Azure AD B2C-Mandanten ab](active-directory-b2c-get-started.md). Nachdem Sie alle Schritte in diesem Artikel folgen, müssen Sie Ihr Startmenü angeheftete B2C-Features-Blade.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Navigieren Sie zu B2C-Features blade

Haben Sie Ihr Startmenü angeheftete B2C-Features-Blade, sehen Sie das Blade als [Azure-Portal](https://portal.azure.com/) als globaler Administrator B2C-Mandanten Anmeldung.

Klicken im linken Navigationsbereich der [Azure-Portal](https://portal.azure.com/) **Durchsuchen** und **Azure AD B2C** können Sie auch das Blade zugreifen.

> [AZURE.IMPORTANT] Sie müssen ein globaler Administrator B2C Mieter B2C Features Blade zugreifen zu können. Von anderen Mandanten ein globaler Administrator oder ein Benutzer alle Mieter kann nicht darauf zugreifen.  Die B2C-Mandanten können mithilfe der Tenant-Wechsler rechts oben das Azure-Portal.

## <a name="register-an-application"></a>Registrieren einer Anwendung

1. B2C-Features Blade Azure-Portal klicken Sie auf **Programme**.
2. Klicken Sie auf **+ Hinzufügen** oben das Blade.
3. Geben Sie einen **Namen** für die Anwendung, die die Anwendung für Verbraucher beschreiben. Sie könnten beispielsweise "Contoso B2C-app" eingeben.
4. Wenn Sie eine Web-basierte Anwendung schreiben, Schalter **sind WebApp / web API** auf **Ja**. **Antwort-URLs** sind Endpunkte, Azure AD B2C alle Token zurück, die die Anwendung anfordert. Geben Sie z. B. `https://localhost:44321/`. Wenn die Webanwendung auch einige Web-API von Azure AD B2C gesichert zurückgerufen wird, sollten Sie einen **Geheimen Schlüssel** sowie zu erstellen, klicken Sie auf **Schlüssel generieren** .

    > [AZURE.NOTE] Einen **Geheimen Schlüssel** ist ein wichtiges Sicherheitsfeature und sollte entsprechend gesichert werden.

5. Wenn Sie eine mobile Anwendung schreiben, Schalter **einschließen native Client** auf **Ja**. Kopieren Sie die Standard- **URI umleiten** , die automatisch für Sie erstellt.
6. Klicken Sie auf **Erstellen** , um Ihre Anwendung registrieren.
7. Klicken Sie auf die Anwendung, die Sie gerade erstellt haben, und kopieren Sie die global eindeutige **Client-ID der Anwendung** , die Sie später in Ihrem Code verwenden.

> [AZURE.IMPORTANT] Anwendung B2C Features Blatt erstellt haben am gleichen Speicherort verwaltet. B2C bearbeiten Applikationen mit PowerShell oder ein anderes Portal sie werden nicht unterstützt und funktioniert möglicherweise nicht mit Azure AD B2C.

## <a name="build-a-quick-start-application"></a>Quick-Start-Anwendung erstellen

Jetzt haben Sie eine Anwendung mit Azure AD B2C registriert, können Sie einen Schnellstart-Lernprogramme bis und abschließen. Hier sind einige Vorschläge:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
