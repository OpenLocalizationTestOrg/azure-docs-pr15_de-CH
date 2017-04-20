<properties
  pageTitle="Azure IoT Suite und Azure Active Directory | Microsoft Azure"
  description="Beschreibt, wie Azure IoT Suite Azure Active Directory verwendet, um Berechtigungen zu verwalten."
  services=""
  suite="iot-suite"
  documentationCenter=""
  authors="aguilaaj"
  manager="timlt"
  editor=""/>

<tags
  ms.service="iot-suite"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="na"
  ms.date="10/24/2016"
  ms.author="araguila"/>
  
# <a name="permissions-on-the-azureiotsuitecom-site"></a>Berechtigungen auf der Website azureiotsuite.com

## <a name="what-happens-when-you-sign-in"></a>Was geschieht, wenn Sie sich anmelden

Bei Ihrer ersten Anmeldung bei [azureiotsuite.com][lnk-azureiotsuite], der Standort bestimmt die Berechtigungsstufen auf den aktuell ausgewählten Azure Active Directory (AAD) Mieter und Azure-Abonnement basierend sind.

1.  Die Website erfährt zuerst von Azure die AAD Mieter angehören zum Füllen der Liste der Mandanten neben Ihrem angemeldeten Benutzernamen angezeigt. Zu diesem Zeitpunkt kann die Website nur Benutzertoken für einen Mandanten gleichzeitig abrufen. Daher beim Wechseln zu einem anderen Mandanten mithilfe der Dropdownliste oben rechts in meldet erneut die Website in diesem Mandanten zu Token, Mieter.

2.  Dann erfährt die Website von Azure welche Abonnements Sie ausgewählte Mandanten zugeordnet haben. Verfügbare Abonnements wird angezeigt, wenn eine neue vorkonfigurierte Lösung erstellen.

3.  Schließlich ruft die Website alle Ressourcen in der Abonnements und Ressourcengruppen als vorkonfigurierte Lösung und füllt die Kacheln auf der Startseite.

Die folgenden Abschnitte beschreiben die Rollen, die Zugriffskontrolle vorkonfigurierte Lösungen.

## <a name="aad-roles"></a>AAD Rollen

AAD Rollen Möglichkeit Bereitstellung vorkonfigurierte Lösungen steuern und Verwalten von Benutzern in eine vorkonfigurierte Lösung.

Finden Sie weitere Informationen über Administratorfunktionen in AAD [Zuweisen von]Administratorrollen in Azure AD[lnk-aad-admin], aber dieser Artikel konzentriert sich hauptsächlich auf den **Globalen Administrator** und **Mitglied der Domäne/Benutzer** Rollen, vorkonfigurierten Lösungen verwendet.

**Globale Administrator:** Es kann viele globale Administratoren pro AAD-Mandanten. Beim Erstellen eines AAD-Mandanten sind Sie standardmäßig der globale Administrator von diesem Mandanten. Der globale Administrator kann eine vorkonfigurierte Lösung bereitstellen und **eine Administratorrolle für die Anwendung in ihre AAD-Mandanten** zugewiesen. Wenn ein anderer Benutzer denselben AAD Mandanten eine Anwendung erstellt, ist jedoch die Standardrolle gewährt der globale Administrator **Implizit schreibgeschützt**. Globale Administratoren können Rollen für Applikationen mit der [Azure-Verwaltungsportal][lnk-classic-portal].

**Mitglied der Domäne/Benutzer:** Es kann viele Benutzer/Domänenmitglieder pro AAD-Mandanten. Domänenbenutzer kann eine vorkonfigurierte Lösung [azureiotsuite.com] Bereitstellung[ lnk-azureiotsuite] Website. Die Standardrolle, die sie für die Anwendung gewährt werden, die sie bereitstellen ist **ADMINISTRATOR**. Sie erstellen eine Anwendung, das build.cmd-Skript in der [Azure-Iot-Remote-Überwachung] [ lnk-rm-github-repo] oder [Azure-Iot-vorhersehbaren-Wartung] [ lnk-pm-github-repo] Repository aber Standardrolle erteilt ist **Implizit schreibgeschützt**, da sie nicht über die Berechtigung zum Zuweisen von Rollen. Wenn ein anderer Benutzer in AAD Mandanten eine Anwendung erstellt, werden sie **Implizite READONLY** -Rolle für die Anwendung standardmäßig zugewiesen. Zuweisen von Rollen für Applikationen verfügen; Daher können Benutzer oder Rollen für Benutzer einer Anwendung hinzufügen, wenn auch sie bereitgestellt.

