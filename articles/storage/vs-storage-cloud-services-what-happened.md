<properties
    pageTitle="Was ist Cloud-Dienstprojekt? | Microsoft Azure | Visual Studio verbunden services"
    description="Beschreibt in einem Projekt Cloud Services nach Services Verbinden einer Azure Storage-Konto mithilfe von Visual Studio verbunden werden."
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

# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Was passiert mit meinem Cloud Services-Projekt (Visual Studio Azure Storage verbunden Service)?

## <a name="references-added"></a>Verweise hinzugefügt

Azure Storage NuGet-Paket wurde dem Visual Studio-Projekt hinzugefügt.  
Dieses Paket fügt die folgenden auf:

- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.WindowsAzure.Configuration**
- **Microsoft.WindowsAzure.Storage**
- **Newtonsoft.Json**
- **System.Data**
- **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure Storage hinzugefügt
Elemente wurden mit des ausgewählten Speicherkontos Verbindungszeichenfolge Schlüssel erstellt. Die folgenden Dateien wurden Änderungen vorgenommen:

- **ServiceDefinition.csdef**
- **ServiceConfiguration.Cloud.cscfg**
- **ServiceConfiguration.Local.cscfg**
