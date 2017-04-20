<properties
    pageTitle="Wo ist ASP.NET Projekt? | Microsoft Azure | Visual Studio verbunden services"
    description="Beschreibt, was geschieht, nachdem mit Visual Studio ein ASP.NET Projekt hinzufügen Azure Storage Services verbunden"
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

# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Wo Mein Projekt ASP.NET (Visual Studio Azure Storage verbunden Service)?

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

##<a name="connection-string-for-azure-storage-added"></a>Verbindungszeichenfolge für Azure Storage hinzugefügt
In der Datei web.config des Projekts wurde ein Element mit des ausgewählten Speicherkontos Verbindungszeichenfolge Schlüssel erstellt.

Weitere Informationen finden Sie unter [ASP.NET](http://www.asp.net).
