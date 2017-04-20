<properties
    pageTitle="Versionshinweise für Hadoop Komponenten Azure HDInsight | Microsoft Azure"
    description="Neueste Versionsinformationen und Versionen Hadoop Komponenten für Azure HDInsight. Tipps und Informationen für Hadoop Apache Storm und HBase abrufen."
    services="hdinsight"
    documentationCenter=""
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="nitinme"/>


# <a name="release-notes-for-hadoop-components-on-azure-hdinsight"></a>Versionshinweise für Azure HDInsight Hadoop Komponenten

## <a name="notes-for-10262016-release-of-r-server-on-hdinsight"></a>Hinweise zur Version 10, 26/2016 R Server auf HDInsight

- R-Server auf HDInsight Cluster bereitgestellt wurde optimiert.
- R-Server im HDInsight steht als reguläre HDInsight "R Server" cluster-Typ und nicht als separate HDInsight Anwendung installiert. Kantenknoten und R Server-Binärdateien werden jetzt als Teil der Bereitstellung R Server Cluster bereitgestellt. Dies verbessert die Geschwindigkeit und Zuverlässigkeit der Bereitstellung. Preismodell für R Server entsprechend aktualisiert.
- R Server Cluster Typ Preis basiert auf Standard-Tier-Preis plus Aufschlag Preis R Server jetzt. Premium werden nun für Premium-Funktionen über unterschiedliche Arten und nicht für R Server Cluster verwendet. Diese Änderung wirkt sich nicht auf effektive Preise R Server geändert, sondern nur Darstellung die Zuschläge in der Rechnung. Alle vorhandenen R Servercluster weiterhin arbeiten und ARM Vorlagen funktionieren weiterhin bis auf veraltete Objekte. **Es wird empfohlen, die skriptgesteuerte Bereitstellung um neue ARM-Vorlage aktualisieren.**


## <a name="notes-for-08302016-release-of-r-server-on-hdinsight"></a>Notizen für 30/08/2016 Version von R-Server auf HDInsight

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen   |Ambari erstellen |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8268980    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8268980    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8269383    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Die vollständige Versionsnummern für HDInsight Windows-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1033.2559206   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1033.2559206    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1033.2559206    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1033.2559206    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1033.2559206    |2.3 |2.3.3.1-25    |

## <a name="notes-for-08172016-release-of-r-server-on-hdinsight"></a>Notizen für 17/08/2016 Version von R-Server auf HDInsight

