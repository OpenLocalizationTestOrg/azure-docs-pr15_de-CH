<properties
   pageTitle="Überwachen ein Clusters Azure Container Service mit Sysdig | Microsoft Azure"
   description="Überwachen eines Clusters Azure Container Service mit Sysdig."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Container, DC/OS Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Überwachen eines Clusters Azure Container Service mit Sysdig

In diesem Artikel werden wir alle Agent-Knoten im Cluster Azure Container Service Sysdig Agenten ausbringen. Benötigen Sie ein Konto mit Sysdig für diese Konfiguration. 

## <a name="prerequisites"></a>Erforderliche Komponenten 

[Bereitstellen](container-service-deployment.md) und [Verbinden](container-service-connect.md) eines Clusters Azure Container Service konfiguriert. Untersuchen der [Marathon Benutzeroberfläche](container-service-mesos-marathon-ui.md). Gehen Sie zu [http://app.sysdigcloud.com](http://app.sysdigcloud.com) Sysdig Cloud-Konto einrichten. 

## <a name="sysdig"></a>Sysdig

Sysdig ist ein Überwachungsdienst, der die Containern innerhalb des Clusters überwachen kann. Sysdig zu Problembehandlung bekannt, aber hat auch die grundlegenden überwachen Metriken für CPU, Netzwerk, Speicher und e/a. Sysdig erleichtert zu sehen, welche Container sind am schwierigsten oder im Wesentlichen mit den Arbeitsspeicher und CPU. Diese Ansicht ist im Abschnitt "Übersicht", die derzeit in der Betaphase. 

![Sysdig-Benutzeroberfläche](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurieren einer Sysdig Bereitstellung Marathon

Diese Schritte zeigen zum Konfigurieren und Bereitstellen von Sysdig Applikationen zum Cluster Marathon. 

Zugriff auf die Benutzeroberfläche DC-Betriebssystem über [http://localhost: 80 /](http://localhost:80/) einmal DC-OS-Benutzeroberfläche Navigieren zu "Universums", die unten links und suchen Sie nach "Sysdig".

![Sysdig im Universum DC-OS](./media/container-service-monitoring-sysdig/sysdig1.png)

Um die Konfiguration abzuschließen benötigen Sie jetzt ein Sysdig Cloud oder ein kostenloses Testabo. Nachdem Sie Sysdig Cloud Website angemeldet sind, klicken Sie auf Ihren Benutzernamen, und auf der Seite finden Sie die "Zugriffstaste". 

![Sysdig API-Schlüssel](./media/container-service-monitoring-sysdig/sysdig2.png) 

Geben Sie die Zugriffstaste Sysdig Konfiguration im Universum DC-OS. 

![Sysdig Konfiguration im Universum DC-OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Jetzt werden die Instanzen 10000000 so Sysdig Cluster ein neuer Knoten hinzugefügt wird automatisch Bereitstellung ein Agenten auf diesen Knoten. Dies wird als Zwischenlösung um sicherzustellen, dass Sysdig für alle neuen Agenten im Cluster bereitstellen. 

![Sysdig Konfiguration in DC/OS Universum-Instanzen](./media/container-service-monitoring-sysdig/sysdig4.png)

Nach der Installation des Pakets zurück zum UI Sysdig navigieren und können zu unterschiedlichen Metriken für Container innerhalb des Clusters. 