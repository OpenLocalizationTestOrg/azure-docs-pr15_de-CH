<properties
    pageTitle="Übersicht über Logik Apps Connectors | Microsoft Azure"
    description="Übersicht über Connectors in eine Logik-app verwendet werden können"
    services=""
    documentationCenter="" 
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="using-connectors-in-a-logic-app"></a>Verwenden von Connectors in einer Logik-app

Connectors bieten schnellen Zugriff auf Ereignisse, Daten und Aktionen für Dienste, Protokolle und Plattformen.  Die vollständige Liste der Anschlüsse, die Logik Apps unterstützt können [finden Sie hier](apis-list.md).  Connectors kann als Trigger oder eine Aktion eine Logik-app verwendet und erfordern eine konfigurierte *Verbindung* verwenden (zum Beispiel: Autorisierung Twitter-Konto zuzugreifen oder in Ihrem Auftrag buchen).

## <a name="basics"></a>Grundlagen

Connectors sind gehosteten Dienste, die Sie als Teil einer Logik app integrieren mit anderen Dynamics Azure Salesforce [mehr](apis-list.md)zugreifen können.  Sie werden bereitgestellt und von Microsoft, können Sie Ihre Workflows Integration mit Skalierung, Durchsatz und Sicherheit gesorgt.  Sie können einen Connector Logik app hinzufügen suchen und Auswählen eines Connector-Aktion oder Trigger unter **Microsoft anzeigen verwaltete APIs**.

![Kontextmenü für Trigger auswählen][1]

Jeder Connector Aktion oder Trigger haben einen Satz von Eigenschaften konfigurieren.  Klicken auf die Schaltfläche Aktion kennen oder Verweis der Dokumentation [erfahren](apis-list.md).

Wenn ein Dienst oder eine nicht noch einen Connector können Logik apps über einen [benutzerdefinierten Connector](../app-service-logic/app-service-logic-create-api-app.md) erweitern oder rufen Sie direkt an den Dienst über ein Protokoll wie HTTP API integriert werden soll.

## <a name="triggers"></a>Trigger

Einige Anschlüsse haben einen Trigger ein Ereignis von diesem Connector wird ausgelöst, eine Anwendung Logik und Daten als Teil der Trigger übergeben.  Ein Trigger ist immer zuerst eine Logik-app.  Beliebte Trigger gehören Vorgänge wie:
 
 * Wiederholung - stündlich ausführen
 * Wenn eine HTTP-Anforderung empfangen wird
 * Wenn ein Element in einer Warteschlange hinzugefügt
 * Wenn eine e-Mail
 
Einige Trigger Instant kommt ein Ereignis über eine Benachrichtigung an die Anwendung Logik und andere benötigen ein Serienintervall konfiguriert wie oft die Anwendung Logik Service für ein Ereignis (bis zu 15 Sekunden) überprüfen.  

Wenn ein Ereignis empfangen wird, wird Logik app ausgeführt und Aktionen im Workflow werden gestartet.  Sie werden auch auf den Trigger im Workflow Daten zugreifen (z. B. Trigger "auf einen neuen Tweet' Tweet in der Ausführung übergibt).

## <a name="actions"></a>Aktionen

Die meisten Connectors verfügen ein oder mehrere Aktionen, die als Teil des Workflows ausgeführt werden können.  Aktionen sind Maßnahmen geschehen nach die Ausführung von einem Trigger ausgelöst.  Um eine Aktion klicken Sie auf die **Neue** Schaltfläche und suchen für den Connector Sie verwenden möchten.  Aktiviert (und nach dem Konfigurieren von [Verbindungen](#connections) , die möglicherweise erforderlich) sehen Sie die aktionskarte können Sie konfigurieren.  Sie können Daten aus den vorherigen Schritten Token für Ausgaben auf, oder in einer anderen Konfiguration geben Sie bei Bedarf.

![Aktion Connector konfigurieren][2]

## <a name="connections"></a>Anschlüsse

Die meisten Connectors müssen Sie eine *Verbindung* konfigurieren, bevor Sie den Connector verwenden können.  Eine *Verbindung* ist Anmeldung oder Verbindung erforderliche Konfiguration für den Connector zugreifen.  Connectors, die OAuth verwenden, erstellen Sie eine Verbindung bedeutet Service (wie Office 365, Salesforce oder GitHub) wo der Zugriffstoken verschlüsselt und sicher in einem Azure geheimen Informationsspeicher anmelden.  Andere Connectors (wie FTP und SQL) erfordern eine Verbindung, die Konfiguration wie Adresse, Benutzername und Kennwort enthält.  Dieser Konfigurationsdetails Verbindung auch verschlüsselt und gespeichert.  Anschlüsse werden auf den Dienst zugreifen, solange der Dienst ermöglicht.  Azure Active Directory OAuth-Verbindungen (wie Office 365 und Dynamics) können wir weiterhin das Zugriffstoken unbegrenzt aktualisieren.  Andere Dienste können setzen Grenzen wie lange es ohne aktualisiert wird einen Token verwenden kann.  Bestimmte Aktionen wie das Ändern des Kennworts werden grundsätzlich alle Zugriffstoken ungültig.  

Verbindungen können angezeigt und durch Klicken auf **Durchsuchen** und **API-**Anschlüsse in Azure verwaltet werden.  Aus der API-Verbindungen können Sie anzeigen, bearbeiten, aktualisieren oder erneut erstellten Verbindung zu autorisieren.

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen Sie Ihre erste Anwendung Logik](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [Erläutert häufig verwendet und Logik apps-Beispiele](../app-service-logic/app-service-logic-examples-and-scenarios.md)
- [Erste Schritte mit Enterprise Integration Auslöser und Aktionen](../app-service-logic/app-service-logic-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png