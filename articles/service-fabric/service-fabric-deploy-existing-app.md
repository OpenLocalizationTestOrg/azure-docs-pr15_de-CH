<properties
   pageTitle="Bereitstellen eine vorhandene ausführbaren Datei Azure Service Fabric | Microsoft Azure"
   description="Exemplarische Vorgehensweise zur Verwendung eine vorhandene Anwendung als Gast ausführbaren Paket so zu einem Cluster Service Fabric bereitgestellt werden kann"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>Ausführbare Gast Fabric Service bereitstellen

Sie können jede Art von Anwendung node.js, Java oder systemeigene Anwendung in Azure Service Fabric ausführen. Service Fabric bezeichnet diese Anwendungstypen als Gast ausführbare Dateien.
Gast ausführbare Dateien werden wie zustandsloser Dienste durch Service behandelt. Daher werden sie auf Knoten in einem Cluster, je nach Verfügbarkeit und andere platziert. Dieser Artikel beschreibt das Verpacken und Bereitstellen von ausführbaren Gast zu einem Service Fabric-Cluster mithilfe von Visual Studio oder ein Befehlszeilen-Dienstprogramm.

In diesem Artikel untersuchen wir die Schritte zum Verpacken ausführbare Gast und Fabric Service bereitstellen.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Ausführen von ausführbaren Service Fabric Gast

Es gibt mehrere Vorteile bei der Ausführung in einem Cluster Service Fabric ausführbare Gast:

- Hohe Verfügbarkeit. In Service Fabric ausgeführt Applications sind hochgradig verfügbar. Fabric Service wird sichergestellt, dass Instanzen einer Anwendung ausgeführt werden.
- Health monitoring. Service Fabric Überwachung erkennt, wenn eine Anwendung ausgeführt wird und Diagnose-Informationen ist ein Fehler.   
- Anwendungslebenszyklus-Verwaltung. Neben Upgrades ohne Ausfallzeiten, bietet Service Fabric automatischen Rollback auf die vorherige Version wird eine falsche Ereignis während einer Aktualisierung.    
- Dichte. In einem Cluster entfallen für jede Anwendung auf eigener Hardware ausgeführt, können Sie mehrere Programme ausführen.


## <a name="overview-of-application-and-service-manifest-files"></a>Übersicht und Manifestdateien service

Als Teil der Bereitstellung von ausführbaren Gast ist Service Fabric Verpackung verstehen und Bereitstellungsmodell als beschrieben [Anwendungsmodell](service-fabric-application-model.md). Service Fabric-Paketerstellungsmodell beruht auf zwei XML-Dateien: Manifeste Anwendung und Service. Die Schemadefinition für die ApplicationManifest.xml und ServiceManifest.xml ist in *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*Service Fabric-SDK installiert.

* **Anwendungsmanifest** Das Anwendungsmanifest wird verwendet, um die Anwendung beschreiben. Die Dienste, die sie andere Parameter, die definieren sowie, wie ein oder mehrere Dienste bereitgestellt werden soll, wie die Anzahl der Instanzen aufgelistet.

  Fabric-Dienst ist eine Anwendung eine Bereitstellung und Aktualisierung. Eine Anwendung kann als einzelne Einheit aktualisiert werden, potenzielle Fehler und potenzielle Rollbacks verwaltet werden. Service Fabric garantiert, dass die Aktualisierung entweder Erfolg oder Fehlschlagen die Aktualisierung nicht die Anwendung unbekannt/instabil lassen.

* **Service manifest** Das Manifest beschreibt die Komponenten eines Dienstes. Sie enthält Daten wie Namen und Service, sowie der Code, Konfiguration und Daten. Das Manifest enthält auch einige zusätzliche Parameter, die verwendet werden können, um den Dienst zu konfigurieren, nachdem sie bereitgestellt wurde.


## <a name="application-package-file-structure"></a>Paket Datei Anwendungsstruktur
Zur Bereitstellung einer Anwendung mit Fabric Service benötigt die Anwendung eine vordefinierte Verzeichnisstruktur folgen. Im folgenden Beispiel dieser Struktur.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

Die ApplicationPackageRoot enthält die ApplicationManifest.xml-Datei, die die Anwendung definiert. Unterverzeichnis für jeden Dienst die Anwendung wird alle Artefakte, die der Dienst benötigt – der ServiceManifest.xml und in der Regel die folgenden drei Verzeichnisse enthalten:

