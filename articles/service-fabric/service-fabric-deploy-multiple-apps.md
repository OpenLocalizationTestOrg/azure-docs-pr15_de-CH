<properties
   pageTitle="Bereitstellen eine Anwendung Node.js, MongoDB verwendet | Microsoft Azure"
   description="Exemplarische Vorgehensweise zum Paket mehrere ausführbare Dateien Gast Azure Service Fabric-Cluster bereitstellen"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Bereitstellen Sie mehrerer Gast ausführbare Dateien

Dieser Artikel veranschaulicht das Verpacken und Bereitstellen von ausführbaren Dateien mehrere Gast Azure Service Fabric. Erstellen und Bereitstellen von Service Fabric-Paket lesen [ausführbare Service Fabric Gast](service-fabric-deploy-existing-app.md)bereitstellen.

Während dieser exemplarischen Vorgehensweise veranschaulicht, wie eine Anwendung mit einem Node.js bereitstellen, MongoDB als Datenspeicher verwendet, können Sie die Schritte für jede Anwendung anwenden, die eine andere Anwendung abhängig ist.   

Visual Studio können Sie das Anwendungspaket erstellen, das mehrere Gast ausführbaren Dateien. Finden Sie unter [Visual Studio verwenden, um eine vorhandene Anwendung](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Nachdem Sie die erste Gast ausführbare Datei hinzugefügt haben, klicken Sie mit der rechten Maustaste auf das Anwendungsprojekt, und wählen Sie **Hinzufügen neuer Service Fabric Service ->** zweite ausführbare Gast-Projekt der Projektmappe hinzufügen. Hinweis: Möchten Sie die Quelle im Visual Studio-Projekt verknüpfen, wird die Visual Studio-Projektmappe erstellen sicherstellen, dass das Anwendungspaket mit Änderungen in der Quelle ist. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>Manuelles Verpacken Sie mehrere Gast ausführbare Anwendung
Alternativ können Sie manuell den ausführbare Gast packen. Für das manuelle Verpacken verwendet dieser Artikel an [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool)Service Fabric-Tool Verpackung.

### <a name="packaging-the-nodejs-application"></a>Packen der Node.js-Anwendung
Es wird vorausgesetzt, dass Node.js auf Knoten im Cluster Service Fabric nicht installiert ist. Folglich müssen Sie Node.exe in das Stammverzeichnis der Anwendung vor dem Packen Knoten hinzufügen. Die Verzeichnisstruktur der Anwendung Node.js (mit Express Webframework Jade Vorlagenmodul) sollte ähnlich der folgenden aussehen:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Als Nächstes erstellen Sie ein Anwendungspaket für Node.js-Anwendung. Im folgenden Code wird ein Service Fabric Anwendungspaket, Node.js-Anwendung enthält.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Es folgt eine Beschreibung der Parameter, die verwendet werden:

- **Source** verweist auf das Verzeichnis der Anwendung verpackt werden soll.
- **/ target** definiert das Verzeichnis, in dem das Paket erstellt werden soll. Dieses Verzeichnis muss das Quellverzeichnis unterscheidet.
- **/ appname** definiert den Anwendungsnamen der bestehenden Anwendung. Es ist wichtig zu verstehen, dass dies den Dienstnamen im Manifest und nicht auf den Namen der Service Fabric übersetzt.
- definiert die ausführbare Datei Fabric Service in diesem Fall starten soll, **/exe** `node.exe`.
- **NetShield** definiert das Argument, das zum Starten des Programms verwendet wird. Node.js nicht installiert ist, muss Service Fabric starten Node.js-Webserver ausführen `node.exe bin/www`.  `/ma:'bin/www'`weist das Verpackung Tool mit `bin/ma` als Argument für node.exe.
- **/AppType** definiert den Typnamen Service Fabric-Anwendung.

Wenn Sie das Verzeichnis durchsuchen, die in der/Target-Parameter angegeben wurde, sehen Sie, dass das Tool eine voll funktionsfähige Service Fabric-Paket erstellt hat, wie folgt:

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Generierte ServiceManifest.xml hat jetzt einen Abschnitt, der beschreibt wie Webserver Node.js gestartet werden soll, wie im folgenden Codeausschnitt dargestellt:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
In diesem Beispiel überwacht Node.js-Webserver auf Port 3000, müssen Sie den Endpunkt in der Datei ServiceManifest.xml wie folgt aktualisieren.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>Packen der Anwendung MongoDB
Nun, Node.js-Anwendung verpackt haben, können Sie fortfahren und MongoDB-Paket. Wie bereits erwähnt, sind die Schritte, die jetzt durchlaufen nicht spezifisch Node.js und MongoDB. Sie gelten für alle Programme, die zusammen als ein Service Fabric-Anwendung verpackt werden sollen.  

Um MongoDB packen möchten Sie sicherstellen, dass Mongod.exe und Mongo.exe zu verpacken. Beide Binärdateien befinden sich der `bin` des Installationsverzeichnisses MongoDB Verzeichnis. Die Verzeichnisstruktur ähnelt ein.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric muss MongoDB mit einem Befehl wie dem folgenden starten, müssen Sie mithilfe der `/ma` Parameter beim MongoDB.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Die Daten werden nicht bei Ausfall eines Knotens erhalten Wenn Sie MongoDB Datenverzeichnis auf das lokale Verzeichnis des Knotens. Sie sollten dauerhaften Speicher oder implementieren eine Replikatgruppe MongoDB um Datenverluste zu vermeiden.  

In PowerShell oder die Befehlsshell ausführen wir die Verpackung mit den folgenden Parametern:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Um das Anwendungspaket Service Fabric MongoDB hinzufügen, müssen Sie sicherstellen, daß der/Target-Parameter verweist in dasselbe Verzeichnis, das die Anwendung bereits enthält Node.js-Anwendung. Außerdem müssen Sie sicherstellen, dass gleichnamige Anwendungstyps.

Wir wechseln Sie zum Verzeichnis und untersuchen, was das Tool erstellt wurde.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Wie Sie sehen können, hinzugefügt mit dem neuen Ordner MongoDB, das Verzeichnis mit MongoDB Binärdateien Beim Öffnen der `ApplicationManifest.xml` Datei können Sie sehen, dass das Paket jetzt die Anwendung Node.js und MongoDB enthält. Der folgende Code zeigt den Inhalt des Anwendungsmanifests.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Veröffentlichen der Anwendungdes
Der letzte Schritt ist die Anwendung der lokale Dienstfabric Cluster mithilfe von PowerShell-Skripts unten veröffentlichen:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Nach Veröffentlichung der Anwendung erfolgreich zu dem lokalen Cluster, möglich Node.js-Anwendung den Anschluss, die wir in das Manifest der Anwendung Node.js - Beispiel http://localhost: 3000 eingegeben haben.

In diesem Lernprogramm wurde gezeigt, wie zwei vorhandene Applikationen einfach als ein Service Fabric-Anwendung verpacken. Sie haben auch gelernt, Fabric Service bereitzustellen, damit sie einige der Funktionen Fabric Service Health Systemintegration wie hohe Verfügbarkeit profitieren.

## <a name="next-steps"></a>Nächste Schritte

- Zur Bereitstellung mit [Service und Container (Übersicht)](service-fabric-containers-overview.md)
