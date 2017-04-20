<properties
   pageTitle="Externe Benutzer token Format Azure Active Directory B2B Zusammenarbeit Vorschau | Microsoft Azure"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure AD B2B Zusammenarbeit Vorschau: externe Benutzer token Format

Die Ansprüche Token beschriebenen standardmäßigen Azure AD in Artikel [unterstützt und Ansprüche](active-directory-token-and-claims.md) auf azure.microsoft.com.

Die Ansprüche, die für einen authentifizierten B2B Zusammenarbeit externe Benutzer lauten wie folgt:<br/>
- **OID:** die Objekt-ID der Ressource Mieter<br/>
- **TID**: Mieter ID Ressource Mieter<br/>
- **Aussteller**: Dies ist die Ressource Mieter<br/>
- **IDP**: Dies ist privat Mieter des Benutzers<br/>
- **AltSecId**: Dies ist die alternative Sicherheits-ID, die für Sie<br/>

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit:

- [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funktionsweise](active-directory-b2b-how-it-works.md)
- [Ausführliche Anleitung](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Externe Benutzer objektänderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuelle Vorschau Grenzen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
