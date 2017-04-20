<properties
    pageTitle="Importieren von Daten in Machine Learning Studio von online-Datenquellen | Microsoft Azure"
    description="Wie die Trainingsdaten Azure Machine Learning Studio aus verschiedenen Online-Quellen importieren."
    keywords="Importieren von Daten, Datenformat, Datentypen, Datenquellen, Daten"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;garye" />


# <a name="import-data-into-azure-machine-learning-studio-from-various-online-data-sources-with-the-import-data-module"></a>Importieren von Daten in Azure Machine Learning Studio aus verschiedenen Online-Datenquellen mit den importierten Daten

Dieser Artikel beschreibt die Unterstützung für den Import von Daten aus verschiedenen Quellen und die Informationen zum Verschieben von Daten aus diesen Quellen in einem Experiment Azure maschinelles lernen.

> [AZURE.NOTE] Dieser Artikel enthält allgemeine Informationen über die [Importdaten] [ import-data] Modul. Ausführlichere Informationen über die Arten von Daten können Sie zugreifen, Formate, Parameter und Antworten auf häufige Fragen finden Sie im Modul Referenzthema für die [Importierten Daten] [ import-data] Modul.

<!-- -->

[AZURE.INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

## <a name="introduction"></a>Einführung

Sie können Daten in Azure Machine Learning Studio eines Online-Datenquellen zugreifen, laufendem Test mit den [Importierten Daten] [ import-data] Modul:

- Eine Web-URL mithilfe von HTTP
- Hadoop mit HiveQL
- Azure BLOB-Speicher
- Azure-Tabelle
- Azure SQL-Datenbank oder SQL Server auf Azure VM
- Lokale SQL Server-Datenbank
- Daten derzeit OData-Anbieter
 
Der Workflow für die Durchführung von Experimenten in Azure Machine Learning Studio besteht aus Komponenten auf die Leinwand ziehen und ablegen. Zugriff auf Online-Datenquellen hinzufügen die [Importdaten] [ import-data] Modul das Experiment **Datenquelle**auswählen und dann die Parameter für den Datenzugriff. Unterstützt Online-Datenquellen werden in der folgenden Tabelle aufgeführt. Diese Tabelle werden auch die Dateiformate unterstützt werden und Parameter für den Datenzugriff.

Beachten Sie, dass diese Daten zugegriffen werden, während der Test ausgeführt wird, nur für dieses Experiment. In einem Dataset Modul gespeicherten Daten stehen dagegen jedes Experiment im Arbeitsbereich.

> [AZURE.IMPORTANT] Derzeit die [Importdaten] [ import-data] und [Daten exportieren] [ export-data] Module können Daten lesen und Schreiben von Azure-Speicher mit dem klassischen Bereitstellungsmodell erstellt. Der neue Azure BLOB-Speicher Kontotyp, der einen hot Speicherebene Zugang oder coole Speicherebene Zugang bietet ist also noch nicht unterstützt. 

> Im Allgemeinen Azure-Speicherkonten erstellt haben möglicherweise vor dieser Service-Option wurde nicht betroffen sein sollen. Benötigen Sie ein neues Konto erstellen, wählen Sie **Classic** für das Bereitstellungsmodell verwenden Sie Ressourcenmanager oder **Allgemeine** als **BLOB-Speicher**für **Konto Art**wählen. 

> Weitere Informationen finden Sie unter [Azure BLOB-Speicher: warme und kalte Speicherebenen](../storage/storage-blob-storage-tiers.md).



## <a name="supported-online-data-sources"></a>Unterstützt online-Datenquellen
Azure Machine Learning **Importdaten** Modul unterstützt die folgenden Datenquellen:

Datenquelle | Beschreibung | Parameter |
---|---|---|
Web-URL über HTTP |Liest Daten in kommagetrennten Werten (CSV), Tab-getrennten (TSV) Attribut Beziehung Dateiformat (ARFF) und Vektor Maschinen (SVM-hell) Formate aus beliebigen Web-URL, die HTTP verwendet | <b>URL</b>: Gibt den vollständigen Namen der Datei, einschließlich Website-URL und Dateiname mit Erweiterung. <br/><br/><b>Datenformat</b>: Gibt ein unterstützte Formate: CSV, TSV, ARFF oder SVM-Licht. Wenn die Daten eine Kopfzeile, dient sie Spaltennamen zuordnen.|
Hadoop-bietet|Liest Daten aus dezentralen Hadoop. Sie geben die Daten mithilfe von HiveQL eine SQL-ähnliche Sprache. HiveQL kann auch verwendet werden, Daten und Daten filtern Machine Learning Studio Daten hinzu.|<b>Struktur der Datenbankabfrage</b>: Gibt die Hive-Abfrage verwendet, um die Daten zu generieren.<br/><br/><b>HCatalog Server-URI</b> : den Namen des Clusters im Format *angegeben &lt;der Clustername&gt;. azurehdinsight.net.*<br/><br/><b>Hadoop Benutzernamen</b>: Gibt den Hadoop Benutzerkontonamen Cluster bereitgestellt.<br/><br/><b>Hadoop Benutzerkennwort</b> : Gibt die Anmeldeinformationen beim Cluster bereitstellen. Weitere Informationen finden Sie [in HDInsight Cluster Hadoop erstellen](../hdinsight/hdinsight-provision-clusters.md).<br/><br/><b>Speicherort der Daten</b>: Gibt an, ob die Daten in einem Hadoop verteiltes Dateisystem (HDFS) oder in Azure gespeichert werden. <br/><ul>Speichern von Daten in bietet festzulegen Sie bietet Server URI. (Achten Sie darauf den Clusternamen HDInsight ohne das Präfix HTTPS:// verwenden). <br/><br/>Wenn Sie die Ausgabedaten in Azure speichern, müssen Sie Kontonamen Azure-Speicher, Zugriffstaste Speicher und Speicher Containernamen angeben.</ul>|
SQL-Datenbank |Liest Daten, die in einer Azure SQL-Datenbank oder in einer SQL Server-Datenbank auf eine Azure virtuellen Computer gespeichert ist. | <b>Servername</b>: Gibt den Namen des Servers, auf dem die Datenbank ausgeführt wird.<br/><ul>Geben Sie den Servernamen, der generiert wird, bei Azure SQL-Datenbank. Normalerweise ist * &lt;Generated_identifier&gt;. database.windows.net.* <br/><br/>Geben Sie bei einer gehosteten Azure Virtual Machine SQL Servers *Tcp:&lt;DNS-Name des virtuellen Computers&gt;, 1433*</ul><br/><b>Datenbankname </b>: Gibt den Namen der Datenbank auf dem Server. <br/><br/><b>Server-Benutzerkontoname</b>: Gibt einen Benutzernamen für ein Konto mit Berechtigungen für die Datenbank. <br/><br/><b>Server-Benutzerkennwort</b>: Gibt das Kennwort für das Benutzerkonto.<br/><br/><b>Akzeptieren Sie alle Serverzertifikat</b>: Diese Option (weniger sicher) Wenn Sie überspringen das Websitezertifikat überprüfen, bevor Sie Daten lesen möchten.<br/><br/><b>Abfrage</b>: Geben Sie eine SQL-Anweisung, die die Daten beschreiben möchten.|
Lokalen SQL-Datenbank |Liest Daten in einer lokalen SQL-Datenbank gespeichert. | <b>Daten-Gateway</b>: Gibt den Namen des Data Management Gateway auf einem Computer installiert, die SQL Server-Datenbank zugreifen. Informationen zum Einrichten von Gateway finden Sie unter [Ausführen erweiterter Analytics mit Azure Computer mithilfe von Daten aus einer lokalen SQL Server](machine-learning-use-data-from-an-on-premises-sql-server.md).<br/><br/><b>Servername</b>: Gibt den Namen des Servers, auf dem die Datenbank ausgeführt wird.<br/><br/><b>Datenbankname </b>: Gibt den Namen der Datenbank auf dem Server. <br/><br/><b>Server-Benutzerkontoname</b>: Gibt einen Benutzernamen für ein Konto mit Berechtigungen für die Datenbank. <br/><br/><b>Benutzername und Kennwort</b>: Klicken Sie auf <b>Werte geben</b> Ihre Anmeldeinformationen eingeben. Sie können integrierte Windows-Authentifizierung oder SQL Server-Authentifizierung, je nach Konfiguration der lokalen SQL Server.<br/><br/><b>Abfrage</b>: Geben Sie eine SQL-Anweisung, die die Daten beschreiben möchten.|
Azure-Tabelle|Liest Daten aus der Tabelle Service im Azure-Speicher.<br/><br/>Wenn Sie große Datenmengen selten lesen, nutzen Sie Azure Tabelle. Es bietet eine flexible nicht relationale (NoSQL), massiv skalierbaren, kostengünstigen und hochverfügbare Storage-Lösung.| Die Optionen in den **Importierten Daten** abhängig, ob Sie Zugriff auf sind öffentliche oder private Speicherkonto, die Anmeldeinformationen erfordert. Dieser <b>Authentifizierungstyp</b> der Wert "PublicOrSAS" oder "Konto", bestimmt jeweils einen eigenen Satz von Parametern verfügt. <br/><br/><b>Öffentlichen oder freigegebenen Zugriff Signatur (SAS) URI</b>: die Parameter sind:<br/><br/><ul><b>Tabelle URI</b>: Gibt den öffentlichen oder SAS-URL für die Tabelle.<br/><br/><b>Gibt die Zeilen nach Namen suchen</b>: Werte <i>Top n</i> auf die angegebene Anzahl von Zeilen Scannen oder <i>ScanAll</i> auf alle Zeilen in der Tabelle zu erhalten. <br/><br/>Wenn Daten homogenen und vorhersehbar ist, wird empfohlen wählen Sie *Top n* und n Geben Bei großen Tabellen kann dies schneller lesen Zeiten führen.<br/><br/>Wenn die Daten mit Eigenschaften, die variieren je nach Tiefe und Position der Tabelle aufgebaut ist, die Option *ScanAll* auf alle Zeilen durchsuchen. Dies gewährleistet die Integrität der resultierende-Eigenschaft und Metadaten konvertieren.<br/><br/></ul><b>Privater Speicher-Konto</b>: die Parameter sind: <br/><br/><ul><b>Kontoname</b>: Gibt den Namen des Kontos mit der Tabelle zu lesen.<br/><br/><b>Konto-Taste</b>: Gibt den Speicherschlüssel dem Konto zugeordnet.<br/><br/><b>Tabellenname</b> : Gibt den Namen der Tabelle, die die zu lesenden Daten enthält.<br/><br/><b>Zeilen nach Namen suchen</b>: Werte <i>Top n</i> auf die angegebene Anzahl von Zeilen Scannen oder <i>ScanAll</i> auf alle Zeilen in der Tabelle zu erhalten.<br/><br/>Daten homogenen und vorhersagbar ist, sollten Sie *Top n* auswählen, und geben Sie n Bei großen Tabellen kann dies schneller lesen Zeiten führen.<br/><br/>Wenn die Daten mit Eigenschaften, die variieren je nach Tiefe und Position der Tabelle aufgebaut ist, die Option *ScanAll* auf alle Zeilen durchsuchen. Dies gewährleistet die Integrität der resultierende-Eigenschaft und Metadaten konvertieren.<br/><br/>|
Azure BLOB-Speicher | Liest Daten im BLOB-Dienst in Azure-Speicher, z. B. Bilder, unstrukturierten Text oder Binärdaten.<br/><br/>Den BLOB-Dienst können Daten öffentlich verfügbar machen oder Anwendungsdaten Privat speichern. Sie können überall auf Ihre Daten zugreifen mithilfe von HTTP oder HTTPS-Verbindung. | Die Optionen im Datenmodul **Importieren** abhängig, ob Sie Zugriff auf sind öffentliche oder private Speicherkonto, die Anmeldeinformationen erfordert. Dieser <b>Authentifizierungstyp</b> bestimmt der Wert "PublicOrSAS" oder "Konto".<br/><br/><b>Öffentlichen oder freigegebenen Zugriff Signatur (SAS) URI</b>: die Parameter sind:<br/><br/><ul><b>URI</b>: Gibt den öffentlichen oder SAS-URL für Speicher-Blob.<br/><br/><b>Dateiformat</b>: Gibt das Format der Daten im Blob-Dienst. Die unterstützten Formate sind CSV, TSV und ARFF.<br/><br/></ul><b>Privater Speicher-Konto</b>: die Parameter sind: <br/><br/><ul><b>Kontoname</b>: Gibt den Namen des Kontos, das Blob enthält Sie lesen möchten.<br/><br/><b>Konto-Taste</b>: Gibt den Speicherschlüssel dem Konto zugeordnet.<br/><br/><b>Container, Verzeichnis oder BLOB-Pfad</b> : Gibt den Namen des BLOBs, die die zu lesenden Daten enthält.<br/><br/><b>BLOB-Dateiformat</b>: Gibt das Format der Daten im Blob-Dienst. Die unterstützten Datenformate werden CSV-TSV ARFF, CSV angegebene Codierung und Excel. <br/><br/><ul>Ist das Format CSV- oder TSV, müssen Sie angeben, ob die Datei eine Kopfzeile enthält.<br/><br/>Die Option Excel können zum Lesen von Daten aus Excel-Arbeitsmappen. Die Option <i>Excel Datenformat</i> anzugeben, ob die Daten in einem Excel-Arbeitsblatt-Bereich oder in einer Excel-Tabelle ist. Die Option <i>Excel-Tabelle oder eingebettete Tabelle </i>Geben Sie den Namen der das Blatt oder die Tabelle, die Sie lesen möchten.</ul><br/>|
Feed-Datenanbieter | Liest Daten aus einer unterstützten feed-Anbieter. Derzeit nur das Open Data Protocol (OData) wird unterstützt. | <b>Inhaltstyp der Daten</b>: Gibt das OData-Format.<br/><br/><b>Quell-URL</b>: Gibt die vollständige URL für die Daten. <br/>Beispielsweise liest die folgende URL aus der Northwind-Beispieldatenbank: http://services.odata.org/northwind/northwind.svc/|


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C/
