<properties
   pageTitle="Saldo-Container in einem Cluster Azure Container Service laden | Microsoft Azure"
   description="Lastenausgleich über mehrere Container in einem Cluster Azure Container Service."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="DC/OS, Azure Container Micro-services"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/11/2016"
   ms.author="rogardle"/>

# <a name="load-balance-containers-in-an-azure-container-service-cluster"></a>Load Balance Container in einem Cluster Azure Container Service

In diesem Artikel erforschen wir zum Erstellen einer internen Lastenausgleich in einem DC/BS gemanagte Azure Container Service mit Marathon Pfd. Dies ermöglicht Ihrer Anwendung horizontal skalieren. Es auch ermöglicht zu der öffentlichen und privaten Agent Cluster mit der Lastenausgleich der öffentlichen und der Anwendungscontainer private Cluster.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Bereitstellen einer Instanz von Azure Container Service](container-service-deployment.md) mit Orchestrator DC/OS und [den Client zum Cluster verbinden kann](container-service-connect.md). 

## <a name="load-balancing"></a>Lastenausgleich

Es gibt zwei Lastenausgleich Ebenen im Container Service-Cluster werden wir: 

  1. Azure Lastenausgleich bietet öffentlichen Einstiegspunkte (diejenigen, die Endbenutzer erreicht wird). Dies erfolgt automatisch durch Azure Container Service und ist standardmäßig so konfiguriert, dass Port 80, 443 und 8080 verfügbar machen.
  2. Lastenausgleich Marathon (Marathon lb) leitet eingehende Anfragen Container-Instanzen, die diese Anfragen. Wie wir Webdienst bereitstellen Container skalieren, passt sich dynamisch Marathon Pfd. Dieses System zum Lastenausgleich wird nicht standardmäßig in Ihrem Container-Dienst bereitgestellt, aber es ist sehr einfach zu installieren.

## <a name="marathon-load-balancer"></a>Lastenausgleich Marathon

Lastenausgleich Marathon neu dynamisch basierend auf den Container, die Sie bereitgestellt haben. Ist auch gegen den Verlust von einem Container oder Agent - in diesem Fall Apache Mesos startet den Container an anderer Stelle einfach und Marathon lb passen.

Installieren web Marathon Lastenausgleich Verwendung der DC/OS-Benutzeroberfläche oder die Befehlszeile.

### <a name="install-marathon-lb-using-dcos-web-ui"></a>Installieren Marathon LB mit DC-OS-Webbenutzeroberfläche

  1. Klicken Sie auf 'Universum'
  2. Suchen Sie nach "Marathon-LB"
  3. Klicken Sie auf "Installieren"

![Installieren von Marathon-lb über die Weboberfläche DC-OS](./media/dcos/marathon-lb-install.png)

### <a name="install-marathon-lb-using-the-dcos-cli"></a>Installieren Sie Marathon-LB mit DC-OS CLI

Nach der Installation der DC-OS-CLI und sicherstellen können Sie dem Cluster führen den folgenden Befehl vom Client-Computer verbinden:

```bash
dcos package install marathon-lb
```

Dieser Befehl installiert automatisch Lastenausgleich des Clusters öffentlichen Bediensteten.

## <a name="deploy-a-load-balanced-web-application"></a>Bereitstellen einer Load Balanced Anwendung

Jetzt haben wir Marathon-lb-Paket können wir einen Anwendungscontainer bereitstellen, die wir Lastenausgleich möchten. Für dieses Beispiel werden wir einfache Webserver bereitstellen, mit der folgenden Konfiguration:

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}

