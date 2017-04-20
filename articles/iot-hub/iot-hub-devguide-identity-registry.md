<properties
 pageTitle="Entwicklerhandbuch - geräteidentitätsregistrierung | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch - Beschreibung die Registrierung des Geräts Identität und Ihre Geräte verwalten"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="manage-device-identities-in-iot-hub"></a>Verwalten von Identitäten in IoT Hub Gerät

## <a name="overview"></a>Übersicht

Jede IoT Hub ist eine Gerät identitätsregistrierung, die speichert Informationen über die Geräte, die mit dem Hub verbinden dürfen. Bevor Sie ein Gerät an einen Hub anschließen kann, muss ein Eintrag für das Gerät in der Hub-Gerät identitätsregistrierung sein. Ein Gerät muss auch mit dem Hub basierend auf Gerät identitätsregistrierung gespeicherten Anmeldeinformationen authentifizieren.

Auf einer hohen Ebene besteht die Registrierung des Geräts Identität REST-fähigen Identität Geräteressourcen. Beim Hinzufügen eines Eintrags in der Registrierung erstellt IoT Hub einen Satz pro Gerät Ressourcen im Dienst wie die Warteschlange, die Cloud-zu-Gerät-Flight-Nachrichten enthält.

### <a name="when-to-use"></a>Verwendung

Verwenden Sie die Registrierung des Geräts Identität müssen Bereitstellung Geräte die Verbindung IoT Hub und Sie pro Gerät Zugriff auf Geräte mit Endpunkten im Hub steuern möchten.

> [AZURE.NOTE] Die Registrierung des Geräts Identität enthält keine anwendungsspezifischen Metadaten.

## <a name="device-identity-registry-operations"></a>Gerätevorgänge Identity-Registrierung

IoT Hub Gerät identitätsregistrierung stellt die folgenden Vorgänge:

* Gerät Identität
* Geräteidentität aktualisieren
* Identität des Geräts nach ID abrufen
* Geräteidentität löschen
* Bis zu 1000 Identitäten auflisten
* Alle Identitäten in Azure BLOB-Speicher exportieren
* Importieren von Identitäten von Azure BLOB-Speicher

Diese Vorgänge können vollständige Parallelität als im [RFC7232][lnk-rfc7232].

> [AZURE.IMPORTANT] Ist die einzige Möglichkeit, alle Identitäten in einem Hub identitätsregistrierung abrufen [Exportieren] verwenden[ lnk-export] Funktionen.

Ein IoT Hub-geräteidentitätsregistrierung:

- Enthält alle Metadaten für die Anwendung.
- Wie ein Wörterbuch erfolgt mithilfe der **Geräte-ID** als Schlüssel.
- Beeindruckende Abfragen unterstützt nicht.

IoT-Lösung ist in der Regel einen separaten lösungsspezifische Speicher, der anwendungsspezifischen Metadaten enthält. Beispielsweise würde lösungsspezifische Speicher in einer Projektmappe erstellen smart Raum aufzeichnen in dem Temperatursensor bereitgestellt wird.

> [AZURE.IMPORTANT] Verwenden Sie die Registrierung des Geräts Identität nur für Device-Management und provisioning Operationen. Hoher Durchsatz Vorgänge zur Laufzeit abhängen nicht Vorgänge in die Registrierung des Geräts Identität. Überprüfen den Verbindungsstatus eines Geräts vor dem Senden eines Befehls ist z. B. nicht unterstützte Muster. [Drosselung Sätze] überprüfen[ lnk-quotas] für die Registrierung des Geräts Identität und [Gerät Takt] [ lnk-guidance-heartbeat] Muster.

## <a name="disable-devices"></a>Deaktivieren Sie Geräte

Aktualisieren die **Status** -Eigenschaft einer Identität in der Registrierung, um Geräte zu deaktivieren. Normalerweise verwenden Sie diese Eigenschaft in zwei Szenarien:

- Während einer Bereitstellung Orchestrierung. Weitere Informationen finden Sie unter [Bereitstellung Gerät][lnk-guidance-provisioning].
- Wenn aus irgendeinem Grund betrachten ein gefährdet ist oder nicht autorisierte geworden.

## <a name="import-and-export-device-identities"></a>Importieren und Exportieren von Geräte-Identitäten

Exportieren Gerät Identitäten in Massen aus einem IoT Hub identitätsregistrierung durch Verwendung asynchroner Vorgänge [IoT Hub Ressource Anbieterendpunkt][lnk-endpoints]. Exporte werden Aufträge mit langer Ausführung, mit denen Kunden bereitgestellten BLOB-Container Lesen aus der identitätsregistrierung Identity Gerätedaten speichern.

