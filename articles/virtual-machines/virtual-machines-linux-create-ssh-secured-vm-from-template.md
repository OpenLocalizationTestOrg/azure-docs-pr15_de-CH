<properties
    pageTitle="Erstellen einer Linux VM mit einer Azure Vorlage | Microsoft Azure"
    description="Erstellen Sie Linux VM Azure mit einer Azure-Ressourcen-Manager-Vorlage."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-service-management,azure-resource-manager" />

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/24/2016"
    ms.author="v-livech"/>

# <a name="create-a-linux-vm-using-an-azure-template"></a>Erstellen einer Linux VM mit einer Azure Vorlage

Dieser Artikel beschreibt, wie schnell bereitstellen einer virtuellen Linux-Maschine auf Azure mit einer Azure-Vorlage.  Der Artikel sind erforderlich:

- ein Azure-Konto ([kostenlos testen](https://azure.microsoft.com/pricing/free-trial/)).

- angemeldet bei [Azure CLI](../xplat-cli-install.md) `azure login`.

- Azure-CLI _muss_ Azure-Ressourcen-Manager-Modus `azure config mode arm`.

Sie können auch Linux VM-Vorlage mithilfe der [Azure-Portal](virtual-machines-linux-quick-create-portal.md)bereitstellen.

## <a name="quick-command-summary"></a>Schnelle Befehl Zusammenfassung

```bash
azure group create \
-n myResourceGroup \
-l westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Ausführliche Anleitung

Vorlagen können Sie die VMs auf Azure mit Einstellungen, die Sie beim Start anpassen möchten, wie Benutzernamen und Hostnamen erstellen. In diesem Artikel starten wir eine Azure Vorlage mit einem Ubuntu VM mit einer Netzwerk-Sicherheitsgruppe (NSG) Anschluss 22 öffnen SSH.

Azure Ressourcenmanager Vorlagen sind JSON-Dateien für einfache einmalige Aufgaben wie eine Ubuntu VM starten, wie in diesem Artikel.  Azure Vorlagen können auch zum Erstellen komplexer Azure Konfigurationen der gesamten Umgebung wie einen Test, Entwicklung oder Produktion Bereitstellung verwendet werden.

## <a name="create-the-linux-vm"></a>Linux VM erstellen

Im folgenden Codebeispiel wird veranschaulicht, wie `azure group create` eine Ressourcengruppe erstellen und Bereitstellen einer SSH-gesicherte Linux VM gleichzeitig mit [dieser Vorlage Azure-Ressourcen-Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Beachten Sie, dass in Ihrem Beispiel benötigen Sie Namen verwenden, die in Ihrer Umgebung eindeutig sind. Dieses Beispiel verwendet `myResourceGroup` als Gruppennamen Ressource und `myVM` als der Name.

```bash
azure group create \
--name myResourceGroup \
--location westus \
--template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

Die Ausgabe sieht wie die folgende Ausgabe:

```bash
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Diesem Beispiel bereitgestellte VM mit der `--template-uri` Parameter.  Sie können herunterladen oder lokal eine Vorlage auch übergeben der Vorlage mit den `--template-file` Parameter mit einem Pfad zu der Vorlagendatei als Argument. Azure-CLI werden Sie aufgefordert, die Vorlage erforderlichen Parameter.

## <a name="next-steps"></a>Nächste Schritte

Suchen Sie [Vorlagensammlung](https://azure.microsoft.com/documentation/templates/) ermitteln welche Anw.-Frameworks weiter bereitstellen.