```

  * Legen Sie den Wert der `HAProxy_0_VHOST` auf den vollqualifizierten Domänennamen des Lastenausgleich für Agenten. Dies ist `<acsName>agents.<region>.cloudapp.azure.com`. Beispielsweise erstellen ein Clusters Container Service mit Namen `myacs` im Bereich `West US`, der vollqualifizierte Domänenname wäre `myacsagents.westus.cloudapp.azure.com`. Sie finden auch diese Lastenausgleich mit den Namen "Agent" suchen, wenn Sie die Ressourcen in der Ressourcengruppe suchen, die für Container [Azure-Portal](https://portal.azure.com)erstellt.
  * Festlegen der ServicePort an einen Port > = 10.000. Identifiziert den Dienst in diesem Container - läuft Marathon lb verwendet diese Dienste identifizieren, die auf verteilen soll.
  * Legen Sie die `HAPROXY_GROUP` "extern" bezeichnen.
  * Legen Sie `hostPort` 0. Dies bedeutet, dass Marathon willkürlich einen verfügbaren Port zuweisen.
  * Legen Sie `instances` , die Anzahl der Instanzen, die Sie erstellen möchten. Sie können immer diese oben und unten später skalieren.

Es lohnt sich Noing standardmäßig Marathon private Cluster bereitstellen, dies bedeutet, dass über Bereitstellung nur werden über Ihre Lastenausgleich wird normalerweise Wir wünschen.

### <a name="deploy-using-the-dcos-web-ui"></a>Mithilfe von DC-OS-Webbenutzeroberfläche

  1. Seite Marathon am http://localhost/marathon (nach dem Einrichten des [SSH-Tunnel](container-service-connect.md) und auf`Create Appliction`
  2. In der `New Application` Dialogfeld auf `JSON Mode` in der oberen rechten Ecke
  3. Der Editor über JSON einfügen
  4. Klicken Sie auf`Create Appliction`

### <a name="deploy-using-the-dcos-cli"></a>Mithilfe von DC-OS-CLI

Bereitstellen dieser Anwendung mit der CLI DC-OS kopieren Sie einfach über JSON in einer Datei namens `hello-web.json`, und:

```bash
dcos marathon app add hello-web.json
```

## <a name="azure-load-balancer"></a>Azure Lastenausgleich

Standardmäßig stellt Azure Lastenausgleich Ports 80, 8080 und 443. Verwenden Sie eine dieser drei ports (wie in dem obigen Beispiel) und nichts tun müssen. Sie sollen die Agent-Lastenausgleich FQDN - Treffer und jedes Mal aktualisieren Sie schlagen eine der drei Webserver eine Roundrobin. Wenn Sie einen anderen Port verwenden, müssen Sie jedoch Roundrobin-Regel und einen Prüfpunkt Lastenausgleich für den Port hinzuzufügen, die Sie verwendet. Hierzu von der [Azure-CLI](../xplat-cli-azure-resource-manager.md)mit den Befehlen `azure network lb rule create` und `azure network lb probe create`. Sie können auch dazu Azure-Portal.


## <a name="additional-scenarios"></a>Weitere Szenarien

Sie hätten ein Szenario, in verschiedenen Domänen mit anderen Dienste. Zum Beispiel:

mydomain1.com -> Azure-LB:80 -> Marathon-Lb:10001 -> Mycontainer1:33292  
mydomain2.com -> Azure-LB:80 -> Marathon-Lb:10002 -> Mycontainer2:22321

Zu diesem Zweck checken Sie [virtuelle Hosts](https://mesosphere.com/blog/2015/12/04/dcos-marathon-lb/)die Möglichkeit, Domänen in bestimmten Marathon-lb Pfade verbinden aus.

Sie können unterschiedliche Ports verfügbar und an den richtigen Dienst hinter Marathon lb zuordnen. Zum Beispiel:

Azure Lb:80 -> Marathon-Lb:10001 -> Mycontainer:233423  
Azure Lb:8080 -> Marathon-Lb:1002 -> Mycontainer2:33432


## <a name="next-steps"></a>Nächste Schritte

Finden Sie DC-OS für weitere auf [Marathon Pfd](https://dcos.io/docs/1.7/usage/service-discovery/marathon-lb/).
