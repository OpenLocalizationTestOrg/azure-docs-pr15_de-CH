<properties
   pageTitle="DC/OS CLI installieren | Microsoft Azure"
   description="Installieren Sie DC/OS CLI."
   services="container-service"
   documentationCenter=""
   authors="rgardler"
   manager="timlt"
   editor=""
   tags="acs, azure-container-service"
   keywords="DC/OS, Azure Container Micro-services"/>

<tags
   ms.service="container-service"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="rogardle"/>

>[AZURE.NOTE] Dies ist für die Zusammenarbeit mit ACS DC/OS-basierten Clustern. Es ist nicht erforderlich für ACS Schwarm-basierten Clustern.

Erstens [ACS DC/OS-basierten Cluster herstellen](../articles/container-service/container-service-connect.md). Nachdem Sie dies getan haben, können Sie auf dem Clientcomputer mit den folgenden Befehlen DC-OS-CLI installieren:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Wenn Sie eine ältere Version von Python verwenden, bemerken Sie einige "InsecurePlatformWarnings". Sie können diese ignorieren.

Um zunächst ohne Neustart der Shell ausführen:

```bash
source ~/.bashrc
```

Dieser Schritt ist nicht erforderlich, beim Starten von neuer Schalen.

Jetzt können Sie bestätigen, dass die CLI installiert ist:

```bash
dcos --help
```