- *Code*. Dieses Verzeichnis enthält den Code.
- *Config*. Dieses Verzeichnis enthält eine Datei Settings.xml (und andere Dateien, falls erforderlich) der Dienst zur Laufzeit Abrufen bestimmten Konfigurationen zugreifen kann.
- *Daten*. Dies ist ein zusätzliches Verzeichnis zusätzliche lokale Daten speichern, die möglicherweise der Dienst. Hinweis: Daten sollte zum Speichern temporärer Daten verwendet. Service Fabric ist nicht kopieren/replizieren sich das Datenverzeichnis sollte der Dienst – beispielsweise bei einem Failover verschoben werden.

Hinweis: Sie müssen erstellen die `config` und `data` Verzeichnisse, wenn Sie sie benötigen.

## <a name="packaging-an-existing-executable"></a>Eine vorhandene ausführbare Datei packen

Beim Packen einer ausführbaren Datei Gast können Sie entweder ein Visual Studio-Projektvorlage oder [Anwendungspaket manuell erstellen](#manually). Verwendung von Visual Studio werden Paket Anwendungsstruktur und manifest-Dateien durch den Assistenten für neue für Sie erstellt.

>[AZURE.NOTE] Die einfachste Methode zum Packen einer vorhandenen Windows ausführbare in einen Dienst ist mit Visual Studio.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Mithilfe von Visual Studio eine vorhandene ausführbare Datei packen

Visual Studio bietet eine Service Fabric Servicevorlage Gast ausführbare Service Fabric-Cluster bereitstellen.

Wechseln Sie mithilfe der folgenden Schritte die Veröffentlichung abgeschlossen:

1. Wählen Sie Datei -> Neues Projekt und Fabric erstellen.
2. Wählen Sie Gast ausführbare Datei als Dienstvorlage.
3. Klicken Sie auf Durchsuchen, wählen Sie den Ordner mit der ausführbaren Datei und füllen Sie die restlichen Parameter zum Erstellen des Dienstes.
    - *Code Paket Verhalten* kann festgelegt werden, kopieren Sie den Inhalt des Ordners Visual Studio-Projekt die ist nützlich, wenn die ausführbare Datei nicht geändert wird. Wenn die ausführbare Datei zu ändern und die Möglichkeit, neue Builds dynamisch erwartet, können Sie die Verknüpfung mit dem Ordner. Beachten Sie, dass Sie beim Erstellen des Anwendungsprojekts in Visual Studio verknüpfte Ordnern verwenden können. Diese Links zu den Quellspeicherort innerhalb des Projekts ermöglichen Sie ausführbare Gast in der Quelle-Ziel aktualisieren müssen Teil des Anwendungspakets Build Updates.
    - *Anwendung* – wählen Sie die ausführbare Datei zum Starten des Diensts ausgeführt werden soll.
    - *Argumente* - Geben Sie die Argumente, die an die ausführbare Datei übergeben werden sollen. Eine Liste mit Argumenten möglich.
    - *WorkingFolder* - gibt das Arbeitsverzeichnis für den Prozess gestartet werden soll. Sie können drei Werte:
        - `CodeBase`Gibt an, dass das Arbeitsverzeichnis auf das Verzeichnis im Anwendungspaket wird (`Code` Verzeichnis in der Dateistruktur angezeigt vor).
        - `CodePackage`Gibt an, dass das Arbeitsverzeichnis auf den Stamm des Anwendungspakets festgelegt wird (`GuestService1Pkg` in der Dateistruktur angezeigt vor).
        - `Work`Gibt an, dass die Dateien in ein Unterverzeichnis Arbeit befinden
4. Benennen Sie dem Dienst, und klicken Sie auf OK.
5. Wenn Ihr Dienst einen Endpunkt für die Kommunikation benötigt, können Sie jetzt Protokoll, Port und Typ ServiceManifest.xml-Datei hinzufügen. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Sie können nun das Paket und veröffentlichen Aktion gegen den lokalen Cluster Debuggen der Projektmappe in Visual Studio. Wenn Sie veröffentlichen Sie die Anwendung auf einem Remotecluster oder die Projektmappe zur Versionskontrolle einchecken.
7. Wechseln Sie zum Ende dieses Artikels zu sehen, wie Sie Gast ausführbare im Service Fabric-Explorer anzeigen.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Manuelles Verpacken und Bereitstellen einer vorhandenen ausführbaren Datei
Der Prozess des Verpackens manuell einer ausführbare Gast basiert auf die folgenden Schritte aus:

1. Erstellen der Verzeichnisstruktur Paket.
2. Fügen Sie der Anwendung Code und Konfiguration.
3. Bearbeiten Sie die Manifestdatei Service.
4. Bearbeiten Sie die Anwendungsmanifestdatei.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Erstellen der Verzeichnisstruktur Paket
Sie können zunächst Erstellen der Verzeichnisstruktur wie oben beschrieben.

### <a name="add-the-applications-code-and-configuration-files"></a>Fügen Sie der Anwendung Code und Konfiguration
Nach dem Erstellen der Verzeichnisstruktur können Sie den Anwendungscode und Dateien in den Verzeichnissen Code und Konfiguration hinzufügen. Sie können auch zusätzliche Verzeichnisse und Unterverzeichnisse unter Code oder Config-Verzeichnisse erstellen.

Service Fabric tut Xcopy Inhalt des Stammverzeichnisses der Anwendung keine vordefinierte Struktur als Erstellen von beiden obersten Verzeichnisse, Code und Einstellungen verwenden. (Sie können unterschiedliche Namen wählen, wenn Sie möchten. Weitere Informationen werden im nächsten Abschnitt.)

>[AZURE.NOTE] Stellen Sie sicher, dass Sie alle Dateien/Abhängigkeiten der Anwendung. Service Fabric kopiert den Inhalt des Anwendungspakets auf allen Knoten im Cluster, wohin die Anwendungsdienste bereitgestellt werden. Das Paket sollte den Code, den die Anwendung zur Ausführung benötigt. Wir empfehlen nicht, sofern die Abhängigkeiten installiert sind.

### <a name="edit-the-service-manifest-file"></a>Die Dienstmanifestdatei bearbeiten
Der nächste Schritt ist der Dienst manifest Datei um folgende Angaben:

- Der Name des Diensttyps. Dies ist eine ID Service Fabric verwendet, um einen Dienst zu identifizieren.
- Der Befehl zum Starten der Anwendung (ExeHost) verwenden.
- Jedes Skript, das ausgeführt werden, legen Sie oben/konfigurieren Sie die Anwendung (SetupEntrypoint).

Im folgenden ist ein Beispiel für eine `ServiceManifest.xml` Datei:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Sehen wir uns die verschiedenen Teile der Datei, die Sie aktualisieren möchten:

#### <a name="updating-the-servicetypes"></a>Aktualisieren der ServiceTypes

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Wählen Sie alle Namen für `ServiceTypeName`. Der Wert wird verwendet, der `ApplicationManifest.xml` Datei des Dienstes.
- Sie müssen angeben `UseImplicitHost="true"`. Dieses Attribut weist Fabric-Dienst, dass der Dienst eine eigenständige Anwendung basiert auf alle Service Fabric muss als Prozess gestartet und dessen Zustand überwacht wird.

#### <a name="updating-the-codepackage"></a>Aktualisieren der CodePackage
Das CodePackage-Element gibt den Speicherort und Version der Dienstcode.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Die `Name` Element wird verwendet, um den Namen des Verzeichnisses im Anwendungspaket angeben, die Service-Code enthält. `CodePackage`auch die `version` Attribut. Dadurch wird die Version des Codes – und kann auch zum Aktualisieren der Dienstcode mit Fabric Service Application Lifecycle Management-Infrastruktur.
#### <a name="optional-updating-the-setupentrypoint"></a>Optional: Aktualisieren der SetupEntrypoint

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Das SetupEntrypoint-Element wird verwendet, eine ausführbare Datei oder Batchdatei Datei an, die vor dem Start des Diensts Code ausgeführt werden soll. Es ist optional, sodass es nicht notwendig ist, wenn keine Initialisierung/Setup erforderlich ist. Die SetupEntryPoint wird jedes Mal ausgeführt, der Dienst neu gestartet wird.

Gibt es nur eine SetupEntrypoint müssen Skripts Setup-Konfiguration erfordert die Anwendung Setup/Config mehrere Skripts in einem einzigen Stapelverarbeitungsprogramm gruppiert werden. Die SetupEntrypoint kann beliebige Dateitypen – ausführbare Dateien, Batchdateien und PowerShell-Cmdlets ausführen. Weitere Informationen finden Sie unter [SetupEntryPoint konfigurieren](service-fabric-application-runas-security.md).

Im vorhergehenden Beispiel führt die SetupEntrypoint eine Batchdatei namens `LaunchConfig.cmd` , befindet sich in der `scripts` Unterverzeichnis des Codes (WorkingFolder Element soll CodeBase vorausgesetzt).

#### <a name="updating-the-entrypoint"></a>Aktualisieren der Einstiegspunkt

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Die `Entrypoint` Element in der Dienstmanifestdatei angeben, wie der Dienst verwendet. Die `ExeHost` Element gibt die ausführbare Datei (und Argumente) zum Starten des Dienstes verwendet werden sollte.

- `Program`Gibt den Namen der ausführbaren Datei, die zum Starten des Diensts ausgeführt werden soll.
- `Arguments`Gibt die Argumente, die an die ausführbare Datei übergeben werden sollen. Eine Liste mit Argumenten möglich.
- `WorkingFolder`Gibt das Arbeitsverzeichnis für den Prozess gestartet werden soll. Sie können drei Werte:
    - `CodeBase`Gibt an, dass das Arbeitsverzeichnis auf das Verzeichnis im Anwendungspaket wird (`Code` in der vorhergehenden Dateistruktur Verzeichnis).
    - `CodePackage`Gibt an, dass das Arbeitsverzeichnis auf den Stamm des Anwendungspakets festgelegt wird (`GuestService1Pkg` in der vorhergehenden Dateistruktur).
  - `Work`Gibt an, dass die Dateien in ein Unterverzeichnis Arbeit befinden

Die WorkingFolder eignet sich das richtige Arbeitsverzeichnis festlegen, dass relative Pfade durch die Anwendung oder Initialisierung Skripts verwendet werden können.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>Aktualisieren der Endpunkte und für die Kommunikation mit Naming Service registrieren

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Im vorherigen Beispiel der `Endpoint` Element gibt die Endpunkte die Anwendung überwachen kann. In diesem Beispiel überwacht die Anwendung Node.js http Port 3000.

Wenden Sie außerdem Service Fabric diesen Endpunkt für den Namensdienst veröffentlichen, damit andere Dienste die Endpunktadresse für diesen Dienst finden. Dadurch können zwischen den Diensten kommunizieren Gast ausführbare Dateien.
Die veröffentlichten Endpunktadresse hat die Form `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`und `PathSuffix` sind optionale Attribute. `IPAddressOrFQDN`ist die IP-Adresse oder den vollqualifizierten Domänennamen des Knotens diese ausführbare Datei gebracht wird und für Sie berechnet.

Im folgenden Beispiel nach der Bereitstellung des Dienstes im Explorer Fabric Service Sie sehen einen Endpunkt mit `http://10.1.4.92:3000/myapp/` für die Dienstinstanz veröffentlicht oder die lokalen Computer angezeigt `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
[Reverse Proxy](service-fabric-reverseproxy.md) können diese Adressen Sie zwischen Diensten kommunizieren.

### <a name="edit-the-application-manifest-file"></a>Die Anwendungsmanifestdatei bearbeiten

Nach der Konfiguration der `Servicemanifest.xml` -Datei müssen Sie zum Ändern der `ApplicationManifest.xml` um sicherzustellen, dass die richtigen Dienst und Namen verwendet werden.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

In der `ServiceManifestImport` Element, Sie können einen oder mehrere Dienste, die Sie in die Anwendung aufnehmen möchten. Services wird mit `ServiceManifestName`, gibt den Namen des Verzeichnisses an, in dem die `ServiceManifest.xml` befindet.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Protokollierung einrichten
Gast-Programmdateien eignet sich die sehen konsolenprotokolle, um herauszufinden, ob die Anwendung und Konfiguration Skripts Fehler anzeigen.
Umleitung der Konsole konfiguriert werden kann die `ServiceManifest.xml` mithilfe der `ConsoleRedirection` Element.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`verwendet Ausgabe in der Konsole (Stdout und Stderr) in ein Arbeitsverzeichnis umleiten, damit sie verwendet werden können, um sicherzustellen, dass keine Fehler bei der Installation oder Ausführung der Anwendung im Cluster Service Fabric.

    * `FileRetentionCount`Bestimmt, wie viele Dateien im Arbeitsverzeichnis gespeichert werden. Der Wert 5, also beispielsweise Protokolldateien für die vorherigen fünf Einheiten im Arbeitsverzeichnis gespeichert werden.
    * `FileMaxSizeInKb`Gibt die maximale Größe der Protokolldateien an.

Protokolldateien werden in einem Arbeitsverzeichnisse des Dienstes gespeichert. Um festzustellen, wo sich die Dateien befinden, müssen Sie mit Service Fabric-Explorer bestimmt die Knoten, dass der Dienst ausgeführt wird und welche Arbeitsverzeichnis verwendet wird. Dieser Prozess wird weiter unten in diesem Artikel behandelt.

## <a name="deployment"></a>Bereitstellung
Der letzte Schritt ist die Anwendung bereitstellen. PowerShell-Skript veranschaulicht, wie die Anwendung lokale Entwicklung-Cluster bereitstellen und einen neuen Service Fabric-Dienst starten.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
Service Fabric-Dienst kann in verschiedenen "Konfigurationen" bereitgestellt werden Beispielsweise als einzelner oder mehrerer Instanzen bereitgestellt werden, oder so, dass eine des Dienstes auf jedem Knoten des Clusters Fabric Service Instanz bereitgestellt werden.

Die `InstanceCount` Parameter der `New-ServiceFabricService` Cmdlet wird verwendet, um anzugeben, wie viele Instanzen des Dienstes im Cluster Service Fabric gestartet werden soll. Legen Sie die `InstanceCount` Wert je nach Anwendung, die Sie bereitstellen. Die beiden häufigsten Szenarien sind:

* `InstanceCount = "1"`. In diesem Fall wird nur eine Instanz des Dienstes im Cluster bereitgestellt. Service Fabric Planer bestimmt die Knoten der Dienst bereitgestellt werden soll.

* `InstanceCount ="-1"`. In diesem Fall wird eine Instanz des Dienstes auf jedem Knoten im Cluster Service Fabric bereitgestellt. Das Ergebnis hat (und einzige) eine Instanz des Dienstes für jeden Knoten im Cluster.

Dies ist eine nützliche Konfiguration für Front-End-Applikationen (z. B. einen REST-Endpunkt) da Clientanwendungen müssen auf allen Knoten im Cluster mit dem Endpunkt "Verbinden". Diese Konfiguration kann auch verwendet werden, wenn beispielsweise alle Knoten des Clusters Fabric Service ein Lastenausgleich verbunden sind, sodass Datenverkehr von den Clients über den Dienst verteilt werden kann, die auf allen Knoten im Cluster ausgeführt wird.

## <a name="check-your-running-application"></a>Überprüfen Sie Ihre Anwendung

Identifizieren Sie im Service Fabric-Explorer den Knoten, in dem der Dienst ausgeführt wird. In diesem Beispiel wird er auf Node1:

![Knoten, in dem Dienst ausgeführt wird](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Navigieren Sie zum Knoten und wechseln Sie zur Anwendung grundlegende Knoteninformationen, beispielsweise zu deren Speicherort auf der Festplatte angezeigt.

![Speicherort auf dem Datenträger](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Wenn Sie mit dem Server-Explorer zu dem Verzeichnis wechseln, finden das Arbeitsverzeichnis Sie und des Dienstes Ordner wie im folgenden Bild gezeigt.

![Speicherort der Protokolldateien](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie das Paket einer ausführbare Gast und Fabric Service bereitstellen. Als Nächstes können Sie Zusatzinformationen zu diesem Thema Auschecken.

- [Beispiel für das Packen und Bereitstellen von Gast basierend auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), eine Verknüpfung mit der Vorabversion des Verpackungswerkzeugs
- [Bereitstellen Sie mehrerer Gast ausführbare Dateien](service-fabric-deploy-multiple-apps.md)
- [Erstellen Sie Ihre erste Service Fabric-Anwendung mit Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
