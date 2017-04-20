<properties
   pageTitle="Azure Container Container Servicemanagement mit Andockfenster Schwarm | Microsoft Azure"
   description="Bereitstellen Sie Container für Andockfenster Schwarm in Azure Container Service"
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

# <a name="container-management-with-docker-swarm"></a>Container-Management mit Andockfenster Schwarm

Andockfenster Schwarm stellt eine Umgebung für die Bereitstellung von Containern Arbeitslasten eine zusammengefasste Gruppe von Andockfenster Hosts. Andockfenster Schwarm verwendet die systemeigene API Andockfenster. Der Workflow für die Verwaltung auf Andockfenster Schwarm ist fast identisch, was es auf einem einzelnen containerhost. Dieses Dokument enthält Beispiele Container Arbeitslasten in einer Azure Container Service Andockfenster Schwarm Instanz bereitstellen. Ausführliche Dokumentation auf Andockfenster Schwarm finden Sie in der [Andockfenster Schwarm auf Docker.com](https://docs.docker.com/swarm/).

Komponenten zu diesem Dokument:

[Erstellen eines Clusters Schwarm in Azure Container Service](container-service-deployment.md)

[Verbindung mit dem Cluster Schwarm in Azure Container Service](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Bereitstellen eines neuen Containers

Verwenden Sie zum Erstellen eines neuen Containers im Andockfenster Schwarm der `docker run` Befehl (sodass SSH-Tunnel auf den Mastern gemäß der oben genannten Komponenten geöffnet haben). Dieses Beispiel erstellt einen Container aus der `yeasy/simple-web` Bild:


```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Verwenden Sie nach dem Erstellen der Container `docker ps` Informationen über den Container zurückgegeben. Beachten Sie hier aufgeführten Schwarm-Agent, der den Container befindet.


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

Sie können jetzt die Anwendung zugreifen, die in diesem Container über den öffentlichen DNS-Namen Schwarm Agent Lastenausgleich ausgeführt wird. Sie finden diese Informationen im Azure-Portal:  


![Besuchen Sie echte Ergebnisse](media/real-visit.jpg)  

Standardmäßig hat das Lastenausgleichsmodul Ports 80, 8080, 443 öffnen. Möchten Sie einen anderen Anschluss herstellen müssen Sie diesen Port auf den Azure-Lastenausgleich für den Agent zu öffnen.

## <a name="deploy-multiple-containers"></a>Bereitstellen von mehreren Containern

Mehrere Container 'Andockfenster ausführen mehrere Male ausführen gestartet werden, können Sie die `docker ps` Befehl finden Sie unter dem Container gehostet auf ausführen. Im folgenden Beispiel werden drei gleichmäßig über die drei Schwarm Agents:  


```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Bereitstellen von Containern mit Andockfenster erstellen

Andockfenster erstellen können Sie die Bereitstellung und Konfiguration von mehreren Containern automatisieren. Dazu sicherzustellen Sie, dass Secure Shell (SSH) Tunnel erstellt wurde und die Variable DOCKER_HOST (siehe oben aufgeführten erforderlichen Komponenten) festgelegt wurde.

Eine Andockfenster compose.yml-Datei auf Ihrem lokalen System zu erstellen. Hierzu verwenden Sie in diesem [Beispiel](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml).

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Ausführen `docker-compose up -d` Container Bereitstellung gestartet:


```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Schließlich wird die Liste der laufenden Container zurückgegeben. Diese Liste spiegelt die Container mit Andockfenster erstellen bereitgestellt wurden:


```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Natürlich können Sie `docker-compose ps` nur die gemäß Container zu Ihrem `compose.yml` Datei.

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Andockfenster Schwarm](https://docs.docker.com/swarm/)
