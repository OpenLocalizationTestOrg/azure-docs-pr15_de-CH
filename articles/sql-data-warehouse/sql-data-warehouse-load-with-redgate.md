<properties
   pageTitle="Verwenden Redgates Data Platform Studio zum Laden von Daten in SQL Data Warehouse | Microsoft Azure"
   description="Informationen Sie zum Redgates Daten-Plattform Studio für Data warehousing-Szenarios verwenden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>


# <a name="load-data-with-redgate-data-platform-studio"></a>Laden von Daten mit Redgate Daten Plattform

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)

Dieses Lernprogramm veranschaulicht die [Redgates Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) verwenden, um Daten aus einer lokalen SQL Server in Azure SQL Data Warehouse verschieben. Daten Plattform Studio wendet die am besten geeignete Kompatibilitätspatches und Optimierungen ist am schnellsten Einstieg in SQL Data Warehouse.

> [AZURE.NOTE] [Redgate](http://www.red-gate.com) ist ein langjähriger Microsoft-Partner, der verschiedene SQL Server-Tools bietet. Dieses Feature Data Platform Studio frei für kommerziellen und nicht kommerziellen verwendet wurde.

## <a name="before-you-begin"></a>Bevor Sie beginnen
### <a name="create-or-identify-resources"></a>Erstellen oder Ressourcen

Bevor Sie dieses Lernprogramm starten, benötigen Sie:

- **Lokale SQL Server-Datenbank**: die SQL Data Warehouse importieren möchten Daten aus einer lokalen SQL Server (Version 2008 R2 oder höher). Daten-Plattform Studio können keine Daten direkt aus einer Azure SQL-Datenbank oder aus Dateien importieren.
- **Azure Storage-Konto**: Data Platform Studio Stufen Daten in Azure BLOB-Speicher vor dem Laden in SQL Data Warehouse. Das Speicherkonto muss das "Klassische" Bereitstellungsmodell Resource Manager-Bereitstellungsmodell (Standardeinstellung) verwenden. Haben Sie ein Speicherkonto, erfahren Sie, wie ein Speicherkonto erstellen. 
- **SQL Data Warehouse**: Dieses Lernprogramm verschiebt die Daten aus lokalen SQL Server SQL Data Warehouse müssen Sie online ein Datawarehouse haben. Wenn Sie noch nicht über ein Datawarehouse einer Azure SQL Data Warehouse erstellen verfügen.

> [AZURE.NOTE] Leistung wird verbessert, wenn das Speicherkonto und das Datawarehouse in derselben Region erstellt werden.

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Schritt 1: Daten Plattform Studio mit Ihrem Azure-Konto anmelden
Öffnen Sie Ihren Webbrowser, und navigieren Sie zu [Daten Plattform Studio](https://www.dataplatformstudio.com/) -Website. Melden Sie sich mit der gleichen Azure Konto Lagerhaus Konto und Daten erstellen. Wenn Ihre e-Mail-Adresse einer Arbeit oder schulkonto und ein Microsoft-Konto zugeordnet ist, müssen Sie das Konto auswählen, das auf Ressourcen zugreifen.

> [AZURE.NOTE] Ist dies zum ersten Mal Daten Plattform Studio verwenden, müssen Sie der Berechtigung zum Verwalten von Azure Ressourcen der Anwendung.

## <a name="step-2-start-the-import-wizard"></a>Schritt 2: Starten Sie den Importassistenten
Wählen Sie auf dem Hauptbildschirm DPS in Azure SQL Data Warehouse Verknüpfung Starten des Importassistenten importieren.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Schritt 3: Installieren des Platform-Studio-Gateways
Verbindung mit der lokalen SQL Server-Datenbank müssen Sie die DPS-Gateway installiert. Das Gateway ist ein Client-Agent ermöglicht den Zugriff auf Ihre lokale Umgebung, die die Daten extrahiert und an das Speicherkonto hochgeladen. Daten durchlaufen niemals Redgate Server. So installieren Sie das Gateway

1.  Klicken Sie auf **Gateway erstellen**
2. Downloaden Sie und installieren Sie das Gateway mit dem bereitgestellten installer

![][2]

> [AZURE.NOTE] Das Gateway kann auf jedem Computer mit Netzwerkzugriff auf die SQL Server-Datenbank installiert werden. Er greift auf die SQL Server-Datenbank mit den Anmeldeinformationen des aktuellen Benutzers.

Nach der Installation der Gateway-Status wechselt zu verbunden, und wählen Sie weiter.

## <a name="step-4-identify-the-source-database"></a>Schritt 4: Identifizieren der Quelldatenbank
Geben Sie im Textfeld *Servername Geben Sie* den Namen des Servers, der die Datenbank hostet, und wählen Sie **Weiter**. Im Dropdown-Menü Wählen Sie die Datenbank Daten importiert werden sollen.

![][3]

DPS überprüft die ausgewählte Datenbank Tabellen importieren. Standardmäßig importiert DPS alle Tabellen in der Datenbank. Sie können aktivieren oder Deaktivieren von Tabellen durch die Verknüpfung aller Tabellen erweitern. Wählen Sie die Schaltfläche Weiter nach vorne verschieben.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Schritt 5: Wählen Sie ein Speicherkonto zu Daten
DPS aufgefordert, einen Speicherort für die Daten bereitstellen. Wählen Sie ein vorhandenes Speicherkonto aus Ihrem Abonnement und **Weiter**.

> [AZURE.NOTE] DPS erstellen einen neuen BLOB-Container in das ausgewählte Speicherkonto und unterschiedlichen Ordner für jeden Import verwenden.

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Schritt 6: Auswählen eines Datawarehouses
Wählen Sie eine online [Azure SQL Data Warehouse](http://aka.ms/sqldw) -Datenbank die Daten importieren. Nach Auswahl die Datenbank müssen Sie die Anmeldeinformationen zum Verbinden mit der Datenbank und wählen **Weiter**.

![][5]

> [AZURE.NOTE] DPS verbindet die Datenquelltabellen in das Datawarehouse. DPS warnt Sie, wenn der Tabellenname überschreiben vorhandene Tabellen im Datawarehouse erforderlich ist. Sie können optional vorhandenen Objekte im Datawarehouse löschen Löschen markieren alle Objekte vor dem Import.

## <a name="step-7-import-the-data"></a>Schritt 7: Importieren
DPS bestätigt, dass Sie die Daten importieren möchten. Klicken Sie die Schaltfläche Start importieren, um den Datenimport zu starten.

![][6]

DPS zeigt den Fortschritt und Hochladen der Daten aus der lokalen SQL Server und den Fortschritt des Importvorgangs in SQL Data Warehouse Visualisierung.

![][7]

Nachdem der Importvorgang abgeschlossen ist, zeigt DPS eine Zusammenfassung des Datenimports und ein Bericht des Kompatibilitätspatches, die ausgeführt wurden.

![][8]

## <a name="next-steps"></a>Nächste Schritte
Starten Sie zum Untersuchen der Daten im SQL Data Warehouse anzeigen:

- [Abfrage SQL Azure Datawarehouse (Visual Studio)][]
- [Daten Sie mit Power BI][]

Weitere Informationen zu Redgates Daten-Plattform Studio:

- [Besuchen Sie die DPS-homepage](http://www.dataplatformstudio.com/)
- [Demo über DPS auf Kanal 9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Eine Übersicht über andere Arten migrieren und Laden der Daten im SQL Data Warehouse finden Sie unter:

- [Migrieren der Lösung zu SQL Data Warehouse][]
- [Laden von Daten in Azure SQL Data Warehouse](./sql-data-warehouse-overview-load.md)

Weitere Hinweise zur Entwicklung finden Sie unter [SQL Data Warehouse Development Overview](./sql-data-warehouse-overview-develop.md).

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Abfrage SQL Azure Datawarehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Daten Sie mit Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrieren der Lösung zu SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md