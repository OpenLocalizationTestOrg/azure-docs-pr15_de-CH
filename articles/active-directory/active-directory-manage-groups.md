<properties
    pageTitle="Verwalten des Zugriffs auf Ressourcen mit Active Directory Azure | Microsoft Azure"
    description="Verwendung von Gruppen in Azure Active Directory zum Verwalten des Benutzerzugriffs auf lokale Cloudanwendungen und Ressourcen."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="managing-access-to-resources-with-azure-active-directory-groups"></a>Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen

Azure Active Directory (Azure AD) ist eine umfassende Identitäts- und Management Lösung einen robusten Satz von Funktionen zum Verwalten des Zugriffs auf lokale und Cloudanwendungen und Ressourcen einschließlich der Microsoft-Onlinedienste wie Office 365 und eine Microsoft SaaS-Applikationen. Dieser Artikel bietet eine Übersicht über aber möchten Sie verwenden Azure AD-Gruppen, führen Sie die Schritte [Verwalten von Sicherheitsgruppen in Azure AD](active-directory-accessmanagement-manage-groups.md). Möchten Sie sehen, wie PowerShell zum Verwalten von in Azure Active Directory verwendet erfahren Sie mehr in [Azure Active Directory Vorschau Cmdlets für die Verwaltung](active-directory-accessmanagement-groups-settings-v2-cmdlets.md).


