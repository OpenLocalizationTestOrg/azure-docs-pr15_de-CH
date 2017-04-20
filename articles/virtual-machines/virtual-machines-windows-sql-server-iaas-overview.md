<properties
    pageTitle="Übersicht über SQL Server auf Azure Virtual Machines | Microsoft Azure"
    description="Enthält Informationen zum vollständige SQL Server-Editionen auf Azure virtuelle Maschinen ausführen. Direktlinks für alle SQL Server-VM-Images und verwandte Inhalte zu erhalten."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Übersicht über SQL Server auf Azure Virtual Machines

Beschrieben die Optionen zum Ausführen von SQL Server auf Azure virtuelle Maschinen (VMs) eine Übersicht über [Allgemeine Aufgaben](#manage-your-sql-vm)und [Links zu Portal-Bilder](#option-1-create-a-sql-vm-with-per-minute-licensing) .

>[AZURE.NOTE] Sind bereits mit SQL Server und zum Bereitstellen einer SQL Server-VM sehen wollen, finden Sie unter [Bereitstellen einer SQL Server-VM in Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>Übersicht
Wenn Sie einen Datenbankadministrator oder ein Entwickler sind, können Azure VMs Ihre lokalen SQL Server-Arbeitslasten und Applikationen in die Cloud verschieben. Das folgende Video bietet eine technische Übersicht über SQL Server Azure VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

Das Video behandelt die folgenden Themen:

|Zeit|Bereich|
|---|---|
| 00:21 | Was sind Azure VMs? |
| 01:45 | Sicherheit |
| 02:50 | Konnektivität |
| 03:30 | Speicherzuverlässigkeit und Leistung |
| 05:20 | VM-Größen |
| 05:54 | Hohe Verfügbarkeit und SLA |
| 07:30 | Konfiguration |
| 08:00 | Überwachung |
| 08:32 | Demo: Erstellen einer SQL Server 2016 VM |

>[AZURE.NOTE] Das Video konzentriert sich auf SQL Server 2016 jedoch Azure bietet VM-Images für viele Versionen von SQL Server 2008, 2012 2014 und 2016 einschließlich. 

## <a name="scenarios"></a>Szenarien
Es gibt viele Gründe, die Ihre Daten in Azure gehostet werden soll. Wenn Ihre Anwendung in Azure verschoben wird, verbessert die Leistung, die Daten zu verschieben. Aber es gibt andere Vorteile. Sie haben automatisch Zugriff auf mehrere Rechenzentren für eine globale Präsenz und Disaster Recovery. Die Daten sind auch sehr sichere und robuste.

SQL Server auf Azure VMs ist eine Option für das Speichern von relationalen Daten in Azure. Es ist gut für verschiedene Szenarien. Sie möchten z. B. Azure VM möglichst auf einen lokalen SQL Server-Computer entsprechend konfigurieren. Oder Sie können zusätzliche Programme und Dienste auf demselben Datenbankserver ausführen. Es gibt zwei wichtigste Ressourcen, die Ihnen auch Szenarien und Hinweise helfen können:

 - [SQL Server auf Azure virtuellen Maschinen](https://azure.microsoft.com/services/virtual-machines/sql-server/) bietet eine Übersicht über die besten Szenarios für SQL Server auf Azure VMs. 
 - [Wählen Sie eine Wolke SQL Server Option: Azure SQL (PaaS) Datenbank oder SQL Server auf Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) ermöglicht einen detaillierten Vergleich zwischen SQL und SQL Server auf einem virtuellen Computer ausgeführt.

## <a name="create-a-new-sql-vm"></a>Erstellen einer neuen SQL VM
In den folgenden Abschnitten vorsehen der SQL Server-VM Galeriebilder Direktlinks Azure-Portal. Abhängig vom gewählten Bild können Sie entweder SQL Server Lizenzierung pro Minute Kosten, oder Sie eigene Lizenz (BYOL).

Finden Sie schrittweise Anleitung für diesen Prozess im Lernprogramm [Bereitstellen einer SQL Server-VM in Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Überprüfen Sie die [best Practices zur Performance für SQL Server VMs](virtual-machines-windows-sql-performance.md), erklärt, wie die entsprechenden Computers Größe und andere Funktionen während der Bereitstellung.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>Option 1: Erstellen einer SQL VM Lizenzierung pro minute
Die folgende Tabelle enthält eine Matrix der verfügbaren SQL Server-Bilder im virtuellen Katalog. Klicken Sie auf eine beliebige Verknüpfung zum Erstellen einer neuen SQL VM mit angegebenen Version, Version und Betriebssystem.

|Version|Betriebssystem|Edition|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Entwickler](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>Option 2: Erstellen einer SQL VM mit einer bestehenden Lizenz
Sie bringen auch eigene Lizenz (BYOL). In diesem Fall Zahlen Sie nur für die VM ohne zusätzlichen Kosten für die SQL Server-Lizenzierung. Um eigene Lizenz verwenden, verwenden Sie die Matrix der SQL Server-Versionen, Editionen und Betriebssysteme unten. Das Portal ist **{BYOL}**diese Bildnamen vorangestellt.

|Version|Betriebssystem|Edition|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Unternehmen BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Unternehmen BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Unternehmen BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] Um BYOL VM Bilder verwenden, müssen Sie und Konzernvertrag [Lizenz](https://azure.microsoft.com/pricing/license-mobility/)Mobilität durch Software Assurance in Azure. Sie benötigen eine gültige Lizenz für die Version/Edition von SQL Server verwenden möchten. Sie müssen [die notwendigen BYOL Informationen an Microsoft](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) innerhalb von **10** Tagen der VM-Bereitstellung.

## <a name="manage-your-sql-vm"></a>Verwalten Sie Ihrer SQL-VM
Nach der Bereitstellung Ihrer SQL Server-VM sind optionale Verwaltungsaufgaben. In vieler Hinsicht konfigurieren und Verwalten von SQL Server, genau wie eine lokale SQL Server-Instanz verwalten. Einige Aufgaben sind jedoch bestimmte Azure. Die folgenden Abschnitte stellen einige mit Links zu weiteren Informationen.

### <a name="connect-to-the-vm"></a>Verbinden der VM
Die grundlegenden Schritte gehört Verbindung zu Ihrer SQL Server-VM durch Tools wie SQL Server Management Studio (SSMS). Informationen zum Herstellen der neuen SQL Server-VM finden Sie unter [Verbindung zu einem SQL Server virtuellen Computer in Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrieren von Daten
Wenn Sie eine vorhandene Datenbank verfügen, sollten Sie neu bereitgestellten SQL-VM bewegen. Eine Liste der Optionen und Hinweise finden Sie unter [Migration einer Datenbank in SQL Server auf eine Azure-VM](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Hohen Verfügbarkeit konfigurieren
Wenn Sie hohen Verfügbarkeit benötigen, sollten Sie SQL Server Verfügbarkeitsgruppen konfigurieren. Dies umfasst mehrere Azure VMs in einem virtuellen Netzwerk. Azure-Portal ist eine Vorlage, die diese Konfiguration für Sie einrichtet. Weitere Informationen finden Sie unter [Konfigurieren einer Gruppe AlwaysOn Availability in Azure Resource Manager virtuelle Computer](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Der Availability Group und zugeordneten Listener manuell konfigurieren, finden Sie unter [Konfigurieren AlwaysOn Availability Gruppen in Azure VM](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Andere Faktoren hohe Verfügbarkeit finden Sie unter [hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Sichern von Daten
Azure VMs können [Automatisierte Backup](virtual-machines-windows-sql-automated-backup.md)nutzen die regelmäßig Sicherungskopien der Datenbank BLOB-Speicher erstellt. Sie können dieses Verfahren auch manuell verwenden. Weitere Informationen finden Sie unter [Verwenden Azure-Speicher für SQL Server sichern und Wiederherstellen](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Eine Übersicht über alle Optionen zum Sichern und Wiederherstellen finden Sie unter [Backup und Wiederherstellung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatisieren Sie updates
Azure VMs können [Automatische Patches](virtual-machines-windows-sql-automated-patching.md) für die Installation von wichtigen Windows ein Wartungsfenster einplanen und SQL Server automatisch aktualisiert.

### <a name="customer-experience-improvement-program-ceip"></a>Customer Experience Improvement-Programm (CEIP)
Customer Experience Improvement Programm (CEIP) ist standardmäßig aktiviert. Sendet in regelmäßigen Abständen Berichte an Microsoft SQL Server zu verbessern. Es ist keine managementaufgabe mit CEIP erforderlich, wenn Sie nach der Bereitstellung deaktivieren möchten. Sie können anpassen oder am CEIP deaktivieren, indem Sie die VM mit Remotedesktop herstellen. Führen Sie das Dienstprogramm **SQL Server Error and Usage Reporting** . Folgen Sie zum reporting deaktivieren. 

Weitere Informationen finden Sie im Abschnitt CEIP Thema [Lizenzvertrag akzeptieren](https://msdn.microsoft.com/library/ms143343.aspx) . 

## <a name="next-steps"></a>Nächste Schritte
[Durchsuchen der Learning Path](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) für SQL Server auf Azure virtuellen Computern.

Frage? Zuerst finden Sie [In Azure virtuelle Computer häufig gestellte Fragen zu SQL Server](virtual-machines-windows-sql-server-iaas-faq.md). Aber auch Ihre Fragen und Kommentare an das Ende jeder SQL VM Themen Interaktion mit Microsoft und der Community.
