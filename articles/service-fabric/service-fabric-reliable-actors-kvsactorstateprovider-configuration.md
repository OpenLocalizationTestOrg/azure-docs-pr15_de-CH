<properties
   pageTitle="Übersicht über die Konfiguration von Azure Service Fabric zuverlässige Akteure KVSActorStateProvider | Microsoft Azure"
   description="Informationen Sie zur Konfiguration von Azure Service Fabric statusbehafteten Akteure vom Typ KVSActorStateProvider."
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
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Zuverlässige Akteure - KVSActorStateProvider konfigurieren
Sie können die Standardkonfiguration des KVSActorStateProvider durch Ändern der Datei Settings.XML ausgeführt, die in Microsoft Visual Studio Paketstamm im Konfigurationsordner für den angegebenen Akteur generiert wird.

Azure Service Fabric-Laufzeit sucht nach vordefinierten Abschnittsnamen in der Datei settings.xml und nutzt die Konfigurationswerte beim Erstellen des zugrunde liegenden Laufzeitkomponenten.

>[AZURE.NOTE] Führen Sie **nicht** löschen oder ändern Sie die Abschnittsnamen der folgenden Konfigurationen in der Datei Settings.XML ausgeführt, die in der Visual Studio-Projektmappe generiert.

## <a name="replicator-security-configuration"></a>Replicator-Sicherheitskonfiguration
Replicator Sicherheitskonfigurationen werden verwendet, um den Kommunikationskanal, der während der Replikation verwendet wird. Dies bedeutet, dass Dienste nicht gegenseitig Replikationsverkehr, sicherstellen, dass die Daten hochverfügbar ist auch sicher angezeigt werden.
Standardmäßig verhindert dass eine leere Sicherheitskonfigurationsabschnitt Replication Security.

### <a name="section-name"></a>Abschnitt
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Replicator-Konfiguration
Replicator konfigurieren den Replikator dafür Akteur Anbieters Zustand zuverlässige zuständig ist.
Die Standardkonfiguration von der Visual Studio-Vorlage generiert und sollte ausreichen. Über zusätzliche Konfigurationen zur Optimierung des Replikators erörtert.

### <a name="section-name"></a>Abschnitt
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfigurationsnamen

|Name|Einheit|Standardwert|Beschreibung|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekunden|0,015|Zeitraum für die Replicator am sekundären wartet nach Erhalt einen Vorgang vor dem Senden an einen primären. Andere Empfangsbestätigungen für Vorgänge innerhalb dieses Intervalls gesendet werden, werden als eine Antwort gesendet.|
|ReplicatorEndpoint|N/A|Keine Standard - erforderliche parameter|IP-Adresse und Port Replicator primäre/sekundäre mit der Kommunikation mit anderen Replikatoren im Replikat festlegen. Dies sollte einen Ressource TCP-Endpunkt in das Manifest verweisen. Erhalten [Service Manifestressourcen](service-fabric-service-manifest-resources.md) mehr zum Definieren von Ressourcen in das Manifest. |
|RetryInterval|Sekunden|5|Zeitraum, nach dem der Replikator eine Nachricht erneut überträgt erhält er keine Bestätigung für einen Vorgang.|
|MaxReplicationMessageSize|Bytes|50 MB|Maximale Größe der Replikationsdaten in einer einzelnen Nachricht übertragen werden können.|
|MaxPrimaryReplicationQueueSize|Anzahl der Vorgänge|1024|Maximale Anzahl von Vorgängen in der primären Warteschlange. Ein Vorgang ist frei, nach dem primäre Replicator von sekundären Replikatoren eine Bestätigung erhält. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|
|MaxSecondaryReplicationQueueSize|Anzahl der Vorgänge|2048|Maximale Anzahl von Vorgängen in der sekundären Warteschlange. Ein Vorgang ist frei nach Zustand durch Persistenz hoch verfügbar zu machen. Dieser Wert muss größer als 64 und eine Potenz von 2 sein.|

## <a name="store-configuration"></a>Speicherkonfiguration
Store-Konfigurationen wird den lokalen Speicher konfigurieren, der verwendet wird, um den Zustand beizubehalten, der repliziert wird.
Die Standardkonfiguration von der Visual Studio-Vorlage generiert und sollte ausreichen. In diesem Abschnitt spricht über zusätzliche Konfigurationen zur lokalen Speicher optimieren.

### <a name="section-name"></a>Abschnitt
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfigurationsnamen

|Name|Einheit|Standardwert|Beschreibung|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Millisekunden|200|Maximale Intervall permanenten lokalen Speicher Commits Batchverarbeitung festgelegt.|
|MaxVerPages|Anzahl der Seiten|16384|Die maximale Anzahl der Seiten in der lokalen Version speichern Datenbank. Bestimmt die maximale Anzahl von Transaktionen.|

## <a name="sample-configuration-file"></a>Beispiel-Konfigurationsdatei

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
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
