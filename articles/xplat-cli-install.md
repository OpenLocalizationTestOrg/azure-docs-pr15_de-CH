<properties
    pageTitle="Installieren der Befehlszeilenschnittstelle Azure | Microsoft Azure"
    description="Installieren der Azure-Befehlszeilenschnittstelle (CLI) für Mac, Linux und Windows Azure Services nutzen"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="squillace"
    services="virtual-machines-linux,virtual-network,storage,azure-resource-manager"
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="rasquill"/>
    
# <a name="install-the-azure-cli"></a>Azure CLI installieren

> [AZURE.SELECTOR]
- [PowerShell](powershell-install-configure.md)
- [Azure CLI](xplat-cli-install.md)

Installieren Sie der Azure-Befehlszeilenschnittstelle (CLI Azure) zur Verwendung der Open Source-Shell basierende Befehle zum Erstellen und Verwalten von Ressourcen in Microsoft Azure schnell. Sie haben verschiedene Optionen, um diese plattformübergreifende Tools auf Ihrem Computer installiert: 

* **Paket Npm** - Npm (Package Manager für JavaScript) das neueste Azure-CLI-Paket auf dem Linux-Distribution oder Betriebssystem installieren ausführen. Erfordert node.js und Npm auf Ihrem Computer.
* **Installer** - Download eine einfache Installation auf Mac oder Windows Installer.
* **Andockfenster Container** - mit der neuesten CLI in Container Andockfenster sofort ausführen. Andockfenster Host auf dem Computer erforderlich.
    
Weitere Optionen und Hintergrund finden Sie auf das Projekt-Repository [GitHub](https://github.com/azure/azure-xplat-cli). 

Azure-CLI installiert, [mit der Azure-Abonnement verbinden](xplat-cli-connect.md) und **Azure** -Befehle über die Befehlszeilenschnittstelle (Bash, Terminal, Befehlszeile usw.) mit Azure Ressourcen ausgeführt.



## <a name="option-1-install-an-npm-package"></a>Option 1: Installieren eines Pakets npm

Um die CLI ein Npm Paket installieren, stellen Sie sicher heruntergeladen und installiert die [neueste Node.js und Npm](https://nodejs.org/en/download/package-manager/). Führen Sie dann das **Npm installieren** , um Azure-Cli-Paket zu installieren: 

    npm install -g azure-cli

Auf Linux-Distributionen müssen Sie **Sudo** erfolgreich Befehls __Npm__ wie folgt verwenden:

    sudo npm install -g azure-cli

> [AZURE.NOTE]Sie installieren oder Node.js und Npm auf dem Linux-Distribution oder Betriebssystem aktualisieren müssen, sollten Node.js LTS aktuellste (4.x) installieren. Wenn Sie eine ältere Version verwenden, erhalten Sie Fehler bei der Installation. 

Sie laden die neueste Linux [Tar-Datei] [ linux-installer] des Npm-Pakets lokal. Installieren Sie das heruntergeladene Npm Paket dann wie folgt (Linux-Distributionen **Sudo**verwenden müssen):

    npm install -g <path to downloaded tar file>

## <a name="option-2-use-an-installer"></a>Option 2: Verwenden eines Installationsprogramms

Wenn Sie Mac oder Windows Computer verwenden, werden die folgenden CLI-Installationsprogramme zum Download zur Verfügung:

* [Mac OS X Installationsprogramm][mac-installer]

* [Windows MSI][windows-installer] 

>[AZURE.TIP]Unter Windows können Sie auch den [Webplattform-Installer](https://go.microsoft.com/?linkid=9828653) die CLI installieren herunterladen. Dieses Installationsprogramm kann zusätzliche Azure SDK und Befehlszeilentools installieren, nachdem die CLI. 


## <a name="option-3-use-a-docker-container"></a>Option 3: Verwenden eines Andockfenster Containers

Wenn Ihr Computer als Host [Andockfenster](https://docs.docker.com/engine/understanding-docker/) eingerichtet haben, können Sie die neuesten Azure-CLI in einem Container Andockfenster ausführen. Führen Sie den folgenden Befehl (auf Linux-Distributionen **Sudo**verwenden müssen):

```
docker run -it microsoft/azure-cli
```


## <a name="run-azure-cli-commands"></a>Azure-CLI-Befehle ausführen
Nachdem der Azure-CLI installiert ist, führen Sie den Befehl **Azure** von der Befehlszeile Benutzeroberfläche (Bash Terminal, Befehlszeile und usw.). Geben Sie Folgendes ein, um den Hilfe-Befehl:

```
azure help
```
> [AZURE.NOTE]Auf einigen Linux-Distributionen möglicherweise eine Fehlermeldung ähnlich `/usr/bin/env: ‘node’: No such file or directory`. Dieser Fehler stammt aus aktuellen Installationen von Node.js in /usr/bin/nodejs installiert. Um dies zu beheben, erstellen Sie einen symbolischen Link auf /usr/bin/node durch Ausführen dieses Befehls:

```
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

Version der CLI Azure finden Sie installiert, geben Sie Folgendes ein:

```
azure --version
```

Jetzt sind Sie bereit! Die CLI-Befehle mit eigenen Ressourcen [Verbinden mit der Azure-Abonnement von Azure-CLI](xplat-cli-connect.md)zu gelangen.

>[AZURE.NOTE] Bei Verwendung von Azure CLI sehen Sie gefragt, ob Microsoft Informationen gesammelt werden sollen. Die Teilnahme ist freiwillig. Wenn Sie teilnehmen möchten, können Sie aufhören jederzeit mit `azure telemetry --disable`. Um Teilnahme jederzeit aktivieren, führen Sie `azure telemetry --enable`.


## <a name="update-the-cli"></a>Aktualisieren der CLI

Microsoft veröffentlicht regelmäßig aktualisierte Versionen der Azure-CLI. Die CLI mit dem Installer für Ihr Betriebssystem installieren oder den neuesten Andockfenster Container ausgeführt werden. Oder wenn Sie die neuesten Node.js und Npm installiert haben, aktualisieren eingeben (auf Linux-Distributionen **Sudo**verwenden müssen).

```
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Aktivieren der Registerkarte Abschluss

Registerkarte Abschluss der CLI-Befehle wird für Mac und Linux unterstützt.

Um diese Zsh zu aktivieren, führen Sie Folgendes aus:

```
echo '. <(azure --completion)' >> .zshrc
```

Um in Bash zu aktivieren, führen Sie Folgendes aus:

```
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Nächste Schritte 

* [Verbinden von der CLI Azure-Abonnement](xplat-cli-connect.md) erstellen und Verwalten von Azure-Ressourcen.

* Weitere Informationen über die CLI Azure Quellcode, Probleme, herunterladen oder das Projekt, besuchen Sie [GitHub Repository für Azure-CLI](https://github.com/azure/azure-xplat-cli).

* Haben Sie Fragen zur Verwendung der Azure-CLI oder Azure finden Sie [Azure-Foren](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).

* Wenn Sie möchten, können Sie die Python-basierten [Azure CLI 2.0 Vorschau](https://github.com/azure/azure-cli)versuchen.

[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: virtual-machines-command-line-tools.md
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
