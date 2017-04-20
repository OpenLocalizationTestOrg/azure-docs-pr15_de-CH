<properties
    pageTitle="Konfigurieren Sie Gruppe immer auf Verfügbarkeit in Azure VM automatisch - Ressourcen-Manager"
    description="Erstellen einer verfügbarkeitsgruppe Azure virtuelle Computer in Azure-Ressourcen-Manager-Modus. Diese praktische Einführung verwendet die Benutzeroberfläche hauptsächlich die gesamte Lösung automatisch erstellt."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Konfigurieren Sie Gruppe immer auf Verfügbarkeit in Azure VM automatisch - Ressourcen-Manager

> [AZURE.SELECTOR]
- [Ressourcen-Manager: Vorlage](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Ressourcen-Manager: manuell](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassisch: Benutzeroberfläche](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassisch: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Dieses Ende-Lernprogramm veranschaulicht die SQL Server-Verfügbarkeit Azure Resource Manager virtuelle Computer Erstellen einer. Das Lernprogramm verwendet Azure Blades Vorlage konfigurieren. Sie überprüfen die Standardeinstellungen, geben Sie erforderliche Einstellung und Blades im Portal aktualisieren Sie dieses Lernprogramm durchgehen.

Am Ende des Lernprogramms besteht Projektmappe Gruppe SQL Server-Verfügbarkeit in Azure den folgenden Elementen:

- Ein virtuelles Netzwerk mit mehreren Subnetzen, einschließlich einer Front-End- und Back-End-Subnetz

- Zwei Domänencontroller einer Domäne Active Directory (AD)

- Zwei SQL Server-VMs auf dem Back-End-Subnetz bereitgestellt und der AD-Domäne

- Ein WSFC-Cluster mit Quorummodell Knotenmehrheit 3 Knoten

- Eine verfügbarkeitsgruppe mit zwei synchrone Commit einer Datenbank Verfügbarkeit

Die Abbildung wird grafisch der Lösung.

![Lab-Architektur AG in Azure testen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Alle Ressourcen in dieser Lösung gehören zu einer einzelnen Ressourcengruppen.

In diesem Lernprogramm wird Folgendes vorausgesetzt:

- Sie haben bereits ein Azure-Konto. Haben Sie eine [für ein Testkonto anmelden](http://azure.microsoft.com/pricing/free-trial/).

- Sie wissen bereits, wie eine SQL Server-VM aus dem virtuellen Katalog über die Benutzeroberfläche bereitgestellt. Weitere Informationen finden Sie unter [Bereitstellung eines virtuellen Computers mit SQL Server auf Azure](virtual-machines-windows-portal-sql-server-provision.md)

- Sie haben bereits ein solides Grundwissen über Verfügbarkeitsgruppen. Weitere Informationen finden Sie unter [ständig Verfügbarkeitsgruppen (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Wenn Sie SharePoint mit Verfügbarkeitsgruppen interessiert sind, siehe auch [Konfigurieren Sie SQL Server 2012 immer auf Verfügbarkeitsgruppen für SharePoint 2013](http://technet.microsoft.com/library/jj715261.aspx).

In diesem Lernprogramm verwenden Sie im Azure-Portal:

- Wählen Sie die Vorlage stets über das Portal

- Überprüfen Sie die Vorlageneinstellungen und aktualisieren Sie einige Konfigurationen für Ihre Umgebung

- Überwachen Sie Azure erstellt die gesamte Umgebung

- Verbinden Sie zu einem Domänencontroller und dann zu SQL Server

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>Bestimmung des Clusters aus der Galerie

Azure bietet ein Galeriebild für die gesamte Lösung. Um die Vorlage zu suchen:

1.  Melden Sie sich mit Ihrem Konto Azure-Portal.
1.  Azure Portal auf **+ neu.** Das Portal öffnet neue Blade.
1.  Neue Blade suchen **AlwaysOn**.
![AlwaysOn Vorlage](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Suchen Sie in den Suchergebnissen **SQL Server AlwaysOn Cluster**.
![AlwaysOn-Vorlage](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  Wählen Sie **ein Bereitstellungsmodell** wählen Sie **Ressourcenmanager aus**

### <a name="basics"></a>Grundlagen

Klicken Sie auf **Grundlagen** und konfigurieren Sie Folgendes zu:

- **Administrator-Benutzername** ist ein Benutzerkonto mit Domänenadministrator-Berechtigungen und Mitglied der festen SQL Server-Rolle Sysadmin auf beiden Instanzen von SQL Server. Verwenden Sie für dieses Lernprogramm **DomainAdmin "**.

- **Kennwort** ist das Kennwort für das Administratorkonto der Domäne. Verwenden Sie ein komplexes Kennwort. Bestätigen Sie das Kennwort ein.

- **Abonnement** ist das Abonnement Azure Rechnung für alle Ressourcen der Gruppe Verfügbarkeit bereitgestellt. Wenn Ihr Konto mehrere Abonnements hat, können Sie ein anderes Abonnement angeben.

- **Ressourcengruppe** ist der Name für die Gruppe alle Azure-Ressourcen von diesem Lernprogramm erstellt gehören. In diesem Lernprogramm verwenden Sie **SQL-HA-RG**. Weitere Informationen finden Sie unter (Azure Ressourcenmanager Overview) [Ressource-Gruppe-overview.md / #-Ressourcengruppen].

- **Speicherort** ist der Azure-Region, in dem die Ressourcen für dieses Lernprogramm erstellt wird. Wählen Sie eine Azure Region die Infrastruktur zu hosten.

Folgt aussehen **Grundlagen** Blade:

![Grundlagen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Klicken Sie auf **OK**.

### <a name="domain-and-network-settings"></a>Domänen- und Netzwerkinformationen settings

Diese Galerievorlage Azure erstellt eine neue Domäne neue Domänencontroller. Es erstellt auch ein neues Netzwerk und zwei Subnetzen. Die Vorlage ermöglicht keine Server in einer vorhandenen Domäne oder virtuelles Netzwerk erstellen. Der nächste Schritt ist die Domänen- und Netzwerkinformationen konfigurieren.

Überprüfen Sie auf **Domänen- und Netzwerkinformationen Einstellungen** der vordefinierten Werte für die Domäne und Netzwerk:

- **Gesamtstruktur-Stammdomänenname** ist den Domänennamen für die Active Directory-Domäne verwendet, die Cluster hosten. Verwenden Sie für das Lernprogramm für **contoso.com**.

- **Virtuelle** wird der Netzwerkname für Azure virtuelle Netzwerk. In diesem Lernprogramm verwenden Sie **AutohaVNET**.

- **Domänencontroller Subnetz Name** ist der Name eines Teils des virtuellen Netzwerks, die den Domänencontroller als Host fungiert. Verwenden Sie für dieses Lernprogramm **Subnetz 1**. Diesem Subnetz verwendet Adresse Präfix **10.0.0.0/24**.

- **SQL Server-Subnetz Name** ist der Name eines Teils des virtuellen Netzwerks, die SQL Server-Hosts und der Dateifreigabe Zeuge. Verwenden Sie für dieses Lernprogramm **Subnetz 2**. Diesem Subnetz verwendet Adresse Präfix **10.0.1.0/26**.

Weitere Informationen zu virtuellen Netzwerken in [Azure finden Sie unter Übersicht über Virtual Network](../virtual-network/virtual-networks-overview.md).  

Die **Domänen- und Netzwerkinformationen Einstellungen** sollte wie folgt aussehen:

![Domänen- und Netzwerkinformationen settings](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Bei Bedarf können Sie diese Werte ändern. Für dieses Lernprogramm verwenden Sie die voreingestellten Werte.

- Überprüfen Sie die Einstellungen, und klicken Sie auf **OK**.

###<a name="availability-group-settings"></a>verfügbarkeitseinstellungen

Überprüfen Sie auf **verfügbarkeitseinstellungen** der vordefinierten Werte für die Verfügbarkeit und den Listener.

- **Verfügbarkeit Gruppenname** ist die Clusterressource Namens für die Availability Group. In diesem Lernprogramm verwenden Sie **Contoso ag**.

- **Verfügbarkeit Listener Gruppenname** wird von Cluster und internen Lastenausgleich verwendet. Diese Namen können mit SQL Server entsprechende Replikat der Datenbank herstellen. In diesem Lernprogramm verwenden Sie **Contoso-Listener**.

-  **Verfügbarkeit Gruppe Überwachungsport** gibt an, dass TCP-Port, den SQL Server Listener verwenden. In diesem Lernprogramm verwenden Sie den Standardport **1433**.

Bei Bedarf können Sie diese Werte ändern. In diesem Lernprogramm verwenden Sie die voreingestellten Werte.  

![verfügbarkeitseinstellungen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Klicken Sie auf **OK**.

###<a name="vm-size-storage-settings"></a>Virtueller Speicher Einstellungen

**Virtueller Speicher, Einstellungen** SQL Server virtuellen Größe und überprüfen Sie die Einstellungen.

- **Größe des virtuellen Computers mit SQL Server** ist die Größe Azure Virtual Machine für SQL Server. Wählen Sie einen virtuellen Computer für Ihre Arbeitslast. Sie erstellen diese Umgebung für das Lernprogramm verwenden **DS2**. Für Produktionsarbeitslasten Größe des virtuellen Computers, die die Arbeitslast unterstützen kann auswählen. Viele Produktionsarbeitslasten erfordern **DS4** oder größer. Die Vorlage zwei virtuellen Maschinen dieser Größe erstellen und Installieren von SQL Server auf jedem. Weitere Informationen finden Sie unter [Größe für virtuelle Computer](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure wird SQL Server Enterprise Edition installieren. Die Kosten hängen die Edition und die Größe des virtuellen Computers. Detaillierte Informationen zu aktuellen Kosten finden Sie unter [virtuelle Computer Preise](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

- **Domain Controller VM-Größe** ist die Größe des virtuellen Computers für die Domänencontroller. Verwenden Sie für dieses Lernprogramm **D2**.

- **Datei freigeben Zeuge virtuellen Größe** ist die Größe des virtuellen Computers für die Zeugendateifreigabe. Verwenden Sie für dieses Lernprogramm **A1**.

- **SQL Speicherkonto** heißt das Speicherkonto für die SQL Server-Daten und Betriebssystem-Datenträger. In diesem Lernprogramm verwenden Sie **alwaysonsql01**.

- **DC Speicherkonto** heißt das Speicherkonto für die Domänencontroller. In diesem Lernprogramm verwenden Sie **alwaysondc01**.

- **SQL Server Daten Festplattengröße** TB ist die Größe des SQL Server-Datenträger TB. Geben Sie eine Zahl von 1 bis 4. Dies ist die Größe der an jeder SQL Server-Datenträger. Verwenden Sie für dieses Lernprogramm **1**.

- **Optimierung** legt bestimmte Konfigurationseinstellungen für SQL Server virtuelle Computer basierend auf der Arbeitslast. Alle SQL Server in diesem Szenario verwenden Sie Storage Premium mit Azure Host Datenträgercache auf schreibgeschützt festgelegt. Darüber hinaus können Sie SQL Server-Einstellung für die Arbeitslast optimieren, einer dieser drei Optionen:

    - **Allgemeine Aufgaben** setzt keine bestimmte Konfigurationen

    - **Transactional processing** festgelegt Ablaufverfolgungsflag 1117 und 1118

    - **Data warehousing** Ablaufverfolgungsflag 1117 festgelegt und 610

In diesem Lernprogramm verwenden Sie **Allgemeine Aufgaben**.

![VM speichereinstellungen](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Überprüfen Sie die Einstellungen, und klicken Sie auf **OK**.

####<a name="a-note-about-storage"></a>Informationen zum Speicher

Zusätzliche Optimierungen hängt von der Größe der SQL Server-Datenträger. Azure fügt für jede Terabyte Datenträger zusätzlichen 1 TB Premium Speicher (SSD). Wenn ein Server mit 2 TB oder mehr erfordert, erstellt die Vorlage ein Speicher-Pools auf jeder SQL Server. Ein Speicher-Pool ist eine Form der Speichervirtualisierung, sind mehrere Datenträger konfiguriert höhere Kapazität, Flexibilität und Leistung bereitstellen.  Die Vorlage erstellt einen Speicherplatz im Speicherpool und zeigt diese als einzelnen Daten des Betriebssystems. Die Vorlage legt Datenträger als den Datenträger für SQL Server. Die Vorlage optimiert Speicherpool für SQL Server folgendermaßen:

- Stripe-Größe ist die Interleave-Einstellung für den virtuellen Datenträger. Dies ist für transaktionale Workloads auf 64 KB festgelegt. Die Einstellung ist für Data warehousing-Arbeitslasten 256 Kbyte.

- Stabilität ist einfach (keine Flexibilität).

>[AZURE.NOTE] Premium Azure-Speicher ist lokal redundant und drei Kopien der Daten in einem einzigen Bereich so widerstandsfähiger am Speicherpool nicht erforderlich ist.

- Anzahl der Spalten entspricht der Anzahl der Datenträger im Speicherpool.

Finden Sie weitere Informationen zu Speicher Platz und Speicher-Pools:

- [Übersicht über Leerzeichen](http://technet.microsoft.com/library/hh831739.aspx).

- [Windows Server-Sicherung und Speicher-Pools](http://technet.microsoft.com/library/dn390929.aspx)

Weitere Informationen zu best Practices für die SQL Server-Konfiguration finden Sie unter [best Practices zur Performance für SQL Server in Azure virtuellen Maschinen](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>SQL Server-Einstellung

Überprüfen Sie auf **SQL Server** und ändern Sie Präfix der SQL Server-VM, SQL Server-Version SQL Server-Dienstkonto und Kennwort und SQL automatisch gepatcht Wartungsplan.

- **SQL Server Name Präfix** zum Namen für jede SQL Server erstellen. In diesem Lernprogramm verwenden Sie **Contoso ag**. Die SQL Server-Namen werden *Contoso ag 0* und *Contoso ag 1*.

- **SQL Server-Version** ist die Version von SQL Server. In diesem Lernprogramm verwenden Sie **SQL Server 2014**. Sie können auch **SQL Server 2012** oder **SQL Server 2016**.

- **SQL Server-Dienst-Benutzernamen** ist das Domänenkonto für den SQL Server-Dienst. Verwenden Sie für dieses Lernprogramm **Sqlservice**.

- **Kennwort** ist das Kennwort für das SQL Server-Dienstkonto.  Verwenden Sie ein komplexes Kennwort. Bestätigen Sie das Kennwort ein.

- **Wartungsplan SQL automatisch gepatcht** identifiziert den Wochentag, Azure automatisch SQL Server patchen. Geben Sie für dieses Lernprogramm **Sonntag**.

- **SQL automatisch gepatcht Wartung starten Stunde** wird die Tageszeit der Azure-Region automatische Patches begonnen wird.

>[AZURE.NOTE]Patch Fenster für jede VM wird um eine Stunde versetzt. Nur ein virtueller Computer wird gleichzeitig gepatcht, um Unterbrechung der Dienste zu verhindern.

![SQL Server-Einstellung](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Überprüfen Sie die Einstellungen, und klicken Sie auf **OK**.

###<a name="summary"></a>Zusammenfassung

Auf der Zusammenfassungsseite überprüft Azure Einstellungen. Sie können auch die Vorlage. Überprüfen Sie die Zusammenfassung. Klicken Sie auf **OK**.

###<a name="buy"></a>Kaufen

Diese endgültige Blade enthält **Rechtliche Hinweise**und **Datenschutz**. Lesen Sie diese Informationen. Wenn Sie zum Erstellen der virtuellen Computer und alle erforderlichen Ressourcen für die Availability Group Azure bereit sind, klicken Sie auf **Erstellen**.

Azure-Portal wird die Ressourcengruppe und alle Ressourcen erstellen.

##<a name="monitor-deployment"></a>Monitor-Bereitstellung

Überwachen Sie den Fortschritt der Bereitstellung von Azure-Portal. Ein Symbol für die Bereitstellung wird automatisch dem Azure Portal Dashboard fixiert.

![Azure-Dashboard](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Verbinden mit SQL Server

Neuen Instanzen von SQL Server werden auf virtuellen Computern ausgeführt, die keine Verbindung zum Internet haben. Die Domänencontroller müssen jedoch eine Verbindung mit Internetzugriff. Um SQL Server mit Remotedesktop, erste RDP zu einem Domänencontroller herstellen. Öffnen Sie vom Domänencontroller eine zweite RDP zu SQL Server.

RDP auf den primären Domänencontroller gehen Sie folgendermaßen vor:

1.  Aus dem Azure Portal sehr erfolgreichen Bereitstellung Dashboard.

1.  **Klicken Sie auf.**

1.  Klicken Sie auf **Ad primäre Domänencontroller** den Computernamen des virtuellen Computers für den primären Domänencontroller Blatt **Ressourcen** .

1.  Klicken Sie auf das Blade für **Ad primäre Domänencontroller** **Verbinden**. Ihr Browser fragt geöffnet oder remote Connection-Objekt gespeichert werden soll. Klicken Sie auf **Öffnen**.
![Verbinden mit DC](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Remotedesktopverbindung** kann Sie warnen, dass der Herausgeber von dieser Verbindung identifiziert werden kann. Klicken Sie auf **Verbinden**.

1.  Sicherheit von Windows aufgefordert, Ihre Anmeldeinformationen für die Verbindung die IP-Adresse des primären Domänencontrollers eingeben. Klicken Sie auf **ein anderes Konto verwenden**. Geben Sie **Benutzernamen** **Contoso\DomainAdmin**. Dies ist für Administratorbenutzernamen gewählte Konto. Verwenden Sie komplexe Kennwörter, die Sie ausgewählt haben, wenn die Vorlage konfiguriert.

1.  **Remote Desktop** kann Sie warnen, dass der Remotecomputer aufgrund von Problemen mit dem Sicherheitszertifikat nicht authentifiziert werden konnte. Es wird der Name des Zertifikats angezeigt. Wenn Sie das Lernprogramm befolgt werden der Namen **Ad primäre dc.contoso.com**. Klicken Sie auf **Ja**.

Sie sind jetzt mit dem primären Domänencontroller verbunden. RDP, SQL Server folgendermaßen Sie vor:

1.  Öffnen Sie **Remotedesktopverbindung**auf dem Domänencontroller.

1.  Geben Sie für **Computer**den Namen eines SQL Server. Geben Sie für dieses Lernprogramm **Sqlserver-0**.

1.  Verwenden Sie dasselbe Benutzerkonto und Kennwort RDP zum Domänencontroller.

Sie sind jetzt mit RDP an SQL Server verbunden. Öffnen Sie SQL Server Management Studio, Verbinden mit der Standardinstanz von SQL Server, und überprüfen Sie die Verfügbarkeit Gruppe konfiguriert ist.


