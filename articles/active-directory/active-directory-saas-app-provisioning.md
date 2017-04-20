<properties
    pageTitle="Automatisierte Bereitstellung in Azure AD SaaS-App-Benutzer | Microsoft Azure"
    description="Einführung in Azure AD Verwendung automatisch bereitstellen de bereitstellen und aktualisieren ständig anwendungsübergreifende Drittanbietern SaaS Benutzerkonten."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatisieren der Bereitstellung und Entfernung SaaS-Anwendung in Azure Active Directory Benutzer

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Was ist die automatisierte Benutzerbereitstellung für SaaS-Apps?

Azure Active Directory (Azure AD) ermöglicht die Erstellung, Wartung und Entfernung von Identitäten in Cloudanwendungen ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) wie Dropbox, Salesforce und ServiceNow automatisieren.

**Nachfolgend Beispiele einige wie dieses Feature Sie können:**

- Erstellen Sie automatisch neue Konten in die SaaS apps für neue Leute Wenn sie Ihrem Team.
- Deaktivieren Sie automatisch Konten SaaS-apps Menschen zwangsläufig das Team verlassen.
- Sicherstellen Sie, dass die Identitäten in SaaS-apps Stand sind basierend auf im Verzeichnis.
- Nicht Benutzer Objekte, wie Gruppen SaaS-Apps, die diese unterstützen.

**Automatisches benutzerbereitstellung auch die folgende Funktionen:**

- Die Möglichkeit, vorhandene Identitäten zwischen Azure übereinstimmen und SaaS-apps.
- Anpassungsoptionen Hilfe Azure AD passt die aktuellen Konfigurationen SaaS-Apps, die Ihre Organisation derzeit verwendet.
- Optionale e-Mail-Benachrichtigungen für die Bereitstellung von Fehlern.
- Reporting und Aktivitätsprotokollen, Überwachung und Problembehandlung bei.

##<a name="why-use-automated-provisioning"></a>Warum verwenden automatisierte Bereitstellung?

Einige allgemeine Gründe für diese Funktion sind:

- Zu Kosten, Ineffizienz und menschliche Fehler manuelle Bereitstellung, Prozessen zugeordnet.
- Sichern Ihrer Organisation sofort Benutzeridentität Schlüssel SaaS-Apps entfernen, wenn sie die Organisation verlassen.
- Eine Bulk Anzahl Benutzer problemlos in eine bestimmte SaaS-Anwendung zu importieren.
- Genießen Sie bequem Ihre Bereitstellung Lösung aus derselben Zugriffsrichtlinien app, die für Azure AD Single Sign-On definiert.

##<a name="frequently-asked-questions"></a>Häufig gestellte Fragen

**Wie oft schreibt Azure AD Verzeichniswechsel SaaS-App?**

Azure AD prüft ändert alle fünf bis zehn Minuten. Wenn SaaS-app mehrere Fehler (z. B. bei ungültigen Administratoranmeldeinformationen) zurückgeben, dann verlangsamt Azure AD stufenweise die Frequenz bis einmal pro Tag, bis die Fehler behoben werden.

**Wie lange dauert es Benutzer bereitstellen?**

Ändert fast sofort passieren, aber wenn Sie die meisten Ihres Verzeichnisses bereitstellen, dann Es hängt von der Anzahl von Benutzern und Gruppen. Kleine Verzeichnisse nur einige Minuten dauern, mittlere Verzeichnisse können einige Minuten dauern und sehr große Verzeichnissen können mehrere Stunden dauern.

**Wie kann ich den Status der aktuellen Bereitstellungsauftrag verfolgen?**

Sie können die Bereitstellung Bericht im Bereich Berichte des Verzeichnisses überprüfen. Eine weitere Option ist die Dashboard-Registerkarte SaaS-Anwendung aufrufen, die zum Bereitstellen und sehen Sie unter "Integration" Statusbereich am unteren Rand der Seite.

**Wie erfahre ich, wenn Benutzer ordnungsgemäß bereitgestellt bekommen?**

Am Ende der Bereitstellung Konfiguration steht es Assistenten zum Abonnieren von Benachrichtigungen für die Bereitstellung von Fehlern. Sie können auch Überprüfen der Bereitstellung Fehler Bericht sehen, welche Benutzer bereitgestellt werden und warum.

**Kann Azure AD ändert SaaS-App wieder in das Verzeichnis schreiben?**

