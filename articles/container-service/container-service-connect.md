<properties
   pageTitle="Verbinden mit einem Cluster Azure Container Service | Microsoft Azure"
   description="Verbindung zu einem Cluster Azure Container Service mit einem SSH-tunnel"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Andockfenster Container Micro-Services DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>


# <a name="connect-to-an-azure-container-service-cluster"></a>Verbinden Sie mit einem Cluster Azure Container Service

DC-OS und Andockfenster Schwarm Clustern von Azure Container Service bereitgestellt setzen REST-Endpunkte. Diese Endpunkte sind jedoch nicht nach außen öffnen. Um diese Endpunkte zu verwalten, müssen Sie einen Secure Shell (SSH)-Tunnel erstellen. Nach einer SSH wurde Tunnel aufgebaut, Cluster-Endpunkte Befehle ausführen und Cluster Benutzeroberfläche über einen Browser auf Ihrem System anzeigen. Dieses Dokument führt Sie durch Erstellen eines SSH-Tunnels von Linux, OS X und Windows.

>[AZURE.NOTE] Sie können eine SSH-Sitzung mit Cluster erstellen. Jedoch empfohlen nicht das. Direkt an ein Managementsystem macht das Risiko unbeabsichtigten Konfiguration.   

## <a name="create-an-ssh-tunnel-on-linux-or-os-x"></a>Erstellen Sie SSH-Tunnel auf Linux oder OS X

Was beim Erstellen eines Tunnels SSH unter Linux oder OS X ist den öffentlichen DNS-Master Lastenausgleich zu suchen. Dazu erweitern Sie Ressourcengruppe, sodass jeder Ressource angezeigt wird. Suchen Sie und markieren Sie die öffentliche IP-Adresse des Master-Shapes. Dies öffnet ein Blatt mit der öffentlichen IP-Adresse die DNS-Namen enthält. Speichern Sie dieser Name für die spätere Verwendung. <br />


![Öffentlichen DNS-Namen](media/pubdns.png)

Öffnen Sie nun eine Shell und führen Sie den folgenden Befehl:

**PORT** ist der Port des Endpunkts ist, den Sie verfügbar machen möchten. Schwarm ist dies 2375. Verwenden Sie für DC/OS Port 80.  
**Benutzername** ist der Benutzername, der bei der Bereitstellung des Clusters bereitgestellt wurde.  
**DNSPREFIX** ist das DNS-Präfix, das Sie bei der Bereitstellung des Clusters bereitgestellt.  
**Bereich** ist der Bereich, in dem sich die Ressourcengruppe befindet.  
**PATH_TO_PRIVATE_KEY** [OPTIONAL] ist der Pfad für den privaten Schlüssel, der die den öffentlichen Schlüssel entspricht, der beim Erstellen des Clusters Container Service bereitgestellt. Diese Option-i kennzeichnen.

```bash
ssh -L PORT:localhost:PORT -f -N [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com -p 2200
```
> SSH wird-Verbindung 2200 - nicht den standard-Port 22.

## <a name="dcos-tunnel"></a>DC-OS-tunnel

Öffnen Sie einen Tunnel zu DC, Betriebssystem-bezogenen Endpunkte führen Sie einen Befehl ähnlich dem folgenden ist:

```bash
sudo ssh -L 80:localhost:80 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Sie können jetzt die Endpunkte DC, Betriebssystem-bezogenen zugreifen:

- DC/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Ebenso können Sie die Rest-APIs für jede Anwendung über diesen Tunnel erreichen.

## <a name="swarm-tunnel"></a>Schwarm tunnel

Öffnen Sie einen Tunnel zum Endpunkt Schwarm Programmierer, die der folgenden ähnelt:

```bash
ssh -L 2375:localhost:2375 -f -N azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com -p 2200
```

Jetzt können Sie die DOCKER_HOST-Umgebungsvariable wie folgt festlegen. Sie können weiterhin die Andockfenster Befehlszeilenschnittstelle (CLI) wie gewohnt verwenden.

```bash
export DOCKER_HOST=:2375
```

## <a name="create-an-ssh-tunnel-on-windows"></a>Erstellen eines Tunnels SSH unter Windows

Es gibt mehrere Optionen zum Erstellen von SSH Tunneln unter Windows. Dieses Dokument wird kitten verwenden beschrieben.

Kitten Windows-System herunterladen und Ausführen der Anwendungdes.

Geben Sie den Hostnamen, der Cluster Administrator-Benutzernamen und den öffentlichen DNS-Namen des ersten Masters im Cluster besteht. **Hostnamen** werden wie folgt aussehen: `adminuser@PublicDNS`. Geben Sie 2200 für den **Port**.

![PuTTY Konfiguration 1](media/putty1.png)

Wählen Sie **SSH** und **Authentifizierung**. Fügen Sie private Schlüsseldatei für die Authentifizierung.

![PuTTY Konfiguration 2](media/putty2.png)

Wählen Sie **Tunnel** und konfigurieren Sie die folgenden Ports weitergeleitet:
- **Quellport:** Ihre Einstellung – mit 80 für DC-OS oder 2375 Schwarm.
- **Ziel:** Verwenden Sie Localhost:80 für DC-Betriebssystem oder Localhost:2375 Schwarm.

Im folgende Beispiel für DC-OS konfiguriert, sondern sucht ähnliche Andockfenster Schwarm.

>[AZURE.NOTE] Port 80 muss nicht verwendet werden, wenn diesen Tunnel erstellen.

![PuTTY Konfiguration 3](media/putty3.png)

Wenn Sie fertig sind, speichern Sie die Verbindungskonfiguration und Herstellen Sie PuTTY Sitzung. Wenn Sie verbinden, sehen Sie die Portkonfiguration in der PuTTY.

![PuTTY Ereignisprotokoll](media/putty4.png)

Wenn den Tunnel für DC-OS konfiguriert haben, können Sie den zugehörigen Endpunkt an zugreifen:

- DC/OS:`http://localhost/`
- Marathon:`http://localhost/marathon`
- Mesos:`http://localhost/mesos`

Den Tunnel für Andockfenster Schwarm konfiguriert haben, können Sie über CLI Andockfenster Schwarm Cluster zugreifen. Sie müssen zuerst Windows-Umgebungsvariable konfigurieren `DOCKER_HOST` mit ` :2375`.

## <a name="next-steps"></a>Nächste Schritte

Bereitstellen und Verwalten mit DC-Betriebssystem oder Schwarm:

- [Arbeiten Sie mit Azure Containerservice und DC-OS](container-service-mesos-marathon-rest.md)
- [Azure Container und Andockfenster Schwarm arbeiten](container-service-docker-swarm.md)
