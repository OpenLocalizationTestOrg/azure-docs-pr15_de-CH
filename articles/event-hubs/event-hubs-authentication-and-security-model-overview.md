<properties 
    pageTitle="Übersicht über Authentifizierung und Sicherheitsmodell Ereignis Hubs | Microsoft Azure"
    description="Ereignis-Hubs Authentifizierung und Übersicht."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/16/2016"
    ms.author="sethm;clemensv" />

# <a name="event-hubs-authentication-and-security-model-overview"></a>Ereignis-Hubs Authentifizierung und Übersicht

Das Sicherheitsmodell Ereignis Hubs erfüllt folgende:

- Nur Geräte, die gültige Anmeldeinformationen können Daten an einen Hub Ereignis senden.
- Ein Gerät kann nicht einem anderen Gerät imitieren.
- Senden von Daten an einen Hub-Ereignis kann ein bösartig agierendes Gerät gesperrt werden.

## <a name="device-authentication"></a>Geräteauthentifizierung

Event Hubs-Sicherheitsmodell basiert auf [Shared Access Signatur (SAS)](../service-bus-messaging/service-bus-shared-access-signature-authentication.md) -Token und *Ereignisherausgeber*aus. Ein Ereignisherausgeber definiert einen virtuellen Endpunkt einen Ereignis-Hub. Verleger kann nur Nachrichten mit einem Ereignis-Hub verwendet werden. Kann nicht zum Empfangen von Nachrichten von einem Verleger.

Normalerweise verwendet ein Ereignis Hub einen Publisher pro Gerät. Alle Nachrichten, die an einen Verleger Event Hub gesendet werden Warteschlange innerhalb dieser Ereignis-Hub. Herausgeber können detaillierte Zugriffskontrolle und Drosselung.

Jedes Gerät ist ein eindeutiges Token zugeordnet, die an das Gerät übertragen. Die Token entstehen, dass jede eindeutiges Token, das einen anderen eindeutigen Herausgeber Zugriff gewährt. Ein Gerät, das einen Token besitzt kann nur von einem Verleger, jedoch kein Verlag senden. Wenn mehrere Geräte gleichzeitig freigeben, teilt diesen Geräten Verleger.

Obwohl nicht empfohlen, ist möglich mit Token, die einem Ereignis-Hub direkten Zugriff gewähren. Jedes Gerät, das das token enthält kann direkt an den Hub-Ereignis senden. Gerät werden nicht Drosselung. Darüber hinaus kann nicht das Gerät gesperrt werden an diesen Hub Ereignis senden.

Alle Token sind mit SAS-Schlüssel signiert. In der Regel sind alle Token mit demselben Schlüssel signiert. Geräte sind nicht über den Schlüssel. Dadurch wird verhindert, dass Geräte Produktion Token.

### <a name="create-the-sas-key"></a>Erstellen Sie die SAS-Taste

Wenn einen Ereignis Hubs Namespace erstellen, generiert Azure Ereignis Hubs einen 256-Bit-SAS-Schlüssel mit dem Namen **RootManageSharedAccessKey**. Dieser Schlüssel gewährt senden, empfangen und Verwaltungsrechte für den Namespace. Sie können weitere Schlüssel erstellen. Es wird empfohlen, einen Schlüssel erstellen, gewährt Berechtigungen an bestimmten Event Hub senden. Im Rest dieses Themas wird davon ausgegangen, dass dieser Schlüssel mit dem Namen `EventHubSendKey`.

Das folgende Beispiel erstellt einen Schlüssel senden nur beim Hub Ereignis erstellen:

```
// Create namespace manager.
string serviceNamespace = "YOUR_NAMESPACE";
string namespaceManageKeyName = "RootManageSharedAccessKey";
string namespaceManageKey = "YOUR_ROOT_MANAGE_SHARED_ACCESS_KEY";
Uri uri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, string.Empty);
TokenProvider td = TokenProvider.CreateSharedAccessSignatureTokenProvider(namespaceManageKeyName, namespaceManageKey);
NamespaceManager nm = new NamespaceManager(namespaceUri, namespaceManageTokenProvider);

// Create Event Hub with a SAS rule that enables sending to that Event Hub
EventHubDescription ed = new EventHubDescription("MY_EVENT_HUB") { PartitionCount = 32 };
string eventHubSendKeyName = "EventHubSendKey";
string eventHubSendKey = SharedAccessAuthorizationRule.GenerateRandomKey();
SharedAccessAuthorizationRule eventHubSendRule = new SharedAccessAuthorizationRule(eventHubSendKeyName, eventHubSendKey, new[] { AccessRights.Send });
ed.Authorization.Add(eventHubSendRule); 
nm.CreateEventHub(ed);
```

