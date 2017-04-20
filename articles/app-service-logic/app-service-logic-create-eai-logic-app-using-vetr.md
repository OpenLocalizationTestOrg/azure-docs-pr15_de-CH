<properties
   pageTitle="Erstellen einer EAI-Lösung Logik Logik apps in Azure App Service bei VETR | Microsoft Azure"
   description="Überprüfen, codieren und Transformieren von XML BizTalk services"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="rajeshramabathiran"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="create-eai-logic-app-using-vetr"></a>Erstellen einer EAI-Lösung Logik mit VETR

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Die meisten Enterprise Application Integration (EAI) Szenarien vermitteln zwischen einer Quelle und einem Ziel. In solchen Situationen sind häufig ein Standardsatz an:

- Stellen Sie sicher, dass Daten aus anderen Systemen korrekt formatiert sind.
- Führen Sie "Suche" auf eingehende Daten zu.
- Konvertieren Sie Daten von einem Format in ein anderes. Beispielsweise können konvertieren Sie Daten aus Datenformat eines CRM-Systems in ein ERP-System Datenformat.
- Routendaten Anwendung oder System.

Dieser Artikel beschreibt ein allgemeines Muster Integration: "unidirektionale Nachrichtenvermittlung" oder VETR (Validate anreichern Transformation Route). VETR Muster vermittelt zwischen eine Quellentität und eine Zielentität. In der Regel sind Quelle und Ziel Datenquellen.

Sollten Sie eine Website, die Aufträge akzeptiert. Benutzer senden Aufträge zum System mit HTTP. Das System im Hintergrund eingehenden Daten auf Korrektheit überprüft, er normalisiert und in eine Warteschlange Service Bus zur weiteren Verarbeitung beibehalten. Aufträge aus der Warteschlange in einem bestimmten Format erwartet wird. Daher erfolgt die End-to-End:

**HTTP** → **validieren** → **umwandeln** → **Servicebus**

![Grundlegende VETR Fluss][1]

BizTalk-API Apps Aufbau dieses Muster:

* **Http-Trigger** - Quelle Triggerereignis Nachricht
* **Validate** - überprüft die Richtigkeit der Daten
* **Transform** - Transformationen Daten eingehenden Format Formatieren von downstream-System benötigt
* **Bus-Konnektors** - Zielentität, in dem Daten gesendet werden


## <a name="constructing-the-basic-vetr-pattern"></a>Das grundlegende Muster VETR erstellen
### <a name="the-basics"></a>Die Grundlagen

Wählen Sie in Azure-Portal **+ neu**, wählen Sie **Web + Mobile**und wählen Sie **Logik-App**. Wählen Sie Name, Position, Abonnements, Ressourcengruppe und Position arbeitet. Ressourcengruppen fungieren als Container für Ihre apps; alle Ressourcen für Ihre Anwendung werden derselben Ressourcengruppe.

Als Nächstes fügen Sie Trigger und Aktionen.


## <a name="add-http-trigger"></a>HTTP-Trigger hinzufügen
1. Wählen Sie im **Logik App Vorlagen** **Erstellen völlig**aus.
1. Wählen Sie **HTTP-Listener** aus der Galerie, um einen neuen Listener erstellen. Rufen sie **HTTP1**.
2. Festlegen der **Antwort automatisch senden?** auf false festgelegt. Konfigurieren Sie den Trigger durch _HTTP-Methode_ _POST_ und _Relativ_ zum _/OneWayPipeline_:  
    ![HTTP-Trigger][2]
3. Wählen Sie das grüne Häkchen Triggers abgeschlossen.

## <a name="add-validate-action"></a>Fügen Sie Aktionen überprüfen

Jetzt gebe Aktionen ausgeführt, wenn der Trigger ausgelöst wird – d. h. wenn ein Anruf auf den HTTP-Endpunkt.

1. Fügen Sie **BizTalk XML Validator** aus der Galerie hinzu und benennen Sie _(Validate1)_ zum Erstellen einer Instanz.
2. Konfigurieren eines XSD-Schemas XML-Nachrichten überprüfen. Wählen Sie die Aktion _Überprüfen_ und _Triggers('httplistener').outputs. Content_ als Wert für den Parameter _InputXml_ .

