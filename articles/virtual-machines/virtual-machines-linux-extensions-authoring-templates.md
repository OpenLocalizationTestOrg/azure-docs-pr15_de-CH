<properties
   pageTitle="Erstellen von Vorlagen mit Linux VM | Microsoft Azure"
   description="Informationen Sie zum Erstellen von Vorlagen mit der Azure-Ressourcen-Manager für Linux VMs"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Erstellen von Vorlagen mit Linux VM Azure-Ressourcen-Manager

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

Führen Sie in Azure CLI auf folgenden Verbindungsbefehl:

      Azure VM extension list

Dieser Befehl gibt Herausgebernamen, Namen und Version wie folgt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Diese drei Eigenschaften "Publisher", "Typ" und "TypeHandlerVersion" in den oben stehenden Ausschnitt Vorlage zuordnen.

>[AZURE.NOTE]Empfohlen hat immer um die neueste Erweiterung der neuesten Funktionen zu verwenden.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Das Schema für die Erweiterung Konfigurationsparameter identifizieren

Nächste Schritt mit der Erstellung einer Vorlage Erweiterung ist das Format für die Bereitstellung von Konfigurationsparametern identifizieren. Jede Erweiterung unterstützt einen eigenen Satz von Parametern.

Beispielkonfigurationen für Linux Extensions zu betrachten, klicken Sie auf die Dokumentation finden Sie unter [Linux eExtensions Beispiele](virtual-machines-linux-extensions-configuration-samples.md).

Folgende vollständig Vorlage mit VM-Erweiterungen zu finden.

[Benutzerdefiniertes Skript-Erweiterung für Linux VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Nach dem Erstellen der Vorlage können Sie mithilfe der Azure-CLI bereitstellen.
