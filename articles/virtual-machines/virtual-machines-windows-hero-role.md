<properties
    pageTitle="Installieren von IIS auf Ihre erste Windows-VM | Microsoft Azure"
    description="Experimentieren Sie mit des ersten virtuellen Computers für Windows, IIS und Port 80 mit Azure-Portal öffnen."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Probieren Sie eine Rolle auf Ihrer Windows-VM installieren
    
Haben Sie Ihre erste Virtual Machine (VM laufen), können Sie zum Installieren von Software und Services verschieben. In diesem Lernprogramm gehen wir zum Server-Manager auf die Windows Server-VM IIS installieren. Anschließend erstellen wir ein Netzwerk Sicherheit Gruppe (NSG) Azure-Portal auf Port 80 auf IIS-Datenverkehr verwenden. 

Wenn Sie bereits Ihre erste VM erstellt haben, sollten Sie erstellen [Ihre erste Windows virtuellen Computer in Azure-Portal](virtual-machines-windows-hero-tutorial.md) zurück in diesem Lernprogramm fortfahren.

## <a name="make-sure-the-vm-is-running"></a>Stellen Sie sicher, dass die VM ausgeführt wird

1. [Azure-Portal](https://portal.azure.com)zu öffnen.
2. Klicken Sie im Hub auf **virtuellen Maschinen**. Wählen Sie den virtuellen Computer aus.
3. Ist der Status **beendet (Deallocated)**, klicken Sie auf die **Grundlagen** der VM auf **Starten** . Wenn der Status **ausgeführt**wird, können Sie mit dem nächsten Schritt verschieben.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Verbinden mit dem virtuellen Computer, und melden Sie sich

1.  Klicken Sie im Hub auf **virtuellen Maschinen**. Wählen Sie den virtuellen Computer aus.

3. Klicken Sie auf dem Blatt für den virtuellen Computer **Verbinden**. Erstellt und downloadet eine Datei Remote Desktop Protocol (RDP-Datei), die wie eine Verknüpfung zu Ihrem Computer herstellen. Sie möchten die Datei auf Ihrem Desktop für einfachen Zugriff speichern. **Öffnen** dieser Datei VM herstellen.

    ![Screenshot der Azure-Portal zum Herstellen der VM](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Sie erhalten eine Warnung, dass die RDP von einem unbekannten Herausgeber. Dies ist normal. Klicken Sie im Fenster Remotedesktop auf **Verbinden** möchten.

    ![Screenshot von eine Warnung zu einem unbekannten Herausgeber](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Geben Sie im Fenster Windows-Sicherheit den Benutzernamen und das Kennwort für das lokale Konto erstellt, wenn die VM erstellt. Der Benutzername wird als *Vmname*& 92 eingegeben. *Benutzernamen*, klicken Sie auf **OK**.

    ![Screenshot des VM-Name, Benutzername und Kennwort eingeben](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Sie erhalten eine Warnung, dass das Zertifikat nicht überprüft werden kann. Dies ist normal. Klicken Sie auf **Ja,** die Identität der virtuellen Maschine und Protokollierung auf Fertig stellen.

    ![Screenshot zeigt eine Nachricht stoßen, die Identität der VM](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Wenn Sie auf Probleme beim Ausführen beim Verbinden, finden Sie unter [Problembehandlung bei Remotedesktopverbindungen zu einem Windows-basierten Azure Virtual Machine](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>Installieren Sie IIS auf Ihrem virtuellen Computer

Jetzt anmelden für die VM wird wir eine Serverrolle installieren, damit Sie mehr experimentieren können.

1. Öffnen Sie **Server-Manager** , wenn er nicht bereits geöffnet ist. Klicken Sie auf **Start** und dann auf **Server-Manager**.
2. Wählen Sie im **Server-Manager** **Lokalen Server** im linken Bereich. 
3. Wählen Sie im Menü **Verwalten** > **Hinzufügen von Rollen und Features**.
4. Hinzufügen von Rollen und Features-Assistenten auf der Seite **Installationsart** wählen Sie **Rolle oder Funktion-basierten Installation**und klicken Sie auf **Weiter**.

    ![Screenshot zeigt die Registerkarte Hinzufügen von Rollen und Features Assistent für Installation](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Wählen Sie die VMs aus der Server aus und auf **Weiter**.
6. Wählen Sie auf der Seite **Serverrollen** **Webserver (IIS)**.

    ![Screenshot zeigt die Registerkarte Hinzufügen von Rollen und Features Assistent für Serverrollen](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Im Popupfenster zum Hinzufügen von Features für IIS erforderlichen **Verwaltungstools** ausgewählt ist, und klicken Sie auf **Features hinzufügen**. Das Popupfenster schließen, klicken Sie auf **Weiter** im Assistenten.

    ![Screenshot zeigt Popup bestätigen die IIS-Rolle hinzufügen](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Klicken Sie auf der Seite Eigenschaften auf **Weiter**.
9. Klicken Sie auf der Seite **Webserver Rolle (IIS)** auf **Weiter**. 
10. Klicken Sie auf der Seite **Rollendienste** auf **Weiter**. 
11. Klicken Sie auf **der Bestätigungsseite** auf **Installieren**. 
12. Wenn die Installation abgeschlossen ist, klicken Sie auf den Assistenten **Schließen** .



## <a name="open-port-80"></a>Öffnen Sie Port 80 

Reihenfolge für die VM eingehenden Datenverkehr über Port 80 akzeptieren müssen Sie eine eingehende Regel Netzwerk-Sicherheitsgruppe hinzufügen. 

1. [Azure-Portal](https://portal.azure.com)zu öffnen.
2. Wählen Sie in **virtuellen Maschinen** VM, die Sie erstellt haben.
3. In den Einstellungen virtuelle Computer wählen Sie **Netzwerkschnittstellen** und dann die vorhandenen Netzwerkschnittstelle.

    ![Screenshot zeigt die Einstellung für virtuelle Computer für die Netzwerkschnittstellen](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. **Grundlagen** für die Netzwerkschnittstelle klicken Sie auf **Netzwerk-Sicherheitsgruppe**.

    ![Screenshot zeigt Abschnitt Essentials für die Netzwerkschnittstelle](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. Blatt **Essentials** für die NSG benötigen Sie eine vorhandene eingehende Standardregel für **Standard kann Rdp** ermöglicht Ihnen die VM anmelden. Sie fügen einen anderen eingehende Regel zum Zulassen von Datenverkehr von IIS. Klicken Sie auf **Eingehende Regel**.

    ![Screenshot zeigt Abschnitt Essentials für die NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. **Eingehende Sicherheitsregeln**klicken Sie auf **Hinzufügen**.

    ![Screenshot zeigt die Schaltfläche eine Regel hinzufügen](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. **Eingehende Sicherheitsregeln**klicken Sie auf **Hinzufügen**. Geben Sie in den Portbereich **80** und **Zulassen** ausgewählt ist. Wenn Sie fertig sind, klicken Sie auf **OK**.

    ![Screenshot zeigt die Schaltfläche eine Regel hinzufügen](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Weitere Informationen zu NSGs, eingehenden und ausgehenden Regeln finden Sie unter [externen Zugriff für die VM mit Azure-portal](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Verbinden Sie mit der IIS-Standardwebsite

1. In Azure-Portal auf **virtuelle Computer** und wählen Sie die VM.
2. **Essentials** Blatt kopieren Sie Ihre **öffentliche IP-Adresse**.

    ![Screenshot zeigt, wie die öffentliche IP-Adresse der VM](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Öffnen Sie einen Browser, und geben Sie in der Adressleiste die öffentliche IP-Adresse wie folgt: http://<publicIPaddress> und **Geben** diese Adresse zu.
3. Ihr Browser sollte die standardmäßige IIS-Webseite öffnen. Es sieht folgendermaßen aus:

    ![Screenshot zeigt die standardmäßige IIS-Seite in einem Browser sieht](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Nächste Schritte

- Sie können auch mit dem virtuellen Computer mit [einen Datenträger anfügen](virtual-machines-windows-attach-disk-portal.md) experimentieren. Datenträger bieten mehr Speicher für den virtuellen Computer.