**Gast Gastbenutzer:** Viele Gäste Benutzer/Gäste pro AAD Mieter kann. Gastbenutzer haben eine begrenzte Rechte AAD-Mandanten. Gastbenutzer können nicht dadurch eine vorkonfigurierte Lösung AAD Mandanten bereitstellen.

Weitere Informationen finden Sie in folgenden Ressourcen:

- [Benutzer erstellen oder Bearbeiten in Azure AD][lnk-create-edit-users]
- [Zuweisen von Rollen in AAD App][lnk-assign-app-roles]

## <a name="azure-subscription-administrator-roles"></a>Azure-Abonnement-Administratorfunktionen

Azure Administratorrollen gesteuert AD-Mandanten Azure-Abonnement zuzuordnen.

Erfahren Sie mehr über Azure Co-Administrator und Dienstadministrator Kontoadministrator Rollen im Artikel [Hinzufügen oder Ändern von Azure Co-Administrator, Dienstadministratoren und Konto Administrator][lnk-admin-roles].

## <a name="application-roles"></a>Anwendungsrollen

Anwendungsrollen steuern den Zugriff auf Geräte in der vorkonfigurierte Lösung.

Es gibt zwei definiert und eine implizite Rolle der Anwendung erstellt wird, wenn Sie eine vorkonfigurierte Lösung bereitstellen.

-   **ADMINISTRATOR:** Vollzugriff hinzufügen, verwalten und Entfernen von Geräten

-   **Schreibgeschützt:** Kann Geräte anzeigen

-   **Implizit schreibgeschützt:** Dies entspricht nur lesen jedoch gewährt allen Benutzern AAD-Mandanten. Dies wurde während der Entwicklung aus praktischen Gründen durchgeführt. Entfernen Sie diese Rolle ändern [RolePermissions.cs] [ lnk-resource-cs] Datei.

### <a name="changing-application-roles-for-a-user"></a>Anwendung Rollen für einen Benutzer

Das folgende Verfahren können zu einem Benutzer in Active Directory Administrator vorkonfigurierte Lösung.

AAD globalen Administrator Rollen für einen Benutzer ändern muss:

1. [Azure-Verwaltungsportal]zu[lnk-classic-portal].

2. Wählen Sie **Active Directory**.

3. Klicken Sie auf die AAD-Mandanten (Dies ist das Verzeichnis gewählte auf azureiotsuite.com beim Bereitstellen der Lösung).

4. Klicken Sie auf **Anwendung**.

5. Klicken Sie auf den Namen der Anwendung, die die vorkonfigurierte Lösung übereinstimmt. Wenn die Anwendung in der Liste nicht angezeigt wird, wechseln Sie **Applikationen Mein Unternehmen** Dropdownliste **Anzeigen** und klicken Sie auf das Häkchen.

7. Klicken Sie auf **Benutzer**.

8. Wählen Sie den Benutzer die Rollen wechseln möchten.

9. Klicken Sie auf **zuweisen** und wählen Sie die Rolle (z. B. **Admin**) möchten Benutzer zuzuweisen, klicken Sie auf das Häkchen.

## <a name="faq"></a>Häufig gestellte Fragen

### <a name="im-a-service-administrator-and-id-like-to-change-the-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-do-this"></a>Ich bin ein Dienstadministrator und verzeichniszuordnung zwischen Abonnement und einem bestimmten AAD-Mandanten ändern möchte. Wie funktioniert dies?

1. [Azure-Verwaltungsportal]zu[lnk-classic-portal], **Klicken Sie in der Liste auf der linken Seite** .

2. Wählen Sie das Abonnement die verzeichniszuordnung ändern möchten.

3. Klicken Sie auf **Verzeichnis bearbeiten**.

4. Wählen Sie das **Verzeichnis** in der Dropdown-Liste verwenden möchten. Klicken Sie auf den Vorwärtspfeil.

