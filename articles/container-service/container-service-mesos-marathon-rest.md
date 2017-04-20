<properties
   pageTitle="Azure Container Container Servicemanagement über die REST API | Microsoft Azure"
   description="Bereitstellen Sie Container mit einem Azure Container Service Mesos Cluster mithilfe Marathon REST API."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Andockfenster Container Micro-Services Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-rest-api"></a>Container-Management durch die REST-API

DC-Betriebssystem stellt eine Umgebung für die Bereitstellung und Skalierung gruppierte Arbeitslasten und die zugrunde liegende Hardware. Auf DC/OS ist ein Framework, die Planung und Ausführung Compute Arbeitslasten verwaltet.

Zwar Frameworks für viele gängige Arbeitslasten, beschreibt dieses Dokument das Erstellen und Container Bereitstellungen mit Marathon skalieren. Vor dem Durcharbeiten dieser Beispiele benötigen Sie einen DC-OS-Cluster, der in Azure Container Service konfiguriert. Sie müssen auch Remoteverbindungen mit diesem Cluster. Weitere Informationen zu diesen Elementen finden Sie in folgenden Artikeln:

- [Bereitstellen eines Clusters Azure Container Service](container-service-deployment.md)
- [Herstellen einer Verbindung zu einem Cluster Azure Container Service](container-service-connect.md)

Nach der Verbindung mit dem Cluster Azure Container Service können Sie über Port Http://localhost:local DC/OS und verwandte REST-APIs zugreifen. Die Beispiele in diesem Dokument davon ausgehen, dass Sie Port 80 Tunneln. Z. B. Marathon Endpunkt erreichen Sie unter `http://localhost/marathon/v2/`. Weitere Informationen zu den verschiedenen APIs finden Sie unter der Dokumentation Mesosphäre [Marathon-API](https://mesosphere.github.io/marathon/docs/rest-api.html) und die [Chronos-API](https://mesos.github.io/chronos/docs/api.html)und die Apache-Dokumentation für [Mesos Planer-API](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Informationen von DC-OS und Marathon

Vor dem Bereitstellen von Containern DC-OS-Cluster Informationen Sie einige zum Cluster DC-Betriebssystem wie die Namen und den aktuellen Status des DC-OS. Hierzu Abfragen der `master/slaves` Endpunkt des DC-OS REST-API. Wenn alles gut geht, sehen Sie eine Liste der DC-OS-Agenten und verschiedene Eigenschaften für jeden.

```bash
curl http://localhost/mesos/master/slaves
```

Nun verwenden Marathon `/apps` Endpunkt für die aktuelle anwendungsbereitstellung DC-OS-Cluster überprüft. Dies ist ein neuer Cluster wird ein leeres Array für apps angezeigt.

```
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Einen Andockfenster formatiert Container bereitstellen

Andockfenster formatiert Container durch Marathon bereitstellen mithilfe einer JSON-Datei, die die vorgesehene Bereitstellung. Im folgende Beispiel wird Nginx Container Bindung Port 80 des DC/OS Agent an Port 80 der Container bereitstellen. Beachten Sie, dass die Eigenschaft 'AcceptedResourceRoles' auf 'Slave_public' festgelegt ist. Dies wird ein Agent in der öffentlich zugänglichen Agent Skalierungsgruppe Container bereitgestellt.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
    "acceptedResourceRoles": [
    "slave_public"
  ],
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Andockfenster formatiert Container bereitstellen, erstellen Sie JSON-Datei, oder verwenden Sie das Beispiel [Azure Container Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)Demo. In einem zugänglichen Speicherort speichern. Weiter, um den Container bereitzustellen, führen Sie den folgenden Befehl ein. Geben Sie den Namen der JSON-Datei.

```
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

Die Ausgabe ist ähnlich der folgenden:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Bei einer Abfrage Marathon für Clientanwendungen wird diese neue Anwendung in der Ausgabe angezeigt.

```
curl localhost/marathon/v2/apps
```

## <a name="scale-your-containers"></a>Skaliert die Containern

Sie können auch Marathon API skalieren oder Skalieren bei der anwendungsbereitstellung. Im vorherigen Beispiel wird eine Instanz einer Anwendung bereitgestellt. Wir dieses aus drei Instanzen einer Anwendung skalieren Dazu eine JSON-Datei mithilfe von JSON-Text erstellen und in einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl Skalieren der Anwendung.

>[AZURE.NOTE] Der URI werden http://localhost/marathon/v2/apps/ und die ID der Anwendung skaliert. Verwenden Sie das Nginx Beispiel ist hier, würde der URI http://localhost/marathon/v2/apps/nginx sein.

```json
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Schließlich Fragen Sie Marathon Endpunkt für Applikationen ab. Sie sehen, dass jetzt drei Nginx Container.

```
curl localhost/marathon/v2/apps
```

## <a name="use-powershell-for-this-exercise-marathon-rest-api-interaction-with-powershell"></a>Verwenden Sie für diese Übung PowerShell: Interaktion mit PowerShell Marathon REST-API

Mithilfe von PowerShell-Befehlen unter Windows können Sie die gleichen Aktionen ausführen.

Führen Sie den folgenden Befehl, um Informationen zum Cluster DC/OS Agent-Namen wie Agent-Status.

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Andockfenster formatiert Container durch Marathon bereitstellen mithilfe einer JSON-Datei, die die vorgesehene Bereitstellung. Im folgende Beispiel wird Nginx Container Bindung Port 80 des DC/OS Agent an Port 80 der Container bereitstellen.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 16.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

Erstellen Sie JSON-Datei, oder verwenden Sie das Beispiel [Azure Container Service](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/marathon/marathon.json)Demo. In einem zugänglichen Speicherort speichern. Weiter, um den Container bereitzustellen, führen Sie den folgenden Befehl ein. Geben Sie den Namen der JSON-Datei.

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Sie können auch Marathon API skalieren oder Skalieren bei der anwendungsbereitstellung. Im vorherigen Beispiel wird eine Instanz einer Anwendung bereitgestellt. Wir dieses aus drei Instanzen einer Anwendung skalieren Dazu eine JSON-Datei mithilfe von JSON-Text erstellen und in einem zugänglichen Speicherort speichern.

```json
{ "instances": 3 }
```

Führen Sie den folgenden Befehl Skalieren der Anwendung.

> [AZURE.NOTE] Der URI werden http://localhost/marathon/v2/apps/ und die ID der Anwendung skaliert. Das Nginx Beispiel hier Verwendung wäre der URI http://localhost/marathon/v2/apps/nginx.

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Nächste Schritte

- [Mehr über Mesos HTTP-Endpunkte]( http://mesos.apache.org/documentation/latest/endpoints/).
- [Mehr über Marathon REST-API]( https://mesosphere.github.io/marathon/docs/rest-api.html).
