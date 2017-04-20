<properties 
   pageTitle="Erstellen eine Handelspartnervereinbarung in Azure App Service | Microsoft Azure" 
   description="Erstellen Sie Partner Handelsabkommen" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
    ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="08/23/2016"
   ms.author="rajram"/>

# <a name="creating-a-trading-partner-agreement"></a>Erstellen einer Handelspartnervereinbarung   

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Handelspartner sind die Beteiligten B2B (Business-to-Business) Kommunikation. Wenn zwei Partner eine Beziehung wird dies als *Vereinbarung*bezeichnet. Der Vertrag definiert basiert auf Kommunikation die beiden Partner möchte erreichen und Protokoll oder eine bestimmte Übertragung ist. Die verschiedenen Protokolle B2B und Transporte von Azure App Service unterstützt gehören:

- AS2 (Anwendbarkeit Anweisung 2)
- EDIFACT (un/elektronischen Datenaustausch für Verwaltung, Handel und Verkehr (UN/EDIFACT))
- X12 (AUF X12)

### <a name="biztalk-api-apps-that-support-b2b-scenarios"></a>BizTalk-API-Apps, die B2B-Szenarien unterstützen
Die folgenden API-Apps ermöglichen diese Funktionen über eine umfassende und intuitive Oberfläche im Azure-Portal:


## <a name="biztalk-trading-partner-management-tpm"></a>BizTalk Trading Partner-Management (TPM)
- Erstellung und Verwaltung von Partnern, Profile und Identitäten
- Speicherung und Verwaltung von EDI-Schemata
- Speicherung und Verwaltung von Zertifikaten (AS2-Protokoll)
- Erstellung und Verwaltung von AS2-Agreements
- Erstellung und Verwaltung von EDIFACT-Verträge (einschließlich auf der Batchverarbeitung)
- Erstellung und Verwaltung von X12 Agreements (einschließlich auf der Batchverarbeitung)

![][1]


## <a name="as2-connector"></a>AS2-Connector
- AS2-Abkommen führt im zugehörigen TPM API-App-Instanz
- Flächen AS2-Verarbeitung/Informationen zur Fehlerbehebung


## <a name="biztalk-edifact"></a>BizTalk EDIFACT
- Führt EDIFACT Abkommen im zugehörigen TPM API-App-Instanz
- Flächen EDIFACT Verarbeitung/Informationen zur Fehlerbehebung
- In EDIFACT Verträge in verwandten TPM API-App-Instanz definiert bietet Verwaltung Batches (Start und Stop)


## <a name="biztalk-x12"></a>BizTalk-X12
- Führt X12 Abkommen im zugehörigen TPM API-App-Instanz 
- Flächen X12 Informationen zur Verarbeitung/Überwachung
- Verwaltung von Batches (Start und Stop) im X12 bietet Verträge in der zugehörigen TPM API-App-Instanz

Wie bereits erwähnt AS2 X 12 und EDIFACT API-Apps benötigen ein TPM API App funktionieren wie erwartet.


## <a name="getting-started"></a>Erste Schritte
Erstellung von Handelsabkommen partner

1. Erstellen Sie eine Instanz des Connectors **BizTalk Trading Partner-Management** . Hierfür wird eine leere SQL-Datenbank Funktion. Vor dem Starten Sie eine leere Datenbank verfügbar und betriebsbereit sein.
2. Hochladen Sie Schemas und Zertifikate gemäß dem Abkommen. Zu diesem Zweck die TPM-Instanz erstellt und die in dem Abschnitt "Schemas" bzw. "" Durchsuchen
3. Suchen Sie die TPM-Instanz erstellt und in der **Partner** Teil
4. Erstellen Sie Partner nach Bedarf. Außerdem bearbeiten Sie Profile nach Bedarf und die erforderlichen Identitäten
5. Jetzt mithilfe des **Abkommen** Teils erstellen. Beim Erstellen einer Vereinbarung müssen Sie das Protokoll auswählen, das verwendet werden soll. Die übrigen Konfigurationsoptionen basieren auf das Protokoll, die Sie ausgewählt.

![][2]

![][3]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-trading-partner-agreement/TPMResourceView.png
[2]: ./media/app-service-logic-create-a-trading-partner-agreement/ProtocolSelection.png
[3]: ./media/app-service-logic-create-a-trading-partner-agreement/X12AgreementCreation.png
 
