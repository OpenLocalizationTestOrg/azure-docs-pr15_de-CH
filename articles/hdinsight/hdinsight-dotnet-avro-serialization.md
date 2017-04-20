<properties
    pageTitle="Serialisieren von Daten mit Microsoft Avro Library | Microsoft Azure"
    description="Erfahren Sie, wie Azure HDInsight Avro verwendet, um große Daten serialisieren."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="jgao"/>


# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Serialisieren von Daten in Hadoop mit Microsoft Avro Library

Dieses Thema zeigt, wie <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro-Bibliothek</a> verwenden, um Objekte und andere Datenstrukturen in Streams serialisieren, um sie auf Speicher, einer Datenbank oder einer Datei beizubehalten und sie zum Wiederherstellen der ursprünglichen Objekte deserialisiert.

[AZURE.INCLUDE [windows-only](../../includes/hdinsight-windows-only.md)]

##<a name="apache-avro"></a>Apache Avro
<a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro-Bibliothek</a> implementiert Apache Avro Serialisierung Datensystem Microsoft.NET Umgebung. Apache Avro bietet eine kompakte Binärdaten Interchange Format für die Serialisierung. <a href="http://www.json.org" target="_blank">JSON</a> verwendet, um einen sprachenunabhängigen Schema definieren, die Sprachinteroperabilität abschließt. In einer Sprache serialisierte Daten können in einer anderen gelesen werden. C, C++, C#, Java, PHP, Python und Ruby werden derzeit unterstützt. Ausführliche Informationen zum Format finden in der <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro-Spezifikation</a>. Beachten Sie, dass die aktuelle Version von Microsoft Avro Library Remoteprozeduraufrufe (RPCs) aufrufen Teil dieser Spezifikation nicht unterstützt.

Die serialisierte Darstellung eines Objekts in der Avro-System besteht aus zwei Teilen: Schema und dem tatsächlichen Wert. Avro-Schema beschreibt sprachunabhängigen Datenmodell mit JSON serialisierten Daten. Seite eine binäre Darstellung der Daten vorhanden ist. Müssen getrennt von der binären Darstellung Schema kann jedes Objekt keine pro-Wert bei gleichzeitigem so schnell Serialisierung und die Darstellung klein geschrieben werden.

##<a name="the-hadoop-scenario"></a>Hadoop-Szenario
Das Serialisierungsformat Apache Avro ist in Azure HDInsight und andere Apache Hadoop verbreitet. Avro bietet eine bequeme Möglichkeit, komplexe Datenstrukturen innerhalb eines Projektes Hadoop MapReduce dargestellt. Das Format der Avro (Avro-Objekt Container-Datei) zur Unterstützung verteilten MapReduce-Programmiermodells. Die wichtigste Funktion, die die Verteilung ermöglicht ist, dass die Dateien "es" insofern eine jederzeit in einer Datei suchen und aus einem bestimmten Block lesen.

##<a name="serialization-in-avro-library"></a>Serialisierung in Avro-Bibliothek
Die Bibliothek .NET Avro unterstützt zwei Methoden Serialisieren von Objekten:

- **Reflection** - Schema für die Typen der JSON basiert automatisch aus den Daten Vertragsattribute .NET Typen serialisiert werden.
- **generischer Datensatz** - ein JSON-Schema wurde explizit festgelegten dargestellt von der [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) -Klasse keine .NET beschreiben das Schema für die zu serialisierenden Daten vorhanden sind.

Wenn das Schema der Writer und Reader des Streams bekannt ist, können die Daten ohne dessen Schema gesendet. Bei Avro Container Objektdatei wird das Schema bei in der Datei gespeichert. Andere Parameter wie der Codec zur Datenkompression können angegeben werden. Diese Szenarien sind ausführlich beschrieben und in den folgenden Codebeispielen veranschaulicht.


## <a name="install-avro-library"></a>Avro-Bibliothek installieren

Die folgenden müssen vor der Installation der Bibliothek:

- <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft.NET Framework 4</a>
- <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 oder höher)

Beachten Sie, dass die Abhängigkeit Newtonsoft.Json.dll automatisch mit der Installation von Microsoft Avro Library heruntergeladen wird. Die Vorgehensweise wird im folgenden bereitgestellt.


