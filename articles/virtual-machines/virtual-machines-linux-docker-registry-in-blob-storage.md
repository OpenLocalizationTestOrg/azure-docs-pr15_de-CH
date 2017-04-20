<properties 
  pageTitle="Eigene Private Andockfenster Registrierung Azure bereitstellen | Microsoft Azure"
  description="Beschreibt, wie Sie Andockfenster Registrierung Container Bilder Azure BLOB-Speicher-Dienst hosten."
  services="virtual-machines-linux"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="timlt"
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines-linux"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="09/27/2016" 
  ms.author="ahmetb" />

# <a name="deploying-your-own-private-docker-registry-on-azure"></a>Eigene Private Andockfenster Registrierung Azure bereitstellen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



Dieses Dokument beschreibt, welche Andockfenster privat ist und zeigt, wie Sie eine private Andockfenster Registrierung auf Microsoft Azure Azure BLOB-Speicher ein Andockfenster Registrierung 2.0 Container Bild bereitstellen können.

Dieses Dokument wird vorausgesetzt:

1. Sie können Andockfenster und Andockfenster Bilder speichern. (Nicht? [Erfahren Sie mehr über Andockfenster](https://www.docker.com))
2. Sie haben einen Server mit Andockfenster Engine installiert. (Nicht? [Schnell auf Azure tun.](https://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## <a name="what-is-a-private-docker-registry"></a>Was ist eine Private Andockfenster Registrierung?

Versand Container Anwendung in die Cloud ein Andockfenster Container-Image Erstellen und es irgendwo speichern, sodass sie sich und andere verwendet werden kann. 

Container-Abbild erstellen und in die Cloud Versand einfach ist, ist es eine Herausforderung generierte Bild zuverlässig speichern. Aus diesem Grund Andockfenster bietet einen zentralisierten Dienst namens [Andockfenster Hub] [ docker-hub] speichern Container Bilder in der Cloud und können Sie jederzeit mit diesen Bildern Container erstellen.

Obwohl die [Andockfenster Hub] [ docker-hub] ein kostenpflichtigen Dienst zum Speichern von privaten Anwendungscontainer Bilder Andockfenster Hinsicht Entwickler benötigt und bietet eine Open-Source-Toolset zum Speichern von Bildern in Ihrer eigenen privaten Andockfenster Registrierung hinter einer Firewall oder lokal ohne Internetzugang.
Azure BLOB-Speicher können einfach zu gewährleisten, Sie schnell verwenden, da zum Erstellen und Verwenden einer privaten Andockfenster Registrierung in Azure selbst steuern.

## <a name="why-should-you-host-a-docker-registry-on-azure"></a>Warum sollten Sie eine Registrierung Andockfenster Azure hosten?

Ihre Registrierung Andockfenster Instanz auf Microsoft Azure und Speichern von Bildern auf Azure BLOB-Speicher haben Sie mehrere Vorteile:

**Sicherheit:** Andockfenster Bilder lassen Azure-Datencentern, damit sie das öffentliche Internet nicht überschreiten, als wären Sie Andockfenster Hub verwenden.
  
**Leistung:** Andockfenster Bilder werden im gleichen Datencenter oder Region als Anwendung gespeichert. Dies bedeutet, dass die Bilder schneller und zuverlässiger gegenüber Andockfenster Hub gezogen werden.

**Zuverlässigkeit:** Mit Microsoft Azure BLOB-Speicher, machen Sie viele Speicher Eigenschaften wie hohe Verfügbarkeit, Redundanz, Premium-Speicher (SSDs) und so weiter.

## <a name="configuring-docker-registry-to-use-azure-blob-storage"></a>Andockfenster Registrierung Azure BLOB-Speicher verwenden

(Es wird empfohlen, die [Andockfenster Registrierung 2.0-Dokumentation][Registrierung Dokumente] vor.)

Sie können [Konfigurieren] [ registry-config] der Registrierung Andockfenster unterschiedlich.
Sie können:

1. Verwenden einer `config.yml` Datei. Sie müssen in diesem Fall erstellen Sie ein separates Andockfenster Bild auf `registry` Bild.
2. Überschreiben die Standardkonfigurationsdatei über Umgebungsvariablen: Aufgaben erstellen und Verwalten eines separaten Andockfenster Bilds wird.

Der Einfachheit halber folgt diesem Thema Option 2 mit Umgebungsvariablen.

Ausführen Instanz einer Registrierung Andockfenster der:

* verwendet das Speicherkonto Azure für speichern
* die virtuellen Computer Anschluss 5000
* keine Authentifizierung konfiguriert (nicht empfohlen, siehe Hinweis unten)

müssen Sie die folgenden Andockfenster in Bash Terminal Befehl (ersetzen Sie `<storage-account>` und `<storage-key>` mit Ihren Anmeldeinformationen):

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

Nachdem der Befehl beendet sehen den Container, der die private Andockfenster Registrierung Instanz mit der `docker ps` auf dem Host:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Konfigurieren der Sicherheit für die Andockfenster Registrierung wird in diesem Dokument nicht behandelt und die Registrierung werden ohne Authentifizierung standardmäßig Port Port Registrierung für den virtuellen Endpunkt öffnen oder Lastenausgleich Befehls Bereitstellung verwenden.
>
> Bitte lesen Sie die [Andockfenster Registrierung konfigurieren] [ registry-config] Dokumentation Registrierung Instanz und Bilder sichern.

## <a name="next-steps"></a>Nächste Schritte

Haben Sie Ihre Registrierung einrichten, ist es Zeit mehr verwenden. Beginnen Sie mit Andockfenster [Registrierung Dokumente]. 

[docker-hub]: https://hub.docker.com/
[registry]: https://github.com/docker/distribution
[Registrierung-Dokumente]: http://docs.docker.com/registry/
[registry-config]: http://docs.docker.com/registry/configuration/
 
