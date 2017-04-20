<properties 
pageTitle="Kommunikation in Cloud Services Rollen | Microsoft Azure" 
description="Instanzen im Cloud-Dienste können Endpunkte (http, Https, Tcp und Udp) definiert, die mit oder anderen Instanzen kommunizieren." 
services="cloud-services" 
documentationCenter="" 
authors="Thraka" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="09/06/2016" 
ms.author="adegeo"/>

# <a name="enable-communication-for-role-instances-in-azure"></a>Kommunikation für Instanzen in azure

Cloud-Dienstverwaltungsrollen kommunizieren über interne und externe Verbindungen. Externe Anschlüsse **input Endpunkte** heißen Verbindungen **interne Endpunkte**bezeichnet werden. Dieses Thema beschreibt die [Dienstdefinition](cloud-services-model-and-package.md#csdef) Endpunkte erstellen ändern.


## <a name="input-endpoint"></a>Input-Endpunkt
Input Endpunkt wird verwendet, wenn Sie einen Port nach außen offen legen möchten. Den Protokoll und den Port des Endpunkts gilt dann für die externe und interne Ports für den Endpunkt angeben. Wenn Sie möchten, soll einen anderen internen Anschluss für den Endpunkt mit dem Attribut [LokalerAnschluss](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) .

Input Endpunkt können die folgenden Protokolle: **http, Https, Tcp und Udp**.

**Endpunkte** Bestandteil einer Web-oder Arbeitskraft um input erstellen fügen Sie **InputEndpoint** untergeordnetes Element hinzu.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Instanz input Endpunkt
Instanz input Endpunkte ähneln Eingabe Endpunkte können Sie jedoch bestimmte öffentliche Ports für jede einzelne Rolleninstanz mit Lastenausgleich portweiterleitung zuordnen. Sie können einen einzelnen öffentlichen Port oder einen Portbereich angeben.

Instanz input Endpunkt kann nur **Tcp** oder **Udp** als Protokoll verwenden.

Erstellen Sie einen Instanz input Endpunkt **Endpunkte** Bestandteil einer Web-oder Arbeitskraft fügen Sie **InstanceInputEndpoint** untergeordnetes Element hinzu.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>Interne Endpunkt
Interne Endpunkte sind für Instanzen-Kommunikation. Der Anschluss ist optional und nicht angegeben, wird zum Endpunkt ein dynamischer Port zugewiesen. Portbereich kann verwendet werden. Maximal fünf interne Endpunkte pro Rolle ist.

Interne Endpunkt können die folgenden Protokolle: **http, Tcp, Udp, alle**.

Erstellen Sie einen internen input Endpunkt **Endpunkte** Bestandteil einer Web-oder Arbeitskraft fügen Sie **InternalEndpoint** untergeordnetes Element hinzu.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Sie können auch einen Portbereich.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Worker-Rollen und Webrollen

Ein kleiner Unterschied mit Endpunkten wird mit Arbeitskraft und Webrollen. Die Web-Rolle muss mindestens einen einzelnen Eingaben Endpunkt über **http** .


```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>.NET SDK verwenden auf einem Endpunkt
Azure verwaltete Bibliothek bietet Methoden für Instanzen zu Laufzeit. Code in einer Instanz der Rolle können Sie Informationen über das Vorhandensein von anderen Instanzen und Endpunkte sowie Informationen über die aktuelle Instanz der Rolle abrufen.

> [AZURE.NOTE] In der Cloud-Dienst ausgeführt werden und mindestens einen internen Endpunkt definieren, können Sie Informationen zu Instanzen nur abrufen. Daten über Instanzen aus einem anderen Dienst kann nicht abgerufen werden.

[Instanzen](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) -Eigenschaft können Sie Varianten einer Rolle. Zunächst mithilfe der [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) einen Verweis auf die aktuelle Instanz der Rolle zurückzugeben, und verwenden Sie die [Role](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) -Eigenschaft zum Zurückgeben eines Verweises auf die Rolle.

Beim Herstellen einer Rolleninstanz programmgesteuert über .NET SDK ist relativ einfach auf die Endpunktinformationen. Wenn Sie bereits eine bestimmte rollenumgebung verbunden sind, können Sie Anschluss einen bestimmten Endpunkt mit folgendem Code abrufen:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

Die **Instanzen** -Eigenschaft gibt eine Auflistung von **RoleInstance** -Objekten. Diese Auflistung enthält immer die aktuelle Instanz. Wenn die Rolle keinen internen Endpunkt definiert, enthält die Auflistung der aktuellen Instanz jedoch keine Instanzen. Die Anzahl der Instanzen in der Auflistung werden immer 1 in den Fällen, wo keine interner Endpunkt für die Rolle definiert ist. Rolle einen internen Endpunkt definiert, deren Instanzen werden zur Laufzeit sichtbar und die Anzahl der Instanzen in der Auflistung entspricht der Anzahl der Instanzen für die Rolle in der Service-Konfigurationsdatei angegeben.

> [AZURE.NOTE] Azure verwaltete Bibliothek bietet keine bestimmen, den Zustand der anderen Instanzen, aber diese Bewertungen Zustand selbst implementieren sollte der Dienst diese Funktionalität. [Azure-Diagnose](cloud-services-dotnet-diagnostics.md) können Sie Informationen zum Ausführen von Instanzen.

[InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) -Eigenschaft können Sie ermitteln, die Port-Nummer für einen internen Endpunkt eine Rolleninstanz, ein Dictionary-Objekt zurückzugeben, Endpunktnamen und ihre entsprechenden IP-Adressen und Ports enthält. [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) -Eigenschaft gibt die IP-Adresse und Port für einen angegebenen Endpunkt. Die **PublicIPEndpoint** -Eigenschaft gibt des Anschluss für einen Lastenausgleich Endpunkt. IP-Adressteil der **PublicIPEndpoint** -Eigenschaft wird nicht verwendet.

Hier ist ein Beispiel, in dem Instanzen durchläuft.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Hier ist ein Beispiel für eine Worker-Rolle, die den Endpunkt verfügbar gemacht über die Service-Definition und Überwachung von Verbindungen.

> [AZURE.WARNING] Dieser Code funktioniert nur für einen bereitgestellten Dienst. Bei der Ausführung der Azure-Serveremulator werden Service-Konfigurationselemente, die direkten Anschluss Endpunkte (**InstanceInputEndpoint** Elemente) erstellen ignoriert.

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;
        
        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;
                
        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);
                
        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Netzwerk-Verkehrsregeln rollenkommunikation steuern
Nachdem Sie interne Endpunkte definieren, können Sie Netzwerkregeln Datenverkehr (basierend auf den Endpunkten, die Sie erstellt haben) Steuerelement hinzufügen wie Instanzen kommunizieren können. Die folgende Abbildung zeigt einige häufige Szenarien rollenkommunikation steuern:

![Netzwerk-Datenverkehr Regeln Szenarien] (./media/cloud-services-enable-communication-role-instances/scenarios.png "Netzwerk-Datenverkehr Regeln Szenarien")

Das folgende Codebeispiel zeigt Rollendefinitionen für die Rollen in der vorherigen Abbildung gezeigt. Jede Rollendefinition enthält mindestens einen internen Endpunkt definiert:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [AZURE.NOTE] Einschränkung der Kommunikation zwischen Rollen kann interne Endpunkte der sowohl feste Ports automatisch auftreten.

Standardmäßig nach ein interner Endpunkt definiert ist, können von jeder der internen Endpunkt einer Rolle uneingeschränkt Kommunikationsflusses. Um Kommunikation einzuschränken, müssen Sie ein **NetworkTrafficRules** Element **ServiceDefinition** Element in der Service-Definitionsdatei hinzufügen.

### <a name="scenario-1"></a>Szenario 1
Nur Netzwerkverkehr von **WebRole1** an **WorkerRole1**zulassen.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Szenario 2
Nur können Netzwerkverkehr aus **WebRole1** **WorkerRole1** und **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Szenario 3
Ermöglicht nur Netzwerkverkehr aus **WebRole1** , **WorkerRole1**, **WorkerRole1** , **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Szenario 4
Nur können Netzwerkverkehr von **WebRole1** **WorkerRole1**, **WebRole1** , **WorkerRole2**, **WorkerRole1** , **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Ein XML-Schemareferenz für die Elemente über finden Sie [hier](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Cloud Service- [Modell](cloud-services-model-and-package.md).