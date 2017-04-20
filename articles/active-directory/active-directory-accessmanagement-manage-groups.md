<properties
    pageTitle="Verwalten von Gruppen in Active Directory Azure | Microsoft Azure"
    description="Das Erstellen und Verwalten von Gruppen zum Verwalten von Azure Azure Active Directory mit Benutzern."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/29/2016"
    ms.author="curtand"/>


# <a name="managing-groups-in-azure-active-directory"></a>Verwalten von Gruppen in Active Directory Azure

> [AZURE.SELECTOR]
- [Azure-portal](active-directory-groups-create-azure-portal.md)
- [Azure-Verwaltungsportal](active-directory-accessmanagement-manage-groups.md)
- [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)


Features von Azure Active Directory (Azure AD) Verwaltung gehört die Möglichkeit, Gruppen von Benutzern erstellen. Mithilfe eine Gruppe Verwaltungsaufgaben wie Lizenzen oder Berechtigungen für eine Anzahl von Benutzern gleichzeitig zuweisen. Sie können Gruppen auch Berechtigung zuweisen

- Ressourcen wie Objekte im Verzeichnis
- Ressourcen in das Verzeichnis SaaS-Applikationen Azure Services, SharePoint-Websites oder auf lokale Ressourcen

Darüber hinaus kann ein Ressourcenbesitzer auch Zugriff eine Ressource einer Azure AD-Gruppe im Besitz von Personen zuweisen. Diese Zuordnung wird Mitglied dieser Gruppe den Zugriff auf die Ressource erteilt. Der Besitzer der Gruppe verwaltet die Mitgliedschaft in der Gruppe. Besitzer der Ressource delegiert, der Besitzer der Gruppe die Berechtigung, Ihre Benutzer zuweisen.

## <a name="how-do-i-create-a-group"></a>Erstellen eine Gruppe

Je nach den Diensten, die Ihre Organisation abonniert hat, können Sie eine Gruppe mit einer der folgenden erstellen:
- Azure-Verwaltungsportal
- Office 365-Konto-portal
- Windows Intune-Kontoportal

Wie im klassischen Azure-Portal werden Aufgaben beschrieben. Weitere Informationen über nicht Azure Portals zu Azure AD-Verzeichnis finden Sie unter [Verwalten von Azure AD-Verzeichnis](active-directory-administer.md).

1. Aktivieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **Active Directory**und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** .

3. Wählen Sie **Gruppe hinzufügen**.

4. Geben Sie im Fenster **Gruppe hinzufügen** den Namen und die Beschreibung einer Gruppe.


## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a>Wie hinzufügen oder Entfernen einzelner Benutzer in eine Gruppe?

**Um einen einzelnen Benutzer zu einer Gruppe hinzufügen**

1. Aktivieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **Active Directory**und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** .

3. Öffnen Sie die Gruppe, die Sie Mitglieder hinzufügen möchten. Öffnen Sie die Registerkarte **Mitglieder** der ausgewählten Gruppe aus, wenn es nicht bereits angezeigt.

4. Wählen Sie **Mitglieder hinzufügen**.

5. Wählen Sie auf der Seite **Mitglieder hinzufügen** den Namen des Benutzers oder einer Gruppe als Mitglied dieser Gruppe hinzufügen möchten. Stellen Sie sicher, dass dieser Name in den Bereich **ausgewählte** hinzugefügt wird.


**Um einen einzelnen Benutzer aus einer Gruppe entfernen**

1. Aktivieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **Active Directory**und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** .

3. Öffnen Sie die Gruppe, aus der Sie Mitglieder entfernen möchten.

4. Wählen Sie die Registerkarte **Mitglieder** , wählen Sie den Namen des Elements, das Sie aus dieser Gruppe entfernen möchten und klicken Sie dann auf **Entfernen**.

6. Bestätigen Sie aufgefordert, dass Sie diesen Member aus der Gruppe entfernen möchten.


## <a name="how-can-i-manage-the-membership-of-a-group-dynamically"></a>Wie kann ich die Mitgliedschaft einer Gruppe dynamisch verwalten?

In Azure AD können Sie problemlos eine einfache Regel einrichten, die bestimmen, welche Benutzer Mitglieder der Gruppe. Eine einfache Regel ist nur einen Vergleich macht. Beispielsweise wenn eine SaaS-Anwendung eine Gruppe zugewiesen ist, können Sie eine Regel einrichten, Benutzer hinzuzufügen, die Berufsbezeichnung "Vertriebsmitarbeiter". Diese Regel gewährt Zugriff auf diese SaaS-Anwendung für alle Benutzer mit dieser Berufsbezeichnung im Verzeichnis.

Wenn Attribute eine Änderung überprüft alle dynamischen Regeln in einem Verzeichnis auf das Ändern des Benutzers auslösen eine Gruppe hinzugefügt oder entfernt. Wenn ein Benutzer eine Regel für eine Gruppe entspricht, dieser Gruppe als Mitglied hinzugefügt. Wenn sie nicht die Regel einer Gruppe erfüllen sind sie Mitglied von Gruppe als Mitglieder entfernt.

> [AZURE.NOTE] Sie können eine Regel für dynamische Mitgliedschaft in Sicherheitsgruppen oder Office 365-Gruppen einrichten. Verschachtelte Gruppenmitgliedschaften werden derzeit für gruppenbasierte Zuordnung Anwendung nicht unterstützt.
>
> Dynamische Mitgliedschaft für Gruppen müssen eine Azure AD Premium-Lizenz zugewiesen werden
>
> - Der Administrator die Regel auf eine Gruppe verwaltet
> - Alle Mitglieder der Gruppe

**So aktivieren Sie dynamische Mitgliedschaft einer Gruppe**

1. Aktivieren Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) **Active Directory**und wählen Sie den Namen des Verzeichnisses für Ihre Organisation.

2. Wählen Sie die Registerkarte **Gruppen** , und öffnen Sie die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren** und **Dynamische Mitgliedschaften aktivieren** auf **Ja**festgelegt.

4. Richten Sie eine einfache Regel für die Gruppe wie dynamische Mitgliedschaft in dieser Gruppenfunktionen steuern. Stellen Sie sicher, dass der **Benutzer hinzufügen,** Option ausgewählt ist, und wählen Sie eine Eigenschaft aus der Liste (z. B. Abteilung, Position usw.),

5. Wählen Sie eine Bedingung (nicht gleich gleich beginnt mit, beginnt mit, enthält nicht, enthält, übereinstimmen, Übereinstimmung).

6. Geben Sie einen Vergleichswert für die ausgewählten Benutzer-Eigenschaft.

Informationen zum Erstellen von *erweiterten* (Regeln, die mehrere Vergleiche enthalten kann) für dynamische Mitgliedschaft finden Sie unter [Using Attribute erweiterte Regeln erstellen](active-directory-accessmanagement-groups-with-advanced-rules.md).

## <a name="additional-information"></a>Weitere Informationen

Diese Artikel bieten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Azure Active Directory-Cmdlets für die Gruppe konfigurieren](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikel-Index für das Anwendungsmanagement in Azure Active Directory](active-directory-apps-index.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integration von lokalen Identitäten in Azure Active Directory](active-directory-aadconnect.md)