Microsoft Avro Library wird als NuGet-Paket verteilt, die von Visual Studio über folgendermaßen installiert werden:

1. Wählen Sie auf der Registerkarte **Projekt** -> **NuGet-Pakete verwalten...**
2. Suchen Sie nach "Microsoft.Hadoop.Avro" in das **Online durchsuchen** .
3. Klicken Sie auf **Microsoft Azure HDInsight Avro Bibliothek**neben **Installieren** .

Beachten Sie, dass die Newtonsoft.Json.dll (> = 6.0.4) Abhängigkeit auch automatisch mit Microsoft Avro Library heruntergeladen.

Sie möchten die <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library-Homepage</a> um die aktuellen Versionsinformationen zu lesen.


Microsoft Avro Library-Quellcode ist <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library-Homepage</a>.

##<a name="compile-schemas-using-avro-library"></a>Kompilieren Sie Schemas mit Avro Bibliothek

Die Microsoft Avro Library enthält ein Code Generation Programm, C#-Typen automatisch basierend auf den zuvor definierten JSON-Schema erstellen. Das Dienstprogramm zum Generieren von Code nicht als ausführbare Binärdatei verteilt, sondern kann einfach über das folgende Verfahren erstellt:

1. Die ZIP-Datei mit der neuesten Version des Quellcodes HDInsight SDK von <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK Hadoop</a>herunterladen (Klicken Sie **herunterladen** , keine **Downloads** Registerkarte.)

2. Extrahieren Sie HDInsight SDK in ein Verzeichnis auf dem Computer mit.NET Framework 4 installiert und an das Internet erforderlichen Abhängigkeit NuGet-Pakete herunterladen. Unten wird davon ausgegangen, dass der Quellcode in C:\SDK extrahiert.

3. Wechseln Sie zum Ordner C:\SDK\src\Microsoft.Hadoop.Avro.Tools, und führen Sie build.bat. (Die Datei rufen MSBuild von der 32-Bit-Verteilung von.NET Framework. Möchten Sie die 64-Bit-Version verwenden, bearbeiten Sie den Kommentaren in der Datei build.bat.) Stellen Sie sicher, dass der Build erfolgreich ist. (Auf einigen Systemen kann MSBuild Warnungen erzeugt. Diese Warnung wirken nicht das Dienstprogramm als gibt es keine Buildfehler.)

4. Das kompilierte Programm befindet sich im C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.


Um mit der Befehlszeilensyntax vertraut aus dem Ordner das Dienstprogramm zum Generieren von Code aus führen Sie den folgenden Befehl aus:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

Testen Sie das Dienstprogramm erzeugen Sie C#-Klassen von der JSON-Schema Beispieldatei mit dem Quellcode bereitgestellt. Führen Sie den folgenden Befehl ein:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Erstellen zwei C#-Dateien im aktuellen Verzeichnis soll: SensorData.cs und Location.cs.

Die Logik, die das Dienstprogramm zum Generieren von Code während der Konvertierung von JSON-Schema C#-Typen verwenden, finden Sie die Datei GenerationVerification.feature im C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Beachten Sie, dass Namespaces aus dem JSON Schema im vorherigen Absatz erwähnten Datei beschriebene Logik extrahiert werden. Aus dem Schema extrahiert Vorrang was mit n-Parameter in der Befehlszeile des Dienstprogramms bereitgestellt wird. Im Schema enthaltenen Namespaces überschreiben, verwenden Sie den Parameter/nf. Beispielsweise, um alle Namespaces aus der SampleJSONSchema.avsc in my.own.nspace ändern, führen Sie den folgenden Befehl aus:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="samples"></a>Beispiele
Sechs Beispiele in diesem Thema bereitgestellten für Szenarien von Microsoft Avro Library unterstützt. Microsoft Avro Library soll Stream arbeiten. In diesen Beispielen werden die Daten per Arbeitsspeicherstreams statt Dateistreams Datenbanken für Einfachheit und Konsistenz geändert. Der Ansatz einer hängt erforderlichen Szenario, Datenquelle und Volume einzelnen und anderen Faktoren.