Importieren Gerät Identitäten gebündelt in einem IoT Hub identitätsregistrierung durch Verwendung asynchroner Vorgänge [IoT Hub Ressource Anbieterendpunkt][lnk-endpoints]. Importe sind Aufträge mit langer Ausführung, mit denen Daten in einem BLOB-Kunden bereitgestellten Container Identitätsdaten in die Registrierung des Geräts Identität schreiben.

- Ausführliche Informationen über das Importieren und Exportieren APIs finden Sie unter [IoT Hub Ressourcenanbieter REST-APIs][lnk-resource-provider-apis].
- Erfahren mehr über das Ausführen von Import und export Jobs finden Sie unter [Bulk Management IoT Hub Gerät Identitäten][lnk-bulk-identity].

## <a name="device-provisioning"></a>Gerät bereitstellen

Die Daten, die eine bestimmte IoT-Lösung speichert hängt die Anforderungen der Lösung. Aber mindestens eine Lösung Gerät Identitäten und Authentifizierungsschlüssel speichern muss. Azure IoT Hub enthält eine identitätsregistrierung, die Werte für die einzelnen Geräte-IDs und Authentifizierungsschlüssel Statuscodes speichern kann. Eine Lösung können andere Azure-Dienste wie Azure-Tabellenspeicher, Azure BLOB-Speicher oder Azure DocumentDB zusätzliches Gerätedaten gespeichert werden.

*Gerät Bereitstellung* ist die ersten Daten an die Shops in der Projektmappe hinzufügen. Um ein neues Gerät für die Verbindung zu Ihrem Hub zu aktivieren, müssen Sie eine neue Geräte-ID und Schlüssel IoT Hub identitätsregistrierung hinzufügen. Als Teil des Bereitstellungsprozesses müssen Sie gerätespezifische Daten in anderen Shops Lösung zu initialisieren.

## <a name="device-heartbeat"></a>Gerät-Takt

IoT Hub identitätsregistrierung enthält ein Feld namens **ConnectionState**. Verwenden Feld **ConnectionState** nur während der Entwicklung und Debuggen IoT Solutions sollten keine Abfrage das Feld zur Laufzeit (z. B. überprüfen, ob ein Gerät angeschlossen ist, um zu entscheiden, ob eine Cloud-Gerät oder eine SMS senden).

IoT-Lösung muss wissen, ob ein Gerät verbunden ist, (entweder zur Laufzeit oder genauer als die **ConnectionState** -Eigenschaft), sollte die Lösung *Heartbeat-Muster*.

Im Muster Takt sendet das Gerät Gerät Cloud-Nachrichten mindestens einmal alle festen Zeitraum (z. B. einmal pro Stunde). Dies bedeutet, dass selbst wenn ein Gerät keine Daten haben, weiterhin Botschaft eine leere Gerät Cloud (normalerweise mit einer Eigenschaft, die als Takt bezeichnet). Auf der Dienstseite Lösung verwaltet eine Zuordnung mit den letzten Takt für jedes Gerät empfangen und wird vorausgesetzt, dass ein Problem mit einem Gerät erhält er keine Heartbeat-Nachrichten innerhalb der erwarteten Zeit.

Eine komplexere Implementierung beispielsweise Informationen über [Vorgänge überwachen] [ lnk-devguide-opmon] Geräte ermitteln, die versuchen, eine Verbindung herstellen und kommunizieren jedoch fehlschlägt. Beim Implementieren des Heartbeat-Musters überprüfen Sie [IoT Hub und Drosselungen][lnk-quotas].

> [AZURE.NOTE] Eine IoT Lösung Geräteverbindungsstatus nur fest, ob Cloud-Gerät senden, und Nachrichten werden nicht an große Gruppen von Geräten übertragen, ist eine viel einfachere Muster müssen kurze Ablaufzeit verwenden. Dadurch wird das gleiche Ergebnis wie eine Gerät Verbindung Zustand Registrierung mit der Takt, während wesentlich effizienter verwalten. Es ist auch möglich, anfordern Nachricht Empfangsbestätigungen IoT Hub welche Geräte Nachrichten empfangen und die nicht online oder sind nicht benachrichtigt werden.

## <a name="reference-topics"></a>Themen:

Die folgenden Themen bieten weitere Informationen Devcie Identität.

## <a name="device-identity-properties"></a>Geräteeigenschaften-Identität

Geräte-Identitäten werden als JSON-Dokumente mit den folgenden Eigenschaften dargestellt.

