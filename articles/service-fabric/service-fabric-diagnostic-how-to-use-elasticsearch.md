<properties
   pageTitle="Mit Elasticsearch als Service Fabric-Anwendung Trace Shop | Microsoft Azure"
   description="Beschreibt, wie Service Fabric Applikationen mit Elasticsearch und Kibana speichern, Index und suchen Anwendung Spuren (Protokolle)"
   services="service-fabric"
   documentationCenter=".net"
   authors="karolz-ms"
   manager="adegeo"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/09/2016"
   ms.author="karolz@microsoft.com"/>

# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Mit Elasticsearch als Service Fabric-Anwendung Spur speichern
## <a name="introduction"></a>Einführung
Dieser Artikel beschreibt, wie [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) Applikationen **Elasticsearch** und **Kibana** für Anwendungsspeicher Trace, Indizierung und Suche. [Elasticsearch](https://www.elastic.co/guide/index.html) ist eine Open Source, verteilte und skalierbare Echtzeit Suche und Analyse-Engine, die für diese Aufgabe geeignet ist. Es kann auf Windows und Linux Microsoft Azure auf virtuellen Computern installiert werden. Elasticsearch kann *strukturierte* Spuren erzeugt Technologien wie **Event Tracing for Windows (ETW)**verarbeiten.

ETW wird von Service Fabric Laufzeit diagnostische Informationen (Spuren) verwendet. Es ist die empfohlene Methode für Service Fabric Applikationen die Diagnoseinformationen zu beziehen. Derselben Mechanismus ermöglicht Korrelation zwischen Common Language Runtime bereitgestellten und Anwendung bereitgestellten Spuren und Problembehebung wird erleichtert. Service Fabric-Projektvorlagen in Visual Studio enthalten eine Protokollierung API (basierend auf .NET **EventSource** -Klasse), die ETW-Spuren standardmäßig ausgibt. Eine allgemeine Übersicht über Service Fabric Anwendung Verfolgung in ETW finden Sie unter [Überwachung und Diagnose-Services in einer lokalen Entwicklung Einrichtung](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Die Spuren im Elasticsearch angezeigt werden müssen sie Service Fabric-Clusterknoten in Echtzeit aufgezeichnet (während die Anwendung ausgeführt wird) und einen Elasticsearch-Endpunkt an. Es gibt zwei Hauptoptionen für Trace erfassen:

+ **Prozess Trace erfassen**  
Die Anwendung oder genauer Dienstprozess ist verantwortlich für das Senden von Diagnosedaten an den Trace-Speicher (Elasticsearch).

+ **Out-of-Process-Trace erfassen**  
Separater Agent Spuren Dienstprozess oder Prozesse erfassen und Trace-Shop senden.

Nachstehend beschreiben wir wie Elasticsearch auf Azure einrichten, besprechen Sie die Vorteile und Nachteile für beide Optionen erfassen und erläutert, wie Sie Service Fabric-Dienst zum Senden von Daten an Elasticsearch konfigurieren.


## <a name="set-up-elasticsearch-on-azure"></a>Elasticsearch auf Azure einrichten
Am einfachsten einrichten Elasticsearch Dienst auf Azure ist [**Azure Ressourcenmanager**](../azure-resource-manager/resource-group-overview.md)Vorlagen. Eine umfassende [Schnellstart Azure Ressourcenmanager Vorlage für Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) ist Azure Schnellstart Vorlagen-Repository verfügbar. Diese Vorlage verwendet separate Speicherkonten für Maßeinheiten (Gruppen von Knoten). Sie können auch separate Client und Server-Knoten mit unterschiedlichen Konfigurationen und Anzahl der Datenträger angeschlossen bereitstellen.

Hier verwenden Sie eine andere Vorlage **ES MultiNode** von [Azure Diagnosetools](https://github.com/Azure/azure-diagnostics-tools)aufgerufen. Diese Vorlage ist einfacher zu verwenden und erstellt einen Elasticsearch Cluster von HTTP-Standardauthentifizierung geschützt. Bevor Sie fortfahren, herunterladen Sie das Repository von GitHub auf den Computer (durch Klonen Repository oder eine Zip-Datei herunterladen). ES MultiNode-Vorlage befindet sich im Ordner mit dem gleichen Namen.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Vorbereiten einer Maschine Elasticsearch Installationsskripts
Verwenden ES MultiNode-Vorlage am einfachsten durch eine bereitgestellte Azure PowerShell-Skript namens `CreateElasticSearchCluster`. Zur Verwendung dieses Skripts müssen Sie PowerShell-Module und **Openssl**Tools installieren. Diese wird benötigt, zum Erstellen von SSH-Schlüssel, der zur Remoteverwaltung von Elasticsearch Cluster verwendet werden kann.

`CreateElasticSearchCluster`Skript dient zur Bedienung ES MultiNode-Vorlage aus einem Windows-Computer. Kann die Vorlage auf einem Windows-Computer verwenden, aber dieses Szenario ist nicht Gegenstand dieses Artikels.

1. Wenn Sie sie bereits installiert haben, installieren Sie [**Azure PowerShell-Module**](http://aka.ms/webpi-azps). Wenn Sie aufgefordert werden, klicken Sie auf **Ausführen**, **Installieren**. Azure PowerShell 1.3 oder höher ist erforderlich.

2. **Openssl** -Tool ist in der [**Git für Windows**](http://www.git-scm.com/downloads)-Distribution enthalten. Wenn dies noch nicht getan haben, installieren Sie jetzt [Git für Windows](http://www.git-scm.com/downloads) . (Die Standard-Installationsoptionen sind OK.)

3. Sofern Git installiert, aber nicht im Systempfad aufgenommen wurde, öffnen Sie ein Microsoft Azure PowerShell und führen Sie die folgenden Befehle:

    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```

    Ersetzen Sie die `<Git installation folder>` mit Git Speicherort auf dem Computer; Der Standardwert ist **"c:\Programme\Microsoft Files\Git"**. Beachten Sie das Semikolon am Anfang der erste Pfad.

4. Sicherstellen, dass Sie in Azure angemeldet sind (über [`Add-AzureRmAccount`](https://msdn.microsoft.com/library/mt619267.aspx) Cmdlet) und ausgewählten Abonnements verwendet elastische Suche Cluster erstellt werden soll. Dass richtige Abonnement ausgewählt wurde, können Sie `Get-AzureRmContext` und `Get-AzureRmSubscription` Cmdlets.

5. Wenn Sie dies noch nicht getan haben, ändern Sie das aktuelle Verzeichnis ES MultiNode-Ordner.

### <a name="run-the-createelasticsearchcluster-script"></a>Führen Sie das Skript CreateElasticSearchCluster
Öffnen Sie vor dem Ausführen des Skripts die `azuredeploy-parameters.json` Datei überprüfen und geben Sie Werte für die Skriptparameter. Die folgenden Parameter stehen zur Verfügung:

|Parametername           |Beschreibung|
|-----------------------  |--------------------------|
|Zeichenfolge |Der Name, mit dem sichtbaren DNS-Namen für den Cluster elastische Suche erstellen (die Domäne Azure Region bereitgestellten Namen anhängen). Wenn dieser Parameter "MyBigCluster" ist der gewählten Azure Region Westen der USA, ist der resultierende DNS-Namen für den Cluster myBigCluster.westus.cloudapp.azure.com. <br /><br />Dieser Name dient auch als einen Stammnamen für viele Cluster elastische suchen wie Daten Knoten zugeordnete Artefakte.|
|adminUsername           |Der Name des Administratorkontos für Clusternamen elastische suchen (entsprechende SSH-Schlüssel werden automatisch erzeugt).|
|dataNodeCount           |Die Anzahl der Knoten im Cluster elastische suchen. Die aktuelle Version des Skripts unterscheidet nicht zwischen Daten und Abfrage; alle Knoten Rolle sowohl. Der Standardwert ist 3 Knoten.|
|dataDiskSize            |Die Größe der Datenträger (in GB), die für jeden Datenknoten zugeordnet ist. Jeder Knoten erhält 4 Datenträger ausschließlich elastische Suchdienst.|
|Region                  |Der Name der Azure-Region, elastische Suche Cluster befinden soll.|
|esUserName              |Der Benutzername des Benutzers, der Zugriff auf ES Cluster (unter HTTP-Standardauthentifizierung) konfiguriert ist. Das Kennwort ist nicht Teil der Datei und muss bereitgestellt werden, wenn `CreateElasticSearchCluster` Skript aufgerufen.|
|vmSizeDataNodes         |Für Clusterknoten elastische Suche Azure VM-Größe. Der Standardwert ist Standard_D2.|

Jetzt können Sie das Skript ausführen. Geben Sie folgenden Befehl ein:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

wo 

|Skriptname Parameter    |Beschreibung|
|-----------------------  |--------------------------|
|`<es-group-name>`        |Der Name der Azure-Ressourcengruppe, die alle elastische Clusterressourcen enthalten.|
|`<azure-region>`         |Der Name der Azure-Region Clusters elastische suchen Erstellungsort.|         
|`<es-password>`          |Das Kennwort für den Benutzer elastische suchen.|

>[AZURE.NOTE] Man NullReferenceException Cmdlets Test-AzureResourceGroup vergessen haben Azure anmelden (`Add-AzureRmAccount`).

Das Skript ausführen, erhalten Sie eine Fehlermeldung und Sie feststellen, dass der Fehler durch eine falsche Vorlage Parameterwert verursacht wurde, korrigieren Sie die Datei und führen Sie das Skript erneut mit einer anderen Gruppe Ressourcenname. Sie verwenden die gleiche Gruppe Ressourcenname und das Skript Bereinigen der alten Datei durch Hinzufügen der `-RemoveExistingResourceGroup` Parameter an das Skript aufrufen.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Ergebnis der Ausführung des Skripts CreateElasticSearchCluster
Nach dem Ausführen der `CreateElasticSearchCluster` Skript die folgenden wichtigsten Artefakte erstellt werden. In diesem Beispiel davon ausgegangen, dass die Verwendung von "MyBigCluster" als Wert der `dnsNameForLoadBalancerIP` -Parameter und der Bereich, in dem Cluster erstellt, Westen der USA.

|Artefakt|Name, Speicherort und Beschreibung|
|----------------------------------|----------------------------------|
|SSH-Schlüssel für die Remoteverwaltung |myBigCluster.key Datei (im Verzeichnis, aus dem der CreateElasticSearchCluster ausgeführt wird). <br /><br />Diese Schlüsseldatei kann Knoten Admin und (über den Knoten Admin) Verbindung Daten Knoten im Cluster verwendet werden.|
|Admin-Knoten                        |MyBigCluster admin.westus.cloudapp.azure.com <br /><br />Eine dedizierte VM für die Remoteverwaltung Elasticsearch Cluster - das einzige, das externe SSH-Verbindung ermöglicht. Elasticsearch Dienste nicht ausgeführt, aber im gleichen virtuellen Netzwerk als die Elasticsearch-Clusterknoten ausgeführt.|
|Datenknoten                        |myBigCluster1... MyBigCluster*N* <br /><br />Datenknoten, die Elasticsearch und Kibana ausgeführt werden. Sie können über SSH für jeden Knoten, sondern nur über den Knoten Admin.|
|Elasticsearch cluster             |http://myBigCluster.westus.cloudapp.Azure.com/es/ <br /><br />Die primären Elasticsearch Cluster (Beachten Sie das Suffix/es einsetzen). Diese HTTP-Standardauthentifizierung geschützt (die Anmeldeinformationen wurden die angegebene EsUserName-EsPassword-Parameter ES MultiNode-Vorlage). Der Cluster hat auch Kopf plug-in installiert (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) für die Verwaltung von Clustern.|
|Kibana-Dienst                    |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Kibana-Dienst ist für die Daten aus der erstellten Elasticsearch Cluster eingerichtet. Es ist die gleiche Authentifizierungsinformationen Cluster selbst geschützt.|

## <a name="in-process-versus-out-of-process-trace-capturing"></a>Prozess und Out-of-Process-Trace erfassen
In der Einführung erwähnt zwei grundlegende Arten Sammeln von Diagnosedaten: prozessintern und Out-of-Process. Jeder hat Stärken und Schwächen.

**Prozess Trace erfassen** bietet folgende Vorteile:

1. *Einfache Konfiguration und Bereitstellung*

    * Diagnosedaten Auflistung ist nur in der Anwendungskonfiguration. Es ist einfach zu immer "synchron" mit dem Rest der Anwendung.

    * Pro Anwendung oder jeden Dienst Konfiguration ist einfach.

    * Out-of-Process-Trace erfassen muss normalerweise eine separate Bereitstellung und Konfiguration von Diagnose Agent eine zusätzliche administrative Aufgabe und eine potenzielle Fehlerquelle ist. Häufig kann bestimmte Agent-Technologie nur eine Instanz des Agents pro virtueller Maschine (Knoten). Das bedeutet die Konfiguration für die Auflistung der Diagnose Konfiguration unter Alle Programme Gemeinsame Dienste auf diesem Knoten ausgeführt.

2. *Flexibilität*

    * Die Anwendung die Daten senden, wo diese benötigt werden, als Client Bibliothek gezielte Datenspeichersystem unterstützt. Neue Empfänger hinzugefügt werden wie gewünscht.

    * Komplexe Regeln für Erfassung, Filtern und Daten-Aggregation können implementiert werden.

    * Ein Out-of-Process-Trace Erfassung wird häufig Daten senken beschränkt, die der Agent unterstützt. Einige Agenten sind erweiterbar.

3. *Zugriff auf interne Daten und Kontext*

    * Diagnose-Subsystem innerhalb der Anwendung bzw. der Dienst ausgeführt kann Spuren mit Kontextinformationen leicht erweitern.

    * Out-of-Process-Ansatz müssen die Daten für einen Agent über einige Kommunikationsmechanismus, wie Event Tracing for Windows gesendet. Dieser Mechanismus kann zusätzliche Einschränkungen vorsehen.

**Erfassen von prozessexternen Trace** bietet folgende Vorteile:

1. *Überwachen und sammeln Speicherabbilder*

    * Prozessinterne Trace erfassen kann nicht erfolgreich sein, wenn die Anwendung gestartet oder stürzt ab. Ein unabhängiger Agent ist eine viel bessere Chance erfassen wichtige Informationen zur Problembehandlung.<br /><br />

2. *Fälligkeit, Stabilität und bewährten performance*

    * Ein Agent einen Plattformanbieter (z. B. einem Microsoft Azure Diagnostics-Agent) entwickelt wurde strengen Tests und Schlacht Hardening.

    * Mit prozessinternen erfassen muss darauf geachtet werden, damit Aktivität Senden von Daten von einer Anwendung nicht stören die Anwendung Aufgaben oder Timing oder Leistungsproblemen führen. Ein unabhängig ausgeführt ist weniger anfällig für diese Probleme und speziell Auswirkung auf das System beschränken.

Es ist möglich, kombinieren und beide Ansätze. In der Tat möglicherweise die beste Lösung für viele Applikationen.

Hier verwenden wir **Microsoft.Diagnostic.Listeners Bibliothek** und prozessinterne Trace zum Senden von Daten aus einer Anwendung Service Fabric zu einem Cluster Elasticsearch erfassen.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Elasticsearch Daten an verwenden Sie die Listener-Bibliothek
Microsoft.Diagnostic.Listeners-Bibliothek ist Teil PartyCluster Beispiel Service Fabric-Anwendung. Verwendung:

1. [Das Beispiel PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) von GitHub herunterladen.

2. Kopieren Sie die Projekte Microsoft.Diagnostics.Listeners und Microsoft.Diagnostics.Listeners.Fabric (ganze Ordner) aus dem Beispielverzeichnis PartyCluster Projektmappenordner des Programms, das Senden der Daten an Elasticsearch.

3. Öffnen Sie die Projektmappe Ziel rechten Maustaste auf den Projektmappenknoten im Projektmappen-Explorer und wählen Sie **Vorhandenes Projekt hinzufügen**. Das Microsoft.Diagnostics.Listeners-Projekt der Projektmappe hinzufügen. Wiederholen Sie für das Projekt Microsoft.Diagnostics.Listeners.Fabric.

4. Zwei zusätzliche Projekte einen Projektverweis aus Ihrem Service-Projekte hinzufügen. (Jeder Dienst, das Senden von Daten an Elasticsearch sollten Microsoft.Diagnostics.EventListeners und Microsoft.Diagnostics.EventListeners.Fabric verweisen).

    ![Projektverweise auf Bibliotheken Microsoft.Diagnostics.EventListeners und Microsoft.Diagnostics.EventListeners.Fabric][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Fabric allgemeiner Verfügbarkeit Release und Microsoft.Diagnostics.Tracing Nuget-Paket
Mit Fabric allgemeiner Verfügbarkeit Release (2.0.135, 31. März 2016 veröffentlicht) als Ziel **.NET Framework 4.5.2**. Diese Version ist die höchste Version von.NET Framework bei GA-Release von Azure unterstützt. Leider fehlt diese Version von Framework bestimmte EventListener-APIs, die die Microsoft.Diagnostics.Listeners-Bibliothek. Da Ereignisquelle (die Komponente die Grundlage Fabric Applikationen APIs anmelden) und EventListener eng gekoppelt sind, muss jedes Projekt, das die Microsoft.Diagnostics.Listeners-Bibliothek verwendet eine alternative Implementierung der Ereignisquelle verwenden. Diese Implementierung bietet die **Microsoft.Diagnostics.Tracing Nuget-Paket**von Microsoft erstellt wurden. Das Paket ist vollständig abwärtskompatibel mit Quelle in Framework enthalten keine Änderungen nicht referenzierten Namespaces erforderlich ist.

Um mit der Microsoft.Diagnostics.Tracing Implementierung der Ereignisquelle-Klasse, für jeden Dienstprojekt, das Senden von Daten an Elasticsearch folgendermaßen Sie vor:

1. Mit der rechten Maustaste auf das Webdienstprojekt, und wählen Sie **Nuget-Pakete verwalten**.

2. Wechseln in die Paketquelle nuget.org (falls es nicht bereits aktiviert ist), und suchen Sie nach "**Microsoft.Diagnostics.Tracing**".

3. Installieren der `Microsoft.Diagnostics.Tracing.EventSource` -Paket (und die zugehörigen Dateien).

4. Öffnen Sie die Datei **ServiceEventSource.cs** oder **ActorEventSource.cs** in Ihrem Service-Projekt und ersetzen den `using System.Diagnostics.Tracing` Richtlinie auf die Datei mit den `using Microsoft.Diagnostics.Tracing` Richtlinie.

Diese Schritte werden nicht nach **.NET Framework 4.6** von Microsoft Azure unterstützt wird.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Elasticsearch Listener Instanziierung und Konfiguration
Der letzte Schritt zum Senden von Daten an Elasticsearch wird zum Erstellen einer Instanz des `ElasticSearchListener` und mit Elasticsearch Daten konfigurieren. Der Listener erfasst automatisch alle Ereignisse über EventSource Klassen im Projekt für den Dienst definiert. Erstellen Sie am besten im Initialisierungscode Service ist während der Lebensdauer der Dienst aktiv sein muss. Der Initialisierungscode für statusfreie Service nach ändern aussehen könnte (Additions hingewiesen Kommentare mit `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider);
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Elasticsearch Daten sollten in einem separaten Abschnitt in der Konfigurationsdatei Service (**PackageRoot\Config\Settings.xml**). Name des Abschnitts muss übergebene Wert entspricht der `FabricConfigurationProvider` Konstruktor beispielsweise:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Die Werte der `serviceUri`, `userName` und `password` Parameter entsprechen Elasticsearch Cluster Endpunktadresse, Elasticsearch-Benutzername und Kennwort. `indexNamePrefix`Das Präfix für Elasticsearch Indizes ist; die Bibliothek Microsoft.Diagnostics.Listeners erstellt einen neuen Index für die Daten täglich.

### <a name="verification"></a>Überprüfung
Das wars! Wenn der Dienst ausgeführt wird, wird eine Spuren auf den Elasticsearch-Dienst in der Konfiguration angegeben. Sie können überprüfen, dass dies durch Öffnen der Kibana UI die Zielinstanz Elasticsearch zugeordnet. In diesem Beispiel wird die Adresse http://myBigCluster.westus.cloudapp.azure.com/. Überprüfen des Namenpräfixes für ausgewählte Indizes der `ElasticSearchListener` Instanz tatsächlich erstellt und mit Daten gefüllt.

![Kibana mit PartyCluster-Anwendungsereignisse][2]

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zu Diagnose und Überwachung Service Fabric service](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
