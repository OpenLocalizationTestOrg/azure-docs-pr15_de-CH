<properties
    pageTitle="Verwenden Sie rollenbasierte Zugriffskontrolle in Azure-Portal | Microsoft Azure"
    description="Einstieg in Access Management Role-Based Access Control in Azure-Portal. Verwenden Sie Arbeitsaufträge für Benutzerrollen Zuweisen von Berechtigungen zu Ressourcen."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Arbeitsaufträge für Benutzerrollen mit dem Zugriff auf Ihre Ressourcen Azure-Abonnement verwalten

> [AZURE.SELECTOR]
- [Verwalten des Zugriffs nach Benutzer oder Gruppe](role-based-access-control-manage-assignments.md)
- [Verwalten des Zugriffs von Ressource](role-based-access-control-configure.md)

(Azure Role-Based Access Control, RBAC) ermöglicht die detaillierte Verwaltung für Azure. RBAC verwenden, können Sie dem Ausmaß des Zugriffs erteilen Benutzer für ihre Aufgaben benötigen. Dieser Artikel hilft Ihnen Betrieb mit RBAC in Azure-Portal. Ausführliche Informationen dazu, wie Rollenbasierten Zugriff verwalten kann, finden Sie unter [Was ist Role-Based Access Control](role-based-access-control-what-is.md).

## <a name="view-access"></a>Lesezugriff
Sie sehen, wer hat Zugriff auf eine Ressource, eine Ressourcengruppe oder eine Abonnement aus seiner wichtigsten Blade in [Azure-Portal](https://portal.azure.com). Zum Beispiel wollen wir Zugriff auf einen unserer Ressourcengruppen:

1. **Ressourcengruppen** in der Navigationsleiste auf der linken Seite auswählen  
    ![Ressourcengruppen - Symbol](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Wählen Sie den Namen der Ressourcengruppe aus dem Blade **Ressourcengruppen** .
3. **Zugriffskontrolle (IAM)** aus dem linken Menü auswählen  
4. Access Control Blade Listet alle Benutzer, Gruppen und Applikationen Ressource Zugriff gewährt wurde.  

    ![Benutzer-Blade - zugewiesen geerbte Vs Zugriff screenshot](./media/role-based-access-control-configure/view-access.png)

Beachten Sie, dass einige Benutzer **zugewiesen** wurden andere **Inherited** darauf zugreifen. Zugriff speziell für die Ressourcengruppe zugewiesen oder eine Zuordnung zum übergeordneten Abonnement geerbt.

> [AZURE.NOTE] Klassische Abonnement Admins und co-Admins gelten als Eigentümer des Abonnements im neuen RBAC-Modell.


## <a name="add-access"></a>Hinzufügen des Zugriffs
Zugriff aus Ressourcen, Ressourcengruppe oder Abonnement ist die Zuweisung der Sicherheitsrolle gewähren.

1. Wählen Sie auf der Access-Steuerelement **Hinzufügen** .  
2. Wählen Sie die Rolle, die Sie aus dem Blade **Wählen Sie eine Rolle** zuweisen möchten.
3. Wählen Sie die Benutzer, die Gruppe oder die Anwendung im Verzeichnis, dem Sie gewähren möchten. Sie können das Verzeichnis mit Namen, e-Mail-Adressen und Objektbezeichner suchen.  

    ![Hinzufügen Benutzer Blade - Screenshot suchen](./media/role-based-access-control-configure/grant-access2.png)

4. Wählen Sie **OK** , um die Aufgabe zu erstellen. **Hinzufügen von Benutzer** -Popup verfolgt den Fortschritt.  
    ![Hinzufügen von Benutzer-Statusanzeige - screenshot](./media/role-based-access-control-configure/addinguser_popup.png)

Zuweisung der Sicherheitsrolle erfolgreich hinzugefügt, wird er auf dem **Benutzer** angezeigt.

## <a name="remove-access"></a>Entfernen

1. Wählen Sie den Arbeitsauftrag Rolle auf das Access Control Blade.
2. Wählen Sie im Blatt Details Zuordnung **Entfernen** .  
3. Wählen Sie **Ja** zu bestätigen.  
    ![Benutzer Blade - Screenshot Rolle entfernen](./media/role-based-access-control-configure/remove-access1.png)

Geerbte Arbeitsaufträge können nicht entfernt werden. Beachten Sie in der folgenden Abbildung, dass die Entfernen-Taste deaktiviert ist. Sehen Sie die Details **Zugewiesen an** . Fahren Sie mit der Ressource entfernen die Funktion Zuordnung vorhanden.

![Benutzer-Blade - entfernen geerbten deaktiviert die Schaltfläche screenshot](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Andere Tools zum Verwalten des Zugriffs
Sie können Rollen zuweisen und Verwalten mit Azure RBAC Befehle Tools als Azure-Portal.  Führen Sie die Links, um weitere Informationen zu den erforderlichen Komponenten und mit Azure RBAC-Befehle.

- [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
- [REST-API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Nächste Schritte
- [Erstellen einer Access Änderungshistorie-Berichte](role-based-access-control-access-change-history-report.md)
- Finden Sie unter [integrierte RBAC-Rollen](role-based-access-built-in-roles.md)
- Definieren Sie eigene [benutzerdefinierte Funktionen in Azure RBAC](role-based-access-control-custom-roles.md)
