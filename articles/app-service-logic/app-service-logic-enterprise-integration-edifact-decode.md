<properties 
    pageTitle="Erfahren Sie mehr über Enterprise Integrationspaket decodieren EDIFACT Nachricht Connector | Microsoft Azure App Service | Microsoft Azure" 
    description="Erfahren Sie, wie mit Enterprise-Integrationspaket und Logik apps" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-edifact-message"></a>Erste Schritte mit EDIFACT-Nachricht entschlüsseln

EDI und Partner-spezifische Eigenschaften überprüft, generiert XML-Dokument für jede Transaktion und Anerkennung verarbeiteten Transaktion.

## <a name="create-the-connection"></a>Erstellen der Verbindung

### <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Ein Konto Integration muss decodiert EDIFACT Nachricht Connector verwenden. Einzelheiten Sie zum Erstellen einer [Integration Konto](./app-service-logic-enterprise-integration-create-integration-account.md)und [Partnern](./app-service-logic-enterprise-integration-partners.md) [EDIFACT-Vereinbarung](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Verbinden Sie mit decodieren EDIFACT-Nachricht mit den folgenden Schritten:

1. [Erstellen einer Anwendung Logik](./app-service-logic-create-a-logic-app.md) enthält ein Beispiel.

2. Dieser Connector keinen Trigger. Verwenden Sie andere Trigger zum Starten der Anwendung Logik wie Anforderung Trigger.  Im Designer Anwendung Logik Hinzufügen eines Triggers und eine Aktion hinzufügen.  Wählen Sie zeigen Microsoft verwalteten APIs in der Dropdown-Liste und geben Sie dann im Suchfeld "EDIFACT".  Wählen Sie decodieren EDIFACT-Nachricht

    ![EDIFACT suchen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Wenn Sie zuvor alle Verbindungen Integration Konto erstellt haben, werden Sie die Details der Verbindung aufgefordert

    ![Integration-Konto erstellen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Geben Sie die Details Integration.  Eigenschaften mit einem Sternchen sind erforderlich

  	| Eigenschaft | Details |
  	| -------- | ------- |
  	| Verbindungsname * | Geben Sie einen beliebigen Namen für die Verbindung |
  	| Integration Konto * | Geben Sie den Kontonamen Integration. Die Integration Konto und Logik-app in Azure dort sind |

    Abschließend ähneln die Verbindungsdetails folgende

    ![Integration-Konto erstellt](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Wählen Sie **Erstellen**

6. Beachten Sie, dass die Verbindung hergestellt wurde

    ![Integration Verbindung Kontodetails](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Wählen Sie EDIFACT Flatfile-Nachricht entschlüsseln

    ![obligatorische Felder bereitstellen](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT Decodieren ist nach

* Beheben der Vereinbarung mit dem absenderqualifizierer Bezeichner und Empfänger Qualifizierer & Bezeichner
* Mehrere Austauschvorgänge in einer einzelnen Nachricht in Separate teilt.
* Überprüft den Umschlag Geschäften partnervereinbarung
* Disassembliert den Austausch.
* EDI und Partner-spezifische Eigenschaften überprüft enthält
    * Überprüfung der Struktur des Umschlags Austausch.
    * Schema-Validierung Umschlag für das Steuerelement Schema.
    * Schemaüberprüfung der Transaktion Satz von Datenelementen gegen das Nachrichtenschema.
    * EDI-Überprüfung auf Transaktionssatz Daten
* Überprüft, ob Austausch, Gruppe und Transaktion Satz Steuerelement Zahlen nicht Duplikate (falls konfiguriert) 
    * Überprüft die Austauschkontrollnummer gegen empfangenen Austauschvorgänge. 
    * Gruppenkontrollnummer im Austausch mit anderen Gruppe Steuerelement überprüft. 
    * Überprüft die Transaktion Kontrollnummer mit anderen Transaktion Satz Steuerelement in der Gruppe festgelegt.
* Generiert ein XML-Dokument für jede Transaktion.
* Den gesamten Austausch konvertiert in XML 
    * Austausch von Teilen als Transaktionssätze - suspend Transaktionssätze Fehler: analysiert jede Transaktion ein Austausch in einer separaten XML-Dokument festgelegt. Wenn Transaktion Gruppen im Austausch, validiert und EDIFACT decodieren nur Transaktionssätze hält. 
    * Austausch von Teilen als Transaktionssätze - Austausch Fehler unterbrechen: analysiert jede Transaktion ein Austausch in einer separaten XML-Dokument festgelegt.  Wenn mindestens eine Transaktion im Austausch wird fehlschlagen Sie Validierung EDIFACT decodieren den gesamten Austausch hält.
    * Erhalten der Austausch - Transaktionssätze bei Fehler anhalten: erstellt ein XML-Dokument für den gesamten Batch Austausch. EDIFACT decodieren hält nur Transaktionssätze, die validiert gleichzeitig alle anderen Transaktionssätze verarbeiten
    * Erhalten der Austausch - Austausch bei Fehler anhalten: erstellt ein XML-Dokument für den gesamten Batch Austausch. Wenn mindestens eine Transaktion im Austausch wird fehlschlagen Sie Validierung EDIFACT decodieren den gesamten Austausch hält, 
* Generiert eine technische (Steuerelement) bzw. funktionsbestätigung (falls konfiguriert).
    * Eine technische Bestätigung oder CONTRL ACK Testergebnisse eine syntaktische Überprüfung der vollständige Austausch empfangen.
    * Eine funktionale Bestätigung bestätigt annehmen oder Ablehnen einer empfangenen Austausch oder eine Gruppe

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket") 