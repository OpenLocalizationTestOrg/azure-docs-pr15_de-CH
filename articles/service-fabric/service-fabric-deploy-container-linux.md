<properties
   pageTitle="Service Fabric und Bereitstellen von Containern in Linux | Microsoft Azure"
   description="Service Fabric und die Verwendung von Andockfenster Container Microservice Anwendung bereitstellen. Dieser Artikel beschreibt die Funktionen Fabric Service für Container enthält und zum Bereitstellen eines Abbilds Andockfenster Container in einem Cluster"
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
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="deploy-a-docker-container-to-service-fabric"></a>Einen Andockfenster Container für Fabric Service bereitstellen

> [AZURE.SELECTOR]
- [Bereitstellen von Windows-Container](service-fabric-deploy-container.md)
- [Andockfenster Container bereitstellen](service-fabric-deploy-container-linux.md)

Dieser Artikel führt Sie durch Haustechnik Container Andockfenster Container unter Linux.

Fabric Service bietet verschiedene Container, mit denen Sie beim Erstellen von Microservices sind in Containern bestehen Applikationen. Diese sind Container Dienste bezeichnet.

Die Funktionen umfassen.

- Container Bereitstellung und Aktivierung
- Ressource-governance
- Repository-Authentifizierung
- Container-Port Host Port-Zuordnung
- Containern, Discovery und Kommunikation
- Konfigurieren und Festlegen von Umgebungsvariablen


## <a name="packaging-a-docker-container-with-yeoman"></a>Einen Andockfenster Container Verpackung mit yeoman
Beim Packen Container unter Linux können Sie entweder Yeoman Vorlage oder [das Anwendungspaket manuell erstellen](service-fabric-deploy-container.md#manually-packaging-and-deploying-a-container).

Service Fabric-Anwendung kann eine oder mehrere Container, jeweils eine bestimmte Rolle in der Anwendung Funktionalität enthalten. Service Fabric-SDK für Linux enthält einen [Yeoman](http://yeoman.io/) -Generator, der leicht zu der Anwendung ein Bild Container hinzufügen. Nehmen Sie zum Erstellen einer neuen Anwendung mit einer einzigen Andockfenster Behälter *SimpleContainerApp*Yeoman. Sie können weitere Dienste später Dateien durch Bearbeiten der generierten manifest.

## <a name="create-the-application"></a>Erstellen der Anwendung

1. Geben Sie in einem Terminal **yo Azuresfguest**.

2. Rahmen wählen Sie **Container** .

3. Nennen Sie die Anwendung z. B. SimpleContainerApp

4. Geben Sie die URL für das Bild Container DockerHub Repo. Sie hat das Format [Repo] / [image Name]

![Service Fabric Yeoman-Generator für Container][sf-yeoman]

## <a name="deploy-the-application"></a>Bereitstellen der Anwendung

Sobald die Anwendung kompiliert wird, können Sie es mit lokalen CLI Azure bereitstellen.

1. Verbinden Sie mit dem lokalen Cluster Service Fabric.

    ```bash
    azure servicefabric cluster connect
    ```

2. Verwenden Sie in der Vorlage bereitgestellte Skript für die Installation des Clusters Abbildspeicher Anwendungspaket soll den Anwendungstyp registrieren und erstellen Sie eine Instanz der Anwendung.

    ```bash
    ./install.sh
    ```

3. Öffnen Sie einen Browser, und navigieren Sie zur Http://localhost:19080-Explorer (Localhost ersetzen mit der privaten IP-VM, wenn Vagrant auf Mac OS X) Service Fabric-Explorer.

4. Erweitern Sie den Knoten Applikationen und gibt nun einen Eintrag für Ihre Anwendung und einen weiteren für die erste Instanz des Typs.

5. Verwenden Sie in der Vorlage enthaltenen Deinstallationsskript löschen die Anwendungsinstanz und Anwendungstyp Registrierung.

    ```bash
    ./uninstall.sh
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Übersicht über Service und Container](service-fabric-containers-overview.md)
- [Interaktion mit Service Fabric-Cluster mithilfe der Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman.png