Die ersten beiden Beispiele zeigen, wie zum Serialisieren und Deserialisieren von Daten in Streams Speicherpuffer mit Reflektion und generische Datensätze. Das Schema in beiden Fällen wird zwischen Readern und Writern Out-of-Band.

Die dritte und vierte Beispiele veranschaulichen zum Serialisieren und Deserialisieren von Daten mithilfe von Avro Objekt Container-Dateien. In einem Avro-Container-Datei Daten wird ihr Schema immer mit gespeichert, weil das Schema für die Deserialisierung freigegeben werden muss.

Im Beispiel mit der ersten vier kann <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-86055923" target="_blank">Azure Codebeispiele</a> Website heruntergeladen werden.

Das fünfte Beispiel zeigt, wie ein Codec benutzerdefinierte Komprimierung Avro Container Objektdateien mit. Beispiel enthält den Code für dieses Beispiel kann von <a href="http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111" target="_blank">Azure Codebeispiele</a> Website heruntergeladen werden.

Im sechste Beispiel veranschaulicht, wie mit Avro Serialisierung Azure BLOB-Speicher Daten hochladen und analysieren Sie sie mit einem Cluster HDInsight (Hadoop) Struktur. Es kann von <a href="https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure Codebeispiele</a> Website heruntergeladen werden.

Hier sind Links zu den sechs Beispielen im Thema behandelt:

 * <a href="#Scenario1">**Serialisierung mit Reflektion**</a> - die JSON-Schema für Typen serialisiert werden automatisch aus den Daten Vertragsattribute baut auf.
 * <a href="#Scenario2">**Serialisierung mit generischen**</a> - die JSON-Schema wird explizit festgelegten wird keinerlei .NET zur Reflektion.
 * <a href="#Scenario3">**Serialisierung mit Objektdateien mit Container**</a> - das JSON-Schema wird automatisch erstellt und mit den serialisierten Daten über eine Avro Objekt Container freigegeben.
 * <a href="#Scenario4">**Serialisierung mit Objektdateien mit generischen Container**</a> - das JSON-Schema explizit vor der Serialisierung angegeben und Daten über eine Avro Objekt Container freigegeben.
 * <a href="#Scenario5">**Serialisierung mit Container Objektdateien mit einem benutzerdefinierten Compression Codec**</a> - Beispiel veranschaulicht das Erstellen einer Objektdatei Container Avro mit einer benutzerdefinierten .NET Implementierung des Deflate Data Compression Codec.
 * <a href="#Scenario6">**Mithilfe von Avro zum Hochladen von Daten für den Dienst Microsoft Azure HDInsight**</a> - Beispiel veranschaulicht wie Avro Serialisierung mit der HDInsight-Dienst interagiert. Ein aktives Azure-Abonnement und Zugriff auf Azure HDInsight-Cluster sind zum Ausführen dieses Beispiels erforderlich.

###<a name="Scenario1"></a>Beispiel 1: Serialisierung mit Reflektion

JSON-Schema für die Typen kann automatisch von Microsoft Avro Library über Reflektion aus den Daten Vertragsattribute serialisiert werden C#-Objekte erstellt werden. Die Microsoft Avro Library erstellt ein [**IAvroSeralizer<T> **](http://msdn.microsoft.com/library/dn627341.aspx) Feld serialisierende identifizieren.

In diesem Beispiel Objekte (einer **SensorData** Klasse Member **Speicherort** Struct) in einen Speicherstream serialisiert und wiederum diesen Stream deserialisiert wird. Das Ergebnis wird dann mit der ersten Instanz zu bestätigen, dass das wiederhergestellte **SensorData** -Objekt mit dem Original identisch verglichen.

Das Schema in diesem Beispiel wird angenommen, dass zwischen Readern und Writern freigegeben werden, damit das Avro Objekt Containerformat nicht erforderlich ist. Beispiel zum Serialisieren und Deserialisieren von Daten in Speicherpuffer mit Reflektion Containerformat Objekt beim Schema mit den Daten freigegeben werden muss finden Sie unter <a href="#Scenario3">Serialisierung mit Container Objektdateien mit</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-2-serialization-with-a-generic-record"></a>Beispiel 2: Serialisierung mit generischen

