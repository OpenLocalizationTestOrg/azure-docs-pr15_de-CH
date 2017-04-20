<properties
   pageTitle="Externe Benutzer objektänderungen Azure Active Directory B2B Zusammenarbeit Vorschau | Microsoft Azure"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure AD B2B Zusammenarbeit Vorschau: externe Benutzer objektänderungen

Jeder Benutzer in Azure AD-Verzeichnis wird von einem Objekt dargestellt. Das Benutzerobjekt in Azure AD wird Änderungen in verschiedenen Phasen des B2B-Zusammenarbeit einladen-einlösen Fluss. Der Benutzer Objekt darstellen der Partnerbenutzer im Verzeichnis Attribute besitzt am einlösen Zeit klickt der Partnerbenutzer auf den Link in der Einladung per e-Mail. Insbesondere:

- Die Attribute **SignInName** und **AltSecId** werden aufgefüllt
- Ändert das **DisplayName** -Attribut von Benutzerprinzipalnamen (user_fabrikam.com#EXT#@contoso.com) , die ein(user@fabrikam.com)

Verfolgen diese Attribute in Azure AD bei der Problembehandlung hilfreich, und zwar unabhängig davon, ob ein Partner-Benutzer ihre Einladung zur Zusammenarbeit von B2B eingelöst.

## <a name="related-articles"></a>Verwandte Artikel
Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit:

- [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funktionsweise](active-directory-b2b-how-it-works.md)
- [Ausführliche Anleitung](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Externe Benutzer token format](active-directory-b2b-references-external-user-token-format.md)
- [Aktuelle Vorschau Grenzen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
