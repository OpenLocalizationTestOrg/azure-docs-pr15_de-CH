<properties
   pageTitle="Lernprogramm: Bearbeiten EDIFACT mit Azure BizTalk Services | Microsoft Azure BizTalk-Dienste"
   description="Zum Erstellen und konfigurieren Sie ein Connector oder API-app und in einer Anwendung Logik in Azure App Service verwenden"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/31/2016"
   ms.author="deonhe"/>

# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Lernprogramm: Prozess EDIFACT Rechnungen Azure BizTalk-Dienste verwenden
Das BizTalk-Portal können Sie konfigurieren und Bereitstellen von X12 und EDIFACT Abkommen. In dieser Übung betrachten wir erstellen eine EDIFACT-Vereinbarung für den Austausch von Rechnungen zwischen Handelspartnern. In diesem Lernprogramm wird geschrieben, um eine End-to-End-Lösung mit zwei Handelspartner Northwind und Contoso die EDIFACT-Nachrichten austauschen.  

## <a name="sample-based-on-this-tutorial"></a>In diesem Lernprogramm Probe
In diesem Lernprogramm wird geschrieben um eine Probe **Senden EDIFACT Rechnungen mithilfe der BizTalk-Dienste**, der [MSDN Code Gallery](http://go.microsoft.com/fwlink/?LinkId=401005)herunterladen. Sie können das Beispiel und anhand dieses Lernprogramms zu verstehen, wie das Beispiel erstellt wurde. Alternativ können Sie in diesem Lernprogramm erstellen Sie Ihre eigene Lösung Grund auf. Dieses Lernprogramm richtet sich an die zweite, damit Sie verstehen, wie diese Lösung erstellt wurde. Außerdem so weit wie möglich das Lernprogramm entspricht der Probe und verwendet dieselbe für Artefakte (z. B. Schemas, Transformationen) wie im Beispiel.  

>[AZURE.NOTE] Da diese Lösung eine EAI-Brücke eines Sendevorgangs ein EDI-Brücke beinhaltet, verwendet [BizTalk Services Bridge verketten Beispiel](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) Beispiel.  

## <a name="what-does-the-solution-do"></a>Was die Lösung?

In dieser Lösung erhält Northwind EDIFACT Rechnungen von Contoso. Diese Rechnungen sind nicht in einem standardmäßigen EDI-Format. So muss vor dem Senden der Rechnung an Northwind, es (auch INVOIC genannt) Rechnungsbeleg EDIFACT transformiert werden. Beim Empfang muss Northwind EDIFACT Rechnung verarbeitet und eine Control Message (auch CONTRL genannt) an Contoso zurückgegeben.

![][1]  

Contoso verwendet für die Erreichung dieses Geschäftsszenario die Funktionen von Microsoft Azure BizTalk Services.

*   Contoso verwendet EAI-Brücken die ursprüngliche Rechnung EDIFACT INVOIC transformiert.

*   EAI-Brücke sendet die Nachricht dann an eine EDI senden Brücke als Teil des Abkommens im BizTalk-Portal konfiguriert.

*   EDI senden Bridge EDIFACT INVOIC verarbeitet und an Northwind weitergeleitet.

*   Nach Erhalt der Rechnung erhalten Northwind gibt eine CONTRL Nachricht dem EDI-Bridge als Bestandteil der Vereinbarung.  

> [AZURE.NOTE] Optional veranschaulicht diese Lösung auch die Batchverarbeitung verwenden Rechnungen in Batches, anstatt jede Rechnung getrennt.  

Um das Szenario verwenden wir Servicebuswarteschlangen zu Rechnung von Contoso zu Northwind Northwind Bestätigung erhalten. Diese Warteschlangen können mithilfe einer Clientanwendung heruntergeladen und gehört das Beispielpaket steht erstellt im Rahmen dieses Lernprogramms.  

## <a name="prerequisites"></a>Erforderliche Komponenten

*   Sie müssen einen Service Bus-Namespace. Informationen zum Erstellen eines Namespace finden Sie [How To: Erstellen oder Ändern einer Service Bus Service Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Nehmen wir an, dass Sie bereits einen Service Bus-Namespace bereitgestellt, **Edifactbts**aufgerufen.

*   Sie müssen ein Abonnement BizTalk-Dienste. Eine Anleitung finden Sie unter [Erstellen eines BizTalk-Dienstes verwenden Azure-Verwaltungsportal](http://go.microsoft.com/fwlink/?LinkID=302280). Angenommen Sie für dieses Lernprogramm, Sie BizTalk-Dienste, **Contosowabs**aufgerufen haben.

*   Registrieren Sie Ihr Abonnement BizTalk-Dienste auf das BizTalk-Portal. Informationen finden Sie unter [Registrieren einer BizTalk Service-Bereitstellung auf das BizTalk-Portal](https://msdn.microsoft.com/library/hh689837.aspx)

*   Sie müssen Visual Studio installiert.

*   Sie müssen BizTalk-Dienste SDK installiert. Sie können das SDK von [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057) herunterladen.  

## <a name="step-1-create-the-service-bus-queues"></a>Schritt 1: Erstellen der Servicebuswarteschlangen  
Diese Lösung verwendet Servicebuswarteschlangen zum Austausch von Nachrichten zwischen Handelspartnern. Contoso und Northwind Nachrichten in Warteschlangen aus, wo Sie EAI oder EDI-Brücken nutzen. Diese Lösung erfordert drei Servicebuswarteschlangen:

*   **Northwindreceive** – erhält Northwind Rechnung von Contoso über diese Warteschlange.

*   **Contosoreceive** – empfängt Contoso Empfangsbestätigung Northwind in diese Warteschlange.

*   Alle ausgesetzt **ausgesetzt** – Nachrichten in diese Warteschlange weitergeleitet werden. Nachrichten werden angehalten, wenn sie während der Verarbeitung nicht.

Dieser Service Bus-Warteschlangen können mithilfe einer Clientanwendung im Beispielpaket enthalten.  

1.  Öffnen Sie aus, wo das Beispiel herunterladen, **Lernprogramm senden Rechnungen verwenden BizTalk Services EDI-Bridges.sln**.

2.  Drücken Sie **F5** zum Erstellen und Starten der **Tutorial** -Clientanwendung.

3.  Geben Sie im Bildschirm Service Bus ACS Namespace, Ausstellername und Aussteller Schlüssel.

    ![][2]  
4.  Ein Meldungsfeld fordert drei Warteschlangen in Ihrem Service Bus-Namespace erstellt. Klicken Sie auf **OK**.

5.  Lassen Sie der Lernprogramm ausführen. Öffnen, klicken Sie auf **Service Bus** > **_Service Bus-Namespace_** > **Warteschlangen**, und stellen Sie sicher, dass die drei Warteschlangen erstellt werden.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Schritt 2: Erstellen und Bereitstellen von handelspartnervereinbarung
Erstellen einer handelspartnervereinbarung zwischen Contoso und Northwind. Eine handelspartnervereinbarung definiert einen Handel zwischen zwei Geschäftspartnern wie die Nachrichtenschema mit der messaging-Protokoll usw. verwenden. Eine handelspartnervereinbarung umfasst zwei EDI-Brücken Nachrichten an Handelspartner ( **EDI senden Bridge**genannt) senden und empfangen Nachrichten von Handelspartnern ( **Empfangen von EDI-Bridge**bezeichnet).

Im Kontext dieser Lösung EDI senden Bridge entspricht der Sendeseite des Abkommens und wird verwendet, um die EDIFACT von Contoso zu Northwind Rechnung. Ebenso EDI empfangen Bridge entspricht der Empfangsseite des Abkommens und zum Empfangen von Empfangsbestätigungen von Northwind.  

### <a name="create-the-trading-partners"></a>Die Handelspartner erstellen

Erstellen Sie mit Handelspartnern für Contoso und Northwind.  

1.  Klicken Sie im BizTalk-Portal, auf der Registerkarte **Partner** **Hinzufügen**.

2.  Neue Partner-Seite Geben Sie **Contoso** als Partnername ein und dann auf **Speichern**.

3.  Wiederholen Sie die Schritte zum Erstellen des zweiten Partners **Northwind**.  

### <a name="create-the-agreement"></a>Die Vereinbarung erstellen
Handelsabkommen Partner werden zwischen Geschäftsprofile Handelspartner erstellt. Diese Lösung verwendet der Standardprofile für Partner, die automatisch erstellt werden, wenn wir Partner erstellt.  

1.  Klicken Sie im BizTalk-Portal auf **Verträge** > **Hinzufügen**.

2.  Die neue Vereinbarung **General Settings** Seite Geben Sie die Werte wie in der folgenden Abbildung dargestellt und klicken Sie auf **Weiter**.

    ![][3]  

    Nachdem Sie auf **Weiter**klicken, stehen zwei Registerkarten **Einstellungen empfangen** und **Senden** .

3.  Erstellen Sie senden Vereinbarung zwischen Contoso und Northwind. Diese Vereinbarung regelt, wie Contoso EDIFACT Rechnung an Northwind sendet.

    1.  Klicken Sie auf **Settings**.

    2.  Behalten Sie die Standardwerte auf den Registerkarten **Eingehende URL**, **Transformieren**und **Batching** .

    3.  Laden Sie auf der Registerkarte **Protokoll** Abschnitt **Schemas** **EFACT_D93A_INVOIC.xsd** Schema. Dieses Schema ist das Beispielpaket verfügbar.

        ![][4]  
    4.  Geben Sie auf der Registerkarte **Transport** Details für die Servicebuswarteschlangen. Für die Vereinbarung Seite senden verwenden wir die Warteschlange **Northwindreceive** EDIFACT Rechnung an Northwind und **unverarbeiteten Nachrichten weiterleiten, die während der Verarbeitung Fehler angehalten** . Erstellt diese Warteschlangen in **Schritt 1: erstellen die Servicebuswarteschlangen** (in diesem Thema).

        ![][5]  

        Unter **Transport Settings > Transporttyp** und **Federung Nachricht > Transporttyp**Azure Service Bus auswählen, und geben Sie die Werte wie in der Abbildung dargestellt.

4.  Erstellen Sie Abkommens empfangen zwischen Contoso und Northwind. Diese Vereinbarung regelt, wie Contoso die Bestätigung von Northwind empfängt.

    1.  Klicken Sie auf **Einstellungen angezeigt**.

    2.  Behalten Sie die Standardwerte auf den Registerkarten **Transport** und **Transformieren** .

    3.  Laden Sie auf der Registerkarte **Protokoll** Abschnitt **Schemas** **EFACT_4.1_CONTRL.xsd** Schema. Dieses Schema ist das Beispielpaket verfügbar.

    4.  Die Registerkarte **Arbeitsplan** erstellen Sie einen Filter, um sicherzustellen, dass nur Empfangsbestätigungen von Northwind Contoso weitergeleitet werden. Klicken Sie auf **Hinzufügen** , um das routing Filtern unter **Route Settings**.

        ![][6]  
        1.  Geben Sie Werte für **Name der** **Routenregel**und **Ziel** wie in der Abbildung dargestellt.

        2.  Klicken Sie auf **Speichern**.

    5.  Auf der Registerkarte **Route** , geben Sie, unterbrochene Empfangsbestätigungen (Empfangsbestätigungen, die während der Verarbeitung nicht) an weitergeleitet werden. Legen Sie den Transporttyp auf Azure Service Bus, route Zieltyp an **Warteschlange**Authentifizierungstyp **SAS** (SAS) SAS-Verbindungszeichenfolge für den Service Bus-Namespace bereitzustellen, und geben Sie den Warteschlangennamen als **angehalten**.

5.  Klicken Sie abschließend auf **Bereitstellen** des Abkommens bereitstellen. Beachten Sie die Endpunkte, senden und Empfangen von Abkommen Implementierung.

    *   Beachten Sie auf der Registerkarte **Gesendet Settings** unter **Eingehende URL**Endpunkt. Um eine Nachricht von Contoso zu Northwind mit EDI senden Bridge zu senden, müssen Sie diesen Endpunkt eine Nachricht senden.

    *   Beachten Sie auf der Registerkarte **Einstellungen erhalten** unter **Transport**Endpunkt. Eine Nachricht von Northwind Contoso Bridge erhalten die EDI verwenden, müssen Sie diesen Endpunkt eine Nachricht senden.  

## <a name="step-3-create-and-deploy-the-biztalk-services-project"></a>Schritt 3: Erstellen Sie und Bereitstellen Sie der BizTalk-Dienste

Im vorherigen Schritt bereitgestellt EDI senden und empfangen Abkommen EDIFACT Rechnungen und Empfangsbestätigungen. Diese Abkommen können nur Nachrichten verarbeiten, die dem standard EDIFACT Nachricht entsprechen. Allerdings sendet pro Szenario für diese Lösung Contoso eine Rechnung an Northwind in einem internen proprietäre Schema. Also vor dem Senden der Nachricht EDI senden Brücke müssen sie das interne Schema standard EDIFACT Rechnung Schema transformiert werden. Die BizTalk-Dienste EAI Project jenes.

BizTalk-Dienste Projekt **InvoiceProcessingBridge**, die Nachricht transformiert ist auch Bestandteil des Beispiels heruntergeladene. Das Projekt umfasst die folgenden Elemente:

*   **INHOUSEINVOICE. XSD** -Schema der internen Rechnung an Northwind.

*   **EFACT_D93A_INVOIC. XSD** -Schema standard EDIFACT Rechnung.

*   **EFACT_4.1_CONTRL. XSD** -Schema EDIFACT-Bestätigung, die Northwind Contoso sendet.

*   **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – die Transformation, die interne Rechnung Schema das Standardschema EDIFACT Rechnung zugeordnet ist.  

### <a name="create-the-biztalk-services-project"></a>BizTalk Services-Projekt erstellen
1.  In der Visual Studio-Projektmappe erweitern Sie das InvoiceProcessingBridge-Projekt, und öffnen Sie die Datei **MessageFlowItinerary.bcs** .

2.  Klicken Sie auf der Leinwand und der **BizTalk-Dienst-URL** in das Eigenschaftenfeld Namen der BizTalk-Dienste an. Z. B. `https://contosowabs.biztalk.windows.net`.

    ![][7]  
3.  Ziehen Sie aus der Toolbox eine **Unidirektionale XML-Bridge** auf die Leinwand. Legen Sie die Eigenschaften **Entitätsnamen** und **Relative Adresse** der Brücke zu **ProcessInvoiceBridge**. Doppelklicken Sie auf **ProcessInvoiceBridge** Fläche Konfiguration Bridge öffnen.

4.  Klicken Sie im Feld **Nachrichtentypen** auf das Pluszeichen (**+**) um das Schema der eingehenden Nachricht anzugeben. Da die eingehende Nachricht EAI-Brücke immer intern Rechnung ist, legen Sie den **INHOUSEINVOICE**.

    ![][8]  
5.  Klicken Sie auf die **XML-Transformation** Form und im Eigenschaftenfeld für die Eigenschaft **wird** auf das Auslassungszeichen (**...**). Klicken Sie im Dialogfeld **Karten Auswahl** wählen Sie Transformationsdatei **INHOUSEINVOICE_to_D93AINVOIC aus** und klicken Sie dann auf **OK**.

    ![][9]  
6.  Zurück zu **MessageFlowItinerary.bcs**und aus, ziehen Sie eine **Bidirektionale externe Endpunkt** rechts vom **ProcessInvoiceBridge**. Die **Entitätsname** -Eigenschaft auf **EDIBridge**festgelegt.

7.  Im Projektmappen-Explorer erweitern Sie **MessageFlowItinerary.bcs** , und doppelklicken Sie auf die **EDIBridge.config** . Ersetzen Sie den Inhalt des **EDIBridge.config** mit folgenden.

    > [AZURE.NOTE] Warum muss ich die config-Datei bearbeiten? Wir Bridge Designer Leinwand hinzugefügt externen Dienstendpunkt stellt EDI-Brücken, die wir zuvor bereitgestellt. EDI-Brücken sind bidirektionale Brücken Send und Seite erhalten. Unidirektionale Brücke ist EAI-Brücke, die wir Bridge-Designer hinzugefügt. Verwenden zum Behandeln der verschiedenen Nachrichtenaustauschmuster zwei Brücken wir benutzerdefinierte Bridge-Verhalten mit der Konfiguration in der Datei config. Darüber hinaus verarbeitet das benutzerdefinierte Verhalten die Authentifizierung EDI senden Bridge Endpunkt. Dieses benutzerdefinierte Verhalten steht als separate Beispiel zur [BizTalk Services Bridge Sample - EAI EDI-Verkettung](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Diese Lösung verwendet das Beispiel.  
    
    ```
<?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <extensions>
          <behaviorExtensions>
            <add name="BridgeAuthentication" 
                  type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
          </behaviorExtensions>
        </extensions>
        <behaviors>
          <endpointBehaviors>
            <behavior name="BridgeAuthenticationConfiguration">
              <!-- Enter the ACS namespace, issuer name and issuer secret of the BizTalk Services deployment -->
              <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                    issuername="owner" 
                                    issuersecret="[YOUR ACS SECRET]" />
              <webHttp />
            </behavior>
          </endpointBehaviors>
        </behaviors>
        <bindings>
          <webHttpBinding>
            <binding name="BridgeBindingConfiguration">
              <security mode="Transport" />
            </binding>
          </webHttpBinding>
        </bindings>
        <client>
          <clear />
          <!--
            Go BizTalk Portal > Agreement > Send Settings > Inbound URL
            Copy the Endpoint URL and paste it in the below address field
          -->
          <endpoint name="TwoWayExternalServiceEndpointReference1" 
                    address="[YOUR EDI BRIDGE SEND URI]" 
                    behaviorConfiguration="BridgeAuthenticationConfiguration" 
                    binding="webHttpBinding" 
                    bindingConfiguration="BridgeBindingConfiguration" 
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />
        </client>
      </system.serviceModel>
    </configuration>

    ```
8.  Aktualisieren Sie die EDIBridge.config Datei Konfigurationsdetails

    *   Unter _<behaviors>_, ACS-Namespace und der BizTalk-Dienste-Abonnement zugeordneten Schlüssel bereitstellen.

    *   Unter _<client>_, bieten den Endpunkt, EDI senden Vereinbarung bereitgestellt.

    Speichern Sie und schließen Sie die Konfigurationsdatei.

9.  Aus klicken Sie den **Anschluss** und die Komponenten **ProcessInvoiceBridge** und **EDIBridge** . Markieren Sie den Verbinder, und legen Sie Eigenschaften im Feld **Bedingung** auf **Alles**. Dadurch der EDI-Brücke von EAI-Bridge verarbeitet alle Nachrichten weitergeleitet werden.

    ![][10]  
10.  Speichern der Projektmappe.  

### <a name="deploy-the-project"></a>Bereitstellen des Projekts

1.  Downloaden Sie auf dem Computer, in dem das BizTalk-Dienste-Projekt erstellt und installieren Sie das SSL-Zertifikat für die BizTalk-Dienste-Abonnement. Unter BizTalk-Dienste auf **Dashboard**, und klicken Sie dann auf **Zertifikat downloaden**. Doppelklicken Sie auf das Zertifikat, und folgen Sie der Aufforderung zum Abschließen der Installation. Stellen Sie sicher, dass unter Zertifikatspeicher für **Vertrauenswürdige Stammzertifizierungsstellen** -Zertifikat installieren.

2.  Im Visual Studio-Projektmappen-Explorer mit der rechten Maustaste des **InvoiceProcessingBridge** Projekts und dann auf **Bereitstellen**.

3.  Geben Sie die Werte wie in der Abbildung gezeigt, und klicken Sie auf **Bereitstellen**. Auf **Informationen** aus der BizTalk-Dienste Dashboard können Sie die ACS-Anmeldeinformationen für BizTalk-Dienste abrufen.

    ![][11]  

    Kopieren Sie aus dem Ausgabebereich den Endpunkt, die EAI-Brücke, z. B. bereitgestellt ist `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Sie benötigen diese Endpunkt-URL später.  

## <a name="step-4-test-the-solution"></a>Schritt 4: Testen der Lösung


In diesem Abschnitt betrachten wir die Lösung mithilfe der **Tutorial** -Clientanwendung als Teil des Beispiels testen.  

1.  In Visual Studio das **Lernprogramm Client**starten F5.

2.  Der Bildschirm muss die Werte aus dem Schritt aufgefüllt, haben wir die Service Bus-Warteschlangen. Klicken Sie auf **Weiter**.

3.  Im nächsten Fenster ACS Anmeldeinformationen für BizTalk-Dienste-Abonnement und die Endpunkte bereitstellen, EAI und EDI (empfangen) Brücken bereitgestellt.

    EAI-Brücke Endpunkt wurde im vorherigen Schritt kopiert werden. Für EDI Bridge Endpunkt im BizTalk-Portal erhalten, gehen Sie zu der Vereinbarung > Einstellungen erhalten > Transport > Endpunkt.

    ![][12]  
4.  Im nächsten Fenster klicken Sie unter Contoso auf **Interne Rechnung senden** . Öffnen Sie in der Datei im Dialogfeld zu, öffnen Sie die Datei INHOUSEINVOICE.txt. Untersuchen Sie den Inhalt der Datei, und klicken Sie dann auf **OK** , um die Rechnung.

    ![][13]  
5.  Die Rechnung wird in wenigen Sekunden Northwind empfangen. Klicken Sie auf **Nachricht anzeigen** , um die Northwind übermittelten Rechnung anzuzeigen. Beachten Sie, dass Northwind erhaltenen Rechnung im Standardschema EDIFACT gesendet von Contoso ein internes Schema war.

    ![][14]  
6.  Wählen Sie die Rechnung und klicken Sie **Bestätigung senden**. Beachten Sie im Dialogfeld, das erscheint, dass die Austausch-ID bei empfangenen Rechnung und die Bestätigung gesendet werden. Klicken Sie im Dialogfeld **Bestätigung senden**

    ![][15]  
7.  In wenigen Sekunden wird die Bestätigung bei Contoso erfolgreich empfangen.

    ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Schritt 5 (optional): Senden EDIFACT Rechnung in Stapeln 
BizTalk EDI-Brücken unterstützt Batchverarbeitung ausgehender Nachrichten. Diese Funktion eignet sich für den Empfang von Partnern, die Nachrichten (Erfüllung bestimmter Kriterien) statt einzelner Nachrichten erhalten möchten.

Der wichtigste Aspekt beim Arbeiten mit Batches ist die tatsächliche Version des Stapels auch bezeichnet die Freigabekriterien. Die Version kann Kriterien wie Empfangspartner Nachrichten empfangen möchte. Wenn die Batchverarbeitung aktiviert ist, sendet die EDI-Brücke die ausgehende Nachricht an dem empfangenden Partner bis die Freigabekriterien erfüllt. Beispielsweise eine Batchverarbeitung basieren auf Nachricht Größe Dispatches einen Stapel nur wenn ' n ' Nachrichten zusammengefasst. Kriterien für die Stapelverarbeitung auch möglich zeitbasiert, ein Stapel zu einer festen Zeit täglich gesendet wird. In dieser Lösung versuchen wir die Nachrichtengröße basieren Kriterien.

1.  Klicken Sie im BizTalk-Portal auf zuvor erstellten Vertrag. Klicken Sie auf Senden > Batchverarbeitung > Batch hinzufügen.

2.  Namen geben Sie **InvoiceBatch** **Klicken**und eine Beschreibung.

3.  Kriterien Sie einen Batch, die definiert, welche Nachrichten verarbeitet werden müssen. In dieser Lösung batch wir alle Nachrichten. So wählen Sie die erweiterte Option Produktdefinitionen und geben Sie **1 = 1 ein**. Dies ist eine Bedingung, die immer true ist und daher alle Nachrichten verarbeitet. Klicken Sie auf **Weiter**.

    ![][17]  
4.  Geben Sie einen Stapel Freigabekriterien. Wählen Sie aus dem Dropdownfeld **MessageCountBased**, und geben Sie für **Anzahl** **3**. Dies bedeutet, dass drei Nachrichten an Northwind gesendet wird. Klicken Sie auf **Weiter**.

    ![][18]  
5.  Überprüfen Sie die Zusammenfassung, und klicken Sie auf **Speichern**. Klicken Sie auf **Bereitstellen** , um die Vereinbarung erneut bereitstellen.

6.  Zurück an den **Client Lernprogramm**auf **Interne Rechnung senden**und folgen der Rechnung. Sie werden feststellen, dass keine Rechnung an Northwind empfangen werden, da die Batchgröße nicht erfüllt wird. Wiederholen Sie diesen Schritt noch zweimal, so dass Sie drei Nachrichten an Northwind Rechnung. Dies erfüllt die Kriterien für die Stapelverarbeitung Version 3 Nachrichten und eine Rechnung an Northwind sollte jetzt angezeigt werden.


<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

