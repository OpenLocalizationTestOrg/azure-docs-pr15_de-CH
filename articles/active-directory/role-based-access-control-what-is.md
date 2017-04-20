<properties
    pageTitle="Rollenbasierte Zugriffskontrolle | Microsoft Azure"
    description="Einstieg in Access Management Azure rollenbasierte Zugriffskontrolle im Azure-Portal. Verwenden Sie Arbeitsaufträge für Benutzerrollen Zuweisen von Berechtigungen im Verzeichnis."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/03/2016"
    ms.author="kgremban"/>

# <a name="get-started-with-access-management-in-the-azure-portal"></a>Erste Schritte mit Access Management im Azure-portal

Sicherheit Unternehmen konzentrieren sollten den Mitarbeitern die genauen Berechtigungen, die sie benötigen. Zu viele Berechtigungen stellt ein Konto für Angreifer. Zu wenige Berechtigungen bedeutet, dass Mitarbeiter effizienter arbeiten können. (Azure Role-Based Access Control, RBAC) können dieses Problem beheben, indem abgestimmte Verwaltung für Azure bietet.

RBAC können Sie Aufgaben innerhalb Ihres Teams aufteilen und das Ausmaß des Zugriffs für Benutzer, die sie für ihre Aufgaben gewähren. Anstatt alle uneingeschränkten Berechtigungen in der Azure-Abonnement oder Ressourcen können Sie bestimmte Aktionen. Z. B. RBAC können Mitarbeiter virtuelle Computer in einem Abonnement verwalten, während eine andere SQL-Datenbanken in dieselbe Abonnement verwalten kann.

## <a name="basics-of-access-management-in-azure"></a>Grundlagen der Verwaltung in Azure
Jede Azure-Abonnement ist Azure Active Directory (AD) Verzeichnis zugeordnet. Benutzer, Gruppen und Applikationen aus dem Verzeichnis können Ressourcen in Azure-Abonnement verwalten. Weisen Sie Berechtigungen mithilfe der Azure-Portal Azure Befehlszeilentools und Azure Management-APIs.

Erteilen des Zugriffs über entsprechende RBAC-Rolle für Benutzer, Gruppen und Programme in einem bestimmten Bereich zuweisen. Der Geltungsbereich einer Zuweisung der Sicherheitsrolle kann ein Abonnement, eine Ressourcengruppe oder eine Ressource. Eine Rolle in einem übergeordneten Bereich gewährt auch darin enthaltenen untergeordneten Elemente. Beispielsweise kann Benutzer mit Zugang zu Ressourcen alle Ressourcen verwalten, wie Websites, virtuellen Rechner und Subnets.

![Beziehung zwischen Azure Active Directory - Diagramm](./media/role-based-access-control-what-is/rbac_aad.png)

RBAC-Rolle, die Sie zuweisen bestimmt, welche Ressourcen in diesem Bereich Benutzer, Gruppe oder Anwendung verwalten kann.

## <a name="built-in-roles"></a>Integrierte Rollen
Azure RBAC besteht aus drei grundlegende Funktionen für alle Ressourcen gelten:

- **Besitzer** hat vollen Zugriff auf alle Ressourcen, einschließlich der Zugriff auf andere Benutzer übertragen.
- **Teilnehmer** können erstellen und Management aller Arten von Azure Ressourcen aber den anderen nicht möglich.
- **Leser** können vorhandene Azure Ressourcen anzeigen.

Die restlichen RBAC-Rollen in Azure Verwaltung von Azure Ressourcen zulassen. Virtual Machine Teilnehmerrolle kann z. B. der Benutzer erstellen und Verwalten von virtuellen Maschinen. Es gibt sie Zugriff auf das virtuelle Netzwerk oder Subnetz, dem mit dem virtuellen Computer verbunden.

[Integrierte RBAC-Rollen](role-based-access-built-in-roles.md) sind Rollen für Azure aufgeführt. Gibt die Operationen und Bereich Benutzer jede integrierte Rolle gewährt. Wenn Sie für noch mehr Kontrolle eigene Rollen definieren suchen, finden Sie unter [benutzerdefinierte Funktionen in Azure RBAC](role-based-access-control-custom-roles.md)erstellen.

## <a name="resource-hierarchy-and-access-inheritance"></a>Ressource-Hierarchie und Access Vererbung
- Jedes **Abonnement** in Azure gehört nur ein Verzeichnis.
- Jede **Ressourcengruppe** gehört nur ein Abonnement.
- Jede **Ressource** gehört zu nur einer Ressourcengruppe.

Zugriff auf übergeordnete Bereiche gewähren wird in untergeordneten Bereichen geerbt. Zum Beispiel:

- Sie haben ein Azure AD-Gruppe auf Abonnementebene Rolle zuweisen. Mitglieder dieser Gruppe können jede Ressourcengruppe und die Ressource im Abonnement anzeigen.
- Sie wird eine Anwendung unter Gruppenbereich Ressource der Beitragendenrolle zuweisen. Sie können Ressourcen aller Typen in der Gruppe, jedoch nicht anderen Ressourcengruppen im Abonnement verwalten.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC im Vergleich zu klassischen Abonnement-Administratoren
Klassische Abonnement Administratoren und co-Administratoren haben vollen Zugriff auf den Azure-Abonnement. Sie können Ressourcen mit Azure-Ressourcen-Manager-APIs oder das klassische Bereitstellungsmodell [Azure-Verwaltungsportal](https://manage.windowsazure.com) und Azure [Azure-Portal](https://portal.azure.com) verwalten. Im RBAC-Modell sind klassische Administratoren Besitzerrolle auf Abonnementebene zugewiesen.

Azure-Portal und neue Azure-Ressourcen-Manager-APIs unterstützen Azure RBAC. Anwender und Applications RBAC-Rollen zugewiesen werden können nicht klassische Verwaltungsportal und Azure klassischen Bereitstellungsmodell verwenden.

## <a name="authorization-for-management-vs-data-operations"></a>Autorisierung für Management und Datenvorgänge
Azure RBAC unterstützt nur Verwaltungsvorgänge Azure Ressourcen in Azure-Portal und Azure-Ressourcen-Manager-APIs. Es kann nicht alle Datenoperationen auf Azure Ressourcen autorisieren. Sie können z. B. autorisieren jemand Speicher, Konten nicht für Blobs oder Tabellen in ein Speicherkonto nicht. Ebenso kann eine SQL-Datenbank verwaltet werden, jedoch nicht die Tabellen in diesem.

## <a name="next-steps"></a>Nächste Schritte
- Erste Schritte mit [Role-Based Access Control in Azure-Portal](role-based-access-control-configure.md).
- Finden Sie unter [integrierte RBAC-Rollen](role-based-access-built-in-roles.md)
- Definieren Sie eigene [benutzerdefinierte Funktionen in Azure RBAC](role-based-access-control-custom-roles.md)
