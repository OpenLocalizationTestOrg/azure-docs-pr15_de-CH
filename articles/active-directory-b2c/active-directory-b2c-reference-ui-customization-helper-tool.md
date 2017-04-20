<properties
    pageTitle="Azure Active Directory B2C: Page UI Anpassung Hilfsprogramm | Microsoft Azure"
    description="Ein Hilfsprogramm veranschaulichen die Seite Anpassung Benutzeroberflächenfunktion in Azure Active Directory B2C"
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
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory B2C: Ein Hilfsprogramm veranschaulichen die Seite Benutzer Benutzeroberfläche Anpassungsfeature

Dieser Artikel ist eine Ergänzung zur [Benutzeroberfläche Anpassung Artikel](active-directory-b2c-reference-ui-customization.md) in Azure Active Directory (Azure AD) B2C. Die folgenden Schritte beschreiben, wie Übung Benutzeroberflächenfunktion Anpassung Seite mit HTML und CSS Beispielinhalte, die wir bereitgestellt haben.

## <a name="get-an-azure-ad-b2c-tenant"></a>Abrufen einer Azure AD B2C Mieters

Bevor Sie etwas anpassen können, müssen Sie zu [einem Azure AD B2C-Mandanten](active-directory-b2c-get-started.md) , haben Sie bereits eine.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Erstellen einer Richtlinie anmelden oder Einloggen

Beispielinhalte vorliegende Customze zwei Seiten in einer [Anmeldung oder Richtlinie](active-directory-b2c-reference-policies.md)verwendet werden: die [einheitliche Seite](active-directory-b2c-reference-ui-customization.md) und die [Seite selbst bestätigt](active-directory-b2c-reference-ui-customization.md). Wenn [Richtlinien für die Anmeldung oder](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy)lokales Konto (e-Mail-Adresse), Facebook, Google und Microsoft als **Identitätsprovider**hinzufügen. Dies sind nur vertriebenen, die unser Beispiel HTML-Inhalt akzeptieren.  Falls gewünscht, können Sie auch eine Teilmenge dieser vertriebenen hinzufügen.

## <a name="register-an-application"></a>Registrieren einer Anwendung

Sie müssen [Registrieren einer Anwendung](active-directory-b2c-app-registration.md) in Ihrem B2C-Mandanten, die mit die Richtlinie ausgeführt werden können. Nach der Registrierung der Anwendung haben Sie einige Optionen, mit denen Sie die Registrierungsrichtlinie tatsächlich ausgeführt:

