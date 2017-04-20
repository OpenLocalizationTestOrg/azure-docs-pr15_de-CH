<properties
   pageTitle="Apache Storm Topologien mit Visual Studio und C# | Microsoft Azure"
   description="Informationen Sie zum Sturm Topologien in C# erstellen, erstellen Sie eine einfache Word Count Topologie in Visual Studio mit HDInsight-Tools für Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Entwickeln von C# Topologien für Apache Storm auf HDInsight mit Hadoop Tools für Visual Studio

Informationen Sie zum Erstellen einer C# Storm-Topologie mit HDInsight-Tools für Visual Studio. Dieses Lernprogramm führt schrittweise ein neues Storm-Projekt in Visual Studio erstellen, lokal testen und ein Sturm Apache HDInsight Cluster bereitstellen.

Außerdem erfahren, wie Hybridtopologien erstellen, die C# und Java-Komponenten verwenden.

> [AZURE.IMPORTANT] Die Schritte in diesem Dokument in einer windowsumgebung-Entwicklung mit Visual Studio zu verlassen, kann das kompilierte Projekt zu Linux oder Windows-basierten HDInsight Cluster gesendet werden. Linux-basierten Clustern erstellt nur nach 28/10/2016 Unterstützung SCP.NET Topologien.
>
> Um eine C#-Topologie mit Linux-basierten Cluster verwenden, müssen Sie Microsoft.SCP.Net.SDK NuGet-Paket vom Projekt verwendete Version 0.10.0.6 oder höher aktualisieren. Die Version des Pakets muss auch Hauptversion Sturm auf HDInsight installiert. Auf HDInsight Version 3.3 und 3.4 z. B. sturmversion 0.10.x, während HDInsight 3.5 Sturm verwendet 1.0.x.
> 
> C#-Topologien auf Linux-basierten Clustern müssen verwenden .NET 4.5 und Mono auf HDInsight Cluster ausgeführt. Meiste funktioniert jedoch das Dokument [Mono-Kompatibilität](http://www.mono-project.com/docs/about-mono/compatibility/) für Inkompatibilitäten überprüfen soll.

## <a name="prerequisites"></a>Erforderliche Komponenten

