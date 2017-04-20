<properties 
    pageTitle="LOB-anwendungsumgebung | Microsoft Azure" 
    description="Eine Web-basierte branchenspezifische Anwendung in einer hybriden Cloud-Umgebung für erstellen IT Pro- oder Entwicklungstests." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>Webbasierte LOB-Anwendung in einer hybridcloud zu Testzwecken eingerichtet

Dieses Thema führt Sie durch Erstellen einer simulierten Hybrid Cloud-Umgebung für das Testen einer webbasierten Textzeile branchenspezifische Anwendung in Microsoft Azure. Hier ist die resultierende Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Diese Konfiguration umfasst:

- Eine simulierte lokalen Netzwerk in Azure (Testlabor VNet) gehostet.
- Eine standortübergreifende virtuelles Netzwerk in Azure (TestVNET) gehostet.
- VNet VNet VPN-Verbindung.
- Eine webbasierte LOB-Server, SQL Server und sekundärer Domänencontroller im virtuellen Netzwerk TestVNET.

Diese Konfiguration bietet eine Grundlage und allgemeiner Ausgangspunkt aus dem können Sie:

- Entwickeln Sie und Testen Sie der LOB-Anwendung auf Internet Information Services (IIS) mit SQL Server 2014 Datenbank-Backends in Azure gehostet.
- Testen des simulierten Hybriden Cloud-basierten IT erfolgen.

Es gibt drei Hauptphasen dieser hybriden Cloudumgebung einrichten:

1.  Richten Sie die simulierte Hybriden Cloud-Umgebung.
2.  Konfigurieren des SQL Server-Computers (SQL1).
3.  Konfigurieren Sie den LOB-Server (LOB1).

