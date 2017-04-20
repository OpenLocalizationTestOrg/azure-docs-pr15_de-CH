<properties 
    pageTitle="Überblick über Enterprise-Integrationspaket | Microsoft Azure App Service | Microsoft Azure" 
    description="Mit der Business-Prozess und Integration mit Microsoft Azure App Service Verfasser die Funktionen von Enterprise-Integrationspaket" 
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

# <a name="enterprise-integration-with-xml-transforms"></a>Enterprise-Integration mit XML-Transformationen

## <a name="overview"></a>Übersicht
Enterprise-Integration Transformation Connector konvertiert Daten von einem Format in ein anderes Format. Beispielsweise müssen Sie eine eingehende Nachricht mit dem aktuellen Datum im Format Jahrmonattag. Eine Transformation können Sie das Datum im Format MonthDayYear formatieren.

## <a name="what-does-a-transform-do"></a>Was eine Transformation?
Eine Transformation ist auch eine Zuordnung besteht aus einer Quelle XML-Schema (Eingabe) und Ziel-XML-Schema (Ausgabe). Verschiedene integrierte Funktionen können zu ändern oder das Datensteuerelement einschließlich Zeichenfolgen bedingte Aufgaben arithmetische Ausdrücke, Datum Uhrzeit Formatierungsprogramme und sogar Schleifenkonstrukte.

## <a name="how-to-create-a-transform"></a>Eine Transformation erstellen
Mit Visual Studio [Enterprise Integration SDK](https://aka.ms/vsmapsandschemas)können Sie eine Transformation-Zuordnung erstellen. Sie abschließend erstellen und Testen der Transformations, laden Sie die Transformation berücksichtigt Integration. 

## <a name="how-to-use-a-transform"></a>Verwendung eine Transformation
Nach dem Hochladen der Transformations berücksichtigt Integration können Sie Logik-app erstellen. Die Anwendung Logik wird ausgeführt Transformationen Logik app ausgelöst (und es input Inhalt umgewandelt werden).

**Hier werden Schritte zur Transformation**:

### <a name="prerequisites"></a>Erforderliche Komponenten 
In der Vorschau müssen Sie:  

-  [Erstellen Sie einen Container Azure-Funktionen] (https://ms.portal.azure.com/#create/Microsoft.FunctionApp "Erstellen Sie einen Container Azure-Funktionen")  
-  [Hinzufügen einer Funktion zum Container Azure-Funktionen] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-transform-function%2Fazuredeploy.json "Diese Vorlage erstellt eine Webhook Basis C# Azure Funktion mit Transformation Logik apps Integrationsszenarios verwenden")    
-  Erstellen Sie ein Konto Integration und fügen Zuordnung hinzu  

>[AZURE.TIP] Notieren Sie die den Namen des Containers Azure Funktionen und Azure Funktion Sie benötigen sie im nächsten Schritt.  

Nachdem Sie die erforderlichen Komponenten durchgeführt haben, ist Ihre Anwendung Logik erstellen werden:  

1. Erstellen Sie eine Logik-app und [Verknüpfen Sie es mit Ihrem Konto Integration](./app-service-logic-enterprise-integration-accounts.md "lernen einer Firma Integration Logik app verknüpfen") mit der Zuordnung.
2. Einen **Anforderung - Wenn eine HTTP-Anforderung empfangen** Trigger für Ihre Anwendung Logik hinzufügen  
![](./media/app-service-logic-enterprise-integration-transforms/transform-1.png)    
3. Fügen Sie **XML transformieren** Tätigwerden erste Auswahl **eine Aktion hinzufügen**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-2.png)   
4. Geben Sie *Transformieren* Wort in das Suchfeld um alle Aktionen zu den Filtern, die Sie verwenden möchten  
![](./media/app-service-logic-enterprise-integration-transforms/transform-3.png)  
5. Wählen Sie die Aktion **XML transformieren**   
![](./media/app-service-logic-enterprise-integration-transforms/transform-4.png)  
6. Wählen Sie die **Funktion CONTAINER** mit der Funktion verwendet. Dies ist der Name des Containers Azure Funktionen früher in diesen Schritten erstellten.
7. Wählen Sie die **Funktion** verwenden möchten. Dies ist der Name der zuvor erstellten Azure-Funktion.
8. Fügen Sie die XML- **Inhalte** , die Sie umwandeln. Beachten Sie, dass Sie XML-Daten in der HTTP-Anforderung als die- **Inhalt**erhalten. Wählen Sie in diesem Beispiel den Hauptteil der HTTP-Anforderung, die die Anwendung Logik ausgelöst.
9. Wählen Sie die **Karte** , die zum Durchführen der Transformation verwenden möchten. Die Zuordnung muss bereits in Ihrem Konto Integration. In einem früheren Schritt haben Sie bereits Ihre Logik app Zugriff Kontos Integration, der die Zuordnung enthält.
10. Speichern Sie Ihre Arbeit  
![](./media/app-service-logic-enterprise-integration-transforms/transform-5.png) 

An diesem Punkt können Sie nach dem Erstellen der Zuordnung. In einer realen Anwendung sollten Sie die transformierten Daten in einer LOB-Anwendung wie SalesForce speichern. Sie können als Aktion Salesforce die Ausgabe der Transformation an. 

Sie können jetzt Ihre Transformation durch eine Anforderung an den HTTP-Endpunkt testen.  

## <a name="features-and-use-cases"></a>Funktionen und Einsatzbeispiele

- Die Transformation erstellt eine Zuordnung kann einfach, wie Name und Adresse von einem Dokument in ein anderes kopieren. Oder mittels der vordefinierten Karte komplexere Transformationen erstellen.  
- Mehrere Zuordnungsoperationen oder Funktionen stehen, einschließlich Zeichenfolgen, Datum Zeitfunktionen und So weiter.  
- Führen Sie eine direkte Datenkopie zwischen den Schemas. In der Zuordnung im SDK enthalten ist dies so einfach wie eine Linie, die die Elemente im Quellschema mit deren Gegenstücken im Zielschema verbindet.  
- Beim Erstellen einer Zuordnung anzeigen grafisch der Karte zeigen die Beziehung und Links, die Sie erstellen.
- Die Funktion Zuordnung testen Beispiel für eine XML-Nachricht hinzufügen. Mit einem Klick können Sie getestet erstellten und generierte Ausgabe sehen.  
- Vorhandene Karten hochladen  
- Bietet Unterstützung für das XML-Format.


## <a name="learn-more"></a>Weitere Informationen
- [Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket")  
- [Erfahren Sie mehr über Karten] (./app-service-logic-enterprise-integration-maps.md "Erfahren Sie mehr über Unternehmensintegration Karten")  
 