<properties
   pageTitle="Übersicht über die Konfiguration von Azure Fabric zuverlässigen Dienst | Microsoft Azure"
   description="Lernen Sie in Azure Service Fabric statusbehaftete zuverlässige Dienste konfigurieren."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Statusbehaftete zuverlässige Dienste konfigurieren

Es gibt zwei Konfigurationen für zuverlässige Dienste. Eine ist für alle zuverlässige Dienste im Cluster der Satz für bestimmte Serviceangebot.

## <a name="global-configuration"></a>Konfiguration

Globale zuverlässigen Service-Konfiguration wird im clustermanifest für den Cluster im Abschnitt KtlLogger angegeben. Es ermöglicht die Konfiguration der freigegebenen Standort und Größe sowie die globale Speichergrenzen von der Protokollierung verwendet. Das clustermanifest ist eine einzelne XML-Datei, die Einstellungen und Konfigurationen, die für alle Knoten und Dienste im Cluster gelten. Die Datei heißt in der Regel ClusterManifest.xml. Cluster Siehe manifest für den Cluster mithilfe der Get-ServiceFabricClusterManifest-Powershell-Befehl.

### <a name="configuration-names"></a>Konfigurationsnamen

|Name|Einheit|Standardwert|Beschreibung|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|KB|8388608|Minimale Anzahl der KB im Kernelmodus für die Protokollierung schreiben Pufferpool Speicher reservieren. Diesen Speicherpool wird zum Zwischenspeichern von Informationen vor dem Schreiben auf die Festplatte.|
|WriteBufferMemoryPoolMaximumInKB|KB|Keine Begrenzung|Maximale Größe, die Protokollierung schreiben Pufferpool Speicher, wächst.|
|SharedLogId|GUID|""|Gibt eine eindeutige GUID zum Identifizieren der freigegebenen Standardprotokolldatei verwendet durch alle zuverlässige Dienste auf allen Knoten im Cluster, die nicht die SharedLogId in ihrer spezifischen Konfiguration angeben. Wenn SharedLogId angegeben wird, muss auch SharedLogPath angegeben werden.|
|SharedLogPath|Vollqualifizierter Pfad|""|Gibt den vollqualifizierten Pfad, wo die freigegebene Datei von allen zuverlässige Dienste auf allen Knoten im Cluster verwendet, die nicht die SharedLogPath in ihrer spezifischen Konfiguration angeben. Wenn SharedLogPath angegeben wird, muss SharedLogId auch angegeben werden.|
|SharedLogSizeInMB|MB|8192|Gibt die Anzahl der MB Speicherplatz für freigegebene Protokoll statisch reservieren. Der Wert muss größer oder 2048.|

### <a name="sample-cluster-manifest-section"></a>Beispiel-Cluster Manifestabschnitt
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Beschreibung
Die Protokollierung wurde einen globalen Pool reserviert nicht ausgelagerten Kernel-Speicher zur zuverlässige Dienste auf einem Knoten zum Zwischenspeichern von Daten vor dem zuverlässigen Service Replikat zugeordnet dediziertes Protokoll geschrieben wird. Die Poolgröße ist WriteBufferMemoryPoolMinimumInKB und WriteBufferMemoryPoolMaximumInKB Optionen gesteuert. WriteBufferMemoryPoolMinimumInKB gibt die Anfangsgröße der diesen Speicherpool und die geringstmögliche Größe, der Speicherpool verkleinern kann. WriteBufferMemoryPoolMaximumInKB ist die höchste Größe, der Speicherpool zunehmen kann. Replikats zuverlässig geöffnet wird erhöht die Größe des Speicherpools ein System bestimmt Betrag bis WriteBufferMemoryPoolMaximumInKB. Wenn weitere für den Speicher aus dem Pool Nachfrage als verzögert Anträge Speicher bis Arbeitsspeicher verfügbar ist. Daher ist der Pufferpool Speicher schreiben zu klein für eine bestimmte Konfiguration möglicherweise Leistung beeinträchtigt.

Die Eigenschaften SharedLogId und SharedLogPath werden immer gemeinsam verwendet GUID und Speicherort für das freigegebene Standardprotokoll für alle Knoten im Cluster definiert. Freigegebene Standardprotokoll wird für alle zuverlässige Dienste verwendet, die nicht die Einstellungen in der settings.xml für Dienst angeben. Für eine optimale Leistung sollte freigegebene Dateien auf Datenträgern befinden, die ausschließlich für die freigegebene Datei zu Konflikten verwendet werden.

SharedLogSizeInMB gibt den Speicherplatz für freigegebene Standardprotokoll auf allen Knoten vorab zugeordnet.  SharedLogId und SharedLogPath müssen nicht angegeben werden, für die SharedLogSizeInMB angegeben werden.


