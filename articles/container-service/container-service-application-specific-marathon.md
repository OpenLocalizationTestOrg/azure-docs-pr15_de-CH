<properties
   pageTitle="Anwendung oder benutzerspezifische Marathon Service | Microsoft Azure"
   description="Erstellen Sie eine Anwendung oder eine benutzerspezifische Marathon service"
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="Container Marathon Micro-Services DC/OS, Azure"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/12/2016"
   ms.author="rogardle"/>

# <a name="create-an-application-or-user-specific-marathon-service"></a>Erstellen Sie eine Anwendung oder eine benutzerspezifische Marathon service

Azure Container Service bietet eine Reihe von Masterserver auf denen wir Apache Mesos und Marathon vorkonfigurieren. Diese orchestrieren Anwendung im Cluster verwendet werden können, aber es ist am besten nicht die Masterserver für diesen Zweck verwenden. Anpassung der Konfigurations Marathon erfordert beispielsweise die master Server anmelden und Änderungen – Dies fördert eindeutige Masterserver, die anders Standard und gepflegt und verwaltet werden müssen. Ein Team erforderliche Konfiguration möglicherweise außerdem nicht die optimale Konfiguration für ein anderes Teamprojekt.

In diesem Artikel erklären wir, wie eine Anwendung oder eine benutzerspezifische Marathon Service hinzufügen.

Da dieser Dienst auf einen einzelnen Benutzer oder ein Team gehört, können sie in irgendeiner Weise konfigurieren, die sie wünschen. Azure Container Service wird auch sichergestellt, dass der Dienst wird weiterhin ausgeführt. Wenn der Dienst nicht starten Azure Container Service für Sie. In den meisten Fällen bemerken Sie nicht einmal Ausfallzeiten hatte.

## <a name="prerequisites"></a>Erforderliche Komponenten

[Bereitstellen einer Instanz von Azure Container Service](container-service-deployment.md) mit Orchestrator DC/OS und [den Client zum Cluster verbinden kann](container-service-connect.md). Außerdem gehen Sie folgendermaßen vor.

[AZURE.INCLUDE [install the DC/OS CLI](../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a>Erstellen Sie eine Anwendung oder eine benutzerspezifische Marathon service

Zunächst erstellen eine JSON-Konfigurationsdatei, die den Namen des Application Service definiert, die Sie erstellen möchten. Hier verwenden wir `marathon-alice` als Frameworknamens. Speichern Sie die Datei als `marathon-alice.json`:

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

Als Nächstes verwenden Sie CLI DC-OS Marathon-Instanz mit den Optionen installiert, die in der Konfigurationsdatei festgelegt werden:

```bash
dcos package install --options=marathon-alice.json marathon
```

Sollte nun angezeigt werden Ihre `marathon-alice` Dienst auf der Registerkarte Dienste die DC-OS-Benutzeroberfläche. Die Benutzeroberfläche ist `http://<hostname>/service/marathon-alice/` direkt zugreifen soll.

## <a name="set-the-dcos-cli-to-access-the-service"></a>Festlegen der CLI DC-OS auf den Dienst zuzugreifen

Sie können optional die CLI DC-Betriebssystem um diesen neuen Dienst zugreifen die `marathon.url` -Eigenschaft auf die `marathon-alice` Instanz wie folgt:

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

Sie können überprüfen, welche Instanz der CLI gegen arbeitet Marathon der `dcos config show` Befehl. Sie können mit Ihren master Marathon-Dienst mit dem Befehl Wiederherstellen `dcos config unset marathon.url`.
