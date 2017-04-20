<properties
    pageTitle="Azure Access Sammelvorgänge anzeigen | Microsoft Azure"
    description="Zeigen Sie an und verwalten Sie die Role-Based Access Control Arbeitsaufträge für alle Benutzer oder Gruppen in der Azure-portal"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="jeffsta"/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="view-access-assignments-for-users-and-groups-in-the-azure-portal---public-preview"></a>Anzeigen von Arbeitsaufträgen Zugriff für Benutzer und Gruppen in der Azure-Portal - Public preview

> [AZURE.SELECTOR]
- [Verwalten des Zugriffs nach Benutzer oder Gruppe](role-based-access-control-manage-assignments.md)
- [Verwalten des Zugriffs von Ressource](role-based-access-control-configure.md)

Mit Role-Based Access Control (RBAC) in der Vorschau Azure Active Directory können Sie Zugriff auf Ihre Azure Ressourcen verwalten. [Neuigkeiten in der Vorschau](active-directory-preview-explainer.md)

Mit RBAC zugewiesen ist abgestimmte zweierlei Berechtigungen einschränken können:

- **Bereich:** RBAC-Rollen zugewiesen sind bestimmte Abonnement, Ressourcengruppe oder Ressource beschränkt. Ein Benutzer erhält Zugriff auf eine Ressource kann keine Ressourcen in der gleichen Anmeldung zugreifen.
- **Funktion:** Im Rahmen der Zuordnung Zugriff eingeschränkt durch eine Rolle zuweisen. Rollen können auf höherer Ebene wie Besitzer oder bestimmte wie virtuelle Computer sein.

Rollen können nur von Abonnements, Ressourcengruppe oder Ressource ist für die Zuordnung zugewiesen werden. Jedoch können alle Access-Aufgaben für einen bestimmten Benutzer oder eine Gruppe an einem einzigen Ort.

Erhalten Sie weitere Informationen zum [verwenden Rolle Aufgaben zum Verwalten des Zugriffs auf Ressourcen Azure-Abonnement](role-based-access-control-configure.md).

##  <a name="view-access-assignments"></a>Access-Aufgaben anzeigen

Starten Sie zum Suchen nach Access-Aufgaben für einen einzelnen Benutzer oder eine Gruppe in Azure Active Directory in [Azure-Portal](http://portal.azure.com).

1. **Azure Active Directory**auswählen Wenn diese Option nicht auf der Navigationsleiste angezeigt wird, wählen Sie **Weitere Dienste** und unten **Azure Active Directory**blättern.
2. Wählen Sie **Benutzer und Gruppen**und dann entweder **alle Benutzer** oder **alle Gruppen**. In diesem Beispiel liegt der Schwerpunkt auf einzelne Benutzer.
    ![Verwalten von Benutzern und Gruppen in Azure Active Directory - screenshot](./media/role-based-access-control-manage-assignments/rbac_users_groups.png)
3. Suchen Sie nach Namen oder Benutzernamen des Benutzers.
4. Wählen Sie **Azure Ressourcen** , auf die Benutzer. Alle Access-Aufgaben für den Benutzer angezeigt.

### <a name="read-permissions-to-view-assignments"></a>Leseberechtigungen Sie für Aufgaben anzeigen

Diese Seite zeigt nur die Aufgaben zugreifen, die Berechtigung zum Lesen. Beispielsweise haben Lesezugriff auf ein Abonnement und zur Azure Ressourcen Blade Aufgaben des Benutzers zu überprüfen. Sehen ihre Access-Aufgaben für ein Abonnement, aber sehen, dass sie auch Zugriff auf Arbeitsaufträge Abonnements b

## <a name="delete-access-assignments"></a>Access-Aufgaben löschen

Dieses Blatt können Sie Access-Aufgaben löschen, die direkt an einen Benutzer oder eine Gruppe zugewiesen wurden. Wenn Access-Zuordnung einer übergeordneten Gruppe geerbt wurde, müssen Sie auf die Abonnements und Verwalten der Zuordnung.

1. Wählen Sie aus der Liste alle Access-Aufgaben für einen Benutzer oder eine Gruppe, die Sie löschen möchten.
2. Wählen Sie **Entfernen** und dann auf **Ja** zu bestätigen.
    ![Access Zuweisung - screenshot](./media/role-based-access-control-manage-assignments/delete_assignment.png)

## <a name="related-topics"></a>Verwandte Themen

- Erste Schritte mit Role-Based Access Control [verwendet Rolle](role-based-access-control-configure.md) Aufgaben zum Verwalten des Zugriffs auf Ressourcen Azure-Abonnement
- Finden Sie unter [integrierte RBAC-Rollen](role-based-access-built-in-roles.md)