Ein JSON-Schema kann in einen allgemeinen Datensatz explizit angegeben werden, wenn Reflektion verwendet werden kann, da die Daten über einen Datenvertrag .NET Klassen dargestellt werden können. Diese Methode ist langsamer als die Verwendung von Reflektion. In diesem Fall werden das Schema für die Daten dynamisch, d. h. möglicherweise auch nicht zur Kompilierungszeit bekannt. Daten als kommagetrennte (CSV)-Dateien, deren Schema bekannt ist, bevor es zur Laufzeit Avro Format transformiert wird, ist ein Beispiel für diese Art von dynamischen Szenario.

Dieses Beispiel zeigt das Erstellen und Verwenden einer [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) ein JSON-Schema explizit angeben, wie mit Daten gefüllt und zum Serialisieren und deserialisieren. Das Ergebnis ist dann gegenüber der ersten Instanz zu bestätigen, dass der wiederhergestellte Datensatz mit dem Original identisch ist.

Das Schema in diesem Beispiel wird angenommen, dass zwischen Readern und Writern freigegeben werden, damit das Avro Objekt Containerformat nicht erforderlich ist. Beispiel zum Serialisieren und Deserialisieren von Daten in Speicherpuffer mit einen allgemeinen Datensatz Objekt-Container-Format bei Schema mit den serialisierten Daten finden Sie unter <a href="#Scenario4">Serialisierung mit Objektdateien mit generischen Container</a> -Beispiel.


    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn will be then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Beispiel 3: Serialisierung mit Container Objektdateien Serialisierung mit Reflektion

Dieses Beispiel ist ähnlich wie im <a href="#Scenario1">ersten Beispiel wird</a>das Szenario, das Schema mit Reflektion implizit angegeben. Die Differenz, die hier ist das Schema nicht als Reader bekannt, die es deserialisiert. **SensorData** Objekte serialisiert werden und deren Schema implizit angegeben werden in Avro Container Objektdatei dargestellt durch die [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) -Klasse gespeichert.