> [AZURE.NOTE] Um Azure Active Directory verwenden, benötigen Sie ein Azure-Konto. Wenn Sie ein Konto haben, können Sie [für ein kostenloses Azure-Konto anmelden](https://azure.microsoft.com/pricing/free-trial/).


In Azure AD ist eines der wichtigsten Features Zugriff auf Ressourcen zu verwalten. Diese Ressourcen können im Verzeichnis wie Berechtigungen zum Verwalten von Objekten über Rollen im Verzeichnis oder externen Directory SaaS-Applikationen Azure Services und SharePoint-Websites oder auf lokale Ressourcen Ressourcen sein. Es gibt vier Methoden, die ein Benutzer Zugriffsrechte auf eine Ressource zugewiesen werden kann:


1. Direkte Zuordnung

    Benutzer können direkt auf eine Ressource vom Besitzer der Ressource zugewiesen werden.

2. Gruppenmitgliedschaft

    Eine Gruppe kann eine Ressource Besitzer der Ressource und damit zugewiesen werden die Mitglieder dieser Gruppe den Zugriff auf die Ressource gewähren. Mitgliedschaft in der Gruppe kann dann vom Besitzer der Gruppe verwaltet werden. Der Besitzer der Ressource delegiert, die Berechtigung, die Ressource an den Besitzer der Gruppe Benutzer zuweisen.

3. Regelbasierte

    Eine Regel können die Ressourcenbesitzer express, welche Benutzer Zugriff auf eine Ressource zugewiesen werden soll. Das Ergebnis der Regel hängt in der Regel und deren Werten für bestimmte Benutzer verwendeten Attribute und damit delegiert Besitzer der Ressource effektiv das Recht zum Zugriff auf die Ressource die autorisierende Quelle für Attribute zu verwalten, die in der Regel verwendet werden. Der Besitzer der Ressource noch die Regel verwaltet und bestimmt, welche Attribute und Werte auf die Ressource zugreifen.

4. Externen

    Zugriff auf eine Ressource wird von einer externen Quelle abgeleitet. Angenommen, eine Gruppe, die autorisierende Quelle wie einem lokalen Verzeichnis oder SaaS-app wie Arbeitstag synchronisiert. Besitzer der Ressource weist der Gruppe Zugriff auf die Ressource und die externe Quelle verwaltet die Mitglieder der Gruppe.

  ![Übersicht über Access Management Diagramm](./media/active-directory-access-management-groups/access-management-overview.png)


## <a name="watch-a-video-that-explains-access-management"></a>In diesem Video, die Verwaltung erläutert

Sie sehen ein kurzes Video, das mehr über diese erläutert:

**Azure AD: Einführung in dynamische Mitgliedschaft für Gruppen**

> [AZURE.VIDEO azure-ad--introduction-to-dynamic-memberships-for-groups]

## <a name="how-does-access-management-in-azure-active-directory-work"></a>Wie greift Management in Azure Active Directory arbeiten?
In der Mitte des Azure AD ist Lösung für die Verwaltung der Sicherheitsgruppe. Mit einer Sicherheitsgruppe Zugriff auf Ressourcen verwalten ist ein bekannter ermöglicht eine flexible und leicht verständlichen Möglichkeit für die gewünschte Benutzergruppe Zugriff auf eine Ressource zu. Der Besitzer der Ressource (oder der Administrator des Verzeichnisses) können eine Gruppe eine bestimmte direkt auf die Ressourcen zugreifen, die sie besitzen. Mitglieder der Gruppe erfolgt des Zugriffs und die Ressourcenbesitzer kann das Recht zur Verwaltung der Mitgliederliste der Gruppe an jemanden, Abteilungsleiter oder ein Helpdesk-Administrator delegieren.

![Azure Active Directory Access Management-Diagramm](./media/active-directory-access-management-groups/active-directory-access-management-works.png)

Der Besitzer einer Gruppe kann auch die Gruppe für Self-service-Anfragen zur Verfügung. Damit können Benutzer suchen und die Gruppe und eine Anforderung an, effektiv Erlaubnis zum Zugriff auf Ressourcen, die verwaltet werden durch die Gruppe. Der Besitzer der Gruppe kann die Gruppe so einrichten, dass automatisch genehmigten Verknüpfung oder Genehmigung durch den Besitzer der Gruppe. Wenn ein Benutzer zu einer Gruppe anfordert, wird die Anfrage an den Besitzer der Gruppe weitergeleitet. Wenn der Besitzer die Anforderung genehmigt, der anfordernde Benutzer benachrichtigt, und der Benutzer Mitglied der Gruppe. Der Besitzer die Anforderung verweigert, der anfordernde Benutzer benachrichtigt jedoch nicht Mitglied der Gruppe.


## <a name="getting-started-with-access-management"></a>Erste Schritte mit Access management
Startbereit? Sie sollten einige der grundlegenden Aufgaben mit Azure AD tun. Mit der speziellen unterschiedliche Personengruppen unterschiedliche Ressourcen in Ihrer Organisation Zugriff auf diese Funktionen. Ersten grundlegenden Schritte werden nachstehend aufgeführt.

* [Erstellen eine einfache Regel dynamische Mitgliedschaften für die Gruppe konfigurieren](active-directory-accessmanagement-manage-groups.md#how-can-i-manage-the-membership-of-a-group-dynamically)

* [Verwenden einer Gruppe Zugriff auf SaaS-Programme verwalten](active-directory-accessmanagement-group-saasapps.md)

* [Erstellen einer Gruppe für Endbenutzer Self-service](active-directory-accessmanagement-self-service-group-management.md)

* [Synchronisieren einer lokalen Gruppe in Azure mit Azure AD verbinden](active-directory-aadconnect.md)

* [Verwalten von Besitzer für eine Gruppe](active-directory-accessmanagement-managing-group-owners.md)


## <a name="next-steps-for-access-management"></a>Nächste Schritte für Verwaltung
Nun, dass Sie die Grundlagen der Verwaltung verstanden haben, stehen hier einige zusätzlichen erweiterten Funktionen in Azure Active Directory für die Verwaltung des Zugriffs auf Programme und Ressourcen.

* [Verwenden von Attributen für erweiterte Regeln erstellen](active-directory-accessmanagement-groups-with-advanced-rules.md)

* [Verwalten von Sicherheitsgruppen in Azure AD](active-directory-accessmanagement-manage-groups.md)

* [Dedizierte Gruppen einrichten in Azure AD](active-directory-accessmanagement-dedicated-groups.md)

* [Graph-API-Referenz für Gruppen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#GroupFunctions)

* [Azure Active Directory-Cmdlets für die Gruppe konfigurieren](active-directory-accessmanagement-groups-settings-cmdlets.md)
