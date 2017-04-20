<properties
    pageTitle="Erstellen Sie einen virtuellen Computer mit MySQL | Microsoft Azure"
    description="Erstellen Sie einen virtuellen Azure-Computer mit Windows Server 2012 R2 und der MySQL-Datenbank mit dem klassischen Bereitstellungsmodell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>Installieren von MySQL auf einem Computer mit Windows Server 2012 R2 mit klassischen Bereitstellungsmodell erstellt

[MySQL](http://www.mysql.com) ist ein gängiger open Source, SQL-Datenbank. In diesem Lernprogramm wird das Installieren und Ausführen der Gemeinschaft Version von MySQL 5.6.23 als MySQL-Server auf einem virtuellen Computer unter Windows Server 2012 R2 veranschaulicht. Anleitung zur Installation von MySQL Linux finden Sie in: [MySQL Azure installieren](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Erstellen Sie einen virtuellen Computer mit Windows Server 2012 R2

Haben Sie bereits einen virtuellen Computer unter Windows Server 2012 R2, können dieses [Lernprogramm](virtual-machines-windows-classic-tutorial.md) Sie den virtuellen Computer erstellt. 

## <a name="attach-a-data-disk"></a>Legen Sie einen Datenträger

Nachdem der virtuelle Computer erstellt wurde, können Sie optional einen zusätzlichen Datenträger anfügen. Dies empfiehlt sich für Produktionsarbeitslasten zu verhindern, dass der Speicherplatz auf dem Betriebssystem-Laufwerk (c) umfasst das Betriebssystem.

[Wie einen Datenträger auf einem Windows-Computer an](virtual-machines-windows-classic-attach-disk.md) und führen Sie die Schritte zum Anhängen eines leeren Datenträger. Festlegen der Host Cache **keine** oder **schreibgeschützt**.

## <a name="log-on-to-the-virtual-machine"></a>Melden Sie sich am virtuellen Computer

Anschließend werden Sie [auf dem virtuellen Computer anmelden](virtual-machines-windows-classic-connect-logon.md) MySQL installieren.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Installieren und Ausführen von MySQL Community Server auf dem virtuellen Computer

Gehen Sie zum Installieren, konfigurieren und Ausführen der Gemeinschaft Version von MySQL-Server folgendermaßen vor:

> [AZURE.NOTE] Diese Schritte sind für die 5.6.23.0 Communityversion von MySQL und Windows Server 2012 R2. Ihre Erfahrung kann für unterschiedliche Versionen von MySQL oder Windows Server sein.

1.  Nachdem Sie den virtuellen Computer mithilfe von Remotedesktop verbunden sind, klicken Sie auf **Internet Explorer** die Startseite angezeigt.
2.  Wählen Sie die Schaltfläche **Extras** rechts (rollenandruck radsymbol) und dann auf **Internetoptionen**. Klicken Sie auf die Registerkarte **Sicherheit** , klicken Sie auf das Symbol **Vertrauenswürdige Sites** und klicken Sie dann auf die Schaltfläche **Sites** . Fügen Sie http://*. MySQL zur Liste der vertrauenswürdigen Sites. Klicken Sie auf * *Schließen**, und klicken Sie dann auf * *OK**.
3.  Geben Sie in der Adressleiste von Internet Explorer, http://dev.mysql.com/downloads/mysql/ ein.
4.  Mithilfe der MySQL-Website suchen und downloaden die neueste Version von MySQL-Installer für Windows. Bei MySQL-Installationsprogramm die Version, die die vollständige Datei festlegen (z. B. der Mysql-Installer-Community-5.6.23.0.msi mit einer Größe von 282.4 MB), und speichern Sie das Installationsprogramm.
5.  Wenn das Installationsprogramm heruntergeladen wurde, klicken Sie auf Starten Setup **Ausführen** .
6.  Auf der Seite **Lizenzvertrag** den Lizenzvertrag, und klicken Sie auf **Weiter**.
7.  Auf der Seite **Setup-Typ auswählen** auf den gewünschten Installationstyp und klicken Sie dann auf **Weiter**. Die folgenden Schritte gehen an die **Server** -Setup-Typ.
8.  Klicken Sie auf der Seite **Installation** **Ausführen**. Nach Abschluss der Installation klicken Sie auf **Weiter**.
9.  Klicken Sie auf der Seite **Konfiguration** auf **Weiter**.
10. Auf der Seite **Typ und Netzwerke** angeben der gewünschten Art und Konnektivität Konfigurationsoptionen den TCP-Port bei Bedarf. Wählen Sie **Show Advanced Options**, und klicken Sie dann auf **Weiter**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Geben Sie auf **Konten und Rollen** ein MySQL Root-Kennwort ein. Nach Bedarf fügen Sie zusätzliche MySQL-Benutzerkonten hinzu, und klicken Sie auf **Weiter**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Geben Sie auf der Seite **Windows** ändert die Standardeinstellungen für die Ausführung der MySQL-Server als Windows-Dienst Bedarf an und dann auf **Weiter**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Geben Sie auf der Seite **Erweiterte Optionen** ändern Protokollierungsoptionen an, nach Bedarf und klicken Sie auf **Weiter**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Klicken Sie auf der Seite **Server-Konfiguration übernehmen** auf **Ausführen**. Wenn die Konfigurationsschritte ausgeführt haben, klicken Sie auf **Fertig stellen**.
15. Klicken Sie auf der Seite **Konfiguration** auf **Weiter**.
16. Klicken Sie auf der Seite **Installation beendet** **Protokoll in Zwischenablage kopieren** , wenn später überprüfen und dann auf **Fertig stellen**.
17. Geben Sie aus dem Startbildschirm **Mysql**, und klicken Sie dann auf **MySQL 5.6 Befehlszeilen-Client**.
18. Geben Sie das Root-Passwort, das Sie in Schritt 11 angegeben und Sie werden mit einer Aufforderung angezeigt, können Sie MySQL konfigurieren Befehle. Einzelheiten der Befehle und Syntax finden Sie unter [Handbücher MySQL](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Server-Konfiguration Standardeinstellungen Base und Daten Verzeichnisse und Laufwerke konfigurieren Sie Einträge in der Datei C:\Program Files (x86) \MySQL\MySQL Server 5.6\my-default.ini. Weitere Informationen finden Sie unter [5.1.2 Server Konfiguration standardmäßig](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Endpunkte konfigurieren

Soll den MySQL-Server-Dienst MySQL-Client-Computern im Internet zur Verfügung, müssen Sie konfigurieren Sie einen Endpunkt für den TCP-Port der MySQL-Server-Dienst überwacht und eine weitere Windows-Firewall-Regel erstellen. TCP-Port 3306 ist, wenn Sie einen anderen Anschluss auf der Seite **Typ und Netzwerke** (Schritt 10 des vorherigen Verfahrens) angegeben


> [AZURE.NOTE] Sicherheitsaspekte dabei, sollten Sie sorgfältig prüfen, da dadurch den MySQL-Server-Dienst auf allen Computern im Internet verfügbar ist. Sie können die Quell-IP-Adressen definieren, die der Endpunkt mit einer Zugriffssteuerungsliste (ACL) verwenden. Weitere Informationen finden Sie unter [Festlegen von Endpunkten einer virtuellen Maschine](virtual-machines-windows-classic-setup-endpoints.md).


So konfigurieren Sie einen Endpunkt für den Dienst MySQL Server

1.  Klicken Sie im klassischen Azure-Portal auf **virtuellen Maschinen**, klicken Sie auf den Namen des virtuellen Computers MySQL und klicken Sie **Endpunkte**.
2.  Klicken Sie in der Befehlszeile **Hinzufügen**.
3.  Klicken Sie auf den Pfeil nach rechts auf der Seite **einen Endpunkt einer virtuellen Maschine hinzufügen** .
4.  Bei Verwendung der Standard MySQL TCP Port 3306 **MySQL** **Namen**auf und klicken Sie auf das Häkchen.
5.  Wenn Sie einen anderen TCP-Port verwenden, geben Sie einen eindeutigen Namen in **Name**. Wählen Sie **TCP** Protokoll, geben Sie die Portnummer im **Öffentlichen Port** und **Private Port**und klicken Sie auf das Häkchen.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>Hinzufügen einer Windows-Firewall-Regel MySQL Datenverkehr zulässt

Um eine Windows-Firewall-Regel hinzuzufügen, die MySQL Datenverkehr aus dem Internet zulässt, führen Sie den folgenden Befehl auf Windows PowerShell belegen auf MySQL Server virtueller Computer.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Testen der Remoteverbindung


Testen Sie die Remoteverbindung auf Azure Virtual Machine MySQL-Server-Dienst müssen Sie zunächst den Cloud-Dienst, der virtuellen Computer mit MySQL-Server wird für DNS-Namen ermitteln.

1.  Klicken Sie im klassischen Azure-Portal auf **virtuellen Maschinen**MySQL Server virtuellen Computers klicken Sie und klicken Sie auf **Dashboard**.
2.  Notieren Sie den **DNS-Namen** Wert Abschnitt **Kurzer Blick** , Dashboard virtuellen Computer. Hier ist ein Beispiel:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Führen Sie von einem lokalen Computer MySQL oder MySQL-Client ausgeführt wird den folgenden Befehl als eine MySQL-Benutzer anmelden.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    MySQL Benutzer namens dbadmin3 und testmysql.cloudapp.net DNS-Namen für den virtuellen Computer, z. B. den folgenden Befehl ein.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das Ausführen von MySQL finden Sie in der [MySQL-Dokumentation](http://dev.mysql.com/doc/).
