<properties 
    pageTitle="Erste Schritte mit elastische Datenbanktools" 
    description="Grundlegende Erklärung elastische Datenbank Tools Feature von Azure SQL-Datenbank einschließlich einfache Beispiel-app ausgeführt." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor="CarlRabeler"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="get-started-with-elastic-database-tools"></a>Erste Schritte mit elastische Datenbanktools

Dieses Dokument führt Sie in den Entwicklungsprozess mit Beispiel-app. Das Beispiel erstellt eine einfache Anwendung mit Sharding und Schlüsselfunktionen elastische Datenbanktools untersucht. Das demonstriert die Funktionen der [Clientbibliothek elastische Datenbank](sql-database-elastic-database-client-library.md)

Um die Bibliothek zu installieren, gehen Sie zu [Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). Beachten Sie, dass die Bibliothek beschriebenen Beispiel-app installiert ist.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. Visual Studio 2012 oder höher mit C# ist erforderlich. Herunterladen einer kostenlosen Version unter [Visual Studio-Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs.aspx).
2. NuGet 2.7 oder höher. Die neueste Version finden Sie unter [Installieren von NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)

## <a name="download-and-run-the-sample-app"></a>Herunterladen und Ausführen der Beispiel-app

Die **elastische Datenbank mit SQL Azure – erste Schritte** Beispiel veranschaulicht die wichtigsten Aspekte der Entwicklung Sharding Anwendungsmöglichkeiten mit SQL Azure elastische Database Tools. Es konzentriert sich auf wichtige Einsatzbeispiele für [Splitter Management](sql-database-elastic-scale-shard-map-management.md), [datenabhängiges routing](sql-database-elastic-scale-data-dependent-routing.md) und [Multi-Splitter Abfragen](sql-database-elastic-scale-multishard-querying.md). Herunterladen und Ausführen des Beispiels gehen Sie folgendermaßen vor: 

1. Öffnen Sie Visual Studio, und wählen Sie **Datei-Neu -> Projekt**.
2. Klicken Sie im Dialogfeld auf **Online**.

    ![Neues Projekt > Online][2]