5. Directory-Zuordnung bestätigen und Co-Administratoren betroffen. Beachten Sie, dass Sie ein anderes Verzeichnis verschieben, alle CO-Administratoren aus dem ursprünglichen Verzeichnis entfernt werden.

### <a name="im-a-domain-usermember-on-the-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a>Ich bin Mitglied einer Domäne Benutzer/AAD Mieter und ich habe eine vorkonfigurierte Lösung. Wie führen Sie ich eine Rolle für die Anwendung zugewiesen?

Bitten Sie einen globalen Administrator als globaler Administrator AAD Mieter zu Berechtigungen zuweisen von Rollen für Benutzer selbst zuweisen oder bitten Sie einen globalen Administrator, eine Rolle zuweisen. AAD Mieter vorkonfigurierte Lösung ändern möchten wurde bereitgestellt, um die nächste Frage anzuzeigen.

### <a name="how-do-i-switch-the-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a>Wie wechsle ich AAD Mieter Meine remote monitoring vorkonfigurierte Lösung und Anwendung zugeordnet?

Sie können eine Cloudbereitstellung von <https://github.com/Azure/azure-iot-remote-monitoring> ausführen und eine neu erstellte AAD Mieter erneut. Sie zugreifen zum Hinzufügen von Benutzern und Zuweisen von Rollen zu Benutzern, da Sie standardmäßig als globaler Administrator beim Erstellen eines neuen AAD-Mandanten sind.

1. Erstellen Sie ein neues Verzeichnis AAD in [Azure-Verwaltungsportal][lnk-classic-portal].

2. Gehen Sie zu <https://github.com/Azure/azure-iot-remote-monitoring>.

3. Ausführen `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (z. B. `build.cmd cloud debug myRMSolution`)

4. Bei Aufforderung legen Sie **Tenantid** auf die neu erstellte Mieter anstelle der vorherigen Mandanten.


### <a name="i-want-to-change-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a>Sie möchten ein Dienstadministrator oder Co-Administrator beim Anmelden mit einem organisatorischen Konto ändern

Finden Sie im Artikel [Ändern Service und Co-Administratoren mit einem organisatorischen Konto angemeldet][lnk-service-admins].

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-the-proper-permissions-to-create-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a>Warum sehe ich diesen Fehler? "Ihr Konto besitzt nicht die erforderlichen Berechtigungen zum Erstellen einer Lösung. Bitte wenden Sie sich an den Kontoadministrator oder mit einem anderen Konto."

Betrachten Sie das Diagramm:

![][img-flowchart]

> [AZURE.NOTE] Wenn Sie weiterhin der Fehler nach dem validieren Sie ein globaler Administrator AAD Mieter und Co-Administrator das Abonnement, Ihren Konto-Administrator zu entfernen und erneut zuzuweisen erforderlichen Berechtigungen in dieser Reihenfolge: Fügen Sie den Benutzer als globaler Administrator und Benutzer als Co-Administrator auf der Azure-Abonnement hinzufügen. Bleiben die Probleme bestehen, wenden Sie [Hilfe und Support][lnk-help-support].

**Warum sehe ich diesen Fehler, wenn der Azure-Abonnement ist?** *Ein Azure-Abonnement muss vorkonfigurierte Lösung erstellen. Sie können ein kostenloses Testabo in wenigen Minuten erstellen.*

Wenn Sie sicher sind, dass ein Azure-Abonnement haben, überprüfen Sie Pächter für Ihr Abonnement zu und sicherzustellen Sie, dass der richtige Mieter in der Dropdownliste ausgewählt ist. Wenn Sie überprüft haben den gewünschten Mandanten richtig, folgen Sie das Diagramm oben und überprüfen Sie die Zuordnung Ihres Abonnements und AAD-Mandanten.

## <a name="next-steps"></a>Nächste Schritte

Weiter kennenlernen IoT Suite sehen, wie Sie [eine vorkonfigurierte Lösung anpassen][lnk-customize].

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-aad-admin]: https://azure.microsoft.com/documentation/articles/active-directory-assign-admin-roles/
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-create-edit-users]: https://azure.microsoft.com/documentation/articles/active-directory-create-users/
[lnk-assign-app-roles]: https://azure.microsoft.com/documentation/articles/active-directory-application-manifest/
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: https://azure.microsoft.com/documentation/articles/billing-add-change-azure-subscription-administrator/
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
