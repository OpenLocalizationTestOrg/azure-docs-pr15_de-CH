<properties
 pageTitle="Developer guide - Gerät im Vergleich zu verstehen | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - Verwendung Gerät im Vergleich zu Status und Konfiguration Daten zwischen IoT Hub und Ihre Geräte synchronisieren"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="understand-device-twins---preview"></a>Gerät im Vergleich zu verstehen – Vorschau

## <a name="overview"></a>Übersicht

*Gerät im Vergleich* sind JSON-Dokumente, die Gerätestatusinformationen (Metadaten, Konfigurationen und Zustände) zu speichern. IoT Hub besteht ein Gerät und für jede IoT Hub Verbindung Gerät. Dieser Artikel beschreibt:

* die Struktur der zwei Geräte: *Tags* *gewünschten* und *Eigenschaften gemeldet*, und
* die Vorgänge, die Gerät apps und Back-End auf Gerät im Vergleich ausführen können.

> [AZURE.NOTE] Zu diesem Zeitpunkt sind Geräte im Vergleich nur Verbindung IoT Hub mit dem Protokoll MQTT Geräte zugegriffen werden. Siehe [MQTT unterstützen] [ lnk-devguide-mqtt] Artikel Hinweise vorhandenen Gerät app konvertiert MQTT verwenden.

### <a name="when-to-use"></a>Verwendung

Gerät im Vergleich zu verwenden:

* Speichern Sie Gerät bestimmte Metadaten in der Cloud, z.B. Bereitstellungsspeicherort ein Automat.
* Aktuellen Status Berichtsinformationen verfügbaren Funktionen und Zustände Ihrer App Gerät, z. B. ein Gerät über Funk- oder Wifi.
* Synchronisieren Sie den Zustand des lang andauernde Workflows zwischen Gerät app und hinten, z.B. Backend angeben die neue Firmwareversion installieren und reporting der verschiedenen Phasen des Aktualisierungsvorgangs Gerät-app.
* Abfrage der Geräte-Metadaten, Konfiguration oder Zustand.

[Gerät-zu-Cloud] -Nachrichten[ lnk-d2c] für Zeitstempel Abläufe wie Zeitreihe von Daten oder Alarme. [Cloud-zu-Gerät-] Methoden[ lnk-methods] für die interaktive Steuerung von Geräten wie Lüfter einschalten.

## <a name="device-twins"></a>Gerät im Vergleich

Gerät im Vergleich zu Gerät Informationen speichern, die:

- Gerät und zurück enden können Geräte Konditionen und Konfiguration synchronisiert.
- Anwendung Back-End können Abfragen und Ziel lang andauernde Vorgänge.

Der Lebenszyklus von Gerät und das entsprechende [geräteidentität]verknüpft ist[lnk-identity]. Im Vergleich implizit erstellt und gelöscht, wenn neue geräteidentität erstellt oder IoT Hub gelöscht.

Gerät und ist ein JSON-Dokument enthält:

* **Tags**. Ein JSON-Dokument von Back-End gelesen und geschrieben. Tags werden nicht zum Gerät apps angezeigt.
* **Gewünschte Eigenschaften**. Zusammen mit gemeldeten verwendet synchronisieren Gerätekonfiguration oder -Bedingung. Eigenschaften können nur festgelegt werden von der Anwendung wieder Ende und App Gerät gelesen werden. Gerät app kann in Echtzeit auf die gewünschten Eigenschaften auch benachrichtigt werden.
* **Eigenschaften gemeldet**. Zusammen mit Eigenschaften verwendet zum Synchronisieren Gerätekonfiguration oder -Bedingung. Gemeldete Eigenschaften kann nur festgelegt werden, von der app Gerät lesen und Back-End Anwendung abgefragt.

Stamm des Geräts und enthält außerdem die schreibgeschützten Eigenschaften von entsprechenden Identität wie in [geräteidentitätsregistrierung][lnk-identity].

![][img-twin]

Hier ist ein Beispiel für ein Gerät und JSON-Dokument:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

In das Stammobjekt Systemeigenschaften und Containerobjekte für `tags` und `reported` und `desired properties`. Die `properties` Container enthält einige schreibgeschützte Elemente (`$metadata`, `$etag`, und `$version`) beschriebenen bzw. in den [Metadaten zwei] [ lnk-twin-metadata] und [vollständige Parallelität] [ lnk-concurrency] Abschnitte.

### <a name="reported-property-example"></a>Gemeldete-Eigenschaft (Beispiel)

Im obigen Beispiel enthält zwei Geräte einen `batteryLevel` Eigenschaft von Gerät app gemeldet wird. Diese Eigenschaft ermöglicht das Abfragen und auf Geräte basierend auf dem letzten gemeldeten Batteriestand. Ein weiteres Beispiel hätte app Bericht Gerät Gerätefunktionen oder Konnektivitätsoptionen.

