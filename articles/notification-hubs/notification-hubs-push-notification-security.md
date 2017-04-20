<properties
    pageTitle="Sicherheit für Notification Hubs"
    description="Dieses Thema erklärt Sicherheit für Azure Notification Hubs."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="security"></a>Sicherheit

##<a name="overview"></a>Übersicht

Dieses Thema beschreibt das Sicherheitsmodell von Azure Notification Hubs. Da Notification Hubs ein Service Bus sind, implementieren sie das gleiche Sicherheitsmodell wie Service Bus. Weitere Informationen finden Sie auf [Service Bus Authentifizierung](https://msdn.microsoft.com/library/azure/dn155925.aspx) .

##<a name="shared-access-signature-security-sas"></a>Sicherheit des freigegebenen Signatur (SAS) 

Benachrichtigung Hubs implementiert eine Entitätsebene Sicherheitsschema als SAS (SAS) bezeichnet. Dieses Schema ermöglicht messaging Einheiten bis zu 12 Autorisierungsregeln in ihrer Beschreibung deklarieren, die Zugriffsrechte auf diese Entität.

Jede Regel enthält einen Namen, einen Wert (gemeinsamer geheimer Schlüssel) und eine Reihe von rechten, wie im Abschnitt "Sicherheitsansprüche." Beim Erstellen einer Benachrichtigungshub werden zwei Regeln automatisch erstellt: eine mit Listen (die die Clientanwendung verwendet) und eine mit alle Rechte (die app Backend verwendet).

Bei Registrierung Management von Clientanwendungen, wenn Informationen über Benachrichtigung reagiert nicht (z. B. Wetterdaten), auf eine Benachrichtigungshub wird die Clientanwendung den Schlüsselwert der Regel nur hören zu und app Back-End-Schlüsselwert Regel Vollzugriff zu.

Einbetten den Schlüsselwert in Windows Store-Clientanwendungen wird nicht empfohlen. Einbetten des Schlüsselwertes vermeiden soll die Clientanwendung aus Backend app beim Start abrufen.

Es ist wichtig zu verstehen, dass Schlüssel mit Listen eine Clientanwendung für jedes Tag registrieren können. Wenn Ihre Anwendung Registrierung bestimmte Tags für bestimmte Clients (z. B. Einschränkung Wenn Benutzer-IDs stehen) müssen Ihre app Back-End-Registrierungen. Weitere Informationen finden Sie unter Registrierung Management. Beachten Sie, dass dabei die Clientanwendung Direktzugriff auf Benachrichtigungshubs wird.

##<a name="security-claims"></a>Sicherheitsansprüche

Wie andere Entitäten Notification Hub sind zulässig für drei Sicherheitsansprüche: hören, senden und verwalten.

| Forderung | Beschreibung | Zulässige Operationen |
|-------|-------------|--------------------|
| Überwachen | Erstellen/aktualisieren, lesen und Löschen von einzelnen erfassen | Registrierung erstellen/aktualisieren<br><br>Lesen der Registrierung<br><br>Lesen Sie aller Registrierungen für einen Ziehpunkt<br><br>Registrierung löschen |
| Senden | Senden von Nachrichten an den benachrichtigungshub | Nachricht senden |
| Verwalten | CRUDs Benachrichtigungshubs (inklusive PNS Anmeldeinformationen und Sicherheitsschlüssel aktualisieren) und Lesen Sie Einträge basierend auf tags | Erstellen/Aktualisieren/lesen/löschen benachrichtigungshubs<br><br>Registrierung von Tags gelesen |


Benachrichtigungshubs übernehmen Ansprüche gewährt Microsoft Azure Access Control-Token und Signaturtoken mit einem gemeinsamen Schlüssel direkt am Hub Benachrichtigung konfiguriert generiert.