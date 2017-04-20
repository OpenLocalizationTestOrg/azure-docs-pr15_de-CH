<properties
   pageTitle="Service Fabric-Anwendungsmodell | Microsoft Azure"
   description="Das Modell und Programme und Dienste in Service Fabric beschreiben."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor="mani-ramaswamy"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/10/2016"   
   ms.author="seanmck"/>

# <a name="model-an-application-in-service-fabric"></a>Modellieren einer Anwendung in Service Fabric

Dieser Artikel bietet eine Übersicht über das Anwendungsmodell Azure Service Fabric. Er beschreibt auch Anwendung als Dienst über Manifestdateien zu definieren und die Anwendung gepackt und einsatzbereit.

## <a name="understand-the-application-model"></a>Verstehen der Anwendung

Eine Anwendung ist eine Auflistung der einzelnen Dienste, die eine bestimmte Funktion oder Funktionen ausführen. Ein Dienst führt eine vollständige und eigenständige Funktion (gestartet und unabhängig von anderen Diensten ausgeführt) und besteht aus Code, Konfiguration und Daten. Jeder Dienst Code besteht aus ausführbaren Binärdateien bestehen aus beliebigen statischen Daten vom Dienst verbraucht werden und Konfiguration umfasst Einstellungen, die zur Laufzeit geladen werden können. Jede Komponente dieses hierarchische Anwendung möglich versioniert und aktualisierte unabhängig voneinander.

![Das Anwendungsmodell Service Fabric][appmodel-diagram]


Ein Anwendungstyp ist eine Kategorisierung einer Anwendung und ein Bündel von Diensttypen aus. Ein Service ist eine Kategorisierung eines Dienstes. Die Kategorisierung haben unterschiedliche Einstellungen und Konfigurationen, aber die Kernfunktionalität bleibt unverändert. Instanzen eines Diensts sind unterschiedliche Service Configuration Varianten des gleichen Diensttyp.  

Klassen (oder "Typen") von Programmen und Diensten werden durch XML-Dateien (Anwendungsmanifeste und Service Manifeste) Vorlagen für die Anwendung aus dem Cluster Abbildspeicher instanziiert werden. Die Schemadefinition für die ServiceManifest.xml und ApplicationManifest.xml wird mit Service Fabric-SDK Tools *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*installiert.

Der Code für verschiedene Instanzen werden als separate Prozesse selbst wenn derselbe Service Fabric-Knoten gehostet ausgeführt. Darüber hinaus kann jede Anwendungsinstanz Lebenszyklus (d. h. verwaltet werden aktualisiert) unabhängig. Das folgende Diagramm zeigt, wie Anwendungstypen Diensttypen, bestehen die wiederum Code, Konfiguration und Pakete bestehen. Um das Diagramm zu vereinfachen, Pakete nur die Code / / Konfigurationsdaten für `ServiceType4` werden angezeigt, wenn jeder Diensttyp einige zählen oder alle Pakettypen.

![Service Fabric Anwendungstypen und Typen][cluster-imagestore-apptypes]

Zwei verschiedene Manifestdateien dienen auch beschreiben: Dienstmanifest und Anwendungsmanifest. Diese werden in den folgenden Abschnitten ausführlich behandelt.

Es können eine oder mehrere Instanzen eines Diensttyps im Cluster aktiv sein. So erzielen statusbehaftete Dienstinstanzen oder Replikate Zuverlässigkeit durch Replikation Zustand zwischen Replikaten auf anderen Knoten im Cluster gespeichert. Diese Replikation Redundanz im Wesentlichen den Dienst zur Verfügung, selbst wenn einer der Knoten in einem Cluster ausfällt. [Partitionierten Dienst](service-fabric-concepts-partitioning.md) weiter teilt seine Status (und Zugriffsmuster, Status) auf Knoten im Cluster.

Das folgende Diagramm zeigt die Beziehung zwischen Anwendung und Dienstinstanzen, Partitionen und Replikate.

![Partitionen und Replikate innerhalb eines Dienstes][cluster-application-instances]


>[AZURE.TIP] Anzeigen des Layouts der Anwendung in einem Cluster Service Fabric-Explorer Tools mit http://&lt;Yourclusteraddress&gt;: 19080-Explorer. Weitere Informationen finden Sie unter [Visualisierung Cluster mit Service Fabric-Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="describe-a-service"></a>Beschreiben Sie eine Dienstleistung

