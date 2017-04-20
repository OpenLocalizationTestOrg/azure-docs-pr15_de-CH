<properties
   pageTitle="Azure AD B2B Zusammenarbeit Vorschau: Funktionsweise | Microsoft Azure"
   description="Beschreibt die Azure Active Directory B2B-Zusammenarbeit unternehmensübergreifende Beziehungen Unterstützung von Business Partner selektiv corporate Anwendung zugreifen"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure AD B2B Zusammenarbeit Vorschau: Funktionsweise
Azure AD B2B-Zusammenarbeit basiert auf Einladung und Modell einlösen. Geben Sie die e-Mail-Adressen der Vertragsparteien zu zusammen mit der Anwendung arbeiten Sie verwenden sollen. Azure AD sendet ihnen eine Einladung per e-Mail mit einem Link. Partner-Benutzer den Hyperlink und wird aufgefordert ihre Azure Konto oder Zeichen für eine neue Azure Anzeige berücksichtigen.

1. Der Administrator lädt Partner [strukturierte CSV-Datei](active-directory-b2b-references-csv-file-format.md) mithilfe des Azure-Portals hochladen.
2. Die Portalwebsite sendet einladen e-Mails an diese Partnerbenutzer
3. Partnerbenutzer klicken Sie auf den Link in der e-Mail und werden aufgefordert, sich mit ihren Anmeldeinformationen arbeiten (Wenn sie bereits im Azure sind) oder als Azure AD B2B Zusammenarbeit Benutzer anmelden.
4. Partnerbenutzer umgeleitet werden an die Anwendung, denen sie eingeladen wurden, in dem sie jetzt zugreifen.

## <a name="directory-operations"></a>Directory-Vorgänge
Partnerbenutzer vorhanden in Ihrem Azure AD als externe Benutzer. Das bedeutet, Ihr Administrator Lizenzen bereitstellen, Gruppenmitgliedschaften zuweisen und weitere gewähren Zugriff auf Unternehmens-apps durch Azure-Portal oder Azure PowerShell wie für Benutzer in Ihrem Unternehmen kann.

Während eine bezahlte Anzeige Azure Abonnement (Basic oder Premium) ist nicht erforderlich, Azure AD B2B Mieter verwenden, die ein bezahltes Abonnement Azure AD (Basic oder Premium) erhalten die folgenden zusätzlichen Vorteile:

 - Administratoren können Gruppen apps zuweisen für einfachere Verwaltung der eingeladene Benutzer.
 - Admin Mieter branding verwendet, um die Einladungen zu kennzeichnen und Rückzahlung Erfahrung, mehr Kontext eingeladen Partnerbenutzer.

## <a name="related-articles"></a>Verwandte Artikel
 Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit

 - [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Ausführliche Anleitung](active-directory-b2b-detailed-walkthrough.md)
 - [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
 - [Externe Benutzer token format](active-directory-b2b-references-external-user-token-format.md)
 - [Externe Benutzer objektänderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Aktuelle Vorschau Grenzen](active-directory-b2b-current-preview-limitations.md)
 - [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
