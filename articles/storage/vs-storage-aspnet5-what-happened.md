<properties
    pageTitle="Wo Mein Projekt ASP.NET 5 (Visual Studio verbunden Services) | Microsoft Azure-Speicher"
    description="Beschreibt, was geschieht, nachdem Dienste mit einer Azure Speicherkonto in einem Visual Studio ASP.NET 5-Projekt mit Visual Studio verbunden werden."
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

# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Wo Mein Projekt ASP.NET 5 (Visual Studio Azure Storage verbunden Services)?

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

Außerdem wurde das NuGet-Paket **Microsoft.Framework.Configuration.Json** hinzugefügt.

## <a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure Storage hinzugefügt
In der Datei config.json des Projekts wurde ein Element mit des ausgewählten Speicherkontos Verbindungszeichenfolge Schlüssel erstellt.

Weitere Informationen finden Sie unter [ASP.NET 5](http://www.asp.net/vnext).