- R Server 8.0.5 – vor allem eine Fehlerkorrektur-Version. Siehe [R Server Release Notes](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes) für Weitere Informationen. 
- AzureML-Paket auf dem edgeknoten – [Dieses Paket R](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html) kann R Modelle veröffentlicht und als Webdienst Azure ML verbraucht werden.  Siehe Abschnitt ["Modell durchsetzen"](hdinsight-hadoop-r-server-overview.md#operationalize-a-model) unsere Artikel ["Übersicht der R Server auf HDInsight"](hdinsight-hadoop-r-server-overview.md) .
- Linux Abhängigkeiten [Top 100 beliebteste R Pakete](https://github.com/metacran/cranlogs) – diese Linux paketabhängigkeiten jetzt installiert wurden.  
- Option Repo CRAN beim Hinzufügen von R-Datenknoten Pakete. Siehe Abschnitt ["Installieren R Pakete"](hdinsight-hadoop-r-server-get-started.md#install-r-packages) unsere ["Erste Schritte mit R Server HDInsight"](hdinsight-hadoop-r-server-get-started.md) Artikel.
- Verbessert die Zuverlässigkeit von R-Server bereitstellen, wenn Cluster erstellt werden.


## <a name="notes-for-08012016-release-of-hdinsight"></a>Notizen 01/08/2016 Version von HDInsight

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen   |Ambari erstellen |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.8028416    |2.2 |2.2.9.1-19  |2.2.1.12-4   |
|3.3 |3.3.1000.0.8028416    |2.3 |2.3.3.1-25  |2.2.1.12-4   |
|3.4 |3.4.1000.0.8053402    |2.4 |2.4.2.4-5   |2.2.1.12-4   |

Die vollständige Versionsnummern für HDInsight Windows-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.1005.2488842   |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.1005.2488842    |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.1005.2488842    |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.1005.2488842    |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.1005.2488842    |2.3 |2.3.3.1-25    |

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Clustertyp (z. B. Spark, Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| 3.4 HDInsight Cluster geändert | Der Standardwert für folgende Struktur Konfigurationen die Leistung geändert <ul><li>`hive.vectorized.execution.reduce.enabled=true`</li><li>`hive.tez.min.partition.factor=1f`</li><li>`hive.tez.max.partition.factor=3f`</li><li>`tez.shuffle-vertex-manager.min-src-fraction=0.9`</li><li>`tez.shuffle-vertex-manager.max-src-fraction=0.95`</li><li>`tez.runtime.shuffle.connect.timeout= 30000`</li></ul>| Dienst    | Alle| N/A|
| Folgende Updates sind in dieser Version enthalten. | STRUKTUR-13632 HIVE-12897,HIVE-12907,HIVE-12908,HIVE-12988,HIVE-13510,HIVE-13572,HIVE-13716,HIVE-13726,HIVE-12505,HIVE-13632,HIVE-13661,HIVE-13705,HIVE-13743,HIVE-13810,HIVE-13857,HIVE-13902,HIVE-13911,HIVE-13933| Dienst    | Alle| N/A

## <a name="notes-for-07142016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 14/07/2016

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen   |Ambari erstellen |
|----|----------------------|----|------------|-------------|
|3.2 |3.2.1000.0.7932505    |2.2 |2.2.9.1-11  |2.2.1.12-2   |
|3.3 |3.3.1000.0.7932505    |2.3 |2.3.3.1-18  |2.2.1.12-2   |
|3.4 |3.4.1000.0.7933003    |2.4 |2.4.2.0     |2.2.1.12-2   |

Die vollständige Versionsnummern für HDInsight Windows-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.989.2441725    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.989.2441725     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.989.2441725     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.989.2441725     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.989.2441725     |2.3 |2.3.3.1-21    |

## <a name="notes-for-07072016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 07/07/2016

Die vollständige Versionsnummern für HDInsight Linux-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen   |
|----|----------------------|----|------------|
|3.2 |3.2.1000.0.7864996    |2.2 |2.2.9.1-11  |
|3.3 |3.3.1000.0.7864996    |2.3 |2.3.3.1-18  |
|3.4 |3.4.1000.0.7861906    |2.4 |2.4.2.0     |

Die vollständige Versionsnummern für HDInsight Windows-basierten Cluster mit dieser Version bereitgestellt:

|HDI |HDI Cluster-version   |HDP |HDP erstellen     |
|----|----------------------|----|--------------|
|2.1 |2.1.10.977.2413853    |1.3 |1.3.12.0-01795|
|3.0 |3.0.6.977.2413853     |2.0 |2.0.13.0-2117 |
|3.1 |3.1.4.977.2413853     |2.1 |2.1.16.0-2374 |
|3.2 |3.2.7.977.2413853     |2.2 |2.2.9.1-11    |
|3.3 |3.3.0.977.2413853     |2.3 |2.3.3.1-21    |

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Clustertyp (z. B. Spark, Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| [HDInsight Tools für IntelliJ](hdinsight-apache-spark-intellij-tool-plugin.md) | IntelliJ IDEA-Plugin für HDInsight Spark-Cluster ist jetzt in Azure Toolkit für IntelliJ integriert. Unterstützt Azure SDK v2.9.1 neueste Java SDKs und enthält alle Features von eigenständigen HDInsight Plugin für IntelliJ.| Tools    | Funken| N/A|
| [HDInsight Tools für Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md) | Azure-Toolkit für Eclipse unterstützt nun HDInsight Spark-Cluster. Die folgenden Features aktiviert. <ul><li>Erstellen Sie und Schreiben Sie Spark-Anwendung problemlos uns und Java mit erstklassigen Unterstützung für IntelliSense Autom Fehlersuche usw. erstellen.</li><li>Testen Sie die Spark Anwendung lokal.</li><li>Aufträge mit HDInsight Spark-Cluster und Abrufen der Ergebnisse.</li><li>Azure melden Sie an und Zugriff auf alle zugeordneten Abonnements Azure Spark-Cluster.</li><li>Navigieren Sie alle zugeordneten Speicherressourcen des Clusters HDInsight Spark.</li></ul>| Tools    | Funken| N/A

Ab dieser Version haben wir die OS Patch für Gäste für Linux-basierte HDInsight Cluster geändert. Die neue Richtlinie soll reduziert die Anzahl der Neustarts durch Patches. Die neue Richtlinie wird Patch virtuelle Maschinen (VMs) in Linux-Clustern jeden Montag oder Donnerstag ab 12 Uhr UTC versetzten Weise über alle Knoten eines bestimmten Clusters. Bestimmten VM wird jedoch nur einmal alle 30 Tage durch Gast BS-Patches starten. Darüber hinaus wird der erste Neustart bei einem neu erstellten Cluster frühestens 30 Tage ab dem Datum Cluster nicht passieren.

>[AZURE.NOTE] Diese Änderung gilt nur auf neu erstellten Cluster gleich oder größer als diese Version.

## <a name="notes-for-06062016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 06/06/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

|HDP    |HDI-Version    |Spark-Version  |Ambari-Buildnummer    |HDP Buildnummer|
|-------|---------------|---------------|-----------------------|----------------|
|2.3    |3.3.1000.0.7702215|    1.5.2|  2.2.1.8-2|  2.3.3.1-18|
|2.4    |3.4.1000.0.7702224|    1.6.1|  2.2.1.8-2|  2.4.2.0|


Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Clustertyp (z. B. Spark, Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark auf HDInsight ist | Diese Version bringt verbesserte Verfügbarkeit, skalierbar und Produktivität Apache Spark auf HDInsight öffnen. <ul><li>Branchenführende Verfügbarkeit SLA 99,9 % für hohe Arbeitslasten von Unternehmen geeignet.</li><li>Skalierbare Speicherschicht Azure See Datenspeicher verwenden.</li><li>Produktivitätstools für jede Phase Daten und Entwicklung. Jupyter Notebooks mit benutzerdefinierten Spark-Kernel können interaktive datenexploration Integration BI Dashboards Power BI Tableaus und Qlik eignet sich für schnellen Datenaustausch und kontinuierliche Berichterstattung, IntelliJ Plugin zuverlässige Option für langfristige Artefakt Codeentwicklung und Debuggen.</li></ul>| Dienst    | Funken| N/A|
| HDInsight Tools für IntelliJ | Dies ist ein IntelliJ IDEA-Plugin für HDInsight Spark-Cluster. Die folgenden Features aktiviert.<ul><li>Erstellen Sie und Schreiben Sie Spark-Anwendung problemlos uns und Java mit erstklassigen Unterstützung für IntelliSense Autom Fehlersuche usw. erstellen.</li><li>Testen Sie die Spark Anwendung lokal.</li><li>Aufträge mit HDInsight Spark-Cluster und Abrufen der Ergebnisse.</li><li>Azure melden Sie an und Zugriff auf alle zugeordneten Abonnements Azure Spark-Cluster.</li><li>Navigieren Sie alle zugeordneten Speicherressourcen des Clusters HDInsight Spark.</li><li>Navigieren Sie alle Aufträge Geschichte und Auftragsinformationen für Ihren HDInsight Spark.</li><li>Debuggen Sie Spark Aufträge Remote vom Computer.</li></ul>| Tools    | Funken| N/A

## <a name="notes-for-05132016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 13/05/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.922.2266903 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.922.2266903 (HDP 2.2.9.1-11)
* HDInsight (Windows) 3.3.0.922.2266903 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.2.1000.0.7565644 (HDP 2.2.9.1-11)
* HDInsight (Linux) 3.3.1000.0.7565644 (HDP 2.3.3.1-18)
* HDInsight (Linux) 3.4.1000.0.7548380 (HDP 2.4.2.0)

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Clustertyp (z. B. Spark, Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Spark Version aktualisieren und andere Korrekturen | Diese Version 1.6.1 Spark-Version im HDInsight-Cluster aktualisiert und behebt andere Fehler| Dienst    | Funken| N/A

## <a name="notes-for-04112016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 11/04/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.889.2191206 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.889.2191206 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.889.2191206 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.889.2191206 (HDP 2.2.9.1-10)
* HDInsight (Windows) 3.3.0.889.2191206 (HDP 2.3.3.1-16-unverändert)
* HDInsight (Linux) 3.2.1000.0.7339916 (HDP 2.2.9.1-10)
* HDInsight (Linux) 3.3.1000.0.7339916 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.4.1000.0.7338911 (HDP 2.4.1.1-3)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Probleme beim Update des benutzerdefinierten Metastore für HDI 3.4 | Clustererstellung würde fehlschlagen, wenn Sie eine benutzerdefinierte Metastore verwendet die zuvor auf eine niedrigere Version von anderen HDInsight Cluster verwendet wurde. Dies wurde durch eine Aktualisierung Skriptfehler behoben wurde| Erstellung des Clusters    | Alle | N/A
| Livius Wiederherstellung | Bietet Job Status Stabilität für einen Auftrag über Livius gesendet | Zuverlässigkeit | Spark unter Linux| N/A
| Jupyter Inhalt HA | Ermöglicht das Speichern und Laden Jupyter Notebook Inhalt zu und von dem Cluster zugeordnete Speicherkonto. Weitere Informationen finden Sie unter [Kernels für Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-kernels.md).| Notebooks | Spark unter Linux| N/A
| Entfernen von HiveContext in Jupter | Mit `%%sql` anstelle von Magic `%%hive` Magic. Zur SqlContext entspricht HiveContext. Weitere Informationen finden Sie unter [Kernels für Jupyter notebooks](hdinsight-apache-spark-jupyter-notebook-kernels.md)| Notebooks    | Spark-Cluster unter Linux| N/A
| Veraltete Objekte Spark-Versionen | Ältere Version 1.3.1 Spark werden aus der Service auf 5-31 entfernt | Dienst | Spark-Cluster unter Windows | N/A

## <a name="notes-for-03292016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 29/03/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.875.2159884 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.875.2159884 (2.2.9.1-7 HDP - unverändert)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (2.2.9.1-8 HDP - unverändert)
* HDInsight (Linux) 3.3.1000.0.7193255 (2.3.3.1-7 HDP - unverändert)
* HDInsight (Linux) 3.4.1000.0.7195842 (HDP 2.4.1.0-327)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Hinzugefügte Version 3.4 HDInsight und aktualisierte HDP für alle HDInsight-Cluster | In dieser Version haben wir HDInsight v3. 4 (basierend auf HDP 2.4) und andere HDP Versionen wurden ebenfalls aktualisiert. HDP 2.4 Versionsinformationen sind verfügbar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html) und Weitere Informationen zum HDInsight Versionen gefunden [hier](hdinsight-component-versioning.md).| Dienst    | Linux-Clustern| N/A
| HDInsight Premium | HDInsight ist jetzt in zwei - Standard und Premium verfügbar. HDInsight Premium ist derzeit in der Vorschau und nur Hadoop und Spark Linux Cluster. Weitere Informationen finden Sie [hier](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).| Dienst    | Hadoop und Spark unter Linux| N/A
| Microsoft R Server | HDInsight Premium bietet Microsoft R Server unter Linux mit Hadoop und Spark berücksichtigt werden können. Weitere Informationen finden Sie in der [Übersicht von R Server auf HDInsight](hdinsight-hadoop-r-server-overview.md).| Dienst    | Hadoop und Spark unter Linux| N/A
| Spark 1.6.0 | 3.4 HDInsight Cluster enthalten jetzt Spark 1.6.0| Dienst    | Spark-Cluster unter Linux| N/A
| Jupyter Notebook enhancements | Jupyter Notebooks mit Spark Cluster bieten zusätzliche Spark Kernels. Dazu gehören auch verbesserte wie %% Magic, Auto-Visualisierung und Integration mit Python Visualisierung Bibliotheken (wie Matplotlib). Weitere Informationen finden Sie unter [Kernels für Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-kernels.md). | Dienst | Spark-Cluster unter Linux | N/A

## <a name="notes-for-03222016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 22/03/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.875.2159884 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.875.2159884 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.875.2159884 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.875.2159884 (2.2.9.1-7 HDP - unverändert)
* HDInsight (Windows) 3.3.0.875.2159884 (HDP 2.3.3.1-16)
* HDInsight (Linux) 3.2.1000.0.7193255 (2.2.9.1-8 HDP - unverändert)
* HDInsight (Linux) 3.3.1000.0.7193255 (2.3.3.1-7 HDP - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster | In dieser Version haben wir HDInsight Versionen für alle HDInsight-Cluster aktualisiert.| Dienst    | Alle| N/A


## <a name="notes-for-03102016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 10/03/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.859.2123216 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.859.2123216 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.859.2123216 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.859.2123216 (HDP 2.2.9.1-7)
* HDInsight (Windows) 3.3.0.859.2123216 (2.3.3.1-5 HDP - unverändert)
* HDInsight (Linux) 3.2.1000.7076817 (HDP 2.2.9.1-8)
* HDInsight (Linux) 3.3.1000.7076817 (HDP 2.3.3.1-7)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster | In dieser Version haben wir HDInsight Versionen für alle HDInsight-Cluster aktualisiert.| Dienst    | Alle| N/A

## <a name="notes-for-01272016-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 27/01/2016

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.817.2028315 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.817.2028315 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.817.2028315 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.817.2028315 (HDP 2.2.9.1-1)
* HDInsight (Windows) 3.3.0.817.2028315 (2.3.3.1-5 HDP - unverändert)
* HDInsight (Linux) 3.2.1000.4072335 (HDP 2.2.9.1-1)
* HDInsight (Linux) 3.3.1000.4072335 (HDP 2.3.3.1-1)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster | In dieser Version haben wir HDInsight Versionen für alle HDInsight-Cluster aktualisiert.| Dienst    | Alle| N/A

## <a name="notes-for-12022015-release-of-hdinsight"></a>Hinweise zur Version HDInsight 12/02/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.763.1931434 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.763.1931434 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.763.1931434 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.763.1931434 (2.2.7.1-34 HDP - unverändert)
* HDInsight (Windows) 3.3.1000.0 (HDP 2.3.3.1-5)
* HDInsight (Linux) 3.2.1000.0.6392801 (2.2.7.1-34 HDP - unverändert)
* HDInsight (Linux) 3.3.1000.0 (HDP 2.3.3.0-3039)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Hinzugefügte Version 3.3 HDInsight und aktualisierte HDP für alle HDInsight-Cluster | In dieser Version haben wir HDInsight v3. 3 (basierend auf HDP 2.3) und andere HDP Versionen wurden ebenfalls aktualisiert. HDP 2.3 Versionsinformationen sind verfügbar [hier](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html) und Weitere Informationen zum HDInsight Versionen gefunden [hier](hdinsight-component-versioning.md).| Dienst    | Alle| N/A

## <a name="notes-for-11302015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 30/11/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.757.1923908 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.757.1923908 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.757.1923908 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.757.1923908 (HDP 2.2.7.1-34)
* HDInsight (Linux) 3.2.1000.0.6392801 (HDP 2.2.7.1-34)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster und HDP für HDInsight 3.2 Cluster (Windows und Linux) | In dieser Version wurden HDInsight und HDP Versionen aktualisiert | Dienst    | Alle| N/A


## <a name="notes-for-10272015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 10/27/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight (Windows) 2.1.10.726.1866228 (1.3.12.0-01795 HDP - unverändert)
* HDInsight (Windows) 3.0.6.726.1866228 (2.0.13.0-2117 HDP - unverändert)
* HDInsight (Windows) 3.1.4.726.1866228 (2.1.15.0-2374 HDP - unverändert)
* HDInsight (Windows) 3.2.7.726.1866228 (HDP 2.2.7.1-33)
* HDInsight (Linux) 3.2.1000.0.6035701 (HDP 2.2.7.1-33)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster (Windows und Linux) | In dieser Version wurden HDInsight und HDP Versionen aktualisiert | Dienst    | Alle| N/A
| Feste Jupyter für Windows Spark-Cluster mit Großbuchstaben | Cluster, die DNS-in Großbuchstaben angegeben Namen war Jupyter Notebooks aufgrund einer Prüfung der Ursprungs-Anforderung. Das Update wurde der DNS-Name für die Konfiguration des Jupyter in Kleinbuchstaben ändern. | Dienst    | HDInsight Spark (Windows)| N/A


## <a name="notes-for-10202015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 10/20/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.716.1846990 (Windows) (HDP 1.3.12.0-01795 - unverändert)
* HDInsight 3.0.6.716.1846990 (Windows) (HDP 2.0.13.0-2117 - unverändert)
* HDInsight 3.1.4.716.1846990 (Windows) (HDP 2.1.16.0-2374)
* HDInsight 3.2.7.716.1846990 (Windows) (HDP 2.2.7.1-0004)
* HDInsight 3.2.1000.0.5930166 (Linux) (HDP 2.2.7.1-0004)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP Standardversion HDP 2.2 geändert | HDP 2.2 ist die Standardversion für HDInsight Windows-Clustern geändert. HDInsight Version 3.2 (HDP 2.2) ist seit Februar 2015 allgemein verfügbar. Diese Änderung spiegelt nur die Standardversion Cluster bei explizite Auswahl nicht während der Bereitstellung des Clusters mithilfe der Azure-Portal, PowerShell-Cmdlets und das SDK. | Dienst    | Alle| N/A                  |
|Ändert im Format VM für die Bereitstellung von mehreren HDInsight in Linux-Clustern in einem virtuellen Netzwerk | Unterstützung für mehrere HDInsight Linux-Cluster in einem virtuellen Netzwerk bereitstellen wird in dieser Version hinzugefügt. Im Rahmen dieser änderte sich das Format der virtuellen Namen des Clusters von Hauptknoten\*, Workernode\* und Zookeepernode\* , hn\*, unten\*, und Zk\* bzw.. Es ist nicht empfohlen, das Format der virtuellen Namen eine direkte Abhängigkeit übernehmen, da dies ändern. Verwenden Sie "Hostname -f" auf dem lokalen Computer oder Ambari APIs, die Liste der Hosts und die Zuordnung von Komponenten zu Hosts ermitteln. Finden Sie weitere Informationen unter [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/hosts.md) und [https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/host-components.md). | Dienst | HDInsight-Cluster unter Linux | N/A |
| Ändern der Konfiguration | 3.1 HDInsight Cluster sind jetzt folgendermaßen aktiviert: <ul><li>tez.yarn.ATS.Enabled und yarn.log.server.url. Dadurch Zeitachse Anwendungsserver und Server Protokoll Protokolle dienen können.</li></ul>Die folgenden Konfigurationen wurden für HDInsight 3.2 Cluster geändert: <ul><li>MapReduce.fileoutputcommitter.Algorithm.Version wurde auf 2 festgelegt. Dies ermöglicht die V2-Version der FileOutputCommitter.</li></ul> | Dienst | Alle | N/A |


## <a name="notes-for-09092015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 09/09/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.675.1768697 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.675.1768697 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.4.675.1768697 (2.1.15.0-2334 HDP - unverändert)
* HDInsight 3.2.6.675.1768697 (2.2.6.1-0012 HDP - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster | In dieser Version wurden HDInsight Versionen aktualisiert | Dienst    | Alle| N/A                  |

## <a name="notes-for-07312015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 07/31/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.640.1695824 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.640.1695824 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.4.640.1695824 (2.1.15.0-2334 HDP - unverändert)
* HDInsight 3.2.6.640.1695824 (2.2.6.1-0012 HDP - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| Beheben Sie Spark Cluster Knoten erneut imaging workflow | Fehler, die Spark Clusterknoten nicht wiederherstellen nach image verursacht wurde | Dienst    | Funken| N/A                  |


## <a name="notes-for-07312015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 07/31/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.635.1684502 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.635.1684502 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.4.635.1684502 (2.1.15.0-2334 HDP - unverändert)
* HDInsight 3.2.6.635.1684502 (2.2.6.1-0012 HDP - unverändert)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDInsight Updates für alle HDInsight-Cluster | In dieser Version wurden HDInsight Versionen aktualisiert | Dienst    | Alle| N/A                  |


## <a name="notes-for-07072015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 07/07/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.610.1630216 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.610.1630216 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.4.610.1630216 (2.1.15.0-2334 HDP - unverändert)
* HDInsight 3.2.4.610.1630216 (HDP 2.2.6.1-0012)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

| Titel                                           | Beschreibung                                          | Betroffener Bereich (z. B. Service, Komponente oder SDK) | Cluster-Typ (z. B. Hadoop, HBase oder Sturm) | JIRA (falls zutreffend) |
|-------------------------------------------------|------------------------------------------------------|---------------------------------------------------------|-----------------------------------------------------|----------------------|
| HDP Updates für HDInsight 3.2 Cluster | In dieser Version stellt HDInsight 3.2 HDP 2.2.6.1-0012 | Dienst    | Alle                                                 | N/A                  |


## <a name="notes-for-06262015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 26/06/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.601.1610731 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.601.1610731 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.4.601.1610731 (2.1.15.0-2334 HDP - unverändert)
* HDInsight 3.2.4.601.1610731 (HDP 2.2.6.1-0011)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>HDP Updates für HDInsight 3.2 Cluster</td>
<td>In dieser Version stellt HDInsight 3.2 HDP 2.2.6.1</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06182015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 18/06/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.596.1601657 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.596.1601657 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.4.596.1601657 (HDP 2.1.15.0-2334)
* HDInsight 3.2.4.596.1601657 (HDP 2.2.6.1-0002)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Zusätzliche HTTPS-Ports geöffnet</td>
<td>Cloud-Dienst jetzt 5 Ports 8001 8005 im Cluster Z.B. geöffnet. Bei https://<clustername>.azurehdinsight.net:8001/. Anträge auf diese URLs werden mit denselben grundlegenden Kennwort Authentifizierungsmechanismus als Port 443 authentifiziert. Diese Ports an denselben Port auf dem aktiven Hauptknoten gebunden. Skript-Aktionen dienen zu Kundendienst diese Ports auf dem Hauptknoten und Route außerhalb des Clusters überwachen.</td>
<td>Cloud-Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Zeitweise MapReduce Shuffle Problem HDInsight 3.2</td>
<td>Korrektur für einen seltenen, zeitweise Racebedingung in Map reduzieren Shuffle in großen Clustern was gelegentliche Taskfehler. Weitere Informationen finden Sie unter <a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a> .</td>
<td>Hadoop Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/MAPREDUCE-6361" target="_blank">MAPREDUCE-6361</a></td>
</tr>

<tr>
<td>Verschieben auf neueste Azure Java SDK 2.2 HDInsight 3.2</td>
<td>Neueste Version von Azure SDK verschoben für Java WASB Treiber verwendet. Das neueste SDK einige Updates und Versionshinweise für dieselbe am https://github.com/Azure/azure-storage-java/blob/master/ChangeLog.txt.</td>
<td>Hadoop Core</td>
<td>Alle</td>
<td><a href="https://issues.apache.org/jira/browse/HADOOP-11959" target="_blank">HADOOP 11959</a></td>
</tr>

<tr>
<td>HDP 2.1.15 für HDInsight 3.1 Cluster verschieben</td>
<td>Hortonworks Versionshinweise für die Veröffentlichung stehen <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.15-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.15.html" target="_blank">hier</a>.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06042015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 06/04/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.583.1575584 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.583.1575584 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.3.583.1575584 (2.1.12.1-0003 HDP - unverändert)
* HDInsight 3.2.4.583.1575584 (HDP 2.2.6.1-1)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Bei dem Fehler 502 Fehlerhaftes Gateway für Storm-Cluster</td>
<td>Diese Version behebt einen Fehler auf der Bewerbung API, die Website, nach einem Neustart verursacht.</td>
<td>Dienst</td>
<td>Sturm</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-06012015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 06/01/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.577.1563827 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.577.1563827 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.3.577.1563827 (2.1.12.1-0003 HDP - unverändert))
* HDInsight 3.2.4.577.1563827 (2.2.6.0-2800 HDP - unverändert)
* SDK 1.5.8


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Verschiedene Fehlerbehebungen</td>
<td>Diese Version behebt Fehler, Cluster bereitstellen.</td>
<td>Dienst</td>
<td>Alle cluster</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-05272015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 27/05/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 3.2.4.570.1554102 (HDP 2.2.6.0-2800)
* Andere Versionen von Cluster und SDK werden nicht als Teil dieser Version bereitgestellt.


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>HDP 2.2 aktualisieren</td>
<td>Diese Version von HDInsight 3.2 enthält HDP 2.2.6 und HDInsight bringt mehrere wichtige Updates. Notes-Vollversion steht unter <a href="http://dev.hortonworks.com.s3.amazonaws.com/HDPDocuments/HDP2/HDP-2.2.6/HDP_RelNotes_v226/index.html">HDP 2.2.6 Versionshinweise</a>.</td>
<td>HDP</td>
<td>Alle cluster</td>
<td>N/A</td>
</tr>

<tr>
<td>Standardmäßig Garns Container Speicherkonfiguration ändern</td>
<td>In diesem Update werden der standardmäßig verfügbaren Speicher aus Containern mb yarn.nodemanager.resource.memory und yarn.scheduler.maximum-Zuordnung-mb von Node Manager gestartet 5632 MB erhöht. Zuvor wurde 4608 MB verringert dies basierend auf verschiedenen ausgeführt, der neue Wert muss bieten bessere Zuverlässigkeit und Leistung für die meisten Projekte ist daher eine bessere Standard. Wie üblich, wenn Sie eine kritische Abhängigkeit auf diese Speicherkonfiguration, geben Sie beim Erstellen des Clusters explizit festgelegt.</td>
<td>HDP</td>
<td>Alle cluster</td>
<td>N/A</td>
</tr>

<tr>
<td>Default Config Parität für HBase und Storm-Cluster</td>
<td>Dieses Update stellt Hbase und Sturm Cluster dieselben Werte aus Konfigurationen Hadoop Cluster verwenden. Dies erfolgt für die Parität über alle Cluster.</td>
<td>HDP</td>
<td>HBase, Sturm</td>
<td>N/A</td>
</tr>

</table>

## <a name="notes-for-05202015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 20/05/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.564.1542093 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.564.1542093 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.3.564.1542093 (HDP 2.1.12.1-0003)
* HDInsight 3.2.4.564.1542093 (HDP 2.2.4.6-2)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>SCP.NET-EventHub-Unterstützung</td>
<td>Die aktualisierte Cluster-Pakete für HDInsight Sturm bringen SCP.NET neue Funktionen. Jetzt haben Zugriff auf neue APIs im Topologie-Generator, der EventHubSpout oder Java Tüllen vereinfachen. Ihr SCP.NET Client SDK mit neuen erwartungsgemäß aktualisiert die Verträge müssen aktualisiert werden. Details auf die neue APIs, Verwendung und Release Notes (einschließlich Updates) finden Sie die Readme-Datei im SCP.NET Nuget-Paket enthalten.</td>
<td>VS-Werkzeuge</td>
<td>Storm-HDInsight 3.2-Cluster</td>
<td>N/A</td>
</tr>

<tr>
<td>JDBC-Treiber-update</td>
<td>Aktualisieren Sie den Treiber der Version SQL Server unterstützt sqljdbc_4.1.5605.100.</td>
<td>Metastore</td>
<td>Alle</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04272015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 27/04/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.537.1486660 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.537.1486660 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.3.537.1486660 (2.1.12.0-2329 HDP - unverändert)
* HDInsight 3.2.3.537.1486660 (HDP 2.2.2.2-4)
* SDK 1.5.8

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Beheben von DLL-Abhängigkeit</td>
<td>HDInsight Komponententestframeworks Abhängigkeit entfernt.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Fehlerkorrektur für Race-Bedingung</td>
<td>Erstellen eines Clusters Anforderung jetzt wartet auf PUT-Anforderung vor dem Abrufen der Status angenommen</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>
</table>

## <a name="notes-for-04142015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 14/04/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.521.1453250 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.521.1453250 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.3.521.1453250 (2.1.12.0-2329 HDP - unverändert)
* HDInsight 3.2.3.525.1459730 (HDP 2.2.2.2-2)
* SDK 1.5.6

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Tez Updates</td>
<td>Updates für Apache TEZ 2214 und TEZ 1923 sind in dieser Version 3.2-HDI enthalten. Diese werden speziell für bestimmte Abfragen Struktur Tez mischen großer Datenmengen erfordern.
</td>
<td>HDP</td>
<td>Hadoop</td>
<td><a href="https://issues.apache.org/jira/browse/TEZ-2214">TEZ 2214</a></br><a href="https://issues.apache.org/jira/browse/TEZ-1923">TEZ 1923</a></td>
</tr>
</table>

## <a name="notes-for-04062015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 06/04/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.521.1453250 (1.3.12.0-01795 HDP - unverändert)
* HDInsight 3.0.6.521.1453250 (2.0.13.0-2117 HDP - unverändert)
* HDInsight 3.1.3.521.1453250 (2.1.12.0-2329 HDP - unverändert)
* HDInsight 3.2.3.521.1453250 (HDP 2.2.2.2-1)
* SDK 1.5.6

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>HDInsight .NET SDK 1.5.6</td>
<td>Updates für einige internen Klassen für HDInsight unter Linux entfernen.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Avro-Bibliothek 1.5.6</td>
<td><b>KnownTypeAttribute</b> Methode <b>GetAllKnownTypes</b>hinzugefügt. Feste NullReferenceException, wenn eine GetAllKnownTypes Methode null ist.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Fehlerkorrekturen</td>
<td>Verschiedene Fehlerbehebungen für den Dienst</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-04012015-release-of-hdinsight"></a>Notizen 01/04/2015 Version von HDInsight

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.513.1431705 (HDP 1.3.12.0-01795)
* HDInsight 3.0.6.513.1431705 (HDP 2.0.13.0-2117)
* HDInsight 3.1.3.513.1431705 (HDP 2.1.12.0-2329)
* HDInsight 3.2.3.513.1431705 (HDP 2.2.2.1-2600)
* SDK 1.5.5

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Möglichkeit zum remote desktop Anmeldeinformationen für Windows Cluster über .NET SDK aktivieren</td>
<td>Programmgesteuerte Unterstützung aktivieren oder Deaktivieren von RDP Anmeldeinformationen für Windows Cluster.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Ermöglicht remote desktop Anmeldeinformationen auf Cluster während sie bereitgestellt werden</td>
<td>Programmgesteuerte Unterstützung für remote desktop Anmeldeinformationen Aktivieren der Cluster erstellt wird. Dies entfernt Schritten zunächst Cluster bereitstellen und Aktivieren des Remotedesktops.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Aktualisierte Python 2.7.8</td>
<td>Aktualisierte Python HDInsight Cluster Python 2.7.8, einige wichtige Security enthält Updates für HDInsight Versionen 2.1, 3.0, 3.1 und 3.2</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>GARN-Konfiguration ändern</td>
<td>Geänderte GARN Konfiguration yarn.resourcemanager.max abgeschlossen-Programme für alle Cluster HDInsight Versionen 3.1 und 3.2 1000. Dieser Wert steuert nur die Liste der Bewerbungen in der Benutzeroberfläche aus. Um Informationen zu erhalten, die vor der Liste der Programme auf der Benutzeroberfläche angezeigten übermittelt wurden, gelangen Sie direkt zum Server Geschichte.</td>
<td>GARNE AUS</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Ändern der Größe von Knoten in einem Cluster HBase</td>
<td>HBase-Clustern können jetzt Größe der Knoten für HDInsight Versionen 3.1 und 3.2 (oben und unten)</td>
<td>Dienst</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>JDBC-Aktualisierung</td>
<td>SQL-JDBC-Treiber ist für HDInsight Version 3.2 auf Version sqljdbc_4.0.2206.100 aktualisiert. Diese Version enthält wichtige Sicherheitsfunktionen.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Aktualisierung der JVM</td>
<td>Aktualisierte JVM Konfiguration networkaddress.cache.ttl auf 300 Sekunden vom Standardwert-1 für HDInsight Versionen 3.1 und 3.2. Diese Konfigurationswert steuert die Cacherichtlinie für erfolgreiche Suchvorgänge nach Namen-Service. Dies behebt einen Fehler vergrößern und verkleinern HBase Cluster.</td>
<td>Dienst</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>Upgrade auf Azure Storage SDK für Java 2.0</td>
<td>HDInsight Version 3.2 wird aktualisiert, um die neueste Version von Azure Storage SDK für Java verwenden. Enthält mehrere wichtige Updates über die aktuellen 0.6.0 Version.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Apache WASB Quellcode aktualisiert</td>
<td>HDInsight Version 3.2 jetzt verwendet die neueste code für WASB-Dateisystemtreiber von Apache Hadoop. Diese Änderung wird WASB Treiber jetzt als separate JAR-Datei verpackt. Dies ist lediglich eine Verpackung und keine Änderungen Verhalten des Treibers WASB. Der Name der JAR-Datei ist Hadoop Azure 2.6.0.jar.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>JAR-Dateiname Updates in HDInsight 3.2</td>
<td>Dieses Update HDInsight Version 3.2 enthält mehrere Fehlerbehebungen und einige interne Gläser als Teil des HDP aktualisiert wurden. Beachten Sie, dass dieser JAR-Dateien internen HDP Paket und nicht für die direkte Verwendung durch den Kunden. Programme sollten ihre eigene Version der Gläser Verpacken, sodass Kunden ein Upgrade auf die internen HDP Gläser nicht aufbrechen.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-03032015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 03 03/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.488.1375841 (1.3.9.0-01351 HDP - unverändert)
* HDInsight 3.0.6.488.1375841 (2.0.9.0-2097 HDP - unverändert)
* HDInsight 3.1.3.488.1375841 (2.1.10.0-2290 HDP - unverändert)
* HDInsight 3.2.3.488.1375841 (HDP-2.2.10.0-2340 - unverändert)
* SDK 1.5.0 (unverändert)

Dieses Release enthält das folgende Update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA</th>
</tr>


<tr>
<td>Verbesserte Zuverlässigkeit</td>
<td>Updates, die den Dienst mit erhöhte Last bei Clustererstellung besser skaliert wurde.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-02182015-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 18/02/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.471.1342507 (1.3.9.0-01351 HDP - unverändert)
* HDInsight 3.0.6.471.1342507 (2.0.9.0-2097 HDP - unverändert)
* HDInsight 3.1.3.471.1342507 (2.1.10.0-2290 HDP - unverändert)
* HDInsight 3.2.3.471.1342507 (HDP-2.2.10.0-2340)
* SDK 1.5.0

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>3.2 HDInsight Cluster</td>
<td>Hadoop 2.6/HDP2.2 mit HDInsight 3.2 verfügbar ist. Es enthält wichtige Updates für alle Open Source-Komponenten. Weitere Informationen finden Sie unter Neuigkeiten in HDInsight und <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html" target="_blank">HDP 2.2.0.0 Release Notes</a>.</td>
<td>Open-Source-software</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>HDinsight unter Linux (Vorschau)</td>
<td>Cluster können auf Ubuntu Linux bereitgestellt werden. Weitere Informationen finden Sie unter Erste Schritte mit HDInsight unter Linux.</td>
<td>Dienst</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Storm allgemeine Verfügbarkeit</td>
<td>Apache Storm-Cluster sind erhältlich. Weitere Informationen finden Sie unter fangen mit Sturm in HDInsight.</td>
<td>Dienst</td>
<td>Sturm</td>
<td>N/A</td>
</tr>

<tr>
<td>Größe der virtuellen Computer</td>
<td>Azure HDInsight ist virtueller Maschinen und Größen verfügbar. HDInsight nutzen A2 A7 Größen für allgemeine Zwecke entwickelt; D-Serie-Knoten die Solid-State-Laufwerken (SSDs) und 60 Prozent schnellere Prozessoren verfügen; und A8 und A9, die InfiniBand unterstützen für schnelles Netzwerk.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Cluster-Skalierung</td>
<td>Ändern der Anzahl Datenknoten ausgeführten HDInsight Cluster ohne löschen oder neu erstellen. Derzeit können nur Hadoop Abfrage- und Apache Storm Clustertyp dieser, aber Unterstützung für Apache HBase Clustertyp bald ist folgen. Weitere Informationen finden Sie unter Verwalten HDInsight-Cluster.</td>
<td>Dienst</td>
<td>Hadoop Sturm</td>
<td>N/A</td>
</tr>

<tr>
<td>Visual Studio-Tools</td>
<td>Neben vollständigen Werkzeuge für Apache Storm wurde die Werkzeuge für Apache-Struktur in Visual Studio Anweisungsabschluss, lokale und verbesserte Debugunterstützung aktualisiert. Weitere Informationen finden Sie unter Erste Schritte mit HDInsight Hadoop Tools für Visual Studio.</td>
<td>Werkzeuge</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Hadoop Connector für DocumentDB</td>
<td>Hadoop Connector für DocumentDB können Sie über Ihr Schema weniger JSON Dokumente DocumentDB Sammlungen oder Datenbank Konten komplexe Aggregationen, Analyse und Manipulationen ausführen. Weitere Informationen und ein Lernprogramm finden Sie unter Ausführen Hadoop Aufträge mit DocumentDB und HDInsight.</td>
<td>Dienst</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Fehlerkorrekturen</td>
<td>Wir haben verschiedene kleinere Fehlerbehebungen für HDInsight Dienste. Keine Kunden Verhalten ändern sollten.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-02062015-release-of-hdinsight"></a>Hinweise zur Version von HDInsight 02/06/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.463.1325367 (1.3.9.0-01351 HDP - unverändert)
* HDInsight 3.0.6.463.1325367 (2.0.9.0-2097 HDP - unverändert)
* HDInsight 3.1.2.463.1325367 (HDP 2.1.10.0-2290)
* SDK-N

Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Fehlerkorrekturen</td>
<td>Wir haben verschiedene kleinere Fehlerbehebungen für HDInsight Dienste. Keine Kunden Verhalten ändern sollten.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP 2.1 Wartung aktualisieren</td>
<td>HDInsight 3.1 aktualisiert HDP 2.1.10.0 bereitstellen. Weitere Informationen finden Sie unter <a href ="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.10/bk_releasenotes_hdp_2.1/content/ch_relnotes-HDP-2.1.10.html" target="_blank">Versionsinformationen HDP-2.1.10</a>. </td>
<td>Open-Source-software</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP binäre updates</td>
<td>Gibt es ein paar JAR-Dateien in HBase für die Datei Namen aktualisiert wurden. Diese JAR-Dateien werden durch HBase, intern verwendet, damit nicht erwartet wird, dass die Kunden eine Abhängigkeit auf den Namen der JAR-Dateien. Dazu gehören:
<ul>
<li>./lib/jetty-6.1.26.HWx.jar</li>
<li>./lib/jetty-sslengine-6.1.26.HWx.jar</li>
<li>./lib/jetty-util-6.1.26.HWx.jar</li>
</ul>
</td>
<td>Open-Source-software</td>
<td>HBase</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-1292015-release-of-hdinsight"></a>Hinweise zur Version des HDInsight 1/29/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.455.1309616 (1.3.9.0-01351 HDP - unverändert)
* HDInsight 3.0.6.455.1309616 (2.0.9.0-2097 HDP - unverändert)
* HDInsight 3.1.2.455.1309616 (2.1.9.0-2196 HDP - unverändert)
* SDK-N

Dieses Release enthält das folgende Update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Betroffener Bereich (z. B. Service, Komponente oder SDK)</p></th>
<th>Cluster-Typ (z. B. Hadoop, HBase oder Sturm)</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>

<td>Fehlerkorrekturen</td>
<td>Wir haben einige wichtige Updates, die die Zuverlässigkeit der HDInsight bei Azure Upgrades zu verbessern.</td>
<td>Dienst</td>
<td>Alle</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-152015-release-of-hdinsight"></a>Hinweise zur Version des HDInsight 1/5/2015

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt:

* HDInsight 2.1.10.420.1246118 (1.3.9.0-01351 HDP - unverändert)
* HDInsight 3.0.6.420.1246118 (2.0.9.0-2097 HDP - unverändert)
* HDInsight 3.1.2.420.1246118 (2.1.9.0-2196 HDP - unverändert)


Diese Version enthält die folgenden Updates.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Beispiele für Twitter Trendanalyse und Mahout-Films recommendations</td>
<td><p>In dieser Version wurde die Konsole HDInsight Abfrage zwei weitere Beispiele:</p>

<p><b>Trendanalyse Twitter</b><br>
Von Websites wie Twitter bereitgestellten öffentlichen APIs bieten nützliche Daten für Analyse und beliebte Trends. Erfahren Sie in diesem Lernprogramm, wie Struktur verwenden, um eine Liste der Twitter-Benutzer, die ein bestimmtes Wort enthält die meisten Tweets gesendet. </p>

<p><b>Mahout Film Empfehlung</b><br>
Apache Mahout ist eine Apache Hadoop Bibliothek lernen. Mahout enthält Algorithmen für die Verarbeitung von Daten (z. B. filtern, Klassifizierung und clustering). Verwenden Sie in diesem Lernprogramm eine Empfehlung Engine zu Film eingeführter Filme Freunden gesehen haben.</p></td>
<td>Abfragekonsole</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Standardwert für Struktur-Konfiguration ändern: hive.auto.convert.join.noconditionaltask.size</td>
<td><p>Diese Größenkonfiguration gilt auf Karte automatisch konvertiert. Der Wert stellt die Summe der Größen der Tabellen Hash Karten konvertiert werden können, die in den Speicher passen. In einer früheren Version erhöht diesen Wert vom Standardwert 10 MB, 128 MB Jedoch wurde der neue Wert von 128 MB Aufträge scheitern an Speicher verursacht. Diese Version wird den Standardwert auf 10 MB. Kunden können diesen Wert während der Clustererstellung überschreiben aufgrund ihrer Abfragen und Tabellen. Weitere Informationen zu dieser Einstellung und das Überschreiben finden Sie unter <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.0.2/ds_Hive/optimize-joins.html#JoinOptimization-OptimizeAutoJoinConversion" target="_blank">Optimieren automatisch verknüpfen Konvertierung</a> in Hortonworks Dokumentation. </p></td>
<td>Struktur</td>
<td>Hadoop Hbase</td>
<td>N/A</td>
</tr>

</table>
<br>

## <a name="notes-for-12232014-release-of-hdinsight"></a>Notizen 12. 23 2014 Version von HDInsight

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.10.420.1246783 (HDP Version unverändert)
* HDInsight 3.0.6.420.1246783 (HDP Version unverändert)
* HDInsight 3.1.1.420.1246783 (HDP Version unverändert)

Dieses Release enthält das folgende Update.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>


<tr>
<td>Unterbrochene Erstellung Clusterfehler aufgrund übermäßiger Auslastung</td>
<td><p>Verbesserter Algorithmus für HDP herunterzuladen während der Clustererstellung kann stabiler Behandlung aufgrund übermäßiger Auslastung.</p></td>
<td>Dienst</td>
<td>Hadoop, Hbase, Sturm</td>
<td>N/A</td>
</tr>



</table>
<br>

## <a name="notes-for-12182014-release-of-hdinsight"></a>Notizen 12. 18 2014 Version von HDInsight

Diese Version enthält das folgenden Komponentenupdate.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td><a href = "hdinsight-hadoop-customize-cluster.md" target="_blank">Cluster-Anpassung allgemeine Avalability</a></td>
<td><p>Anpassung bietet die Möglichkeit zur Anpassung von Apache Hadoop-Ökosystem verfügbaren Azure HDInsight Cluster mit Projekten. Mit dieser neuen Funktion können experimentieren und Hadoop Projekte für Azure HDInsight bereitstellen. Durch das Feature **Skriptaktion** wird aktiviert die Hadoop-Cluster mithilfe von benutzerdefinierten Skripts auf beliebige Weise ändern können. Diese Anpassung ist auf alle Arten von HDInsight Cluster einschließlich Hadoop, HBase und Sturm. Demonstrieren die Leistungsfähigkeit dieser Funktion haben wir beliebte <a href = "hdinsight-hadoop-spark-install.md" target="_blank">Spark</a>, <a href = "hdinsight-hadoop-r-scripts.md" target="_blank">R</a>, <a href = "hdinsight-hadoop-solr-install.md" target="_blank">Solr</a>und <a href = "hdinsight-hadoop-giraph-install.md" target="_blank">Giraph</a> Module installieren dokumentiert. Diese Version fügt auch die Möglichkeit für Kunden, die an ihre benutzerdefinierte Skriptaktion Portal Azure enthält Richtlinien und bewährte Methoden zum Erstellen benutzerdefinierter Skriptaktionen mit Hilfsmethoden und Leitlinien über die Skriptaktion testen. </p></td>
<td>Allgemeine Funktion Verfügbarkeit</td>
<td>Alle</td>
<td>N/A</td>
</tr>


</table>
<br>

## <a name="notes-for-12052014-release-of-hdinsight"></a>Hinweise zur Version 12/05/2014 HDInsight

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.9.406.1221105 (HDP 1.3.9.0-01351)
* HDInsight 3.0.5.406.1221105 (HDP 2.0.9.0-2097)
* HDInsight 3.1.1.406.1221105 (HDP 2.1.9.0-2196)
* HDInsight SDK k.a.

Diese Version enthält die folgenden Updates für Komponenten.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>Fehlerkorrektur: gelegentlich Fehler beim Hinzufügen von großen Anzahl von Partitionen zu einer Tabelle in einer Struktur DDL </td>
<td><p>Zeitweise Verbindungsfehler mit der Struktur Metastore Datenbank wird eine Tabelle Struktur viele Partitionen hinzufügen, kann die Struktur DDL fehlschlagen. Die folgende Anweisung und tritt dieser Fehler im Fehlerprotokoll Struktur angezeigt: </p><p>"[Main] Fehler: ql. Treiber (SessionState.java:printError(547)) - Fehler: Fehler Rückgabecode 1 org.apache.hadoop.hive.ql.exec.DDLTask. MetaException (Message:java.lang.RuntimeException: CommitTransaction wurde aufgerufen, aber OpenTransactionCalls = 0. Das bedeutet wahrscheinlich, dass unausgeglichene aufrufen OpenTransaction-CommitTransaction)"</p></td>
<td>Struktur</td>
<td>Hadoop Hbase</td>
<td>Struktur 482 (Dies ist ein interner JIRA kann nicht extern angegeben werden. Siehe hier für Referenz.)</td>
</tr>

<tr>
<td>Fehlerkorrektur: gelegentlich hängt in der HDInsight-Abfrage-Konsole</td>
<td>In diesem Fall die folgende Anweisung im Log WebHCat für WebHCat-Start Job sehen: <p>"org.apache.hive.hcatalog.templeton.CatchallExceptionMapper | org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.yarn.exceptions.YarnRuntimeException): Datei {Url zu der Datei Wasb} konnte nicht geladen werden "</p></td>
<td>WebHCat</td>
<td>Hadoop</td>
<td>Struktur 482 (Dies ist ein interner JIRA kann nicht extern angegeben werden. Siehe hier für Referenz.)</td>
</tr>

<tr>
<td>Fehlerkorrektur: gelegentlich Sammlung Latenz Hbase Abfragen</td>
<td>In diesem Fall sehen Benutzer eine gelegentliche Sammlung von 3 Sekunden in der Hbase Abfragen. </td>
<td>HDInsight Cluster-Gateway</td>
<td>HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>HDP JAR-Datei ändern</td>
<td>Cluster-Version 3.0 gibt es ein paar Änderungen internen HDP installierten JAR-Dateien für HDI. Steg-6.1.26.jar wurde mit Steg 6.1.26.hwx.jar ersetzt. Steg-Util-6.1.26.jar wurde mit Steg Util 6.1.26.hwx.jar ersetzt. Diese Änderungen Hadoop, Mahout, WebHCat und Oozie.</td>
<td>Hadoop, Mahout, WebHCat, Oozie</td>
<td>Hadoop HBase</td>
<td>N/A</td>
</tr>

</table>
<br>


## <a name="notes-for-11212014-release-of-hdinsight"></a>Hinweise zur Version 11 21. 2014 HDInsight

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.9.382.1169709 (keine Änderung von 14/11/2014)
* HDInsight 3.0.5.382.1169709 (keine Änderung von 14/11/2014)
* HDInsight 3.1.1.382.1169709 (keine Änderung von 14/11/2014)
* HDINsight SDK 1.4.0

Diese Version enthält die folgenden Updates für Komponenten.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>Zugriff auf Anwendungsprotokolle</td>
<td>Fähigkeit, programmgesteuert Applikationen aufgelistet, die im Cluster ausgeführt und Downloaden der entsprechenden Anwendung oder spezifische Container Protokolle, um problematische Anwendung debuggen.</td>
<td>SDK</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Bereichsname in IHdInsightClient.DeleteCluster angeben </td>
<td>Azure HDInsight SDK ermöglicht einen angeben, wenn Sie **DeleteCluster**verwenden. Dadurch wird Kunden, zwei Ressourcen mit demselben Namen in unterschiedlichen Regionen und Sie löschen nicht, zulassen.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>ClusterDetails.DeploymentId</td>
<td>Das **ClusterDetails** -Objekt gibt ein **DeploymentID** -Feld, das einen eindeutigen Bezeichner für den Cluster darstellt. Es garantiert für Cluster Erstellversuche mit gleichen Namen eindeutig sein.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/A</td>
</tr>
</table>
<br>

## <a name="notes-for-11142014-release-of-hdinsight"></a>Hinweise zur Version 14/11/2014 HDInsight

Die vollständige Versionsnummern für HDInsight-Cluster mit dieser Version bereitgestellt werden:

* HDInsight 2.1.9.382.1169709
* HDInsight 3.0.5.382.1169709
* HDInsight 3.1.1.382.1169709

Diese Version enthält folgende neue Features, Komponentenupdates und Fehlerkorrekturen.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>Skript für Aktion (Vorschau)</td>
<td>Vorschau des KE Anpassung Cluster, die Änderung von Hadoop ermöglicht Cluster auf beliebige Weise mithilfe von benutzerdefinierten Skripts. Mit diesem Feature können experimentieren und Projekte zur Apache Hadoop Ökosystem Azure HDInsight-Cluster bereitstellen. Diese Anpassung erfolgt auf alle Arten von HDInsight Clustern Hadoop, HBase und Sturm.</td>
<td>Neue Funktion</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>Vordefinierte Aufträge für Azure Websites und Storage-Protokoll</td>
<td>HDInsight Abfrage-Konsole hat eine erste Schritte Galerie, die Lösungen auf Ihre Daten oder Beispieldaten unterstützt.
<p>**Lösungen, die Ihre Daten arbeiten**:<br>
Wir haben für einige der am häufigsten verwendeten Daten Analyse Szenarien, die als Ausgangspunkt zum Erstellen eigener Projektmappen Arbeitsplätze. Können Sie Ihre Daten mit der Funktionsweise. Dann, wenn Sie bereit sind, verwenden Sie Gelernte eine Lösung erstellen, die nach vordefinierten Auftrag modelliert.</p>
<p>**Projektmappen, die Beispieldaten arbeiten**:<br>
Informationen Sie zum Arbeiten mit HDInsight einigen einfachen Szenarien (wie Webprotokollen und Daten) durchlaufen. Sie erfahren, wie Sie HDInsight solche Daten analysieren und wie Sie auch auf diese Daten zugreifen können. Visualisieren von Daten an Microsoft Excel enthält ein Beispiel der Stärke dieses Ansatzes.</p></td>
<td>Abfragekonsole</td>
<td>Hadoop</td>
<td>N/A</td>
</tr>

<tr>
<td>Memory Leck Fix Templeton</td>
<td>Speicherverlust in Templeton, die Kunden betroffen langer Cluster oder wurden 100 s Auftrag senden Anfragen ein zweites behandelt wurde. Als Templeton 5xx-Fehler und die Lösung war, den Dienst neu zu starten. Die Lösung wird nicht mehr benötigt.</td>
<td>Templeton</td>
<td>Alle</td>
<td>https://Issues.apache.org/jira/Browse/HADOOP-11248</td>
</tr>
</table>
<br>


**Hinweis**: demonstrieren Sie die neuen Funktionen von Cluster-Anpassung zur Verfügung gestellt Skriptaktion Spark und R-Module auf einem Cluster installieren mit Prozeduren dokumentiert. Weitere Informationen finden Sie unter:

* [Installieren und Verwenden von Spark 1.0 auf HDInsight-Cluster](hdinsight-hadoop-spark-install.md)
* [Installieren und Verwenden von R HDInsight Hadoop-Cluster](hdinsight-hadoop-r-scripts.md)




## <a name="notes-for-11072014-release-of-hdinsight"></a>Hinweise zur Veröffentlichung von HDInsight 11/07/2014

Die vollständige Versionsnummern für HDInsight-Cluster, die mit dieser Version bereitgestellt werden:

* HDInsight 2.1 2.1.9.374.1153876
* HDInsight 3.0 3.0.5.374.1153876
* HDInsight 3.1 3.1.1.374.1153876

Diese Version enthält die folgenden Updates für Komponenten.

<table border="1">
<tr>
<th>Titel</th>
<th>Beschreibung</th>
<th>Komponente</th>
<th>Clustertyp</th>
<th>JIRA (falls zutreffend)</th>
</tr>

<tr>
<td>HDP 2.1.7</td>
<td>Diese Version basiert auf Hortonworks Daten Plattform (HDP) 2.1.7. Weitere Informationen finden Sie unter <a href="http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html" target="_blank">Release Notes HDP 2.1.7</a>.</td>
<td>HDP</td>
<td>Alle</td>
<td>N/A</td>
</tr>

<tr>
<td>AUS Zeitachse Server</td>
<td>AUS Zeitachse Server (auch bekannt als die generische Geschichte Anwendungsserver) ist standardmäßig aktiviert. Zeitachse Server bietet allgemeine Informationen über Bewerbungen (z. B. ID der Anwendung Anwendungsname, Anwendungsstatus, Anwendung Sendezeit und Beendigungszeit Anwendung).

Diese Anwendungsinformationen aus der Head-Knoten auf den URI Http://headnodehost:8188 oder durch Ausführen des Befehls aus abgerufen: Garn Anwendung – AppStates – Liste aller.

Diese Informationen kann auch Remote Wenn eine REST-API unter https://{ClusterDnsName abgerufen werden}. azurehdinsight.NET/WS/v1/applicationhistory/.

Weitere Informationen finden Sie unter <a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">GARN Zeitachse Server</a>.</td>
<td>GARN-Dienst</td>
<td>Hadoop HBase</td>
<td>N/A</td>
</tr>

<tr>
<td>Cluster Deployment-ID</td>
<td>Ab Version 1.3.3.1.5426.29232 SDK, können Benutzer eine eindeutige ID für jeden Cluster zugreifen HDInsight ausgestellt. So können Unternehmen eindeutige Instanzen von Clustern herausfinden, wenn ein DNS-Name wiederverwendet wird über erstellen oder Löschen von Szenarien.</td>
<td>SDK</td>
<td>Alle</td>
<td>N/A</td>
</tr>
</table>
<br>

**Hinweis**: die Versionsnummer im Portal angezeigt oder das SDK oder Windows PowerShell zurückgegeben wird verhindert Fehler wurde in dieser Version behoben wurden.

## <a name="notes-for-10152014-release"></a>Hinweise zur Version 10/15/2014

Diese Hotfixes behoben einen Speicherverlust in Templeton, die der Schwere Templeton betroffen. In einigen Fällen sieht Benutzer Templeton stark ausgeübt Fehler 500 Fehlercodes da Anfragen nicht genügend Arbeitsspeicher zur Ausführung bezeichnet. Abhilfe für dieses Problem wurde Templeton Dienst neu starten. Dieses Problem wurde behoben.


## <a name="notes-for-1072014-release"></a>Hinweise zur Version 10, 7/2014

* Verwendung von Ambari-Endpunkt "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *Host_name* -Feld gibt den vollqualifizierten Domänennamen (FQDN) des Knotens statt nur den Hostnamen. Beispielsweise erhalten Sie anstatt "**headnode0**" den FQDN "**headnode0. {} ClusterDNS} .azurehdinsight .net**". Diese Änderung war erforderlich, um Szenarien zu ermöglichen, mehrere Clustertypen (wie HBase und Hadoop) in einem virtuellen Netzwerk bereitgestellt werden kann. Dies geschieht beispielsweise bei Verwendung von HBase als Back-End-Plattform für Hadoop.

* Wir haben neue speichereinstellungen für Standard-Bereitstellung von HDInsight Cluster bereitgestellt. Vorherigen Standardeinstellungen Arbeitsspeicher berücksichtigte nicht angemessen der Anleitung für die Anzahl der CPU-Kerne bereitgestellt wird. Diese neuen speichereinstellungen soll bessere Standardwerte (nach Hortonworks empfohlen). Um diese zu ändern, finden Sie die SDK Dokumentation zum Ändern der Clusterkonfiguration. Neue speichereinstellungen, die standardmäßig 4 CPU Core (8 Container) HDInsight Cluster verwendet werden in der folgenden Tabelle aufgeführt. (Die Werte vor dieser Version werden auch nebenbei bereitgestellt.)

<table border="1">
<tr><th>Komponente</th><th>Speicher belegen</th></tr>
<tr><td> yarn.Scheduler.Minimum-Zuordnung</td><td>768 MB (zuvor 512 MB)</td></tr>
<tr><td> yarn.Scheduler.Maximum-Zuordnung</td><td>6144 MB (unverändert)</td></tr>
<tr><td>yarn.NodeManager.Resource.Memory</td><td>6144 MB (unverändert)</td></tr>
<tr><td>MapReduce.Map.Memory</td><td>768 MB (zuvor 512 MB)</td></tr>
<tr><td>MapReduce.Map.java.OPTS</td><td>Optionen =-Xmx512m (-Xmx410m)</td></tr>
<tr><td>MapReduce.reduce.Memory</td><td>1536 MB (zuvor 1024 MB)</td></tr>
<tr><td>MapReduce.reduce.java.OPTS</td><td>Optionen =-Xmx1024m (-Xmx819m)</td></tr>
<tr><td>yarn.app.MapReduce.am.Resource</td><td>768 MB (zuvor 1024 MB)</td></tr>
<tr><td>yarn.app.MapReduce.am.Command</td><td>Optionen =-Xmx512m (-Xmx819m)</td></tr>
<tr><td>MapReduce.Task.IO.Sort</td><td>256 MB (zuvor 200 MB)</td></tr>
<tr><td>tez.am.Resource.Memory</td><td>1536 MB (unverändert)</td></tr>

</table><br>

Weitere Informationen über den Arbeitsspeicher Konfigurationseinstellungen GARN und MapReduce auf der Hortonworks, die von HDInsight verwendet wird, finden Sie unter [Bestimmen HDP Memory Configuration Settings](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1-latest/bk_installing_manually_book/content/rpm-chap1-11.html). Hortonworks stellte auch ein Tool zur richtigen speichereinstellungen berechnen.

Bezüglich der Azure PowerShell und HDInsight SDK-Fehlermeldung: "*Cluster nicht für HTTP-Zugriff konfiguriert*":

* Dieser Fehler ist ein bekanntes [Kompatibilitätsproblem](https://social.msdn.microsoft.com/Forums/azure/a7de016d-8de1-4385-b89e-d2e7a1a9d927/hdinsight-powershellsdk-error-cluster-is-not-configured-for-http-services-access?forum=hdinsight) , die unterschiedliche HDInsight SDK oder Azure PowerShell Version und der Version des Clusters auftreten können. Cluster auf 8-15 oder höher erstellt bieten Unterstützung für neue provisioning-Funktionen in virtuellen Netzwerke. Aber diese Funktion ist mit älteren Versionen von HDInsight SDK oder Azure PowerShell nicht richtig interpretiert. Das Ergebnis ist ein Fehler in einigen Übermittlung Jobs. Verwenden Sie HDInsight SDK APIs oder Azure PowerShell-Cmdlets (**Mit AzureRmHDInsightCluster** oder **AzureRmHDInsightHiveJob aufrufen**) Aufträge senden, möglicherweise die Vorgänge mit der Fehlermeldung "*Cluster <clustername> nicht für HTTP-Zugriff konfiguriert*." Oder (je nach Vorgang) können andere Fehlermeldungen wie "*keine Verbindung zum Cluster herstellen*".

* Diese Kompatibilitätsprobleme werden in den neuesten Versionen des HDInsight SDK und Azure PowerShell aufgelöst. Empfehlen wir HDInsight SDK, Version 1.3.1.6 oder höher und Azure PowerShell Version 0.8.8 oder höher. Sie erhalten Zugriff auf die neuesten HDInsight SDK von [](http://nuget.codeplex.com/wikipage?title=Getting%20Started) und Azure PowerShell-Tools wie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).



## <a name="notes-for-9122014-release-of-hdinsight-31"></a>Hinweise zur Version 9 12. 2014 HDInsight 3.1

* Diese Version basiert auf Hortonworks Daten Plattform (HDP) 2.1.5. Eine Liste der behobenen Probleme in dieser Version finden Sie auf der Website Hortonworks Seite [in dieser Version korrigiert](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.5/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.5-fixed.html) .
* Schwein Bibliotheken im Ordner "Avro-Mapred-1.7.4.jar" Datei "Avro-Mapred-1.7.4-hadoop2.jar." wurde Der Inhalt dieser Datei enthält eine kleine Fehlerkorrektur geschütztes ist. Es wird empfohlen, dass Kunden keine direkte Abhängigkeit auf den Namen der JAR-Datei Pausen zu, wenn Dateien umbenannt werden.


## <a name="notes-for-8212014-release"></a>Hinweise zur Version 8, 21/2014

* Wir fügen die folgende WebHCat Konfiguration (HIVE-7155) der Speichers Standardgrenzwert Stellensuche Templeton Controller auf 1 GB festgelegt. (Vorherige Standardwert wurde 512 MB).

     templeton.Mapper.Memory.MB (= 1024)

    * Diese Änderung behebt bestimmte Struktur, aufgrund niedriger Speicher in Abfragen mussten folgenden Fehler: "Container wird Arbeitsspeicher Grenzen ausgeführt."
    * Um die alten Standardeinstellungen wiederherzustellen, können diesen Konfigurationswert 512 über Azure PowerShell zur Erstellungszeit Cluster festlegen mithilfe des folgenden Befehls:

        Fügen Sie AzureRmHDInsightConfigValues-Kern@{"templeton.mapper.memory.mb"="512";}


* Der Hostname der Rolle Zookeeper wurde in *Zookeeper*geändert. Dies betrifft Auflösung innerhalb des Clusters, aber es wirkt sich nicht auf externe REST-APIs. Haben Sie Komponenten, die den *Zookeepernode* -Hostnamen verwenden, müssen Sie diese neuen Namen aktualisieren. Neuen Namen für die drei Zookeeper-Knoten sind:
    * zookeeper0
    * zookeeper1
    * zookeeper2
* HBase Version Supportmatrix wird aktualisiert. Nur HDInsight Version 3.1 (HBase Version 0,98) werden für die Produktion für HBase Workloads. (Die Vorschau) Version 3.0 wird nicht vorwärts verschieben unterstützt.

## <a name="notes-about-clusters-created-prior-to-8152014"></a>Hinweise zum Cluster vor 15/8/2014 erstellt

Azure PowerShell oder HDInsight SDK Fehlermeldung "Cluster <clustername> nicht für HTTP-Zugriff konfiguriert" (oder abhängig von der Operation andere Fehlermeldungen wie: "Kann nicht mit Cluster verbinden") aufgrund einer Version zwischen Azure PowerShell SDK HDInsight und einem Cluster auftreten. Cluster auf 8-15 oder höher erstellt bieten Unterstützung für neue provisioning-Funktionen in virtuellen Netzwerke. Diese Funktion ist nicht mit älteren Versionen von Azure PowerShell oder HDInsight SDK führt zu Fehlern bei Einreichung Jobs richtig interpretiert. Verwenden Sie HDInsight SDK APIs oder Azure PowerShell-Cmdlets (z. B. mit AzureRmHDInsightCluster oder AzureRmHDInsightHiveJob aufrufen) Aufträge senden, möglicherweise die Vorgänge eines Fehlermeldungen.

Diese Kompatibilitätsprobleme werden in den neuesten Versionen des HDInsight SDK und Azure PowerShell aufgelöst. Empfehlen wir HDInsight SDK, Version 1.3.1.6 oder höher und Azure PowerShell Version 0.8.8 oder höher. Sie erhalten Zugriff auf die neuesten HDInsight SDK von [NuGet][nuget-link]. Zugriff Azure PowerShell-Tools mit [Microsoft-Webplattform-Installer][webpi-link].


## <a name="notes-for-7282014-release"></a>Hinweise zur Version 7, 28/2014

* **HDInsight in neue Regionen**: wir HDInsight Präsenz auf drei Bereiche erweitert. HDInsight-Kunden können in diesen Regionen Cluster erstellen:
    * Ostasien
    * Norden der USA – zentral
    * Südlichen zentralen USA
* HDInsight Version 1.6 (HDP 1.1 und Hadoop 1.0.3) und HDInsight Version 2.1 (HDP1.3 und Hadoop 1.2) werden von Azure-Portal entfernt. Sie können weiterhin Hadoop Cluster für diese Versionen des Azure PowerShell-Cmdlets [Neu AzureRmHDInsightCluster](http://msdn.microsoft.com/library/dn593744.aspx) oder mit [HDInsight SDK](http://msdn.microsoft.com/library/azure/dn469975.aspx)erstellen. Siehe Seite [HDInsight Komponente Versionskontrolle](hdinsight-component-versioning.md) für Weitere Informationen.
* Hortonworks Daten Plattform (HDP) ändert sich in dieser Version:

<table border="1">
<tr><th>HDP</th><th>Ändern</th></tr>
<tr><td>HDP 1.3 / HDI 2.1</td><td>Keine Änderung</td></tr>
<tr><td>HDP 2.0 / 3.0 HDI</td><td>Keine Änderung</td></tr>
<tr><td>HDP 2.1 / HDI 3.1</td><td>Zookeeper: [3.4.5.2.1.3.0-1948] [3.4.5.2.1.3.2-0002] -></td></tr>


</table><br>

## <a name="notes-for-6242014-release"></a>Hinweise zur Version 6, 24/2014

Dieses Release enthält verbesserte HDInsight-Dienst:

* **HDP 2.1 Verfügbarkeit**: HDInsight 3.1 (enthält HDP 2.1) ist und die Standardversion für jeden neuen Cluster.
* **HBase – Azure Portal Verbesserung**: Wir machen HBase Cluster in der Vorschau zur Verfügung. Sie können HBase Cluster aus dem Portal mit nur wenigen Mausklicks erstellen. 

Mit HBase können Sie vielfältige Echtzeit Arbeitslasten auf HDInsight, interaktive Websites erstellen, die mit großen Datasets Dienste speichern Sensor und Telemetrie Daten aus Millionen von Endpunkten. Der nächste Schritt wäre zum Analysieren der Daten in diesen Workloads Hadoop Aufträge und kann in HDInsight Azure PowerShell und Struktur Cluster Dashboard.

### <a name="apache-mahout-preinstalled-on-hdinsight-31"></a>Apache Mahout HDInsight 3.1 vorinstalliert

 [Mahout](http://hortonworks.com/hadoop/mahout/) ist auf HDInsight 3.1 Hadoop Cluster Mahout Aufträge ohne zusätzliche Konfiguration ausgeführt werden kann. Beispielsweise können Sie remote in einem Hadoop-Cluster mit Remote Desktop Protocol (RDP) und ohne weitere Schritte können Hello World Mahout Folgendes ausführen:

        mahout org.apache.mahout.classifier.df.tools.Describe -p /user/hdp/glass.data -f /user/hdp/glass.info -d I 9 N L  

        mahout org.apache.mahout.classifier.df.BreimanExample -d /user/hdp/glass.data -ds /user/hdp/glass.info -i 10 -t 100

Eine vollständige Erläuterung dieses Vorgangs finden Sie unter Dokumentation [Breiman Beispiel](https://mahout.apache.org/users/classification/breiman-example.html) Apache Mahout-Website.


### <a name="hive-queries-can-use-tez-in-hdinsight-31"></a>Struktur Abfragen können Tez in HDInsight 3.1

Struktur 0,13 steht in HDInsight 3.1 und kann Abfragen mit Tez für beträchtliche Leistungssteigerungen genutzt werden kann.
Tez ist nicht standardmäßig für Hive-Abfragen aktivieren. Um es zu verwenden, müssen Sie anmelden. Tez können mit den folgenden Codeausschnitt:

        set hive.execution.engine=tez;
        select sc_status, count(*), histogram_numeric(sc_bytes,5) from website_logs_orc_local group by sc_status;

Hortonworks hat eine detaillierte Aufschlüsselung der Struktur Abfrage Leistungssteigerungen mit Tez veröffentlicht Standards an. Details finden Sie unter [Benchmarking Apache Hive 13 für Enterprise Hadoop](http://hortonworks.com/blog/benchmarking-apache-hive-13-enterprise-hadoop/).

Weitere Informationen zur Struktur mit Tez finden Sie in der [Struktur auf Tez](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez).

###<a name="global-availability"></a>Globale Verfügbarkeit
Mit der Veröffentlichung von HDInsight Hadoop 2.2 hat Microsoft HDInsight in allen wichtigen Regionen zur Verfügung, Azure verfügbar ist. West Europa und Asien Rechenzentren haben, online geschaltet wurde. Dadurch können Kunden zu Cluster in einem Datencenter schließen wird und potenziell in einer ähnlichen Vorschriften.


###<a name="dos--donts-between-cluster-versions"></a>Aufgaben & zwischen Cluster-Versionen

**Oozie Metastores mit einem HDInsight 3.1 verwendet sind nicht abwärtskompatibel mit HDInsight 2.1 und kann nicht mit dieser vorherigen Version verwendet werden**.

Eine benutzerdefinierte Oozie Metastore Datenbank mit einem HDInsight 3.1 Cluster bereitgestellt kann mit einem HDInsight 2.1 wiederverwendet werden. Dies ist der Fall, selbst wenn die Metastore mit einem HDInsight 2.1 stammt. Dieses Szenario wird nicht unterstützt, da Metastore Schema aktualisiert wird, wenn mit einem Cluster HDInsight 3.1 verwendet nicht mehr kompatibel mit Metastore HDInsight 2.1 Cluster erforderlich. Jeder Versuch, einen Oozie Metastore verwenden, die mit einem HDInsight 3.1 verwendet wird HDInsight 2.1 Cluster unbrauchbar machen.

**Oozie Metastores kann über Cluster verwendet werden.**

Oozie Metastores bestimmten Cluster zugeordnet werden und sie nicht über Cluster gemeinsam genutzt werden.

###<a name="breaking-changes"></a>Grundlegend geändert

**Präfix Syntax**: nur die "Wasbs: / /" Syntax in HDInsight 3.1 und 3.0 Cluster unterstützt. Die älteren "Luftsauerstoff: / /" Syntax HDInsight 2.1 und 1,6 Cluster unterstützt, jedoch wird in HDInsight 3.1 oder 3.0-Clustern nicht unterstützt. Dies bedeutet, dass alle Aufträge an eine HDInsight 3.1 oder 3.0 Cluster gesendet, die explizit die "Luftsauerstoff: / /" Syntax fehl. Die "Wasbs: / /" Syntax sollte stattdessen verwendet werden. Auch Aufträge alle HDInsight 3.1 oder 3.0 Cluster mit einem vorhandenen Metastore erstellt werden, die explizite Verweise auf Ressourcen enthält die "Luftsauerstoff: / /" Syntax fehl. Diese Metastores müssen neu erstellt werden, mit der "Wasbs: / /" Syntax Adressenressourcen.


**Anschlüsse**: von der HDInsight-Dienst verwendeten Ports haben. Die Portnummern verwendet wurden im temporären Portbereich des Windows-Betriebssystems. Ports werden automatisch zwischen vordefinierten flüchtigen kurzlebige Internet Protocol-basierte Kommunikation zugeordnet. Der neue Satz von zulässigen Hortonworks Daten Plattform (HDP) Service-Portnummern sind außerhalb dieses Bereichs auf dem Head-Knoten ausgeführten Dienste verwendeten Ports entstehen Konflikte verhindern. Die neuen Portnummern dürfen nicht einer führen. Die Zahlen sind wie folgt:

 **HDInsight 1.6 (HDP 1.1)**
<table border="1">
<tr><th>Name</th><th>Wert</th></tr>
<tr><td>DFS.http.Address</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.Ipc.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.Secondary.http.Address</td><td>0.0.0.0:30090</td></tr>
<tr><td>mapred.Job.Tracker.http.Address</td><td>jobtrackerhost:30030</td></tr>
<tr><td>mapred.Task.Tracker.http.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>MapReduce.History.Server.http.Address</td><td>0.0.0.0:31111</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

 **HDInsight 3.1 und 3.0 (HDP 2.1 und 2.0)**
<table border="1">
<tr><th>Name</th><th>Wert</th></tr>
<tr><td>DFS.namenode.HTTP-Adresse</td><td>namenodehost:30070</td></tr>
<tr><td>DFS.namenode.HTTPS-Adresse</td><td>headnodehost:30470</td></tr>
<tr><td>DFS.datanode.Address</td><td>0.0.0.0:30010</td></tr>
<tr><td>DFS.datanode.http.Address</td><td>0.0.0.0:30075</td></tr>
<tr><td>DFS.datanode.Ipc.Address</td><td>0.0.0.0:30020</td></tr>
<tr><td>DFS.namenode.Secondary.HTTP-Adresse</td><td>0.0.0.0:30090</td></tr>
<tr><td>yarn.NodeManager.WebApp.Address</td><td>0.0.0.0:30060</td></tr>
<tr><td>templeton.Port</td><td>30111</td></tr>
</table><br>

###<a name="dependencies"></a>Abhängigkeit

Die folgenden Abhängigkeiten wurden in HDInsight 3.x (HDP2.x):

* Guice-servlet
* Optiq-Kern
* javax.inject
* Aktivierung
* jsr305
* Geronimo-Jaspic_1.0_spec
* Juli slf4j
* Java-xmlbuilder
* Ant
* Commons-compiler
* Jdo-api
* Commons math3
* paranamer
* Jaxb impl
* stringtemplate
* Eigenbase xom
* Jersey servlet
* Commons exec
* Jaxb-api
* Steg alle server
* janino
* xercesImpl
* Optiq avatica
* jta
* Eigenbase-Eigenschaften
* einzigartige alle
* Hamcrest-Kern
* e-Mail-Nachrichten
* linq4j
* jpam
* Jersey-client
* aopalliance
* Geronimo-Annotation_1.0_spec
* Ant-Startprogramm
* Jersey guice
* XML-apis
* Stax-api
* ASM commons
* ASM-Struktur
* wadl
* Geronimo-Jta_1.1_spec
* guice
* Alle leveldbjni
* Geschwindigkeit
* über Bord werfen
* leicht lesbare java
* Steg-alle
* Commons-Daemon

Folgenden Abhängigkeiten vorhanden nicht in HDInsight 3.x (HDP2.x):

* jdeb
* KFS
* sqlline
* Efeu
* aspectjrt
* JSON
* Core
* jdo2-api
* Avro mapred
* Datanucleus-Verstärker
* JSP
* Commons-Protokollierung-api
* Commons Mathematik
* JavaEWAH
* aspectjtools
* javolution
* hdfsproxy
* hbase
* Flotten

###<a name="version-changes"></a>Ändert

Die folgende Version Änderungen zwischen HDInsight 2.x (HDP1.x) und HDInsight 3.x (HDP2.x):

* Metriken Core: [2.1.2]-[3.0.0] >
* Derbynet: [10.4.2.0] [10.10.1.1] ->
* Datanucleus: ['Rdbms-3.0.8'] -> ['Rdbms-3.2.9']
* JASPER Compiler: [5.5.12]-[5.5.23] >
* log4j: [1.2.15', '1.2.16.'] -> ['1.2.16.', '1.2.17']
* Derbyclient: [10.4.2.0] [10.10.1.1] ->
* Httpcore: [4.2.4]-[4.2.5] >
* Hsqldb: [1.8.0.10]-[2.0.0] >
* jets3t: [0.6.1]-[0.9.0] >
* Protobuf Java: [2.4.1]-[2.5.0] >
* Derby: [10.4.2.0] [10.10.1.1] ->
* JASPER: ['Runtime-5.5.12'] -> ['Runtime-5.5.23']
* Commons-Daemon: [1.0.1]-[1.0.13] >
* zentrale Datanucleus: [3.0.9]-[3.2.10] >
* Datanucleus-api-Jdo: [3.0.7]-[3.2.6] >
* Zookeeper: [3.4.5.1.3.9.0-01320] [3.4.5.2.1.3.0-1948] ->
* Bonecp: [0.7.1.RELEASE] -> ['
* 0.8.0.RELEASE']


### <a name="drivers"></a>Treiber
Java Database Connnectivity (JDBC) Treiber für SQL Server wird intern vom HDInsight verwendet und ist nicht für externe Vorgänge verwendet. Verwenden Sie möchten HDInsight mit Open Database Connectivity (ODBC), Microsoft Struktur ODBC-Treiber. Weitere Informationen finden Sie unter [Verbinden Excel HDInsight mit Microsoft Struktur ODBC-Treiber](hdinsight-connect-excel-hive-odbc-driver.md).


### <a name="bug-fixes"></a>Fehlerkorrekturen

In dieser Version haben wir die folgenden HDInsight Versionen mit mehreren Updates aktualisiert:

* HDInsight 2.1 (HDP 1.3)
* HDInsight 3.0 (HDP 2.0)
* HDInsight 3.1 (HDP 2.1)


## <a name="hortonworks-release-notes"></a>Hortonworks Versionshinweise

Versionshinweise für Hortonworks Datenplattformen (HDPs), die HDInsight Version Clustern verwendet werden an folgenden Speicherorten:

* HDInsight Version 3.1 verwendet Hadoop Verteilung, die auf der [Plattform Hortonworks 2.1.7][hdp-2-1-7]. Dies ist bei Verwendung von Azure-Portal nach 7/11/2014 erstellt standardmäßig Hadoop Cluster. 3.1 HDInsight Cluster vor 7/11/2014 basieren auf der [Hortonworks-Plattform 2.1.1] erstellt[hdp-2-1-1]

* HDInsight Version 3.0 verwendet, die auf [Hortonworks Daten Plattform 2.0]Hadoop Verteilung[hdp-2-0-8].

* HDInsight Version 2.1 verwendet, die auf [Hortonworks Daten Plattform 1.3]Hadoop Verteilung[hdp-1-3-0].

* HDInsight Version 1.6 verwendet, die auf [Hortonworks Daten Plattform 1.1]Hadoop Verteilung[hdp-1-1-0].

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[nuget-link]: https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.HDInsight/

[webpi-link]: http://go.microsoft.com/?linkid=9811175&clcid=0x409

[hdinsight-install-spark]: ../hdinsight-hadoop-spark-install/
[hdinsight-r-scripts]: ../hdinsight-hadoop-r-scripts/
 
