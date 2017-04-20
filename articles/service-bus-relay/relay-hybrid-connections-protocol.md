<properties 
    pageTitle="Azure Relay-Hybrid-Verbindungen Protokoll | Microsoft Azure"
    description="Zure Relay Hybrid Verbindungen Protokoll Guide."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure Relay-Hybrid-Verbindungen Protokoll

Azure-Relay ist eine der Säulen Schlüsselfunktionen der Azure Service Bus-Plattform. Das Relay neue "Hybrid Connections" Funktion ist eine Entwicklung sicherer, offene Protokoll basiert auf HTTP und WebSockets.

Es ersetzt ehemaligen gleichermaßen namens "BizTalk-Dienste"-Funktion, die auf ein proprietäres Protokoll erstellt wurde. Die Integration der Hybrid Azure App Services wird weiterhin als-ist.

"Hybridverbindungen" erlaubt binären Datenstrom bidirektionale Kommunikation zwischen zwei vernetzten wobei einer oder beider Parteien hinter NATs oder Firewalls befinden.

Dieses Dokument beschreibt die clientseitige Interaktionen mit der Hybrid-Verbindung für die Verbindung von Clients mit Listener und Absender Rollen wie Listener neue Verbindungen akzeptiert.

## <a name="interaction-model"></a>Interaktionsmodell

Hybrid-Verbindungen Relay verbindet zwei Parteien mit Treffpunkt in Azure Cloud, die Parteien erkennen und aus eigenen Netzwerk herstellen können. Dieser Treffpunkt heißt "Hybridverbindung" und andere Dokumentation in den APIs und im Azure-Portal. Der Dienstendpunkt Hybrid-Verbindungen wird als "Service" für den Rest des Dokuments bezeichnet.

Das Interaktionsmodell stützt sich auf viele andere Netzwerk-APIs festgelegten Nomenklatur:

Es gibt ein Listener, der zuerst gibt Bereitschaft eingehend behandelt und anschließend eintreffen akzeptiert. Auf der anderen Seite ist ein verbundenen Client, der Listener erwartet dieser Verbindung akzeptiert einen bidirektionalen Kommunikationspfad verbinden. "Connect", "Warten", "Akzeptieren" gelten demselben finden Sie auf den meisten Socket-APIs.

Weitergeleitete Kommunikationsmodell hat Vertragspartei ausgehende Verbindungen zu einem Dienstendpunkt stellen "Listener" auch "Client" Umgangssprache verwendet und kann auch anderen Überladungen Terminologie. präzise Terminologie verwenden wir daher für hybride lautet wie folgt:

Programme auf beiden Seiten einer Verbindung werden "Client" bezeichnet, sind Clients den Dienst. Client, der wartet und Verbindungen akzeptiert, ist "Listener" oder die "Listener" fungieren soll. Der Client, der eine neue Verbindung zu einem Listener über den Dienst initiiert heißt "Sender" oder "Absender".

## <a name="listener-interactions"></a>Listener-Aktivitäten

Der Listener verfügt über vier Interaktionen mit dem Dienst; alle Kabel Details werden weiter unten in diesem Dokument im Referenzabschnitt beschrieben.

### <a name="listen"></a>Überwachen

Bereitschaft zum Dienst an, die ein Listener erstellt Verbindungen akzeptieren eine Socketverbindung ausgehende Web. Verbindungshandshake trägt den Namen einer Hybrid-Verbindung konfiguriert den Relay-Namespace und ein Sicherheitstoken, das rechts verleiht "Spielen", der name. Wenn Web Socket vom Dienst akzeptiert wird, die Registrierung und bestehende Web Sockets werden als "Steuerungskanal" für alle nachfolgenden Interaktionen ermöglichen. Der Dienst ermöglicht bis zu 25 gleichzeitige Listener Hybrid-Verbindung. Sind 2 oder mehr aktive Listener, werden eingehend über sie in zufälliger Reihenfolge ausgeglichen; gerechte ist nicht gewährleistet.

### <a name="accept"></a>Akzeptieren

Wenn ein Absender eine neue Verbindung für den Dienst geöffnet wird, wird der Dienst auswählen und Benachrichtigen eines aktiven Listener Hybrid-Verbindung. Die Benachrichtigung wird über die offenen Kanal an den Listener gesendet, als JSON-Nachricht mit dem URL der Web Sockets Endpunkt, den der Listener zum Akzeptieren der Verbindungs herstellen muss.

