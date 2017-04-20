<properties
   pageTitle="Übersicht über die Konfiguration von Azure Service Fabric zuverlässige Akteure ReliableDictionaryActorStateProvider | Microsoft Azure"
   description="Informationen Sie zur Konfiguration von Azure Service Fabric statusbehafteten Akteure vom Typ ReliableDictionaryActorStateProvider."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/18/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--reliabledictionaryactorstateprovider"></a>Zuverlässige Akteure - ReliableDictionaryActorStateProvider konfigurieren
Die Standardkonfiguration des ReliableDictionaryActorStateProvider können durch Ändern der Datei settings.xml in Visual Studio Paketstamm im Konfigurationsordner für den angegebenen Akteur.

Azure Service Fabric-Laufzeit sucht nach vordefinierten Abschnittsnamen in der Datei settings.xml und nutzt die Konfigurationswerte beim Erstellen des zugrunde liegenden Laufzeitkomponenten.

>[AZURE.NOTE] Führen Sie **nicht** löschen oder ändern Sie die Abschnittsnamen der folgenden Konfigurationen in der Datei Settings.XML ausgeführt, die in der Visual Studio-Projektmappe generiert.

Es gibt auch Globale Einstellungen die Konfiguration der ReliableDictionaryActorStateProvider.

## <a name="global-configuration"></a>Konfiguration

Die Konfiguration wird im clustermanifest für den Cluster im Abschnitt KtlLogger angegeben. Es ermöglicht die Konfiguration der freigegebenen Standort und Größe sowie die globale Speichergrenzen von der Protokollierung verwendet. Beachten Sie, dass das clustermanifest alle Dienste, die ReliableDictionaryActorStateProvider verwenden und zuverlässige statusbehaftete Dienste beeinflussen.

Das clustermanifest ist eine einzelne XML-Datei, die Einstellungen und Konfigurationen, die für alle Knoten und Dienste im Cluster gelten. Die Datei heißt in der Regel ClusterManifest.xml. Cluster Siehe manifest für den Cluster mithilfe der Get-ServiceFabricClusterManifest-Powershell-Befehl.

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

## <a name="replicator-security-configuration"></a>Replicator-Sicherheitskonfiguration
Replicator Sicherheitskonfigurationen werden verwendet, um den Kommunikationskanal, der während der Replikation verwendet wird. Dies bedeutet, dass Dienste können nicht die Replikationsdaten hochverfügbar ist Daten auch sicher ist.
Standardmäßig verhindert dass eine leere Sicherheitskonfigurationsabschnitt Replication Security.

### <a name="section-name"></a>Abschnitt
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicator-Konfiguration
Replicator-Konfigurationen wird den Replikator konfigurieren, obliegt Akteur Anbieters Zustand zuverlässige Replikation und lokal den Zustand beizubehalten.
Die Standardkonfiguration von der Visual Studio-Vorlage generiert und sollte ausreichen. Über zusätzliche Konfigurationen zur Optimierung des Replikators erörtert.

### <a name="section-name"></a>Abschnitt
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurationsnamen

