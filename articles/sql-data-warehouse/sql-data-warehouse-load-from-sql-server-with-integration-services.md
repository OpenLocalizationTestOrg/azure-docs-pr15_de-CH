<properties
   pageTitle="Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS) | Microsoft Azure"
   description="Veranschaulicht das Erstellen eines Pakets SQL Server Integration Services (SSIS), um Daten aus einer Vielzahl von Datenquellen in SQL Data Warehouse verschieben."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)

> [AZURE.SELECTOR]
- [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Erstellen eines Pakets SQL Server Integration Services (SSIS) zum Laden von Daten aus SQL Server in Azure SQL Data Warehouse. Sie können optional umstrukturieren, Transformieren und Bereinigen von Daten an SSIS-Datenfluss.

In diesem Lernprogramm werden Sie:

- Erstellen Sie ein neues Integration Services-Projekt in Visual Studio.
- Verbinden Sie mit Datenquellen, einschließlich SQL Server (als Quelle) und SQL Data Warehouse (als Ziel).
- Entwerfen Sie eine SSIS-Paket, die Daten aus der Quelle in das Ziel geladen.
- Führen Sie das SSIS-Paket, um die Daten zu laden.

Diese praktische Einführung verwendet SQL Server als Datenquelle. SQL Server kann lokal oder in Azure virtuellen Computer ausgeführt werden.

## <a name="basic-concepts"></a>Grundlegende Konzepte

Das Paket ist in SSIS Arbeitseinheit. Zugehörige Pakete sind in Projekten gruppiert. In Visual Studio mit SQL Server Tools erstellen Sie Projekte und Designpakete. Der Entwurfsprozess ist visuelle Prozess in dem Sie ziehen legen Sie Komponenten aus der Toolbox auf die Entwurfsoberfläche, verbinden und deren Eigenschaften festlegen. Nachdem Sie das Paket haben, können Sie optional es auf SQL Server für umfassendes Management, Überwachung und Sicherheit bereitstellen.

## <a name="options-for-loading-data-with-ssis"></a>Optionen zum Laden von Daten mit SSIS

SQL Server Integration Services (SSIS) ist eine flexible Reihe von Tools, die eine Vielzahl von Optionen zum Verbinden mit und Laden von Daten in SQL Data Warehouse bietet.

1. Verbindung mit SQL Data Warehouse verwenden Sie ein ADO-NET-Ziel. In diesem Lernprogramm verwendet ein ADO-NET-Ziel, da die wenigsten Konfigurationsoptionen hat.
2. Verwenden Sie OLE DB-Ziel für die Verbindung mit SQL Data Warehouse. Diese Option können etwas bessere Leistung als die ADO NET.
3. Mit Azure Blob hochladen Task können Daten in Azure BLOB-Speicher bereitstellen. Können Sie SSIS führen SQL-Task ein Polybase Skript gestartet, das die Daten in SQL Data Warehouse lädt. Diese Option bietet die beste Leistung von drei hier aufgeführten Optionen. Zu der Aufgabe Azure Blob hochladen [Microsoft SQL Server 2016 Integration Services Feature Pack für Azure][]herunterladen Weitere Informationen zu Polybase finden Sie unter [PolyBase Guide][].

## <a name="before-you-start"></a>Bevor Sie beginnen

Um dieses Lernprogramm durchgehen, müssen wie folgt vor:

1. **SQL Server Integrationsservices (SSIS)**. SSIS ist eine Komponente von SQL Server und erfordert eine Testversion oder eine Vollversion von SQL Server. Eine Evaluierungsversion von SQL Server 2016 Vorschau finden Sie unter [SQL Server evaluieren möchten][].
2. **Visual Studio**. Die kostenlose Visual Studio 2015 Community Edition finden Sie unter [Visual Studio-Community][].
3. **SQL Server Data Tools für Visual Studio (SSDT)**. SQL Server Data Tools für Visual Studio 2015 finden Sie unter [Download SQL Server Data Tools (SSDT)][].
4. **Beispieldaten**. Diese praktische Einführung verwendet Beispieldaten in SQL Server in der Datenbank AdventureWorks als Quelldaten in SQL Data Warehouse geladen werden. Die Beispieldatenbank AdventureWorks finden Sie [Beispieldatenbanken für AdventureWorks 2014][].
5. **Ein SQL Data Warehouse-Datenbank und Berechtigungen**. Dieses Lernprogramm mit SQL Data Warehouse-Instanz verbunden und Daten in diese lädt. Sie müssen Berechtigungen zum Erstellen und Laden von Daten.
6. **Eine Firewall-Regel**. Sie müssen eine Firewallregel SQL Data Warehouse mit der IP-Adresse des lokalen Computers erstellen, bevor Sie SQL Data Warehouse Daten hochladen können.

## <a name="step-1-create-a-new-integration-services-project"></a>Schritt 1: Erstellen Sie Neues Integration Services-Projekt

1. Starten Sie Visual Studio 2015.
2. Wählen Sie im Menü **Datei** auf neue **| Projekt**.
3. Navigieren Sie zu der **installiert | Vorlagen | Business Intelligence | Integrationsservices** Projekttypen.
4. **Integration Services-Projekt**auswählen Geben Sie Werte für **Name** und **Speicherort**, und klicken Sie auf **OK**.

Visual Studio wird geöffnet und erstellt ein neues Integration Services (SSIS)-Projekt. Öffnet Visual Studio den Designer für die einzelnen neuen SSIS-Paket (Package.dtsx) im Projekt. Sehen Sie die folgenden Flächen:

- Auf der linken Seite der **Toolbox** SSIS-Komponenten.
- In der Mitte der Entwurfsoberfläche mit mehreren Registerkarten. Sie verwenden in der Regel mindestens Registerkarten **Daten** und **Steuerung** .
- Rechts im **Projektmappen-Explorer** und das Fenster **Eigenschaften** .

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Schritt 2: Erstellen der Daten

1. Ziehen Sie einen Datenflusstask aus der Toolbox in die Mitte der Entwurfsoberfläche (auf der Registerkarte **Steuerung** ).

    ![][02]

2. Doppelklicken Sie auf Datenflusstask wechseln zur Registerkarte Daten.
3. Ziehen Sie ein ADO.NET Quelle aus anderen Quellen in der Toolbox auf die Entwurfsoberfläche. Ändern Sie den Namen mit markiertem Quelladapter mit **SQL Server-Datenquelle** im **Eigenschaftenbereich** .
4. Ziehen Sie ein ADO.NET Ziel aus anderen Ziele in der Toolbox auf die Entwurfsoberfläche unter ADO.NET Quelle. Mit dem Zieladapter ausgewählt ändern Sie den Namen **SQL DW** Ziel im **Eigenschaftenbereich** .

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Schritt 3: Konfigurieren der Quelladapter

1. Doppelklicken Sie auf Quelladapter **ADO.NET Quellcode-Editor**öffnen.

    ![][03]

2. Der **Verbindungs-Manager-** Registerkarte **ADO.NET Quellcode-Editor**klicken Sie auf **neu** neben **ADO.NET Verbindungs-Manager-** Liste zu öffnen das Dialogfeld **Konfigurieren ADO.NET Verbindungs-Manager-** Verbindungszeichenfolge für die SQL Server-Datenbank aus der in diesem Lernprogramm Daten lädt.

    ![][04]

3. Klicken Sie im Dialogfeld **Konfigurieren ADO.NET Verbindungs-Manager** klicken Sie **neu** im Dialogfeld **Verbindungs-Manager** öffnen und erstellen Sie eine neue Datenverbindung.

    ![][05]

4. Klicken Sie im Dialogfeld **Verbindungs-Manager** tun.

    1. Wählen Sie für **Anbieter**SqlClient Data Provider.
    2. Geben Sie den Namen SQL Server **Servername**.
    3. Wählen Sie im Abschnitt **Server melden** oder geben Sie Authentifizierungsinformationen.
    4. Wählen Sie unter **Verbindung mit einer Datenbank** der Beispieldatenbank AdventureWorks.
    5. Klicken Sie auf **Testverbindung**.
    
        ![][06]
    
    6. Klicken Sie im Dialogfeld, das die Ergebnisse des Verbindungstests meldet, klicken Sie auf **OK** , um das Dialogfeld **Verbindungs-Manager** zurückzukehren.
    7. Klicken Sie im Dialogfeld **Verbindungs-Manager** klicken Sie auf **OK** , um das Dialogfeld **Konfiguration ADO.NET Verbindungs-Manager** zurückzukehren.
 
5. Klicken Sie im Dialogfeld **ADO.NET Verbindungs-Manager konfigurieren** auf **OK** , um den **ADO.NET Quellcode-Editor**zurückzukehren.
6. Wählen Sie im **ADO.NET Quellcode-Editor**in der Liste **Name der Tabelle oder der Ansicht** **Sales.SalesOrderDetail** Tabelle.

    ![][07]

7. Klicken Sie auf **Vorschau** , um die ersten 200 Zeilen der Daten in der Quelltabelle im Dialogfeld **Vorschau der Abfrageergebnisse** anzuzeigen.

    ![][08]

8. Klicken Sie im Dialogfeld **Vorschau der Abfrageergebnisse** **Schließen** **ADO.NET Quellcode-Editor**zurückzukehren.
9. Klicken Sie in **ADO.NET Quellcode-Editor**auf **OK** , um die Konfiguration der Datenquelle abzuschließen.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Schritt 4: Quelladapter Ziel-Adapter anschließen

1. Wählen Sie den Quelladapter auf der Entwurfsoberfläche.
2. Wählen Sie den blauen Pfeil, der von dem Quelladapter erweitert und in den Ziel-Editor ziehen Sie, bis er einrastet.

    ![][10]

    In einem normalen SSIS-Paket wird eine Reihe von anderen Komponenten von SSIS-Toolbox zwischen Quelle und Ziel umstrukturieren, Transformieren und bereinigen Sie Ihre Daten an SSIS-Datenfluss verwenden. Um dieses Beispiel möglichst einfach zu halten, verbinden wir die Quelle an das Ziel.

## <a name="step-5-configure-the-destination-adapter"></a>Schritt 5: Konfigurieren Sie den Zieladapter

1. Doppelklicken Sie auf den Zieladapter **ADO.NET Ziel-Editor**öffnen.

    ![][11]

2. Der **Verbindungs-Manager-** Registerkarte **ADO.NET Ziel-Editor**klicken Sie auf **neu** neben der **Verbindungs-Manager** -Liste **Konfigurieren ADO.NET Verbindungs-Manager** -Dialogfeld öffnen und Einstellungen für dieses Lernprogramm in die Daten lädt Azure SQL Data Warehouse-Datenbank erstellen.
3. Klicken Sie im Dialogfeld **Konfigurieren ADO.NET Verbindungs-Manager** klicken Sie **neu** im Dialogfeld **Verbindungs-Manager** öffnen und erstellen Sie eine neue Datenverbindung.
4. Klicken Sie im Dialogfeld **Verbindungs-Manager** tun.
    1. Wählen Sie für **Anbieter**SqlClient Data Provider.
    2. **Servername**Geben Sie den Namen SQL Data Warehouse.
    3. Im Abschnitt **Anmelden an den Server** wählen Sie **mit SQL Server-Authentifizierung** und geben Sie Authentifizierungsinformationen.
    4. Wählen Sie im Abschnitt **mit Datenbank verbinden** einer vorhandenen SQL Data Warehouse-Datenbank.
    5. Klicken Sie auf **Testverbindung**.
    6. Klicken Sie im Dialogfeld, das die Ergebnisse des Verbindungstests meldet, klicken Sie auf **OK** , um das Dialogfeld **Verbindungs-Manager** zurückzukehren.
    7. Klicken Sie im Dialogfeld **Verbindungs-Manager** klicken Sie auf **OK** , um das Dialogfeld **Konfiguration ADO.NET Verbindungs-Manager** zurückzukehren.
5. Klicken Sie auf **OK** , um die **Ziel-Editor für ADO.NET**zurückzukehren im Dialogfeld **ADO.NET Verbindungs-Manager konfigurieren** .
6. Klicken Sie im **Ziel-Editor für ADO.NET** **neu** neben der **Tabelle oder Ansicht** Liste öffnen Sie das Dialogfeld **Create Table** erstellen Sie eine neue Zieltabelle mit einer Spaltenliste, der die Tabelle entspricht.

    ![][12a]

7. Klicken Sie im Dialogfeld **Create Table** tun.

    1. Ändern Sie den Namen der Zieltabelle **SalesOrderDetail**.
    2. Entfernen Sie die **Rowguid** -Spalte. **Uniqueidentifier** -Datentyp wird im SQL Data Warehouse nicht unterstützt.
    3. Ändern Sie den Datentyp der Spalte **"LineTotal"** in **Money**. Der **decimal** -Datentyp wird im SQL Data Warehouse nicht unterstützt. Informationen zu unterstützten Datentypen finden Sie unter [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][].
    
        ![][12b]
    
    4. Klicken Sie auf **OK** , um die Tabelle erstellen und in **ADO.NET Ziel-Editor**zurückzukehren.

8. **Ziel-Editor für ADO.NET**Registerkarte **Zuordnung** zu sehen, wie die Spalten in der Datenquelle Spalten im Ziel zugeordnet werden.

    ![][13]

9. Klicken Sie auf **OK** , um die Konfiguration der Datenquelle abzuschließen.

## <a name="step-6-run-the-package-to-load-the-data"></a>Schritt 6: Ausführung des Pakets zum Laden der Daten

Führen Sie das Paket auf der Symbolleiste auf die Schaltfläche **Start** oder durch Auswahl einer der Optionen **Führen Sie** im Menü **Debuggen** .

Das Paket beginnt, gelbe sich drehenden Räder an Aktivität sowie die Anzahl der bisher verarbeiteten Zeilen angezeigt.

![][14]

Wenn das Paket ausgeführt wurde, sehen Sie grünes Häkchen an Erfolg sowie die Gesamtzahl der Zeilen mit Daten aus der Quelle zum Ziel.

![][15]

Herzlichen Glückwunsch! Sie haben erfolgreich SQL Server Integration Services zum Laden von Daten in Azure SQL Data Warehouse verwendet.

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über den SSIS-Datenfluss. Hier beginnen: [Datenfluss][].
- Informationen Sie zum Debuggen und Problembehandlung Pakete rechts in der Design-Umgebung. Hier beginnen: [Problembehandlungstool für Package Development][].
- Erfahren Sie, wie Ihre SSIS-Projekte und Pakete mit Integration Services-Server oder an einen anderen Speicherort bereitstellen. Hier beginnen: [Bereitstellung von Projekten und Paketen][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase-Handbuch]: https://msdn.microsoft.com/library/mt143171.aspx
[Herunterladen von SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[Erstellen Sie (Azure SQL Datawarehouse, Parallel Datawarehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Datenfluss]: https://msdn.microsoft.com/library/ms140080.aspx
[Problembehandlung für Paketentwicklung]: https://msdn.microsoft.com/library/ms137625.aspx
[Bereitstellung von Projekten und Paketen]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integrationsservices Feature Pack für Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Produkttests]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio-Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks-Beispieldatenbanken 2014]: https://msftdbprodsamples.codeplex.com/releases/view/125550
