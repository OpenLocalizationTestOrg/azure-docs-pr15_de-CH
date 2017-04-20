<properties
   pageTitle="Einführung in Azure Container Service | Microsoft Azure"
   description="Azure Container Service bietet eine Möglichkeit zum vereinfachen die Erstellung, Konfiguration und Verwaltung der Cluster virtuellen Computern, die vorkonfiguriert sind, dass Container ausgeführt."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Andockfenster Container Micro-Services Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="azure-container-service-introduction"></a>Einführung in Azure Container Service

Azure Container Service erleichtert das Erstellen, konfigurieren und Verwalten von Cluster virtuellen Computern, die vorkonfiguriert sind, dass Container ausgeführt. Eine optimierte Konfiguration von gängigen Open Source-Planung und Orchestrierung verwendet. Dadurch können Sie Ihre vorhandenen Kenntnisse oder auf eine große und wachsende Community Fachwissen bereitstellen und Verwalten von Container-basierte Anwendung auf Microsoft Azure zeichnen.


![Azure Container Service bietet die Möglichkeit, auf mehrere Hosts Azure Container verwalten.](./media/acs-intro/acs-cluster.png)


Azure Container Service nutzt das Containerformat Andockfenster um sicherzustellen, dass Ihre Anwendungscontainer portablen. Es unterstützt auch Ihrer Wahl Marathon DC/OS und Andockfenster Schwarm diesen Container Tausende oder sogar Zehntausende skaliert werden kann.

Mithilfe von Azure Container Service können Sie unternehmensweite Funktionen von Azure nutzen gleichzeitig Anwendungsportabilität – einschließlich Portabilität auf der Orchestrierung.

<a name="using-azure-container-service"></a>Nutzung der Azure-Container
-----------------------------

Mit Azure Container Service ist ein Behälter Hostingumgebung mit Open Source-Tools und -Technologie, die heute unseren Kunden beliebt sind. Zu diesem Zweck setzen wir API Standardendpunkte für die ausgewählten Orchestrator (DC-OS oder Andockfenster Schwarm). Mithilfe dieser Endpunkte können Sie Software nutzen, die mit diesen Endpunkten. Beispielsweise bei den Endpunkt Andockfenster Schwärmen können Sie Andockfenster Befehlszeilenschnittstelle (CLI) verwenden. Für DC/OS können Sie DCOS CLI verwenden.

<a name="creating-a-docker-cluster-by-using-azure-container-service"></a>Erstellen eines Clusters Andockfenster mithilfe von Azure Container Service
-------------------------------------------------------

Azure Container Dienst zunächst stellen Sie mithilfe einer Vorlage Azure Ressourcenmanager ([Andockfenster Schwarm](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm) oder [DC-OS](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)) oder über die [CLI](/documentation/articles/xplat-cli-install/)einen Cluster Azure Container Service Portal (Suche nach "Azure Container Service") bereit. Schnellstart bereitgestellten Vorlagen können geändert werden, um zusätzliche oder erweiterte Azure-Konfiguration. Weitere Informationen zum Bereitstellen eines Clusters Azure Container Service finden Sie unter [Bereitstellen einer Azure Container Service-Cluster](container-service-deployment.md).

<a name="deploying-an-application"></a>Bereitstellen einer Anwendung
------------------------

Azure Container Service bietet eine Auswahl an Andockfenster Schwarm oder DC-Betriebssystem für die Orchestrierung. Wie Sie Ihre Anwendung bereitstellen, hängt von Ihrer Wahl Orchestrator.

### <a name="using-dcos"></a>DC-OS verwenden

DC-OS ist ein verteiltes Betriebssystem basierend auf Apache Mesos verteilte Systeme Kernel. Apache Mesos ist bei der Apache Software Foundation und enthält einige der [größten IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) Benutzer als Mitwirkende.

![Azure Container Service Schwarm mit Agenten und Master konfiguriert.](media/acs-intro/dcos.png)

DC-Betriebssystem und Apache Mesos beinhalten eine beeindruckend:

-   Bewährte Skalierung

-   Fehlertolerante repliziert Master und Slaves mit Apache ZooKeeper

-   Unterstützung für Container Andockfenster formatiert

-   Systemeigene Isolierung zwischen Aufgaben mit Linux

-   Multiresource Planung (Speicher, CPU, Datenträger und Ports)

-   Java, Python und C++-APIs für neue parallele Anwendungsentwicklung

-   Eine Webbenutzeroberfläche zum Clusterstatus anzeigen

Standardmäßig umfasst DC/OS auf Azure Container Service Marathon Orchestrierung Plattform für die Planung von Arbeitslasten. Jedoch ist mit ACS DC-Betriebssystem enthalten die Mesosphäre Dienste, die zu Ihrem Dienst hinzugefügt werden kann, darunter Spark, Hadoop Cassandra und vieles mehr.

![DC-OS-Universum in Azure Containerservice](media/dcos/universe.png)

#### <a name="using-marathon"></a>Mithilfe von Marathon

Marathon ist für den gesamten Cluster Init und Steuerung in Cgroups – oder bei Azure Container Service Container Andockfenster formatiert. Marathon bietet eine Webbenutzeroberfläche die Bereitstellung Ihrer Anwendung. Können Sie zugreifen, eine URL ähnelt `http://DNS_PREFIX.REGION.cloudapp.azure.com` , in denen DNS\_Präfix und REGION sind zum Zeitpunkt der Bereitstellung definiert. Natürlich können Sie auch Ihre eigenen DNS-Namen bereitstellen. Weitere Informationen zum Ausführen von eines Containers mit der Webbenutzeroberfläche Marathon finden Sie unter [Container Management über Web-Benutzeroberfläche](container-service-mesos-marathon-ui.md).

![Anwendungsliste Marathon](media/dcos/marathon-applications-list.png)

Die REST-APIs können für die Kommunikation mit Marathon. Es gibt eine Anzahl für jedes Client-Bibliotheken. Sie umfassen eine Vielzahl von Sprachen und natürlich können Sie das HTTP-Protokoll in einer beliebigen Sprache. Darüber hinaus unterstützen viele beliebte DevOps Tools Marathon. Dies bietet maximale Flexibilität für das Operations-Team bei der Arbeit mit einem Cluster Azure Container Service. Weitere Informationen zum Ausführen von Container mit Marathon REST-API finden Sie unter [Container Management die REST-API](container-service-mesos-marathon-rest.md).

### <a name="using-docker-swarm"></a>Andockfenster Schwarm verwenden

Andockfenster Schwarm bietet systemeigene clustering Andockfenster. Da Andockfenster Schwarm standard Andockfenster-API fungiert, können Werkzeug, der bereits mit Andockfenster Daemon Schwarm transparent an mehrere Hosts in Azure Container Service skalieren.

![Azure Container Service DC/OS – mit Jumpbox, Agents und Master konfiguriert.](media/acs-intro/acs-swarm2.png)

Unterstützte Tools zum Verwalten von auf einen Schwarm Cluster enthalten, jedoch nicht die folgenden:

-   Dokku

-   Andockfenster CLI und Andockfenster erstellen

-   Krane

-   Jenkins

<a name="videos"></a>Videos
------

Erste Schritte mit Azure Container Service (101):  

> [AZURE.VIDEO azure-container-service-101]

Building Applications Nutzung der Azure-Container (Build 2016)

> [AZURE.VIDEO build-2016-building-applications-using-the-azure-container-service]