Das Manifest definiert deklarativ Diensttyp und Version. Es gibt Metadaten wie Diensttyp, Zustandseigenschaften Lastenausgleich Metriken, Binärdateien und Konfigurationsdateien.  Anders ausgedrückt, beschreibt es Code, Konfiguration und Datenpakete, die ein Service-Paket eine oder mehrere Diensttypen Unterstützung bilden. Hier ist ein einfaches Beispiel Dienstmanifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
~~~

**Versionsattribute** unstrukturierte Zeichenfolgen und nicht vom System analysiert. Sind verwendet jede Komponente für Upgrades.

**ServiceTypes** erklärt, welche Diensttypen in diesem Manifest von **CodePackages** unterstützt werden. Wenn ein Dienst mit einer der folgenden Diensttypen instanziiert wird, werden alle Pakete in diesem Manifest deklariert mit ihren Einstiegspunkte aktiviert. Die resultierende Prozesse sollen unterstützten Diensttypen Laufzeit zu registrieren. Beachten Sie, dass Diensttypen auf manifest und nicht die Paket-Ebene deklariert werden. So gibt es mehrere Pakete, sind sie alle aktiviert, wenn das System eines der deklarierten Typen gesucht.

**SetupEntryPoint** ist eine privilegierte, die mit den gleichen Anmeldeinformationen als Dienst (normalerweise das Konto *LocalSystem* ) vor anderen Einstiegspunkt ausgeführt wird. Die ausführbare Datei **EntryPoint** angegeben ist normalerweise langer Diensthosts. Das Vorhandensein einer anderen Setup-Einstiegspunkt vermeidet Diensthost für längere Zeit mit hohen Berechtigungen ausgeführt. Die ausführbare Datei **EntryPoint** angegeben wird ausgeführt, nachdem **SetupEntryPoint** erfolgreich beendet. Der daraus resultierende Prozess überwacht und neu gestartet (beginnend mit **SetupEntryPoint**) jemals beendet oder stürzt ab.

**DataPackage** deklariert einen Ordner durch das **Name** -Attribut, das zur Laufzeit vom Prozess verbraucht werden statische Daten enthält.

**ConfigPackage** deklariert einen Ordner durch das **Name** -Attribut, das eine Datei *Settings.xml* enthält. Diese Datei enthält Abschnitte Einstellungen benutzerdefinierte, Schlüssel-Wert-Paar, das der Prozess zur Laufzeit lesen kann. Während der Aktualisierung nur die **ConfigPackage** **Version** geändert wurde, wird anschließend ausgeführte Prozess nicht neu gestartet. Stattdessen benachrichtigt ein Rückruf den Prozess Konfiguration geändert haben, sodass sie dynamisch neu geladen werden können. Hier ist eine *Settings.xml* :

~~~
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSecion">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
~~~

> [AZURE.NOTE] Ein Servicemanifest kann mehrere Code-Konfiguration und Datenpakete enthalten. Diese kann separat versioniert werden.

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Beschreiben einer Anwendung


Das Anwendungsmanifest beschreibt deklarativ den Anwendungstyp und die Version. Es gibt Komposition Metadaten wie stabil, Partitionierung Schema, Instanz Count-Replikation Faktor Sicherheit-Isolation Policy, platzierungsbedingungen, Konfiguration überschreibt und Konstituierende Typen. Lastenausgleich Domänen in die Anwendung eingefügt werden ebenfalls beschrieben.

So ein Anwendungsmanifest beschreibt auf Anwendungsebene und verweist auf eine oder mehrere Service-Manifeste um einen Anwendungstyp verfassen. Hier ist ein einfaches Beispiel Anwendungsmanifest:

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
~~~

Wie dienstmanifeste **Versionsattribute** unstrukturierte Zeichenfolgen und nicht vom System analysiert. Auch verwendet jede Komponente für Upgrades.

**ServiceManifestImport** enthält Verweise auf Service-Manifeste, die dieser Anwendungstyp bilden. Importierte Service Manifeste ermitteln welche Diensttypen innerhalb dieses Anwendungstyps gültig sind.

