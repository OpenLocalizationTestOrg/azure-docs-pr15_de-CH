<properties
    pageTitle="Azure Active Directory Domain Services: Windows Server-VM zu einer verwalteten Domäne beitreten | Microsoft Azure"
    description="Fügen Sie einen virtuellen Computer Windows Server Azure Active Directory-Domänendienste"
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

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Eine Windows Server-VM zu einer verwalteten Domäne beitreten

> [AZURE.SELECTOR]
- [Klassische Azure-Portal - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell - Fenster](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

Dieser Artikel beschreibt, wie an einen virtuellen Computer mit Windows Server 2012 R2 eine Azure Active Directory-Domänendienste verwalteten Domäne mit klassischen Azure-Portal.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Schritt 1: Erstellen der virtuellen Computer Windows Server
Gehen Sie im [Erstellen eines virtuellen Computers ausgeführt Windows Azure-Verwaltungsportal im](../virtual-machines/virtual-machines-windows-classic-tutorial.md) Lernprogramm beschrieben. Es ist wichtig, dass neu erstellten virtuellen Computer im gleichen virtuellen Netzwerk zugeordnet ist die Aktivierung von Azure Active Directory-Domänendienste. Die Option 'Quick erstellen' ermöglicht keine virtuellen Computer ein virtuelles Netzwerk hinzuzufügen. Daher müssen Sie die Option "Aus Galerie" Erstellen der virtuellen Computer.

Führen Sie die folgenden Schritte zum Erstellen einer virtuellen Windows-Maschine verknüpft das virtuelle Netzwerk in dem Azure AD-Domänendienste aktiviert haben.

1. Klicken Sie im klassischen Azure-Portal, auf der Befehlsleiste am unteren Rand des Fensters auf **neu**.

2. Unter **berechnen** **virtuellen**klicken **Aus Galerie**.

3. Der erste Bildschirm können Sie **Wählen Sie ein Bild** für den virtuellen Computer aus der Liste der verfügbaren Bilder. Wählen Sie das entsprechende Bild.

    ![Bild auswählen](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Im zweiten Fenster können Sie einen Computernamen, Größe und administrativen Benutzernamen und Kennwort auswählen. Verwenden Sie Tier und Größe für Ihre app oder Arbeitslast ausgeführt. Der Benutzername, den Sie hier auswählen, ist ein lokaler Administrator-Benutzer auf dem Computer. Geben Sie ein Domänenbenutzerkonto Anmeldeinformationen eingeben.

    ![Konfigurieren virtueller Computer](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Der dritte Bildschirm können Sie Ressourcen für Netzwerke, Speicher und Verfügbarkeit zu konfigurieren. Sicherstellen Sie, dass das virtuelle Netzwerk wählen in dem Azure AD-Domänendienste aus **Region-Affinität Gruppe und virtuellen Netzwerk** aktiviert. **Cloud Service DNS-Namen** für den virtuellen Computer entsprechend angegeben.

    ![Virtuelles Netzwerk für virtuelle Computer auswählen](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Stellen Sie sicher, dass der virtuelle Computer mit dem gleichen virtuellen Netzwerk beitreten in der Azure AD-Domänendienste aktiviert haben. Daher kann der virtuelle Computer "sehen" die Domäne und Aufgaben wie die Domäne. Möchten Sie den virtuellen Computer in ein anderes virtuelles Netzwerk erstellen, verbinden Sie das virtuelle Netzwerk an das virtuelle Netzwerk in dem Azure AD-Domänendienste aktiviert haben.

6. Vierte Bildschirm können Sie den VM-Agent installieren und einige der verfügbaren Erweiterungen konfigurieren.

    ![Fertig](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Nach dem Erstellen der virtuellen Maschine führt das Verwaltungsportal neuen virtueller Computers unter dem Knoten **virtuelle Computer** . Sowohl der virtuellen Maschine und Cloud-Dienst automatisch gestartet und deren Status **als**aufgelistet.

    ![Virtual Machine wird ausgeführt](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Schritt 2: Verbinden Sie mit Windows Server virtueller Computer mit dem lokalen Administratorkonto
Jetzt verbinden wir die neu erstellte Windows Server virtuellen Computer der Domäne beitreten. Verwenden Sie beim Erstellen der virtuellen Computer herstellen angegebene lokale Administratoranmeldeinformationen.

Führen Sie die folgenden Schritte auf dem virtuellen Computer verbinden.

1. Navigieren Sie zum **virtuellen Computer** Knoten im klassischen Portal. Den virtuellen Computer, den Sie in Schritt 1 erstellt haben, und klicken Sie auf der Befehlsleiste am unteren Fensterrand auf **Verbinden** .

    ![Verbinden Sie mit virtuellen Windows-Maschine](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Das Verwaltungsportal fordert Sie zum Öffnen oder Speichern einer Datei mit der Erweiterung "RDP", die zum Verbinden mit dem virtuellen Computer. Klicken Sie auf, um die Datei zu öffnen, wenn der Download abgeschlossen ist.

3. Anmeldung aufgefordert werden Geben Sie Ihre **Anmeldeinformationen für lokale Administratoren**, die beim Erstellen des virtuellen Computers angegeben. Beispielsweise haben wir in diesem Beispiel "Localhost\mahesh" verwendet.

An diesem Punkt sollten Sie auf den neu erstellten virtuellen Windows-Maschine mit lokalen Administratorberechtigungen angemeldet sein. Im nächste Schritt wird der virtuelle Computer der Domäne hinzuzufügen.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Schritt 3: Verbinden der virtuellen Maschine AAD DS verwalteten Domäne Windows Server
Die folgenden Schritte Windows Server virtuellen Computer verwalteten AAD DS-Domäne hinzuzufügen.

1. Windows-Server verbinden Sie, wie in Schritt2. Öffnen Sie **Server-Manager**aus dem Startbildschirm.

2. Klicken Sie im linken Bereich des Server-Managers auf **Lokalen Server** .

    ![Starten Sie Server-Manager auf dem virtuellen Computer](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Klicken Sie im Bereich **Eigenschaften** auf **Arbeitsgruppe** . Klicken Sie auf der Eigenschaftenseite **Systemeigenschaften** auf **Ändern** , um der Domäne beizutreten.

    ![System-Eigenschaftenseite](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Geben Sie der Domänennamen des Azure Active Directory-Domänendienste verwalteten Domäne im Textfeld **Domain** und klicken Sie auf **OK**.

    ![Geben Sie die Domäne angehören](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Sie werden aufgefordert, Ihre Anmeldeinformationen für die Domäne einzugeben. Stellen Sie sicher, **Geben Sie die Anmeldeinformationen eines Benutzers AAD DC Administrators zur** Gruppe. Mitglieder dieser Gruppe sind berechtigt, Computer der verwalteten Domäne hinzuzufügen.

    ![Geben Sie Anmeldeinformationen für Domäne beitreten](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Sie können Anmeldeinformationen auf folgenden Arten angeben:

    - UPN-Format: Geben Sie das UPN-Suffix für das Benutzerkonto in Azure AD konfiguriert. In diesem Beispiel ist das UPN-Suffix des Benutzers 'Bob' 'bob@domainservicespreview.onmicrosoft.com'.

    - SAMAccountName-Format: Sie können den Kontonamen im Format "sAMAccountName". In diesem Beispiel muss der Benutzer "Bob" "CONTOSO100\bob" eingeben.

        > [AZURE.NOTE] **Wir empfehlen das UPN-Format angeben.** SAMAccountName möglicherweise automatisch generierter werden ist ein Benutzer-UPN-Präfix übermäßig lange (z. B. 'Joereallylongnameuser'). Wenn mehrere Benutzer denselben UPN-Präfix (z. B. "Bob") in Azure AD-Mandanten, möglicherweise SAMAccountName Format vom Dienst automatisch generiert. In diesen Fällen werden das UPN-Format sicher Anmelden bei der Domäne verwendet.

7. Nach dem Domänenbeitritt erfolgreich ist, wird die folgende Meldung zur Begrüßung in der Domäne angezeigt. Starten Sie den virtuellen Computer der Domäne beitreten-Vorgang abgeschlossen.

    ![Willkommen in der Domäne](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Problembehandlung bei Domänenbeitritt
### <a name="connectivity-issues"></a>Verbindungsprobleme
Wenn der virtuelle Computer die Domäne finden kann, finden Sie die folgenden Problembehandlungsschritte aus:

- Überprüfen Sie die Verbindung der virtuellen Computer mit dem gleichen virtuellen Netzwerk, Domänendienste in aktiviert haben. Wenn dies nicht der Fall, den virtuellen Computer keine Verbindung mit der Domäne ist und deshalb nicht der Domäne beitreten.

- Der virtuelle Computer mit einem anderen virtuellen Netzwerk verbunden ist, sicher, dass dieses virtuelle Netzwerk an das virtuelle Netzwerk besteht in der Domänendienste aktiviert haben.

- Versuchen Sie, ping die Domäne den Domänennamen der verwalteten Domäne (beispielsweise "Ping contoso100.com"). Wenn Sie nicht, versuchen Sie Ping die IP-Adressen für die Domäne auf der Seite angezeigt, Azure Active Directory-Domänendienste aktiviert (z. B. "10.0.0.4 ping"). Bist du Pingen Sie die IP-Adresse aber nicht bei der Domäne, kann DNS nicht ordnungsgemäß konfiguriert. Sie können nicht die IP-Adressen der Domäne als DNS-Server für das virtuelle Netzwerk konfiguriert haben.

- Versuchen Sie, leeren den DNS-Auflösungscache des virtuellen Computers ("Ipconfig/flushdns").

Wenn das Dialogfeld angezeigt wird, die Anmeldeinformationen für die Domäne anfordert, haben Sie keine Konnektivitätsprobleme.


### <a name="credentials-related-issues"></a>Anmeldeinformationen
Finden Sie die folgenden Schritte Wenn Sie Probleme mit Anmeldeinformationen die Domäne beitreten können.

- Verwenden Sie das UPN-Format angeben. Der SAMAccountName für Ihr Konto möglicherweise automatisch generiert, wenn mehrere Benutzer mit demselben UPN-Präfix in Ihrem Mandanten oder UPN-Präfix zu lang ist. Deshalb SAMAccountName Format für Ihr Konto möglicherweise anders als Sie erwarten oder in der lokalen Domäne verwenden.

- Verwenden Sie die Anmeldeinformationen des Benutzers, der Gruppe 'AAD DC Administratoren' an Computern der verwalteten Domäne angehört.

- Achten Sie nach die Schritte im Handbuch Erste Schritte [Kennwortsynchronisation aktiviert](active-directory-ds-getting-started-password-sync.md) .

- Den UPN des Benutzers verwenden Sie in Azure AD konfiguriert (z. B. 'bob@domainservicespreview.onmicrosoft.com') anmelden.

- Sicherstellen Sie, dass Sie Kennwortsynchronisation gemäß Handbuch Erste Schritte abgeschlossen lange gewartet haben.


## <a name="related-content"></a>Verwandte Inhalte

- [Azure Active Directory-Domänendienste - erste Schritte](./active-directory-ds-getting-started.md)

- [Eine verwaltete Azure Active Directory-Domänendienste-Domäne verwalten](./active-directory-ds-admin-guide-administer-domain.md)
