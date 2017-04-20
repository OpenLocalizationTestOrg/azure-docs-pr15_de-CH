<properties
   pageTitle="Azure Active Directory B2B-Zusammenarbeit | Microsoft Azure"
   description="Azure Active Directory B2B-Zusammenarbeit ermöglicht Geschäftspartnern Zugriff auf Ihre Unternehmen-Applikationen, mit der Benutzer durch einzelne Azure AD dargestellt Konto"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B-Zusammenarbeit

Azure Active Directory (Azure AD) B2B-Zusammenarbeit ermöglicht Unternehmen Anwendung Partner verwaltete Identitäten Zugriff ermöglichen. Unternehmensübergreifende Beziehungen zu erstellen und Autorisieren von Benutzern von Partnerunternehmen auf die Ressourcen zugreifen. Weniger komplex, da jedes einmal Azure Active Directory Verbindung und jeder Benutzer mit einem Azure AD-Konto. Sicherheit wird erhöht, wenn Geschäftspartner ihre Konten in Azure AD verwalten, da beim Partnerbenutzer in ihrer Organisation beendet und unerwünschten Zugriff über Gruppenmitgliedschaft in interne Verzeichnisse verhindert Zugriff gesperrt wird. Für Geschäftspartner, die bereits Azure AD nicht hat B2B-Zusammenarbeit optimierte Anmeldeprozess Geschäftspartnern Azure AD-Konten anzubieten.

-   Ihre Geschäftspartner verwenden ihre eigenen Anmeldeinformationen, befreit von externen Partnerverzeichnis verwalten und muss den Zugriff entfernen, wenn Benutzer die Organisation verlassen.

-   Zugriff verwalten auf Ihre apps unabhängig von Ihren Geschäftspartner Konto Lebenszyklus. Dies bedeutet beispielsweise, dass Zugriff widerrufen werden können, ohne die IT-Abteilung des Geschäftspartners keine Fragen.

## <a name="capabilities"></a>Funktionen

B2B-Zusammenarbeit vereinfacht das Management und erhöht die Sicherheit der Partnerzugriff auf Unternehmensressourcen z. B. SaaS-apps, z. B. Office 365, Salesforce, Azure Services und jedes Mobile cloud und lokalen Ansprüche unterstützende Anwendung. B2B-Partnern ermöglicht ihre eigenen Konten und Unternehmen können Sicherheitsrichtlinien auf Partner.

Azure Active Directory B2B Zusammenarbeit einfach zu konfigurieren ist vereinfacht für Partner aller Größenordnungen, auch wenn sie ihre eigenen Azure Active Directory über einen Prozess e-Mail überprüft. Es steht Ihnen keine externen Verzeichnisse oder Partner Föderation Konfigurationen verwalten.

## <a name="b2b-collaboration-process"></a>B2B-Zusammenarbeit-Prozess

1. Azure AD B2B-Zusammenarbeit ermöglicht ein Unternehmensadministrator zu laden und eine Reihe von externen Benutzern durch Hochladen einer Wertedatei durch Trennzeichen getrennte (CSV) nicht mehr als 2000 Zeilen B2B-Collaboration-Portal autorisieren.

  ![CSV-Datei hochladen Dialogfeld](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Das Portal senden Einladungen zu dieser externen Benutzer.

3. Der eingeladene Benutzer ein Arbeit-Konto mit Microsoft (verwaltet in Azure AD) anmelden, oder ein neues Konto für die Arbeit in Azure AD abgerufen.

4. Nach der Anmeldung wird der Benutzer zur app umgeleitet, die für sie freigegeben wurde.

Einladung an Consumer e-Mail-Adressen (z. B. Gmail oder [*comcast.net*](http://comcast.net/)) werden derzeit nicht unterstützt.

Mehr über die Funktionsweise von B2B-Zusammenarbeit finden Sie [in diesem Video](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Nächste Schritte
Durchsuchen Sie unsere Artikel Azure AD B2B-Zusammenarbeit.

- [Was ist Azure AD B2B-Zusammenarbeit?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Funktionsweise](active-directory-b2b-how-it-works.md)
- [Ausführliche Anleitung](active-directory-b2b-detailed-walkthrough.md)
- [Übersicht der CSV-Datei](active-directory-b2b-references-csv-file-format.md)
- [Externe Benutzer token format](active-directory-b2b-references-external-user-token-format.md)
- [Externe Benutzer objektänderungen](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Aktuelle Vorschau Grenzen](active-directory-b2b-current-preview-limitations.md)
- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