|Name|Einheit|Standardwert|Beschreibung|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0,015|Zeitraum für die Replicator am sekundären wartet nach Erhalt einen Vorgang vor dem Senden an einen primären. Andere Empfangsbestätigungen für Vorgänge innerhalb dieses Intervalls gesendet werden, werden als eine Antwort gesendet.||
|ReplicatorEndpoint|N/A|Keine Standard - erforderliche parameter|IP-Adresse und Port Replicator primäre/sekundäre mit der Kommunikation mit anderen Replikatoren im Replikat festlegen. Dies sollte einen Ressource TCP-Endpunkt in das Manifest verweisen. Erhalten [Service Manifestressourcen](service-fabric-service-manifest-resources.md) mehr Ressourcen im Dienstmanifest definieren. |
|MaxReplicationMessageSize|Bytes|50 MB|Maximale Größe der Replikationsdaten in einer einzelnen Nachricht übertragen werden können.|
|MaxPrimaryReplicationQueueSize|Anzahl der Vorgänge|8192|Maximale Anzahl von Vorgängen in der primären Warteschlange. Ein Vorgang ist frei, nach dem primäre Replicator von sekundären Replikatoren eine Bestätigung erhält. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl der Vorgänge|16384|Maximale Anzahl von Vorgängen in der sekundären Warteschlange. Ein Vorgang ist frei nach Zustand durch Persistenz hoch verfügbar zu machen. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|CheckpointThresholdInMB|MB|200|Menge Platz in der Protokolldatei nach dem der Status angefallen ist.|
|MaxRecordSizeInKB|KB|1024|Größte Datensatzgröße, die der Replikator in das Protokoll schreiben kann. Dieser Wert muss ein Vielfaches von 4 und 16 größer sein.|
|OptimizeLogForLowerDiskUsage|Boolescher Wert|true|Bei true ist das Protokoll so konfiguriert, dass das Replikat dedizierte Protokolldatei erstellt wird, mit einer NTFS-Datei mit geringer Datendichte. Dies senkt die tatsächliche Datenträgerspeicherplatz-Nutzung für die Datei. Wenn false, wird die Datei mit festen, erstellt bieten die beste Leistung schreiben.|
|SharedLogId|GUID|""|Gibt eine eindeutige Guid zum Identifizieren der freigegebenen Protokolldatei mit diesem Replikat verwendet. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Wenn SharedLogId angegeben wird, muss SharedLogPath auch angegeben werden.|
|SharedLogPath|Vollqualifizierter Pfad|""|Gibt den vollqualifizierten Pfad die freigegebenen Datei dieses Replikat Erstellungsort. Normalerweise müssen Dienste nicht diese Einstellung verwenden. Wenn SharedLogPath angegeben wird, muss SharedLogId auch angegeben werden.|


## <a name="sample-configuration-file"></a>Beispiel-Konfigurationsdatei

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="180" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
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

## <a name="remarks"></a>Beschreibung
Der Parameter BatchAcknowledgementInterval steuert Replikationswartezeit. Der Wert "0" führt die niedrigste Latenz, Durchsatz Kosten (wie weitere Bestätigungsnachrichten gesendet und verarbeitet, mit weniger Empfangsbestätigungen).
Je größer der Wert für BatchAcknowledgementInterval, je höher der Replikation Gesamtdurchsatz, aber höhere Latenz der Operation. Dies entspricht direkt der Latenz der Transaktion festgeschrieben.

Der Parameter CheckpointThresholdInMB steuert die Speichermenge, die der Replikator Zustandsinformationen in das Replikat dedizierte Protokolldatei speichern kann. Erhöhung dieser auf einen höheren Wert als den Standardwert möglich Neukonfiguration schneller als ein neues Replikat der Gruppe hinzugefügt wird. Dies ist aufgrund der partiellen Zustand, die Verfügbarkeit weiterer Verlauf der Vorgänge im Protokoll stattfindet. Dies kann die Recovery-Zeit von einem Replikat möglicherweise nach einem Absturz erhöhen.

Wenn Sie OptimizeForLowerDiskUsage auf True festgelegt, wird Platz in der Protokolldatei über bereitgestellt, sodass active Replikate Weitere Informationen in die Protokolldateien gespeichert werden inaktive Replikate weniger Speicherplatz verwenden können. Dies ermöglicht mehrere Replikate auf einem Knoten enthalten. Wenn OptimizeForLowerDiskUsage auf False festgelegt, wird die Zustandsinformationen schneller in die Protokolldateien geschrieben.

Die MaxRecordSizeInKB-Einstellung definiert die maximale Größe eines Datensatzes, die vom Replikator in die Protokolldatei geschrieben werden. In den meisten Fällen ist die Standardgröße 1024 Eintrag optimal. Allerdings der Dienst größere Datenelemente zu Zustandsinformationen verursacht, müssen diesen Wert erhöht werden. Gibt es wenig Vorteile bei MaxRecordSizeInKB kleiner als 1024, kleinere Datensätze nur des erforderlichen Speicherplatzes für die kleineren Datensatz verwenden. Wir erwarten, dass dieser Wert nur in seltenen Fällen geändert werden.

Eigenschaften SharedLogId und SharedLogPath werden immer gemeinsam verwendet ein Dienst einen separaten freigegebenen aus freigegebenen Standardprotokoll für den Knoten verwenden. Für optimale Effizienz sollten so viele Dienste möglichst gleiche freigegebene Protokoll angeben. Freigegebene Dateien sollten auf Datenträgern befinden, die ausschließlich für die freigegebene Protokolldatei zu Leseköpfe Konflikte verwendet werden. Wir erwarten, dass diese Werte nur in seltenen Fällen geändert werden müssen.