Hinweis wie gemeldete Eigenschaften Szenarien, in denen das Back-End den letzten bekannten Wert einer Eigenschaft interessiert, zu vereinfachen. [Gerät-zu-Cloud] -Nachrichten[ lnk-d2c] Wenn das Back-End Gerät Telemetrie in Form von Zeitstempel Ereignisse wie Zeitreihe verarbeiten.

### <a name="desired-configuration-example"></a>Konfiguration-Beispiel

Im obigen Beispiel die `telemetryConfig` gewünschte und gemeldete Eigenschaften wird vom Back-End und Gerät app um Telemetrie-Konfiguration für dieses Gerät zu synchronisieren. Zum Beispiel:

1. App-Back-End wird die gewünschte Eigenschaft mit der gewünschten Konfiguration. Hier ist der Teil des Dokuments mit der gewünschten Eigenschaft:

        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
        
2. Gerät-app wird die Änderung sofort, wenn verbunden, oder die erste Verbindung benachrichtigt. App Gerät meldet aktualisierte Konfiguration (oder einen Fehler Zustand der `status` Eigenschaft). Hier ist der Teil des gemeldeten Eigenschaften:

        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...

3. App-Back-End bleiben Verfolgen der Ergebnisse der Konfigurationsvorgang auf vielen Geräten durch [Abfragen] [ lnk-query] im Vergleich.

> [AZURE.NOTE] Der oben stehenden Codeausschnitte sind Beispiele für die Lesbarkeit der Möglichkeit zum Codieren von Device-Konfiguration und den Status optimiert. IoT Hub erlegt ein bestimmtes Schema für die gewünschten und gemeldeten Eigenschaften im Gerät im Vergleich.

In vielen Fällen sind im Vergleich zur Synchronisation von zeitintensiver Vorgänge wie Firmware-Updates. [Verwenden Sie Eigenschaften zum Konfigurieren von Geräten] finden Sie[ lnk-twin-properties] Weitere Informationen zur Verwendung von Eigenschaften synchronisieren und Nachverfolgen lange Betrieb auf Geräten.

## <a name="back-end-operations"></a>Back-End-Betrieb

Back-End ist auf die mit folgenden atomaren Operationen über HTTP verfügbar gemacht:

1. **Zwei ID abzurufen**. Dieser Vorgang gibt der Inhalt der zwei Dokuments, einschließlich Tags und gegebenenfalls gemeldet und Systemeigenschaften.
2. **Teilweise und aktualisieren**. Dieser Vorgang ermöglicht Back-End teilweise die zwei Tags oder gewünschten Eigenschaften aktualisiert. Partielle Aktualisierung wird als JSON-Dokument angegeben, die hinzugefügt oder aktualisiert alle genannten Eigenschaften. Eigenschaften `null` werden entfernt. Folgende erstellt beispielsweise eine neue gewünschte Eigenschaft mit `{"newProperty": "newValue"}`, überschreibt den vorhandenen Wert `existingProperty` mit `"otherNewValue"`, und ganz `otherOldProperty`. Keine auftreten anderen vorhandenen gewünschten Eigenschaften oder Tags:

        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }

3. **Ersetzen Sie die gewünschte Eigenschaften**. Dieser Vorgang ermöglicht allen gewünschte Eigenschaften überschreiben und Ersetzen durch ein neues JSON-Dokument für Back-End `properties/desired`.
4. **Ersetzen von Tags**. Analog zum Ersetzen der gewünschten Eigenschaften dieser ermöglicht Back-End vollständig überschreiben alle vorhandenen Tags und ein neues Dokument JSON für `tags`.

Die oben aufgeführten Vorgänge unterstützt [vollständige Parallelität] [ lnk-concurrency] und **ServiceConnect** Berechtigung im [Security] [ lnk-security] Artikel.

Neben diesen Back-End kann die im Vergleich mit einer [Abfragesprache]wie SQL Abfragen[lnk-query], und Operationen für große im Vergleich [mit][lnk-jobs].

## <a name="device-operations"></a>Gerätevorgänge

Geräteanwendung arbeitet auf die folgenden atomaren Operationen mit:

1. **Zwei abrufen**. Dieser Vorgang gibt den Inhalt des Dokuments die zwei (einschließlich Tags und gewünscht, und Systemeigenschaften) für die derzeit verbundenen Gerät.
2. **Teilweise Aktualisierung Eigenschaften gemeldet**. Dieser Vorgang ermöglicht die teilweise Aktualisierung der Eigenschaften der gemeldeten derzeit verbundene Gerät. Diese verwendet das gleiche JSON Update Format als Back-End mit teilweise Aktualisierung der gewünschten Eigenschaften.
3. **Beobachten gewünschten Eigenschaften**. Das aktuell angeschlossene Gerät können Updates für die gewünschten Eigenschaften benachrichtigt werden, sobald sie auftreten. Das Gerät empfängt desselben Formulars aktualisieren (oder Austausch) von Back-End ausgeführt.

Die oben aufgeführten Vorgänge erfordern die Berechtigung **DeviceConnect** im [Security] [ lnk-security] Artikel.

