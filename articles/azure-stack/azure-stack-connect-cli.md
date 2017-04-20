<properties
    pageTitle="Verbinden mit Azure Stapel mit CLI | Microsoft Azure"
    description="Enthält Informationen zum Verwenden von plattformübergreifenden Befehlszeilenschnittstelle (CLI) verwalten und Bereitstellen von Ressourcen Azure Stapel"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Installieren und Konfigurieren von Azure Stapel CLI

In diesem Dokument führen wir Sie durch den Prozess der Azure-Stack auf Linux und Mac Clientplattformen Ressourcen mit Azure-Befehlszeilenschnittstelle (CLI).  

## <a name="install-azure-stack-cli"></a>Azure Stapel CLI installieren

Wenn Sie Mac oder Linux verwenden, erhalten Sie die CLI, mithilfe des folgenden Befehls:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Verbinden mit Azure Stapel
In den folgenden Schritten konfigurieren Sie Azure CLI Azure Stack herstellen. Dann anmelden und Abonnementinformationen abzurufen.

1.  Rufen Sie den Wert für Active-Directory-Ressource-Id durch diese PowerShell ausführen:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  Verwenden Sie folgenden CLI-Befehl hinzufügen die Azure-Stapel Umgebung aktualisieren *-Active-Directory-Ressource-Id* mit Daten-URL im vorherigen Schritt abgerufen:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Melden Sie sich mit den folgenden Befehl ein (ersetzen *Benutzernamen* mit Ihren Benutzernamen):

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Wenn Sie Überprüfungsprobleme Zertifikat erhalten, deaktivieren Sie Zertifikat durch Ausführen des Befehls `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Legen Sie Azure Konfigurationsmodus, Azure-Ressourcen-Manager mit dem folgenden Befehl:

        azure config mode arm

5.  Abrufen einer Liste der Abonnements.

        azure account list     

## <a name="next-steps"></a>Nächste Schritte

[Bereitstellen von Vorlagen mit Azure-CLI](azure-stack-deploy-template-command-line.md)

[Verbinden mit PowerShell](azure-stack-connect-powershell.md)

[Benutzerberechtigungen verwalten](azure-stack-manage-permissions.md)