- Eine der folgenden Versionen von Visual Studio

    - Visual Studio 2012 [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) oder [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015 oder [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 oder höher

- HDInsight-Tools für Visual Studio: finden Sie unter [Erste Schritte mit HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) installieren und Konfigurieren von HDInsight-Tools für Visual Studio.

    > [AZURE.NOTE] HDInsight-Tools für Visual Studio werden in Visual Studio Express nicht unterstützt.

-   Apache Storm HDInsight Cluster: finden Sie unter [Erste Schritte mit Apache auf HDInsight](hdinsight-apache-storm-tutorial-get-started.md) Schritte zum Erstellen eines Clusters.

## <a name="templates"></a>Vorlagen

Die HDInsight-Tools für Visual Studio bieten die folgenden Vorlagen:

| Projekttyp | Zeigt |
| ------------ | ------------- |
| Storm-Anwendung | Ein leeres Projekt der Storm-Topologie |
| Storm Azure SQL Writer-Beispiel | Schreiben in Azure SQL-Datenbank |
| Storm Reader DocumentDB Beispiel | Lesen von Azure DocumentDB |
| Storm DocumentDB Writer-Beispiel | Schreiben in Azure DocumentDB |
| Storm Reader EventHub Beispiel | Lesen von Azure-Ereignis |
| Storm EventHub Writer-Beispiel | Schreiben in Azure Ereignis Hubs |
| Storm Reader HBase Beispiel | Lesen von HBase auf HDInsight-Cluster |
| Storm HBase Writer-Beispiel | Schreiben in HBase für HDInsight-Cluster |
| Storm Hybrid-Beispiel | Wie Sie eine Java-Komponente |
| Storm-Beispiel | Eine grundlegende Word Count-Topologie |

> [AZURE.NOTE] HBase Reader und Writer-Beispiele verwenden HBase REST API mit HBase auf HDInsight-Cluster nicht die HBase Java API kommunizieren.

In den Schritten in diesem Dokument verwenden Sie grundlegenden Projekttyp von Storm-Anwendung eine neue Topologie erstellen.

## <a name="create-a-c-topology"></a>Erstellen einer C#-Topologie

1. Wenn Sie nicht die neueste Version der HDInsight-Tools für Visual Studio installiert haben, finden Sie unter [Erste Schritte mit HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Öffnen Sie Visual Studio, wählen Sie **Datei** > **neu**und **Projekt**.

3. Erweitern Sie im Bildschirm **Neues Projekt** **installiert** > **Vorlagen**, und wählen Sie **HDInsight**. Wählen Sie aus der Liste der Vorlagen **Sturm Anwendung**. Geben Sie am unteren Bildschirmrand **WordCount** als den Namen der Anwendung.

    ![Bild](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Nachdem das Projekt erstellt wurde, müssen Sie die folgenden Dateien:

    - **Program.cs**: Topologie für das Projekt definiert. Beachten Sie, dass eine Standardtopologie, die aus einer Schnauze und eine Schraube wird standardmäßig erstellt.

    - **Spout.cs**: ein Beispiel Schnauze, die Zufallszahlen ausgibt.

    - **Bolt.cs**: ein Beispiel Bolzen, der eine Anzahl Zahlen Schnauze ausgegeben wird.

    Als Teil des Projekts erstellen wird NuGet neuesten [SCP.NET Pakete](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) heruntergeladen werden.

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

In den nächsten Abschnitten ändern Sie dieses Projekt in einer einfachen WordCount-Anwendung.

### <a name="implement-the-spout"></a>Implementieren der Schnauze

1. Öffnen Sie **Spout.cs**. Ausläufe werden verwendet, um Daten in einer Topologie von einer externen Quelle zu lesen. Die Hauptkomponenten einer Schnauze sind:

    - **NextTuple**: Wenn Schnauze neue Tupel ausgeben Sturm aufgerufen.

    - **ACK** (Transaktions-Topologie): behandelt Empfangsbestätigungen initiiert von anderen Komponenten in der Topologie für Tupel aus diesem Schnauze gesendet. Ein Tupel bestätigen kann Schnauze wissen, dass sie von Downstreamkomponenten erfolgreich verarbeitet wurde.

    - **Fehler** (Transaktions-Topologie): Tupeln, die andere Komponenten in der Topologie Fail-Verarbeitung werden behandelt. Dies bietet die Möglichkeit, das Tupel erneut ausgeben, damit es erneut verarbeitet werden kann.

2. Ersetzen Sie den Inhalt der Klasse **Auslauf** folgt. Dies erstellt eine Schnauze, die zufällig einen Satz in der Topologie ausgibt.

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    Nutzen Sie lesen Sie die Kommentare zu verstehen, was dieser Code.

### <a name="implement-the-bolts"></a>Implementieren Sie die Schrauben

1. Löschen Sie die vorhandene **Bolt.cs** -Datei aus dem Projekt.

2. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts, und wählen Sie **Hinzufügen** > **neu**. Wählen Sie aus der Liste **Sturmblitz**und geben Sie **Splitter.cs** als Namen. Wiederholen Sie diese zweite Schraube benannten **Counter.cs**erstellen.

    - **Splitter.cs**: Bolzen, der Satz in einzelne Wörter aufgeteilt und gibt einen neuen Datenstrom Wörter implementiert.

    - **Counter.cs**: eine Schraube, die zählt jedes Wort und gibt einen neuen Datenstrom Wörter und die Anzahl für jedes Wort implementiert.

    > [AZURE.NOTE] Diese Schrauben lesen und Schreiben von Streams, aber Sie können auch ein wie einer Datenbank oder einem Dienst kommunizieren.

3. Öffnen Sie **Splitter.cs**. Beachten Sie, dass es nur eine Methode standardmäßig: **Ausführen**. Wird aufgerufen, wenn die Schraube ein Tupel zur Verarbeitung erhält. Lesen Sie hier verarbeitet eingehende Tupel und ausgehende Tupel ausgeben.

4. Ersetzen Sie den Inhalt der **Splitter** -Klasse durch den folgenden Code:

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    Nutzen Sie lesen Sie die Kommentare zu verstehen, was dieser Code.

5. Öffnen Sie **Counter.cs** und Ersetzen Sie die Klasseninhalte durch den folgenden:

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    Nutzen Sie lesen Sie die Kommentare zu verstehen, was dieser Code.

### <a name="define-the-topology"></a>Definieren der Topologie

Ausläufe und Schrauben in einem Diagramm angeordnet, die definiert, wie der Datenfluss zwischen Komponenten. Bei dieser Topologie ist das Diagramm wie folgt:

![Bild der Anordnung von Komponenten](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Sätze werden aus der Schnauze ausgegeben Instanzen des Bolzens Splitter verteilt. Splitter Bolzen in Wörtern an Bolzen Zähler Sätze aufgeteilt.

Da Word Count in der lokal gehalten wird, möchten wir sicherstellen, dass bestimmte Wörter in die gleiche Schraube Zählerinstanz flow haben wir nur einmal Überblick über ein bestimmtes Wort. Doch für Bolzen Splitter wirklich egal welche Schraube welcher Satz erhält, damit wir einfach Saldo Sätze für diese Instanzen laden möchten.

Öffnen Sie **Program.cs**. Wichtige Methode ist **GetTopologyBuilder**, die zur Definition der Topologie übermittelt wird, Sturm. Ersetzen Sie den Inhalt des **GetTopologyBuilder** mit dem folgenden Code beschriebene Topologie implementieren:

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

Nutzen Sie lesen Sie die Kommentare zu verstehen, was dieser Code.

## <a name="submit-the-topology"></a>Senden der Topologie

1. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts, und wählen Sie **Absenden an auf HDInsight**.

    > [AZURE.NOTE] Wenn Sie aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement. Wenn Sie über mehrere Abonnements verfügen, melden Sie sich Ihre Sturm HDInsight Cluster enthält.

2. Wählen Sie Ihre Sturm HDInsight Cluster aus Liste **Storm-Cluster** , und wählen Sie **Senden**. Sie können überwachen, ist die Übermittlung erfolgreich mit **Ausgabefenster** .

3. Wenn die Topologie erfolgreich übermittelt wurde, erscheinen **Sturm Topologien** für den Cluster. Wählen Sie aus der Liste Informationen zu laufenden Topologie **WordCount** -Topologie.

    > [AZURE.NOTE] **Storm Topologien** im **Server-Explorer**auch angezeigt: Erweitern Sie **Azure** > **HDInsight**ein Sturms HDInsight Cluster Maustaste, und wählen Sie **Ansicht Sturm Topologien**.

    Verwenden Sie die Links für die Ausläufe oder Schrauben Informationen zu diesen Komponenten. Für jedes ausgewählte Element wird ein neues Fenster öffnet.

4. **Topologie-Zusammenfassung** anzeigen klicken Sie auf die Topologie beenden **Beenden** .

    > [AZURE.NOTE] Storm-Topologien weiter, bis sie deaktiviert oder Cluster gelöscht.

## <a name="transactional-topology"></a>Transaktions-Topologie

Die vorherige Topologie ist nicht transaktional. Die Komponenten in der Topologie implementieren Funktionen für Nachrichten wiedergeben, schlägt Verarbeitung von einer Komponente in der Topologie. Erstellen eines neuen Projekts eine transaktionale Beispieltopologie und **Sturm** Stichprobe als Projekttyp.

Transaktionale Topologien implementieren Folgendes ein, um die Wiedergabe von Daten unterstützen:

- **Zwischenspeichern von Metadaten**: Schnauze Metadaten ausgegeben, damit die Daten abgerufen und erneut ausgegeben, wenn ein Fehler auftritt, Daten speichern muss. Da die Daten im Beispiel ausgegeben klein ist, ist die unformatierten Daten für jedes Tupels in einem Wörterbuch für die Wiedergabe gespeichert.

- **Bestätigung**: jede Schraube in der Topologie erreichen `this.ctx.Ack(tuple)` Ack hat erfolgreich verarbeitet ein Tupel. Bei Schrauben Überarbeitungen der Tupel die `Ack` -Methode der Schnauze aufgerufen. Dadurch wird der Schnauze zwischengespeicherte Daten für die Wiedergabe zu entfernen, da die Daten vollständig verarbeitet wurde.

- **Fehler**: jede Schraube erreichen `this.ctx.Fail(tuple)` an, dass die Verarbeitung für ein Tupel fehlgeschlagen ist. Der Fehler wird an die `Fail` zwischengespeicherte Metadaten Methode der Schnauze, Tupel mit wiedergegeben werden kann.

- **Sequenz-ID**: Wenn ein Tupel, kann eine Sequenz-ID angegeben werden. Dies sollte ein Wert handeln, der Tupel Wiedergabe (Ack und Fail) Verarbeitung identifiziert. Beispielsweise der Schnauze im **Sturm Beispielprojekt** folgenden Wenn Daten:

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    Dies gibt ein neues Tupel, das einen Satz der Standarddatenstrom mit der Sequenz-ID-Wert aus **LastSeqId**enthält. Beispielsweise wird die **LastSeqId** für jedes Tupel ausgegeben einfach erhöht.

Wie im Beispielprojekt **Sturm** , kann ob eine Komponente transaktional ist zur Laufzeit festgelegt basierend auf.

## <a name="hybrid-topology"></a>Hybrid-Topologie

HDInsight-Tools für Visual Studio können auch Hybridtopologien, in denen einige Komponenten C# und andere Java erstellen verwendet werden.

Für ein Hybrid-Topologie wird ein neues Projekt erstellen und **Sturm Hybrid**Stichprobe. Dies erstellt eine vollständig kommentierte Beispieldatei mit mehreren Topologien, die Folgendes zeigen:

- **Auslauf von Java** und **C# Bolzen**: in **HybridTopology_javaSpout_csharpBolt** definiert

    - Eine transaktionale Version wird in **HybridTopologyTx_javaSpout_csharpBolt** definiert.

- **Auslauf C#** und **Java Bolzen**: in **HybridTopology_csharpSpout_javaBolt** definiert

    - Eine transaktionale Version wird in **HybridTopologyTx_csharpSpout_javaBolt** definiert.

        > [AZURE.NOTE] Diese Version veranschaulicht auch Makrosystem Code aus einer Textdatei als Java-Komponente verwenden.

Bewegen Sie zum Wechseln der Topologie, die verwendet wird, wenn das Projekt gesendet wird die `[Active(true)]` Anweisung zur Topologie, bevor sie an den Cluster verwenden möchten.

> [AZURE.NOTE] Alle erforderlichen Java-Dateien werden als Teil dieses Projekts im Ordner " **JavaDependency** " bereitgestellt.

Berücksichtigen Sie beim Erstellen und Senden einer hybridtopologie:

- **JavaComponentConstructor** müssen zum Erstellen einer neuen Instanz der Java-Klasse Schnauze oder Bolzen verwendet werden.

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** sollte verwendet werden, Daten in oder aus Java-Komponenten aus Java-Objekten zu JSON serialisiert.

- Wenn die Topologie Server verwenden Sie die Option **zusätzliche Konfigurationen** , **Java Dateipfade**angeben. Der angegebene Pfad sollte das Verzeichnis mit den JAR-Dateien, die Ihre Java-Klassen enthalten.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.Net Version 0.9.4.203 führt eine neue Klasse und Methode speziell für die Arbeit mit Ereignis Hub Auslauf (ein Java Schnauze, die vom Ereignis Hub liest.) Erstellung eine Topologie, die diese Schnauze verwendet, verwenden Sie die folgenden Methoden:

- **EventHubSpoutConfig** -Klasse: erstellt ein Objekt mit der Konfiguration der Komponente Schnauze

- **TopologyBuilder.SetEventHubSpout** -Methode: Fügt die Komponente Ereignis Hub Auslauf der Topologie

> [AZURE.NOTE] Während diese Ereignis Hub Auslauf als andere Java-Komponenten arbeiten erleichtern, müssen Sie noch die CustomizedInteropJSONSerializer Schnauze Daten serialisieren verwenden.

## <a id="configurationmanager"></a>ConfigurationManager verwenden

Verwenden von ConfigurationManager auf Konfigurationswerte Abrufen von Bolzen und Auslauf Komponenten; Dies führt zu einer Ausnahme null-Zeiger. Stattdessen wird die Konfiguration für das Projekt Storm-Topologie als Schlüssel-Wert-Paar in der Topologie Kontext übergeben. Jede Komponente, die auf Werten müssen sie aus dem Kontext Initialisierung abrufen.

Der folgende Code veranschaulicht, wie diese Werte abgerufen:

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

Verwenden Sie ein `Get` Methode, um eine Instanz der Komponente, müssen beide übergibt die `Context` und `Dictionary<string, Object>` Parameter für den Konstruktor. Das folgende Beispiel ist ein `Get` -Methode, die diese Werte korrekt übergibt:

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>SCP.NET aktualisieren

Aktuelle Versionen von SCP.NET support-paketaktualisierung über NuGet. Wenn ein neues Update verfügbar ist, erhalten Sie eine Upgrade-Benachrichtigung. Um eine Aktualisierung manuell überprüfen, gehen Sie wie folgt vor:

1. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts, und wählen Sie **NuGet-Pakete verwalten**.

2. Wählen Sie den Paketmanager **Updates**. Wenn ein Update verfügbar ist, wird es aufgeführt. Klicken Sie auf die Schaltfläche **Update** für das Paket zu installieren.

> [AZURE.IMPORTANT] Wenn das Projekt mit früheren Versionen von SCP.NET, die nicht NuGet für Paket-Updates verwenden erstellt wurde, führen Sie die folgenden Schritte zum Aktualisieren auf die neue Version:
>
> 1. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts, und wählen Sie **NuGet-Pakete verwalten**.
> 2. Mit **dem Suchfeld** suchen und dann **Microsoft.SCP.Net.SDK** zum Projekt hinzufügen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="null-pointer-exceptions"></a>NULL-Zeiger Ausnahmen

Bei C#-Topologie mit einem Linux-basierten HDInsight Cluster Bolzen und Auslauf Komponenten, die ConfigurationManager lesen Konfigurationen zur Laufzeit Nullzeiger Ausnahmen zurückgeben können. Dies ist die Konfiguration für die geladene Domäne nicht aus der Assembly, die das Projekt enthält.

Die Konfiguration für das Projekt Storm-Topologie als Schlüssel-Wert-Paar in der Topologie Kontext übergeben und an Komponenten übergeben wird, wenn sie initialisiert Wörterbucheintrags abgerufen werden kann.

Das folgende Beispiel veranschaulicht die Konfigurationswerte im Topologie-Kontext laden, die [ConfigurationManager](#configurationmanager) Abschnitt dieses Dokuments.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Bei einer C#-Topologie mit Linux-basierte HDInsight-Cluster können folgenden Fehler auftreten:

    System.TypeLoadException: Failure has occurred while loading a type.

In der Regel ist bei Verwendung einen binären auftritt, die nicht kompatibel mit der Version von .NET Mono unterstützt.

Für Linux-basierte HDInsight Cluster sollten Sie sicherstellen, dass das Projekt für .NET 4.5 kompilierten Binärdateien verwendet.


### <a name="test-a-topology-locally"></a>Eine Topologie lokal testen

Zwar benutzerfreundliche eine Topologie mit einem Cluster in einigen Fällen müssen Sie eine Topologie lokal testen. Gehen Sie zu der Beispieltopologie in diesem Lernprogramm in Umgebung testen.

> [AZURE.WARNING] Das lokale Testen funktioniert nur für basic, C# nur Topologien. Sie verwenden nicht lokalen Testen Hybridtopologien oder Topologien, mit denen mehrere Datenströme Fehler erhalten.

1. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts und wählen **Eigenschaften**. Ändern Sie den **Ausgabetyp** in den Projekteigenschaften **Konsole**Anwendung.

    ![Ausgabetyp](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] Beachten Sie den **Ausgabetyp** auf **Klassenbibliothek** ändern, bevor die Topologie zu einem Cluster bereitgestellt.

2. Im **Projektmappen-Explorer**mit der rechten Maustaste des Projekts und wählen **Hinzufügen** > **Neu**. Wählen Sie **Klasse** und **LocalTest.cs** als den Klassennamen. Klicken Sie abschließend auf **Hinzufügen**.

3. Öffnen Sie **LocalTest.cs** , und fügen Sie die folgende **using** -Anweisung am Anfang:

        using Microsoft.SCP;

4. Verwenden Sie folgende als Inhalt der **LocalTest** -Klasse:

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    Einen kurzen Augenblick den Kommentaren zu lesen. Dieser Code verwendet die **LocalContext** Komponenten in der ausführen und weiterhin den Datenstrom zwischen Komponenten in Textdateien auf der Festplatte.

5. Öffnen Sie **Program.cs** , und fügen Sie Folgendes zu der **Main** -Methode:

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. Die Änderungen dann auf **F5** oder wählen Sie **Debuggen** > Projekt zu**Debuggen** . Ein Konsolenfenster soll angezeigt werden, und Protokollstatus als Tests ausgeführt. Wenn **Tests abgeschlossen** angezeigt wird, drücken Sie eine beliebige Taste, um das Fenster zu schließen.

7. Mit dem **Windows Explorer** zu dem Verzeichnis mit dem Projekt, z. B. **C:\Users\<Your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Öffnen Sie in diesem Verzeichnis **Bin**, und klicken Sie dann auf **Debuggen**. Sehen Sie die Textdateien, die erstellt wurden, wenn die Tests ausgeführt haben: sentences.txt, counter.txt und splitter.txt. Jede Datei öffnen und die Daten prüfen.

    > [AZURE.NOTE] Zeichenfolge wird als Array von decimal in diesen Dateien beibehalten. Beispielsweise \[[97,103,111]] in der **splitter.txt** Datei ist das Wort "und".

Testen eine einfache Word count zwar Anwendung lokal ziemlich einfach, der reale Wert ist, wenn eine komplexe Topologie, die Kommunikation mit externen Datenquellen oder führt komplexe Daten. Wenn Sie an einem Projekt arbeiten, müssen Sie Haltepunkte und den Code schrittweise in Ihren Komponenten Probleme isolieren festgelegt.

> [AZURE.NOTE] Müssen Sie den **Projekttyp** auf **Klassenbibliothek** festlegen, bevor Sie zu HDInsight-Cluster bereitstellen.

### <a name="log-information"></a>Informationen

Sie können problemlos Protokollinformationen der Topologie Komponenten mit `Context.Logger`. Beispielsweise erstellt folgende Informations-Protokolleintrag:

    Context.Logger.Info("Component started");

Protokollinformationen kann **Hadoop-Protokoll**im **Server-Explorer**gefunden angezeigt werden. Erweitern Sie den Eintrag für Ihr Sturm HDInsight-Cluster und dann **Hadoop-Protokoll**. Wählen Sie abschließend die Protokolldatei anzeigen.

> [AZURE.NOTE] Die Protokolle werden in Azure Storage-Konto gespeichert, die im Cluster verwendet wird. Ist dies ein anderes Abonnement als Sie, mit Visual Studio angemeldet sind, müssen Sie das Abonnement anmelden, die das Speicherkonto zum Anzeigen dieser Informationen enthält.

### <a name="view-error-information"></a>Fehlerinformationen anzeigen

Um Fehler anzuzeigen, die in einer laufenden Topologie aufgetreten sind, gehen Sie folgendermaßen vor:

1. **Server-Explorer**mit der rechten Maustaste auf HDInsight Cluster, und wählen Sie **Ansicht Sturm Topologien**.

2. **Auslauf** und **Schrauben**haben die **Letzter\nfehler** Spalte Informationen zum letzten Fehler, die aufgetreten ist.

3. Auswählen der **Auslauf** oder **Bolzen-Id** für die Komponente einen Fehler angezeigt. Auf der Seite, die angezeigt wird, finden zusätzliche Fehlerinformationen im Abschnitt **Fehler** am unteren Rand der Seite.

4. Für mehr Informationen **Executors** Abschnitt der Seite Storm Worker-Protokoll finden in den letzten Minuten wählen Sie einen **Anschluss aus** .

## <a name="next-steps"></a>Nächste Schritte

Zum Entwickeln und Bereitstellen Sturm Topologien von HDInsight-Tools für Visual Studio Informationen bearbeitet wie zur [Verarbeitung von Ereignissen von Azure Event Hub mit auf HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Finden Sie beispielsweise ein C#-Topologie, die mehrere Datenströme Streamdaten aufgeteilt [C# Sturm Beispiel](https://github.com/Blackmist/csharp-storm-example).

Erfahren Sie weitere Informationen zum Erstellen von C#-Topologien finden Sie unter [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Weitere Methoden zum Arbeiten mit HDInsight und weitere Sturm HDinsight Proben finden Sie hier:

**Microsoft SCP.NET**

* [SCP-Programmführer](hdinsight-storm-scp-programming-guide.md)

**Apache Storm auf HDInsight**

- [Bereitstellen und Überwachen von Topologien mit Apache auf HDInsight](hdinsight-storm-deploy-monitor-topology.md)

- [Topologien für auf HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop auf HDInsight**

- [Hadoop auf HDInsight Struktur verwenden](hdinsight-use-hive.md)

- [Hadoop auf HDInsight Schwein verwenden](hdinsight-use-pig.md)

- [Verwenden Sie MapReduce Hadoop auf HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase auf HDInsight**

- [Erste Schritte mit HBase auf HDInsight](hdinsight-hbase-tutorial-get-started.md)