3. Klicken Sie auf **Visual C#** unter **Beispiele**.

    ![Klicken Sie auf Visual C#][3]
4. Geben Sie im Suchfeld **elastische Db** um das Beispiel zu suchen. Der Titel wird **Elastische DB Tools für SQL Azure - Getting Started** .

    ![Suchfeld][1]
 
5. Die Stichprobe, wählen Sie einen Namen und einen Speicherort für das neue Projekt, und drücken Sie **OK** , um das Projekt zu erstellen.
6. Öffnen Sie die Datei **app.config** in der Projektmappe für das Beispielprojekt, und gehen Sie in der Datei der SQL Azure-Datenbank-Servername und die Anmeldeinformationen (Benutzername und Kennwort) hinzu.
7. Erstellen Sie und führen Sie die Anwendung. Gefragt, kann Visual Studio die NuGet-Pakete der Lösung wiederherstellen. Dadurch wird die Clientbibliothek elastische Datenbank von NuGet herunterladen.
8. Spielen Sie mit unterschiedlichen Optionen Weitere Informationen zu Client-Library-Funktionen. Beachten Sie die Schritte findet die Anwendung in der Konsole ausgegeben und den Code hinter den Kulissen Erkunden Sie.

    ![Fortschritt][4]

Herzlichen Glückwunsch – erfolgreich erstellt und die erste Sharding Anwendung elastische Datenbanktools Azure SQL-Datenbank ausgeführt. Werfen Sie einen Blick auf die Splitter das Beispiel mit Visual Studio oder SQL Server Management Studio auf den Azure-DB-Server erstellt. Beachten Sie neue Beispieldatenbanken Splitter und ein Splitter Karte Manager-Datenbank, die das Beispiel erstellt wurde.

> [AZURE.IMPORTANT] Es wird empfohlen, immer die neueste Version von Management Studio verwenden, Updates Microsoft Azure und SQL Datenbank synchronisiert bleiben. [Aktualisieren Sie SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).


### <a name="key-pieces-of-the-code-sample"></a>Wichtige Teile des Codebeispiels

1. **Verwalten von Splitter und Splitter Karten**: der Code veranschaulicht die Arbeit mit, Bereiche, und die Zuordnung in der Datei **ShardMapManagerSample.cs**. Finden Sie weitere Informationen zu diesem Thema: [Splitter Karte Management](http://go.microsoft.com/?linkid=9862595).  
2. **Datenabhängiges Routing**: Routing auf rechten Splitter im **DataDependentRoutingSample.cs**angezeigt wird. Weitere Informationen finden Sie unter [Datenabhängiges Routing](http://go.microsoft.com/?linkid=9862596). 
3. **Abfragen über mehrere Splitter**: Abfragen über Splitter in der Datei **MultiShardQuerySample.cs**dargestellt. Weitere Informationen finden Sie unter [Abfragen mit mehreren Splitter](http://go.microsoft.com/?linkid=9862597).
4. **Leere Splitter hinzufügen**: iterative hinzufügen neue leere Splitter erfolgt durch den Code in der Datei **AddNewShardsSample.cs**. Hier werden Details zu diesem Thema behandelt: [Splitter Karte Management](http://go.microsoft.com/?linkid=9862595).

### <a name="other-elastic-scale-operations"></a>Andere Vorgänge elastische skalieren

1. **Aufteilen einer vorhandenen Splitter**: die Möglichkeit, Splitter teilen **Teilen Zusammenführungstool**bereitgestellt. Weitere Informationen zu diesem Tool: [Split Zusammenführungstool Overview](sql-database-elastic-scale-overview-split-and-merge.md).
2. **Zusammenführen von vorhandenen Splitter**: Splitter führt werden auch mit **Split - Dienstprogramm**ausgeführt. Weitere Informationen finden Sie unter: [Split Zusammenführungstool Overview](sql-database-elastic-scale-overview-split-and-merge.md).   


## <a name="cost"></a>Kosten

Elastische Datenbanktools sind kostenlos. Elastische Datenbanktools erlegt zusätzliche Kosten auf die Kosten für Ihre Azure-Nutzung. 

Beispielsweise erstellt die beispielanwendung neue Datenbanken. Die Kosten hängt von Azure SQL DB Datenbankedition gewählte und Azure Verwendung der Anwendung ab.

Preisinformationen finden Sie unter [SQL Datenbank Preisdetails](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über die elastische Datenbanktools finden Sie unter:

* [Wegweiser für die Dokumentation von elastische Datenbank-tools](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/) 
-    Codebeispiele: 
    -    [Elastische DB mit SQL Azure - erste Schritte](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-a80d8dc6?SRC=VSIDE)
    -    [Elastische DB mit SQL Azure - Integration mit Entity Framework](http://code.msdn.microsoft.com/Elastic-Scale-with-Azure-bae904ba?SRC=VSIDE)
    -    [Splitter Elastizität Script Center](https://gallery.technet.microsoft.com/scriptcenter/Elastic-Scale-Shard-c9530cbe)
-    Blog: [elastische Skalierung Ankündigung](https://azure.microsoft.com/blog/2014/10/02/introducing-elastic-scale-preview-for-azure-sql-database/)
-    Channel 9: [Übersicht Übersichtsvideo elastische skalieren](http://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Database-Elastic-Scale)
-    Diskussionsforum: [Azure SQL-Datenbank-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted)
-    Messen der Leistung: [Leistungsindikatoren für shardzuordnungs-Manager](sql-database-elastic-database-client-library.md)


<!--Anchors-->
[The Elastic Scale Sample Application]: #The-Elastic-Scale-Sample-Application
[Download and Run the Sample App]: #Download-and-Run-the-Sample-App
[Cost]: #Cost
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-get-started/newProject.png
[2]: ./media/sql-database-elastic-scale-get-started/click-online.png
[3]: ./media/sql-database-elastic-scale-get-started/click-CSharp.png
[4]: ./media/sql-database-elastic-scale-get-started/output2.png
 
