<properties
   pageTitle="Reverse Proxy Fabric Service | Microsoft Azure"
   description="Verwenden Sie Service Fabric reverse Proxy für die Kommunikation mit Microservices von innerhalb und außerhalb des Clusters"
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/04/2016"
   ms.author="vturecek"/>

# <a name="service-fabric-reverse-proxy"></a>Service Fabric Reverse Proxy

Service Fabric Reverse Proxy ist ein reverse-Proxyserver erstellt in Service Stoff, die Adressierung Microservices Service Fabric-Clusters, die HTTP-Endpunkte verfügbar machen kann.

## <a name="microservices-communication-model"></a>Microservices Kommunikationsmodell

Microservices Service Fabric normalerweise auf eine Teilmenge der VM im Cluster ausgeführt und können wechseln einer VM aus verschiedenen Gründen. Die Endpunkte für Microservices dynamisch ändern. Typische Muster zum Kommunizieren mit der Microservice ist die Auflösung Schleife unten,

1. Lösen Sie Dienstspeicherort zunächst über den Namensdienst.
2. Verbindung zum Dienst.
3. Die Ursache der Verbindungsfehler und erneut lösen Dienstspeicherort bei Bedarf.

Dieser Prozess beinhaltet im Allgemeinen umschließen clientseitige Kommunikation Bibliotheken in einer Schleife, die die Auflösung und wiederholen Sie den Richtlinien implementiert.
Weitere Informationen zu diesem Thema finden Sie in der [Kommunikation mit Diensten](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-via-sf-reverse-proxy"></a>Kommunikation über SF reverse proxy
Service Fabric Reverseproxy auf allen Knoten im Cluster ausgeführt wird. Er führt die gesamte Auflösungsprozess im Auftrag des Clients und leitet die Clientanforderung. Damit können Clients auf den Cluster nur clientseitige HTTP-Kommunikation Bibliotheken mit den Zieldienst über SF reverse Proxy lokal auf demselben Knoten ausgeführt.

![Interne Kommunikation][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Erreichen von Microservices außerhalb des Clusters
Das Standardmodell für die externe Kommunikation für Microservices ist **Anmelden** , nicht jeden Dienst standardmäßig direkt von externen Clients zugegriffen werden. [Azure-Lastenausgleich](../load-balancer/load-balancer-overview.md) ist Netzwerkgrenze zwischen externen Clients und Microservices, die Netzwerkadressübersetzung und leitet externe Anfragen an interne **nun** Endpunkte. Um einen Microservice Endpunkt direkt für externe Clients zugänglich machen, muss zunächst Lastenausgleich Azure Verkehr an jeden Port vom Dienst im Cluster verwendet konfiguriert werden. Darüber hinaus die meisten Microservices (insbesondere statusbehaftete Microservices) nicht auf allen Knoten des Clusters live und sie können Knoten bei einem Failover wechseln so Azure Lastenausgleich in solchen Fällen effektiv Zielknoten Replikate ermitteln kann zum Weiterleiten von Datenverkehr an.

### <a name="reaching-microservices-via-the-sf-reverse-proxy-from-outside-the-cluster"></a>Microservices über den SF reverse-Proxyserver von außerhalb des Clusters erreichen

Statt einzelnen Dienst Ports Azure Lastenausgleich konfigurieren, kann nur der SF Reverse-Proxy-Port in Azure Lastenausgleich konfiguriert werden. Dadurch können Clients außerhalb des Clusters zu Dienste innerhalb des Clusters über den reverse-Proxyserver ohne zusätzliche Konfigurationen.

![Externe Kommunikation][0]

>[AZURE.WARNING] Reverse-Proxy-Port für den Lastenausgleich konfigurieren, macht alle micro Dienste im Cluster, die einen HTTP-Endpunkt außerhalb des Clusters addressible zu setzen.


## <a name="uri-format-for-addressing-services-via-the-reverse-proxy"></a>URI-Format für die Adressierung von Diensten über den reverse-Proxyserver

Der Reverseproxy verwendet ein bestimmtes URI-Format der Servicepartition identifizieren die eingehende Anforderung weitergeleitet werden soll:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **http(s):** Reverse Proxy kann konfiguriert werden, um HTTP oder HTTPS-Datenverkehr zu akzeptieren. Bei HTTPS-Datenverkehr tritt SSL-Beendigung auf den reverse-Proxyserver. Reverse Proxy Dienste im Cluster weitergeleitet werden Anfragen werden über http.
 - **Cluster FQDN | interne IP-Adresse:** Für externe Clients kann der reverse-Proxyserver konfiguriert werden, dass durch die Cluster-Domäne (z. B. mycluster.eastus.cloudapp.azure.com) erreichen. Standardmäßig der reverse-Proxyserver auf jedem Knoten ausgeführt wird, also für internen Verkehr es auf Localhost oder IP-internen Knoten (z. B. 10.0.0.1) erreichbar.
 - **Anschluss:** Der Anschluss für den reverse-Proxyserver angegeben wurde. Z. B.: 19008.
 - **ServiceInstanceName:** Dies wird den bereitgestellten Dienst vollständig qualifizierten Namen der sans erreichen Diensts die "Fabric: /" Schema. Z. B. Dienst *Fabric: / Myapp/Myservice*, verwenden Sie *Myapp-Myservice*.
 - **Suffix Pfad:** Dies ist die tatsächliche URL-Pfad für den Dienst, dem Sie möchten. Beispiel: *Myapi/Werte/hinzufügen/3*
 - **PartitionKey:** Für einen partitionierten Dienst ist dies berechnete Partitionsschlüssel der Partition, die Sie erreichen möchten. Beachten Sie, dass dies *nicht* der Partitions-ID-GUID. Dieser Parameter ist nicht erforderlich für das Partitionsschema Singleton-Dienste.
 - **PartitionKind:** Das Partitionsschema Service. Dies ist 'Int64Range' oder "Mit dem Namen". Dieser Parameter ist nicht erforderlich für das Partitionsschema Singleton-Dienste.
 - **Timeout:**  Gibt das Timeout für die HTTP-Anforderung durch den reverse-Proxyserver, den Service für die Clientanforderung erstellt. Der Standardwert ist 60 Sekunden. Dies ist ein optionaler Parameter.

### <a name="example-usage"></a>Beispiel für die Verwendung

Als Beispiel nehmen wir Service **Stoff: MyApp/MyService** , öffnet einen HTTP-Listener unter der folgenden URL:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

In den folgenden Ressourcen:

 - `/index.html`
 - `/api/users/<userId>`

Wenn der Dienst das Partitionierungsschema Singleton verwendet, Abfragezeichenfolgenparameter *PartitionKey* und *PartitionKind* sind nicht erforderlich und der Dienst erreichbar über das Gateway als:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Intern:`http://localhost:19008/MyApp/MyService`

Wenn der Dienst das Partitionierungsschema Uniform Int64 verwendet, müssen Abfragezeichenfolgenparameter *PartitionKey* und *PartitionKind* zu einer Partition des Dienstes verwendet werden:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Um die vom Dienst bereitgestellten Ressourcen zu erreichen, legen Sie Pfad der Ressource nach dem Dienstnamen im URL:

 - Extern:`http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Intern:`http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Das Gateway leiten dann diese Anfragen an die Dienst-URL:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Besondere Behandlung für Port sharing Services

Application Gateway versucht, erneut eine aufgelöst und wiederholen Sie die Anforderung aus, wenn ein Dienst erreicht werden kann. Ist einer der wichtigsten Vorteile von Gateway, wie Clientcode keine eigenen Service-Lösung implementieren und Schleife beheben müssen

In der Regel beim Dienst nicht erreichbar bedeutet Dienstinstanz oder Replikat als Teil des normalen Lebenszyklus auf einen anderen Knoten verschoben wurde. In diesem Fall erhalten das Gateway ein Netzwerkverbindungsfehler ein Endpunkt nicht mehr auf die ursprünglich aufgelöste Adresse geöffnet ist.

Jedoch Replikate Dienstinstanzen können einen Hostprozess freigeben und möglicherweise auch einen Anschluss wird von einem HTTP-basierten Webserver gehostet freigeben einschließlich:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In diesem Fall ist es wahrscheinlich, dass der Webserver Host und Anfragen jedoch aufgelöst Dienstinstanz oder Replikat nicht mehr auf dem Host verfügbar ist. In diesem Fall wird das Gateway eine HTTP 404-Antwort vom Webserver erhalten. HTTP 404 hat daher zwei unterschiedliche Bedeutung:

 1. Die Adresse ist richtig, aber vom Benutzer angeforderte Ressource ist nicht vorhanden.
 2. Die Adresse ist falsch, und die vom Benutzer angeforderte Ressource möglicherweise tatsächlich auf einen anderen Knoten vorhanden.

Im ersten Fall ist dies eine normale HTTP 404, gilt ein Benutzerfehler. Im zweiten Fall der Benutzer möchte eine Ressource vorhanden ist, jedoch das Gateway konnte nicht gefunden werden, da der Dienst selbst verschoben wurde, muss das Gateway erneut die Adresse und versuchen Sie es erneut.

Das Gateway muß daher eine Möglichkeit, zwischen diesen beiden Fällen unterschieden. Hinweis auf dem Server ist erforderlich, unterscheiden.

 - Standardmäßig Application Gateway Fall #2 und versucht, erneut aufgelöst und die Anforderung erneut ausstellen.
 - Fall #1 Application Gateway an, sollte der Dienst HTTP-Antwortheaders zurück:

`X-ServiceFabric : ResourceNotFound`

Diese HTTP-Antwortheader bezeichnet normalen HTTP 404 in der angeforderte Ressource ist nicht vorhanden und das Gateway versucht nicht, die Adresse neu aufzulösen.

## <a name="setup-and-configuration"></a>Installation und Konfiguration
Service Fabric Reverse Proxy kann für den Cluster über [Azure Ressourcenmanager Vorlage](./service-fabric-cluster-creation-via-arm.md)aktiviert werden.

Haben Sie die Vorlage für den Cluster, die Sie bereitstellen möchten (die Beispielvorlagen oder durch Erstellen einer benutzerdefinierten Ressource-Manager-Vorlage) Reverse Proxy ermöglicht werden in der Vorlage die folgenden Schritte.

1. Definieren Sie einen Port für den reverse-Proxyserver im [Abschnitt Parameter](../resource-group-authoring-templates.md) der Vorlage.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geben Sie den Port für Nodetype Objekte im **Cluster** [Resource Abschnitt](../resource-group-authoring-templates.md)

    Für Anforderungsparameter vor ' 2016-09-01' der Anschluss durch den Parameter angegebenen Namen ***HttpApplicationGatewayEndpointPort***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

    Für Anforderungsparameter des am oder nach dem ' 2016-09-01' der Anschluss durch den Parameter angegebenen Namen ***ReverseProxyEndpointPort***

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```

3. Um den reverse-Proxyserver außerhalb des Clusters Azure Adresse setup **Azure Load Balancer Regeln** für den in Schritt 1 angegebenen Port.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Fügen Sie Zertifikat der Eigenschaft HttpApplicationGatewayCertificate im **Cluster** [Resource Abschnitt](../resource-group-authoring-templates.md) konfigurieren SSL-Zertifikate auf den Anschluss für den Reverse-Proxyserver hinzu

    Für Anforderungsparameter vor ' 2016-09-01' das Zertifikat der Parameter anhand name ***HttpApplicationGatewayCertificate***

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```
    Für Anforderungsparameter des am oder nach dem ' 2016-09-01' das Zertifikat der Parameter anhand name ***ReverseProxyCertificate***
    
    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## <a name="next-steps"></a>Nächste Schritte
 - Finden Sie ein Beispiel für HTTP-Kommunikation zwischen Diensten in einem [Beispielprojekt auf GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Remoteprozeduraufrufe mit Remoting zuverlässige Dienste](service-fabric-reliable-services-communication-remoting.md)

 - [Web-API, die zuverlässige Dienste OWIN verwendet](service-fabric-reliable-services-communication-webapi.md)

 - [Zuverlässige Dienste mit WCF-Kommunikation](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png