[Azure IoT Gerät SDKs] [ lnk-sdks] erleichtern die obigen Sprachen und Plattformen verwenden. Weitere Informationen bezüglich der IoT Hub Primitives für die gewünschten Eigenschaften Synchronisierung finden Sie im [Gerät Verbindung Fluss][lnk-reconnection].

> [AZURE.NOTE] Derzeit sind Geräte im Vergleich nur Verbindung IoT Hub mit dem Protokoll MQTT Geräte zugegriffen werden.

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen zum Steuern des Zugriffs auf Ihre IoT Hub.

## <a name="tags-and-properties-format"></a>Format-Tags und Eigenschaften

Tags, gewünschte und gemeldete Eigenschaften sind JSON-Objekte mit folgender Einschränkung:

* Alle Schlüssel im JSON-Objekte sind groß-128 Zeichen Unicode-Zeichenfolgen. Zulässige Zeichen ausgeschlossen Unicode-Steuerzeichen (Segmente C0 und C1) und `'.'`, `' '`, und `'$'`.
* Alle Werte im JSON-Objekt kann der folgenden JSON: Boolean, Zahl, Zeichenfolge, Objekt. Arrays sind nicht zulässig.

## <a name="twin-size"></a>Zwei Größe

IoT Hub erzwingt ein 8KB begrenzt auf die Werte des `tags`, `properties/desired`, und `properties/reported`, schreibgeschützte Elemente ausschließen.
Berechnet die Größe zählen alle Zeichen ohne UNICODE Zeichen (Segmente C0 und C1) und Speicherplatz steuern `' '` erscheint außerhalb einer Zeichenfolgenkonstante.
IoT Hub lehnt mit Fehler alle, die diese Dokumente über das Limit vergrößern würde.

## <a name="twin-metadata"></a>Zwei Metadaten

IoT Hub verwaltet den Zeitstempel der letzten Aktualisierung für jedes JSON-Objekt in gewünschten und gemeldet. Die Zeitstempel in UTC und [ISO8601] -Format codiert `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Zum Beispiel:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Diese Informationen werden auf jeder Ebene (nicht nur die Blätter der JSON-Struktur) zur Aufbewahrung von Updates, die Objektschlüssel entfernen.

## <a name="optimistic-concurrency"></a>Vollständige Parallelität

Tags unterstützen Eigenschaften gewünschte und gemeldete Parallelität.
Tags haben ein Etag nach [RFC7232], die JSON-Repräsentation des Tags darstellt. Diese können bedingte Aktualisierung Vorgänge vom Back-End Sie Konsistenz.

Gewünschte und gemeldete Eigenschaften nicht besitzen Etags, aber ein `$version` garantiert inkrementellen Wert. Analog zu einem Etag Version aktualisieren Partei (Gerät app für gemeldete Eigenschaft) oder Back-End für eine gewünschte Eigenschaft lässt sich Konsistenz von Updates zu erzwingen.

Versionen sind auch hilfreich, wenn eine Beobachtung Agent (wie der Geräte-app verwendet und die gewünschten Eigenschaften) hat zu Rennen das Ergebnis einer Operation abrufen und eine Benachrichtigung. Im Abschnitt [Gerät Verbindung Fluss] [ lnk-reconnection] Weitere Informationen.

## <a name="device-reconnection-flow"></a>Gerät Verbindung Fluss

IoT Hub behält nicht die gewünschten Eigenschaften updatebenachrichtigungen für getrennte Geräte. Daraus folgt, dass ein Gerät verbindet Dokument alle gewünschten Eigenschaften neben Update-Benachrichtigung abonnieren abrufen muss. Angesichts der Möglichkeit der Rennen Updates verfügbar und voll abrufen, müssen die folgenden gewährleistet sein:

1. Gerät app Verbindung IoT Hub.
2. Gerät app abonniert nach Eigenschaften Updates verfügbar.
3. Gerät app Ruft das vollständige Dokument nach Eigenschaften.

Gerät app kann alle Meldungen ignorieren `$version` kleiner oder gleich Version das vollständige Dokument abgerufen. Dies ist möglich, da IoT Hub wird sichergestellt, dass Versionen immer erhöht.

> [AZURE.NOTE] Diese Logik ist bereits in [Azure IoT Gerät SDKs][lnk-sdks]. Diese Beschreibung ist nützlich, wenn Gerät app keine der Azure IoT Gerät SDKs verwenden und muss die MQTT Schnittstelle direkt programmieren.

## <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Nun haben Sie gelernt Gerät im Vergleich Sie in den folgenden Themen Entwicklerhandbuch möglicherweise:

- [Eine direkte Methode auf einem Gerät][lnk-methods]
- [Planen von Projekten auf mehreren Geräten][lnk-jobs]

Möchten Sie einige der in diesem Artikel beschriebenen Konzepte zu testen, können Sie die folgenden Lernprogramme IoT Hub interessiert.

- [Wie Sie das Gerät und][lnk-twin-tutorial]
- [Verwendung und Eigenschaften][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png