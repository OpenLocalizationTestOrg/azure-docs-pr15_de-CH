<properties
    pageTitle="Azure Active Directory Domain Services: Administration Guide | Microsoft Azure"
    description="Erstellen Sie eine Organisationseinheit (OU) auf Azure AD-Domänendienste verwalteten Domänen"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Erstellen Sie eine Organisationseinheit (OU) in einer verwalteten Domäne Azure Active Directory-Domänendienste
Azure Active Directory-Domänendienste verwaltete Domänen umfassen zwei integrierte Container namens AADDC Computer"und"AADDC Users". ' AADDC' Computercontainer hat Computerobjekte für alle Computer, die der verwalteten Domäne angehören. ' AADDC' Benutzercontainer enthält Benutzer und Gruppen in Azure AD-Mandanten. Gelegentlich kann zu Dienstkonten in der verwalteten Domäne bereitstellen Arbeitslasten erforderlich. Zu diesem Zweck können Sie eine benutzerdefinierte Organisationseinheit (OU) auf verwalteten Domäne erstellen und Dienstkonten innerhalb dieser Organisationseinheit erstellen. Dieser Artikel veranschaulicht das Erstellen einer Organisationseinheit in der verwalteten Domäne.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Installieren von Active Directory-Verwaltungstools auf einem Domäne-Computer für die Remoteverwaltung
Azure Active Directory-Domänendienste verwaltete Domänen können mit den vertrauten Active Directory-Verwaltung wie Active Directory Administrative Center (ADAC) oder AD PowerShell remote verwaltet werden. Tenant-Administratoren verfügen nicht Berechtigungen zum Domänencontroller verwalteten Domäne über Remotedesktop herstellen. Installieren Sie zum Verwalten der verwalteten Domäne AD Administration Tools-Funktion auf einem virtuellen Computer der verwalteten Domäne. Finden Sie im Artikel Informationen [verwalten eine Domäne Azure Active Directory Domain Services verwaltet](active-directory-ds-admin-guide-administer-domain.md) .

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Erstellen einer Organisationseinheit in der verwalteten Domäne
Jetzt auf die Active Directory-Verwaltungsprogramme installiert sind virtuelle Computer mit der Domäne verknüpft, können wir diese Tools erstellen Sie eine Organisationseinheit in der verwalteten Domäne. Führen Sie die folgenden Schritte aus:

> [AZURE.NOTE] Nur Mitglieder der Gruppe "Administratoren AAD DC" haben die erforderlichen Berechtigungen zum Erstellen einer benutzerdefinierten OE. Stellen Sie sicher, dass Sie als Benutzer gehen, der dieser Gruppe angehört.

1. Klicken Sie auf " **Verwaltung**" aus dem Startbildschirm. Die AD-Verwaltungstools auf dem virtuellen Computer installiert sollte angezeigt werden.

    ![Verwaltung auf Server installiert](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klicken Sie auf **Active Directory-Verwaltungscenter**.

    ![Active Directory-Verwaltungscenter](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Klicken Sie die Domäne, den Domänennamen im linken Bereich (z. B. ' contoso100.com').

    ![ADAC - Domäne anzeigen](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. Klicken Sie im **Aufgabenbereich rechts** auf **neu** unter dem Namen Domänenknoten. In diesem Beispiel klicken wir unter dem Knoten "contoso100(local)" im **Aufgabenbereich rechts** auf **neu** .

    ![ADAC - Organisationseinheit](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Erstellen einer Organisationseinheit sollte angezeigt werden. Klicken Sie auf **Organisationseinheit** , um das Dialogfeld **Organisationseinheit erstellen** .

6. Geben Sie im Dialogfeld **Organisationseinheit erstellen** einen **Namen** für die neue Organisationseinheit. Geben Sie eine kurze Beschreibung für die Organisationseinheit. Sie können auch **Verwaltet von** -Feld für die Organisationseinheit festlegen. Um benutzerdefinierte Organisationseinheit zu erstellen, klicken Sie auf **OK**.

    ![ADAC - Dialogfeld Organisationseinheit erstellen](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. Die neu erstellte Organisationseinheit wird jetzt in Active Directory Administrative Center (ADAC) angezeigt.

    ![ADAC - Organisationseinheit erstellt](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Sicherheit/Berechtigungen für neu erstellte Organisationseinheiten
Standardmäßig erhält der Benutzer (Mitglied der Gruppe "Administratoren AAD DC"), der benutzerdefinierte Organisationseinheit erstellt der Organisationseinheit Administratorrechte (Vollzugriff). Der Benutzer kann weiter und Erteilen von Berechtigungen an andere Benutzer oder der Gruppe "AAD DC Administrators" wie gewünscht. Wie im folgenden Screenshot der Benutzer 'bob@domainservicespreview.onmicrosoft.com' , die neue Organisationseinheit "MyCustomOU" erstellt, erhält vollständige Kontrolle.

 ![ADAC - neue Organisationseinheit security](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Hinweise zur benutzerdefinierte Organisationseinheiten verwalten
Erstellung eine benutzerdefinierte OE können Sie fortfahren und Benutzer, Gruppen, Computern und Dienstkonten in dieser Organisationseinheit erstellen. Sie können nicht von 'AAD DC 'OU Benutzer Benutzer oder Gruppen mit benutzerdefinierten Organisationseinheiten verschieben.

> [AZURE.WARNING] Benutzerkonten, Gruppen, Dienstkonten und Computerobjekte, die auf benutzerdefinierten Organisationseinheiten erstellen stehen nicht in Azure AD-Mandanten. In anderen Worten, Anzeigen diese Objekte nicht mit der Azure AD Graph-API oder Azure AD-Benutzeroberfläche. Diese Objekte stehen nur in der verwalteten Domäne Azure Active Directory-Domänendienste.


## <a name="related-content"></a>Verwandte Inhalte

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](active-directory-ds-admin-guide-administer-domain.md)

- [Active Directory-Verwaltungscenter: Erste Schritte](https://technet.microsoft.com/library/dd560651.aspx)

- [Schrittweise Anleitung für Dienstkonten](https://technet.microsoft.com/library/dd548356.aspx)