Diese Arbeitslast benötigt Azure-Abonnement. Haben Sie ein Abonnement MSDN oder Visual Studio finden Sie unter [monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

Beispielsweise einer Produktion LOB-Anwendung in Azure anzeigen Sie **Geschäftsanwendungen** Architektur Blueprint unter [Microsoft Software Architekturdiagramme und Entwürfe](http://msdn.microsoft.com/dn630664)

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>Phase 1: Einrichten der simulierten Hybriden Cloud-Umgebung

Erstellen der [Hybriden Cloudumgebung simuliert](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md). Da diese Umgebung nicht das Vorhandensein von APP1-Server im Subnetz Corpnet benötigen, können Sie es jetzt herunterfahren.

Dies ist die aktuelle Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>Phase 2: Konfigurieren des SQL Server-Computers (SQL1)

Azure-Portal Starten des Computers DC2 erforderlich.

Als Nächstes erstellen Sie einen virtuellen Computer für SQL1 mit diesen Befehlen ein Azure PowerShell-Befehlszeile auf dem lokalen Computer. Geben Sie vor dem Ausführen dieser Befehle die Variablenwerte und entfernen Sie die Zeichen < und >.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Mithilfe von Azure Portal SQL1 Verbindung mit dem lokalen Administratorkonto SQL1.

Konfigurieren Sie Windows-Firewall-Regeln grundlegende Konnektivität testen und SQL Server-Datenverkehr. Eine auf Windows PowerShell-Befehlszeile auf SQL1 führen Sie diese Befehle aus.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Der Pingbefehl sollten vier Antworten von IP-Adresse 192.168.0.4 führen.

Fügen Sie zusätzliche Datenträger auf SQL1 als neue Volume mit dem Laufwerkbuchstaben f.

1.  Klicken Sie im linken Bereich des Server-Managers auf **Datei und**klicken Sie dann auf **Datenträger**
2.  Klicken Sie im Inhaltsbereich in der **Festplatten** -Gruppe auf **Diskette 2** ( **Partition** **Unknown**festgelegt).
3.  Klicken Sie auf **Aufgaben**, und klicken Sie dann auf **Neues Volume**.
4.  Auf den Seite des Assistenten zu beginnen, klicken Sie auf **Weiter**.
5.  Wählen Sie auf der Seite Server und Datenträger **Datenträger 2**auf und klicken Sie auf **Weiter**. Wenn Sie aufgefordert werden, klicken Sie auf **OK**.
6.  Geben Sie die Größe der Seite Volume klicken Sie auf **Weiter**.
7.  Zuweisen einer Seite Laufwerk Buchstaben oder Ordner klicken Sie auf **Weiter**.
8.  Klicken Sie auf der Seite Wählen Sie Datei System Settings auf **Weiter**.
9.  Klicken Sie auf der Seite Auswahl bestätigen auf **Erstellen**.
10. Wenn Sie fertig sind, klicken Sie auf **Schließen**.

Diese Befehle auf SQL1 Windows PowerShell-Befehlszeile ausführen:

    md f:\Data
    md f:\Log
    md f:\Backup

SQL1 anschließend mit diesen Befehlen an die Windows PowerShell auf SQL1 CORP Windows Server Active Directory-Domäne beitreten.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Verwenden Sie CORP\User1 Konto Aufforderung Domäne Konto Anmeldeinformationen für den **Computer hinzufügen** -Befehl.

Nach dem Neustart verwenden Sie das Azure-Portal Verbindung SQL1 *mit dem lokalen Administratorkonto SQL1*.

Konfigurieren Sie SQL Server 2014 Laufwerk F: verwenden für neue Datenbanken und Benutzerkontenberechtigungen.

1.  Der Startbildschirm Geben Sie **SQL Server Management**und klicken Sie dann auf **SQL Server 2014 Management Studio**.
2.  **Klicken Sie auf **Verbindung mit Server herstellen**.**
3.  Im Strukturbereich Objekt-Explorer mit der rechten Maustaste **SQL1**und klicken Sie dann auf **Eigenschaften**.
4.  Klicken Sie im Fenster **Eigenschaften** auf **Datenbank**.
5.  Die **Standardspeicherorte für Datenbank** suchen und diese Werte festlegen: 
    - Geben Sie **Daten**Pfad **f:\Data**.
    - Geben Sie **Protokoll**Pfad **f:\Log**.
    - Geben Sie für **Backup**-Pfad **f:\Backup**.
    - Hinweis: Nur neue Datenbanken verwenden diese Speicherorte.
6.  Klicken Sie auf **OK** , um das Fenster zu schließen.
7.  Öffnen Sie im Strukturbereich des **Objekt-Explorer** **Sicherheit**.
8.  Klicken Sie auf **Anmeldung** , und klicken Sie dann auf **Neuer Benutzername**.
9.  Geben Sie unter **Benutzername** **CORP\User1**.
10. Auf der Seite **Serverrollen** **Sysadmin**auf und klicken Sie dann auf **OK**.
11. Schließen Sie Microsoft SQL Server Management Studio.

Dies ist die aktuelle Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>Phase 3: Konfigurieren des LOB-Servers (LOB1)

Erstellen Sie zunächst einen virtuellen Computer für LOB1 mit diesen Befehlen Befehlszeile Azure PowerShell auf dem lokalen Computer.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Als Nächstes mit der Azure-Portal LOB1 mit den Anmeldeinformationen des lokalen Administratorkontos LOB1 herstellen.

Anschließend konfigurieren Sie eine Windows-Firewall-Regel zum Zulassen von Datenverkehr für grundlegende Konnektivität testen. Eine auf Windows PowerShell-Befehlszeile auf LOB1 führen Sie diese Befehle aus.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

Der Pingbefehl sollten vier Antworten von IP-Adresse 192.168.0.4 führen.

Als Nächstes fügen Sie LOB1 CORP Active Directory-Domäne mit diesen Befehlen in Windows PowerShell eingeben.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

Verwenden Sie CORP\User1 Konto Aufforderung Domäne Konto Anmeldeinformationen für den **Computer hinzufügen** -Befehl.

Verwenden Sie nach dem Neustart Azure-Portal mit dem CORP\User1-Konto und Kennwort eine Verbindung zu LOB1.

Konfigurieren Sie LOB1 für IIS und Testen Sie des Zugriffs von CLIENT1.

1.  Klicken Sie von Server-Manager auf **Add Rollen und Features**.
2.  Klicken Sie auf " **Weiter**" auf der Seite **vor** .
3.  Klicken Sie auf der Seite **Installationsmethode** auf **Weiter**.
4.  Klicken Sie auf der Seite **Zielserver auswählen** auf **Weiter**.
5.  Klicken Sie auf der Seite **Serverrollen** in der Liste der **Rollen**auf **Webserver (IIS)** .
6.  Bei Aufforderung klicken Sie auf **Features hinzufügen**, und klicken Sie auf **Weiter**.
7.  Klicken Sie auf der Seite **Funktionen wählen** auf **Weiter**.
8.  Klicken Sie auf der Seite **Web Server (IIS)** auf **Weiter**.
9.  Aktivieren Sie auf der Seite **Rollendienste auswählen** oder deaktivieren Sie die Kontrollkästchen für die Dienste müssen Sie zum Testen der LOB-Anwendung und klicken Sie auf **Weiter**.
10. Klicken Sie auf der Seite **Installationsauswahl bestätigen** auf **Installieren**.
11. Warten Sie, bis die Installation der Komponenten und klicken Sie dann auf **Schließen**.
12. Azure-Portal Verbinden mit dem Computer CLIENT1 mit den Anmeldeinformationen des CORP\User1 und starten Sie Internet Explorer.
13. Geben Sie in der Adressleiste **http://lob1/** , und drücken Sie dann die EINGABETASTE. Die standardmäßige IIS 8 Webseite sollte angezeigt werden.

Dies ist die aktuelle Konfiguration.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Diese Umgebung ist nur bereit für die Web-basierte Anwendung auf LOB1 bereitstellen und Testen der Funktionalität von CLIENT1 Corpnet Subnetz.

## <a name="next-step"></a>Nächstes

- Fügen Sie einen neuen virtuellen Computer mithilfe der [Azure-Portal](virtual-machines-windows-hero-tutorial.md).
