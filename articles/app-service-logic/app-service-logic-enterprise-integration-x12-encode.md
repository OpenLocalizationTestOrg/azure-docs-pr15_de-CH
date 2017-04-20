<properties 
    pageTitle="Erfahren Sie mehr über Enterprise Integrationspaket codieren X12 Nachricht Connctor | Microsoft Azure App Service | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Erste Schritte mit Codierung X12 Nachricht

EDI und Partner-spezifische Eigenschaften überprüft, konvertiert XML-codierten Nachrichten an EDI-Transaktion im Austausch und technischen und funktionalen Bestätigung angefordert

## <a name="create-the-connection"></a>Erstellen der Verbindung

### <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Ein Konto Integration muss codieren X12 Nachricht Connector verwenden. Einzelheiten zum Erstellen eines [Kontos Integration](./app-service-logic-enterprise-integration-create-integration-account.md), [Partner](./app-service-logic-enterprise-integration-partners.md) und [X12 Vertrag](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Verbinden Sie mit Codierung X12 Nachricht mit den folgenden Schritten:

1. [Erstellen einer Anwendung Logik](./app-service-logic-create-a-logic-app.md) Beispiel-

2. Dieser Connector keinen Trigger. Verwenden Sie andere Trigger zum Starten der Anwendung Logik wie Anforderung Trigger.  Im Designer Anwendung Logik Hinzufügen eines Triggers und eine Aktion hinzufügen.  Wählen Sie zeigen Microsoft verwalteten APIs in der Dropdown-Liste und geben Sie dann "X12" in das Suchfeld ein.  Wählen Sie entweder X12-Codierung X12 Nachricht nach Vereinbarungsnamen oder X12-Identitäten X 12-Nachricht codiert.  

    ![X12 suchen](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Wenn Sie zuvor alle Verbindungen Integration Konto erstellt haben, werden Sie die Details der Verbindung aufgefordert

    ![Integration Kontoverbindung](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Geben Sie die Details Integration.  Eigenschaften mit einem Sternchen sind erforderlich

  	| Eigenschaft | Details |
  	| -------- | ------- |
  	| Verbindungsname * | Geben Sie einen beliebigen Namen für die Verbindung |
  	| Integration Konto * | Geben Sie den Kontonamen Integration. Die Integration Konto und Logik-app in Azure dort sind |

    Abschließend ähneln die Verbindungsdetails folgende

    ![Integration Kontoverbindung erstellt](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Wählen Sie **Erstellen**

6. Beachten Sie, dass die Verbindung hergestellt wurde.

    ![Integration Verbindung Kontodetails](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-Codierung X12 Nachricht Vertragsname

7. Wählen Sie X12 aus den Dropdown-Listen und XML-Nachricht codieren.

    ![obligatorische Felder bereitstellen](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-Codierung X12 Nachricht von Identitäten

7.  Sender-ID, Absender, empfängerbezeichner und Empfänger bereitstellen, wie in den X12 konfiguriert Vereinbarung.  Wählen Sie XML-Nachricht codieren

    ![obligatorische Felder bereitstellen](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>X12 codieren ist folgende:

* Vereinbarungsauflösung durch entsprechende Kontexteigenschaften für Absender und Empfänger.
* Konvertieren von XML-codierten Nachrichten an EDI-Transaktion im Austausch EDI-Austauschs serialisiert.
* Transaktion Satz Header und Trailer Segmente gilt
* Generiert eine Austauschkontrollnummer eine Gruppenkontrollnummer und Transaktions-Reihe Steuerelement für jeden ausgehenden Austausch
* Trennzeichen in der Nutzlast ersetzt
* Überprüft EDI und Partner-Eigenschaften
    * -Schema-Validation Transaktion Satzdaten Elemente gegen die Nachricht Schema
    * EDI-Überprüfung auf Transaktion Satz von Datenelementen.
    * Erweiterte Validierung Transaktionssatz Daten
* Fordert eine Bestätigung technischen und funktionalen (falls konfiguriert).
    * Eine technische Bestätigung generiert durch Header-Validierung. Technische Bestätigung meldet den Status der Verarbeitung eine Austausch-Header und einem Nachspann Empfänger Adresse
    * Eine funktionale Bestätigung generiert durch Text-Validierung. Die funktionale Bestätigung meldet jeden Fehler während der Verarbeitung der empfangenen Dokuments

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket") 

