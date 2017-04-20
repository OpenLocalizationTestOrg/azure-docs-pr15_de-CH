<properties
    pageTitle="Wo Mein Projekt Webauftrag (Visual Studio Azure Storage verbunden Service)? | Microsoft Azure"
    description="Beschreibt in Azure Webauftrags Projekt nach Services mit ein Speicherkonto mit Visual Studio verbunden werden."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Wo Mein Projekt Webauftrag (Visual Studio Azure Storage verbunden Service)?

## <a name="references-added"></a>Verweise hinzugefügt

Azure Storage NuGet-Paket wurde hinzugefügt oder im Visual Studio-Projekt aktualisiert.  
Dieses Paket fügt die folgenden auf:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.ConfigurationManager**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure Storage hinzugefügt
Die Einträge **AzureWebJobsStorage** und **AzureWebJobsDashboard** wurden in der Datei App.config des Projekts mit der ausgewählten Speicherkonto Verbindungszeichenfolge und Schlüssel aktualisiert.

Weitere Informationen finden Sie unter [Azure Webaufträge Dokumentationsressourcen](http://go.microsoft.com/fwlink/?linkid=390226).
