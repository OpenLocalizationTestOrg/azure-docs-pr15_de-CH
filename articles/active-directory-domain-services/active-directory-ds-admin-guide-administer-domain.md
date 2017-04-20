<properties
    pageTitle="Azure Active Directory Domain Services: Verwalten eine verwaltete Domäne | Microsoft Azure"
    description="Azure Active Directory Domain Services verwalteten Domains verwalten"
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
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Verwalten Sie eine Azure Active Directory Domain Services verwaltete Domäne
Dieser Artikel beschreibt, wie eine verwaltete Domäne Azure Active Directory (AD) Domain Services verwalten.


## <a name="before-you-begin"></a>Bevor Sie beginnen
In diesem Artikel aufgelisteten Aufgaben, müssen wie folgt vor:

1. Ein gültiges **Azure-Abonnement**.

2. Ein **Azure AD-Verzeichnis** - entweder mit einem lokalen Verzeichnis oder ein Verzeichnis nur Cloud synchronisiert.

3. **Azure Active Directory-Domänendienste** muss für Azure AD-Verzeichnis aktiviert. Wenn Sie dies getan haben, führen Sie alle Aufgaben im [Handbuch Erste Schritte](./active-directory-ds-getting-started.md)beschrieben.

4. Einem **virtuellen Domäne** aus dem Azure AD-Domänendienste verwaltete Domäne verwalten. Haben Sie diese virtuellen Computer, führen Sie alle Aufgaben gemäß dem Artikel [einem Windows-Computer zu einer verwalteten Domäne beitreten](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Sie benötigen die Anmeldeinformationen für ein **Benutzerkonto zur Gruppe ' AAD DC Administrators'** in das Verzeichnis der verwalteten Domäne verwalten.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Verwaltungsaufgaben in einer verwalteten Domäne ausführen
Mitglieder der Gruppe 'AAD DC Administrators' erhalten auf der verwalteten Domäne verfügt, die sie ausführen können:

- Fügen Sie Computer verwalteten Domäne.

- Konfigurieren Sie das integrierte Gruppenrichtlinienobjekt für die "AADDC Computer und"AADDC Users"in der verwalteten Domäne.

- Verwalten von DNS im verwalteten Bereich.

- Erstellen und Verwalten von benutzerdefinierten Organisationseinheiten (OUs) der verwalteten Domäne.

- Administrativen Zugriff auf Computern der verwalteten Domäne hinzugefügt.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Administratorrechte müssen Sie nicht in einer verwalteten Domäne
Die Domäne wird von Microsoft, einschließlich wie patching, Überwachung und Durchführung von Backups. Daher die Domäne gesperrt, und Sie verfügen nicht über Berechtigungen für bestimmte administrative Aufgaben in der Domäne. Einige Aufgaben können nicht zählen unter.

- Domänenadministrator oder Organisationsadministrator Administratorrechte für die verwalteten Domäne werden nicht gewährt.

- Das Schema der verwalteten Domäne kann nicht erweitert werden.

- Sie Verbindung keine Domänencontroller verwalteten Domäne mithilfe von Remotedesktop.

- Sie können nicht verwalteten Domäne Domänencontroller hinzufügen.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Aufgabe 1: Bereitstellung Domäne Windows Server virtuellen Computer remote verwaltete Domäne verwalten
Azure Active Directory-Domänendienste verwaltete Domänen können mit vertrauten Active Directory-Verwaltungstools wie Active Directory Administrative Center (ADAC) oder AD PowerShell verwaltet werden. Tenant-Administratoren verfügen nicht Berechtigungen zum Domänencontroller verwalteten Domäne über Remotedesktop herstellen. Daher können Mitglieder der Gruppe 'AAD DC Administrators' verwaltete Domänen Remote mit AD-Verwaltung auf einem Windows Server-Client-Computer, der verwalteten Domäne angehört verwalten. AD-Verwaltungstools können als Teil der Remote Server Administration Tools (RSAT) optionales Feature Windows Server- und Clientcomputern der verwalteten Domäne installiert.

Zunächst wird ein Windows Server virtueller Computer einrichten, der verwalteten Domäne angehört. Eine Anleitung finden Sie im Artikel [verknüpfen ein Windows Server virtueller Computer einer Domäne Azure Active Directory Domain Services verwaltet](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Remoteverwaltung von verwalteten Domäne von einem Clientcomputer (z. B. Windows 10)
Informationen in diesem Artikel eine Windows Server-VM AAD-DS verwalten verwalteten Domäne. Sie können jedoch auch einen Windows-Client (z. B. Windows 10) virtuellen Computer verwenden.

Sie können auf einem Windows-Client-Computer [installieren (Remote Server Administration Tools RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) gemäß der Anleitung auf TechNet.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Aufgabe 2: installieren Active Directory-Verwaltungstools auf dem virtuellen Computer
Führen Sie die folgenden Schritte zum Installieren von Active Directory-Verwaltungsprogramme auf VM-Domäne beitreten. Weitere [Informationen zur Installation und Verwendung der Remoteserver-Verwaltungstools](https://technet.microsoft.com/library/hh831501.aspx)finden Sie in Technet.

1. Navigieren Sie zum **virtuellen Computer** Knoten im klassischen Azure-Portal. Wählen Sie die virtuellen Computer in Aufgabe 1 erstellte und auf **Verbinden** auf der Befehlsleiste am unteren Rand des Fensters.

    ![Verbinden Sie mit virtuellen Windows-Maschine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Das Verwaltungsportal fordert Sie zum Öffnen oder Speichern einer Datei mit der Erweiterung "RDP", die zum Verbinden mit dem virtuellen Computer. Klicken Sie auf, um die Datei zu öffnen, wenn der Download abgeschlossen ist.

3. Verwenden Sie zur Anmeldung aufgefordert die Anmeldeinformationen eines Benutzers zur Gruppe 'AAD DC Administrators'. Beispielsweise verwenden wir 'bob@domainservicespreview.onmicrosoft.com' in unserem Fall.

4. Öffnen Sie **Server-Manager**aus dem Startbildschirm. Klicken Sie im mittleren Bereich des Server-Managers **Hinzufügen von Rollen und Features** auf.

    ![Starten Sie Server-Manager auf dem virtuellen Computer](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Klicken Sie auf der Seite **Vorbereitung** **Hinzufügen von Rollen und Features Assistent**auf **Weiter**.

    ![Bevor Sie beginnen Seite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Lassen Sie auf der Seite **Installationsart** die **Rolle oder Funktion-basierten** Installationsoption aktiviert und auf **Weiter**.

    ![Installationsseite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Wählen Sie auf der Seite **Auswahl** der aktuellen virtuellen Maschine aus den Server aus und auf **Weiter**.

    ![Serverseite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Klicken Sie auf der Seite **Serverrollen** auf **Weiter**. Wir überspringen diese Seite, da wir keine Rollen auf dem Server installieren.

9. Auf der Seite **Funktionen** den **Remoteserver-Verwaltungstools** Knoten erweitern und erweitern Sie den Knoten **Rollenverwaltungstools** klicken. Wählen Sie Funktion **AD DS und AD LDS-Tools** aus Rollenverwaltungstools.

    ![Features-Seite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Klicken Sie auf **der Bestätigungsseite** , installieren die Anzeige und AD LDS-Tools auf dem virtuellen Computer **Installieren** . Nach erfolgreichem Abschluss der Installation von Features klicken Sie auf **Schließen** , beenden Sie den Assistenten zum **Hinzufügen von Rollen und Features** .

    ![Bestätigungsseite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Aufgabe 3: Öffnen und verwaltete Domäne durchsuchen
Jetzt auf die Active Directory-Verwaltungsprogramme installiert sind virtuelle Computer mit der Domäne verknüpft, können diese Tools zu verwaltete Domäne verwalten.

> [AZURE.NOTE] Sie müssen Mitglied der Gruppe 'AAD DC Administrators' verwaltete Domäne verwalten.

1. Klicken Sie auf " **Verwaltung**" aus dem Startbildschirm. Die AD-Verwaltungstools auf dem virtuellen Computer installiert sollte angezeigt werden.

    ![Verwaltung auf Server installiert](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Klicken Sie auf **Active Directory-Verwaltungscenter**.

    ![Active Directory-Verwaltungscenter](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Klicken Sie auf den Domänennamen im linken Bereich (z. B. "contoso100.com") zu der Domäne. Beachten Sie die zwei Container namens AADDC Computer"und"AADDC Users".

    ![ADAC - Domäne anzeigen](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Klicken Sie auf den Behälter **AADDC Benutzer** alle Benutzer und Gruppen der verwalteten Domäne. Sie sollten Benutzerkonten und Gruppen von Azure AD Mieter anzeigen in diesem Container. Beachten Sie dabei, ein Benutzerkonto für den Benutzer namens "bob" und einer Gruppe namens 'AAD DC Administrators' stehen in diesem Container.

    ![ADAC - Domänenbenutzer](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Klicken Sie auf den Container **Computer AADDC** der Computer diese verwalteten Domäne bezeichnet. Einen Eintrag für den aktuellen virtuellen Computer, der Mitglied der Domäne ist, sollte angezeigt werden. Computerkonten für alle Computer, die die verwalteten Azure Active Directory-Domänendienste-Domäne angehören, werden in diesem Container "AADDC Computer gespeichert.

    ![ADAC - Domäne Computer](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Verwandte Inhalte

- [Azure Active Directory-Domänendienste - erste Schritte](./active-directory-ds-getting-started.md)

- [Einen virtuellen Windows Server-Computer zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Bereitstellen von Remoteserver-Verwaltungstools](https://technet.microsoft.com/library/hh831501.aspx)
