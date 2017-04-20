<properties
    pageTitle="SQL Server auf Azure Virtual Machines FAQ | Microsoft Azure"
    description="Dieser Artikel enthält Antworten auf häufig gestellte Fragen zu SQL Server unter Azure VMs ausgeführt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="sql-server-on-azure-virtual-machines-faq"></a>SQL Server auf Azure Virtual Machines FAQ

Dieses Thema enthält Antworten auf einige häufig gestellte Fragen zum Ausführen von [SQL Server auf Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/sql-server/).

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

1. **Wie erstelle ich einen Azure virtuellen Computer mit SQL Server?**

    Es gibt zwei Methoden zur Verfügung. Die einfachste Lösung ist, die SQL Server enthält einen virtuellen Computer erstellen. Eine Anmeldung bei Azure und SQL VM aus dem Portal finden Sie unter [Bereitstellung eines virtuellen Computers SQL Server in der Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Sie können auch manuell installieren von SQL Server auf einem virtuellen Computer und Wiederverwenden von einer lokalen Lizenz [Lizenz](https://azure.microsoft.com/pricing/license-mobility/)Mobilität durch Software Assurance in Azure.

1. **Was ist der Unterschied zwischen SQL VMs und der SQL-Datenbankdienst?**

    Konzeptionell läuft SQL Server auf Azure virtuellen Computer nicht von SQL Server in einem Rechenzentrum remote ausgeführt. Im Gegensatz dazu bietet [SQL](../sql-database/sql-database-technical-overview.md) -Datenbank als Dienst. Mit SQL-Datenbank haben Sie nicht Zugriff auf den Computer, die Ihre Datenbanken hosten. Einen vollständigen Vergleich finden Sie unter [eine Wolke SQL Server Option auswählen: Azure SQL (PaaS) Datenbank oder SQL Server auf Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

1. **Wie kann ich meine lokale SQL Server-Datenbank in die Cloud migrieren?**

    Zunächst erstellen Sie Azure VM mit einer SQL Server-Instanz. Migrieren Sie Ihre lokalen Datenbanken für diese Instanz. Strategien zum Migrieren finden Sie unter [Migrieren einer SQL Server-Datenbank SQL Server in eine Azure-VM](virtual-machines-windows-migrate-sql.md).

2. **Kann Ändern der installierten Features oder installieren eine zweite Instanz von SQL Server auf dem gleichen virtuellen Computer?**

    Ja. SQL Server-Installationsmedium befindet sich in einem Ordner auf Laufwerk **C** . Führen Sie **Setup.exe** von dort neue SQL Server-Instanzen hinzufügen oder andere installierte Features von SQL Server auf dem Computer ändern.

3. **Wie aktualisiere ich auf eine neue Version/Edition von SQL Server in eine Azure-VM?**

    Derzeit ist keine direkte Aktualisierung für SQL Server in einer VM Azure ausgeführt. Erstellen Sie einen neuen Azure virtuellen Computer mit der gewünschten SQL Server-Version/Edition, und migrieren Sie die Datenbanken auf dem neuen Server mit den [Daten Migrationsmethoden](virtual-machines-windows-migrate-sql.md).

4. **Wie kann ich meine lizenzierte Kopie von SQL Server auf eine Azure-VM installieren?**

    Kopieren der SQL Server-Installationsmedien für die Windows Server-VM und installieren Sie SQL Server auf dem virtuellen Computer. Lizenzierung Gründe, müssen [License Mobility über Software Assurance in Azure](https://azure.microsoft.com/pricing/license-mobility/).

5. **Haben Sie die Kosten SQL für eine VM nur für Standby-Failover verwendet wird?**

    Wenn Sie die SQL-VM in der Sammlung erstellen, benötigen Sie eine separate Lizenz für die Standby-SQL VM und die Preisgestaltung entspricht. Wenn Sie SQL Server auf einem virtuellen Computer mit [Lizenzmobilität](https://azure.microsoft.com/pricing/license-mobility/)manuell installieren, können Sie eine kostenlose passiven SQL Instanz für Failover. Überprüfen Sie die Einschränkungen und.

6. **Wie werden Updates und Servicepacks auf einem SQL Server-VM angewendet?**

    Virtuelle Maschinen bieten Kontrolle über Host-Computer, einschließlich wann und wie Updates anwenden. Für das Betriebssystem können Sie Windows Updates manuell anwenden oder einen Terminplan Dienst namens [Automatische Patches](virtual-machines-windows-classic-sql-automated-patching.md)aktivieren. Automatische Patches installiert alle Updates, die wichtig, einschließlich SQL Server-Updates in dieser Kategorie sind. Andere optionale Updates für SQL Server müssen manuell installiert werden.

7. **Ist es möglich, Konfigurationen im virtuellen Katalog (z. B. Windows 2008 R2 + SQL Server 2012) nicht angezeigt?**

    Nein. Für Katalog Images von virtuellen Maschinen, die SQL Server enthalten, müssen Sie eines der angegebenen Bilder auswählen.

9. **Wie installiere ich Datentools SQL Azure-VM?**

    Downloaden Sie und installieren Sie die SQL-Tools von [Microsoft SQL Server Data Tools - Business Intelligence für Visual Studio 2013 ](https://www.microsoft.com/en-us/download/details.aspx?id=42313).

## <a name="resources"></a>Ressourcen

Eine Übersicht über SQL Server auf Azure Virtual Machines sehen Sie video [Azure VM ist die optimale Plattform für SQL Server 2016 an](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016) Sie erhalten auch eine gute Einführung in das Thema [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

Weitere Ressourcen sind verfügbar:

- [Bereitstellen Sie einen SQL Server virtueller Computer in Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md)
- [Migration einer Datenbank auf SQL Server in Azure VM](virtual-machines-windows-migrate-sql.md)
- [Hohe Verfügbarkeit und Disaster Recovery für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
- [Best Practices zur Performance für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
- [Anwendungsmuster und Entwicklungsstrategien für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)
