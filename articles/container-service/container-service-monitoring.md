<properties
   pageTitle="Überwachen ein Clusters Azure Container Service mit Datadog | Microsoft Azure"
   description="Überwachen eines Clusters Azure Container Service mit Datadog. Verwenden Sie die DC-OS-Web-Benutzeroberfläche der Datadog-Agenten zum Cluster."
   services="container-service"
   documentationCenter=""
   authors="rbitia"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="DC/OS Andockfenster Schwarm Azure-Container"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure"   
   ms.date="07/28/2016"
   ms.author="t-ribhat"/>

# <a name="monitor-an-azure-container-service-cluster-with-datadog"></a>Überwachen eines Clusters Azure Container Service mit Datadog

In diesem Artikel werden wir alle Agent-Knoten im Cluster Azure Container Service Datadog Agenten ausbringen. Für diese Konfiguration benötigen Sie ein Konto mit Datadog. 

## <a name="prerequisites"></a>Erforderliche Komponenten 

[Bereitstellen](container-service-deployment.md) und [Verbinden](container-service-connect.md) eines Clusters Azure Container Service konfiguriert. Untersuchen der [Marathon Benutzeroberfläche](container-service-mesos-marathon-ui.md). Gehen Sie zu [http://datadoghq.com](http://datadoghq.com) Datadog-Konto einzurichten. 

## <a name="datadog"></a>Datadog 

Datadog ist ein Überwachungsdienst, der Daten aus den Containern innerhalb des Clusters Azure Container Service erfasst. Datadog hat ein Andockfenster Integration Dashboard Metriken in den Containern Sie finden. CPU, Speicher, Netzwerk und e/a-Daten gesammelt aus den Containern organisiert. Datadog teilt Metriken in Containern und Bilder. Unten ist ein Beispiel für die Benutzeroberfläche für die CPU-Auslastung sieht.

![Datadog-Benutzeroberfläche](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurieren einer Datadog Bereitstellung Marathon

Diese Schritte zeigen zum Konfigurieren und Bereitstellen von Datadog Applikationen zum Cluster Marathon. 

Zugriff auf die Benutzeroberfläche DC-Betriebssystem über [http://localhost: 80 /](http://localhost:80/). Einmal die DC-OS-Benutzeroberfläche Navigieren zu "Universums" auf der linken und suchen Sie nach "Datadog" und klicken Sie auf "Installieren".

![Datadog-Paket im Universum DC-OS](./media/container-service-monitoring/datadog1.png)

Jetzt zum Abschließen der Konfiguration möchten ein Datadog oder ein kostenloses Testabo. Wenn in Datadog Website sehen links angemeldet sind und zur Integration-API >. 

![Datadog API-Schlüssel](./media/container-service-monitoring/datadog2.png)

Geben Sie den API-Schlüssel in die Datadog Konfiguration im Universum DC-OS. 

![Datadog Konfiguration im Universum DC-OS](./media/container-service-monitoring/datadog3.png) 

In der Konfiguration oben Instanzen 10000000 soll, wenn ein neuer Knoten zum Cluster hinzugefügt wird Datadog Ausbringen eines Agenten automatisch auf diesen Knoten. Dies ist eine vorläufige Lösung. Nachdem Sie das Paket installiert haben, das sollten Sie auf der Datadog Website navigieren und suchen "Dashboards" Dort sehen Sie benutzerdefinierte und Integration Dashboards. Andockfenster Integration Dashboard müssen alle Container Metriken für Cluster überwachen. 
