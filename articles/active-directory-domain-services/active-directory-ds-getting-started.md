<properties
    pageTitle="Azure Active Directory-Domänendienste: Erstellen der AAD DC-Administratorengruppe | Microsoft Azure"
    description="Erste Schritte mit Azure Active Directory-Domänendienste"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Erste Schritte mit Azure Active Directory-Domänendienste

Dieser Artikel führt durch die Konfigurationsaufgaben erforderlich Azure Active Directory-Domänendienste für Azure AD-Mandanten.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Aufgabe 1: Erstellen der Gruppe "Administratoren AAD DC"
Zunächst ist die Erstellung eine administrative Gruppe in Ihrem Mandanten Azure Active Directory. Dieser administrativen Gruppe heißt **AAD DC Administrators**. Mitglieder dieser Gruppe erhalten Administratorrechte auf Computern, die der verwaltete Azure Active Directory-Domänendienste-Domäne Domäne. Auf Domäne wird diese Gruppe der Gruppe "Administratoren" hinzugefügt. Außerdem können Mitglied dieser Gruppe Remotedesktop Sie Domäne beigetretenen Computer Remote-Verbindung.  

> [AZURE.NOTE] Sie müssen Domänenadministrator oder Organisationsadministrator-Berechtigungen in der verwalteten Domäne mit Azure Active Directory-Domänendienste erstellt keinen. Diese Berechtigungen sind vom Dienst reserviert verwalteten Domains und nicht stehen für Benutzer innerhalb der Mieter. In dieser Konfiguration erstellte spezielle Administratorgruppe können Sie jedoch einige privilegierte Vorgänge ausführen. Diese Operationen umfassen Zusammenfassen von Computern zu der Domäne zur Gruppe Administratoren auf Domäne Group Policy usw. konfigurieren.

In dieser Konfiguration die administrative Gruppe erstellen und der Gruppe einen oder mehrere Benutzer im Verzeichnis hinzufügen. Führen Sie die folgenden Schritte zum Erstellen der administrativen Gruppe für Azure Active Directory-Domänendienste:

1. Navigieren Sie zum **Azure-Verwaltungsportal** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. **Active Directory** -Knoten im linken Bereich auswählen.

3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) für den Azure Active Directory-Domänendiensten aktivieren möchten. Sie können nur eine Domäne für jede Azure AD-Verzeichnis erstellen.

    ![Wählen Sie Azure AD-Verzeichnis](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Klicken Sie auf die Registerkarte **Gruppen** .

5. Klicken Sie zum Hinzufügen einer Gruppe zu Azure AD-Mandanten im Aufgabenbereich am unteren Rand der Seite auf **Gruppe hinzufügen** .

    ![Schaltfläche hinzufügen](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Erstellen Sie eine Gruppe namens **AAD DC Administrators**. **GRUPPENTYP** auf **Sicherheit**festgelegt.

    > [AZURE.WARNING] Zum Aktivieren des Zugriffs innerhalb Ihrer Azure AD-Domänendienste verwalteten Domäne, eine Gruppe mit Namen erstellen.

    ![Gruppe erstellen](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Beschreibung für diese Gruppe so andere verstehen, dass diese Gruppe Administratorrechte in Azure Active Directory-Domänendienste gewährt wird.

8. Nachdem die Gruppe erstellt wurde, klicken Sie auf den Namen der Gruppe, die die Eigenschaften dieser Gruppe anzuzeigen. Um Benutzer als Mitglieder dieser Gruppe hinzuzufügen, klicken Sie **Mitglieder hinzufügen** auf den unteren Bereich.

    ![Schaltfläche der Gruppe Mitglieder hinzufügen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. Wählen Sie im Dialogfeld **Mitglieder hinzufügen** die Benutzer Mitglieder dieser Gruppe und aktivieren Sie das Kontrollkästchen, wenn Sie fertig sind.

    ![Hinzufügen von Benutzern zur Administratorgruppe](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Aufgabe 2: Erstellen Sie oder wählen Sie einer Azure virtual Network aus
Die nächste Konfigurationsaufgabe ist oder [Wählen ein Azure virtuelles Netzwerk](active-directory-ds-getting-started-vnet.md).
