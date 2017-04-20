<properties
   pageTitle="Richten Sie unter Linux Umgebung | Microsoft Azure"
   description="Laufzeit und SDK installieren und Linux Entwicklung Cluster erstellen. Nach dieser Installation werden Sie Applikationen erstellt."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/26/2016"
   ms.author="seanmck"/>

# <a name="prepare-your-development-environment-on-linux"></a>Vorbereiten der Umgebung für Linux


> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Bereitstellen und Ausführen von [Azure Service Fabric-Anwendung](service-fabric-application-model.md) auf Ihrem Entwicklungscomputer Linux installieren Sie Runtime und allgemeine SDK. Sie können auch optionale SDKs für Java und .NET Core.

## <a name="prerequisites"></a>Erforderliche Komponenten
### <a name="supported-operating-system-versions"></a>Unterstützte Betriebssystemversionen
Für die Entwicklung sind die folgenden Betriebssystemversionen unterstützt:

- Ubuntu 16.04 (einem Xenial)

## <a name="update-your-apt-sources"></a>Apt Quelltexte

Um das SDK und das zugeordnete Laufzeitpaket über apt-Get zu installieren, müssen Sie zuerst apt Quellen aktualisieren.

1. Öffnen Sie ein Terminal.
2. Service Fabric Repo zur Quellliste hinzufügen.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ trusty main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Den neuen GPG-Schlüssel in Ihren Schlüsselbund apt hinzufügen

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    ```

4. Aktualisieren Sie die Paketliste basierend auf den neu hinzugefügten.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-the-sdk"></a>Installieren und Einrichten des SDK

Nach der Aktualisierung von Quellen können Sie das SDK installieren.

1. Installieren Sie das Paket Service Fabric-SDK. Die Installation bestätigen und einen Lizenzvertrag zustimmen gebeten werden.

    ```bash
    sudo apt-get install servicefabricsdkcommon
    ```

2. Führen Sie das Setupskript SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/sdkcommonsetup.sh
    ```

## <a name="set-up-the-azure-cross-platform-cli"></a>Einrichten von Azure plattformübergreifende CLI

[Azure plattformübergreifende CLI] [ azure-xplat-cli-github] enthält Befehle für die Interaktion mit Service Fabric-Entitäten, einschließlich Cluster und Applikationen. Basiert auf Node.js so [sicher, dass Knoten installiert] [ install-node] vor beschrieben.

1. Klonen Sie Github Repo auf Ihrem Entwicklungscomputer.

    ```bash
    git clone https://github.com/Azure/azure-xplat-cli.git
    ```

2. Wechseln Sie in die geklonte Repo und installieren Sie die CLI Abhängigkeiten mit Knoten Paket-Manager (Npm).

    ```bash
    cd azure-xplat-cli
    npm install
    ```

3. Erstellen Sie einen Symlink aus dem Ordner Bin-Azure geklonte Repo auf /usr/bin/azure wird zum Pfad hinzugefügt, und Befehle aus dem Verzeichnis verfügbar sind.

    ```bash
    sudo ln -s $(pwd)/bin/azure /usr/bin/azure
    ```

4. Schließlich können Sie automatische Vervollständigung Service Fabric-Befehle.

    ```bash
    azure --completion >> ~/azure.completion.sh
    echo 'source ~/azure.completion.sh' >> ~/.bash_profile
    source ~/azure.completion.sh
    ```

## <a name="set-up-a-local-cluster"></a>Richten Sie einen lokalen cluster

Wenn alles erfolgreich installiert wurde, sollte Sie einen lokalen Cluster gestartet.

1. Cluster Setup-Skript ausführen.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```

2. Öffnen Sie einen Webbrowser, und navigieren Sie zu Http://localhost:19080-Explorer. Cluster gestartet wurde, sollte das Dashboard Service Fabric-Explorer angezeigt werden.

    ![Service Fabric-Explorer unter Linux][sfx-linux]

An diesem Punkt können Sie vorgefertigte Service Fabric-Anwendungspakete oder neue basierend auf Gast ausführbare oder Gast Container bereitstellen. Zum Erstellen neuer Services mit Java oder .NET Core-SDKs gehen Sie optionales Setup.

## <a name="install-the-java-sdk-and-eclipse-neon-plugin-optional"></a>Installieren der Java SDK und Neon Eclipse-Plug-in (optional)

Java SDK enthält Bibliotheken und Vorlagen zu Service Fabric-Services mit Java erforderlich.

1. Installieren Sie das Java SDK-Paket.

    ```bash
    sudo apt-get install servicefabricsdkjava
    ```

2. Führen Sie das Setupskript SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/java/sdkjavasetup.sh
    ```

Sie können das Eclipse Plugin für Service Fabric Eclipse Neon IDE.

1. Eclipse sicher, dass Sie Buildship Version 1.0.17 oder höher installiert sein. Überprüfen Sie die Versionen der installierten Komponenten auswählen **Hilfe > Details zur**. Sie können mit der Buildship [hier][buildship-update].

2. Service Fabric-Plugin installieren, **Hilfe > neue Software installieren**

3. Geben Sie im Textfeld "Arbeiten mit": http://dl.windowsazure.com/eclipse/servicefabric

4. Klicken Sie auf Hinzufügen.

    ![Eclipse plugin][sf-eclipse-plugin]

5. Wählen Sie Service Fabric-Plugin, und klicken Sie auf Weiter.

6. Führen Sie die Installation und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

## <a name="install-the-net-core-sdk-optional"></a>Installieren Sie .NET Core SDK (optional)

.NET Core SDK stellt Bibliotheken und Vorlagen mit plattformübergreifende .NET Core Service Fabric-Services erstellen müssen.

1. Installieren Sie das Paket .NET Core SDK.

    ```bash
    sudo apt-get install servicefabricsdkcsharp
    ```

2. Führen Sie das Setupskript SDK.

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/csharp/sdkcsharpsetup.sh
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihre erste Java-Anwendung auf Linux](service-fabric-create-your-first-linux-application-with-java.md)

- [Vorbereiten der Umgebung für OSX](service-fabric-get-started-mac.md)


<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
