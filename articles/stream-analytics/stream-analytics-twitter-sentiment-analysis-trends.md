<properties
    pageTitle="Twitter Stimmung Echtzeitanalyse Stream Analytics | Microsoft Azure"
    description="Informationen Sie zum Stream Analytics Echtzeit Twitter stimmungsanalyse verwenden. Schritt für Schritt Generierung von Daten auf einem aktuellen Dashboard."
    keywords="Trendanalyse Echtzeit Twitter stimmungsanalyse social Media Analyse, Trend-Beispiel"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="social-media-analysis-real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Soziale medienanalyse: Echtzeit Twitter stimmungsanalyse in Azure Stream Analytics

Erfahren Sie, wie eine Stimmung Lösung für social Media Analysen mit Twitter Ereignisse in Echtzeit zu Azure Ereignis erstellen. Schreiben Sie eine Azure Stream Analytics Abfrage um die Daten zu analysieren. Sie müssen dann die Ergebnisse zur späteren Durchsicht oder Dashboard und [Power BI](https://powerbi.com/) Einblicke in Echtzeit verwenden.

Soziale Medien helfen Analytics Organisationen verstehen Trends, Themen und Einstellungen, die eine große Anzahl von Beiträgen in sozialen Medien. Stimmungsanalyse auch *Stellungnahme Mining*bezeichnet wird, wird mit Analysetools für soziale Medien Haltung gegenüber einem Produkt Idee, und bestimmt. Echtzeit Twitter-Trendanalyse ist ein gutes Beispiel, da das Hashtag Abonnement ermöglicht Ihnen bestimmte Schlüsselwörter und Stimmung Analyse der Feed.

## <a name="scenario-sentiment-analysis-in-real-time"></a>Szenario: Stimmungsanalyse in Echtzeit

Ein Unternehmen, das eine Medien-Website ist interessiert einen Vorteil gegenüber seinen Mitbewerbern Websiteinhalt sofort für Leser mit. Soziale medienanalyse verwendet auf Themen für Leser mit Echtzeit-stimmungsanalyse auf Twitter Daten. Insbesondere benötigt das Unternehmen zum Identifizieren von Trends auf Twitter Echtzeitanalysen über Tweet Volumen und Stimmung für wichtige Themen. Ist im Grunde genommen müssen Analysemodul Analytics ein Gefühl, das auf dieser social Media feed.

## <a name="prerequisites"></a>Erforderliche Komponenten
-   Twitter-Konto und [OAuth-Zugriffstoken](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)
-   [TwitterClient.zip](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip) aus dem Microsoft Download Center
-   Optional: Quellcode für Twitter-Client von [GitHub](https://aka.ms/azure-stream-analytics-twitterclient)

## <a name="create-an-event-hub-input-and-a-consumer-group"></a>Erstellen Sie einen Ereignis-Hub eingeben und eine

Beispiel-Anwendung Ereignisse generieren und ein Ereignis Hubs Instanz (kurz ein Ereignis-Hub) drücken. Service Bus Ereignis Hubs sind die bevorzugte Methode der Einnahme für Stream Analytics Ereignis. Ereignis-Hubs Dokumentation in [Service Bus-Dokumentation](/documentation/services/service-bus/).

Gehen Sie zu einem Ereignis-Hub.

1.  Klicken Sie im Azure-Portal **neu** > **Anwendungsdienste** > **SERVICE BUS** > **EVENT HUB** > **Schnell erstellen**und Name, Region und neuen oder vorhandenen Namespace.  
2.  Als bewährte Methode sollte ein einzelnes Ereignis Hubs Verbrauchergruppe Auftrags Stream Analytics gelesen. Wir gehen Sie durch eine später erstellen. Sie können Verbrauchergruppen [Azure Ereignis Hubs](https://msdn.microsoft.com/library/azure/dn836025.aspx)Übersicht erfahren. Erstellen Sie eine gehen Sie an den Hub neu erstellte Ereignis die Registerkarte **VERBRAUCHERGRUPPEN** und auf **Erstellen** am Ende der Seite Geben Sie einen Namen für Ihre consumergruppe.
3.  Um Ereignis-Hub gewähren, müssen wir einen freigegebenen Richtlinien erstellen. Klicken Sie auf die Registerkarte **Konfigurieren** des Haupt-Ereignis.
4.  Erstellen Sie unter **Gemeinsame Richtlinien**eine neue Richtlinie mit Berechtigungen **Verwalten** .

    ![Freigegebene Zugriffsrichtlinien, einer Richtlinie mit Berechtigungen verwalten erstellen.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-ananlytics-shared-access-policies.png)

5.  Klicken Sie am unteren Rand der Seite **Speichern** .
6.  Gehe zu **DASHBOARDS**, klicken Sie auf **Informationen** am unteren Rand der Seite kopieren und Speichern der Verbindungsinformationen. (Verwenden Sie das Kopiersymbol, das unter das Symbol angezeigt wird.)

## <a name="configure-and-start-the-twitter-client-application"></a>Konfigurieren Sie und starten Sie die Clientanwendung Twitter

Wir haben eine Clientanwendung bereitgestellt, die mit Twitter [Twitter Streaming-APIs](https://dev.twitter.com/streaming/overview) parametrisierte Themengruppe Tweet Ereignisse sammeln Daten verbinden. [Sentiment140](http://help.sentiment140.com/) open-Source-Tool wird verwendet, um jeden Tweet Stimmung Wert zuweisen.

- 0 = negativ
- 2 = Neutral
- 4 = positiv

Tweet Ereignisse werden dann an den Hub Ereignis abgelegt.  

Führen Sie diese Schritte zum Einrichten der Anwendung:

1.  [TwitterClient-Lösung herunterladen](http://download.microsoft.com/download/1/7/4/1744EE47-63D0-4B9D-9ECF-E379D15F4586/TwitterClient.zip).
2.  Öffnen Sie TwitterClient.exe.config und Twitter-Token, die die Werte ersetzen Sie Oauth_consumer_key, Oauth_consumer_secret, Oauth_token und Oauth_token_secret.  

    [Schritte zum OAuth-Zugriffstoken generieren](https://dev.twitter.com/oauth/overview/application-owner-access-tokens)  

    Beachten Sie, dass eine leere Anwendung einen Token zu generieren.  
3.  Ersetzen Sie die Werte EventHubConnectionString und EventHubName in TwitterClient.exe.config durch die Verbindungszeichenfolge und den Namen des Haupt-Ereignis. Die Verbindungszeichenfolge, die Sie zuvor kopiert, können Sie die Verbindungszeichenfolge und den Namen des Haupt-Ereignis, werden zu trennen Sie diese jeweils in das richtige Feld. Betrachten Sie beispielsweise die folgende Verbindungszeichenfolge:

        Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey;EntityPath=yourhub

    Die TwitterClient.exe.config-Datei sollte Einstellungen wie im folgenden Beispiel:

        add key="EventHubConnectionString" value="Endpoint=sb://your.servicebus.windows.net/;SharedAccessKeyName=yourpolicy;SharedAccessKey=yoursharedaccesskey"
        add key="EventHubName" value="yourhub"

    Es ist wichtig zu beachten, dass den Text "EntityPath =" ist __nicht__ in der EventHubName-Wert angezeigt.

4.  *Optional:* Passen Sie die Schlüsselwörter für die Suche.  Standardmäßig sucht diese Anwendung "Azure Skype, XBox, Microsoft, Seattle".  Sie können die Werte für **Twitter_keywords** in TwitterClient.exe.config, ggf. anpassen.
5.  TwitterClient.exe zum Starten der Anwendung ausgeführt. Sie sehen Tweet Ereignisse mit den Ereignis-Hub an **CreatedAt**, **Thema**und **SentimentScore** .

    ![Stimmungsanalyse: SentimentScore-Werte mit einem Ereignis-Hub gesendet.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-sentiment-output-to-event-hub.png)

## <a name="create-a-stream-analytics-job"></a>Erstellen Sie einen Stream Analytics-Auftrag

Nachdem die Tweet Ereignisse in Echtzeit Twitter streamen, können wir einen Stream Analytics-Auftrag einrichten dieser Ereignisse in Echtzeit analysiert.

### <a name="provision-a-stream-analytics-job"></a>Bereitstellung eines Stream Analytics-Auftrags

1.  Klicken Sie im [Azure-Portal](https://manage.windowsazure.com/) **neu** > **DATA SERVICES** > **STREAM ANALYTICS** > **Schnell erstellen**.
2.  Geben Sie die folgenden Werte und dann auf **STREAM ANALYTICS-Auftrag erstellen**:

    * **AUFTRAGSNAME**: Geben Sie einen Auftragsnamen ein.
    * **REGION**: Markieren Sie den Bereich der Auftrag ausgeführt werden soll. Sollten Sie den Auftrag und dem Ereignis in derselben Region eine bessere Leistung zu gewährleisten und sicherzustellen, dass Sie keine zur Datenübertragung zwischen Zahlen.
    * **Konto**: Wählen Sie die Option der Azure-Speicher, die Daten für alle Stream Analytics speichern, die innerhalb dieses Bereichs ausgeführt werden soll. Sie können ein vorhandenes Speicherkonto auswählen oder eine neue zu erstellen.

3.  Klicken Sie im linken Bereich der Stream Analytics-Aufträge auflisten **STREAM ANALYTICS** auf.  
    ![Stream Analytics Service-Symbol](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-service-icon.png)

    Das neue Projekt erscheint mit dem Status **erstellt**. Beachten Sie, dass am Ende der Seite auf die Schaltfläche **START** deaktiviert ist. Sie müssen zunächst den Auftrag Auftrag Eingabe-, Ausgabe- und Abfrage konfigurieren.


### <a name="specify-job-input"></a>Job-Eingabe angeben
1.  In Ihrem Auftrag Stream Analytics auf **EINGABEN** vom oberen Rand der Seite und klicken Sie dann auf **Hinzufügen**. Das Dialogfeld führt Sie durch mehrere Schritte, um Ihre Eingabe einzurichten.
2.  Klicken Sie mit der rechte Maustaste auf **DATENSTROM**
3.  Klicken Sie mit der rechte Maustaste auf **EVENT HUB**
4.  Eingeben oder auswählen auf der dritten Seite die folgenden Werte:

    * **INPUT ALIAS**: Geben Sie einen Anzeigenamen für diesen Auftrag eingeben wie *TwitterStream*. Beachten Sie, dass Sie diesen Namen später in der Abfrage verwenden können.
    **Ereignis-HUB**: Ereignis-Hub, der Sie erstellt im gleichen Abonnement als Stream Analytics-Auftrags wählen Sie den Namespace im Ereignis ist.

        Ist Haupt-Ereignis in einem anderen Abonnement auf **Mit Event Hub aus einem anderen Abonnement**und geben Sie Informationen für **SERVICE BUS-NAMESPACE** **HUB EREIGNISNAMEN**, **Ereignis HUB RICHTLINIENNAME**, **Ereignis HUB RICHTLINIENSCHLÜSSEL**und **EREIGNISANZAHL HUB PARTITION**manuell ein.

    * **HUB EREIGNISNAME**: Wählen Sie Ereignis-Hub.

    * **Ereignis HUB RICHTLINIENNAME**: Wählen Sie Ereignis-Hub, der zuvor in diesem Lernprogramm erstellt.

    * **HUB CONSUMER Gruppe**: Geben Sie den Namen der Verbrauchergruppe, die zuvor in diesem Lernprogramm erstellt.
5.  Klicken Sie auf die rechte Maustaste.
6.  Geben Sie die folgenden Werte:

    * **Ereignis SERIALISIERUNGSPROGRAMM FORMAT**: JSON
    * **Codierung**: UTF8

7.  Klicken Sie **Überprüfen** diese Quelle hinzufügen und überprüfen, ob der Stream Analytics Ereignis-Hub herstellen können.

### <a name="specify-job-query"></a>Job-Abfrage

Stream Analytics unterstützt einen einfachen, deklarativen Query-Modell, die Transformationen beschreibt. Weitere Informationen über die Sprache finden Sie unter [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Dieses Lernprogramm hilft Ihnen erstellen und Testen mehrere Abfragen über Twitter-Daten.

#### <a name="sample-data-input"></a>Beispiel-Eingabe

Überprüfen Sie Ihre Abfrage aktuelle Daten können Sie **BEISPIELDATEN** Funktion Ereignisse aus der Stream und eine JSON Ereignisse zu Testzwecken erstellen.

1.  Wählen Sie der Hub Ereigniseingang aus und dann auf **BEISPIELDATEN** am unteren Rand der Seite.
2.  Geben Sie im angezeigten Dialogfeld **STARTZEIT** um sammeln Daten und eine **Dauer** für wie viele zusätzlichen Daten verarbeiten.
3.  Klicken Sie auf die Schaltfläche **DETAILS** , und klicken Sie dann auf den Link **Klicken Sie hier** zum Herunterladen und speichern die Datei generierten JSON.

#### <a name="pass-through-query"></a>Pass-Through-Abfrage
Um zu machen einer einfachen Pass-Through-Abfrage, projiziert die Felder eines Ereignisses wir.

1.  Klicken Sie auf **Abfrage** am oberen Seitenrand Job Stream Analytics.
2.  Ersetzen Sie im Code-Editor ursprünglichen Abfragevorlage Folgendes:

        SELECT * FROM TwitterStream

    Stellen Sie sicher, dass der Name der Datenquelle mit dem Namen der Eingabe übereinstimmt, den Sie zuvor angegeben.

3.  Klicken Sie unter Abfrage-Editor **Testen** .
4.  Gehen Sie zu der JSON-Beispieldatei.
5.  Klicken Sie auf die Schaltfläche **Suchen** , und die Ergebnisse unterhalb der Abfragedefinition.

    ![Ergebnisse unter Abfragedefinition](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sentiment-by-topic.png)

#### <a name="count-of-tweets-by-topic-tumbling-window-with-aggregation"></a>Anzahl der Tweets nach Thema: Tumbling Fenster mit

Um die Anzahl der Vorkommnisse zu Themen zu vergleichen, verwenden wir [TumblingWindow](https://msdn.microsoft.com/library/azure/dn835055.aspx) zu der Anzahl der Vorkommnisse von Thema alle fünf Sekunden.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT System.Timestamp as Time, Topic, COUNT(*)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

    Diese Abfrage verwendet das Schlüsselwort **Zeitstempel von** an ein Timestamp-Feld in der Nutzlast in die zeitliche Berechnung verwendet werden. Wenn dieses Feld nicht angegeben wurde, würde Windowing-Vorgang ausgeführt werden, mithilfe der Zeit, die jedes Ereignis Event Hub eingetroffen.  Erfahren Sie mehr im Abschnitt "Zeit Vs Anwendung Ankunftszeit" [Stream Analytics Abfrage Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Diese Abfrage greift auch einen Zeitstempel für das Ende jedes Fenster mithilfe der **System.Timestamp** -Eigenschaft auf.

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

#### <a name="identify-trending-topics-sliding-window"></a>Trends erkennen: Schiebefenster

Um Trends zu identifizieren, suchen wir nach Themen, die einen Schwellenwert für erwähnt in einem bestimmten Zeitraum gesendet. Für die Zwecke dieses Lernprogramms überprüfen wir Themen, die mehr als 20 Mal in den letzten fünf Sekunden mit einem [SlidingWindow](https://msdn.microsoft.com/library/azure/dn835051.aspx)genannt sind.

1.  Ändern Sie die Abfrage in den Code-Editor: Wählen Sie System.Timestamp Zeit, Thema, COUNT (*) als erwähnt aus TwitterStream Zeitstempel von CreatedAt Gruppe von SLIDINGWINDOW(s, 5), Thema zählen müssen (*) > 20

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .

    ![Gleitende Fenster Abfrageergebnis](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-query-output.png)

#### <a name="count-of-mentions-and-sentiment-tumbling-window-with-aggregation"></a>Anzahl der Vorkommnisse und Stimmung: Tumbling Fenster mit
Die letzte Abfrage wir Testen verwendet **TumblingWindow** zu Nummer erwähnt Mittelwert, Minimum, Maximum und Standardabweichung der Stimmung Faktor für jedes Thema alle fünf Sekunden.

1.  Ändern Sie die Abfrage in den Code-Editor:

        SELECT System.Timestamp as Time, Topic, COUNT(*), AVG(SentimentScore), MIN(SentimentScore),
        Max(SentimentScore), STDEV(SentimentScore)
        FROM TwitterStream TIMESTAMP BY CreatedAt
        GROUP BY TUMBLINGWINDOW(s, 5), Topic

2.  Der Abfrage-Editor die Ergebnisse der Abfrage klicken Sie auf **erneut ausführen** .
3.  Dies ist die Abfrage, die wir für unsere Dashboard verwenden.  Klicken Sie am unteren Rand der Seite **Speichern** .


## <a name="create-output-sink"></a>Ausgabe-Senke erstellen

Haben wir eine Ereignisstream ein Ereignis Hub Ereignisse und eine Abfrage über den Stream Umwandlung Aufnahme Eingabe definiert ist der letzte Schritt eine Ausgabe Senke für das Projekt definiert.  Die aggregierten Tweet Ereignisse schreiben aus unserem auftragsabfrage Azure Blob-Speicher.  Sie konnte Ihre Ergebnisse auch Azure SQL-Datenbank, Azure-Tabellenspeicher drücken oder Ereignis Hubs, abhängig von Ihrer Anwendung benötigt.

Gehen Sie folgendermaßen vor um einen Container für BLOB-Speicher erstellen, haben Sie bereits eine:

1.  Ein Speicherkonto verwenden oder ein neues Speicherkonto auf **neu**erstellen > **DATA SERVICES** > **Speicher** > **Schnell erstellen**, und folgen der Anleitung auf dem Bildschirm.
2.  Wählen Sie das Speicherkonto, **Container** am oberen Rand der Seite auf und klicken Sie auf **Hinzufügen**.
3.  Geben Sie einen **Namen** für den Container und auf **Blob für den öffentlichen** **Zugriff** festgelegt.

## <a name="specify-job-output"></a>Auftrag Ausgabe

1.  In Ihrem Auftrag Stream Analytics auf **Ausgabe** am oberen Rand der Seite und klicken Sie dann auf **Ausgabe hinzufügen**. Das Dialogfeld führt Sie durch mehrere Schritte zum Einrichten der Ausgabe.
2.  Klicken Sie mit der rechte Maustaste auf **BLOB-Speicher**
3.  Eingeben oder auswählen auf der dritten Seite die folgenden Werte:

    * **AUSGABEALIAS**: Geben Sie einen Anzeigenamen für diese Auftragsausgabe.

    * **Abonnement**: BLOB-Speicher, die Sie erstellt im gleichen Abonnement als Stream Analytics-Auftrag, klicken Sie auf **Storage-Konto von Abonnements verwenden**. Ist Ihr Speichersystem in einem anderen Abonnement auf **Speicherkonto aus einem anderen Abonnement verwenden**und geben Sie Informationen für das **SPEICHERKONTO** **SPEICHERSCHLÜSSEL Konto**und **CONTAINER**manuell ein.

    * **Konto**: Wählen Sie den Namen des Speicherkontos.

    * **CONTAINER**: Wählen Sie den Namen des Containers.

    * **Präfix für DATEINAMEN**: Geben Sie ein Präfix beim Schreiben von BLOB-Ausgabe verwendet.

4.  Klicken Sie auf die rechte Maustaste.
5.  Geben Sie die folgenden Werte:
    * **Ereignis SERIALISIERUNGSPROGRAMM FORMAT**: JSON
    * **Codierung**: UTF8
6.  Klicken Sie **Überprüfen** diese Quelle hinzufügen und überprüfen, ob der Stream Analytics das Speicherkonto herstellen können.

## <a name="start-job"></a>Auftrag starten

Da ein Auftrag Eingabe, Abfrage und Ausgabe alle angegeben wurden, können wir den Stream Analytics-Auftrag zu starten.

1.  **DASHBOARD**Auftrag klicken Sie am unteren Rand der Seite **Starten** .
2.  Klicken Sie im Dialogfeld, das geöffnet wird auf **Auftrag starten**und klicken Sie am unteren Rand des Dialogfelds auf die Schaltfläche **Suchen** . Der Status wechselt zu **Starten** und ändert in Kürze **ausgeführt**.


## <a name="view-output-for-sentiment-analysis"></a>Ausgabe für stimmungsanalyse anzeigen

Nachdem Ihr Auftrag ausgeführt und in Echtzeit Twitter-Stream verarbeitet, wählen Sie zum Anzeigen der Ausgabe für die stimmungsanalyse. Verwenden Sie ein Tool wie [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/) oder [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) die Auftragsausgabe in Echtzeit anzeigen. [Power BI](https://powerbi.com/) können Sie hier ein angepasstes Dashboard wie im folgenden Screenshot sind die Anwendung erweitern.

![Soziale medienanalyse: Stream Analytics Stimmung (Stellungnahme Mining) Ausgabe der Analyse in einem Power BI-Dashboard.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-output-power-bi.png)

## <a name="get-support"></a>Support erhalten
Versuchen Sie [Azure Stream Analytics Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)für weitere Unterstützung.


## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
