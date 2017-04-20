<properties 
    pageTitle="Erfahren Sie mehr über Enterprise Integrationspaket decodieren AS2 Nachricht Connctor | Microsoft Azure App Service | Microsoft Azure" 
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

# <a name="get-started-with-decode-as2-message"></a>Erste Schritte mit AS2-Nachricht entschlüsseln

Verbinden Sie mit AS2-Nachricht, Sicherheit und Zuverlässigkeit beim Übertragen von Nachrichten entschlüsseln. Es bietet digitale Signaturen, Entschlüsselung und Empfangsbestätigungen über Message Disposition Benachrichtigungen (MDN).

## <a name="create-the-connection"></a>Erstellen der Verbindung

### <a name="prerequisites"></a>Erforderliche Komponenten

* Ein Azure-Konto; Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.

* Ein Konto Integration muss decodieren AS2 Nachricht Connector verwenden. Einzelheiten Sie zum [Konto Integration](./app-service-logic-enterprise-integration-create-integration-account.md), [Partner](./app-service-logic-enterprise-integration-partners.md) und [AS2-Vereinbarung](./app-service-logic-enterprise-integration-as2.md) erstellen

### <a name="connect-to-decode-as2-message-using-the-following-steps"></a>Verbinden Sie mit decodieren AS2-Nachricht mit den folgenden Schritten:

1. [Erstellen einer Anwendung Logik](./app-service-logic-create-a-logic-app.md) enthält ein Beispiel.

2. Dieser Connector keinen Trigger. Verwenden Sie andere Trigger zum Starten der Anwendung Logik wie Anforderung Trigger.  Im Designer Anwendung Logik Hinzufügen eines Triggers und eine Aktion hinzufügen.  Wählen Sie zeigen Microsoft verwalteten APIs in der Dropdown-Liste und geben Sie dann im Suchfeld "AS2".  Wählen Sie AS2-AS2-Nachricht entschlüsseln

    ![AS2 suchen](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage1.png)

3. Wenn Sie zuvor alle Verbindungen Integration Konto erstellt haben, werden Sie die Details der Verbindung aufgefordert

    ![Integration Verbindung](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage2.png)

4. Geben Sie die Details Integration.  Eigenschaften mit einem Sternchen sind erforderlich

  	| Eigenschaft   | Details |
  	| --------   | ------- |
  	| Verbindungsname *    | Geben Sie einen beliebigen Namen für die Verbindung |
  	| Integration Konto * | Geben Sie den Kontonamen Integration. Die Integration Konto und Logik-app in Azure dort sind |

    Abschließend ähneln die Verbindungsdetails folgende

    ![Integration Verbindung](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage3.png)

5. Wählen Sie **Erstellen**
    
6. Beachten Sie, dass die Verbindung hergestellt wurde.  Nun mit den Schritten in Ihrer Anwendung Logik

    ![Integration Verbindung erstellt](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage4.png) 

7. Anforderung Ausgaben Body und-Header auswählen

    ![obligatorische Felder bereitstellen](./media/app-service-logic-enterprise-integration-AS2connector/as2decodeimage5.png) 

## <a name="the-as2-decode-does-the-following"></a>AS2 decodiert werden folgende

* AS2-HTTP-Header verarbeitet
* Überprüft die Signatur (falls konfiguriert)
* Entschlüsselt die Nachrichten (falls konfiguriert)
* Dekomprimiert die Nachricht (falls konfiguriert)
* Empfangene MDN mit der ursprünglichen ausgehenden Nachricht gleicht
* Aktualisiert und Korrelation von Datensätzen in der Datenbank Zulassung
* Schreibt Datensätze für AS2-Statusberichte
* Ausgabe-Nutzlast Inhalt werden base64-codiert
* Bestimmt, ob ein MDN erforderlich ist, und ob die MDN synchrone sollte oder asynchrone Konfiguration in AS2-Vereinbarung
* Generiert eine synchrone oder asynchrone MDN (basierend auf Vereinbarung Konfigurationen)
* Legt das Korrelationstoken Eigenschaften auf die MDN

##<a name="try-it-for-yourself"></a>Probieren Sie es selbst

Probieren Sie es ein. Klicken Sie auf [hier](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/) eine betriebsbereites Logik app auf eigene Logik Apps AS2-Funktionen bereitstellen 

## <a name="next-steps"></a>Nächste Schritte

[Erfahren Sie mehr über Enterprise-Integrationspaket] (./app-service-logic-enterprise-integration-overview.md "Erfahren Sie mehr über Enterprise-Integrationspaket") 