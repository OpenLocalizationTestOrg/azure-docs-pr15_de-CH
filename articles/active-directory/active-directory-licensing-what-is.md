<properties
    pageTitle="Was ist Microsoft Azure Active Directory Lizenzierung? | Microsoft Azure"
    description="Beschreibung der Lizenzierung von Microsoft Azure Active Directory, wie es funktioniert, wie und best Practices einschließlich Office 365 und Windows Intune und Azure Active Directory Premium-Editionen"
    services="active-directory"
      keywords="Azure AD-Lizenzierung"
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

# <a name="what-is-microsoft-azure-active-directory-licensing"></a>Was ist Microsoft Azure Active Directory Lizenzierung?

##<a name="description"></a>Beschreibung
Azure Active Directory (Azure AD) ist Microsofts Identität als Lösung (IDaaS) und Plattform. Azure AD in funktionale und technische Varianten von Azure AD frei, die mit jeder Microsoft Office 365 und Dynamics, Windows Intune Azure angeboten (Azure AD generiert keine Gebühren Verbrauch in diesem Modus), Azure AD kostenpflichtigen Versionen wie Enterprise Mobility Suite (EMS), Azure AD Premium und Basic, Azure mehrstufige Authentifizierung (MFA). Wie viele Microsoft online Services am Azure AD bezahlte Versionen durch benutzerdefinierte Ansprüche werden in Office 365 Windows Intune und Azure AD sind. In diesen Fällen Einkauf Service mit einem oder mehreren Abonnements dargestellt, und jedes Abonnement umfasst eine Reihe vor dem Kauf von Lizenzen in Ihrem Mandanten. Benutzerdefinierte Berechtigungen sind Lizenz Zuordnung, Erstellen einer Verknüpfung zwischen dem Benutzer und dem Produkt Dienstkomponenten für Benutzer aktivieren und Verwenden eines Prepaid-Lizenzen erreicht.

