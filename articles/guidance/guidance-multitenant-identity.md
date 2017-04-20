<properties
   pageTitle="Identitätsmanagement für mandantenfähigen Applikationen | Microsoft Azure"
   description="Best Practices für Authentifizierung, Autorisierung und Identität Management mandantenfähigen Apps."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Identitätsmanagement für mandantenfähigen Applikationen in Microsoft Azure

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Dieser Serie beschreibt Vorgehensweisen für Tenancy, Azure AD für Authentifizierung und Management.

Beim Erstellen einer mehrinstanzenfähigen Anwendung ist eines der ersten Benutzeridentitäten, weil jetzt jedes Mieter Benutzers verwalten. Zum Beispiel:

- Benutzer mit ihrer Organisation Anmeldeinformationen anmelden.
- Benutzer müssen Zugriff auf ihre Daten jedoch nicht Daten anderen Mandanten.
- Eine Organisation kann für die Anwendung registrieren und Membern Anwendungsrollen zuweisen.

Azure Active Directory (Azure AD) hat einige großartige Funktionen, die diese Szenarios unterstützen.

Zu dieser Artikelreihe erstellt wir auch eine vollständige, [umfassende - Implementierung] [ tailspin] mandantenfähigen App. Der Artikel entsprechend Gelernte beim Erstellen der Anwendung. Zunächst mit der Anwendung finden Sie unter [Umfragen läuft](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Hier ist die Liste der Artikel in dieser Reihe:

- [Einführung in Identitätsmanagement für mandantenfähigen Applikationen](guidance-multitenant-identity-intro.md)
- [Über Umfragen Tailspin-Anwendung](guidance-multitenant-identity-tailspin.md)
- [Authentifizierung mit Azure AD und OpenID](guidance-multitenant-identity-authenticate.md)
- [Arbeiten mit Identitäten Behauptung basiert](guidance-multitenant-identity-claims.md)
- [Anmeldung und Mieter Onboarding](guidance-multitenant-identity-signup.md)
- [Anwendungsrollen](guidance-multitenant-identity-app-roles.md)
- [Rollen und ressourcenbasierte Autorisierung](guidance-multitenant-identity-authorize.md)
- [Absichern von Back-End-Web-API](guidance-multitenant-identity-web-api.md)
- [Zugriffstoken OAuth2 Zwischenspeichern](guidance-multitenant-identity-token-cache.md)
- [Föderation mit einem Kunden AD FS mandantenfähigen Apps in Azure](guidance-multitenant-identity-adfs.md)
- [Verwendung von Client-Assertion Zugriffstoken von Azure AD](guidance-multitenant-identity-client-assertion.md)
- [Key Vault verwenden Anwendung Kennwörter zu](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
