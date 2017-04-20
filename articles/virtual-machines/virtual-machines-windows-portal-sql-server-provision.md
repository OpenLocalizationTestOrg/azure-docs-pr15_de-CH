<properties
    pageTitle="Bereitstellen einer SQL Server-VM | Microsoft Azure"
    description="Erstellen und Verbinden mit einem SQL Server virtuellen Computer in Azure über das Portal. In diesem Lernprogramm wird den Ressourcen-Manager-Modus verwendet."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Bereitstellen Sie einen SQL Server virtueller Computer in Azure-Portal

> [AZURE.SELECTOR]
- [Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [PowerShell](virtual-machines-windows-ps-sql-create.md)

In diesem Lernprogramm zu Ende veranschaulicht die Azure-Portal verwenden, um einen virtuellen Computer mit SQL Server bereitzustellen.

Der Katalog Azure Virtual Machine (VM) enthält mehrere Bilder, die Microsoft SQL Server enthalten. Mit wenigen Mausklicks können SQL VM Bilder aus dem Katalog auswählen und in der Azure-Umgebung bereitstellen.

In diesem Lernprogramm werden Sie:

- [Wählen Sie eine SQL VM Bild aus der Galerie](#select-a-sql-vm-image-from-the-gallery)
- [Konfigurieren und Erstellen der VM](#configure-the-vm)
- [Öffnen Sie die VM mit Remotedesktop](#open-the-vm-with-remote-desktop)
- [Eine Remoteverbindung zu SQL Server](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Wählen Sie eine SQL VM Bild aus der Galerie

1. Melden Sie sich mit Ihrem Konto [Azure-Portal](https://portal.azure.com) .

    >[AZURE.NOTE] Wenn Sie nicht über ein Azure-Konto verfügen, besuchen Sie [Azure kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

1. Azure-Portal klicken Sie auf **neu**. Das Portal öffnet **neue** Blade. Die SQL Server-VM-Ressourcen sind in der **virtuellen Maschinen** am Markt.

1. Klicken Sie auf **virtuellen Maschinen**auf dem Blatt **neu** .

1. Um die verfügbaren Bilder anzuzeigen, klicken Sie auf **Alle** auf dem **virtuellen Computer** .

    ![Azure Virtual Machines Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. Klicken Sie unter **Datenbankserver**auf **SQL Server**. Möglicherweise auf **um Datenbankserver**zu suchen. Überprüfen Sie die verfügbaren SQL Server-Vorlagen.

    ![Virtuelle Maschine SQL Galeriebildern](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Jede Vorlage identifiziert eine SQL Server-Version und Betriebssystem. Wählen Sie eines der Bilder aus. Überprüfen Sie das Blade Details, das eine des virtuellen Bildes Beschreibung.

    >[AZURE.NOTE] SQL-VM-Abbilder enthalten die Lizenzkosten für SQL Server in pro Minute Preise der VM erstellen. Gibt es eine andere Option zu bringen-your-eigenen-Lizenz (BYOL) und Zahlen für die VM. {BYOL} ist die Bildnamen vorangestellt. Weitere Informationen zu dieser Option finden Sie unter [Erste Schritte mit SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

1. Überprüfen Sie unter **Wählen Sie ein Bereitstellungsmodell** **Ressourcenmanager** ausgewählt ist. Ressourcen-Manager ist das empfohlene Bereitstellungsmodell für neue virtuelle Computer. Klicken Sie auf **Erstellen**.

    ![Erstellen von SQL-VM mit Ressourcen-Manager](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>Konfigurieren der VM
Es gibt fünf Blades für einen virtuellen Computer SQL Server konfigurieren.

| Schritt               | Beschreibung                          |
|---------------------|-------------------------------|
| **Grundlagen**              | [Grundlegende konfigurieren](#1-configure-basic-settings)      |
| **Größe**                | [Wählen Sie die Größe des virtuellen Computers](#2-choose-virtual-machine-size)   |
| **Einstellungen**            | [Konfigurieren](#3-configure-optional-features)   |
| **SQL Server-Einstellung** | [SQL Server konfigurieren](#4-configure-sql-server-settings) |
| **Zusammenfassung**             | [Überprüfen Sie die Zusammenfassung](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. Konfiguration von Standardeinstellungen
Das Blade **Grundlagen** bieten Sie Folgendes:

* Geben Sie **einen eindeutigen virtuellen Computer**.
* Geben Sie einen **Benutzernamen** für das lokale Administratorkonto auf dem virtuellen Computer. Dieses Konto wird auch festen SQL Server-Rolle **Sysadmin** hinzugefügt.
* Geben Sie ein sicheres **Kennwort**.
* Wenn mehrere Abonnements verfügen, überprüfen Sie das Abonnement für den neuen virtuellen Computer.
* Geben Sie im Feld **Gruppe** einen Namen für eine neue Ressourcengruppe. Auch wenn Sie eine vorhandene Ressource verwenden Gruppe klicken **Wählen Sie vorhandene**. Eine Ressourcengruppe ist eine Sammlung von verwandten Ressourcen in Azure (VMs Speicherkonten, virtuelle Netzwerke usw.).

    >[AZURE.NOTE] Mit eine neue Ressourcengruppe ist hilfreich, wenn Sie nur testen oder SQL Server-Installationen in Azure Kennenlernen. Nach Abschluss des Tests löschen Sie die Ressourcengruppe um die VM und Gruppe zugeordneten Ressourcen automatisch löschen Weitere Informationen zu Ressourcengruppen finden Sie unter [Azure-Ressourcen-Manager (Übersicht)](../azure-resource-manager/resource-group-overview.md).

* Wählen Sie einen **Speicherort** für die Bereitstellung.
* Klicken Sie auf **OK** , um die Einstellung zu speichern.

    ![SQL-Grundlagen Blade](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Wählen Sie die Größe des virtuellen Computers
Wählen Sie eine Größe des virtuellen Computers auf der **Größe** Blatt **Wählen Sie eine Größe** . Das Blade wird zunächst Computer empfohlene Größe basierend auf der ausgewählten Vorlage angezeigt. Er schätzt auch die monatliche Kosten für die VM.

![VM SQL Optionen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Produktions-Workloads empfohlen, Auswählen einer virtuellen Maschine, die [Premium-Speicher](../storage/storage-premium-storage.md)unterstützt. Wenn dieses Leistungsniveau nicht erforderlich, verwenden Sie die Schaltfläche **Alle anzeigen** alle Computer Optionen zeigt. Beispielsweise können Sie Computer kleiner für eine Entwicklungs- oder Umgebung.

>[AZURE.NOTE] Weitere Informationen zu virtuellen anzeigen Größe [Größen für virtuelle Computer](virtual-machines-windows-sizes.md). Hinweise über SQL Server-VM-Größe finden Sie unter [best Practices zur Performance für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md).

Wählen Sie die Computer und dann auf **auswählen**.

## <a name="3-configure-optional-features"></a>3. konfigurieren
Blatt **Einstellungen** konfigurieren Sie Azure-Speicher, Netzwerk und Überwachung des virtuellen Computers.

- Geben Sie unter **Speicher**einer **Festplatte geben** Standard oder Premium (SSD). Premium-Speicher für Produktionsarbeitslasten empfohlen.

>[AZURE.NOTE] Bei Auswahl von Premium (SSD) für eine Maschine, die Premium-Speicher nicht unterstützt, wird Ihr Computer Größe automatisch geändert.  

- Übernehmen Sie unter **Konto**den Kontonamen automatisch bereitgestellte Speicher. Sie können auch auf **Speicherkonto** ein Konto auswählen und Konfigurieren des Speichertyps Konto klicken. Azure erstellt ein neues Speicherkonto mit lokal redundanter Speicher. Weitere Informationen zu Speicheroptionen finden Sie unter [Azure Storage Replication](../storage/storage-redundancy.md).

- Übernehmen Sie unter **Netzwerk**die Werte automatisch ausgefüllt. Sie können auch auf jede Funktion **virtuellen Netzwerk**, **Subnetz**, **öffentliche IP-Adresse**und **Netzwerk-Sicherheitsgruppe**manuell konfigurieren klicken. Behalten Sie für die Zwecke dieses Lernprogramms die Standardwerte bei.

- Azure ermöglicht **Überwachung** standardmäßig mit demselben Speicherkonto für die VM bezeichnet. Sie können diese Einstellung ändern.

- Geben Sie unter **Verfügbarkeit**eine Verfügbarkeit. Für die Zwecke dieses Lernprogramms können Sie **keine**auswählen. Möchten Sie SQL AlwaysOn Availability Gruppen einrichten, konfigurieren Sie die Verfügbarkeit zur Vermeidung von den virtuellen Computer neu.  Weitere Informationen finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern](virtual-machines-windows-manage-availability.md).

Wenn diese Einstellung konfigurieren, klicken Sie auf **OK**.

## <a name="4-configure-sql-server-settings"></a>4. konfigurieren Sie SQL server
**SQL Server Einstellungen** -Blades konfigurieren Sie Einstellungen und Optimierungen für SQL Server. Folgende: Einstellungen für SQL Server konfiguriert werden können

| Einstellung               |
|---------------------|
| [Konnektivität](#connectivity)              |
| [Authentifizierung](#authentication)                |
| [Speicherkonfiguration](#storage-configuration)            |
| [Automatische Patches](#automated-patching) |
| [Automatische Sicherung](#automated-backup)             |
| [Integration von Azure Key Vault](#azure-key-vault-integration)             |
| [R-Services](#r-services) |

### <a name="connectivity"></a>Konnektivität
Geben Sie unter **SQL-Konnektivität**Zugriffe auf die SQL Server-Instanz auf diesem virtuellen Computer. Wählen Sie für dieses Lernprogramm **öffentliche (Internet-)** SQL Server-Computer oder Dienste im Internet Verbindungen ermöglichen. Mit dieser Option konfiguriert Azure automatisch die Firewall und Netzwerk-Sicherheitsgruppe für den Datenverkehr auf Port 1433.  

![SQL-Konnektivitätsoptionen](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Verbindung zu SQL Server über das Internet müssen Sie auch SQL Server-Authentifizierung aktivieren, die im nächsten Abschnitt beschrieben.

>[AZURE.NOTE] Es ist möglich, mehrere Einschränkungen für die Netzwerkkommunikation VM SQL Server hinzufügen. Hierzu können Sie der Netzwerk-Sicherheitsgruppe bearbeiten, nachdem die VM erstellt. Weitere Informationen finden Sie unter [Neuigkeiten Network Security Group (NSG)?](../virtual-network/virtual-networks-nsg.md)

Wenn Sie nicht das Datenbankmodul über das Internet ermöglichen möchten, wählen Sie eine der folgenden Optionen:

- **(In VM) lokale** Verbindung zu SQL Server nur innerhalb der virtuellen Maschine.
- **Private (innerhalb von Virtual Network)** Verbindung zu SQL Server von Computern oder Diensten im gleichen virtuellen Netzwerk ermöglichen.

>[AZURE.NOTE] Abbild des virtuellen PCs für SQL Server Express Edition wird nicht automatisch das TCP-Protokoll aktiviert. Dies gilt auch für öffentliche und Private Konnektivitätsoptionen. Express Edition müssen Sie SQL Server Configuration Manager nach dem Erstellen der VM [manuell](#configure-sql-server-to-listen-on-the-tcp-protocol) aktivieren, das TCP-Protokoll verwenden.

Im Allgemeinen wählen die restriktivste Konnektivität lässt sich die verbessern Sie Sicherheit. Jedoch alle Optionen sicherungsfähigen SQL-Windows-Authentifizierung und Netzwerk-Sicherheitsgruppe Regeln.

Standardmäßig **Port** 1433. Sie können eine andere Portnummer angeben.
Weitere Informationen finden Sie unter [Verbindung zu einem SQL Server virtuellen Computer (Resource Manager) | Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Authentifizierung
Wenn Sie SQL Server-Authentifizierung benötigen, klicken Sie unter **SQL-Authentifizierung** **Aktivieren** .

![SQL Server-Authentifizierung](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Wenn Sie SQL Server (d. h. die öffentliche Konnektivitätsoption) zugreifen möchten, müssen Sie hier die SQL-Authentifizierung aktivieren. Zugang der Öffentlichkeit zu SQL Server erfordert die Verwendung von SQL-Authentifizierung.

Aktivieren Sie SQL Server-Authentifizierung geben Sie einen **Benutzernamen** und **ein Kennwort**. Dieser Name wird als SQL Server-Authentifizierung anmelden und Mitglied der festen Serverrolle **Sysadmin** konfiguriert. Weitere Informationen zur Authentifizierung finden Sie unter [Auswählen eines Authentifizierungsmodus](http://msdn.microsoft.com/library/ms144284.aspx) .

Wenn Sie SQL Server-Authentifizierung nicht aktivieren, können Sie das lokale Administratorkonto Verbindung zu SQL Server-Instanz auf dem virtuellen Computer verwenden.

### <a name="storage-configuration"></a>Speicherkonfiguration
Klicken Sie auf **Speicherkonfiguration** des Speicherbedarfs angeben.

![SQL-Speicherkonfiguration](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Wenn Sie Standardspeicher auswählen, ist diese Option nicht verfügbar. Automatische Optimierung ist nur für Premium-Speicher verfügbar.

Sie können als Eingabe/Ausgabe-Vorgänge pro Sekunde (IOPs) Durchsatz in MB/s und Gesamtspeichergröße Anforderungen angeben. Konfigurieren Sie diese Werte mithilfe der gleitende Skalierung. Das Portal berechnet automatisch die Anzahl der Laufwerke basierend auf diese Anforderungen.

Standardmäßig optimiert Azure Speicher für 5000 IO/s, 200 MB und 1 TB Speicherplatz. Sie können diese anhand der Arbeitsbelastung Einstellungen ändern. Wählen Sie unter **Speicher optimiert für**eine der folgenden Optionen aus:

- **Allgemein** ist die Standardeinstellung und unterstützt die meisten Arbeitslasten.
- **Transactional** Verarbeitung optimiert bei traditionellen Datenbanken OLTP-Speicher.
- **Data warehousing** optimiert den Speicher für analytische und Berichtstools Arbeitslasten.

>[AZURE.NOTE] Die Obergrenzen für die Schieberegler variieren abhängig vom ausgewählten virtuellen Computer.

### <a name="automated-patching"></a>Automatische Patches
**Automatische Patches** ist standardmäßig aktiviert. Automatische Patches kann Azure SQL Server und Betriebssystem automatisch patchen. Geben Sie einen Tag Wochentag, Uhrzeit und Dauer ein Wartungsfenster. Azure führt diese Wartungsfenster patchen. Der Fenster Wartungsplan verwendet VM Gebietsschema Zeit. Wenn Sie Azure SQL Server und Betriebssystem automatisch patch nicht möchten, klicken Sie auf **Deaktivieren**.  

![SQL automatische Patches](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

Weitere Informationen finden Sie unter [Automatische Patches für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).

### <a name="automated-backup"></a>Automatische Sicherung
Aktivieren Sie automatische Datenbank-Backups für alle Datenbanken unter **Automatisches Backup**. Automatische Sicherung ist standardmäßig deaktiviert.

Wenn Sie SQL automatische Sicherung aktivieren, können Sie Folgendes konfigurieren:

- Aufbewahrungsdauer (in Tagen) für Backups
- Speicherkonto mit backups
- Verschlüsselungsoption und das Kennwort für backups

Klicken Sie auf **Aktivieren**, um die Sicherungskopie zu verschlüsseln. Geben Sie das **Kennwort**ein. Azure erstellt ein Zertifikat zur Verschlüsselung der Backups und das angegebene Kennwort zu diesem Zertifikat verwendet.

![SQL automatisches Backup](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 Weitere Informationen finden Sie unter [Automatische Sicherung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Integration von Azure Key Vault
Zum Speichern von Sicherheit Geheimnisse für die Verschlüsselung in Azure **Azure-Tresor Integration** klicken Sie und **Aktivieren**.

![SQL Azure-Tresor Integration](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Die folgende Tabelle listet die Parameter Azure Key Vault Integration konfigurieren müssen.

|PARAMETER|BESCHREIBUNG|BEISPIEL|
|----------|----------|-------|
|**Key Vault-URL** |Der Speicherort des Schlüssels Depots.|https://contosokeyvault.Vault.Azure.NET/ |
|**Principal name** |Azure Active Directory Dienstprinzipalnamen. Dieser Name wird auch als Client-ID bezeichnet  |fde2b411 - 33d 5-4e11-af04eb07b669ccf2|
| **Haupt-Schlüssel**|Azure Active Directory Service principal geheimen Schlüssel. Dieser Schlüssel wird auch als Client bezeichnet. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm-azF1XDKM =|
|**Anmeldenamen**|**Anmeldenamen**: AKV Integration Anmeldeinformationen in SQL Server ermöglicht die VM auf die wichtigsten Tresor erstellt. Wählen Sie einen Namen für diese Anmeldeinformationen.| mycred1|

Weitere Informationen finden Sie unter [Konfigurieren Azure Key Vault Integration für SQL Server auf Azure VMs](virtual-machines-windows-ps-sql-keyvault.md).

Wenn Sie SQL Server konfigurieren abgeschlossen haben, klicken Sie auf **OK**.

### <a name="r-services"></a>R-services
SQL Server 2016 Enterprise Edition können Sie [SQL Server R Dienste](https://msdn.microsoft.com/library/mt604845.aspx)aktivieren. Dadurch werden erweiterte Analyse mit SQL Server 2016 verwenden. Klicken Sie auf die **SQL Server-Einstellung** **Aktivieren** .

![SQL Server R Dienste aktivieren](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] SQL Server-Bilder, die nicht 2016 Enterprise Edition, ist die Option R aktivieren deaktiviert.

## <a name="5-review-the-summary"></a>5. Überprüfen Sie die Zusammenfassung
Blatt **Zusammenfassung** die Zusammenfassung und klicken Sie auf **OK** , um SQL Server-Ressourcengruppe und für diese VM angegebenen Ressourcen zu erstellen.

Sie können die Bereitstellung von Azure-Portal überwachen. **Benachrichtigung** Schaltfläche am oberen Rand des Bildschirms zeigt grundlegende Status der Bereitstellung.

>[AZURE.NOTE] Um eine Vorstellung zu Bereitstellung bieten, bereitgestellt ich SQL VM auf Region USA OST, mit Standardeinstellungen. Diese Testkonfiguration nahmen 26 Minuten. Aber Sie können schneller oder langsamer Bereitstellungszeit basierend auf der Region Einstellungen ausgewählt.

## <a name="open-the-vm-with-remote-desktop"></a>Öffnen Sie die VM mit Remotedesktop

Gehen Sie den virtuellen Computer mit Remotedesktop herstellen:

1. Nachdem der Azure-VM erstellt wurde, wird das Symbol für die VM im Azure Dashboard. Auch finden sie die vorhandenen virtuellen Computer durchsuchen. Klicken Sie auf den neuen virtuellen Computer für SQL. **VM** -Blade zeigt Details der virtuellen Computer.
1. **Klicken Sie auf oben **virtuellen** Blade.**
1. Browser-downloads eine RDP-Datei für die VM. Öffnen Sie die RDP-Datei.
    ![Remote Desktop SQL VM](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. Die Remotedesktopverbindung benachrichtigt, dass der Herausgeber dieser Remoteverbindung identifiziert werden kann. Klicken Sie auf **Verbinden** möchten.
1. Klicken Sie im Dialogfeld **Windows-Sicherheit** auf **ein anderes Konto verwenden**.
1. **Benutzer** geben ** \<Benutzername >**, wobei <user name> ist der Name angegeben, wenn die VM konfiguriert. Sie müssen einen ersten umgekehrten Schrägstrich vor dem Namen hinzufügen.
1. Geben Sie das **Kennwort** , das Sie zuvor für diese VM konfiguriert, und klicken Sie auf **OK** , um eine Verbindung herzustellen.
1. Wenn ein anderes Dialogfeld **Remotedesktopverbindung** , ob Verbindung fragt, klicken Sie auf **Ja**.

Nach dem Verbinden mit dem virtuellen Computer SQL Server können Sie SQL Server Management Studio starten und Verbinden mit Windows-Authentifizierung mit lokalen Administratorberechtigungen. Wenn Sie SQL Server-Authentifizierung aktiviert haben, können Sie auch mit SQL-Authentifizierung mit SQL Login und Kennwort, die Sie während der Bereitstellung konfiguriert.

Auf dem Computer können Sie Computer und SQL Server Settings Ihre Bedürfnisse direkt ändern. Sie könnten z. B. Firewall konfigurieren oder SQL Server-Konfiguration ändern.

## <a name="connect-to-sql-server-remotely"></a>Eine Remoteverbindung zu SQL Server

In diesem Lernprogramm haben wir **öffentlichen** Zugriff für den virtuellen Computer und die **SQL Server-Authentifizierung**ausgewählt. Diese Einstellung konfiguriert automatisch den virtuellen Computer zu SQL Server-Verbindung von einem Client über das Internet (vorausgesetzt, sie haben die richtige SQL-Anmeldung) zulassen.

>[AZURE.NOTE] Während der Bereitstellung keine Public ausgewählt haben, sind zusätzliche Schritte erforderlich, die SQL Server-Instanz über das Internet Zugriff auf. Weitere Informationen finden Sie unter [Verbinden einer SQL Server-VM](virtual-machines-windows-sql-connect.md).

Die folgenden Abschnitte zeigen die Verbindung zu Ihrer SQL Server-Instanz auf die virtuellen Computer von einem anderen Computer über das Internet.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von SQL Server in Azure finden Sie unter [Häufig gestellte Fragen](virtual-machines-windows-sql-server-iaas-faq.md)und [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md) .

Video Übersicht über SQL Server auf Azure Virtual Machines beobachten Sie [Azure VM ist die optimale Plattform für SQL Server 2016](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure virtuellen Computern.