Für die meisten SaaS-apps ist die Bereitstellung nur ausgehenden, was bedeutet, dass Benutzer aus dem Verzeichnis der Anwendung geschrieben werden und ändert sich von der Anwendung nicht wieder in das Verzeichnis geschrieben werden. Für [Arbeitstag](https://msdn.microsoft.com/library/azure/dn762434.aspx)ist jedoch Bereitstellung nur eingehende, was bedeutet, dass Benutzer aus Arbeitstag in das Verzeichnis importiert und auch Einträge im Verzeichnis sind nicht geschrieben zurück in Arbeitstag.

**Wie kann ich Feedback an das engineering-Team übermitteln?**

Kontaktieren Sie uns über das [Bewertungsportal Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Wie funktioniert der automatisierte Bereitstellung?

Azure AD stellt Benutzern SaaS-apps durch Bereitstellung jeder Anwendung Herstellers Endpunkte verbinden. Diese Endpunkte ermöglichen Azure AD programmgesteuert erstellen, aktualisieren und Entfernen von Benutzern. Es folgt eine kurze Übersicht über die verschiedenen Schritte, die Azure AD Bereitstellung automatisieren.

1. Wenn Sie die Bereitstellung für eine Anwendung zum ersten Mal aktivieren, werden die folgenden Aktionen ausgeführt:
 - Azure AD gleicht vorhandenen Benutzern im SaaS-app mit ihren entsprechenden Identitäten im Verzeichnis. Wenn ein Benutzer zugeordnet ist, für einmaliges Anmelden *nicht* automatisch aktiviert. Damit Benutzer auf die Anwendung zugreifen müssen sie explizit die app in Azure AD direkt oder über eine Gruppenmitgliedschaft zugewiesen werden.
 - Wenn Sie bereits festgelegt haben, welche Benutzer der Anwendung zugewiesen werden soll und Azure AD keine vorhandene Konten für Benutzer bereitstellen Azure AD neue Konten für sie in der Anwendung.
2. Nach die anfängliche Synchronisierung abgeschlossen wurde, wie oben beschrieben überprüft Azure AD 10 Minuten für folgende geändert:
 - Wenn die Anwendung neue Benutzer zugewiesen wurde (entweder direkt oder über eine Gruppenmitgliedschaft), dann werden sie ein neues Konto in der SaaS-app bereitgestellt.
 - Zugriffsrechte eines Benutzers entfernt, wenn ihr Konto in der SaaS-app wird als deaktiviert markiert (Benutzer werden nie gelöscht, die schützt vor Datenverlust bei einer fehlerhaften Konfiguration).
 - Wenn ein Benutzer zuletzt der Anwendung zugewiesen wurde und sie ein Konto in der SaaS-app bereits, dieses Konto als aktiviert gekennzeichnet und bestimmte Eigenschaften können aktualisiert werden, wenn sie veraltet im Vergleich zu dem Verzeichnis sind.
 - Wenn ein Benutzer Informationen (Telefonnummer, Büro usw.) im Verzeichnis geändert wurde, wird diese Informationen auch in SaaS-Anwendung aktualisiert.

Für Weitere Informationen über die Zuordnung von Attributen zwischen Azure und SaaS app Artikel [Attribute Mappings anpassen](active-directory-saas-customizing-attribute-mappings.md)angezeigt.

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Liste der Apps unterstützen automatisierte Bereitstellung von Benutzer

Klicken Sie auf eine Anwendung an ein Lernprogramm zum automatisierten Bereitstellung dafür konfigurieren:

- [Feld](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Stimme](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox für Unternehmen](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Salesforce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Salesforce-Sandbox](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [Arbeitstag](http://go.microsoft.com/fwlink/?LinkId=690250) (eingehende provisioning)

Damit eine Anwendung automatisierte benutzerbereitstellung unterstützen muss diese zuerst die erforderlichen Endpunkte bereitstellen, die für externe Programme zur Automatisierung von Erstellung, Wartung und Entfernung von Benutzern ermöglichen. Daher sind nicht alle SaaS-apps mit diesem Feature kompatibel. Für apps, die dies unterstützen, Azure AD-Entwicklungsteam werden Bereitstellung Connector die Apps erstellen und dabei die Bedürfnisse des Kunden Priorität.

Azure AD-Entwicklungsteam um Bereitstellung Support für zusätzliche Applikationen zu kontaktieren, senden Sie eine Nachricht über das [Bewertungsportal Azure Active Directory](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="related-articles"></a>Verwandte Artikel

- [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)
- [Anpassen der Attribute Mappings für Benutzer bereitstellen](active-directory-saas-customizing-attribute-mappings.md)
- [Schreiben von Ausdrücken für Attribute Mappings](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Bereichsfilter für Benutzer bereitstellen](active-directory-saas-scoping-filters.md)
- [So aktivieren Sie die automatische Bereitstellung von Benutzern und Gruppen aus dem Active Directory Azure Applications verwenden SCIM](active-directory-scim-provisioning.md)
- [Bereitstellen von Benachrichtigungen Konto](active-directory-saas-account-provisioning-notifications.md)
- [Liste der Lernprogramme zur Integration SaaS-Apps](active-directory-saas-tutorial-list.md)
