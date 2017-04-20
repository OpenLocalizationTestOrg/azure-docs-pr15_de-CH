<properties
   pageTitle="Service Fabric nächste Erstellungsschritte | Microsoft Azure"
   description="Dieser Artikel enthält Links zu einer Reihe von wichtigen Entwicklungsaufgaben für Service"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="your-service-fabric-application-and-next-steps"></a>Der Service Fabric-Anwendung und nächste Schritte
Die Azure Service Fabric-Anwendung wurde erstellt. Dieser Artikel beschreibt den Aufbau des Projekts und einige mögliche nächste Schritte.

## <a name="your-application"></a>Die Anwendung
Jede neue Anwendung umfasst ein. Möglicherweise ein oder zwei zusätzliche Projekte je nach Dienst ausgewählt.

### <a name="the-application-project"></a>Das Anwendungsprojekt
Das Anwendungsprojekt besteht aus:

- Ein Satz von Verweisen auf die Dienste, die die Anwendung bilden.

- Zwei veröffentlichen Profile (lokal und Cloud), mit denen Sie Voreinstellungen für das Arbeiten mit unterschiedlichen Umgebungen – z. B. standardmäßig einen Cluster-Endpunkt und ob Upgrade Bereitstellung durchführen Zusammenhang zu.

- Zwei Parameter Anwendungsdateien (lokal und Cloud), mit denen Sie umgebungsspezifischen Anwendungskonfigurationen, wie die Anzahl der Partitionen erstellen für einen Dienst verwalten.

- Ein Bereitstellungsskript, mit denen Sie Ihre Anwendung über die Befehlszeile oder als Teil der automatisierte fortlaufende Integration und Bereitstellung Pipeline bereitstellen.

- Das Anwendungsmanifest, das die Anwendung beschreibt. Das Manifest finden Sie im Ordner ApplicationPackageRoot.

### <a name="stateless-service"></a>Statusfreie service
Beim Hinzufügen eines neuen Dienstes statusfreien Visual Studio Service-Projekt der Projektmappe hinzugefügt, die eine vom enthält `StatelessService`. Der Dienst wird eine lokale Variable in einem Zähler erhöht.

### <a name="stateful-service"></a>Statusbehaftete Dienst
Beim Hinzufügen eines neuen statusbehafteten Dienstes Visual Studio Service-Projekt der Projektmappe hinzugefügt, die eine vom enthält `StatefulService`. Der Dienst erhöht einen Zähler in die `RunAsync` -Methode und speichert das Ergebnis in einem `ReliableDictionary`.

### <a name="actor-service"></a>Schauspieler-service
Beim Hinzufügen eines neuen zuverlässigen Akteurs Visual Studio zwei Projekte der Projektmappe hinzugefügt: ein Akteur-Projekt und ein Projekt.

Schauspieler-Projekt bietet Methoden zum Festlegen und den Wert eines Leistungsindikators abrufen zuverlässig innerhalb der Akteur Zustand beibehalten wird. Das Projekt bietet eine Schnittstelle, mit dem andere Dienste aufrufen den Akteur.

### <a name="stateless-web-api"></a>Statusfreie Web-API
Statusfreie Web API-Projekt bietet einen einfachen Webdienst, mit denen Sie Ihre Anwendung für externe Clients zu öffnen. Weitere Informationen das Projekt aufgebaut, finden Sie unter [Service Fabric Web API-Services mit owin-Self-hosting](service-fabric-reliable-services-communication-webapi.md).

### <a name="aspnet-core"></a>ASP.NET core

Service Fabric-SDK enthält dieselben ASP.NET Core verfügbaren Vorlagen für eigenständige ASP.NET Core Projekte: leere [Web API][aspnet-webapi], und [Web Application][aspnet-webapp].

## <a name="next-steps"></a>Nächste Schritte
### <a name="create-an-azure-cluster"></a>Einen Azure-Cluster erstellen
Service Fabric-SDK stellt einen lokalen Cluster für Entwicklung und testing. Zum Erstellen eines Clusters in Azure finden Sie unter [Einrichten eines Service Fabric-Clusters von Azure-Portal][create-cluster-in-portal].

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>Installieren Sie in Azure kostenlos mit Partei

Möchten Sie versuchen bereitstellen und Verwalten von Applikationen in Azure ohne eigene Cluster einrichten, können Sie kostenlosen [Partei Clusterdienst](http://aka.ms/tryservicefabric).

### <a name="publish-your-application-to-azure"></a>Veröffentlichen Sie die Anwendung in Azure
Sie können die Anwendung direkt aus Visual Studio eine Azure-Cluster veröffentlichen. Um zu erfahren, finden Sie unter [Veröffentlichen der Anwendung in Azure][publish-app-to-azure].

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>Mit dem Service Fabric Explorer Cluster visualisieren
Service Fabric-Explorer bietet eine einfache Möglichkeit des Clusters einschließlich bereitgestellten Applikationen und physischen Layout visuell darstellen. Mehr Informationen finden Sie unter [Visualisierung Cluster Service Fabric-Explorer mit][visualize-with-sfx].

### <a name="version-and-upgrade-your-services"></a>Version und Ihre Dienste
Service Fabric können unabhängige Versionierung und Aktualisierung von unabhängigen Diensten in einer Anwendung. Finden Sie weitere [Versionierung und Aktualisierung Ihrer Dienste][app-upgrade-tutorial].

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>Fortlaufenden Integration mit Visual Studio Team Services konfigurieren
Fortlaufende Integration Prozess für die Anwendung Service Fabric festlegen finden Sie unter [Konfigurieren fortlaufende Integration mit Visual Studio Team Services][ci-with-vso].


<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html
