<properties 
    pageTitle="B2B-Lösungen mit Enterprise Integration | Microsoft Azure App Service | Microsoft Azure" 
    description="Informationen Sie zum Empfangen von Daten mithilfe von B2B-Funktionen des Enterprise-Integration" 
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

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Informationen Sie zum Empfangen von Daten mithilfe von B2B-Funktionen des Enterprise-Integration#

## <a name="overview"></a>Übersicht ##

Dieses Dokument ist Teil der Logik Apps Enterprise Integration Packs. Übersicht über die [Funktionen von Enterprise-Integrationspaket](./app-service-logic-enterprise-integration-overview.md)erfahren anzeigen

## <a name="prerequisites"></a>Erforderliche Komponenten ##

Die AS2 und X12 Aktionen benötigen Sie eine Enterprise-Integration-Konto

[Einem Konto Integration erstellen](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Verwendung der Logik Apps B2B connectors ##

Erstellt ein Konto Integration und Partner und Abkommen, Sie können eine Anwendung Logik erstellen implementiert, einen Unternehmen (B2B) Workflow.

In diesem Exemplarische Übersicht sehen Sie den AS2 und X12 mit Aktionen, die ein Unternehmen Logik-App erstellen, die Daten von einem Handelspartner empfängt.

1. Erstellen Sie eine neue Anwendung Logik und [Integration-Konto verknüpfen](./app-service-logic-enterprise-integration-accounts.md).  
2. Einen **Anforderung - Wenn eine HTTP-Anforderung empfangen** Trigger für Ihre Anwendung Logik hinzufügen  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Fügen Sie **Decodieren AS2** Tätigwerden erste Auswahl **eine Aktion hinzufügen**  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Geben Sie Word **as2** in das Suchfeld ein, um alle Aktionen zu den Filtern, die Sie verwenden möchten  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Wählen Sie die Aktion **AS2 - Decodierung AS2-Nachricht**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Wie gezeigt, **Text** , die Sie ergreifen hinzufügen als Eingabe. Wählen Sie in diesem Beispiel den Hauptteil der HTTP-Anforderung, die die Anwendung Logik ausgelöst. Sie können auch einen Ausdruck, um die Header in der**Header** -Feld eingeben eingeben:

    @triggerOutputs()['headers']

8. Fügen Sie die **Header** , die für AS2 hinzu. Diese werden im Header HTTP-Anforderung. Wählen Sie in diesem Beispiel die Header der HTTP-Anforderung, die die Anwendung Logik ausgelöst.
9. Jetzt fügen Sie die Decode X12 Aktion hinzu, indem Sie erneut **eine Aktion hinzufügen**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Geben Sie Word- **X12** in das Suchfeld ein, um alle Aktionen zu den Filtern, die Sie verwenden möchten  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Wählen Sie die **X12-X12 decodieren Nachricht** App Logik hinzugefügt  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Jetzt müssen die Eingabe dieser Aktion angeben, die die Ausgabe der durchgeführten AS2. Der Nachrichteninhalt wird ein JSON-Objekt und base64-codiert ist. Daher müssen Sie einen Ausdruck angeben, wie die Eingabe so geben Sie den Ausdruck in das Eingabefeld **X12 FLACHE Datei Nachricht zu decodieren**  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Dadurch wird die X12 decodieren, Daten vom Handelspartner und gibt eine Anzahl von Elementen in einem JSON-Objekt. Um den Partner kennen den Empfang der Daten lassen können Sie wieder eine Antwort mit der AS2 Nachricht Disposition Notification (MDN) in einem HTTP-Antwort senden.  
14. Fügen Sie **die Reaktion** durch Hinzufügen **einer Aktion hinzu**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Geben Sie Word **Antwort** in das Suchfeld ein, um alle Aktionen zu den Filtern, die Sie verwenden möchten  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Wählen Sie die Aktion **Antwort** hinzufügen  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Festlegen Sie **Nachrichtenfeld Antwort** mithilfe des folgenden Ausdrucks auf die MDN aus der Ausgabe der Aktion **Decode X12**  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Speichern Sie Ihre Arbeit  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

Zu diesem Zeitpunkt sind Sie fertig Logik B2B-app einrichten. In einer realen Anwendung sollten die decodierten X12 speichern Daten in einer LOB-Anwendung oder Daten. Sie können problemlos weitere Maßnahmen dazu oder benutzerdefinierte APIs zum LOB-Anwendung und diese APIs in Ihrer Anwendung Logik hinzufügen.

## <a name="features-and-use-cases"></a>Funktionen und Einsatzbeispiele ##

- Die AS2 X12 decodieren und Codieren von Aktionen können Sie Daten empfangen und Senden von Daten an Handelspartner Standardprotokolle verwenden Logik apps  
- AS2 und X12 können mit oder ohne einander Datenaustausch mit Handelspartnern Bedarf
- B2B-Aktionen erleichtern Partner und Abkommen Integration Konto erstellen und in eine Anwendung Logik verwenden  
- Durch Erweitern der Logik-app mit anderen Aktionen können senden und Empfangen von Daten aus anderen Systemen und Diensten wie SalesForce  

## <a name="learn-more"></a>Weitere Informationen ##

[Weitere Informationen zu Enterprise-Integrationspaket](./app-service-logic-enterprise-integration-overview.md)  