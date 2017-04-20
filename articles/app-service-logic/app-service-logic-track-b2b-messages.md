<properties 
   pageTitle="B2B-Nachrichten in Ihren apps Logik in Azure App Service überwachen | Microsoft Azure" 
   description="Dieses Thema behandelt die Verfolgung von B2B-Verarbeitung" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="track-b2b-messages"></a>B2B-Nachrichten verfolgen

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

## <a name="b2b-tracking-information"></a>B2B-Tracking-Informationen
B2B-Kommunikation umfasst Nachrichten zwischen Handelspartnern. Die Vertrauensstellungen werden zwischen zwei Handelspartnern definiert. Sobald die Verbindung hergestellt wurde, muss es eine Möglichkeit zur Überwachung die Kommunikation geschieht, wie erwartet. 

Wir implementierten Nachrichtentracking für die folgenden Szenarien B2B: AS2, EDIFACT und X12.

## <a name="as2"></a>AS2
Nach dem Erstellen einer Instanz des AS2-API zur Instanz durchsuchen Sie und wählen Sie **verfolgen aus**. Hier können Sie anzeigen und Filtern die AS2-Verfolgungsinformationen:  

![][1]  

## <a name="edifact"></a>EDIFACT
Nach dem Erstellen einer Instanz des EDIFACT-API zur Instanz durchsuchen Sie und wählen Sie **verfolgen aus**. Hier können Sie anzeigen und Filtern alle Überwachungsinformationen EDIFACT. Darüber hinaus können Austausch Ebene, Gruppenebene und Transaktion Daten in einer einzelnen Ansicht festgelegt. 

Wenn Stapel EDIFACT Abkommen in der zugehörigen Trading Partner Management API-Anwendung erstellt werden, listet Abschnitt Batching diese Batches. Wählen Sie einen Stapel an die aktive Nachricht (falls vorhanden) und die Informationen für die:  

![][2]      

## <a name="x12"></a>X12
Nach dem Erstellen einer Instanz einer X12 API-App Instanz suchen und auswählen, **verfolgen**. Hier können Sie anzeigen und Filtern alle X12 Informationen. Darüber hinaus können Austausch Ebene, Gruppenebene und Transaktion Daten in einer einzelnen Ansicht festgelegt.

Wenn Stapel erstellt werden als Teil des X12 Abkommen in der zugehörigen Trading Partner Management API-Anwendung und Abschnitt Batching Listet alle Batches. Sie können einen Stapel an die aktive Nachricht (falls vorhanden) sowie die Informationen für die abgeschlossene Stapel auswählen.

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.png
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.png