## <a name="service-specific-configuration"></a>Dienstspezifische Konfiguration
Statusbehaftete zuverlässige Dienste Standardkonfigurationen können das Konfigurationspaket (Config) oder Service-Implementierung (Code).

+ **Config** - Konfiguration über die Config-Paket erfolgt durch Ändern der Datei Settings.XML ausgeführt, die in Microsoft Visual Studio Paketstamm im Konfigurationsordner für jeden Dienst in der Anwendung generiert wird.
+ **Code** - Konfiguration über Code erfolgt durch Erstellen einer ReliableStateManager mit einem ReliableStateManagerConfiguration-Objekt mit den gewünschten Optionen.

Azure Service Fabric-Laufzeit standardmäßig vordefinierten Abschnittsnamen in der Datei Settings.xml gesucht und verwendet die Werte beim Erstellen des zugrunde liegenden Laufzeitkomponenten.

>[AZURE.NOTE] **Löschen Sie den Abschnitt der folgenden Konfigurationen in der Settings.xml Dateinamen,** in der Visual Studio-Projektmappe generiert, sofern Sie den Dienst über Code konfigurieren.
Die Config-Paket oder Abschnitt Namen umbenennen müssen Code geändert, beim Konfigurieren der ReliableStateManager.


### <a name="replicator-security-configuration"></a>Replicator-Sicherheitskonfiguration
Replicator Sicherheitskonfigurationen werden verwendet, um den Kommunikationskanal, der während der Replikation verwendet wird. Dies bedeutet, dass Dienste nicht sehen die Replikationsdaten hochverfügbar ist Daten auch sicher ist. Standardmäßig verhindert dass eine leere Sicherheitskonfigurationsabschnitt Replication Security.

### <a name="default-section-name"></a>Standardname für Abschnitt
ReplicatorSecurityConfig

>[AZURE.NOTE] Zum Ändern dieser Abschnittsname Überschreiben des ReplicatorSecuritySectionName-Parameters für den ReliableStateManagerConfiguration-Konstruktor beim Erstellen der ReliableStateManager für diesen Dienst.


### <a name="replicator-configuration"></a>Replicator-Konfiguration
Replicator konfigurieren den Replikator obliegt die statusbehaftete zuverlässigen Service Status zuverlässige replizieren und den Zustand lokal beibehalten wird.
Die Standardkonfiguration von der Visual Studio-Vorlage generiert und sollte ausreichen. Über zusätzliche Konfigurationen zur Optimierung des Replikators erörtert.

### <a name="default-section-name"></a>Standardname für Abschnitt
ReplicatorConfig

>[AZURE.NOTE] Zum Ändern dieser Abschnittsname Überschreiben des ReplicatorSettingsSectionName-Parameters für den ReliableStateManagerConfiguration-Konstruktor beim Erstellen der ReliableStateManager für diesen Dienst.


