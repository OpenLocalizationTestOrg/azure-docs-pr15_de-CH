<properties
    pageTitle="Migrieren eine SQL Server-Datenbank in SQL Server auf einem virtuellen Computer | Microsoft Azure"
    description="Enthält Informationen zum Migrieren einer lokalen Datenbank auf SQL Server in Azure VM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Migrieren einer SQL Server-Datenbank in SQL Server in eine Azure-VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Ressourcen-Manager-Modell.


Es gibt eine Reihe von Methoden für die Migration einer lokalen SQL Server-Datenbank auf SQL Server in eine Azure-VM. In diesem Artikel wird kurz erläutern Sie verschiedene Methoden empfiehlt die beste Methode für verschiedene Szenarien und enthalten ein [Lernprogramm](#azure-vm-deployment-wizard-tutorial) , durch **Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure Virtual Machine** -Assistent führt. 

Mit dem **Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure Virtual Machine** -Assistenten im [Lernprogramm](#azure-vm-deployment-wizard-tutorial) beschriebenen Methode ist für nur das klassische Bereitstellungsmodell. 

## <a name="what-are-the-primary-migration-methods"></a>Was sind die primären Migrationsmethoden?

Die primären Migrationsdatei sind:

- Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistenten verwenden
- Komprimierung für lokale Sicherung durchführen Sie, und kopieren Sie die Sicherungsdatei manuell in Azure Virtual machine
- Führen Sie eine Sicherung wiederherstellen in Azure VM zu URL aus dem URL
- Trennen und Daten-und Protokolldateien in Azure BLOB-Speicher kopieren und dann über eine Verbindung mit SQL Server in Azure VM URL
- Konvertieren Sie lokalen Rechner in Hyper-V virtuelle Festplatte, Azure BLOB-Speicher hochladen Sie und Bereitstellen Sie dann neue VM mit VHD hochladen
- Schiff Festplatte Windows Import/Export-Dienst
- Wenn lokal eine AlwaysOn-Bereitstellung, [Azure-Assistent hinzufügen](virtual-machines-windows-classic-sql-onprem-availability.md) , erstellen Sie ein Replikat in Azure und Failover Azure Datenbankinstanz auf Benutzer verwenden
- Verwenden Sie SQL Server [Transaktionsreplikation](https://msdn.microsoft.com/library/ms151176.aspx) Azure SQL Server-Instanz als Abonnent konfigurieren, und deaktivieren Sie die Replikation, Azure Datenbankinstanz auf Benutzer



## <a name="choosing-your-migration-method"></a>Wählen die Migrationsmethode

Für eine optimale übertragungsleistung ist Migration der Datenbank in Azure VM eine komprimierte Sicherungsdatei verwenden im Allgemeinen am besten. Dies ist die Methode, die das [Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistenten](#azure-vm-deployment-wizard-tutorial) verwendet. Dieser Assistent ist die empfohlene Methode für die Migration eines lokalen Benutzer Datenbank läuft auf SQL Server 2005 oder SQL Server 2014 größer oder größer, wird die komprimierte Datenbanksicherungsdatei weniger als 1 TB.

Zur Minimierung von Ausfallzeiten während der Migration der Datenbank mithilfe der Option AlwaysOn oder Transaktionsreplikation.

Ist es nicht möglich, die oben genannten Methoden verwenden, manuell migrieren der Datenbank. Diese Methode in der Regel beginnen Sie mit eine folgt eine Sicherung der Datenbank in Azure backup und führen Sie eine Wiederherstellung der Datenbank. Auch die Dateien sich in Azure kopieren können und fügen sie. Gibt es mehrere Methoden, diese manuell migrieren einer Datenbank in eine Azure-VM ausführen können.

> [AZURE.NOTE] Wenn Sie SQL Server 2014 oder SQL Server 2016 aus älteren Versionen von SQL Server aktualisieren, sollten Sie, ob erforderlich sind. Wir empfehlen, Adresse alle Abhängigkeiten von Funktionen als Teil des Migrationsprojekts durch die neue Version von SQL Server nicht unterstützt. Weitere Informationen zu unterstützten Editionen und Szenarien finden Sie unter [Aktualisieren auf SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

In der folgenden Tabelle sind alle primären Migrationsmethoden aufgeführt und erläutert, wann die Verwendung jeder Methode am besten geeignet ist.

| -Methode  | Version der Quelldatenbank  |  Version der Zieldatenbank | Quelle Datenbank Sicherungsgröße Einschränkung  | Notizen  |
|---|---|---|---|---|
| [Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistenten verwenden](#azure-vm-deployment-wizard-tutorial) | SQLServer 2005 oder höher | SQL Server 2014 oder größer | < 1 TB  | Schnellste und einfachste Methode verwenden, um eine neue oder bestehende SQL Server-Instanz in Azure virtuellen Computer migrieren | 
| [Verwenden der Azure-Assistent hinzufügen](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 oder höher | SQL Server 2012 oder höher | [Azure VM-Speicherkapazität](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Minimiert Ausfallzeiten Verwendung bei eine gehosteten Bereitstellung AlwaysOn. |
| [SQL Server Transaktionsreplikation](https://msdn.microsoft.com/library/ms151176.aspx) | SQLServer 2005 oder höher | SQLServer 2005 oder höher | [Azure VM-Speicherkapazität](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Verwenden, wenn Sie zur Minimierung von Ausfallzeiten und keine AlwaysOn lokale Bereitstellung |
| [Komprimierung für lokale Sicherung durchführen Sie, und kopieren Sie die Sicherungsdatei manuell in Azure Virtual machine](#backup-to-file-and-copy-to-vm-and-restore) | SQLServer 2005 oder höher | SQLServer 2005 oder höher | [Azure VM-Speicherkapazität](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Verwenden Sie nur wenn können keine Assistenten wie die Version der Datenbank unter SQL Server 2012 SP1 CU2 wird oder die Datenbank backup ist größer als 1 TB (bei SQL Server 2016 12,8 TB) |
| [Führen Sie eine Sicherung wiederherstellen in Azure VM zu URL aus dem URL](#backup-to-url-and-restore) | SQL Server 2012 SP1 CU2 oder höher | SQL Server 2012 SP1 CU2 oder höher | < 12,8 TB für SQL Server 2016 andernfalls < 1 TB | Im Allgemeinen ist [URL backup](https://msdn.microsoft.com/library/dn435916.aspx) äquivalent zu mithilfe des Assistenten und nicht so einfach |
| [Trennen und Daten-und Protokolldateien in Azure BLOB-Speicher kopieren und dann über eine Verbindung mit SQL Server in Azure virtuellen URL](#detach-and-copy-to-url-and-attach-from-url) | SQLServer 2005 oder höher | SQL Server 2014 oder größer | [Azure VM-Speicherkapazität](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Verwenden Sie diese Methode, wenn Sie [diese Dateien mit Azure Blob Storage Service](https://msdn.microsoft.com/library/dn385720.aspx) und Azure-VM, insbesondere bei großen Datenbanken aus SQL Server zuordnen |
| [Lokalen Computer in Hyper-V virtuelle Festplatten konvertieren in Azure BLOB-Speicher hochladen und Bereitstellen eines neuen virtuellen Computers mit hochgeladenen VHD](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQLServer 2005 oder höher | SQLServer 2005 oder höher | [Azure VM-Speicherkapazität](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Verwendung bei Datenbanken im Rahmen der Migration von anderen Datenbanken oder Datenbanken Datenbank zusammen [bringt SQL Server-Lizenz](../sql-database/sql-database-paas-vs-sql-server-iaas.md), eine Datenbank migrieren, die System- und Migration für eine ältere Version von SQL Server oder ausgeführt wird. |
| [Schiff Festplatte Windows Import/Export-Dienst](#ship-hard-drive) | SQLServer 2005 oder höher | SQLServer 2005 oder höher | [Azure VM-Speicherkapazität](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | [Windows Import/Export-Dienst](../storage/storage-import-export-service.md) verwenden Sie, wird manuelle Kopiermethode langsam, wie sehr große Datenbanken |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Azure VM-Bereitstellung Assistent Lernprogramm

**Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure Virtual Machine** -Assistenten in Microsoft SQL Server Management Studio verwenden, um SQL Server 2005 migrieren, SQL Server 2008, SQL Server 2008 R2, SQL Server 2012, 2014 SQL Server oder SQL Server 2016 lokalen Benutzer Datenbank (bis zu 1 TB) SQL Server 2014 oder SQL Server 2016 in Azure VM. Mithilfe dieses Assistenten eine Datenbank vorhandenen Azure virtuellen Computer oder eine VM Azure mit SQL Server während der Migration vom Assistenten erstellten migrieren. Beim Migrieren einer Datenbank zu einer neueren Version von SQL Server wird die Datenbank automatisch während des Vorgangs aktualisiert.

Die Methode ist für nur das klassische Bereitstellungsmodell. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Neueste Version Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistenten abgerufen.

Verwenden Sie die neueste Version von Microsoft SQL Server Management Studio für SQL Server, um sicherzustellen, dass Sie die neueste Version des Assistenten zum **Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure Virtual Machine** . Die aktuellste Version dieses Assistenten enthält die neuesten Updates zum klassischen Azure-Portal und unterstützt die neuesten Azure VM Bilder in der Galerie (ältere Versionen des Assistenten möglicherweise nicht funktionieren). Die neueste Version von Microsoft SQL Server Management Studio für SQL Server [downloaden](http://go.microsoft.com/fwlink/?LinkId=616025) und mit der Datenbank, mit denen Sie migrieren und mit dem Internet auf einem Clientcomputer installieren.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Konfigurieren Sie die vorhandenen virtuellen Computer Azure und SQL Server-Instanz (falls zutreffend)

Wenn Sie eine vorhandene Azure VM migrieren, sind die folgenden Konfigurationsschritte erforderlich:

- Konfigurieren der Azure-VM und die SQL Server-Instanz Verbindung von einem anderen Computer aktivieren, indem Sie die Schritte zum [Verbinden mit der SQL Server-VM-Instanz von SSMS auf einem anderen Computer](virtual-machines-windows-sql-connect.md). Nur SQL Server 2014 und SQL Server 2016 Bilder in der Galerie werden unterstützt, wenn Sie mit dem Assistenten migrieren.
- Konfigurieren eines offenen Endpunkts für den Dienst SQL Server Cloud Adapter Microsoft Azure-Gateways mit privaten 11435. Dieser Port wird als Teil des SQL Server 2014 oder SQL Server 2016 Bereitstellung auf Microsoft Azure VM erstellt. Cloud-Adapter erstellt auch eine Regel Windows Firewall eingehenden TCP-Verbindungen am Standardport 11435 ermöglichen. Dieser Endpunkt kann der Assistent den Dienst Cloud-Adapter die lokale Instanz der Azure-VM Sicherungsdateien verwenden. Weitere Informationen finden Sie in der [Cloud Adapter für SQL Server](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Cloud-Adapter erstellen](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Der Verwendung Bereitstellen einer SQL Server-Datenbank eine Microsoft Azure VM-Assistenten ausführen

1. Öffnen Sie Microsoft SQL Server Management Studio für Microsoft SQL Server 2016 und Verbinden mit der SQL Server-Instanz mit der Benutzerdatenbank ein Azure VM migriert.
2. Mit der rechten Maustaste in der Datenbank, die Sie migrieren, zeigen Sie auf Aufgaben und dann auf Bereitstellen einer Microsoft Azure VM.

    ![Assistenten starten](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Klicken Sie auf der Einführungsseite auf Weiter.
4. Schließen Sie auf der Seite Einstellungen der SQL Server-Instanz mit der Datenbank eine Azure VM migriert werden.
5. Geben Sie einen temporären Speicherort für die Sicherungsdateien. Wenn die Verbindung mit einem remote-Server müssen Sie ein Netzlaufwerk angeben.

    ![Einstellungen](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Klicken Sie auf Weiter.
7. Klicken Sie auf der Seite Microsoft Azure anmelden Anmelden und Azure-Konto anmelden.
8. Wählen Sie das Abonnement, das Sie nutzen möchten und klicken Sie auf Weiter.

    ![Azure anmelden](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Sie können auf der Seite Deployment Settings angeben, einen neuen oder einen vorhandenen Cloud-Dienst Namen und virtuellen Computer:

 - Geben Sie einen neuen Namen Cloud und virtuellen Namen einen neuen Cloud-Dienst mit einem neuen Azure virtuellen Computer mit SQL Server 2014 oder SQL Server 2016 Galerie Bild erstellen.

     - Wenn Sie eine neue Cloud-Dienst angeben, geben Sie das Speicherkonto, das Sie verwenden.

     - Wenn Sie einen vorhandenen Cloud-Dienst-Namen angeben, wird das Speicherkonto abgerufen und eingegeben.

 - Geben Sie einen vorhandenen Cloud-Dienst und neuen virtuellen Namen einen neuen Azure virtuellen Computer in einem vorhandenen Cloud-Dienst erstellen. Nur SQL Server 2014 oder SQL Server 2016 Bild angeben.
 - Geben Sie vorhandenen Cloud-Dienstname und Name des virtuellen Computers mit vorhandenen Azure virtuellen Computer an. Dabei muss es sich um ein Bild mit SQL Server 2014 oder SQL Server 2016 Bild erstellt.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Klicken Sie
  - Wenn Sie einen vorhandenen Cloud-Dienst-Name und Name des virtuellen Computers angegeben, werden Sie aufgefordert, Benutzernamen und Kennwort angeben.

        ![Azure Einstellungen](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Wenn Sie einen neuen virtuellen Namen angegeben haben, werden Sie aufgefordert, wählen Sie ein Bild aus der Galeriebilder folgende Angaben:
      - Bild: Wählen Sie nur SQL Server 2014 oder SQL Server 2016
        - Benutzername
        - Neues Kennwort
        - Kennwort bestätigen
        - Speicherort
        - Größe.
    - Darüber hinaus automatisch generierte Zertifikat für diese neue Microsoft Azure Virtual Machine akzeptieren klicken Sie, und klicken Sie auf OK.

    ![Azure neue Computer settings](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Geben Sie der Name der Zieldatenbank an, falls der Name der Quelldatenbank. Wenn die Zieldatenbank bereits vorhanden ist, wird das System automatisch erhöht den Datenbanknamen anstatt vorhandene Datenbank überschreiben.
12. Klicken Sie auf Weiter, und klicken Sie dann auf Fertig stellen.

    ![Ergebnisse](./media/virtual-machines-windows-migrate-sql/results.png)

13. Nach Abschluss des Assistenten an den virtuellen Computer und überprüfen Sie, ob die Datenbank migriert wurden.
14. Erstellt einen neuen virtuellen Computer konfigurieren Sie durch die Schritte zum [Verbinden mit der SQL Server-VM-Instanz von SSMS auf einem anderen Computer](virtual-machines-windows-sql-connect.md)Azure VM und die SQL Server-Instanz.

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Backup-Datei und Kopieren in die VM und Wiederherstellung

Verwenden Sie diese Methode beim Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistenten verwenden können entweder auf eine Version von SQL Server vor SQL Server 2014 migrieren oder die Sicherungsdatei größer als 1 TB. Ist die Sicherungsdatei größer als 1 TB, müssen Sie es Streifen ist die maximale Größe einer VM-Festplatte 1 TB. Verwenden Sie die folgenden allgemeinen Schritte zum Migrieren einer Datenbank mit dieser manuellen Methode:

1.  Führen Sie eine vollständige Sicherung auf einen lokalen Speicherort.
2.  Erstellen oder einen virtuellen Computer mit der Version von SQL Server gewünscht hochladen.
3.  Richten Sie Ihre Bedürfnisse Konnektivität. [Verbinden mit einer SQL Server-VM auf Azure (Resource Manager)](virtual-machines-windows-sql-connect.md)angezeigt.
4.  Kopieren Sie Ihre Sicherungsdateien für die VM mit remote Desktop, Windows Explorer oder den Kopierbefehl aus.

## <a name="backup-to-url-and-restore"></a>Backup-to-URL und Wiederherstellung

Verwenden Sie die [URL](https://msdn.microsoft.com/library/dn435916.aspx) , wenn Sie das Bereitstellen einer SQL Server-Datenbank Microsoft Azure VM-Assistenten verwenden können, da die Sicherungsdatei größer als 1 TB und von und SQL Server 2016 migrieren. Für Datenbanken, die kleiner als 1 TB oder eine Version von SQL Server vor SQL Server 2016 des Assistenten empfohlen. SQL Server 2016 werden backup Stripesets werden unterstützt aus Leistungsgründen empfohlen und müssen pro Blob Größe überschreiten. Für sehr große Datenbanken empfiehlt sich die Verwendung des [Windows-Import/Export-Dienst](../storage/storage-import-export-service.md) .

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Trennen und URL kopieren und Zuordnen von URL

Verwenden Sie diese Methode, wenn Sie [diese Dateien mit Azure Blob Storage Service](https://msdn.microsoft.com/library/dn385720.aspx) und Azure-VM, insbesondere bei großen Datenbanken aus SQL Server zuordnen. Verwenden Sie die folgenden allgemeinen Schritte zum Migrieren einer Datenbank mit dieser manuellen Methode:

1.  Trennen Sie die Dateien von der lokalen Datenbankinstanz.
2.  Dateien der getrennten Datenbank in Azure BLOB-Speicher mit dem [Befehlszeilen-Dienstprogramm AZCopy](../storage/storage-use-azcopy.md).
3.  Die SQL Server-Instanz in der Azure-VM Datenbankdateien aus dem Azure-URL zuordnen.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Konvertieren in VM URL hochladen und Bereitstellen als neuen VM

Verwenden Sie diese Methode, um alle System- und Benutzerdatenbanken in einer lokalen SQL Server-Instanz Azure virtuellen Computer migrieren. Verwenden Sie die folgenden allgemeinen Schritte eine vollständige SQL Server-Instanz dieser manuellen Methode migrieren:

1.  Konvertieren Sie physischen oder virtuellen Computern mit [Microsoft Virtual Machine Konverter](http://technet.microsoft.com/library/dn873998.aspx)in Hyper-V virtuelle Festplatten.
2.  Hochladen Sie VHD-Dateien auf Azure-Speicher mithilfe des [Add-AzureVHD-Cmdlet](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3.  Bereitstellen Sie einen neuen virtueller Computer mithilfe der hochgeladenen VHD.

> [AZURE.NOTE] Migrieren eine gesamte Anwendung verwenden Sie [Azure Site Recovery](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Festplatte geliefert

Mithilfe der [Windows-Import/Export-Webdienstmethode](../storage/storage-import-export-service.md) große Mengen von Daten in Azure BLOB-Speicher in Situationen übertragen über das Netzwerk hochladen teuer oder nicht durchführbar ist. Mit diesem Dienst senden Sie eine oder mehrere Festplatten mit Daten, die eine Azure-Rechenzentrum, in dem Ihre Daten an das Speicherkonto hochgeladen werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Ausführen von SQL Server auf Azure-Computer finden Sie unter [SQL Server auf Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

Anleitung zum Erstellen einer Azure SQL Server Virtual Machine aus eine Aufnahme finden Sie unter [Tipps & Tricks zum "Klonen" SQL Azure virtueller Maschinen von Aufnahmen](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) CSS SQL Server Ingenieure Blog.