### <a name="generate-tokens"></a>Token generieren

Sie können Token SAS-Taste. Sie müssen nur ein Token pro Gerät erstellen. Token können mithilfe der folgenden Methode erstellt werden. Alle Token sind mit der **EventHubSendKey** -Schlüssel generiert. Jedes Token erhält einen eindeutigen URI.

```
public static string SharedAccessSignatureTokenProvider.GetSharedAccessSignature(string keyName, string sharedAccessKey, string resource, TimeSpan tokenTimeToLive)
```

Beim Aufruf dieser Methode der URI angegeben werden `//<NAMESPACE>.servicebus.windows.net/<EVENT_HUB_NAME>/publishers/<PUBLISHER_NAME>`. Der URI für alle Token ist identisch, mit Ausnahme von `PUBLISHER_NAME`, sollten für jedes Token. Im Idealfall `PUBLISHER_NAME` stellt die ID des Geräts, das das Token empfängt.

Diese Methode generiert ein Token mit der folgenden Struktur:

```
SharedAccessSignature sr={URI}&sig={HMAC_SHA256_SIGNATURE}&se={EXPIRATION_TIME}&skn={KEY_NAME}
```

Ablauf des Tokens Zeit wird in Sekunden seit dem 1. Januar 1970 angegeben. Folgendes ist ein Beispiel für ein Token:

```
SharedAccessSignature sr=contoso&sig=nPzdNN%2Gli0ifrfJwaK4mkK0RqAB%2byJUlt%2bGFmBHG77A%3d&se=1403130337&skn=RootManageSharedAccessKey
```

Normalerweise haben die Token eine Lebensdauer, die entspricht oder übersteigt die Lebensdauer des Geräts. Wenn das Gerät die Möglichkeit, ein neues Token erhalten hat, können Token mit kürzeren Lebensdauer verwendet werden.

### <a name="devices-sending-data"></a>Geräte Senden von Daten

Nachdem die Token erstellt haben, wird jedes Gerät mit eigenen eindeutigen bereitgestellt.

Sendet das Gerät Daten in ein Ereignis, kennzeichnet das Gerät ihres Tokens mit der Anforderung senden. Um zu verhindern, dass ein Angreifer Lauschangriffe und Diebstahl Token muss über einen verschlüsselten Kanal die Kommunikation zwischen dem Gerät und dem Ereignis auftreten.

### <a name="blacklisting-devices"></a>Schwarze Liste Geräte

Wenn ein Token vom Angreifer gestohlen wird, kann der Angreifer das Gerät imitieren, deren Token gestohlen wurde. Reicht ein macht Gerät unbrauchbar, bis das Gerät ein neues Token erhält, das einen anderen Herausgeber verwendet.

## <a name="authentication-of-back-end-applications"></a>Authentifizierung von Back-End-Applikationen

Authentifizierung Back-End-Anwendung, die Daten von Geräten nutzen beschäftigt Ereignis Hubs ein Sicherheitsmodell, das ähnlich wie das Modell für Service Bus Topics verwendet wird. Verbrauchergruppe ein Ereignis Hubs entspricht ein Abonnement für ein Service Bus-Thema. Ein Client kann eine erstellen, Begleitung die Anforderung Consumer erstellen ein Token wird gewährt Berechtigungen verwalten, Ereignis-Hub oder Namespace Event Hub gehört. Ein Client kann Daten eine empfangsanforderung ein Token begleitet wird, die Berechtigung erhalten, Verbrauchergruppe, Event Hub oder Namespace Event Hub gehört verbrauchen.

Die aktuelle Version des Service Bus unterstützt SAS-Regeln nicht für einzelne Abonnements. Das gleiche gilt für Event Hubs Verbrauchergruppen. SAS wird beides in Zukunft unterstützt werden.

Keine Authentifizierung für einzelne Verbrauchergruppen SAS können SAS-Tasten alle Consumer-Gruppen mit einem gemeinsamen Schlüssel sichern. Dadurch kann eine Anwendung Daten aus Benutzergruppen einen Ereignis-Hub.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Ereignis-Hubs finden Sie in den folgenden Themen:

- [Übersicht über Hubs]
- Eine [Warteschlange Messaginglösung] Servicebuswarteschlangen verwenden.
- Eine vollständige [Anwendung mit Event Hubs].

[Übersicht über Hubs]: event-hubs-overview.md
[beispielanwendung, Ereignis Hubs verwendet]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Warteschlange Messaginglösung]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 