Die Validate-Aktion ist die erste Aktion nach der HTTP-Listener: 

![BizTalk XML-Bestätigung][3]

Entsprechend fügen Sie die restlichen Aktionen. 

## <a name="add-transform-action"></a>Transform-Aktion hinzufügen
Wir konfigurieren Transformationen Normalisierung der eingehenden Daten.

1. **Transformiert BizTalk-Dienst** aus dem Katalog hinzufügen.
2. Um eine Transformation zum Transformieren von XML-Nachrichten zu konfigurieren, wählen Sie die **Transformation** als Aktion durchzuführen, wenn die API aufgerufen wird. Wählen Sie ```triggers(‘httplistener’).outputs.Content``` als Wert für _InputXml_. *Map* ist ein optionaler Parameter, die eingehenden Daten mit alle konfigurierten Transformationen abgeglichen und die passenden Schema angewendet.
3. Schließlich wird die Transformation nur erfolgreicher überprüfen. Um diese Bedingung zu konfigurieren, wählen Sie Zahnradsymbol rechts oben und _Hinzufügen einer Bedingung erfüllt werden_. Legen Sie die Bedingung auf ```equals(actions('xmlvalidator').status,'Succeeded')```:  

![BizTalk-Transformationen][4]


## <a name="add-service-bus-connector"></a>Service Bus Verbindung hinzufügen
Als Nächstes fügen Sie das Ziel – Service Bus-Warteschlange-Daten schreiben.

1. Hinzufügen eines **Service Bus-Anschluss** aus der Galerie. Legen Sie den **Namen** , _Servicebus1_, **Verbindungszeichenfolge** die Verbindungszeichenfolge für die Dienstinstanz Bus festgelegt **Entitätsname** _Warteschlange_festlegen und **Abonnementname**überspringen.
2. Wählen Sie die **Nachricht senden** -Aktion, und legen Sie **das Inhaltsfeld für die Aktion** auf _Actions('transformservice').outputs. OutputXml_.
3. Legen Sie das Feld **Content-Typ** **Application/XML:  

![Servicebus][5]


## <a name="send-http-response"></a>HTTP-Antwort senden
Nach Abschluss der Pipeline-Verarbeitung zurücksenden einer HTTP-Antwort für Erfolg und Fehler mit den folgenden Schritten:

1. Fügen Sie einen **HTTP-Listener** aus der Galerie hinzu, und wählen Sie die **HTTP-Antwort senden** -Aktion.
2. **Antwort-ID** *Nachricht*festgelegt.
2. **Inhalt** auf *Pipeline-Verarbeitung abgeschlossen*festgelegt.
3. **Antwortstatuscode** *200* HTTP 200 OK an.
4. Wählen Sie im Dropdown-Menü rechts oben und **Hinzufügen einer Bedingung erfüllt werden**.  Legen Sie die Bedingung auf den folgenden Ausdruck:  
    ```@equals(actions('azureservicebusconnector').status,'Succeeded')```  <br/>
5. Wiederholen Sie diese Schritte zum Senden einer HTTP-Antwort auf Fehler. Der folgende Ausdruck ändern Sie **Bedingung** :  
```@not(equals(actions('azureservicebusconnector').status,'Succeeded'))``` <br/>
6. Wählen Sie **OK** und **Erstellen**.



## <a name="completion"></a>Abschluss
Jedes Mal, wenn jemand den HTTP-Endpunkt eine Nachricht sendet, löst die Anwendung und führt die Aktionen, die Sie gerade erstellt haben. Diese Logik-apps zu erstellen, wählen Sie in der Azure-Portal **Durchsuchen** und wählen Sie **Logik Apps**. Wählen Sie Ihre app mehr Informationen.

Einige hilfreiche Themen:

[Verwalten und Überwachen von API-Apps und Anschlüsse](app-service-logic-monitor-your-connectors.md)  <br/>
[Überwachen von Logik-Apps](app-service-logic-monitor-your-logic-apps.md)

<!--image references -->
[1]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BasicVETR.PNG
[2]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/HTTPListener.PNG
[3]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkXMLValidator.PNG
[4]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/BizTalkTransforms.PNG
[5]: ./media/app-service-logic-create-EAI-logic-app-using-VETR/AzureServiceBus.PNG
