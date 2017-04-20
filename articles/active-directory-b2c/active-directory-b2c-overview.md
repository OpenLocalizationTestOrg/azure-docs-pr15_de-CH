<properties
    pageTitle="Azure Active Directory B2C: Übersicht | Microsoft Azure"
    description="Kundenorientierter Anwendungsentwicklung mit Azure Active Directory B2C"
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
    ms.topic="hero-article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-sign-up-and-sign-in-consumers-in-your-applications"></a>Azure Active Directory B2C: Anmelden und Verbraucher in der Anwendung anmelden

Azure Active Directory B2C ist eine umfassende Cloud Identitätsmanagement-Lösung für Ihre kundenorientierter und -Dienste. Es ist ein hochgradig verfügbaren globalen Dienst, der Hunderte von Millionen von Kundenidentitäten skaliert. Erstellt auf einer sicheren Plattform Unternehmen hält Azure Active Directory B2C Anwendung, Ihr Unternehmen und Ihre Kunden geschützt.

In der Vergangenheit Anwendungsentwickler und Kunden in ihre Programme anmelden würde ihre eigenen Code geschrieben haben. Und sie würden lokalen Datenbanken oder Systemen zum Speichern von Benutzernamen und Kennwörtern. Azure Active Directory B2C bietet Entwicklern ein besseres Consumer Identitätsmanagement in ihre Anwendung mit Hilfe einer sicheren, standardisierten Plattform und umfassende extensible Richtlinien integrieren. Bei Verwendung von Azure Active Directory B2C können Ihre Kunden für die Anwendung ihrer bestehenden sozialen Konten (Facebook, Google, Amazon, LinkedIn) oder neue Anmeldeinformationen (e-Mail-Adresse und Kennwort oder Benutzername und Kennwort), anmelden Wir rufen die letzteren "lokalen Konten."

## <a name="get-started"></a>Erste Schritte

Zum Erstellen einer Anwendung, die Consumer registrieren und anmelden, Sie akzeptiert werden müssen zunächst die Anwendung mit einer Azure Active Directory B2C registrieren Mieter. Rufen Sie eigene Mieter mithilfe der Schritte in [Erstellen einer Azure AD B2C-Mandanten ab](active-directory-b2c-get-started.md).

Sie können Ihre Anwendung mit den Azure Active Directory B2C-Dienst Nachrichten, senden über [OAuth 2.0](active-directory-b2c-reference-protocols.md#oauth2-authorization-code-flow) oder [Open ID verbinden](active-directory-b2c-reference-protocols.md#openid-connect-sign-in-flow), oder Sie arbeiten mit Bibliotheken schreiben. Wählen Sie Ihre bevorzugte Plattform in der folgenden Tabelle und loslegen.

[AZURE.INCLUDE [active-directory-b2c-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

## <a name="whats-new"></a>Neuigkeiten

Hier häufig um Änderungen Azure Active Directory B2C lernen. Wir werden auch tweet über Updates mit @AzureAD.

- Erfahren Sie über unsere [extensible Richtlinienframework](active-directory-b2c-reference-policies.md) und zu den Richtlinien, die Sie erstellen und in der Anwendung verwenden.
- Lesezeichen Sie- [Dienst Blog](https://blogs.msdn.microsoft.com/azureadb2c/) für Benachrichtigungen auf geringfügige Probleme, Updates, Status und Gegenmaßnahmen. Weiterhin [Azure Status Dashboard](https://azure.microsoft.com/status/) sowie.
- Aktuellen [Dienst eingeschränkt, Einschränkungen und Einschränkungen](active-directory-b2c-limitations.md).
- Schließlich ein [Codebeispiel](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c) mit Azure AD B2C und ASP.NET Core.

## <a name="how-to-articles"></a>Gewusst wie-Artikel

Informationen Sie zum Verwenden von Azure Active Directory B2C-Besonderheiten:

- Konfigurieren Sie [Facebook](active-directory-b2c-setup-fb-app.md), [Google](active-directory-b2c-setup-goog-app.md), [Microsoft-Konto](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md)und [LinkedIn](active-directory-b2c-setup-li-app.md) Konten für die Verwendung in Ihrer Anwendung kundenorientierter.
- [Benutzerdefinierte Attribute verwenden, Informationen über Ihre Kunden sammeln](active-directory-b2c-reference-custom-attr.md).
- [Azure mehrstufige Authentifizierung in Clientanwendungen kundenorientierter aktivieren](active-directory-b2c-reference-mfa.md).
- [Einrichten von Self-service für Ihre Kunden Zurücksetzen des Kennworts](active-directory-b2c-reference-sspr.md).
- [Das Erscheinungsbild der Anmeldung, Anmeldung und andere Consumer - Seiten anpassen](active-directory-b2c-reference-ui-customization.md) , die von Azure Active Directory B2C bereitgestellt.
- [Verwenden der Azure Active Directory Graph-API programmgesteuert erstellen, lesen, aktualisieren und Löschen von Verbrauchern](active-directory-b2c-devquickstarts-graph-dotnet.md) in Ihrem Azure Active Directory B2C-Mandanten.

## <a name="next-steps"></a>Nächste Schritte

Diese Links werden den Dienst eingehend untersuchen:

- [Azure Active Directory B2C Preisinformationen](https://azure.microsoft.com/pricing/details/active-directory-b2c/)anzeigen
- Hilfe zum Stapelüberlauf mithilfe von [Azure Active Directory](http://stackoverflow.com/questions/tagged/azure-active-directory) oder [adal](http://stackoverflow.com/questions/tagged/adal) Tags.
- Geben Sie uns Ihre Meinung mit [Benutzer Stimme](https://feedback.azure.com/forums/169401-azure-active-directory/)– wir möchten sie gerne! Verwenden Sie die Phrase "AzureADB2C:" im Titel Ihres Beitrags es finden.
- Überprüfen der [Azure AD B2C-Protokoll Verweis](active-directory-b2c-reference-protocols.md).
- [Azure AD B2C Tokenverweis](active-directory-b2c-reference-tokens.md)überprüfen.
- [Azure Active Directory B2C FAQs](active-directory-b2c-faqs.md)zu lesen.
- [Anfragen für Azure Active Directory B2C Datei](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen Benachrichtigungen Sicherheitsvorfälle auftreten [dieser Seite](https://technet.microsoft.com/security/dd252948) und beratende Sicherheitshinweise abonnieren.
