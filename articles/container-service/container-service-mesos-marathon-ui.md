<properties
   pageTitle="Azure Container Container Servicemanagement über die Webbenutzeroberfläche | Microsoft Azure"
   description="Bereitstellen Sie Container für einen Clusterdienst Azure Container Service mithilfe Marathon Webbenutzeroberfläche."
   services="container-service"
   documentationCenter=""
   authors="neilpeterson"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Andockfenster Container Micro-Services Mesos, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="timlt"/>

# <a name="container-management-through-the-web-ui"></a>Container-Management über Web-Benutzeroberfläche

DC-Betriebssystem stellt eine Umgebung für die Bereitstellung und Skalierung gruppierte Arbeitslasten und die zugrunde liegende Hardware. Auf DC/OS ist ein Framework, die Planung und Ausführung Compute Arbeitslasten verwaltet.

Frameworks für viele gängige Arbeitslasten verfügbar sind, wird das Erstellen und skalieren Container Bereitstellungen mit Marathon dieses Dokuments beschrieben. Vor dem Durcharbeiten dieser Beispiele benötigen Sie einen DC-OS-Cluster, der in Azure Container Service konfiguriert. Sie müssen auch Remoteverbindungen mit diesem Cluster. Weitere Informationen zu diesen Elementen finden Sie in folgenden Artikeln:

- [Bereitstellen eines Clusters Azure Container Service](container-service-deployment.md)
- [Verbinden Sie mit einem Cluster Azure Container Service](container-service-connect.md)

## <a name="explore-the-dcos-ui"></a>DC/OS UI durchsuchen

Suchen Sie mit einem Secure Shell (SSH) eingerichtet http://localhost/. Das DC-OS Webbenutzeroberfläche geladen und zeigt Informationen zum Cluster verwendete Ressourcen Wirkstoffe und Dienste.

![DC-OS-BENUTZEROBERFLÄCHE](media/dcos/dcos2.png)

## <a name="explore-the-marathon-ui"></a>Untersuchen den Marathon Benutzeroberfläche

Wechseln Sie zu http://localhost/Marathon, Marathon-Benutzeroberfläche finden. In diesem Fenster können Sie einen neuen Container oder eine andere Anwendung auf dem Cluster Azure Container Service DC/OS starten. Finden Sie unter Informationen zum Container und Programme ausführen.  

![Marathon Benutzeroberfläche](media/dcos/dcos3.png)

## <a name="deploy-a-docker-formatted-container"></a>Einen Andockfenster formatiert Container bereitstellen

Einen neuen Container mit Marathon bereitzustellen, klicken Sie auf **Anwendung erstellen** und die folgende Informationen in das Formular eingeben:

Feld           | Wert
----------------|-----------
ID              | nginx
Bild           | nginx
Netzwerk         | Überbrücken
Host-Anschluss       | 80
Protokoll        | TCP

![Neue Benutzeroberfläche der Anwendung – Allgemein](media/dcos/dcos4.png)

![Neue Benutzeroberfläche – Andockfenster Container-Anwendung](media/dcos/dcos5.png)

![Neue Anwendung UI - Ports und Dienstermittlung](media/dcos/dcos6.png)

Wenn Sie Port auf dem Agent Container Port statisch zuordnen möchten, müssen Sie JSON-Modus verwenden. Hierzu Wechseln der neue Assistent **JSON** -Modus mithilfe der umschalten Geben Sie Folgendes in die `portMappings` Abschnitt der Anwendungsdefinition. In diesem Beispiel bindet Port 80 des Containers an Port 80 des DC-OS-Agenten. Nachdem Sie diese Änderung vornehmen, können Sie dieser Assistent JSON-Modus wechseln.

```none
"hostPort": 80,
```

![Neue Anwendung UI - Port 80-Beispiel](media/dcos/dcos13.png)

DC-OS-Cluster wird mit privaten und öffentlichen bereitgestellt. Für den Cluster Applikationen über das Internet zugreifen können müssen Sie die Anwendung öffentliche Mittel bereitstellen. Hierzu Registerkarte des Assistenten neue Anwendung **Optional** , und geben Sie **Slave_public** für die **Ressource Rollen akzeptiert**.

![Neue Benutzeroberfläche der Anwendung – öffentliche Mittel festlegen](media/dcos/dcos14.png)

Auf der Hauptseite Marathon sehen Sie den Bereitstellungsstatus für den Container.

![Hauptseite UI - Container Bereitstellungsstatus Marathon](media/dcos/dcos7.png)

Beim Wechseln zum DC-OS Webbenutzeroberfläche (http://localhost/) sehen Sie, dass eine Aufgabe (in diesem Fall einen Container Andockfenster formatiert) auf dem DC-OS-Cluster ausgeführt wird.

![DC-OS Webbenutzeroberfläche - Vorgang auf dem Cluster ausgeführt](media/dcos/dcos8.png)

Sie sehen auch Cluster-Knoten, den die Aufgabe ausgeführt wird.

![DC-OS Webbenutzeroberfläche - Clusterknoten Aufgabe](media/dcos/dcos9.png)

## <a name="scale-your-containers"></a>Skaliert die Containern

Marathon-Benutzeroberfläche können Sie die Anzahl der Instanzen eines Containers skalieren. Dazu zur **Marathon** Seite Wählen Sie den Container zu skalieren und **Skalierung** klicken. Klicken Sie im Dialogfeld **Anwendung** Geben Sie die Anzahl der Containerinstanz und wählen Sie **Anwendung aus**.

![Marathon UI - Dialogfeld Anwendung](media/dcos/dcos10.png)

Nach Abschluss die Skalierung sehen Sie mehrere Instanzen des gleichen Vorgangs DC-OS-Agenten verteilt.

![Benutzeroberfläche Cockpit DC-OS - Aufgabe Verbreitung über Agents](media/dcos/dcos11.png)

![DC-OS Webbenutzeroberfläche - Knoten](media/dcos/dcos12.png)

## <a name="next-steps"></a>Nächste Schritte

- [Arbeiten Sie mit DC/OS und Marathon-API](container-service-mesos-marathon-rest.md)

Tiefer Einblick in Azure Container Service mit Mesos

> [AZURE. VIDEO] Azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos]