[Probieren Sie Azure AD Premium.](https://portal.office.com/Signup/Signup.aspx?OfferId=01824d11-5ad8-447f-8523-666b0848b381&ali=1#0)

> [AZURE.NOTE] Azure AD administrationsportal ist Teil des klassischen Azure-Portal. Während Azure AD mit Azure Einkäufe nicht erfordert, muss Zugriff auf dieses Portal ein aktives Azure-Abonnement oder ein [Testabonnement Azure](https://azure.microsoft.com/pricing/free-trial/).

Eine allgemeine Übersicht über Azure AD-Service-Funktionen finden Sie unter [Azure AD](active-directory-whatis.md).
[Erfahren Sie mehr über Azure AD Service-Level](https://azure.microsoft.com/support/legal/sla/)

> [AZURE.NOTE]  Azure Quellenbesteuerung Abonnements unterscheiden: zusätzlich im Verzeichnis diese Abonnements aktivieren Azure Ressourcen und Zahlungsmethode zuordnen. In diesem Fall gibt es keine Lizenz zählt das Abonnement zugeordnet. Verband mit dem Abonnement Benutzerzugriff verwalten Abonnementressourcen erfolgt durch erteilen sie Berechtigungen auf Azure Ressourcen, die dem Abonnement zugeordnet.


##<a name="how-does-azure-ad-licensing-work"></a>Wie funktioniert Azure AD-Lizenzierung?

Azure AD (Anspruch basiert) Lizenz-basierte Dienste durch Aktivieren eines Abonnements in Ihrem Mandanten Azure AD Directory-Dienst. Nachdem das Abonnement aktiv ist können lizenzierte Benutzer Servicefunktionen von Administratoren Directory-Dienst verwaltet und verwendet werden.

Beim Kauf oder Enterprise Mobility Suite, Azure AD Premium oder Basic Azure AD aktivieren wird das Verzeichnis mit dem Abonnement einschließlich Gültigkeitsdauer und Prepaid-Lizenzen aktualisiert. Die Abonnementinformationen ist einschließlich Status, Lebenszyklus weiter und die Anzahl der zugewiesenen und verfügbaren Lizenzen über klassischen Azure-Portal Registerkarte Lizenzen für bestimmte Verzeichnis. Dies ist auch besten Ihre Lizenz Aufgaben verwalten.

Jedes Abonnement umfasst mindestens Servicepläne jeder Zuordnung enthalten Funktionsebene des Diensttyps. z. B. Azure AD MFA Azure, Windows Intune, Exchange Online und SharePoint Online. Verwaltung von Azure AD erfordert keine Plan Servicelevel-Management. Dies unterscheidet sich von der auf diesem erweiterte Konfiguration auf Leistungen zu Office 365. Azure AD abhängig-Dienstkonfiguration Features aktivieren und verwalten einzelne Berechtigungen.

Azure AD-Abonnementinformationen wird im Allgemeinen durch Azure Verwaltungsportal Registerkarte Lizenzen für bestimmte Verzeichnis verwaltet. Azure AD-Abonnements mit Ausnahme von Azure AD Premium erscheinen im Office-Portal nicht.

> [AZURE.IMPORTANT] Azure AD Premium und Basic und Enterprise Mobility Suite Abonnements sind auf ihre bereitgestellten Verzeichnis-Tenant beschränkt. Abonnements können nicht zwischen Verzeichnissen oder berechtigt Benutzer in anderen Verzeichnissen verwendet. Verschieben eines Abonnements zwischen Verzeichnissen kann aber Senden einer Support-Ticket oder Abbruch und bei direkten Einkäufe Rücknahme-erfordert.

> Beim Kauf von Azure AD oder Enterprise Mobility Suite durch Volumenlizenzierung Abonnement Aktivierung erfolgt automatisch bei die Vereinbarung andere Microsoft Online-Dienste, z. B. Office 365 enthält.

Bezahlt Azure AD-Funktionen umfassen die Breite des Verzeichnisses. Beispiele:
- Gruppenbasierte Zuweisung Anwendung, die unter der jeweiligen Anwendung aktiviert ist, die Sie verwalten.
- Erweiterte und Gruppe Self-service-Management-Funktionen stehen unter Verzeichniskonfiguration oder innerhalb der Gruppe.
- Premium-Sicherheitsberichte werden auf der Registerkarte Bericht
- Cloud Application Discovery wird in Azure-Portal unter Identität.

###<a name="assigning-licenses"></a>Zuweisen von Lizenzen
Zwar erhalten ein Abonnement alle bezahlte Funktionen konfigurieren müssen, erfordert die Verwendung der Azure AD Features bezahlt Lizenzen an die richtigen Personen verteilen. Im Allgemeinen muss alle Benutzer, die Zugriff haben sollten oder, und ein Azure verwaltet wird Feature bezahlt eine Lizenz zugewiesen werden. Lizenzzuweisung ist eine Zuordnung zwischen einem Benutzer und einem Dienst erworbenen wie Azure AD Premium oder Basic Enterprise Mobility Suite.

Verwalten die Benutzer in Ihrem Verzeichnis müssen eine Lizenz ist einfach. Dies kann durch Zuweisen einer Gruppe Zuweisungsregeln Azure AD Administration Portal erstellen oder Zuweisen von Lizenzen direkt an die richtigen Personen Portal, PowerShell oder APIs erfolgen. Wenn einer Gruppe Lizenzen zuweisen, werden alle Mitglieder der Gruppe eine Lizenz zugewiesen. Wenn Benutzer hinzugefügt oder aus der Gruppe entfernt sie zugewiesen oder entfernt die entsprechende Lizenz. Zuordnung nutzen alle zur Verwaltung und gruppenbasierte Zuweisung Anwendung entspricht. Mithilfe dieses Ansatzes Sie Regeln so einrichten, dass automatisch alle Benutzer im Verzeichnis, hat jeder Benutzer mit den entsprechenden Titel eine Lizenz oder auch delegiert die Entscheidung Führungskräften in der Organisation.

Mit Arbeitsgruppen-Lizenz erben alle Benutzer einen Einsatzort fehlt das Verzeichnis bei der Zuordnung. Dieser Speicherort kann jederzeit vom Administrator geändert werden. In Fällen, in denen die automatische Zuweisung Fehler konnte nicht, reflektiert die Informationen unter diesem Lizenztyp dieses Staates.

##<a name="getting-started-with-azure-ad-licensing"></a>Erste Schritte mit Azure AD-Lizenzierung

Erste Schritte mit Azure ist einfach. Sie können das Verzeichnis als Teil einer kostenlosen Testversion von Azure Anmeldung immer erstellen. [Weitere Informationen über die Organisation anmelden](sign-up-organization.md). Folgendes können Sie sicherstellen, dass das Verzeichnis am besten mit anderen Microsoft-Diensten möglicherweise verwenden oder verwenden möchten und Ihre Ziele in den Dienst ausgerichtet ist.

Hier sind ein paar Tipps:
- Wenn Sie bereits Microsoft organisatorische Dienste verwenden, verfügen Sie bereits über ein Azure AD-Verzeichnis. In diesem Fall sollten Sie weiterhin dasselbe Verzeichnis für andere Dienste verwendet, Core Identitätsmanagement sowie Bereitstellung Hybrid SSO zwischen den Diensten verwendet werden kann. Benutzer müssen einen einzelnen Anmeldevorgang und umfangreichere Funktionen über die Services profitieren. Daher sollten ein Azure AD Gebühr für Ihre Mitarbeiter erwerben möchten, Sie dasselbe Verzeichnis dazu verwenden.
- Wenn Sie für eine andere Gruppe von Benutzern (Partner, Kunden und usw.) und möchte Azure AD Dienstleistungen und möchten, dass Ihre produktionsservice isoliert oder wenn Sie eine Sandbox-Umgebung für Ihre Dienste einrichten suchen, empfehlen wir Azure AD verwenden, erstellen Sie zuerst ein neues Verzeichnis über Azure Azure-Verwaltungsportal. [Weitere Informationen zum Erstellen eines neuen Azure AD-Verzeichnis im klassischen Azure-Portal](active-directory-licensing-directory-independence.md). Das neue Verzeichnis erstellt mit Ihrem Konto als externer Benutzer über globale Administratorrechte verfügt. Wenn Sie zum klassischen Azure-Portal mit diesem Konto anmelden, werden Sie zu diesem Verzeichnis alle Verwaltungsaufgaben Verzeichnis zugreifen können. Wir empfehlen erstellen ein lokales Konto mit entsprechenden Berechtigungen zum Verwalten von anderen Microsoft-Diensten (die nicht über die klassischen Azure-Portal). [Erfahren Sie mehr über das Erstellen von Benutzerkonten in Azure AD](active-directory-create-users.md).

> [AZURE.NOTE] Azure AD unterstützt "externe Benutzer" die Benutzerkonten in einer Instanz von Azure AD mit einem Microsoft-Konto (MSA) oder Azure AD-Identität aus einem anderen Verzeichnis erstellt wurden. Zwar werden werden erweitert diese Funktion in der Organisation Microsoft Services jetzt diese Konten nicht in die Dienste Erfahrungen unterstützt. Beispielsweise unterstützt das Verwaltungsportal von Office 365 derzeit diese Benutzer nicht. Daher dürfen externe Benutzer mit Microsoft auf dem Verwaltungsportal von Office 365, während externe Benutzer aus anderen Verzeichnissen Azure AD werden ignoriert. In letzterem Fall nur das lokale Benutzerkonto, Azure AD oder Office 365-Verzeichnis, in dem der Benutzer ursprünglich erstellt wurde, über diese Erfahrung.

Wie erwähnt, hat Azure AD bezahlte Varianten. Diese Versionen wurden einige kleinere Unterschiede in ihrer Bestellung Verfügbarkeit:


| Produkt   | EA-VL     | Öffnen  |   CSP |   Nutzungsrechte für MPN  |   Direkter Kauf | Testversion |
|---|---|---|---|---|---|---|
| Enterprise Mobility Suite |   X | X | X | X |  |      X |
| Azure AD Premium  | X | X | X |   | X | X |
| Azure AD Basic    | X | X | X | X |  <br /> |  <br />  |

###<a name="select-one-or-more-license-trials"></a>Wählen Sie mindestens eine Lizenz Testversionen
 In allen Fällen können Sie ein Testabonnement Azure AD Premium oder Enterprise Mobility Suite durch bestimmte Testversion auf gewünschte Registerkarte Lizenzen in Ihrem Verzeichnis auswählen aktivieren. Eine Testversion enthält ein 30-Tage-Abonnement mit 100 Lizenzen.

![Azure Active Directory Testlizenz Pläne](./media/active-directory-licensing-what-is/trial_plans.png)

![Enterprise Mobility Suite Testlizenz Pläne](./media/active-directory-licensing-what-is/EMS_trial_plan.png)

![Aktive Testlizenz Pläne](./media/active-directory-licensing-what-is/active_license_trials.png)

###<a name="assign-licenses"></a>Zuweisen von Lizenzen
Sobald das Abonnement aktiv ist, sollten Sie eine Lizenz zuweisen und aktualisieren Sie den Browser, um sicherzustellen, dass Sie alle Eigenschaften sehen. Im nächste Schritt wird gezahlt Azure AD-Funktionen den Benutzern Lizenzen zuweisen, die Zugriff oder in enthalten sein müssen. Wie oben unter "Zuweisen von Lizenzen" erwähnt, ist die beste Möglichkeit dazu Gruppe, die die gewünschte Zielgruppe zu identifizieren und die Lizenz zuweisen. auf diese Weise werden Benutzer hinzugefügt oder aus der Gruppe entfernt, während des gesamten Lebenszyklus zugewiesen oder entfernt die Lizenz.

Eine Lizenz für eine Gruppe oder einzelne Benutzer zuordnen möchten, wählen Sie Lizenzplan zuweisen, und klicken auf der Befehlsleiste **zuweisen** möchten.

![Aktive Testlizenz Pläne](./media/active-directory-licensing-what-is/assign_licenses.png)

Klicken Sie im Dialogfeld Zuordnung für den ausgewählten Plan können Sie einmal Benutzern und das **zuweisen** der rechten Spalte auf Hinzufügen auswählen. Der Liste durchblättern oder suchen Sie nach bestimmte Personen mit der Suche auf das Glas rechts vom Benutzer Raster. Gruppen zuzuweisen **Anzeigen** im Menü Wählen Sie aus "Gruppen" und dann auf das Kontrollkästchen rechts auf die Schaltfläche Aktualisieren die Arbeitsaufträge, die angezeigt werden.

![Zuweisen von Lizenzen zu Gruppen](./media/active-directory-licensing-what-is/assign_licenses_to_groups.png)

Sie können jetzt suchen oder durchblättern von Gruppen und Hinzufügen der Spalte **zuweisen** auf die gleiche Weise. Diese können eine Kombination von Benutzern und Gruppen in einem Arbeitsgang zuweisen. Um die Aufgabe abzuschließen, klicken Sie Kontrollkästchen unten rechts auf der Seite.

![Lizenz Zuordnung Statusnachricht](./media/active-directory-licensing-what-is/license_assignment_progress_message.png)

Wenn eine Gruppe zugewiesen wird, erben Member Lizenzen innerhalb von 30 Minuten, aber in der Regel innerhalb von 1 bis 2 Minuten.

Zuweisungsfehler bei Azure AD lizenzzuweisung möglich sind jedoch relativ selten. Zuordnung-Fehler sind auf:
- Zuweisung Konflikt - Wenn ein Benutzer bereits eine Lizenz zugewiesen wurde, die mit der aktuellen Lizenz nicht kompatibel ist. In diesem Fall benötigen die neue Lizenz zuweisen vorherigen entfernen.
- Überschritten verfügbaren Lizenzen - Wenn Benutzer zugewiesenen Gruppen verfügbare Lizenzen mehr Arbeitsauftragsstatus die Benutzer einen Fehler aufgrund fehlender Lizenzen zuweisen wider.

###<a name="view-assigned-licenses"></a>Ansicht zugewiesene Lizenzen

Eine Übersicht der verfügbaren zugewiesen und nächste Abonnement Lebenszyklus Ereignis zugewiesene Lizenzen werden auf der Registerkarte **Lizenzen** angezeigt.

![Zeigt die Anzahl der zugewiesenen Lizenzen](./media/active-directory-licensing-what-is/view_assigned_licenses.png)

Eine detaillierte Liste der zugeordneten Benutzer und Gruppen steht Zuordnungsstatus sowie Pfad (direkt oder geerbt von einer oder mehreren Gruppen) eine Lizenz Plan navigieren.

![Detailanzeige der Lizenzen für einen Lizenzplan zugewiesen](./media/active-directory-licensing-what-is/assigned_licenses_detail.png)

Entfernen von Lizenzen ist ebenso einfach wie zuweisen. Wenn dem Benutzer direkt zugewiesen oder für eine Gruppe zugewiesene Lizenz Lizenztyp auswählen Auswahl **Entfernen**, die Liste der Benutzer oder die Gruppe hinzufügen und bestätigen die Aktion entfernen. Alternativ können Sie eine Lizenz öffnen, wählen Sie bestimmte Benutzer oder Gruppen und tippen Sie auf der Befehlsleiste auf **Entfernen** . Zum Beenden des Benutzers Vererbung einer Lizenz einer Gruppe einfach entfernen Sie Benutzer aus der Gruppe.

###<a name="extending-trials"></a>Erweitern von Testversionen

Testversion Extensions für Kunden stehen als Self-service über Office 365-Portal. Kunden-Admin können [Office-Portal](https://portal.office.com/#Billing) (Zugriff je nach Berechtigungen für das Office-Portal), und wählen Sie Ihre Azure AD Premium-Testversion. Klicken Sie auf **Testversion verlängern** und folgen. Sie müssen eine Kreditkarte eingeben, aber nicht berechnet.

![Anhaltendes eine Lizenz im Office-portal](./media/active-directory-licensing-what-is/extend_license_trial.png)

Kunden können eine Testversion Erweiterung auch eine Support-Anfrage anfordern. Kunden-Admin können Office 365 Portal [Supportseite](http://aka.ms/extendAADtrial) (Zugriff je nach Berechtigungen für Office Support-Seite). Wählen Sie auf dieser Seite "Abonnements und Versuche" unter Features und "Test" unter Symptom. Geben Sie abschließend Informationen über die Umstände

![Ein anhaltendes Lizenz einer Support-Anforderung](./media/active-directory-licensing-what-is/alternate_office_aad_trial_extension.png)

## <a name="next-steps"></a>Nächste Schritte

Jetzt möglicherweise konfigurieren und einige Azure AD Premium-Features verwenden.

- [Self-service-Kennwort zurücksetzen](active-directory-manage-passwords.md)
- [Benutzerverantwortliche Verwaltung](active-directory-accessmanagement-self-service-group-management.md)
- [Azure AD Connect Health](active-directory-aadconnect-health.md)
- [Zuordnung für Clientanwendungen](active-directory-manage-groups.md)
- [Azure mehrstufige Authentifizierung](../multi-factor-authentication/multi-factor-authentication.md)
- [Direkter Kauf von Azure AD Premium-Lizenzen](http://aka.ms/buyaadp)
