<properties
    pageTitle="Azure Active Directory B2C: Häufig gestellte Fragen | Microsoft Azure"
    description="Häufig gestellte Fragen zur Azure Active Directory B2C"
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
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: häufig gestellte Fragen

Diese Seite beantwortet häufig gestellte Fragen zur Azure Active Directory (Azure AD) B2C. Halten Sie nachsehen, ob Updates.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Kann ich Azure AD B2C-Funktionen in vorhandenen, Mitarbeiter-basierte Azure AD Mandanten verwenden?

Derzeit werden nicht Azure AD B2C-Funktionen in Ihre vorhandene Azure AD-Mandanten aktiviert. Empfohlen, einen separaten Mieter erstellen um Azure AD B2C-Features verwenden, um Ihre Kunden zu verwalten.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Können ich Azure AD B2C sozialen Anmeldung (Facebook und Google) in Office 365?

Azure AD B2C kann mit Microsoft Office 365 verwendet werden. Im Allgemeinen kann es für die Authentifizierung zu SaaS apps (Office 365 Salesforce, Arbeitstag usw.) verwendet werden. Identitäts- und Zugriffsmanagement für kundenorientierter und Dienste bietet, und gilt nicht für Mitarbeiter oder Partner-Szenarien.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Was sind lokale Konten in Azure AD B2C? Wie sie Geschäfts-oder in Azure AD sind?

In Azure AD-Mandanten jeder Benutzer in der Mieter (außer Benutzern mit vorhandenen Microsoft-Konten) in eine e-Mail-Adresse des Formulars signiert `<xyz>@<tenant domain>`, wobei `<tenant domain>` eines verifizierten Domänen Pächter bzw. den ursprünglichen `<...>.onmicrosoft.com` Domäne. Dieser Kontotyp ist ein Arbeits- oder Schulcomputer Konto.

Ein Azure AD B2C-Mandanten die meisten apps Benutzer jede beliebige e-Mail-Adresse anmelden möchten (z. B. joe@comcast.net, bob@gmail.com, sarah@contoso.com, oder jim@live.com). Kontoart ist ein lokales Konto. Heute unterstützen wir auch beliebigen Benutzernamen (einfach Zeichenfolgen) als lokale Konten (z. B. Joe, Bob, Sarah und Jim). Sie können eines dieser beiden lokalen Konto in Azure AD B2C-Dienst.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Welche Identitätsanbieter unterstützen Sie jetzt? Möchten welche Sie in Zukunft zu unterstützen?

Wir unterstützen derzeit Facebook, Google, LinkedIn und Amazon. Unterstützung für andere beliebte social Identitätsanbieter basierend auf wird hinzugefügt.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Kann Bereiche zum Sammeln von Informationen über Verbraucher aus verschiedenen sozialen Identitätsanbieter konfigurieren?

Nein, diese Funktion ist in Planung. Standardbereiche für unsere unterstützte Gruppe von sozialen Identitätsanbieter verwendet werden:

- Facebook: e-Mail
- Google: e-Mail
- Microsoft-Konto: Openid e-Mail-Profil
- Amazon: Profil
- LinkedIn: R_emailaddress r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Muss meine Anwendung in Azure ausgeführt werden, arbeiten mit Azure AD B2C?

Nein, können Sie die Anwendung (in der Cloud oder lokal) hosten. Azure AD B2C interagieren muss ist die Möglichkeit zum Senden und Empfangen von HTTP-Anfragen auf öffentlich zugängliche Endpunkte.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Ich habe mehrere Azure AD B2C-Mandanten. Wie kann ich sie in Azure-Portal verwalten?

Jede Azure AD B2C Mieter hat eigene B2C Features Blade Azure-Portal. Siehe [Azure AD B2C: Registrieren Sie die Anwendung](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) um zu erfahren, wie Sie einen bestimmten Mandanten B2C Features Blade Azure-Portal navigieren können. Wechseln zwischen Azure AD B2C-Verzeichnisse der Azure-Portal halten nicht B2C Features Blade bei den meisten Browsern öffnen.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Wie passe Überprüfung e-Mails (Inhalt und der "von:" Feld) von Azure AD B2C gesendet?

[Unternehmen branding-Funktion](../active-directory/active-directory-add-company-branding.md) können Sie den Inhalt Überprüfung e-Mails anpassen. Insbesondere können diese beiden Elemente der e-Mail angepasst werden:

- **Banner Logo**: unten rechts angezeigt.
- **Hintergrundfarbe**: oben.

    ![Screenshot der angepassten e-Mail](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

E-Mail-Signatur enthält B2C Mieter Namen, den Sie beim Erstellen von B2C-Mandanten. Sie können den Namen anhand der folgenden Schritte ändern:

- Melden Sie bei [Azure-Verwaltungsportal](https://manage.windowsazure.com/) als Abonnement-Administrator an.
- Navigieren Sie zu Ihrem B2C-Mandanten.
- Klicken Sie auf die Registerkarte **Konfigurieren** .
- Ändern Sie den **Namen** im Abschnitt **Directory-Eigenschaften** .
- Klicken Sie am unteren Rand der Seite **Speichern** .

Derzeit besteht keine Möglichkeit zum Ändern der "von:" auf e-Mail. Wenn Sie in dieser Funktion und vollständig den Hauptteil der e-Mail anpassen möchten, stimmen Sie für die [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails)-Funktion.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Wie kann ich meine vorhandenen Benutzernamen, Kennwörter und Profile aus einer Datenbank zu Azure AD B2C migrieren?

Azure AD Graph-API können Sie das Migrationstool schreiben. [Graph-API Beispiel](active-directory-b2c-devquickstarts-graph-dotnet.md) Details anzeigen Wir bieten verschiedene Optionen und Tools Out-of-the-Box in der Zukunft.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Welche Kennwortrichtlinie für lokale Konten in Azure AD B2C verwendet wird?

Azure AD B2C-Kennwortrichtlinie für lokale Konten basiert auf der Richtlinie für Azure AD. Azure AD B2C der Anmeldung, Anmeldung oder Anmeldename und Kennwort zurücksetzen Richtlinien verwendet "Kennwort" Stärke und Kennwörter nicht ablaufen. Lesen Sie die [Kennwortrichtlinie Azure AD](https://msdn.microsoft.com/library/azure/jj943764.aspx) Weitere Informationen.

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kann ich Azure AD Connect Kundenidentitäten migrieren, die auf meinem lokalen Active Directory Azure AD B2C verwenden?

Nein, Azure AD verbinden soll nicht mit Azure AD B2C arbeiten. Wir bieten verschiedene Optionen und Tools Out-of-the-Box in der Zukunft.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Funktioniert Azure AD B2C mit Systemen wie Microsoft Dynamics CRM

Derzeit nicht. Diese Systeme ist geplant.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Azure AD B2C wird mit SharePoint lokalen 2016 oder früher?

Derzeit nicht. Azure AD B2C verfügen keine Unterstützung für SAML 1.1-Token, Portale und e-Commerce-Applikationen auf lokalen SharePoint. Beachten Sie, dass Azure AD B2C SharePoint externe Partner-sharing-Szenario nicht. [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) stattdessen anzeigen

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Verwende Azure AD B2C oder B2B ich externe Identitäten verwalten?

Dieser Artikel über [externe Identitäten](../active-directory/active-directory-b2b-compare-external-identities.md) mehr über externe Identität Szenarien die entsprechenden Funktionen zuweisen.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Welche Berichte und Überwachungsfunktionen bietet Azure AD B2C? Stimmen sie in Azure AD Premium?

Leider unterstützt Azure AD B2C dieselben Berichte als Azure AD nicht. Azure AD B2C werden grundlegende Berichterstattung und Überwachung APIs bald veröffentlicht.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Können die Benutzeroberfläche von Azure AD B2C Seiten lokalisieren? Welche Sprachen werden unterstützt?

Derzeit ist Azure AD B2C nur für Englisch optimiert. Wir planen Lokalisierungsfeatures so schnell wie möglich bereitstellen.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Kann ich meine eigene URLs auf Meine Anmeldung und Anmeldung verwenden, die von Azure AD B2C bereitgestellt? Zum Beispiel kann ich die URL login.microsoftonline.com auf login.contoso.com ändern?

Derzeit nicht. Dieses Feature ist geplant. Beachten Sie, dass die Domäne auf der Registerkarte **Domänen** Ihrem Mandanten klassischen Azure-Portal überprüfen dies nicht.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Wie löschen Sie Azure AD B2C Mandanten

Gehen Sie Ihre Azure AD B2C-Mandanten löschen

- Gehen Sie [zu B2C-Features Blade](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) navigieren Azure-Portal.
- Zu Blades **Applikationen** **Identitätsprovider** und **alle Richtlinien** und löschen Sie alle Einträge in jedem.
- Jetzt melden Sie [Azure-Verwaltungsportal](https://manage.windowsazure.com/) als Abonnement-Administrator an. (Dies ist die gleiche Arbeit oder schulkonto oder das gleiche Microsoft-Konto für Azure verwendet.)
- Navigieren Sie zu der Active Directory-Erweiterung auf der linken Seite und auf Ihrem B2C-Mandanten.
- Klicken Sie auf die Registerkarte **Benutzer** .
- Wählen Sie jeden Benutzer wiederum (Exclude Benutzer derzeit als, d. h. Abonnement-Administrator angemeldet sind). Klicken Sie am unteren Rand der Seite **Löschen** , und klicken Sie auf **Ja** .
- Klicken Sie auf die Registerkarte **Applications** .
- Wählen Sie **Applikationen Mein Unternehmen** im Feld Dropdown-Liste **Anzeigen** und klicken Sie auf das Häkchen.
- Eine Anwendung namens **b2c-Extensions-app** unten sehen. Klicken Sie am unteren Rand der Seite **Löschen** , und klicken Sie auf **Ja** .
- Die Active Directory-Erweiterung wieder navigieren Sie, und wählen Sie Ihrem B2C-Mandanten.
- Klicken Sie am unteren Rand der Seite **Löschen** . Anleitung auf dem Bildschirm, um den Vorgang abzuschließen.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kann ich Azure AD B2C als Teil des Enterprise Mobility Suite?

Nein, Azure AD B2C ist nutzungsbasierte Azure Service und ist nicht Teil des Enterprise Mobility Suite.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Wie melde ich Probleme mit Azure AD B2C?

Finden Sie [Anfragen für Azure Active Directory B2C](active-directory-b2c-support.md).

## <a name="more-information"></a>Weitere Informationen

Sie möchten auch aktuelle [Service Einschränkungen, Beschränkungen und Einschränkungen](active-directory-b2c-limitations.md)prüfen.
