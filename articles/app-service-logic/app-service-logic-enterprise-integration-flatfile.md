<properties
    pageTitle="Informationen zum Ver- oder entschlüsseln Flatfiles mit Enterprise-Integrationspaket und Logik apps | Microsoft Azure App Service | Microsoft Azure"
    description="Funktionen von Enterprise-Integrationspaket und Logik apps mit der Ver- oder Entschlüsseln von Dateien"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Unternehmensintegration von Flatfiles

## <a name="overview"></a>Übersicht

Möglicherweise möchten XML-Inhalt vor dem Senden an einen Geschäftspartner in einem Szenario mit Business to Business (B2B) codiert. Flatfile-Codierung Connector können Sie in einer Logik app Logik Apps Feature von Azure App Service erstellt dazu. Die Logik-app erstellen erhalten die XML Content aus einer Vielzahl von Quellen aus einer HTTP-Anforderung Trigger aus einer anderen Anwendung oder sogar von einem viele [Connectors](../connectors/apis-list.md). Weitere Informationen über Logik apps [Logik apps-Dokumentation](./app-service-logic-what-are-logic-apps.md "erfahren Sie mehr über Logik apps")anzeigen  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Codierung Connector Flatfile erstellen

Gehen Sie eine Flatfile Codierung Connector für Ihre Anwendung Logik hinzufügen.

1. Erstellen eines Logik-app und [Verknüpfen Sie es mit Ihrem Konto Integration](./app-service-logic-enterprise-integration-accounts.md "lernen einer Firma Integration Logik Anwendung verknüpfen"). Dieses Konto enthält das Schema verwendet die XML-Daten codiert.  
2. **Anforderung - Wenn eine HTTP-Anforderung empfangen** Trigger für Ihre Anwendung Logik hinzufügen  
![Screenshot der Trigger auswählen](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Fügen Sie Flatfile Codierung Aktion wie folgt hinzu:

    ein. Wählen Sie **die Pluszeichen** .

    b. Wählen Sie **eine Aktion hinzufügen** Link (nach der Auswahl des Pluszeichen angezeigt).

    c. Geben Sie im Suchfeld *flache* alle Aktionen zu den Filtern, die Sie verwenden möchten.

    d. Die Option **Flat Codierung** aus.   
![Screenshot der flachen Codierung option](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. Kontrollkästchen Sie im Dialogfeld **Codierung flache** **Inhalt** Text.  
![Screenshot von Inhalten Textfeld](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Wählen Sie das Body-Tag als Inhalt zu codieren. Das Body-Tag wird Inhalt gefüllt.     
![Screenshot des Body-tag](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Wählen Sie im Listenfeld **Name des Schemas** , und Schemas verwenden, um die Eingabe der Inhalte codieren möchten.    
![Screenshot des Schemas im Listenfeld](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Speichern Sie Ihre Arbeit.   
![Screenshot der speichern-Symbol](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

Zu diesem Zeitpunkt sind Sie fertig die Codierung Flatfile-Verbindung einrichten. In einer realen Anwendung sollten Sie die codierten Daten in einer Line-of-Business-Anwendung wie Salesforce speichern. Oder, partner-codierte Daten, aus einem Handelspartner senden. Sie können problemlos eine Aktion um die Ausgabe der Codierung Aktion Salesforce oder Ihrem Handelspartner mithilfe einer bereitgestellten Connectors hinzufügen.

Sie können jetzt die Verbindung testen, die eine Anforderung an den HTTP-Endpunkt und den XML-Inhalt in den Hauptteil der Anforderung.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Decodieren von Connector Flatfile erstellen

>[AZURE.NOTE] Um diese Schritte auszuführen, müssen Sie eine Schemadatei bereits in Integration Konto hochgeladen haben.

1. **Anforderung - Wenn eine HTTP-Anforderung empfangen** Trigger für Ihre Anwendung Logik hinzufügen  
![Screenshot der Trigger auswählen](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Fügen Sie Flatfile Decodierung Aktion wie folgt hinzu:

    ein. Wählen Sie **die Pluszeichen** .

    b. Wählen Sie **eine Aktion hinzufügen** Link (nach der Auswahl des Pluszeichen angezeigt).

    c. Geben Sie im Suchfeld *flache* alle Aktionen zu den Filtern, die Sie verwenden möchten.

    d. Die Option **Flache Datei Decodierung** aus.   
![Screenshot der flachen Datei Decodierung option](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Wählen Sie **das Steuerelement** . Dies erzeugt eine Liste der früheren Schritte, mit denen Sie als Inhalt entschlüsseln. Beachten Sie, dass *Text* von eingehenden HTTP-Anforderung als Inhalt entschlüsseln verwendet werden. Sie können auch den Inhalt direkt in das Steuerelement **Inhalt** Entschlüsseln eingeben.     
- Wählen Sie das *Body* -Tag. Beachten Sie das Body-Tag ist nun **das Inhaltssteuerelement** .
- Wählen Sie den Namen des Schemas, das Decodieren des Inhalts verwenden möchten. Der folgende Screenshot zeigt, dass *OrderFile* Name des ausgewählten. Diese Schemanamen hatte zuvor berücksichtigt Integration hochgeladen.

 ![Screenshot der flachen Datei Decodierung Dialogfeld](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Speichern Sie Ihre Arbeit.  
![Screenshot der speichern-Symbol](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

Zu diesem Zeitpunkt sind Sie fertig der Flatfile-Decodierung Connector einrichten. In einer realen Anwendung sollten Sie die decodierten Daten in eine LOB Anwendung wie Salesforce speichern. Sie können problemlos eine Aktion um die Ausgabe der Decodierung Aktion Salesforce hinzufügen.

Sie können jetzt die Verbindung testen, indem er eine Anforderung an den HTTP-Endpunkt und den XML-Inhalt Sie im Hauptteil der Anforderung decodieren möchten.  

## <a name="next-steps"></a>Nächste Schritte
- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise Integration Packs").  
