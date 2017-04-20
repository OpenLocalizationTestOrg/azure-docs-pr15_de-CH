<properties
    pageTitle="Azure Active Directory Domain Services: DNS auf verwalteten Domains verwalten | Microsoft Azure"
    description="Verwalten Sie DNS auf Azure Active Directory Domain Services verwalteten domains"
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

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Verwalten von DNS in einer verwalteten Domäne Azure Active Directory-Domänendienste
Azure Active Directory Domain Services enthält einen DNS (Domain Name Resolution)-Server, der DNS-Auflösung für die verwalteten Domäne bereitstellt. Gelegentlich müssen Sie DNS auf verwalteten Domäne konfigurieren. Sie müssen DNS-Einträge für Computer, die keiner Domäne angehören, virtuelle IP-Adressen für Lastenausgleich konfiguriert oder externen DNS-Weiterleitung einrichten. Aus diesem Grund werden Benutzer der Gruppe "Administratoren AAD DC" DNS-Administratorrechte in der verwalteten Domäne gewährt.


## <a name="before-you-begin"></a>Bevor Sie beginnen
In diesem Artikel aufgelisteten Aufgaben, müssen wie folgt vor:

1. Ein gültiges **Azure-Abonnement**.

2. Ein **Azure AD-Verzeichnis** - entweder mit einem lokalen Verzeichnis oder ein Verzeichnis nur Cloud synchronisiert.

3. **Azure Active Directory-Domänendienste** muss für Azure AD-Verzeichnis aktiviert. Wenn Sie dies getan haben, führen Sie alle Aufgaben im [Handbuch Erste Schritte](./active-directory-ds-getting-started.md)beschrieben.

4. Einem **virtuellen Domäne** aus dem Azure AD-Domänendienste verwaltete Domäne verwalten. Haben Sie diese virtuellen Computer, führen Sie alle Aufgaben gemäß dem Artikel [einem Windows-Computer zu einer verwalteten Domäne beitreten](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Sie benötigen die Anmeldeinformationen für ein **Benutzerkonto zur Gruppe "AAD DC Administrators"** in Ihrem Verzeichnis DNS für Ihre verwalteten Domäne verwalten.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Aufgabe 1: Bereitstellung einer virtuellen Maschine Domäne zur Remoteverwaltung von DNS für die verwalteten Domäne
Azure Active Directory-Domänendienste verwaltete Domänen können mit den vertrauten Active Directory-Verwaltung wie Active Directory Administrative Center (ADAC) oder AD PowerShell remote verwaltet werden. Ebenso kann DNS für verwalteten Domäne mit dem DNS-Server-Verwaltungsprogrammen remote verwaltet werden.

Administratoren in Azure AD-Verzeichnis verfügen nicht Berechtigungen zum Domänencontroller verwalteten Domäne über Remotedesktop herstellen. Mitglieder der Gruppe 'AAD DC Administrators' können DNS verwalten, verwalteten Domänen Remote mit DNS-Server-Tools auf einem Windows Server-Client-Computer, der verwalteten Domäne angehört. DNS-Server Tools können als Teil der Remote Server Administration Tools (RSAT) optionales Feature Windows Server- und Clientcomputern der verwalteten Domäne installiert.

Zunächst wird einen Windows Server virtuellen Computer bereitstellen, der verwalteten Domäne angehört. Eine Anleitung finden Sie im Artikel [verknüpfen ein Windows Server virtueller Computer einer Domäne Azure Active Directory Domain Services verwaltet](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Aufgabe 2: DNS-Servertools auf dem virtuellen Computer installieren
Die folgenden Schritte installieren DNS-Verwaltungstools auf dem virtuellen Computer Domäne hinzugefügt. Weitere Informationen zur [Installation und Verwendung der Remoteserver-Verwaltungstools](https://technet.microsoft.com/library/hh831501.aspx)finden Sie auf Technet.

1. Navigieren Sie zum **virtuellen Computer** Knoten im klassischen Azure-Portal. Wählen Sie die virtuellen Computer in Aufgabe 1 erstellte und auf **Verbinden** auf der Befehlsleiste am unteren Rand des Fensters.

    ![Verbinden Sie mit virtuellen Windows-Maschine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Das Verwaltungsportal fordert Sie zum Öffnen oder Speichern einer Datei mit der Erweiterung "RDP", die zum Verbinden mit dem virtuellen Computer. Klicken Sie auf die Datei heruntergeladen wurde.

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

9. Auf der Seite **Funktionen** den **Remoteserver-Verwaltungstools** Knoten erweitern und erweitern Sie den Knoten **Rollenverwaltungstools** klicken. Wählen Sie **DNS-Server** die Programmfunktion aus Rollenverwaltungstools.

    ![Features-Seite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Klicken Sie auf **der Bestätigungsseite** auf Feature Tools DNS-Server auf dem virtuellen Computer installieren **Installieren** . Nach erfolgreichem Abschluss der Installation von Features klicken Sie auf **Schließen** , beenden Sie den Assistenten zum **Hinzufügen von Rollen und Features** .

    ![Bestätigungsseite](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Aufgabe 3: Starten der DNS-Verwaltungskonsole zum Verwalten von DNS
Jetzt auf die DNS-Servertools Funktion installiert ist der virtuellen Domäne, können DNS-Tools zum Verwalten von DNS in der verwalteten Domäne.

> [AZURE.NOTE] Sie müssen Mitglied der Gruppe 'AAD DC Administrators' DNS auf verwalteten Domäne verwalten.

1. Klicken Sie auf " **Verwaltung**" aus dem Startbildschirm. Die **DNS-** Konsole auf dem virtuellen Computer installiert sollte angezeigt werden.

    ![Verwaltung - neu](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Klicken Sie auf **DNS** , um die DNS-Verwaltungskonsole zu starten.

3. Klicken Sie im Dialogfeld **Verbindung mit DNS-Server** klicken Sie auf die Option **folgendem Computer**, und geben Sie den DNS-Domänennamen der verwalteten Domäne (beispielsweise "contoso100.com").

    ![Neu - Domäne herstellen](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. Der DNS-Konsole eine Verbindung mit verwalteten Domäne.

    ![Neu - Domäne verwalten](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Die DNS-Konsole können nun die DNS-Einträge für Computer innerhalb des virtuellen Netzwerks hinzufügen in der Domänendienste AAD aktiviert haben.

> [AZURE.WARNING] Achten Sie bei der Verwaltung von DNS für die verwalteten Domäne DNS-Verwaltungstools. Stellen Sie sicher, dass Sie **Löschen oder ändern Sie die integrierten DNS-Datensätze, die von der Domäne Domäne verwendet werden**. Integrierte DNS-Datensätze enthalten Domain DNS, Namenservereinträge und andere Datensätze für DC Speicherort verwendet. Wenn Sie diese Datensätze ändern, sind Domänendienste virtuellen Netzwerk unterbrochen.


Finden Sie die [DNS-Tools Artikel im Technet](https://technet.microsoft.com/library/cc753579.aspx) Weitere Informationen zum Verwalten von DNS.


## <a name="related-content"></a>Verwandte Inhalte

- [Azure Active Directory-Domänendienste - erste Schritte](./active-directory-ds-getting-started.md)

- [Einen virtuellen Windows Server-Computer zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](active-directory-ds-admin-guide-administer-domain.md)

- [DNS-Verwaltungstools](https://technet.microsoft.com/library/cc753579.aspx)
