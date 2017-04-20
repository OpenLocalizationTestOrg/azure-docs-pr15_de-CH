<properties
   pageTitle="Service Fabric-Endpunkte angeben | Microsoft Azure"
   description="Beschreiben von Ressourcen in einem Dienstmanifest wie HTTPS Endpunkte einrichten"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="specify-resources-in-a-service-manifest"></a>Geben Sie Ressourcen in einem Servicemanifest

## <a name="overview"></a>Übersicht

Das Manifest kann Ressourcen, die vom Dienst deklariert/geändert werden, ohne den kompilierten Code verwendet werden. Azure Service Fabric unterstützt die Konfiguration von Ressourcen für den Service. Der Zugriff auf die Ressourcen, die in das Manifest angegeben kann über SecurityGroup im Anwendungsmanifest gesteuert werden. Die Deklaration von Ressourcen kann diese Ressourcen bei der Bereitstellung bedeutet, dass der Dienst eine neue Konfiguration Mechanismus muss nicht geändert werden. Die Schemadefinition für die ServiceManifest.xml-Datei ist mit dem Service Fabric-SDK Tools *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*installiert.

## <a name="endpoints"></a>Endpunkte

Das Manifest eine Ressource Endpunkt definiert, weist Fabric Service Ports des Portbereichs reservierten Anwendung bei ein Port explizit angegeben wird. Betrachten Sie z. B. Endpunkt *ServiceEndpoint1* im manifest Ausschnitt nach diesem Absatz angegeben angegeben. Darüber hinaus können Dienste einen bestimmten Port in einer Ressource anfordern. Replikate auf anderen Clusterknoten können andere Port-Nummern zugewiesen werden, während Replikate eines Diensts auf demselben Knoten den Port freigeben. Service-Replikate können dann diese Ports nach Bedarf für Replikation und Warten auf Clientanforderungen.

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Anhand [Konfigurieren statusbehaftete zuverlässige Dienste](service-fabric-reliable-services-configuration.md) über Endpunkte aus der Config-Paket verweisen (settings.xml) Datei lesen.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Beispiel: einen HTTP-Endpunkt für den Dienst festlegen

Das folgende Manifest definiert einen TCP-Endpunkt Ressource und zwei HTTP-Ressourcen in die &lt;Ressourcen&gt; Element.

HTTP-Endpunkte sind automatisch ACL von Service Fabric.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Beispiel: einen HTTPS-Endpunkt für den Dienst festlegen

Das HTTPS-Protokoll bietet Authentifizierung und auch zum Verschlüsseln von Client-Server-Kommunikation verwendet. Um HTTPS für den Service Fabric-Dienst aktivieren, geben Sie das Protokoll in der *Ressourcen -> Endpunkte-Endpunkt >* Abschnitt Service, wie zuvor für den Endpunkt *ServiceEndpoint3*gezeigt.

>[AZURE.NOTE] Ein Dienstprotokoll kann während der anwendungsaktualisierung geändert werden, ohne es bildet eine unterbrechende Änderung.


Hier ist ein Beispiel ApplicationManifest HTTPS festlegen müssen. Fingerabdruck des Zertifikats muss angegeben werden. Die EndpointRef ist eine EndpointResource in ServiceManifest, für die das HTTPS-Protokoll festgelegt. Sie können mehrere EndpointCertificate hinzufügen.  

```
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="TestCert1" EndpointRef="ServiceEndpoint3"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="TestCert1" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```
