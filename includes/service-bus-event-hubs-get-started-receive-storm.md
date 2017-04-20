## <a name="receive-messages-with-apache-storm"></a>Nachrichten Sie mit Apache

[**Apache Storm**](https://storm.incubator.apache.org) ist eine verteilte Berechnung in Echtzeit, die zuverlässige unbounded Datenströme vereinfacht. Dieser Abschnitt beschreibt, wie Sie ein Ereignis Hubs Sturm Schnauze Ereignis Hubs Ereignisse empfangen. Apache Storm können Sie Ereignisse für mehrere Prozesse in verschiedenen Knoten gehostet aufteilen. Ereignis Hubs Integration mit vereinfacht Ereignis Verbrauch durch transparente Prüfpunkte den Fortschritt mithilfe des Sturm Zookeeper Installation, permanente Prüfpunkte verwalten und Parallel von Ereignis empfängt.

Weitere Informationen zum Ereignis Hubs erhalten Sie Muster, finden Sie unter [Übersicht über Event Hubs][].

Diese praktische Einführung verwendet eine [HDInsight Storm][] -Installation mit Event Hubs Schnauze bereits.

1. Gehen Sie auf [HDInsight Storm - erste Schritte](../articles/hdinsight/hdinsight-storm-overview.md) , um einen neuen HDInsight-Cluster erstellen und über den Remotedesktop an.

2. Kopieren der `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` Datei lokale Umgebung. Enthält die Ereignisse Sturm Schnauze.

3. Verwenden Sie folgenden Befehl in den Speicher des lokalen Maven installiert. Dadurch werden als Verweis im Projekt Sturm in einem späteren Schritt hinzufügen.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. In Eclipse erstellen Sie ein neues Maven-Projekt (klicken Sie auf **Datei** **neu**dann **Project**).

    ![][12]

5. Wählen Sie **Arbeitsbereich Standardspeicherort verwenden aus**und dann auf **Weiter**

6. Wählen Sie **Maven-Urtyp-Schnellstart** -Urtyp aus und dann auf **Weiter**

7. Fügen Sie eine **GroupId** und **ArtifactId ein**und dann auf **Fertig stellen**

8. Fügen Sie **pom.xml**die folgenden Abhängigkeiten in der `<dependency>` Knoten.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. Im Ordner **Src** eine Datei namens **Config.properties** , und kopieren Sie den folgenden Inhalt der folgenden Werte:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    Der Wert für **eventhub.receiver.credits** bestimmt, wie viele Ereignisse vor dem Sturm Pipeline zusammengefasst werden. Der Einfachheit halber wird dieser Wert auf 10. In der Produktion sollte es in der Regel höhere Werte festgelegt werden. beispielsweise 1024.

10. Erstellen Sie eine neue Klasse namens **LoggerBolt** mit dem folgenden Code:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    Diese sturmblitz protokolliert den Inhalt der empfangenen Ereignisse. Dies kann problemlos erweitert werden Tupel in einem Speicherdienst speichern. [HDInsight Sensor Analyse Lernprogramm] verwendet dasselbe Verfahren zum Speichern von Daten in HBase.

11. Erstellen Sie eine Klasse namens **LogTopology** mit dem folgenden Code:

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    Diese Klasse erstellt einen neuen Ereignis Hubs Schnauze mithilfe der Eigenschaften in der Konfiguration Datei zu Instatiate. Es ist wichtig zu beachten, dass dieses Beispiel viele Ausläufe Vorgänge als die Anzahl der Partitionen in der Ereignis erstellt, um die maximale Parallelität zulässt, Ereignis-Hub verwenden.

<!-- Links -->
[Übersicht über Hubs]: ../articles/event-hubs/event-hubs-overview.md
[HDInsight Sturm]: ../articles/hdinsight/hdinsight-storm-overview.md
[HDInsight Sensor Analyse Lernprogramm]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png