Die URL kann und muss direkt vom Listener ohne zusätzliche Bearbeitung verwendet; die codierte Informationen gilt nur kurzer Zeit im Wesentlichen so lang wie der Sender für die Verbindung festgelegten End-to-End, aber maximal 30 Sekunden warten. Die URL kann nur für eine erfolgreiche Verbindung verwendet werden. Sobald die Web Sockets mit Rendezvous URL Verbindung ist alle weiterer Aktivitäten auf diesem Socket Web von und an den Absender ohne Eingriff Interpretation durch den Dienst weitergeleitet.

### <a name="renew"></a>Erneuern 

Das Sicherheitstoken zu registrieren des Listeners des Steuerungskanals herangezogen werden möglicherweise ablaufen, während der Listener aktiv ist. Der Ablauf wirkt sich nicht auf die laufende Verbindung wird, aber es der Steuerungskanal vom Dienst am oder nach Ablauf Instant gelöscht werden. Die Aktion "Erneuern" ist eine JSON-Nachricht, die der Listener senden kann, um dem Kanal zugeordnete Token ersetzen, sodass der Kanal für längere Zeit beibehalten werden kann.

### <a name="ping"></a>Ping 

Bleibt der Steuerungskanal im Leerlauf lange Zwischenstufen auf dem Weg können wie Load Balancers oder NATs die TCP-Verbindung löschen. Die Aktion "Ping" vermieden werden, senden Sie eine kleine Datenmenge auf den Kanal, der erinnert alle Netzwerk-Route, die die Verbindung aktiv sein soll, und es dient auch als Test Verfügbarkeit für den Listener. Wenn der Ping fehlschlägt, der Steuerungskanal dürfte unbrauchbar und Listener verbinden soll.

## <a name="sender-interaction"></a>Absender-Interaktion

Der Absender hat nur eine einzelne Interaktion mit dem Dienst verbunden;

### <a name="connect"></a>Verbinden

Die Aktion "connect" öffnet einen Socket Web Service mit dem Namen der Hybrid-Verbindung und eine (optionale jedoch standardmäßig erforderlich) Sicherheitstoken eine Berechtigung "" in der Abfragezeichenfolge. Der Dienst des Listeners die oben beschriebene Weise interagieren und haben den Listener Rendezvous-Verbindung erstellen, die mit diesem Web verknüpft werden. Nach der Annahme des Sockets Web werden Web Socket alle Weitere Interaktionen mit einem verbundenen Listener.

## <a name="interaction-summary"></a>Aktivität-Zusammenfassung

Das Ergebnis dieser Interaktionsmodell ist, dass der Absender Client aus der Handshake mit einem "sauberen" Web an einen Listener verbunden ist und benötigt, die keine weiteren präambeln oder Vorbereitung. Dadurch können praktisch alle vorhandenen Web Client Socketimplementierung zu leicht Hybridverbindungen Service durch Bereitstellen einer richtig aufgebauten URL in ihren Web-Socketebene Client.

Rendezvous Verbindung der Listener durch Interaktion annehmen erhält Web-Socket sauber ist und alle vorhandenen Web Server Socketimplementierung einige minimale zusätzliche Abstraktion, die zwischen "akzeptieren" auf ihren Rahmen LAN und Hybrid-Verbindungen remote 'annehmen' übergeben werden.

## <a name="protocol-reference"></a>Protokoll-Referenz

Dieser Abschnitt beschreibt die Details der oben beschriebenen Protokoll-Aktivitäten.

Die Web-Socketverbindungen Port 443 als Upgrade von HTTPS 1.1 erfolgen die häufig von einigen Web-Socket-Framework oder API abstrahiert. Die Beschreibung Implementierung neutral, bleibt ohne ein bestimmtes Framework.

## <a name="listener-protocol"></a>Listener-Protokoll

Listener-Protokoll besteht aus zwei Verbindung Gesten und drei Nachrichtenvorgänge.

### <a name="listener-control-channel-connection"></a>Listener-Steuerelement Channel-Verbindung

Mit Web-Socket-Verbindung zum Erstellen der Steuerungskanal geöffnet:

*Wss: / / {Namespace-Adresse} /* *$servicebus* */* *Hybridconnection /**{Path}? Sb-Hc-Action =... & Sb-Hc-Id =... & Sb-Hc-Token =... "*

Die *Namespace-Adresse* den vollqualifizierten Domänennamen des Azure Relay-Namespace, der Hybrid-Verbindung von der Form befindet ist {*Myname}. servicebus.windows.net.*

Optionen für den Abfrage-Parameter werden wie folgt

| Param        | Erforderlich? | Beschreibung                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-Hc-Aktion | Ja       | Die Listener-Funktion der Parameter muß **Sb-Hc-Action = Listen**                                                                                                                                |
| {Path}       | Ja       | Der URL-codierte Namespacepfad vorkonfigurierte Hybrid Verbindung mit diesem Listener auf registrieren. Dieser Ausdruck angefügt, die * **$servicebus**/**Hybridconnection /*** Pfad. |
| SB-Hc-token  | Ja\*     | Der Listener geben eine gültige, URL-codierte Service Bus freigegebene Zugriffstoken Namespace oder Hybrid-Verbindung, die **hören** Recht verleiht.                                           |
| SB-Hc-id     | Nein        | Dieser Client bereitgestellte optionale ID ermöglicht End-to-End-Diagnoseprotokollierungstypen.                                                                                                                             |

Ausfall von Socketverbindung Hybrid Verbindungspfad nicht registriert, oder ein ungültiger oder fehlender Token oder ein anderer Fehler Fehler Feedback erhalten die reguläre HTTP 1.1 Status Feedback Modell. Beschreibung des enthält einer Tracking-Id-Fehler, die Azure-Unterstützung mitgeteilt werden können:

| Code | Fehler          | Beschreibung                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nicht gefunden      | Der Hybrid-Verbindung- **Pfad** ist ungültig oder die base-URL ist ungültig |
| 401  | Nicht autorisiert   | Das Token ist nicht vorhanden oder fehlerhaft oder ungültig                  |
| 403  | Verboten      | Das Token ist ungültig für diesen Pfad für diese Aktion          |
| 500  | Interner Fehler | Fehler des Dienstes                                    |

Wenn die Socketverbindung Web absichtlich vom Dienst heruntergefahren nachdem es anfangs eingerichtet wurde, den Grund dafür mit einer entsprechenden Web Socket-Fehlercode sowie eine beschreibende Fehlermeldung, die auch eine Tracking-Id mitgeteilt wird. Der Dienst wird auf nicht der Steuerungskanal heruntergefahren, ohne einen Fehlerzustand. Ordnungsgemäßes Herunterfahren ist Client gesteuert.

| WS-Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Hybride Verbindungspfad wurde gelöscht oder deaktiviert.                           |
| 1008      | Das Token ist abgelaufen und wird daher die Autorisierungsrichtlinie verletzt. |
| 1011      | Etwas schief innerhalb des Dienstes.                                           |

### <a name="accept-handshake"></a>Handshake akzeptieren 

Accept-Benachrichtigung wird vom Dienst an dem Listener über den zuvor festgelegten Kanal als JSON-Nachricht in einem Textrahmen Web Sockets gesendet. Es gibt keine Antwort auf diese Nachricht.

Die Nachricht enthält ein JSON-Objekt mit dem Namen "akzeptieren", die zurzeit die folgenden Eigenschaften definiert:

-   **Adresse** – der URL-Zeichenfolge für die Herstellung von Socket an den Dienst verwendet werden, um eine eingehende Verbindung anzunehmen.

-   **Id** -Bezeichner für diese Verbindung. Wenn die Id vom Absender Client bereitgestellt wurde, den Absender angegebene Wert ist andernfalls einen Wert vom System generiert wird.

-   **ConnectHeaders** – alle HTTP-Header, der an den Relay-Endpunkt vom Absender bereitgestellten Sec-WebSocket-Protokoll und die Sec-WebSocket-Extensions-Header.

| Nachricht akzeptieren                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

Die Adresse URL in JSON-Nachricht wird vom Listener Web Socket zum Akzeptieren oder Ablehnen des Absender Sockets herstellen verwendet.

#### <a name="accepting-the-socket"></a>Akzeptieren des Sockets

Um anzunehmen, stellt der Listener WebSocket-Verbindung an die angegebene Adresse.

Beachten Sie die Probezeit Adresse URI können eine bare IP-Adresse und das TLS-Zertifikat vom Server kann nicht auf die Adresse. Diese werden bei der Vorschau korrigiert.

Die Meldung "akzeptieren" "Sec-WebSocket-Protocol" Header enthalten, dürfte Listener nur WebSocket akzeptiert wird, wenn dieses Protokoll unterstützt und wird den Header wie WebSocket ist.

Das gleiche gilt für den Header "Sec-WebSocket-Extensions". Wenn das Framework eine Erweiterung unterstützt, sollte es *Seite Serverantwort erforderlich "Sec-WebSocket-Extensions" Handshake für die Erweiterung* der Header festgelegt.

Die URL verwendet werden-für Socket Accept herstellen, aber die folgenden Parameter:

| Param        | Erforderlich? | Beschreibung                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-Hc-Aktion | Ja       | Für einen Socket akzeptieren muss der Parameter **Sb-Hc-Action = übernehmen**                                                                                                                                                                                                                          |
| {Path}       | Ja       | Der URL-codierte Namespacepfad vorkonfigurierte Hybrid Verbindung mit diesem Listener auf registrieren. Dieser Ausdruck angefügt, die * **$servicebus**/**Hybridconnection /*** Pfad.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB-Hc-Id | No        | Beschreibung der **Id** oben anzeigen                                                                                                                                                                                                                                                              |

Ein Fehler vorliegt, kann der Dienst wie folgt Antworten:

| Code | Fehler          | Beschreibung                         |
|------|----------------|-------------------------------------|
| 403  | Verboten      | Die URL ist ungültig.               |
| 500  | Interner Fehler | Fehler des Dienstes |

Nach dem Herstellen der Verbindung wird der Server Web Sockets Herunterfahren, wenn Absender Web Sockets nach unten oder mit dem folgenden Status wird

| WS-Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Absender-Client beendet die Verbindung                                        |
| 1001      | Hybride Verbindungspfad wurde gelöscht oder deaktiviert.                           |
| 1008      | Das Token ist abgelaufen und wird daher die Autorisierungsrichtlinie verletzt. |
| 1011      | Etwas schief innerhalb des Dienstes.                                           |

#### <a name="rejecting-the-socket"></a>Den Socket ablehnen

Ablehnen des Sockets nach die "Annehmen" Nachrichten erforderlich, ähnlichen Handshake Statuscode und Beschreibung des Kommunikation des Grund für die Ablehnung an den Absender übertragen können.

Die Protokoll-Entwurfsoption soll einen WebSocket-Handshake (die so definierten Fehlerzustand enden), damit Listener Client Implementationen können weiterhin auf eine WebSocket-Client, und müssen nicht Extra beschäftigen, bare HTTP-Client.

Um den Sockel abzulehnen, der Client die Adresse URI aus der Meldung "akzeptieren" und fügt zwei Abfragezeichenfolgen-Parameter:

| Param             | Erforderlich? | Beschreibung                             |
|-------------------|-----------|-----------------------------------------|
| statusCode        | Ja       | Numerische HTTP-Statuscode                |
| statusDescription | Ja       | Menschen lesbaren Grund für die Ablehnung |

Der resultierende URI wird eine WebSocket-Verbindung verwendet. Beachten Sie wieder, die TLS-Zertifikat nicht entsprechen kann die Adresse während der Vorschau haben Überprüfung kann deaktiviert werden.

Bei korrekt fehl Handshakes absichtlich mit HTTP-Fehlercode 410, seit Beginn keine WebSocket. Wenn ein Fehler auftritt, sind die Optionen:

| Code | Fehler          | Beschreibung                         |
|------|----------------|-------------------------------------|
| 403  | Verboten      | Die URL ist ungültig.               |
| 500  | Interner Fehler | Fehler des Dienstes |

### <a name="listener-token-renewal"></a>Listener token erneuern

Wenn der Listener Token abläuft, kann es es SMS Frame zum Dienst über den Kanal etablierten ersetzen. Die Nachricht enthält ein JSON-Objekt mit dem Namen "RenewToken", die zu diesem Zeitpunkt die folgende Eigenschaft definiert:

-   **token** – eine gültige, URL-codierte Service Bus freigegebene Zugriffstoken für den Namespace oder Hybrid-Verbindung, die **hören** Recht verleiht.

| RenewToken Nachricht                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Wenn token Prüfungsfehler, Zugriff verweigert und Cloud-Dienst Websocket Kanal Steuerelement mit einem Fehler schließen, gibt es keine Antwort.

| WS-Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Das Token ist abgelaufen und wird daher die Autorisierungsrichtlinie verletzt. |

<a name="sender-protocol"></a>Absender-Protokoll
---------------

Absender-Protokoll entspricht effektiv wie ein Listener eingerichtet wird. Das Ziel ist Transparenz für den End-to-End-Web-Socket. Entspricht die Adresse zum Herstellen der Listener aber "Aktion" unterscheidet und das Token benötigt eine andere Berechtigung:

*Wss: / / {Namespace-Adresse} /* *$servicebus* */* *Hybridconnection /**{Path}? Sb-Hc-Action =... & Sb-Hc-Id =... \[& Sbc-Hc-Token =... \]*

Die *Namespace-Adresse* den vollqualifizierten Domänennamen des Azure Relay-Namespace, der Hybrid-Verbindung von der Form befindet ist {*Myname}. servicebus.windows.net.*

Die Anforderung kann beliebige zusätzliche Header auch anwendungsspezifische enthalten. Alle angegebenen Header an den Listener fließen und auf das Objekt "ConnectHeader" Die Kontrollnachricht "akzeptieren".

Optionen für den Abfrage-Parameter werden wie folgt

| Param        | Erforderlich? | Beschreibung                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB-Hc-Aktion | Ja       | Die Listener-Funktion der Parameter muß **Aktion = Verbindung**                                                                                                                                                    |
| {Path}       | Ja       | Der URL-codierte Namespacepfad vorkonfigurierte Hybrid Verbindung mit diesem Listener auf registrieren.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB-Hc-Token | Yes\*     | Der Listener geben eine gültige, URL-codierte Service Bus freigegebene Zugriffstoken für Namespace oder Hybrid-Verbindung, die das Recht **Senden** verleiht.                                                            | | SB-Hc-Id | No        | Eine optionale ID, die End-to-End-Diagnoseprotokollierungstypen und stehen in dem Listener während des Handshakes annehmen.                                                                                       |

Ausfall von Socketverbindung Hybrid Verbindungspfad nicht registriert, oder ein ungültiger oder fehlender Token oder ein anderer Fehler Fehler Feedback erhalten die reguläre HTTP 1.1 Status Feedback Modell. Beschreibung des enthält einer Tracking-Id-Fehler, die Azure-Unterstützung mitgeteilt werden können:

| Code | Fehler          | Beschreibung                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nicht gefunden      | Der Hybrid-Verbindung- **Pfad** ist ungültig oder die base-URL ist ungültig |
| 401  | Nicht autorisiert   | Das Token ist nicht vorhanden oder fehlerhaft oder ungültig                  |
| 403  | Verboten      | Das Token ist ungültig für diesen Pfad für diese Aktion          |
| 500  | Interner Fehler | Fehler des Dienstes                                    |

Wenn die Socketverbindung Web absichtlich vom Dienst heruntergefahren nachdem es anfangs eingerichtet wurde, den Grund dafür mit einer entsprechenden Web Socket-Fehlercode sowie eine beschreibende Fehlermeldung, die auch eine Tracking-Id mitgeteilt wird.

| WS-Status | Beschreibung                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | Der Listener heruntergefahren Sockel.                                                 |
| 1001      | Hybride Verbindungspfad wurde gelöscht oder deaktiviert.                           |
| 1008      | Das Token ist abgelaufen und wird daher die Autorisierungsrichtlinie verletzt. |
| 1011      | Etwas schief innerhalb des Dienstes.                                           |

## <a name="next-steps"></a>Nächste Schritte:

- [Relay – häufig gestellte Fragen](relay-faq.md)
- [Erstellen eines Namespaces](relay-create-namespace-portal.md)
- [Erste Schritte mit .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Erste Schritte mit Knoten](relay-hybrid-connections-node-get-started.md)