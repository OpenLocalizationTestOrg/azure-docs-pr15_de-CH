<properties
  pageTitle="Verwalten des Zugriffs auf apps mit Azure AD |  Microsoft Azure"
  description="Beschreibt, wie Active Directory Azure ermöglicht Organisationen die apps angeben, auf die jeder Benutzer zugreifen kann."
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>Verwalten des Zugriffs auf apps

Laufende Verwaltung Verwendung Evaluierung und Berichterstattung weiterhin eine Herausforderung sein, nachdem eine Anwendung Ihre Organisation Identität integriert ist. In vielen Fällen müssen IT-Administratoren oder Helpdesk laufende aktiv Verwalten des Zugriffs auf Ihre apps. Manchmal erfolgt die Zuordnung durch Allgemein oder Abteilung IT-Team. Häufig die Zuordnung Entscheidung soll an Entscheidungsträger, ihre Zustimmung vor IT macht delegiert werden die Zuordnung.  Andere Unternehmen investieren in Integration mit einem vorhandenen automatisierten Identitäts- und -System, wie rollenbasierte Zugriffskontrolle (RBAC) oder Attribut-basierte Zugriffskontrolle (ABAC). Integration und Regel Entwicklung sind spezieller und kostspieliger. Überwachung oder Berichterstattung entweder Managements ist eine eigene separate kostenintensive und komplexe Investition.

## <a name="how-does-azure-active-directory-help"></a>Wie hilft Azure Active Directory?

 Azure AD unterstützt umfassende Verwaltung konfigurierte Anwendungsmöglichkeiten Organisationen automatische, attributbasierten Zuordnung (ABAC oder RBAC-Szenarios) über die Delegierung sowie Administrator Management von rechten Zugriffsrichtlinien erzielt. Azure AD problemlos erreichen komplexe Richtlinien kombiniert mehrere managementmodelle für eine einzelne Anwendung und sogar können Regeln mit der gleichen anwendungsübergreifend.

 - [Hinzufügen von neuen oder vorhandenen Applikationen](active-directory-sso-integrate-saas-apps.md)


 Azure AD Anwendung Zuordnung konzentriert sich auf zwei primäre Zuordnung Modi:

- **Einzelne Aufgaben** IT-Administratoren mit globalen Administratorrechten Verzeichnis wählen einzelne Benutzerkonten und gewähren sie Zugriff auf die Anwendung.
- **Gruppenbasierte Zuweisung (Azure AD bezahlt)** IT-Administratoren mit globalen Verzeichnisberechtigungen kann die Anwendung eine Gruppe zuweisen. Bestimmte Benutzer Zugriff wird bestimmt, Mitglieder der Gruppe zur Zeit werden sie versuchen, die Anwendung zugreifen. Anders gesagt kann ein Administrator effektiv eine Arbeitsauftragsregel besagt "alle aktuellen Mitglieder der zugewiesenen Gruppe hat Zugriff auf die Anwendung" erstellen. Mit dieser Option Zuordnung profitieren Administratoren Azure AD Gruppe Management-Optionen einschließlich [Attribut-basierte dynamische Gruppen](active-directory-accessmanagement-manage-groups.md), externes Systemgruppen (z. B. lokale Active Directory Arbeitstag) oder Administrator verwaltet oder self-service verwaltet Gruppen. Eine Gruppe kann leicht zugewiesen werden mehrere apps damit Applikationen mit Zuordnung Arbeitsauftragsregeln, teilen die allgemeine Managementkomplexität reduzieren. Beachten Sie, dass verschachtelte Gruppe Mitgliedschaften für gruppenbasierte Zuordnung Anwendung zu diesem Zeitpunkt nicht unterstützt.

Mit dieser Aufgabe zwei Modi, können Administratoren alle wünschenswert Zuordnung Management erreichen.

Azure AD ist Verwendung und Zuweisung reporting vollständig integriert, Administratoren einfach Zuweisungsstatus Zuweisungsfehler und sogar Nutzung melden.

## <a name="complex-application-assignment-with-azure-ad"></a>Komplexe Anwendung Zuordnung mit Azure

