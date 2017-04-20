<properties
   pageTitle="Bereitstellen eines Clusters Azure Container Service | Microsoft Azure"
   description="Bereitstellen eines Clusters Azure Container Service mithilfe der Azure-Portal, Azure-CLI oder PowerShell."
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
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/13/2016"
   ms.author="rogardle"/>

# <a name="deploy-an-azure-container-service-cluster"></a>Bereitstellen eines Clusters Azure Container Service

Azure Container Service bietet schnelle Bereitstellung von gängigen Open Source-Container clustering und Orchestrierung Solutions. Mithilfe von Azure Container Service können Sie DC/OS und Andockfenster Schwarm Cluster Azure Ressourcenmanager Vorlagen oder Azure-Portal bereitstellen. Bereitstellen dieser Cluster mithilfe von Azure Virtual Machine skalieren und Cluster Azure Netzwerk- und Angebote nutzen. Für den Zugriff auf Azure Container Service benötigen Sie ein Azure-Abonnement. Wenn Sie haben, können Sie für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935)von signieren.

Dieses Dokument führt Sie durch Bereitstellen eines Clusters Azure Container Service mithilfe der [Azure-Portal](#creating-a-service-using-the-azure-portal), [Azure Befehlszeilenschnittstelle (CLI)](#creating-a-service-using-the-azure-cli)und [Azure PowerShell-Modul](#creating-a-service-using-powershell).  

## <a name="create-a-service-by-using-the-azure-portal"></a>Erstellen Sie einen Dienst mithilfe des Azure-Portals

Azure-Portal melden Sie an, wählen Sie **neu**und suchen Sie **Azure Container Service**Azure Marketplace.

![Bereitstellung 1 erstellen](media/acs-portal1.png)  <br />

Wählen Sie **Azure Container Service**, und klicken Sie auf **Erstellen**.

![Bereitstellung 2 erstellen](media/acs-portal2.png)  <br />

Geben Sie Folgendes ein:

- **Benutzername**: Dies ist der Benutzername für ein Konto auf jedem virtuellen Computer verwendet werden und virtuellen Skalierung in Azure Container Service Cluster festgelegt.
- **Abonnement**: Wählen Sie ein Azure-Abonnement.
- **Gruppe**: Wählen Sie eine vorhandene Ressourcengruppe oder eine neue erstellen.
- **Ort**: Wählen Sie eine Region Azure Azure Container Service-Bereitstellung.
- **Öffentliche SSH-Schlüssel**: Fügen Sie den öffentlichen Schlüssel für die Authentifizierung gegen Azure Container Service virtuelle Computer verwendet werden. Es ist sehr wichtig, dass dieser Schlüssel ohne Zeilenumbrüche und das Präfix "ssh-Rsa" enthält und die 'username@domain' postfix. Es sollte etwa wie folgt aussehen: **ssh Rsa AAAAB3Nz... <>...... UcyupgH azureuser@linuxvm **. Erstellen von Secure Shell (SSH) Schlüsseln Siehe Artikel [Linux]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) und [Windows]( https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) .

Wenn Sie fortfahren möchten, klicken Sie auf **OK** .

![Bereitstellung 3 erstellen](media/acs-portal3.png)  <br />

Wählen Sie eine Orchestrierung. Die Optionen sind:

- **DC-OS**: DC-OS-Cluster bereitgestellt.
- **Schwarm**: Andockfenster Schwarm Cluster bereitgestellt.

Wenn Sie fortfahren möchten, klicken Sie auf **OK** .

![Deployment 4 Erstellen](media/acs-portal4.png)  <br />

Geben Sie Folgendes ein:

- **Master zählen**: die Anzahl der Master im Cluster.
- **Anzahl an Agenten**: für Andockfenster Schwarm werden die anfängliche Anzahl von Agents in den Agent skalieren. DC/OS werden diese anfängliche Anzahl von Agents in einer privat. Außerdem wird eine Reihe öffentlicher Skalierung erstellt, enthält eine vorgegebene Anzahl von Agents. Die Anzahl der Agents in dieser öffentlichen Skala wird bestimmt wie viele Meister im Cluster – eine öffentliche Mittel für einen Master und zwei öffentliche Agents für drei oder fünf Vorlagen erstellt wurden.
- **Größe des virtuellen Computers Agent**: die Größe der Agent virtueller Maschinen.
- **DNS-Präfix**: World eindeutige Namen, die Hauptbestandteile der vollqualifizierten Domänennamen für den Dienst als Präfix.

Wenn Sie fortfahren möchten, klicken Sie auf **OK** .

![Bereitstellung 5 erstellen](media/acs-portal5.png)  <br />

Service-Überprüfung abgeschlossen ist, klicken Sie auf **OK** .

![Bereitstellung 6 erstellen](media/acs-portal6.png)  <br />

Klicken Sie auf **Erstellen** , um den Bereitstellungsprozess zu starten.

![Bereitstellung 7 erstellen](media/acs-portal7.png)  <br />

Wenn Sie die Bereitstellung der Azure-Portal pin gewählt haben, sehen Sie den Bereitstellungsstatus.

![Bereitstellung 8 erstellen](media/acs-portal8.png)  <br />

Wenn die Bereitstellung abgeschlossen ist, kann Azure Container Service-Cluster verwendet.

## <a name="create-a-service-by-using-the-azure-cli"></a>Erstellen Sie einen Dienst mithilfe der Azure-CLI