| Eigenschaft | Optionen | Beschreibung |
| -------- | ------- | ----------- |
| Geräte-ID | erforderliche Updates schreibgeschützt | Groß-/Kleinschreibung String (bis zu 128 Zeichen) ASCII-7-Bit alphanumerischen Zeichen + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId | erforderlich, schreibgeschützt | Ein Hub generiert beachtet Zeichenfolge bis zu 128 Zeichen lang sein. Hiermit wird mit derselben **DeviceId**unterscheiden, wenn sie gelöscht und neu erstellt wurde. |
| ETag | erforderlich, schreibgeschützt | Eine Zeichenfolge, die eine schwache Etag für die Identität des Geräts nach [RFC7232][lnk-rfc7232].|
| Auth | Optionale | Ein zusammengesetztes Objekt mit Authentifizierung und Materialien. |
| auth.symkey | Optionale | Ein zusammengesetztes Objekt mit einem primären und einem sekundären Schlüssel im base64-Format gespeichert. |
| Status | Erforderlich | Ein Access-Indikator. Kann **aktiviert** oder **deaktiviert**. Wenn **aktiviert**, das Gerät eine Verbindung herstellen. **Deaktiviert**das Gerät jeden Endpunkt verbundenen Gerät zugreifen kann. |
| statusReason | Optionale | 128 Zeichen langen String, der den Grund für die Identität Gerätestatus speichert. Alle UTF-8-Zeichen sind zulässig. |
| statusUpdateTime | schreibgeschützt | Eine zeitliche Indikator zeigt Datum und Uhrzeit der letzten statusaktualisierung. |
| connectionState | schreibgeschützt | Ein Feld, der Verbindungsstatus: **verbunden** oder **getrennt**. Dieses Feld stellt die Ansicht IoT Hub Verbindung des Geräts. **Wichtig**: Dieses Feld sollte nur für Entwicklungs-/Debuggen verwendet. Der Verbindungsstatus wird nur für Geräte mit MQTT oder AMQP aktualisiert. Außerdem basiert auf Protokollebene Pings (MQTT Pings oder AMQP Pings) und eine maximale Verzögerung von 5 Minuten aufweisen. Aus diesen Gründen möglich fälschlich wie verbundene Geräte gemeldet, tatsächlich getrennt. |
| connectionStateUpdatedTime | schreibgeschützt | Ein zeitlicher Indikator zeigt Datum und Uhrzeit der letzten Verbindungsstatus wurde aktualisiert. |
| lastActivityTime  | schreibgeschützt | Zeitliche Indikator zeigt Datum und Uhrzeit der letzten Geräts verbunden, empfangen oder eine Nachricht. |

> [AZURE.NOTE] Verbindungsstatus kann nur IoT Hub Anzeigen des Status der Verbindung darstellen. Updates in diesem Zustand können je nach Netzwerk und Konfigurationen verzögert.

## <a name="additional-reference-material"></a>Zusätzliches Referenzmaterial

Andere Referenzthemen in Developer Guide enthalten:

- [IoT Hub Endpunkte] [ lnk-endpoints] beschreibt die verschiedenen Endpunkte jedes IoT Hub Runtime und Management macht.
- [Kontingente und Drosselung] [ lnk-quotas] beschreibt die Quoten für den Dienst IoT Hub und Drosselung Verhalten gelten zu erwarten, wenn Sie den Dienst verwenden.
- [IoT Hub-Gerät und SDKs] [ lnk-sdks] Listet die verschiedenen SDKs Sie können Wenn Sie Anwendungsentwicklung Geräte und Dienste, die interagieren mit IoT Hub.
- [Abfragesprache für im Vergleich, Methoden und Aufträge] [ lnk-query] beschreibt die Abfragesprache IoT Hub über Geräte im Vergleich, Methoden und Aufträge abrufen können.
- [IoT Hub MQTT Unterstützung] [ lnk-devguide-mqtt] MQTT Protokoll IoT Hub unterstützt Weitere Informationen bereit.

## <a name="next-steps"></a>Nächste Schritte

Jetzt mit IoT Hub-geräteidentitätsregistrierung gelernt haben, können Sie in den folgenden Themen Entwicklerhandbuch interessiert sein:

- [Steuern des Zugriffs auf IoT Hub][lnk-devguide-security]
- [Gerät im Vergleich mit dem Zustand und Konfigurationen synchronisiert][lnk-devguide-device-twins]
- [Eine direkte Methode auf einem Gerät][lnk-devguide-directmethods]
- [Planen von Projekten auf mehreren Geräten][lnk-devguide-jobs]

Möchten Sie versuchen, einige der in diesem Artikel beschriebenen Konzepte, können Sie die folgende praktische Einführung IoT Hub interessieren:

- [Erste Schritte mit Azure IoT Hub][lnk-getstarted-tutorial]


<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md