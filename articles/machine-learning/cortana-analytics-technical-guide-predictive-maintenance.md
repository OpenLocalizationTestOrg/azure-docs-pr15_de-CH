<properties
    pageTitle="Technisches Handbuch der Vorlage Cortana Intelligence Lösung für die vorbeugende Wartung Luftfahrt und andere Unternehmen | Microsoft Azure"
    description="Technischen Handbuch zur Vorlage Lösung mit Microsoft Cortana für die vorbeugende Wartung Luftfahrt, Dienstprogramme und Transport."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Technisches Handbuch der Vorlage Cortana Intelligence Lösung für die vorbeugende Wartung Luftfahrt und andere Unternehmen

## <a name="acknowledgements"></a>**Danksagungen**
Dieser Artikel wird von Datenanalysten Yan Zhang Gauher Shaheen Fidan Boylu Uz und Softwareentwickler Dan Grecoe bei Microsoft verfasst.

## <a name="overview"></a>**Übersicht**

Lösung sind Vorlagen beschleunigen das Erstellen einer E2E-Demo auf Cortana Intelligence Suite. Bereitgestellte Vorlage Bereitstellen Ihres-Abonnements erforderlichen Cortana Intelligence Komponenten und Beziehungen zwischen ihnen. Außerdem wird die Datenpipeline mit Beispieldaten aus einem Generator-Anwendung herunterladen und installieren auf dem lokalen Computer nach dem Bereitstellen der Lösungsvorlage generiert Samen. Die Daten aus dem Generator werden unterbrechen Datenpipeline und erzeugen Computer lernen Vorhersagen der Power BI-Dashboard dargestellt werden können. Der Bereitstellungsprozess führt Sie durch mehrere Schritte, um Ihre Anmeldeinformationen Lösung einzurichten. Stellen Sie sicher, dass diese Anmeldeinformationen wie Projektmappennamen, Benutzername und Kennwort während der Bereitstellung bieten aufzeichnen.  

Ziel dieses Dokuments ist die Referenzarchitektur und anderen Komponenten in Ihrem Abonnement als Teil dieser Lösungsvorlage bereitgestellt. Das Dokument spricht auch zum Ersetzen der Beispieldaten mit echten Daten selbst Einblicke und Vorhersagen von Ihren eigenen Daten sehen können. Darüber hinaus enthält das Dokument Teile der Lösung-Vorlage, die geändert werden, wenn Sie die Lösung mit Ihren eigenen Daten anpassen. Am Ende werden Informationen zum Power BI-Dashboard für diese Lösungsvorlage erstellen bereitgestellt.

