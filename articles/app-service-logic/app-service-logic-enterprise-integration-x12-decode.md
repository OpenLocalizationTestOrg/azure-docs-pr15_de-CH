<properties 
    pageTitle="Erfahren Sie mehr über Enterprise Integrationspaket decodieren X12 Nachricht Connctor | Microsoft Azure App Service | Microsoft Azure" 
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

# <a name="get-started-with-decode-x12-message"></a>Erste Schritte mit Decode X12 Nachricht

EDI und Partner-spezifische Eigenschaften überprüft, generiert XML-Dokument für jede Transaktion und Anerkennung verarbeiteten Transaktion.

## <a name="create-the-connection"></a>Erstellen der Verbindung

### <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Decode X12 Nachricht Connector verwenden, ist eine Integration-Konto erforderlich. Einzelheiten zum Erstellen eines [Kontos Integration](./app-service-logic-enterprise-integration-create-integration-account.md), [Partner](./app-service-logic-enterprise-integration-partners.md) und [X12 Vertrag](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Verbinden Sie mit Decode X12 Nachricht mit den folgenden Schritten:

1. [Erstellen einer Anwendung Logik](./app-service-logic-create-a-logic-app.md) Beispiel-

2. Dieser Connector keinen Trigger. Verwenden Sie andere Trigger zum Starten der Anwendung Logik wie Anforderung Trigger.  Im Designer Anwendung Logik Hinzufügen eines Triggers und eine Aktion hinzufügen.  Wählen Sie zeigen Microsoft verwalteten APIs in der Dropdown-Liste und geben Sie dann "X12" in das Suchfeld ein.  Wählen Sie X12-Decodierung X12 Nachricht

    ![X12 suchen](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Wenn Sie zuvor alle Verbindungen Integration Konto erstellt haben, werden Sie die Details der Verbindung aufgefordert

    ![Integration Kontoverbindung](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Geben Sie die Details Integration.  Eigenschaften mit einem Sternchen sind erforderlich

  	| Eigenschaft | Details |
  	| -------- | ------- |
  	| Verbindungsname * | Geben Sie einen beliebigen Namen für die Verbindung |
  	| Integration Konto * | Geben Sie den Kontonamen Integration. Die Integration Konto und Logik-app in Azure dort sind |

    Abschließend ähneln die Verbindungsdetails folgende
    
    ![Integration Kontoverbindung erstellt](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Wählen Sie **Erstellen**
    
6. Beachten Sie, dass die Verbindung hergestellt wurde.

    ![Integration Verbindung Kontodetails](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Select X12 flache an decodieren

    ![obligatorische Felder bereitstellen](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 decodieren kann nach

* Überprüft den Umschlag Geschäften partnervereinbarung
* Generiert ein XML-Dokument für jede Transaktion.
* Überprüft EDI und Partner-Eigenschaften
    * Strukturelle EDI-Überprüfung und erweitertes Schema-Validierung
    * Überprüfung der Struktur des Umschlags Austausch.
    * Schema-Validierung Umschlag für das Steuerelement Schema.
    * Schemaüberprüfung der Transaktion Satz von Datenelementen gegen das Nachrichtenschema.
    * EDI-Überprüfung auf Transaktionssatz Daten 
* Überprüft, ob Austausch, Gruppe und Transaktion Satz Steuerelement Zahlen nicht Duplikate
    * Überprüft die Austauschkontrollnummer gegen empfangenen Austauschvorgänge.
    * Gruppenkontrollnummer im Austausch mit anderen Gruppe Steuerelement überprüft.
    * Überprüft die Transaktion Kontrollnummer mit anderen Transaktion Satz Steuerelement in der Gruppe festgelegt.
* Den gesamten Austausch konvertiert in XML 
    * Austausch von Teilen als Transaktionssätze - suspend Transaktionssätze Fehler: analysiert jede Transaktion ein Austausch in einer separaten XML-Dokument festgelegt. Setzt mindestens eine Transaktion im Austausch fehl X12 Validierung Decode hält nur Transaktionssätze.
    * Austausch von Teilen als Transaktionssätze - Austausch Fehler unterbrechen: analysiert jede Transaktion ein Austausch in einer separaten XML-Dokument festgelegt.  Setzt mindestens eine Transaktion im Austausch fehl X12 Validierung Decode hält den gesamten Austausch.
    * Erhalten der Austausch - Transaktionssätze bei Fehler anhalten: erstellt ein XML-Dokument für den gesamten Batch Austausch. X12 decodieren hält nur Transaktionssätze, die validiert setzt gleichzeitig andere Transaktion verarbeiten
    * Erhalten der Austausch - Austausch bei Fehler anhalten: erstellt ein XML-Dokument für den gesamten Batch Austausch. Setzt mindestens eine Transaktion im Austausch fehl X12 Validierung Decode hält den gesamten Austausch 
* Generiert eine Bestätigung technischen und funktionalen (falls konfiguriert).
    * Eine technische Bestätigung generiert durch Header-Validierung. Technische Bestätigung meldet den Status der Verarbeitung eine Austausch-Header und einem Nachspann Empfänger Adresse.
    * Eine funktionale Bestätigung generiert durch Text-Validierung. Die funktionale Bestätigung meldet jeden Fehler während der Verarbeitung der empfangenen Dokuments

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket") 