**DefaultServices** deklariert Dienstinstanzen, die automatisch erstellt werden, wenn eine Anwendung anhand dieser Anwendungstyp instanziiert wird. Standardmäßig sind nur ein Hilfsmittel und in jeder Hinsicht wie normale Verhalten, nachdem sie erstellt wurden. Sie werden zusammen mit anderen Diensten in der Anwendungsinstanz aktualisiert und können auch entfernt werden.

> [AZURE.NOTE] Ein Anwendungsmanifest kann mehrere Service manifest Einfuhren und Standarddienste enthalten. Jeder Dienst manifest importieren kann separat versioniert werden.

Wie andere Anwendung und Parameter für einzelne Unternehmen finden Sie unter [Verwalten von Anwendungsparameter für mehrere Unternehmen](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->

## <a name="package-an-application"></a>Packen einer Anwendung

### <a name="package-layout"></a>Bildpaket-layout

Das Anwendungsmanifest/die Anwendungsmanifeste Service und anderen erforderlichen Dateien müssen in einem bestimmten Layout für die Bereitstellung in einem Cluster Service Fabric organisiert werden. Die Manifeste Beispiel in diesem Artikel müssen organisiert werden, die folgende Verzeichnisstruktur:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
~~~

Die Ordner werden entsprechend das Attribut **Name** der entsprechenden Elemente benannt. Z. B. wenn das Manifest zwei Pakete mit den Namen **MyCodeA** und **MyCodeB**enthalten, dann zwei Ordner mit demselben Namen erforderlichen Binärdateien für jedes Paket enthält.

### <a name="use-setupentrypoint"></a>SetupEntryPoint verwenden

Folgende gängige Szenarien für die Verwendung von **SetupEntryPoint** sind müssen Sie eine ausführbare Datei vor dem Ausführen oder einen Vorgang mit erhöhten Rechten ausführen müssen. Zum Beispiel:

- Das Einrichten und Initialisieren von Variablen, die die ausführbare Datei. Dies ist nicht nur ausführbare Dateien über den Service Fabric-Programmiermodellen geschrieben auf. Beispielsweise benötigt npm.exe einige Umgebungsvariablen zum Bereitstellen einer node.js-Anwendung konfiguriert.

- Zugriffskontrolle einrichten durch Installieren von Sicherheitszertifikaten.

### <a name="build-a-package-by-using-visual-studio"></a>Erstellen eines Pakets mit Visual Studio

Verwenden Sie Visual Studio 2015 zum Erstellen einer Anwendung, können den Paket-Befehl Sie automatisch ein Paket erstellen, das oben beschriebene Layout entspricht.

Erstellen eines Pakets mit der rechten Maustaste im Projektmappen-Explorer im Anwendungsprojekt und wählen den Befehl Paket wie folgt:

![Packen einer Anwendung mit Visual Studio][vs-package-command]

Nach Abschluss der Verpackung finden Sie im **Ausgabefenster** den Speicherort des Pakets. Beachten Sie, dass der Verpackung Schritt automatisch erfolgt beim Bereitstellen oder Debuggen einer Anwendung in Visual Studio.

### <a name="test-the-package"></a>Testen des Pakets

Sie können lokal über PowerShell-Paketstruktur **Test ServiceFabricApplicationPackage** Befehl überprüfen. Dieser Befehl überprüft Manifest Probleme analysieren und überprüfen Sie alle. Beachten Sie, dass dieser Befehl nur strukturelle Korrektheit der Verzeichnisse und Dateien im Paket überprüft. Es wird nicht überprüfen eines Paketinhalt Code oder Daten über überprüfen, ob alle erforderliche Dateien vorhanden sind.

~~~
PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
~~~

Dieser Fehler zeigt, dass die *MySetup.bat* -Datei in das Manifest **SetupEntryPoint** referenziert das Paket fehlt. Nach die fehlende Datei hinzugefügt wird, übergibt der Anwendung überprüfen:

~~~
PS D:\temp> tree /f .\MyApplicationType

D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat

PS D:\temp> Test-ServiceFabricApplicationPackage .\MyApplicationType
True
PS D:\temp>
~~~

Die Anwendung ordnungsgemäß verpackt und Überprüfung übergibt, ist einsatzbereit.

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellung und Anwendung entfernen][10]

[Anwendung Parameter für mehrere Unternehmen][11]

[RunAs: Ausführen einer Anwendung Service Fabric mit verschiedenen Berechtigungen][12]

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png
[vs-package-command]: ./media/service-fabric-application-model/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