>[AZURE.TIP] Herunterladen und Drucken einer [PDF-Version dieses Dokuments](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Überblick**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Beim Bereitstellen der Lösung verschiedene Azure Services Cortana Analytics Suite aktiviert (*d. h.* Event Hub Stream Analytics HDInsight Data Factory, maschinelles lernen, *etc.*). Das Architekturdiagramm oben zeigt auf hohem Niveau Aufbau vorbeugende Wartung für Luft-und Projektmappenvorlage von Ende zu Ende. Sie werden diese Dienste in Azure-Portal durch Anklicken der Lösung Vorlage Diagramm diesen Dienst bei Bedarf bereitgestellt wird, wenn die zugehörige Rohrleitung Aktivitäten ausführen und gelöschte anschließend mit der Bereitstellung der Lösung mit Ausnahme HDInsight erstellt untersuchen.
Sie können [das Diagramm in voller](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

Die folgenden Abschnitte beschreiben jedes.

## <a name="data-source-and-ingestion"></a>**Datenquelle und Aufnahme**

### <a name="synthetic-data-source"></a>Synthetische Datenquelle

Für diese Vorlage wird die Datenquelle über eine desktop-Anwendung, die Sie herunterladen und lokal ausführen nach einer erfolgreichen Bereitstellung. Finden Sie die Anleitung zum Herunterladen und Installieren dieser Anwendung in der eigenschaftenleiste, wenn auf dem ersten Knoten vorbeugende Wartung Datengenerator Lösung Vorlage Diagramm auswählen. Diese Anwendung feeds [Azure Event Hub](#azure-event-hub) Service mit Daten oder Ereignisse, die im Rest des Flusses Lösung verwendet werden. Diese Datenquelle besteht aus oder öffentlich zugängliche Daten aus dem [NASA-Datenrepository](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) [Turbofan Engine Beeinträchtigung Simulation DataSet](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Ereignis generieren Anwendung füllt Azure Event Hub nur während der Ausführung auf Ihrem Computer.

### <a name="azure-event-hub"></a>Azure Event hub

[Azure Event Hub](https://azure.microsoft.com/services/event-hubs/) -Dienst ist der Empfänger der Benutzereingabe synthetische Datenquelle beschriebenen.

## <a name="data-preparation-and-analysis"></a>**Vorbereitung und Analyse**


### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -Dienst wird in Echtzeit-Analysen im Eingabestream [Azure Event Hub](#azure-event-hub) Service und Ergebnisse auf [Power BI](https://powerbi.microsoft.com) -Dashboard, Archivierung rohen eingehende Ereignisse [Azure Storage](https://azure.microsoft.com/services/storage/) Service für die spätere Verarbeitung von [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) -Dienst verwendet.

### <a name="hd-insights-custom-aggregation"></a>Benutzerdefinierte Aggregation HD Einblicke

Der Dienst Azure HD Insight zur [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (orchestriert von Azure Data Factory) Skripts um Aggregationen auf die rohen Ereignisse bereitzustellen, die den Dienst Azure Stream archiviert wurden.

### <a name="azure-machine-learning"></a>Azure maschinelles lernen

[Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -Dienst (orchestriert von Azure Data Factory) verwendet die Restnutzungsdauer (RUL) eines Motors Flugzeug erhalten die Beiträge Vorhersagen zu.

## <a name="data-publishing"></a>**Daten veröffentlichen**


### <a name="azure-sql-database-service"></a>Azure SQL-Datenbank-Dienst

[Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) -Dienst wird verwendet, um speichern (verwaltet von Azure Data Factory) Vorhersagen empfangen Azure Machine Learning-Dienst, der im [Power BI](https://powerbi.microsoft.com) -Dashboard verwendet werden.

## <a name="data-consumption"></a>**Datenverbrauch**

### <a name="power-bi"></a>Power BI

Der [Power BI](https://powerbi.microsoft.com) -Dienst zur Dashboard anzeigen, enthält Aggregationen Alerts [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -Dienst sowie RUL Vorhersagen in [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) gespeichert, die mit der [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -Dienst erstellt wurden. Hinweise zu Power BI-Dashboard für diese Lösungsvorlage finden Sie in Abschnitt.

## <a name="how-to-bring-in-your-own-data"></a>**So bringen Sie Ihre Daten**

Dieser Abschnitt beschreibt, wie Azure eigene Daten zu und welchen müssten ändert die Daten in dieser Architektur bringen.

Es ist unwahrscheinlich, dass alle Dataset bringen Sie anhand dieser Lösungsvorlage zum [Turbofan Engine Beeinträchtigung Simulation DataSet](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) Dataset entsprechen. Verständnis Ihrer Daten und den Vorschriften ist werden entscheidend wie die Vorlage mit Ihren eigenen Daten zu ändern. Ist dies der Exposition Azure Machine Learning-Service erhalten Sie eine Einführung, anhand des Beispiels im [ersten Test erstellen](machine-learning-create-experiment.md).

In den folgenden Abschnitten besprechen die Abschnitte der Vorlage, die Änderungen erfordern ein neues Dataset eingeführt wird.

### <a name="azure-event-hub"></a>Azure Event Hub

Azure Event Hub Service ist sehr allgemeine Daten an den Hub im Format CSV oder JSON gebucht werden können. Keine besondere Verarbeitung tritt in Azure Event Hub, aber es ist wichtig, dass Sie die Daten, die es zugeführt.

Dieses Dokument wird beschrieben, wie Daten aufnehmen, aber Sie können senden Ereignisse oder an einen Hub Azure-Ereignis mit der Ereignis-Hub-API.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

Azure Stream Analytics-Service dient zur in Echtzeit-Analysen von Daten lesen und Ausgeben von Daten verschiedener Quellen.

Für die vorbeugende Wartung für Luft-Lösungsvorlage besteht aus vier Unterabfragen jedes verwendeten Ereignisse Azure Event Hub-Dienst, und Ausgaben an vier verschiedenen Orten Azure Stream Analytics-Abfrage. Diese Ausgaben bestehen aus drei Power BI Datasets und einem Azure-Speicherort.

Azure Stream Analytics Abfrage finden:

-   Azure-Portal anmelden

-   Stream Analytics-Aufträge suchen ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) , die beim Bereitstellen der Projektmappe generiert wurden (*z. B.* **maintenancesa02asapbi** und **maintenancesa02asablob** für die vorbeugende wartungslösung)

-   Auswählen

    -   ***EINGABEN*** für die Eingabe der Abfrage anzeigen

    -   ***Abfrage*** die Abfrage anzeigen

    -   ***Gibt*** verschiedenen Ausgaben anzeigen

Informationen zum Erstellen der Abfrage Azure Stream Analytics finden [Stream Analytics Abfrage Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) auf MSDN.

In dieser Lösung Ausgabe Abfragen drei Datasets mit nahe Echtzeitanalysen Informationen über den eingehenden Datenstrom zu einem Power BI-Dashboard, die als Teil dieser Lösungsvorlage bereitgestellt wird. Da implizite Kenntnisse über eingehende Datenformat ist muss diese Abfragen geändert werden, auf das Datenformat.

Die Abfrage in der zweiten Stream Analytics Auftrag **maintenancesa02asablob** gibt einfach alle [Ereignis-Hub](https://azure.microsoft.com/services/event-hubs/) Ereignisse in [Azure Storage](https://azure.microsoft.com/services/storage/) und erfordert daher keine Änderung unabhängig von Ihrem Datenformat wie die vollständigen Informationen in den Speicher übertragen.

### <a name="azure-data-factory"></a>Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) -Dienst koordiniert Bewegung und Verarbeitung der Daten. Vorbeugende Wartung für Luft-Lösungsvorlage Data Factory setzt sich aus drei [Rohrleitungen](../data-factory/data-factory-create-pipelines.md) verschieben und die Daten mit verschiedenen Technologien.  Zugriff Werk Daten durch Öffnen der Data Factory Knoten am unteren Rand der Lösung Vorlage Diagramm erstellt, mit der Bereitstellung der Lösung. Dadurch gelangen Sie zur Data Factory Azure-Portal. Wenn Fehler unter Ihren Datensätzen angezeigt wird, können Sie die durch Bereitstellung vor der Datengenerator Daten Factory sind ignorieren. Diese Fehler verhindern Daten Werk nicht funktionieren.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Dieser Abschnitt beschreibt die erforderlichen [Rohrleitungen](../data-factory/data-factory-create-pipelines.md) und [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)enthaltenen [Aktivitäten](../data-factory/data-factory-create-pipelines.md) . Es folgt die Diagrammansicht der Lösung.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Zwei Rohrleitungen dieser Factory enthalten [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Skripts, die und der Daten verwendet werden. Wenn angegeben, die Skripts während der Installation erstellt [Azure Storage](https://azure.microsoft.com/services/storage/) -Konto befinden. Ihr Standort: Maintenancesascript\\\\Skript\\\\Struktur\\ \\ (oder https://[Your Lösung name].blob.core.windows.net/maintenancesascript).

Wie Abfragen [Azure Stream Analytics](#azure-stream-analytics-1) Skripts [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) haben implizites Wissen über eingehende Datenformat, diese Abfragen müssen geändert werden, auf Ihre Daten und die [Featureentwicklung](machine-learning-feature-selection-and-engineering.md) Anforderungen.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Diese [Pipeline](../data-factory/data-factory-create-pipelines.md) enthält eine einzelne Aktivität - eine [HDInsightHive](../data-factory/data-factory-hive-activity.md) mit einer [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , die ein Skript [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) zum Partitionieren der Daten in [Azure Storage](https://azure.microsoft.com/services/storage/) in [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -Auftrag ausgeführt wird.

Das Skript [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) dazu Partitionierung ist ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Diese [Pipeline](../data-factory/data-factory-create-pipelines.md) enthält mehrere Aktivitäten und dieser Lösungsvorlage zugeordnet ist, dessen Ergebnis erzielten Vorhersagen von [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) experimentieren.

Die Aktivitäten in diesem enthalten sind:

-   [HDInsightHive](../data-factory/data-factory-hive-activity.md) Aktivitäten mithilfe einer [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , die eine [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Skript Aggregationen und Featureentwicklung für [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -Experiment ausführt.
    Das Skript [Struktur](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) Partitionierung hierfür ist ***PrepareMLInput.hql***.

-   [Kopieren](https://msdn.microsoft.com/library/azure/dn835035.aspx) -Aktivität, die die Ergebnisse aus der [HDInsightHive](../data-factory/data-factory-hive-activity.md) an ein einzelnes Blob [Azure-Speicher](https://azure.microsoft.com/services/storage/) , der Zugriff durch die [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -Aktivität.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -Aktivität, die [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) ruft experimentieren, die Ergebnisse in die Ergebnisse in ein einzelnes Blob [Azure-Speicher](https://azure.microsoft.com/services/storage/) abgelegt.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Diese [Pipeline](../data-factory/data-factory-create-pipelines.md) enthält eine einzelne Aktivität - eine [Kopie](https://msdn.microsoft.com/library/azure/dn835035.aspx) , die verschiebt experimentieren [Azure maschinelles lernen](#azure-machine-learning) von ***MLScoringPipeline*** in [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) , die als Teil der Installation der Lösung Vorlage bereitgestellt wurde.

### <a name="azure-machine-learning"></a>Azure maschinelles lernen

Für diese Lösungsvorlage verwendete [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) -Experiment bietet die verbleibenden nützliche Leben (RUL) eines Luftfahrzeugs Motors. Das Experiment ist die Datengruppe verwendet und benötigen daher Änderung oder Ersetzung für Daten geschaltet werden.

Weitere Informationen zur Erstellung des Experiments Azure Machine Learning [vorbeugende Wartung: Schritt 1 von 3 Vorbereitung und Featureentwicklung](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Überwachen des Fortschritts**
 Nach dem Start der Datengenerator Pipeline beginnt hydratisiert erhalten und die verschiedenen Komponenten der Lösung starten treten in Aktion folgende Befehle von der ausgestellt. Gibt es zwei Arten die Pipeline zu überwachen.

1. Der Stream Analytics-Auftrags schreibt eingehenden Rohdaten Blob-Speicher. Wenn Sie BLOB-Speicher-Komponente der Projektmappe vom Bildschirm Sie erfolgreich auf die Lösung klicken im rechten Bereich und dem [Verwaltungsportal](https://portal.azure.com/)benötigen. Einmal, klicken Sie auf Blobs. Im nächsten Fenster sehen Sie eine Liste der Container. Klicken Sie auf **Maintenancesadata**. Im nächsten Fenster sehen Sie den **Rohdaten** -Ordner. Ordner Rawdata sehen Sie Ordner mit dem Namen Stunde = 17 Stunden = 18 usw.. Wenn diese Ordner angezeigt, es gibt an, dass die Rohdaten erfolgreich auf Ihrem Computer generiert und im BLOB-Speicher gespeichert. CSV-Dateien, die begrenzte Größe in MB in diesen Ordnern müssen, sollte angezeigt werden.

2. Der letzte Schritt der Pipeline werden Daten (z. B. Vorhersagen aus Computer) in SQL-Datenbank geschrieben. Sie müssen drei Stunden für die Daten in SQL-Datenbank warten. Über [Azure-Portal](https://manage.windowsazure.com/)ist eine Möglichkeit zum Überwachen, wie viele Daten in der SQL-Datenbank verfügbar ist. Suchen Sie im linken Bereich SQL Datenbanken ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) und klicken Sie darauf. Suchen Sie die Datenbank **Pmaintenancedb** und klicken Sie darauf. Klicken Sie auf der nächsten Seite unten auf Verwalten

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Hier können Sie neue Abfrage und Abfrage für die Anzahl der Zeilen (z.B. select count(*) aus PMResult) klicken. Wenn die Datenbank wächst, sollte die Anzahl der Zeilen in der Tabelle erhöhen.


## <a name="power-bi-dashboard"></a>**Power BI-Dashboard**

### <a name="overview"></a>Übersicht

Dieser Abschnitt beschreibt Power BI-Dashboard einrichten, um Ihre Echtzeit Daten von Azure Stream Analytics (Langsamster Pfad) sowie Batch Vorhersageergebnisse aus Azure maschinelles lernen (cold Pfad).

### <a name="setup-cold-path-dashboard"></a>Kalte Pfad Dashboard einrichten

In Datenpipeline kalt Pfad ist das grundlegende Ziel zu vorhersehbaren RUL (Verbleibende Nutzungsdauer) jedes Luftfahrzeug Motor einen Flug (Cycle) abgeschlossen. Die Vorhersage wird aktualisiert alle 3 Stunden Vorhersage Flugzeugmotoren, die einen Flug in den letzten 3 Stunden abgeschlossen haben.

Power BI verbindet mit einer Azure SQL-Datenbank als Datenquelle, wo die Vorhersageergebnisse gespeichert sind. Hinweis: 1) beim Bereitstellen der Lösung eine echte Vorhersage in der Datenbank innerhalb von 3 Stunden erscheint.
Die Pbix-Datei, die mit den Generator Download enthält seedingdaten, damit Sie sofort das Power BI-Dashboard erstellen. (2) in diesem Schritt ist die Voraussetzung zum Herunterladen und Installieren von kostenlosen Software [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Die folgenden Schritte führen Sie zum Zeitpunkt der Bereitstellung der Lösung mit Daten (*z. B.*. erstellt wurde der SQL-Datenbank Pbix Datei herstellen Vorhersageergebnisse) zur Visualisierung.

1.  Erhalten Sie die Anmeldeinformationen.

    **Servername, Datenbankname, Benutzername und Kennwort** benötigen vor dem Verschieben in den nächsten Schritten. Hier sind die Schritte, wie Sie diese führt.

    -   Nach Grün **"Azure SQL-Datenbank"** im Diagramm Vorlage Lösung klicken sie und klicken Sie dann auf **"Öffnen"**.

    -   Sie sehen ein neue Registerkarte/Browserfenster Azure Portalseite angezeigt. Klicken Sie im linken Bereich auf **"Ressourcengruppen"** .

    -   Abonnement verwenden für die Bereitstellung der Lösung, und wählen Sie dann **' YourSolutionName\_ResourceGroup'**.

    -   In den neuen Pop, Bereich, klicken Sie auf die ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) Symbol auf die Datenbank zugreifen. Der Datenbankname ist neben diesem Symbol (*z.*B. **"Pmaintenancedb"**) den **Namen des Datenbankservers** unter Server Name-Eigenschaft aufgeführt und sollte folgendermaßen aussehen **YourSoutionName.database.windows.net**.

    -   Datenbank- **Benutzername** und **Kennwort** entsprechen dem Benutzernamen und Kennwort problemlos während der Bereitstellung der Lösung.

2.  Aktualisieren Sie die Datenquelle des Berichts kalt Pfad mit Power BI Desktop.

    -   Doppelklicken Sie im Ordner auf dem PC Sie heruntergeladen und entpackt Generator-Datei der **PowerBI\\PredictiveMaintenanceAerospace.pbix** Datei. Beim Öffnen der Datei alle Warnhinweise angezeigt wird, ignorieren. Klicken Sie auf die Datei **"Abfragen bearbeiten"**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Sie sehen zwei Tabellen **RemainingUsefulLife** und **PMResult**. Wählen Sie die erste Tabelle, und klicken Sie auf ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) neben **"Source"** unter **"Schritte angewendet"** auf der rechten Seite der **Abfrage Settings "** . Ignorieren Sie alle Warnhinweise.

    -   Ersetzen Sie im Fenster Pop **'Server'** und **'Database'** eigene Server- und Datenbanknamen und klicken Sie dann auf **"OK"**. Servername sicherstellen Sie, dass Sie den Port 1433 (**YourSoutionName.database.windows.net, 1433**) angeben. Lassen Sie das Feld Datenbank als **Pmaintenancedb**. Ignorieren der Warnhinweise auf dem Bildschirm.

    -   Im nächsten Pop Fenster sehen Sie zwei Optionen im linken Fensterbereich (**Windows** und **Datenbank**). Klicken Sie auf **'Datenbank'**, **'Benutzername'** und **'Kennwort'** (Dies ist Benutzernamen und Passwort bei der Lösung und eine SQL Azure-Datenbank erstellt) ausfüllen. ***Wählen Sie die anzuwendende diese Einstellungen***aus Datenbank einchecken. Klicken Sie auf **'Verbinden'**.

    -   In der zweiten Tabelle **PMResult** klicken Sie auf ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) neben **"Source"** unter **"Schritte angewendet"** auf der rechten Seite **"Abfrage Settings"** und aktualisieren Sie die Server- und Namen wie die oben aufgeführten Schritte und klicken Sie auf OK.

    -   Wenn Sie zur vorherigen Seite geführt, schließen Sie das Fenster. Eine Meldung out - klicken Sie auf **Übernehmen**. Abschließend klicken Sie auf **Speichern** , um die Informationen zu speichern. Die Power BI-Datei wurde nun Verbindung zum Server hergestellt. Wenn Ihre Visualisierung leer sind, sicherzustellen Sie, dass der Auswahl auf die Visualisierung alle Daten grafisch Symbol in der oberen rechten Ecke der Legenden Radierer löschen. Verwenden Sie die Aktualisierungsschaltfläche entsprechend neue Daten auf die Visualisierung. Zunächst sehen Sie nur die Ausgangswerte für die Visualisierung wie Data Factory alle 3 Stunden aktualisieren soll. Nach 3 Stunden sehen Sie neue Vorhersagen in die Visualisierung übernommen, wenn Sie die Daten aktualisieren.

3.  (Optional) Veröffentlichen Sie [Power BI online](http://www.powerbi.com/)cold Pfad Dashboard. Beachten Sie, dass dieser Schritt Power BI-Konto (oder Office 365-Konto).

    -   Klicken Sie auf **'Veröffentlichen'** , einige Sekunden später ein Fenster anzeigen "Publishing Power BI Erfolg!" mit einem grünen Häkchen. Klicken Sie auf den Link "Open PredictiveMaintenanceAerospace.pbix in Power BI". Ausführliche Informationen finden Sie unter [Veröffentlichen von Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Ein neues Dashboard erstellen: Klicken Sie auf die **+** neben der Bereich **Dashboards** im linken Bereich. Geben Sie den Namen "Vorbeugende Wartung Demo" das neue Dashboard.

    -   Wenn Sie den Bericht zu öffnen, klicken Sie auf ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) um die Visualisierung zu Ihrem Dashboard zu fixieren. Ausführliche Informationen finden Sie unter [Pin einer Kachel ein Power BI-Dashboard aus einem Bericht](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Besuchen Sie die Dashboardseite und passen Sie die Größe und Position der Visualisierung und bearbeiten Sie Titel. Ausführliche Informationen zum Bearbeiten der Kacheln finden Sie unter [Bearbeiten einer Kachel - Größe ändern, verschieben, umbenennen, Pin löschen, Hyperlink hinzufügen](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Hier ist ein Beispieldashboard mit einige kalte Pfad-Visualisierung, fixiert.  Je nachdem wie lange Sie den Datengenerator ausführen können die Zahlen auf der Visualisierung abweichen.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Um Ablaufplan Aktualisierung der Mauszeiger über das **PredictiveMaintenanceAerospace** -Dataset, klicken Sie auf ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) und klicken Sie dann auf **Zeitplan aktualisieren**.
<br/>
        **Hinweis:** Warnung Massage, auf **Anmeldeinformationen bearbeiten** angezeigt entsprechen die Anmeldeinformationen wie in Schritt 1 beschrieben.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Erweitern Sie den Abschnitt **Zeitplan aktualisieren** . Aktivieren Sie "Daten Stand".
    <br/>
    -   Planen Sie Ihren Bedürfnissen entsprechend zu aktualisieren. Weitere Informationen finden Sie unter [Daten in Power BI aktualisieren](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Langsamster Pfad Dashboard einrichten

Die folgenden Schritte führen Sie wie Echtzeit Datenausgabe Stream Analytics-Aufträge anzeigen, die zum Zeitpunkt der Bereitstellung der Lösung erstellt wurden. Die folgenden Schritte ist ein [Power BI online](http://www.powerbi.com/) -Konto erforderlich. Wenn Sie ein Konto haben, können Sie [eine erstellen](https://powerbi.microsoft.com/pricing).

1.  Power BI-Ausgabe in Azure Stream Analytics (ASA) hinzufügen.

    -  Müssen die Anweisungen in [Azure Stream Analytics & Power BI: ein Dashboard Echtzeitanalysen Echtzeitanzeige von Datenströmen](stream-analytics-power-bi-dashboard.md) die Ausgabe des Projekts Azure Stream Analytics als Power BI-Dashboard einrichten.
    - ASA-Abfrage hat drei Ausgaben sind **Aircraftmonitor**, **Aircraftalert**und **Flightsbyhour**. Sie können die Abfrage auf der Registerkarte Abfrage anzeigen. Für jede dieser Tabellen müssen Sie ASA Ausgabe hinzufügen. Fügen Sie die erste Ausgabe (*z.B.* **Aircraftmonitor**) Achten Sie sind **Ausgabealias**, der **Dataset-Name** und der **Name** der gleichen (**Aircraftmonitor**). Wiederholen Sie die Schritte für Ausgaben für **Aircraftalert**und **Flightsbyhour**. Sobald Sie alle drei Tabellen ausgeben und ASA-Auftrag gestartet hinzugefügt haben, erhalten Sie eine Bestätigungsnachricht (*z.*B. "Start Stream Analytics Auftrag maintenancesa02asapbi erfolgreich").

2. Melden Sie sich [online Power BI](http://www.powerbi.com)

    -   Im linken Abschnitt von Datasets im Arbeitsbereich erscheint die ***DATASET*** -Namen **Aircraftmonitor**, **Aircraftalert**und **Flightsbyhour** . Dies ist die Daten, die im vorherigen Schritt von Azure Stream Analytics geschoben. Dataset- **Flightsbyhour** möglicherweise nicht beide Datasets aufgrund der SQL-Abfrage dahinter gleichzeitig angezeigt. Allerdings sollten sie nach einer Stunde angezeigt.
    -   Vergewissern Sie sich im Bereich ***Visualisierung*** ist geöffnet und auf der rechten Seite des Bildschirms angezeigt.

3. Haben Sie die Daten in Power BI, können Sie die Visualisierung der Daten starten. Unten ist eine Beispieldashboard mit einigen Langsamster Pfad-Visualisierung, fixiert. Sie können andere Dashboards Fliesen auf entsprechende Datensätze erstellen. Je nachdem wie lange Sie den Datengenerator ausführen können die Zahlen auf der Visualisierung abweichen.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Hier sind einige Schritte zur Erstellung eines der oben genannten Felder die "Flotte Ansicht der Sensor 11 und Schwellenwert 48,26" nebeneinander:

    -   Klicken Sie auf Dataset **Aircraftmonitor** auf der linken Abschnitt des Datasets.

    -   Klicken Sie auf **Diagramm** .

    -   Klicken Sie im Bereich **Felder** **verarbeitet** , sodass unter "Achse" im Bereich **Visualisierung** zeigt.

    -   Klicken Sie auf "s11" und "s11\_Alert", damit sie unter "Werte" angezeigt. Klicken Sie auf den kleinen Pfeil neben **s11** und **s11\_Warnung**, "Summe", "Durchschnitt" ändern.

    -   **Klicken Sie auf oben** und Namen für den Bericht "Aircraftmonitor". Der Bericht mit dem Namen "Aircraftmonitor" wird im Abschnitt **Berichte** im **Bereich auf der linken Seite** angezeigt.

    -   Klicken Sie in der oberen rechten Ecke dieses Diagramm auf **Pin Visual** . Ein "Pin-Dashboard" können Sie ein Dashboard auswählen angezeigt. "Vorbeugende Wartung Demo", klicken Sie auf "Pin".

    -   Bewegen Sie die Maus über diese Kachel auf dem Dashboard, klicken Sie auf das Symbol "Bearbeiten" in der oberen rechten Ecke auf "Flotte Ansicht der Sensor 11 und Schwellenwert 48,26" Titel und Untertitel "Durchschnitt Flotte langfristig" ändern.

## <a name="how-to-delete-your-solution"></a>**Ihre Lösung löschen**
Stellen Sie sicher, den Datengenerator mit höhere Kosten entstehen nicht die Lösung mit den Datengenerator zu beenden. Löschen Sie die Lösung, wenn Sie es nicht verwenden. Ihre Lösung löschen werden alle Komponenten, die in Ihrem Abonnement bereitgestellt, wenn die Lösung bereitgestellt. Lösung auf den Namen Ihrer Projektmappe im linken Bereich der Projektmappenvorlage löschen, und klicken auf Löschen.

## <a name="cost-estimation-tools"></a>**Vorkalkulation tools**

Die folgenden beiden Tools sind verfügbar, können Sie besser verstehen, die gesamten Kosten vorbeugende Wartung für Luft-und Projektmappenvorlage in Ihrem Abonnement:

-   [Microsoft Azure Kosten Estimator Tool (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure Kosten Estimator Tool (Desktop)](http://www.microsoft.com/download/details.aspx?id=43376)