In diesem Beispiel werden die Daten serialisiert [**SequentialWriter<SensorData> **](http://msdn.microsoft.com/library/dn627340.aspx) und deserialisierter mit [**SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Das Ergebnis ist dann die ersten Instanzen Identität verglichen.

Die Daten in der Objektdatei Container über Standard- [**Deflate**] komprimiert[ deflate-100] Komprimierungscodec von.NET Framework 4. Der <a href="#Scenario5">fünfte Beispiel</a> in diesem Thema erfahren, wie eine neuere und hervorragende Version des [**Deflate**] [ deflate-110] Komprimierungscodec in.NET Framework 4.5 verfügbar.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will be compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


###<a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Beispiel 4: Serialisierung mit Container Objektdateien Serialisierung mit generischen

Dieses Beispiel ist ähnlich wie im <a href="#Scenario2">zweiten Beispiel wird</a>das Szenario, in dem das Schema explizit mit JSON angegeben. Die Differenz, die hier ist das Schema nicht als Reader bekannt, die es deserialisiert.

Das Test-Dataset in eine Liste von [**AvroRecord**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) -Objekten über einen definierten JSON-Schema erfasst und in Container Objektdatei dargestellt durch die [**AvroContainer**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) -Klasse gespeichert. Diese Containerdatei erstellt einen Writer, der zum Serialisieren der Daten nicht komprimiert, in einen Speicherstream in einer Datei gespeichert wird. Zum Erstellen des Readers [**Codex.Null**](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) -Parameter gibt an, dass diese Daten nicht komprimiert werden.

Die Daten aus der Datei gelesen und in eine Auflistung von Objekten deserialisiert. Diese Auflistung ist im Vergleich zu anfänglichen Avro Datensatzliste zu bestätigen, dass sie identisch sind.


    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data will not be compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




###<a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Beispiel 5: Serialisierung benutzerdefinierte Komprimierungscodec mit Objekt-Container-Dateien

Das fünfte Beispiel zeigt, wie ein Codec benutzerdefinierte Komprimierung Avro Container Objektdateien mit. Beispiel enthält den Code für dieses Beispiel kann von [Azure Codebeispiele](http://code.msdn.microsoft.com/windowsazure/Serialize-data-with-the-67159111) Website heruntergeladen werden.

Die [Avro-Spezifikation](http://avro.apache.org/docs/current/spec.html#Required+Codecs) ermöglicht Verwendung eine optionale Komprimierung Codecs (neben Standard **Null** und **Deflate** ). In diesem Beispiel wird einen vollständig neuen Codec wie Snappy (unterstützt optionale Codec in der [Avro-Spezifikation](http://avro.apache.org/docs/current/spec.html#snappy)genannt) nicht implementiert. Es zeigt, wie.NET Framework 4.5 Implementierung des [**Deflate**] [ deflate-110] Codec bietet einen besseren Komprimierungsalgorithmus basierend auf [die Komprimierung als die Standardversion von.NET Framework 4](http://zlib.net/) .


    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It will catch the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it will rely on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which will deserialize all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

###<a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Beispiel 6: Avro verwenden, um Daten für Microsoft Azure HDInsight-Dienst

Das sechste Beispiel veranschaulicht einige Programmiertechniken mit Azure HDInsight Service beziehen. Beispiel enthält den Code für dieses Beispiel kann von [Azure Codebeispiele](https://code.msdn.microsoft.com/windowsazure/Using-Avro-to-upload-data-ae81b1e3) Website heruntergeladen werden.

Im Beispiel führt Folgendes aus:

* Verbindet mit einem vorhandenen HDInsight Dienst Cluster.
* Serialisiert mehrere CSV-Dateien und das Ergebnis in Azure BLOB-Speicher hochgeladen. (CSV-Dateien werden zusammen mit der Probe und einen Auszug AMEX Stock Verlaufsdaten von [Infochimps](http://www.infochimps.com/) für den Zeitraum 1970 2010 verteilt darstellen. Im Beispiel liest Daten aus CSV, Datensätze für Instanzen der Klasse **Stock** konvertiert und dann mithilfe von Reflektion serialisiert. Typdefinition wird aus einem JSON-Schema über Microsoft Avro Library Code Generation Dienstprogramm erstellt.
* Erstellt eine neue externe Tabelle namens **Bestände** in Struktur und Links zu den Daten im vorherigen Schritt Upload.
* Führt eine Abfrage mit Struktur über die **Aktien** -Tabelle.

Darüber hinaus führt das Beispiel eine Prozedur bereinigen vor und nach wichtigen Vorgänge. Während der Bereinigung Azure BLOB-Daten und Ordner werden entfernt und die Hive-Tabelle gelöscht. Sie können auch bereinigen Verfahren aus der Beispiel-Befehlszeile aufrufen.

Das Beispiel enthält die folgenden Komponenten:

* Ein aktives Microsoft Azure-Abonnement und die Abonnement-ID
* Für das Abonnement mit dem entsprechenden privaten Schlüssel Verwaltungszertifikat. Das Zertifikat sollte im aktuellen Benutzer privaten Speicher auf dem Computer zum Ausführen des Beispiels installiert.
* Active HDInsight-Cluster.
* Ein Azure Storage-Konto verknüpft HDInsight Cluster aus vorherigen Voraussetzung mit primären oder sekundären Zugriffstaste.

Alle Informationen über die Komponenten der Beispiel-Konfigurationsdatei einzutragen, bevor das Beispiel ausgeführt wird. Es gibt zwei mögliche Vorgehensweisen dafür:

* Bearbeiten Sie die Datei app.config im Root-Beispielverzeichnis und erstellen Sie das Beispiel
* Zunächst erstellen Sie das Beispiel und bearbeiten AvroHDISample.exe.config im Build-Verzeichnis

In beiden Fällen alle Bearbeitungsvorgänge erfolgen der **<appSettings>** Einstellungen. Befolgen Sie die Kommentare in der Datei.
Ausführen des Beispiels über die Befehlszeile mit dem folgenden Befehl (wo die ZIP-Datei mit den C:\AvroHDISample, extrahiert werden angenommen wurde, wenn andernfalls verwenden Sie den entsprechenden Pfad):

    AvroHDISample run C:\AvroHDISample\Data

Führen Sie den folgenden Befehl, um den Cluster zu bereinigen:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