- Erstellen der Azure AD B2C Schnellstart Applications im Abschnitt "Erste Schritte" aufgeführten [Signieren einrichten und Verbraucher in der Anwendung anmelden](active-directory-b2c-overview.md#getting-started).
- Verwenden Sie vordefinierte [Azure AD B2C Spielplatz](https://aadb2cplayground.azurewebsites.net) . Möchten Sie den Effekt "Partikelsimulation" verwenden, muss eine Anwendung in Ihrem B2C-Mandanten mit **URI umleiten** registrieren `https://aadb2cplayground.azurewebsites.net/`.
- Mithilfe der Schaltfläche **Ausführen** im [Azure-Portal](https://portal.azure.com/)an.

## <a name="customize-your-policy"></a>Anpassen der Richtlinie

Um das Aussehen und Verhalten Ihrer Richtlinie anzupassen, müssen Sie zunächst HTML- und CSS-Dateien mit bestimmten Azure AD B2C-Konventionen. Dann können Sie statische Inhalte an einen öffentlich zugänglichen Speicherort hochladen, sodass Azure AD B2C zugreifen können. Ihre eigenen dedizierten Webserver, Azure BLOB-Speicher, Azure Content Delivery Network oder andere statische Ressource hosting Anbieter möglicherweise. Voraussetzung sind, dass Ihre Inhalte über HTTPS mit CORS zugegriffen werden. Nachdem Sie Ihre statische Inhalte im Web verfügbar gemacht haben, können Sie die Richtlinie auf diesen Speicherort und präsentieren Ihren Kunden die Inhalte bearbeiten. [UI-Anpassung Artikel](active-directory-b2c-reference-ui-customization.md) ausführlich wie die Azure AD B2C Anpassung funktioniert.

Für die Zwecke dieses Lernprogramms haben wir bereits einige Beispielinhalte erstellt und gehostet auf Azure BLOB-Speicher. Beispielinhalte ist eine sehr einfache Anpassung im Design des fiktiven Unternehmens "Wingtip Toys". Um eine eigene Richtlinie ausprobieren, gehen Sie folgendermaßen vor:

1. Melden Sie sich bei Ihrem Mandanten in [Azure-Portal](https://portal.azure.com/) , und navigieren Sie zu B2C-Features Blade.
2. Klicken Sie auf **anmelden oder Einloggen Richtlinien** und klicken Sie auf die Richtlinie (z. B. "b2c\_1\_Zeichen\_von\_Zeichen\_in").
3. Klicken Sie auf **Seite Benutzeroberfläche anpassen** und **einheitliche Anmeldung oder Seite**.
4. Die **benutzerdefinierte Seite verwenden** Schalter auf **Ja**. Geben Sie im Feld **benutzerdefinierte Seite URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Klicken Sie auf **OK**.
5. Klicken Sie auf **lokales Konto-Anmeldeseite**. Die **benutzerdefinierte Vorlage verwenden** Schalter auf **Ja**. Geben Sie im Feld **benutzerdefinierte Seite URI** `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Wiederholen Sie den gleichen Schritt **sozialen Konto-Anmeldeseite**.
 Klicken Sie auf **OK** zum Schließen der Benutzeroberfläche Anpassung Blades zweimal.
6. Klicken Sie auf **Speichern**.

Nun können Sie die benutzerdefinierte Richtlinie ausprobieren. Sie können Ihrer Anwendung den Spielplatz Azure AD B2C verwenden oder wenn soll, Sie können jedoch auch den Befehl **Ausführen** , in dem Blatt. Die Anwendung im Dropdown Listenfeld und wählen Sie die entsprechende URI-Umleitung. Klicken Sie auf die Schaltfläche **jetzt ausführen** . Eine neue Registerkarte öffnen, und führen Sie die Benutzeroberfläche der Anmeldung für die Anwendung mit den neuen Inhalt an.

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Beispielinhalte in Azure BLOB-Speicher hochladen

Möchten Sie Azure BLOB-Speicher verwenden, um Ihren Inhalt hosten, können Sie eigene Speicherkonto erstellen und unsere B2C-Hilfsprogramm hochzuladenden Dateien verwenden.

### <a name="create-a-storage-account"></a>Ein Speicherkonto erstellen

1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **+ neu** > **Daten + Speicher** > **Konto**. Sie benötigen ein Azure-Abonnement zum Erstellen eines Kontos Azure BLOB-Speicher. Sie können eine kostenlose Testversion der [Azure-Website](https://azure.microsoft.com/pricing/free-trial/)anmelden.
3. Geben Sie einen **Namen** für das Speicherkonto (z. B. "Contoso"), und wählen Sie die entsprechenden Optionen für **Preisgestaltung**, **Ressourcengruppe** und **Abonnements**. Stellen Sie sicher, dass die **an Startmenü anheften** aktiviert Option. Klicken Sie auf **Erstellen**.
4. Zurück auf das Startmenü, und klicken Sie auf dem soeben erstellte Speicherkonto.
5. Im Abschnitt **Zusammenfassung** auf **Container**und klicken Sie dann auf **+ Hinzufügen**.
6. Geben Sie einen **Namen** für den Container (z. B. "b2c"), und wählen Sie den **Zugriffstyp** **Blob** . Klicken Sie auf **OK**.
7. Die erstellte Container wird in der Liste auf die **Blobs** angezeigt. Notieren Sie die URL des Containers. Beispielsweise sollte es ähnlich aussehen `https://contoso.blob.core.windows.net/b2c`. Schließen Sie das Blatt **Blobs** .
8. Blatt Konto Speicher auf **Schlüssel** und notieren Sie **Storage-Kontonamen** und **Access-Primärschlüssel** .

1. Mit der [Azure-Portal](https://portal.azure.com/)anmelden.
2. Klicken Sie auf **+ neu** > **Daten + Speicher** > **Konto**. Sie benötigen ein Azure-Abonnement zum Erstellen eines Kontos Azure BLOB-Speicher. Sie können eine kostenlose Testversion der [Azure-Website](https://azure.microsoft.com/pricing/free-trial/)anmelden.
3. **BLOB-Speicher** unter **Konto**wählen Sie aus, und lassen Sie die Werte als Standard.  Sie können der Ressourcengruppe & Speicherort bearbeiten.  Klicken Sie auf **Erstellen**.
4. Zurück auf das Startmenü, und klicken Sie auf dem soeben erstellte Speicherkonto.
5. Klicken Sie im Abschnitt **Zusammenfassung** auf **+ Container**.
6. Geben Sie einen **Namen** für den Container (z. B. "b2c"), und wählen Sie den **Zugriffstyp** **Blob** . Klicken Sie auf **OK**.
7. Öffnen Sie die **Eigenschaften**, und notieren Sie die URL des Containers; Beispielsweise sollte es ähnlich aussehen `https://contoso.blob.core.windows.net/b2c`. Schließen Sie das Container-Blade.
8. Blatt Konto Speicher klicken Sie auf das **Schlüsselsymbol** und notieren Sie **Storage-Kontonamen** und **Access-Primärschlüssel** .

> [AZURE.NOTE]
    **Primäre Zugriffstaste** ist ein wichtiges Sicherheitsfeature.

### <a name="download-the-helper-tool-and-sample-files"></a>Der Helfer Werkzeug und Dateien

Sie können [Azure BLOB-Speicher Werkzeug und Hilfsdateien als ZIP-Datei](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) herunterladen oder Klonen von GitHub:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Dieses Repository enthält eine `sample_templates\wingtip` Verzeichnis Beispiel HTML, CSS und Bilder enthält. Für diese Vorlagen auf Ihr eigenes Konto Azure BLOB-Speicher müssen Sie die HTML-Dateien bearbeiten. Open `unified.html` und `selfasserted.html` , und Ersetzen Sie alle Instanzen von `https://localhost` mit der URL des eigenen Container, die in den vorherigen Schritten notiert. Den absoluten Pfad der HTML-Dateien verwenden, da in diesem Fall der HTML-Code von Azure AD Domain bedient werden `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Laden Sie die Beispieldateien

In demselben Repository Entpacken `B2CAzureStorageClient.zip` und die `B2CAzureStorageClient.exe` in Datei. Dieses Programm wird einfach die Dateien im Verzeichnis, die an das Speicherkonto hochladen und CORS Zugriff für diese Dateien aktivieren. Wenn Sie die obigen Schritte befolgt haben, werden die HTML- und CSS-Dateien das Speicherkonto jetzt darauf. Beachten Sie, dass das Speicherkonto ist der Teil, der vor `blob.core.windows.net`; z. B. `contoso`. Sie können überprüfen, ob der Inhalt ordnungsgemäß hochgeladen wurde von zugreifen möchten `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` in einem Browser. Auch verwenden Sie [http://test-cors.org/](http://test-cors.org/) , um sicherzustellen, dass der Inhalt nun CORS aktiviert ist. (Suchen Sie nach "XHR Status: 200" im Ergebnis.)

### <a name="customize-your-policy-again"></a>Die Richtlinie erneut anpassen

Beispielinhalte eigene Storage-Konto hochgeladen haben, müssen Sie die Registrierungsrichtlinie darauf verweist bearbeiten. Wiederholen Sie die Schritte im Abschnitt ["Anpassen der Richtlinie"](#customize-your-policy) oben mit eigenen Speicherkonto URLs. Zum Beispiel der Speicherort der Ihre `unified.html` Datei `<url-of-your-container>/wingtip/unified.html`.

Jetzt können Sie die Richtlinie erneut ausführen die Schaltfläche **Jetzt ausführen** oder Ihre eigene Anwendung. Das Ergebnis sieht fast identisch – Sie dasselbe Beispiel HTML und CSS in beiden Fällen. Jedoch Ihre Richtlinien verweisen jetzt eine eigene Instanz der Azure BLOB-Speicher, und Sie können bearbeiten und Hochladen von Dateien als Sie bitte. Weitere Informationen zum Anpassen von HTML und CSS finden Sie im [Hauptartikel Benutzeroberfläche anpassen](active-directory-b2c-reference-ui-customization.md).