Betrachten Sie eine Anwendung wie Salesforce. In vielen Organisationen wird hauptsächlich Salesforce Marketing- und Organisationen verwendet. Häufig haben Mitglieder des Marketingteams hoch privilegierten Zugang zu Salesforce, während Mitglieder des Vertriebsteams beschränkten Zugriff haben. In vielen Fällen einer umfassenden Information Worker haben eingeschränkten Zugriff auf die Anwendung. Ausnahmen erschweren Angelegenheiten. Es ist oft Recht der Marketing- oder Leadership Teams Benutzer erteilen oder ihre Rollen unabhängig von diesen allgemeinen Regeln ändern.

Azure AD kann beispielsweise Salesforce für einmaliges Anmelden (SSO) und automatisierte Bereitstellung. Nach der Konfiguration der Anwendungdes kann ein Administrator die Aktion einmalige erstellen und die entsprechenden Gruppen zuweisen. In diesem Beispiel könnte ein Administrator folgenden Aufgaben ausführen:

- [Dynamische Gruppen](active-directory-accessmanagement-manage-groups.md) können definiert werden, um automatisch alle Teammitglieder Marketing- und Attribute wie Abteilung oder Rolle darstellen:

    - Alle Marketing-Gruppen werden die Rolle "marketing" in Salesforce zugewiesen werden

    - Alle Mitglieder von sales Team "Vertrieb" bei Salesforce zugewiesen werden würde. Eine weitere Verfeinerung können mehrere Gruppen, die regionale Vertriebsteams Salesforce Rollen zugewiesen.

- Aktivieren Sie den Ausnahmemechanismus konnte eine Self-service-Gruppe für jede Rolle erstellt werden. Gruppe "Salesforce marketing Ausnahme" kann beispielsweise als Self-service-Gruppe erstellt werden. Die Gruppe marketing Salesforce-Rolle zugewiesen werden und marketing Führungsteam erfolgen Besitzer. Mitglieder des Marketingteams Führung konnte hinzufügen oder Entfernen Benutzer, eine Richtlinie Join festlegen oder selbst genehmigen oder verweigern Anfragen an einzelne Benutzer. Durch eine entsprechende Erfahrung der Arbeitskraft Informationen wird unterstützt, die keine spezielle Schulung für Eigentümer oder Mitglieder erforderlich ist.

In diesem Fall würde alle zugewiesenen Benutzer automatisch Salesforce bereitgestellt werden zu anderen Gruppen hinzugefügt werden, deren Zuweisung der Sicherheitsrolle in Salesforce aktualisiert. Benutzer wäre erkennen und Salesforce durch die Anwendung Access Bereich Office Webclients oder sogar zu ihrer Organisation Salesforce-Anmeldeseite zugreifen. Administratoren könne mit Azure AD Berichten Verwendung und Zuweisung Status anzuzeigen.

Administratoren können [Azure AD Zugangskontrolle](active-directory-conditional-access.md) festzulegenden Richtlinien für bestimmte Rollen verwenden. Diese Richtlinien gehören, ob Unternehmen und auch mehrstufige Authentifizierung oder Gerät Anforderungen Zugang in verschiedenen Fällen darf.

## <a name="how-can-i-get-started"></a>Wie kann ich anfangen?

Erstens, wenn nicht bereits Azure AD verwenden und Sie ein IT-Administrator sind:

 - [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/) -Melden Sie sich für eine kostenlose 30-tägige Testversion und Ihre erste Cloudlösung in 5 Minuten mit diesem Link bereitstellen

Azure AD bietet Features für die gemeinsame Nutzung von Konten gehören:

- [Zuordnung](active-directory-accessmanagement-self-service-group-management.md)
- Anwendung in Azure AD hinzufügen
- Erste Schritte mit
- Anwendung Zuordnung FAQ
- [App Dashboard/Verwendungsberichten](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>Wo erfahre ich mehr?

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Apps mit bedingten Zugriff schützen](active-directory-conditional-access.md)
- [Gruppe Self-service-Management/SSAA](active-directory-accessmanagement-self-service-group-management.md)
