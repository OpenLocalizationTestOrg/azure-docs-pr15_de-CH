<properties
    pageTitle="Azure Active Directory Domain Services: RHEL VM zu einer verwalteten Domäne beitreten | Microsoft Azure"
    description="Fügen Sie eine virtuelle Maschine von Red Hat Enterprise Linux Azure Active Directory-Domänendienste"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Eine virtuelle Maschine von Red Hat Enterprise Linux 7 einer verwalteten Domäne beitreten
Dieser Artikel veranschaulicht das eine virtuelle Maschine von Red Hat Enterprise Linux (RHEL) 7 einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzuzufügen.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Bereitstellen einer virtuellen Maschine von Red Hat Enterprise Linux
Die folgenden Schritte einen Azure-Portal mit RHEL 7 virtuellen Computer bereitstellen.

1. Mit der [Azure-Portal](https://portal.azure.com)anmelden.

    ![Azure Portal dashboard](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. Klicken Sie im linken Bereich auf **neu** , und geben Sie **Red Hat** in der Suchleiste, wie im folgenden Screenshot gezeigt. Einträge für Red Hat Enterprise Linux in den Suchergebnissen angezeigt. Klicken Sie auf **Red Hat Enterprise Linux 7.2**.

    ![RHEL Ergebnisse auswählen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Die Suchergebnisse im Bereich **Alles** sollte das Red Hat Enterprise Linux 7.2 Bild auflisten. Klicken Sie auf **Red Hat Enterprise Linux 7.2** , um weitere Informationen zu virtuellen Bild anzuzeigen.

    ![RHEL Ergebnisse auswählen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. Im **Red Hat Enterprise Linux 7.2** sollten Sie weitere Informationen zu virtuellen Bild angezeigt. Wählen Sie in der Dropdownliste **Wählen Sie ein Bereitstellungsmodell** **Classic**. Klicken Sie dann auf die Schaltfläche **Erstellen** .

    ![Bilddetails anzeigen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. Geben Sie im **VM erstellen** den **Host-Namen** für den neuen virtuellen Computer. Auch einen lokaler Administrator-Benutzernamen im Feld **Benutzername** und ein **Kennwort**angeben. Sie können auch einen SSH-Schlüssel verwenden, um den lokalen Administrator authentifizieren. Auch ein **Tier Preise** für den virtuellen Computer auswählen.

    ![VM - grundlegenden Details erstellen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Klicken Sie auf **optionale Konfiguration**. Die **optionale Konfiguration** klicken Sie auf **Netzwerk**.

    ![Erstellen von VM - virtuelles Netzwerk konfigurieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Dadurch wird ein Fenster mit dem Titel **Netzwerk**. Klicken Sie im **Netzwerk** auf **Virtuelles Netzwerk** das virtuelle Netzwerk aktivieren, das Linux VM bereitgestellt werden soll. **Virtuelles** Netzwerk wird geöffnet. Wählen Sie im Bereich **Virtuelles Netzwerk** **verwendet ein vorhandenes virtuelles Netzwerk** . Wählen Sie das virtuelle Netzwerk, das Azure AD-Domänendienste verfügbar ist. In diesem Beispiel wählen wir das 'MyPreviewVNet' virtuelle Netzwerk.

    ![Erstellen von VM - virtuelles Netzwerk auswählen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. Klicken Sie im Bereich **optionale Konfiguration** auf **OK** .

    ![Erstellen Sie VM - ausgewählte virtuelles Netzwerk](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Sie können jetzt den virtuellen Computer erstellt. Klicken Sie im **VM erstellen** auf die Schaltfläche **Erstellen** .

    ![Erstellen von VM - bereit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Bereitstellung des neuen virtuellen Computers basierend auf RHEL 7.2 Bild sollte beginnen.

  ![Erstellen von VM - Bereitstellung gestartet](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Nach einigen Minuten sollte die virtuellen Computer erfolgreich und bereit zur Verwendung bereitgestellt werden.

  ![Erstellen Sie VM - Bereitstellung](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Eine Remoteverbindung mit dem neu bereitgestellten virtuellen Linux-Maschine
RHEL 7.2 virtuellen Computer wurde in Azure bereitgestellt. Die nächste Aufgabe ist Remoteverbindung mit dem virtuellen Computer herstellen.

**RHEL 7.2 virtuellen Computer verbinden** Führen Sie die Schritte im Artikel [zu einer virtuellen Maschine unter Linux anmelden](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Die restlichen Schritte angenommen, Verbindung mit RHEL virtuellen Computer kitten SSH-Client verwenden. Weitere Informationen finden Sie unter [kitten-Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Öffnen Sie das Programm PuTTY.

2. Geben Sie den **Hostnamen** für den neu erstellten virtuellen Computer für RHEL. In diesem Beispiel hat der virtuelle Computer den Hostnamen "Contoso rhel.cloudapp .net. Wenn Sie den Hostnamen des VM nicht kennen, finden Sie in VM Dashboard Azure-Portal.

    ![Kitten verbinden](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Melden Sie sich am virtuellen Computer mit Angabe der Erstellung virtueller Computer lokale Administratorrechte. In diesem Beispiel verwendet haben wir das lokale Administratorkonto "Mahesh".

    ![PuTTY login](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Erforderliche Pakete auf virtuellen Linux-Maschine installieren
Nach der Verbindung mit dem virtuellen Computer besteht die nächste Aufgabe für Domänenbeitritt auf dem virtuellen Computer erforderlichen Pakete installieren. Führen Sie die folgenden Schritte aus:

1. **Realmd installieren:** Das Realmd-Paket wird für Domänenbeitritt verwendet. Geben Sie folgenden Befehl in das Terminal PuTTY:

    Sudo Yum installieren realmd

    ![Realmd installieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Nach einigen Minuten muss Realmd Paket auf dem virtuellen Computer installiert.

    ![Realmd installiert](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Sssd installieren:** Das Realmd-Paket hängt Sssd Domain Join-Operationen. Geben Sie folgenden Befehl in das Terminal PuTTY:

    Sudo Yum installieren sssd

    ![Sssd installieren](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Nach einigen Minuten muss Sssd Paket auf dem virtuellen Computer installiert.

    ![Realmd installiert](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Kerberos installieren:** Geben Sie folgenden Befehl in das Terminal PuTTY:

    Sudo Yum installieren krb5-Workstation krb5-Bibliotheken

    ![Installieren von kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Nach einigen Minuten muss Realmd Paket auf dem virtuellen Computer installiert.

    ![Kerberos installiert](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Virtuellen Linux-Maschine verwalteten Domäne beitreten
Da die erforderlichen Pakete auf virtuellen Linux-Maschine installiert sind, besteht die nächste Aufgabe, den virtuellen Computer verwalteten Domäne hinzuzufügen.

1. Entdecken Sie AAD Domänendienste verwaltete Domäne. Geben Sie folgenden Befehl in das Terminal PuTTY:

    Sudo Bereich Suchen CONTOSO100.COM

    ![Entdecken Sie Bereich](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    **Bereich ermitteln** der verwalteten Domäne finden kann, sicher, dass die Domäne von der virtuellen Maschine (versuchen Sie Ping) erreichbar ist. Außerdem sicherstellen Sie, dass die virtuellen Computer tatsächlich mit dem gleichen virtuellen Netzwerk bereitgestellt wurde, in der verwaltete Domäne verfügbar ist.

2. Kerberos zu initialisieren. Geben Sie den folgenden Befehl in das Terminal PuTTY. Stellen Sie sicher, dass Sie einen Benutzer angeben, die zur Gruppe "AAD DC Administratoren" gehört. Diese Benutzer können Computer verwalteten Domäne hinzuzufügen.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Stellen Sie sicher, geben Sie den Namen in Großbuchstaben, andere Kinit fehlschlägt.

3. Den Computer der Domäne beitreten. Geben Sie den folgenden Befehl in das Terminal PuTTY. Geben Sie denselben Benutzer, die im vorherigen Schritt ('Kinit') angegeben.

    Sudo Realm beitreten – ausführliche CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Realm beitreten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Sie sollten eine Meldung ("erfolgreich registrierten Computer im Bereich") bei der Computer erfolgreich der verwalteten Domäne verknüpft.


## <a name="verify-domain-join"></a>Beitreten zu einer Domäne überprüfen
Sie können schnell überprüfen, ob der Computer verwalteten Domäne erfolgreich hinzugefügt wurde. Verbinden mit der neuen Domäne RHEL VM mit SSH und ein Domänenbenutzerkonto und dann das Benutzerkonto ordnungsgemäß aufgelöst wird.

1. Das Terminal PuTTY geben folgenden Befehl zum Herstellen der neuen Domäne RHEL virtuellen Computer über SSH. Verwenden Sie ein Domänenkonto verwalteten Domäne gehört (z. B. 'bob@CONTOSO100.COM' dabei.)

    SSH -l bob@CONTOSO100.COM Contoso-rhel.cloudapp.net

2. Das Terminal PuTTY Geben Sie folgenden Befehl zu sehen, ob das Basisverzeichnis ordnungsgemäß initialisiert wurde.

    PWD

3. Das Terminal PuTTY Geben Sie folgenden Befehl zu sehen, ob die Gruppenmitgliedschaften ordnungsgemäß aufgelöst werden.

    ID

Eine Beispielausgabe Befehlen folgt:

![Beitreten zu einer Domäne überprüfen](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Problembehandlung bei Domänenbeitritt
Finden Sie im Artikel [Problembehandlung Domänenbeitritt](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Verwandte Inhalte
- [Azure Active Directory-Domänendienste - erste Schritte](./active-directory-ds-getting-started.md)

- [Einen virtuellen Windows Server-Computer zu einer verwalteten Azure Active Directory-Domänendienste-Domäne hinzufügen](active-directory-ds-admin-guide-join-windows-vm.md)

- [Anmelden bei einem virtuellen Computer mit Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Installieren von Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Red Hat Enterprise Linux 7 - Integrationsleitfaden für Windows](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
