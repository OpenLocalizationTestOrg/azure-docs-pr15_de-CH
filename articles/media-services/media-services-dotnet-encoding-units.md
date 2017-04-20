<properties 
    pageTitle="Codierung Einheiten hinzufügen" 
    description="Informationen zu Einheiten mit Codierung hinzufügen"  
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/01/2016"
    ms.author="juliako;milangada;gtrifonov"/>


#<a name="how-to-scale-encoding-with-net-sdk"></a>Wie .NET SDK-Codierung

> [AZURE.SELECTOR]
- [Portal](media-services-portal-scale-media-processing.md )
- [.NET](media-services-dotnet-encoding-units.md)
- [REST](https://msdn.microsoft.com/library/azure/dn859236.aspx)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Übersicht

>[AZURE.IMPORTANT] Überprüfen Sie [Thema Weitere Informationen zum Thema Verarbeitung Media skalieren](media-services-scale-media-processing-overview.md) .
 
Um reservierte Einheitentyp und die Anzahl der reservierte Einheiten mit .NET SDK Codierung ändern, führen Sie folgende Schritte aus:

    IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
    encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
    encodingS1ReservedUnit.Update();
    Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);
    
    encodingS1ReservedUnit.CurrentReservedUnits = 2;
    encodingS1ReservedUnit.Update();
    
    Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);

##<a name="opening-a-support-ticket"></a>Support-Ticket öffnen

Standardmäßig kann jedes Media Services-Konto um bis zu 25 Codierung und 5 bei Bedarf Streaming reservierte skaliert werden. Sie können einen höheren Grenzwert Öffnen einer Support-Ticket anfordern.

###<a name="open-a-support-ticket"></a>Support-Ticket öffnen

Zum Öffnen eines Support Ticket folgendermaßen vor:

1. Klicken Sie auf [Support](https://manage.windowsazure.com/?getsupport=true). Wenn Sie nicht angemeldet sind, werden Sie aufgefordert, Ihre Anmeldeinformationen einzugeben.

1. Wählen Sie Ihr Abonnement.

1. Wählen Sie unter Supporttyp "Technisch".

1. Klicken Sie auf "Karte erstellen".

1. Wählen Sie "Azure Media Services" in der Liste auf der nächsten Seite dargestellt.

1. Wählen Sie einen "Problem" geeignet für Ihr Problem.

1. Klicken Sie auf Weiter.

1. Anweisungen auf Seite und geben Sie Details zu Ihrem Problem.

1. Das Ticket öffnen "Absenden" klicken.



##<a name="media-services-learning-paths"></a>Media Services Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Geben Sie feedback

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