### <a name="configuration-names"></a>Konfigurationsnamen
|Name|Einheit|Standardwert|Beschreibung|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0,015|Zeitraum für die Replicator am sekundären wartet nach Erhalt einen Vorgang vor dem Senden an einen primären. Andere Empfangsbestätigungen für Vorgänge innerhalb dieses Intervalls gesendet werden, werden als eine Antwort gesendet.|
|ReplicatorEndpoint|N/A|Keine Standard - erforderliche parameter|IP-Adresse und Port Replicator primäre/sekundäre mit der Kommunikation mit anderen Replikatoren im Replikat festlegen. Dies sollte einen Ressource TCP-Endpunkt in das Manifest verweisen. Erhalten [Service Manifestressourcen](service-fabric-service-manifest-resources.md) mehr Ressourcen in einem Dienstmanifest definieren. |
|MaxPrimaryReplicationQueueSize|Anzahl der Vorgänge|8192|Maximale Anzahl von Vorgängen in der primären Warteschlange. Ein Vorgang ist frei, nach dem primäre Replicator von sekundären Replikatoren eine Bestätigung erhält. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl der Vorgänge|16384|Maximale Anzahl von Vorgängen in der sekundären Warteschlange. Ein Vorgang ist frei nach Zustand durch Persistenz hoch verfügbar zu machen. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|CheckpointThresholdInMB|MB|50|Menge Platz in der Protokolldatei nach dem der Status angefallen ist.|
|MaxRecordSizeInKB|KB|1024|Größte Datensatzgröße, die der Replikator in das Protokoll schreiben kann. Dieser Wert muss ein Vielfaches von 4 und 16 größer sein.|
|MinLogSizeInMB|MB|0 (System bestimmt)|Minimale Größe des Transaktions Protokolls. Das Protokoll kann dann nicht auf eine Größe unterhalb dieses Werts abgeschnitten. 0 gibt an, dass der Replikator die minimale Größe bestimmen. Dieser Wert erhöht die Möglichkeit Teilkopien und inkrementellen Sicherungen seit Erfolgschancen relevanten Protokolleinträge abgeschnitten gesenkt wird.|
|TruncationThresholdFactor|Faktor|2|Legt fest, welche Größe des Protokolls Abschneiden ausgelöst wird. MinLogSizeInMB TruncationThresholdFactor multipliziert Abschneideschwelle bestimmt. TruncationThresholdFactor muss größer als 1 sein. MinLogSizeInMB * TruncationThresholdFactor kleiner als MaxStreamSizeInMB sein.|
|ThrottlingThresholdFactor|Faktor|4|Legt fest, welche Größe des Protokolls, das Replikat startet, gedrosselt. Drosselung Schwellenwert (in MB) festgelegten Max ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Drosselung Schwellenwert (in MB) muss größer als der Schwellenwert (in MB) abgeschnitten. Abschneideschwelle (in MB) muss kleiner als MaxStreamSizeInMB sein.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|Max. kumulierte Größe (in MB) der Sicherungsprotokolle in einer bestimmten sicherungsprotokollkette. Eine inkrementelle Sicherung Anfragen schlägt fehl, wenn die inkrementelle Sicherung ein Sicherungsprotokoll, die kumulierte Sicherungsprotokolle seit der entsprechenden vollständigen Sicherung generieren würde größer als diese Größe wäre. In solchen Fällen muss Benutzer zu einer vollständigen Sicherung.|
|SharedLogId|GUID|""|Gibt eine eindeutige GUID zum Identifizieren der freigegebenen Protokolldatei mit diesem Replikat verwendet. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Wenn SharedLogId angegeben wird, muss SharedLogPath auch angegeben werden.|
|SharedLogPath|Vollqualifizierter Pfad|""|Gibt den vollqualifizierten Pfad die freigegebenen Datei dieses Replikat Erstellungsort. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Wenn SharedLogPath angegeben wird, muss SharedLogId auch angegeben werden.|
|SlowApiMonitoringDuration|Sekunden|300|Setzt das Überwachung Intervall für verwaltete API-Aufrufe. Beispiel: vom Benutzer bereitgestellte Rückruffunktion backup. Nach Ablauf des Intervalls wird ein Integritätsbericht Warnung an den Status-Manager gesendet.|

### <a name="sample-configuration-via-code"></a>Beispielkonfiguration mit code
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Beispiel-Konfigurationsdatei
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>Beschreibung
BatchAcknowledgementInterval steuert die Replikationswartezeit. Der Wert "0" führt die niedrigste Latenz, Durchsatz Kosten (wie weitere Bestätigungsnachrichten gesendet und verarbeitet, mit weniger Empfangsbestätigungen).
Je größer der Wert für BatchAcknowledgementInterval, je höher der Replikation Gesamtdurchsatz, aber höhere Latenz der Operation. Dies entspricht direkt der Latenz der Transaktion festgeschrieben.

Der Wert für CheckpointThresholdInMB steuert die Speichermenge, die der Replikator Zustandsinformationen in das Replikat dedizierte Protokolldatei speichern kann. Erhöhung dieser auf einen höheren Wert als den Standardwert möglich Neukonfiguration schneller als ein neues Replikat der Gruppe hinzugefügt wird. Dies ist aufgrund der partiellen Zustand, die Verfügbarkeit weiterer Verlauf der Vorgänge im Protokoll stattfindet. Dies kann die Recovery-Zeit von einem Replikat möglicherweise nach einem Absturz erhöhen.

Die MaxRecordSizeInKB-Einstellung definiert die maximale Größe eines Datensatzes, die vom Replikator in die Protokolldatei geschrieben werden. In den meisten Fällen ist die Standardgröße 1024 Eintrag optimal. Allerdings der Dienst größere Datenelemente zu Zustandsinformationen verursacht, müssen diesen Wert erhöht werden. Gibt es wenig Vorteile bei MaxRecordSizeInKB kleiner als 1024, kleinere Datensätze nur des erforderlichen Speicherplatzes für die kleineren Datensatz verwenden. Wir erwarten, dass dieser Wert nur selten geändert werden.

Eigenschaften SharedLogId und SharedLogPath werden immer gemeinsam verwendet ein Dienst einen separaten freigegebenen aus freigegebenen Standardprotokoll für den Knoten verwenden. Für optimale Effizienz sollten so viele Dienste möglichst gleiche freigegebene Protokoll angeben. Freigegebene Dateien sollten auf Datenträgern befinden, die ausschließlich für die freigegebene Datei zu Leseköpfe Konflikte verwendet werden. Wir erwarten, dass dieser Wert nur selten geändert werden.

## <a name="next-steps"></a>Nächste Schritte
 - [Debuggen der Service Fabric-Anwendung in Visual Studio](service-fabric-debugging-your-application.md)
 - [Zuverlässige Dienste-Entwicklerreferenz](https://msdn.microsoft.com/library/azure/dn706529.aspx)