Um eine Instanz von Azure Container Service mithilfe der Befehlszeile erstellen, benötigen Sie ein Azure-Abonnement. Wenn Sie haben, können Sie für eine [kostenlose Testversion](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935)von signieren. Sie möchten die Azure-CLI [installiert](../xplat-cli-install.md) und [konfiguriert](../xplat-cli-connect.md) haben.

Um einen DC-OS oder Andockfenster Schwarm Cluster bereitstellen, wählen Sie eine der folgenden Vorlagen GitHub. Beachten Sie, dass beide Vorlagen identisch, mit Ausnahme der Standardauswahl Orchestrator.

* [DC-OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Schwarm Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Anschließend stellen Sie sicher, dass der Azure-CLI Azure-Abonnement verbunden wurde. Dazu können Sie den folgenden Befehl verwenden:

```bash
azure account show
```
Wenn ein Azure-Konto nicht zurückgegeben wird, verwenden Sie folgenden Befehl CLI Azure anmelden.

```bash
azure login -u user@domain.com
```

Konfigurieren Sie anschließend die CLI Azure Tools Azure verwendet.

```bash
azure config mode arm
```

Ein Azure-Ressourcengruppe und Container Service-Cluster mit dem folgenden Befehl erstellen, in denen:

- **RESOURCE_GROUP** ist der Name der Ressourcengruppe, die Sie für diesen Dienst verwenden möchten.
- **Speicherort** ist der Azure-Region der Ressourcengruppe und Azure Container Service-Bereitstellung Erstellungsort.
- **TEMPLATE_URI** ist der Speicherort der Datei mit dem. Beachten Sie, dass dies die Raw-Datei keinen Zeiger auf GitHub UI. Um diese URL zu suchen, wählen Sie die Datei azuredeploy.json in GitHub, und klicken Sie auf **Raw** .

> [AZURE.NOTE] Wenn Sie diesen Befehl ausführen, wird die Shell Bereitstellung Parameterwerte aufgefordert.

```bash
azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Vorlagenparameter angeben

Diese Version des Befehls müssen Sie Parameter interaktiv definieren. Wenn Sie Parameter wie JSON-formatierte Zeichenfolge bereitstellen möchten Sie können dazu die `-p` wechseln. Zum Beispiel:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Alternativ können Sie eine JSON-formatierten Datei mithilfe der `-e` wechseln:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

Sehen Sie eine Beispiel-Parameter-Datei `azuredeploy.parameters.json`, mit den Vorlagen Azure Container Service GitHub suchen.

## <a name="create-a-service-by-using-powershell"></a>Erstellen Sie einen Dienst mithilfe von PowerShell

Sie können auch eine Azure Container Service Cluster mit PowerShell bereitstellen. Dieses Dokument basiert auf der Version 1.0 [Azure PowerShell-Modul](https://azure.microsoft.com/blog/azps-1-0/).

Um einen DC-OS oder Andockfenster Schwarm Cluster bereitstellen, wählen Sie eine der folgenden Vorlagen. Beachten Sie, dass beide Vorlagen identisch, mit Ausnahme der Standardauswahl Orchestrator.

* [DC-OS-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
* [Schwarm Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)

Vor dem Erstellen ein Clusters in der Azure-Abonnement überprüfen, ob die PowerShell-Sitzung in Azure angemeldet wurde. Sie erreichen dies mit der `Get-AzureRMSubscription` Befehl:

```powershell
Get-AzureRmSubscription
```

Benötigen Sie Azure anmelden, die `Login-AzureRMAccount` Befehl:

```powershell
Login-AzureRmAccount
```

Wenn Sie eine neue Ressourcengruppe bereitstellen, müssen Sie die Ressourcengruppe erstellen. Erstellen Sie eine neue Ressourcengruppe verwenden die `New-AzureRmResourceGroup` Befehl und geben Sie einen Namen und einen Zielordner Bereich Ressource Gruppe:

```powershell
New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
```

Nachdem Sie eine Ressourcengruppe erstellen, können Sie mit dem folgenden Befehl Cluster erstellen. Angegebene URI der gewünschten Vorlage für das `-TemplateUri` Parameter. Wenn Sie diesen Befehl ausführen, wird PowerShell Bereitstellung Parameterwerte aufgefordert.

```powershell
New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
```

### <a name="provide-template-parameters"></a>Vorlagenparameter angeben

Wenn Sie mit PowerShell vertraut sind, wissen Sie, dass Sie die verfügbaren Parameter für ein Cmdlet zu durchlaufen können durch ein Minuszeichen (-) eingeben und die TAB-Taste drücken. Diese Funktion funktioniert auch mit Parametern, die in der Vorlage definieren. Als Namen der Vorlage eingeben, das Cmdlet Ruft die Vorlage Parameter analysiert und dynamisch den Befehl Vorlageparameter hinzugefügt. Dies erleichtert die Vorlage Parameterwerte festlegen. Und sollten ein erforderlichen Parameterwerts PowerShell aufgefordert, den Wert.

Folgt der vollständige Befehl mit Parametern enthalten. Sie können eigene Werte für die Namen der Ressourcen bereitstellen.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie einen funktionierenden Cluster diese Dokumente für die Verbindung und Management-Details finden Sie unter:

- [Verbinden Sie mit einem Cluster Azure Container Service](container-service-connect.md)
- [Arbeiten Sie mit Azure Containerservice und DC-OS](container-service-mesos-marathon-rest.md)
- [Arbeiten Sie mit Azure Containerservice und Andockfenster Schwarm](container-service-docker-swarm.md)
