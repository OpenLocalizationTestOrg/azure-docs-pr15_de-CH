<properties 
    pageTitle="Übersicht über XML-Validierung in Enterprise-Integrationspaket | Microsoft Azure App Service | Microsoft Azure" 
    description="Validierung in Enterprise-Integrationspaket und Logik apps Funktionsweise" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-validation"></a>Enterprise-Integration mit XML-Validierung

## <a name="overview"></a>Übersicht
Häufig müssen in B2B-Szenarien Partner eine Vereinbarung überprüfen, die untereinander ausgetauschten Nachrichten gültig sind, bevor die Verarbeitung der Daten beginnen kann. XML-Validierung Connector können Sie in Enterprise-Integrationspaket Dokumente mit einem vordefinierten Schema validiert.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Wie ein Dokument mit der XML-Validierung Connector überprüft
1. Erstellen Sie eine Logik-app und [Verknüpfen Sie es mit Ihrem Konto Integration](./app-service-logic-enterprise-integration-accounts.md "lernen einer Firma Integration Logik app verknüpfen") , die das Schema enthält, mit die XML-Daten überprüfen.
2. Einen **Anforderung - Wenn eine HTTP-Anforderung empfangen** Trigger für Ihre Anwendung Logik hinzufügen  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Hinzufügen der **XML-Validierung** Aktion erste Auswahl **eine Aktion hinzufügen**  
4. Geben Sie *Xml* in das Suchfeld ein, um alle Aktionen zu den Filtern, die Sie verwenden möchten 
5. **XML-Überprüfung** aktivieren     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Wählen Sie das Textfeld **CONTENT**  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Wählen Sie das Body-Tag als Inhalt überprüft werden.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Wählen Sie die **SCHEMANAMEN** Listenfeld und wählen Sie das Schema der eingegebenen *Inhalt* oben überprüfen möchten     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Speichern Sie Ihre Arbeit  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

Zu diesem Zeitpunkt sind Sie fertig Ihre Überprüfung Connector einrichten. In einer realen Anwendung sollten Sie validierten Daten in einer LOB-Anwendung wie SalesForce speichern. Sie können problemlos eine Aktion um die Ausgabe der Überprüfung Salesforce hinzufügen. 

Sie können die Validierung Aktion jetzt durch eine Anforderung an den HTTP-Endpunkt testen.  

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket")   