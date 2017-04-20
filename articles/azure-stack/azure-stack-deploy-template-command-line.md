<properties
    pageTitle="Bereitstellen von Vorlagen mit der Befehlszeile in Azure Stapel | Microsoft Azure"
    description="Erfahren Sie, wie die plattformübergreifende Befehlszeilenschnittstelle (CLI) verwenden, um Vorlagen in der ClientVM oder mit VPN Verbindung zu Azure Stapel bereitzustellen."
    services="azure-stack"
    documentationCenter=""
    authors="heathl17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="helaw"/>

# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Bereitstellen von Vorlagen in Azure Stapel mithilfe der Befehlszeile

Verwenden der Befehlszeile Azure Resource Manager Vorlagen POC Stapel Azure bereitstellen. Azure Ressourcenmanager Vorlagen Bereitstellung und alle Ressourcen für die Anwendung in einer einzigen koordinierten Operation.

## <a name="download-template"></a>Vorlage herunterladen        
Zum Testen einer Bereitstellung mit der CLI Herunterladen der Dateien azuredeploy.json und azuredeploy.parameters.json [Speicher Beispielvorlage erstellen](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account).

## <a name="deploy-template"></a>Vorlage bereitstellen
Navigieren Sie zu dem Ordner, in dem diese Dateien heruntergeladen und führen den folgenden Befehl zum Bereitstellen der Vorlage wurden:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Dieser Befehl stellt die Vorlage auf Ressource Gruppe **CliRG** Azure Stapel POC-Standardspeicherort.

## <a name="validate-template-deployment"></a>Überprüfen der Bereitstellung
Um dieser Gruppe und Speicher Ressourcenkonto anzuzeigen, verwenden Sie die folgenden Befehle:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Nächste Schritte

[Benutzerberechtigungen verwalten](azure-stack-manage-permissions.md)
