<properties
   pageTitle="Aktuelle Vorschau Grenzen für Azure Active Directory B2B-Zusammenarbeit | Microsoft Azure"
   description="Azure Active Directory B2B unterstützt die unternehmensübergreifende Beziehungen Business Partner corporate Anwendung selektiv auf"
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

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure AD B2B Zusammenarbeit Vorschau: aktuelle Vorschau Grenzen

- Mehrstufige Authentifizierung (MFA) für externe Benutzer nicht unterstützt. Beispielsweise können nicht Wenn Contoso MFA, jedoch keine Partner Org, dann Partner Org Benutzern werden MFA B2B-Zusammenarbeit erteilt.
- Lädt sind nur über CSV. einzelne lädt und API-Zugriff werden nicht unterstützt.
- Azure AD globale Administratoren können nur CSV-Dateien hochladen.
- Einladung an Consumer e-Mail-Adressen (z. B. hotmail.com, Gmail.com oder comcast.net) werden derzeit nicht unterstützt.
- Externe Benutzerzugriff auf lokale Anwendung nicht getestet.
- Externe Benutzer sind nicht automatisch bereinigt, wenn der jeweilige Benutzer aus dem Verzeichnis gelöscht wird.
- Einladung an Verteilerlisten werden nicht unterstützt.
- Maximal 2.000 Datensätze kann über CSV hochgeladen werden.

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit:

- [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funktionsweise](active-directory-b2b-how-it-works.md)
- [Ausführliche Anleitung](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Externe Benutzer token format](active-directory-b2b-references-external-user-token-format.md)
- [Externe Benutzer objektänderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
