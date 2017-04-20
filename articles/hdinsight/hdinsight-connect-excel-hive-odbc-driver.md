<properties
   pageTitle="Schließen Sie Excel an Hadoop mit Struktur ODBC-Treiber | Microsoft Azure"
   description="Informationen Sie zum Einrichten und Verwenden des Microsoft-Struktur ODBC-Treibers für Excel zum Abfragen von Daten in einem Cluster HDInsight."
   services="hdinsight"
   documentationCenter=""
   authors="mumian"
   manager="jhubbard"
   tags="azure-portal"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

#<a name="connect-excel-to-hadoop-with-the-microsoft-hive-odbc-driver"></a>Verbinden Sie Excel Hadoop mit Microsoft Struktur ODBC-Treiber

[AZURE.INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

Große Microsoft Lösung integriert Microsoft Business Intelligence (BI) Komponenten mit Apache Hadoop, die von Azure HDInsight bereitgestellt. Ein Beispiel dieser Integration ist die Fähigkeit, Excel in das Datawarehouse Struktur eines Clusters Hadoop in HDInsight mit dem Treiber Microsoft Struktur Open Database Connectivity (ODBC).

Es ist möglich, einen HDInsight-Cluster und anderen Datenquellen, einschließlich andere (nicht HDInsight) Hadoop Cluster aus Excel das Microsoft Power Query-add-in für Excel mit zugeordneten Daten. Informationen zur Installation und Verwendung von Power Query finden Sie unter [Verbinden Excel HDInsight Power Abfrage][hdinsight-power-query].

> [AZURE.NOTE] Während der Schritte in diesem Artikel mit einem Linux- oder Windows-basierten HDInsight verwendet werden können, muss Windows Client-Arbeitsstation.

**Komponenten**:

Vor diesem Artikel benötigen Sie Folgendes:

- **Ein HDInsight-Cluster**. Um eine zu erstellen, finden Sie unter [Erste Schritte mit Azure HDInsight][hdinsight-get-started].
- **Eine Workstation** mit Office 2013 Professional Plus, Office 365 Pro Plus, Excel 2013 eigenständige oder Office 2010 Professional Plus.


##<a name="install-microsoft-hive-odbc-driver"></a>Microsoft Struktur ODBC-Treiber installieren

Downloaden und installieren Sie Microsoft Struktur ODBC-Treiber aus dem [Download Center][hive-odbc-driver-download].

Dieser Treiber kann auf 32-Bit- oder 64-Bit-Versionen von Windows 7, Windows 8, Windows 10, Windows Server 2008 R2 und Windows Server 2012 installiert und können Verbindung zu Azure HDInsight (Version 1.6 und höher) und Azure HDInsight Emulator (v.1.0.0.0 oder höher). Sie installieren die Version, die Version der Anwendung entspricht, in dem Sie den ODBC-Treiber verwenden. In diesem Lernprogramm wird der Treiber aus Office Excel verwendet werden.

##<a name="create-hive-odbc-data-source"></a>Struktur ODBC-Datenquelle erstellen

Die folgenden Schritte veranschaulichen die Struktur ODBC-Datenquelle erstellen.

1. Windows 8 oder Windows 10 Taste Windows Startbildschirm zu öffnen, und geben Sie **Datenquellen**.
2. Die Office-Version klicken Sie auf **ODBC-Datenquellen (32 Bit) eingerichtet** , oder **Richten Sie ODBC-Datenquellen (64 Bit)** . Wenn Sie Windows 7 verwenden, wählen Sie **Verwaltung** **ODBC-Datenquellen (32 Bit)** oder **ODBC-Datenquellen (64 Bit)** . Dadurch wird das Dialogfeld **ODBC-Datenquellenadministrator** .

    ![ODBC-Datenquellenadministrator][img-hdi-simbahiveodbc-datasource-admin]

3. Benutzer-DNS klicken Sie auf **Hinzufügen** , um den Assistenten **Neue Datenquelle erstellen** zu öffnen.
4. Wählen Sie **Microsoft Struktur ODBC-Treiber**und klicken Sie dann auf **Fertig stellen**. Das Dialogfeld **Microsoft Struktur ODBC-Treiber DNS-Setup** wird gestartet.

5. Geben Sie ein oder wählen Sie die folgenden Werte aus:

    Eigenschaft|Beschreibung
    ---|---
    Datenquelle|Geben Sie einen Namen zur Datenquelle
    Host|Geben Sie &lt;HDInsightClusterName >. azurehdinsight.net. Beispielsweise myHDICluster.azurehdinsight.net
    Anschluss|Verwenden Sie <strong>443</strong>. (Dieser Port wurde von 563 auf 443 geändert.)
    Datenbank|<strong>Standardmäßig</strong>verwenden.
    Struktur-Typ|Wählen Sie <strong>Struktur Server 2</strong>
    Mechanismus|Wählen Sie <strong>Azure HDInsight-Dienst</strong>
    HTTP-Pfad|Lassen Sie leer.
    Benutzername|HDInsight Cluster-Benutzernamen eingeben. Dies ist der Benutzername, der bei der Bestimmung der Cluster erstellt. Sie wird die Option schnell erstellen, der Standard-Benutzername <strong>Admin</strong>.
    Kennwort|HDInsight Cluster-Benutzerkennwort eingeben.
    </table>

    Es gibt einige wichtige Parameter berücksichtigen Wenn Sie **Erweiterte Optionen**klicken:

    Parameter|Beschreibung
    ---|---
    Ursprüngliche Abfrage verwenden|Wenn es aktiviert ist, versucht der ODBC-Treiber nicht HiveQL TSQL umzuwandeln. Sie werden nur verwenden, wenn Sie 100 % sicher Senden von reinen HiveQL Aussagen sind. Beim Verbinden mit SQL Server oder Azure SQL-Datenbank sollten Sie deaktiviert lassen.
    Pro Block abgerufenen Zeilen|Wenn eine große Menge von Datensätzen abrufen Optimierung dieser Parameter müssen eine optimale Leistung zu gewährleisten.
    Standardlänge Zeichenfolge Spalte binären Spaltenlänge, Decimal-Spalte skalieren|Der Datentyp, Länge und Präzision beeinträchtigen können, wie Daten zurückgegeben werden. Sie bewirkt, dass falsche Informationen zum Verlust der Genauigkeit oder Abschneiden zurückgegeben werden.


    ![Erweiterte Optionen][img-HiveOdbc-DataSource-AdvancedOptions]

6. Klicken Sie auf **Testen** , um die Datenquelle zu testen. Wenn die Datenquelle richtig konfiguriert ist, wird *TESTS erfolgreich abgeschlossen!*.
7. Klicken Sie auf **OK** , um das Dialogfeld zu schließen. Die neue Datenquelle sollte jetzt auf **ODBC-Datenquellen-Administrator**aufgeführt.
8. Klicken Sie auf **OK** , um den Assistenten zu beenden.

##<a name="import-data-into-excel-from-hdinsight"></a>Importieren von Daten in Excel HDInsight

Die folgenden Schritte beschreiben die Möglichkeit, Daten aus einer Tabelle Struktur in einer Excel-Arbeitsmappe, die die ODBC-Datenquelle, die in den Schritten erstellt.

1. Öffnen Sie eine neue oder vorhandene Arbeitsmappe in Excel.
2. Über die Registerkarte **Daten** **Aus anderen Datenquellen**auf und klicken Sie dann auf **Vom Datenverbindungs-Assistenten** , um den **Datenverbindungs-Assistenten**zu starten.

    ![Datenverbindungs-Assistenten öffnen][img-hdi-simbahiveodbc.excel.dataconnection]

3. Wählen Sie **ODBC DSN** als Datenquelle aus und dann auf **Weiter**.
4. ODBC-Datenquellen wählen Sie der Name der Datenquelle aus, die Sie im vorherigen Schritt erstellt haben und klicken Sie auf **Weiter**.
5. Geben Sie das Kennwort für den Cluster im Assistenten, und klicken Sie auf **Test** , um die Konfiguration erneut überprüfen, wenn erforderlich.
6. Klicken Sie auf **OK** , um das Dialogfeld zu schließen.
7. Klicken Sie auf **OK**. Warten Sie auf die **Datenbank und Tabelle auswählen** -Dialogfeld öffnen. Dies kann einige Sekunden dauern.
8. Wählen Sie die Tabelle, die Sie importieren möchten, und klicken Sie auf **Weiter**. *Hivesampletable* ist eine Beispieltabelle Struktur, die mit HDInsight-Cluster.  Sie können sie auswählen, wenn Sie erstellt haben. Weitere Informationen zum Ausführen Struktur Abfragen und Struktur Tabellen erstellen, [Mit Struktur mit HDInsight][hdinsight-use-hive].
8. Klicken Sie auf **Fertig stellen**.
9. Klicken Sie im Dialogfeld **Daten importieren** Sie ändern oder Angeben der Abfrage. Dazu klicken Sie auf **Eigenschaften**. Dies kann einige Sekunden dauern.
10. Klicken Sie auf die Registerkarte **Definition** und fügen Sie dann zur Struktur select-Anweisung im **Befehlstext** Textfeld **Beschränkung 200** . Die Änderung schränkt die zurückgegebenen Datensatz auf 200 festgelegt.

    ![Verbindungseigenschaften][img-hdi-simbahiveodbc-excel-connectionproperties]

11. Klicken Sie auf **OK** , um das Dialogfeld Eigenschaften zu schließen.
12. Klicken Sie auf **OK** , um das Dialogfeld **Daten importieren** zu schließen.  
13. Geben Sie das Kennwort erneut ein, und klicken Sie auf **OK**. Es dauert ein paar Sekunden, bevor Daten in Excel importiert werden.

##<a name="next-steps"></a>Nächste Schritte

In diesem Artikel haben Sie gelernt, wie Microsoft-Struktur ODBC-Treiber verwenden zum Abrufen von Daten vom HDInsight-Service in Excel. Ebenso können Sie Daten vom HDInsight-Service in SQL-Datenbank abrufen. Es kann auch zum Hochladen von Daten in einem HDInsight-Service. Weitere Informationen finden Sie unter:

- [Datenanalyse Flug Verzögerung mit HDInsight][hdinsight-analyze-flight-data]
- [Upload von Daten auf HDInsight][hdinsight-upload-data]
- [Verwenden Sie Sqoop mit HDInsight] [hdinsight-use-sqoop]


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md

[hive-odbc-driver-download]: http://go.microsoft.com/fwlink/?LinkID=286698

[img-hdi-simbahiveodbc-datasource-admin]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.DataSourceAdmin1.png
[img-HiveOdbc-DataSource-AdvancedOptions]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.HiveOdbc.DataSource.AdvancedOptions1.png
[img-hdi-simbahiveodbc-excel-connectionproperties]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveODBC.Excel.ConnectionProperties1.png
[img-hdi-simbahiveodbc.excel.dataconnection]: ./media/hdinsight-connect-excel-hive-ODBC-driver/HDI.SimbaHiveOdbc.Excel.DataConnection1.png
