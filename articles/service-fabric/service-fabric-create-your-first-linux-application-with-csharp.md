<properties
   pageTitle="Erstellen Ihrer erste Service Fabric-Anwendung auf Linux mit C# | Microsoft Azure"
   description="Erstellen und Bereitstellen von Service Fabric Anwendung C#"
   services="service-fabric"
   documentationCenter="csharp"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="csharp"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/04/2016"
   ms.author="subramar"/>


# <a name="create-your-first-azure-service-fabric-application"></a>Erstellen Sie Ihre erste Azure Service Fabric-Anwendung

> [AZURE.SELECTOR]
- [C# - Windows](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java - Linux](service-fabric-create-your-first-linux-application-with-java.md)
- [C# - Linux](service-fabric-create-your-first-linux-application-with-csharp.md)

Service Fabric bietet SDKs für das Erstellen von Webdiensten unter Linux in .NET Core und Java. In dieser Übung betrachten wir zum Erstellen einer Anwendung für Linux und ein Dienst mit C# (.NET Core).

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass [Linux Umgebung eingerichtet](service-fabric-get-started-linux.md)haben. Wenn Sie Mac OS X verwenden, können Sie [eine Linux-ein-Feld-Umgebung auf einem virtuellen Computer mit Vagrant](service-fabric-get-started-mac.md).

## <a name="create-the-application"></a>Erstellen der Anwendung

Service Fabric-Anwendung kann einen oder mehrere Dienste mit einer bestimmten Rolle in der Anwendung Funktionalität enthalten. Service Fabric-SDK für Linux enthält einen [Yeoman](http://yeoman.io/) -Generator, der den ersten Dienst erstellen und später hinzufügen erleichtert. Nehmen Sie Yeoman zum Erstellen einer Anwendung mit einem einzelnen Dienst.

1. Geben Sie den folgenden Befehl zum Erstellen der Gerüstbau in einem Terminal:`yo azuresfcsharp`

2. Nennen Sie die Anwendung.

3. Wählen Sie den ersten Dienst und Namen. Für die Zwecke dieses Lernprogramms wählen wir Akteur Serviceangebot.

  ![Service Fabric Yeoman-Generator für C#][sf-yeoman]

>[AZURE.NOTE] Weitere Informationen zu den Optionen finden Sie unter [Service Fabric programming Model Overview](service-fabric-choose-framework.md).

## <a name="build-the-application"></a>Erstellen Sie die Anwendung

Service Fabric Yeoman-Vorlagen enthalten ein Buildskript, mit denen Sie die app vom Terminal (nach dem Navigieren in den Anwendungsordner) erstellen.

  ```bash
 cd myapp 
 ./build.sh 
  ```

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

## <a name="start-the-test-client-and-perform-a-failover"></a>Starten Sie den Testclient und führen Sie einen failover

Schauspieler-Projekte auf eigene nicht. Sie müssen einen anderen Dienst oder Client ihnen Nachrichten senden. Akteur-Vorlage enthält einen einfachen Testskripts, mit denen Sie interagieren mit den Akteur.

1. Führen Sie das Skript mit dem Dienstprogramm Überwachung der Akteur Dienst ausgegeben.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie in Service Fabric-Explorer Knoten primäre Replikat für den Akteur-Dienst. In der Abbildung unten ist Knoten 3.

    ![Primäre Replikat Suchen in Service Fabric-Explorer][sfx-primary]

3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt gefunden und wählen Sie **deaktivieren (Neustart aus)** im Menü Aktionen. Diese Aktion startet eine der fünf Knoten im lokalen Cluster erzwingen einen Failover zu einem sekundären Replikat auf einen anderen Knoten. Wie diese Aktion Beachten Sie die Ausgabe Testclient und Hinweis der Zähler weiterhin trotz der Failover erhöht.


## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über zuverlässige Akteure](service-fabric-reliable-actors-introduction.md)
- [Interaktion mit Service Fabric-Cluster mithilfe der Azure-CLI](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
