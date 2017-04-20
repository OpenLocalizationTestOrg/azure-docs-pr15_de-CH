<properties
    pageTitle="Azure AD und: führen Entwickler | Microsoft Azure"
    description="Diese Artikel enthält für IT-Experten geschrieben Richtlinien für Active Directory Azure Applications integrieren."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-and-applications-develop-line-of-business-apps"></a>Azure AD und: Branchen apps zu entwickeln

Dieses Handbuch bietet eine Übersicht über die Anwendungsentwicklung Line-of-Business (LoB) für Azure Active Directory (AD). Die Zielgruppe sind globale Administratoren Active Directory-Office 365.

## <a name="overview"></a>Übersicht

Erstellen von Azure AD integriert ermöglicht Benutzern in der Organisation einmaliges Anmelden mit Office 365. Müssen die Anwendung in Azure AD gibt der Authentifizierungsrichtlinie für die Anwendung steuern. Weitere Informationen zu bedingten Zugriff und zum Schutz von apps mehrstufige Authentifizierung (MFA) finden Sie unter [Zugriffsregeln konfigurieren](active-directory-conditional-access-azuread-connected-apps.md).

Registrieren Sie die Anwendung von Azure Active Directory. Registrieren der Anwendung bedeutet, dass Entwickler Azure AD Benutzer zu authentifizieren und Zugriff auf Benutzerressourcen wie e-Mail, Kalender und Dokumente anfordern.

Jedes Mitglied des Verzeichnisses (nicht Gäste) kann eine Anwendung als *ein Anwendungsobjekt erstellen*registrieren.

Registrieren einer Anwendung kann alle Benutzer folgende Aktionen ausführen:

- Abrufen Sie eine Identität für ihre Anwendung Azure AD erkennt
- Erhalten Sie ein oder mehrere Kennwörter/Schlüssel, mit denen die Anwendung sich AD authentifizieren
- Marke der Anwendung in Azure-Portal mit einem benutzerdefinierten Namen, Logo.
- Ihre app Azure AD Autorisierungsfunktionen zuweisen einschließlich:
  - Rollenbasierte Zugriffskontrolle (RBAC)
  - Azure Active Directory wie oAuth autorisierungsserver (Sicherheit eine API, die von der Anwendung verfügbar gemacht)

- Deklarieren erforderliche Berechtigungen für die Anwendung erwartungsgemäß einschließlich:-Berechtigungen (globale Administratoren). Beispiel: Rollenmitgliedschaft in anderen Azure AD-Anwendung oder Rollenmitgliedschaft gegenüber einer Azure Ressource, Ressourcengruppe, oder Abonnement - Stellvertreter (alle Benutzer). Beispiel: Azure AD anmelden und Lesen Profil


> [AZURE.NOTE]Standardmäßig kann alle Mitglieder eine Anwendung registrieren. Informationen zum Einschränken von Berechtigungen für die Registrierung der Anwendung auf bestimmte Mitglieder sehen Sie [wie Applikationen Azure AD hinzugefügt werden](active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).

Hier ist was Sie globaler Administrator müssen zur Unterstützung von Entwicklern, die ihre Anwendung für die Produktion bereit zu machen:

- Konfigurieren von Zugriffsregeln (Access Policy/MFA)
- Die Anwendung erfordert von Benutzerrechten und Zuweisen von Benutzern zu konfigurieren
- Die Standard-Benutzeroberfläche Zustimmung unterdrücken

## <a name="configure-access-rules"></a>Konfigurieren von Zugriffsregeln

Konfigurieren von Regeln für die SaaS-apps pro Anwendung. Sie können z. B. MFA erforderlich oder gestattet Benutzern den Zugriff auf vertrauenswürdige Netzwerke. Details hierzu stehen im Dokument [Zugriffsregeln konfigurieren](active-directory-conditional-access-azuread-connected-apps.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Die Anwendung erfordert von Benutzerrechten und Zuweisen von Benutzern zu konfigurieren

Standardmäßig können Benutzer Programme ohne zugreifen. Wenn die Anwendung Funktionen bereitstellt oder die Anwendung Zugriff auf des Benutzers angezeigt werden soll, Benutzerrechten benötigen Sie.

[Die von Benutzerrechten](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Wenn Sie Abonnent von Azure AD Premium oder Enterprise Mobility Suite (EMS) sind, empfehlen wir Gruppen. Zuweisen von Gruppen für die Anwendung können Sie laufende Verwaltung der Besitzer der Gruppe delegieren. Erstellen die Gruppe, oder bitten Sie die Verantwortlichen in Ihrer Organisation mit der Einrichtung der Gruppe erstellen.

[Zuweisen von Benutzern zu einer Anwendung](active-directory-applications-guiding-developers-assigning-users.md)  
[Zuweisen von Gruppen zu einer Anwendung](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Zustimmung des Benutzers unterdrücken

Standardmäßig wird jeder Benutzer durch ein Zustimmung anmelden. Die Zustimmung Erfahrung Fragen Benutzer Berechtigungen für eine Anwendung sein für Benutzer verwirrend, die mit dieser Entscheidung nicht vertraut sind.

Handelt, denen Sie vertrauen, können Sie die Benutzeroberfläche der Anwendung für Ihre Organisation zu vereinfachen.

Weitere Informationen über Zustimmung des Benutzers und die Zustimmung in Azure finden Sie [Applikationen Azure Active Directory integrieren](active-directory-integrating-applications.md).

##<a name="related-articles"></a>Verwandte Artikel

- [Aktivieren der sicheren Remotezugriff auf lokale Anwendung mit Azure AD-Anwendungsproxy](active-directory-application-proxy-get-started.md)
- [Vorschau für SaaS-Apps Azure Zugangskontrolle](active-directory-conditional-access-azuread-connected-apps.md)
- [Verwalten des Zugriffs auf apps mit Azure](active-directory-managing-access-to-apps.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
