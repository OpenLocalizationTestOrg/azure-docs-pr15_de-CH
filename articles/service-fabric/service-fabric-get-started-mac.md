<properties
   pageTitle="Einrichten der Umgebung für Mac OS X | Microsoft Azure"
   description="Installieren Sie der Runtime SDK und Tools und erstellen Sie lokale Entwicklung Cluster. Nach dieser Installation werden Sie auf Mac OS X erstellen."
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
   ms.date="09/25/2016"
   ms.author="seanmck"/>

# <a name="set-up-your-development-environment-on-mac-os-x"></a>Einrichten der Umgebung für Mac OS X

> [AZURE.SELECTOR]
-[Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

Service Fabric-Anwendung auf Linux-Clustern mit Mac OS X erstellen. Dieser Artikel beschreibt das Einrichten der Mac für die Entwicklung.

## <a name="prerequisites"></a>Erforderliche Komponenten

Fabric Service läuft unter OS X nicht direkt. Führen Sie einen lokalen Dienstfabric-Cluster bieten wir einen vorkonfigurierten Ubuntu virtuellen Computer mit Vagrant und VirtualBox. Bevor Sie beginnen, benötigen Sie:

- [Vagrant (v1.8.4 oder höher)](http://wwww.vagrantup.com/downloads)
- [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

## <a name="create-the-local-vm"></a>Lokale VM erstellen

Um lokale VM enthält einen Cluster mit 5 Knoten Service Fabric zu erstellen, führen Sie folgende Schritte aus:

1. Vagranttfile Repo Klonen

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```

2. Navigieren Sie zu der lokalen Klon des repo

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```

3. (Optional) VM-Standardeinstellungen ändern

    Standardmäßig wird die lokale VM wie folgt konfiguriert:

    - 3 GB Arbeitsspeicher
    - Private Host-Netzwerk konfigurierten IP 192.168.50.50 Passthrough der Datenverkehr vom Mac-Host aktivieren

    Sie können diese Einstellungen ändern oder anderen VM in der Vagranttfile hinzufügen. [Vagrant Dokumentation](http://www.vagrantup.com/docs) die vollständige Liste der Optionen anzeigen

4. Erstellen Sie den virtuellen Computer

    ```bash
    vagrant up
    ```

    Dieser Schritt downloads der vorkonfigurierten VM-Image es lokal und richten Sie einen lokalen Dienstfabric darin cluster booten. Sie sollten es einige Minuten dauern. Wenn Setup erfolgreich abgeschlossen wird, sehen Sie eine Meldung in die Ausgabe gibt an, dass der Cluster gestartet wird.

    ![Cluster-Setup startet den folgenden VM-Bereitstellung][cluster-setup-script]

5. Test, der Cluster korrekt eingerichtet ist (vorausgesetzt, Sie bleiben standardmäßig private Netzwerk-IP) Http://192.168.50.50:19080/Explorer Service Fabric-Explorer navigieren.

    ![Service Fabric-Explorer vom Host Mac angezeigt][sfx-mac]


## <a name="install-the-service-fabric-plugin-for-eclipse-neon-optional"></a>Installieren Sie Service Fabric-Plugin für Eclipse Neon (optional)

Service Fabric bietet eine Plugin für Eclipse Neon IDE, die das Erstellen und Bereitstellen von Java Services vereinfachen können.

1. Eclipse sicher, dass Sie Buildship Version 1.0.17 oder höher installiert sein. Überprüfen Sie die Versionen der installierten Komponenten auswählen **Hilfe > Details zur**. Sie können mit der Buildship [hier][buildship-update].

2. Service Fabric-Plugin installieren, **Hilfe > neue Software installieren**

3. Geben Sie im Textfeld "Arbeiten mit": http://dl.windowsazure.com/eclipse/servicefabric.

4. Klicken Sie auf Hinzufügen.

    ![Eclipse Neon-Plug-In für Service][sf-eclipse-plugin-install]

5. Wählen Sie Service Fabric-Plugin, und klicken Sie auf Weiter.

6. Führen Sie die Installation und akzeptieren Sie den Endbenutzer-Lizenzvertrag.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihre erste Anwendung Fabric Service für Linux](service-fabric-create-your-first-linux-application-with-java.md)

<!-- Links -->

- [Erstellen Sie einen Cluster Service Fabric in Azure-portal](service-fabric-cluster-creation-via-portal.md)
- [Erstellen Sie einen Service Fabric-Cluster mithilfe der Azure-Ressourcen-Manager](service-fabric-cluster-creation-via-arm.md)
- [Verstehen der Service Fabric-Anwendung](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
