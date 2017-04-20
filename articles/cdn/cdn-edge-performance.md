<properties
    pageTitle="Analysieren der Leistung in Azure CDN | Microsoft Azure"
    description="Knoten in Microsoft Azure CDN Leistung zu analysieren. Rand Leistungsanalysen bereit CDN Datenverkehr und Bandbreite Verbrauch präzise Informationen."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Analysieren der Leistung in Microsoft Azure CDN Knoten

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Übersicht
Rand Leistungsanalysen bereit CDN Datenverkehr und Bandbreite Verbrauch präzise Informationen. Diese Informationen kann dann verwendet werden, trendige Statistiken zu generieren, mit denen Sie erhalten Sie Einblick in Ihre Anlagen wie werden zwischengespeichert und an die Clients übermittelt. Wiederum dadurch zu einer Strategie zur Optimierung der Bereitstellung der Inhalte und um Probleme bestimmen behandelt eine bessere Nutzung das CDN. Daher nicht nur werden Daten Übermittlung Leistung verbessern, sondern auch die CDN-Kosten senken werden.

> [AZURE.NOTE] Alle verwenden UTC-GMT-Notation, wenn einem Zeitpunkt angeben.

## <a name="reports-and-log-collection"></a>Berichte und Protokoll-Auflistung

CDN muss Aktivität vom Rand Leistungsanalysen Modul Daten bevor Berichte auf erstellen können. Diese Auflistung Prozess tritt einmal und deckt die Aktivität, die während des vorherigen Tages stattgefunden. Dies bedeutet einen Bericht Statistiken stellen ein Beispiel für den Tag Statistiken es verarbeitet wurde und nicht unbedingt den vollständigen Satz von Daten für den aktuellen Tag enthalten. Die Hauptfunktion dieser Berichte wird zum Bewerten der Leistung. Sie sollte nicht für Zahlungszwecke oder genaue numerische Statistiken verwendet.

> [AZURE.NOTE] Die Rohdaten aus dem Rand Leistung Analytische Berichte werden ist für mindestens 90 Tage.

## <a name="dashboard"></a>Dashboard

Rand Leistungsanalysen Dashboard verfolgt aktuelle und frühere CDN Datenverkehr eines Diagramms und Statistiken. Mithilfe dieses Dashboard erkennen aktuelle und langfristige Trends auf die Leistung des CDN Datenverkehr für Ihr Konto.

Dieses Dashboard besteht aus:

* Ein interaktives Diagramm, das die Visualisierung der Kennzahlen und Trends ermöglicht.
* Eine Zeitachse, die gewissermaßen langfristig Muster Kennzahlen und bereitstellt.
* Kennzahlen und statistische Informationen verbessert unser Netzwerk CDN Siteverkehr allgemeine Leistung, Auslastung und Effizienz gemessen.

### <a name="accessing-the-edge-performance-dashboard"></a>Zugriff auf die Edge-Leistungsdashboard

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-edge-performance/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Hovern Sie über die Registerkarte **Analytics** und Maus über **Rand Performance Analytics** Flyout.  Klicken Sie auf **Dashboard**.

    Rand Knoten Analytics Dashboard wird angezeigt.

### <a name="chart"></a>Diagramm

Das Dashboard enthält ein Diagramm, das eine Metrik in der Zeitleiste unmittelbar darunter erscheint ausgewählten Zeitraum verfolgt.  Eine Zeitachse, Diagramme zum letzten zwei Jahren CDN oben wird direkt unterhalb des Diagramms angezeigt.

#### <a name="using-the-chart"></a>Mithilfe des Diagramms

* Standardmäßig wird der Cache Effizienzgrad für die letzten 30 Tage dargestellt.
* Dieses Diagramm wird täglich sortierte Daten generiert.
* Mauszeiger auf das Liniendiagramm Tag zeigt Datum und der Wert der Metrik zu diesem Zeitpunkt.
* Klicken Sie auf Markieren Wochenenden um eine Überlagerung Licht graue vertikale Balken einzublenden, die Wochenenden auf Diagramm darstellen. Dieser Typ der Überlagerung ist Wochenenden Verkehrsmuster identifiziert.
* Klicken Sie auf Ansicht ein Jahr vor um eine Überlagerung der Aktivität des vergangenen Jahres im gleichen Zeitraum in das Diagramm einzublenden. Diese Art von Vergleich Einblick in langfristige CDN Verwendungsmuster. Die oberen rechten Ecke des Diagramms enthält eine Legende, die das Farbe für jede Liniendiagramm angibt.

#### <a name="updating-the-chart"></a>Aktualisieren des Diagramms

* Zeitraum: Führen Sie eine der folgenden:
    * Wählen Sie die gewünschte Region im Schnittfenster. Das Diagramm wird mit Daten aktualisiert, die den ausgewählten Zeitraum entspricht.
    * Doppelklicken Sie auf das Diagramm, um alle verfügbaren Verlaufsdaten maximal zwei Jahre anzuzeigen.
* Maßstab: Klicken Sie auf das Diagrammsymbol neben dem gewünschten Metrik. Das Diagramm und die Zeitleiste werden mit Daten für die entsprechenden Metrik aktualisiert.


### <a name="key-metrics-and-statistics"></a>Kennzahlen und Statistiken

#### <a name="efficiency-metrics"></a>Effizienz Metriken

Diese Metriken dient zu sehen, ob Cache-Effizienz verbessert werden kann. Die Hauptvorteile von Cache-Effizienz abgeleitet werden:

* Geringere Last auf dem Ursprungsserver führen:
    * Bessere Leistung des Webservers.
    * Senkung der Betriebskosten.
* Schnellere Bereitstellung Daten mehr Anfragen direkt aus dem CDN serviert werden verbessert.

Feld | Beschreibung
------|------------
Cache-Effizienz | Gibt den Prozentsatz der Daten, die vom Cache verarbeitet wurde. Diese Metrik Maßnahmen bei einer zwischengespeicherten Version des angeforderten Inhalts war direkt aus dem CDN (Edge-Server) anfordern (z. B. Webbrowser)
Trefferquote | Gibt den Prozentsatz der Anfragen, die vom Cache verarbeitet wurden. Diese Metrik Maßnahmen bei einer zwischengespeicherten Version des angeforderten Inhalts war direkt aus dem CDN (Edge-Server), anfordern (z. B. Webbrowser).
% der Bytes Remote - keine Cache-Konfiguration | Gibt den Prozentsatz des Datenverkehrs vom Ursprungsserver CDN (Edge-Server) gesendet wurde, die nicht durch Umgehung Cache-Funktion (HTTP Regelmodul) zwischengespeichert werden.
% der Bytes Remote - Cache abgelaufen | Gibt den Prozentsatz des Datenverkehrs, der Ursprungsserver CDN (Edge-Server) bereitgestellt wurde aufgrund veralteter Content Verlängerung.

#### <a name="usage-metrics"></a>Verwendung Metriken

Diese Metriken soll Einblick in die folgenden Maßnahmen:

* Minimierung der Betriebskosten durch das CDN.
* Reduzieren CDN Ausgaben durch Cache-Effizienz und Komprimierung.

> [AZURE.NOTE] Datenverkehr Volume-Nummern darstellen Datenverkehr, die möglicherweise nur einen Teil des gesamten Datenverkehrs für Großkunden und Berechnung der Verhältnisse und Prozentsätze verwendet wurde.

Feld | Beschreibung
------|------------
Durchschnittliche Bytes gesendet | Gibt die durchschnittliche Anzahl von Bytes für jede Anforderung bedient CDN (Edge-Servern) an die anfordernde Person (z. B. Webbrowser) übertragen.
Kein Cache Config Byte Rate | Gibt den Prozentsatz des Datenverkehrs bedient CDN (Edge-Servern) an die anfordernde Person (z. B. Webbrowser), die durch die Umgehung der Cache-Funktion nicht zwischengespeichert werden.
Komprimierte Bytes Rate | Gibt den Prozentsatz der Datenverkehr aus dem CDN (Edge-Server) zum Anfordern (z. B. Webbrowser) in einem komprimierten Format.
Bytes gesendet | Gibt die Datenmenge in Bytes, die aus dem CDN (Edge-Servern) an die anfordernde Person (z. B. Webbrowser) übermittelt wurden.  
Bytes | CDN (Edge-Server) gibt die Datenmenge in Bytes von anfordernden Personen (z. B. Webbrowser) gesendet.
Bytes Remote | Gibt die Datenmenge in Bytes vom Ursprungsserver CDN und Kunden CDN (Edge-Server) gesendet.

#### <a name="performance-metrics"></a>Performance-Kennzahlen

Diese Metriken dient allgemeinen CDN Leistung für den Datenverkehr.

Feld | Beschreibung
------|------------
Übertragungsrate | Zeigt die durchschnittliche Rate Inhalt aus dem CDN an übertragen wurde.
Dauer | Gibt die durchschnittliche Zeit in Millisekunden, dauerte zu einer Anlage an (z. B. Webbrowser).
Komprimierte Anforderungsrate | Gibt den Prozentsatz der Treffer aus dem CDN (Edge-Servern) an die anfordernde Person (z. B. Webbrowser) übermittelt wurden in einem komprimierten Format.
4xx-Fehlerrate | Gibt den Prozentsatz der Treffer, die einen Statuscode 4xx generiert.
5xx-Fehlerrate | Gibt den Prozentsatz der Treffer, die einen Statuscode 5xx generiert.
Treffer | Gibt die Anzahl der Anfragen für CDN Inhalt an.

#### <a name="secure-traffic-metrics"></a>Sicheren Datenverkehr Metriken

Diese Metriken dient CDN-Leistung für HTTPS-Datenverkehr überwachen.

Feld | Beschreibung
------|------------
Sichere Cache-Effizienz | Gibt den Prozentsatz der Daten aus dem Cache für HTTPS-Anfragen bedient wurden. Diese Metrik Maßnahmen bei einer zwischengespeicherten Version des angeforderten Inhalts war direkt aus dem CDN (Edge-Server), anfordern (z. B. Webbrowser) über HTTPS.
Sichere Übertragungsrate | Gibt die durchschnittliche Rate an der Inhalt aus dem CDN (Edge-Server) anfordern (z. B. Webserver) übertragen wurde über HTTPS an.
Durchschnittliche sicher | Gibt die durchschnittliche Zeit in Millisekunden zu einer Anlage an (z. B. Webbrowser) über HTTPS dauerte.
Sichere Treffer | Gibt die Anzahl der HTTPS-Anfragen für CDN-Inhalt.
Sichere Bytes gesendet | Gibt die HTTPS-Datenverkehr in Bytes, die aus dem CDN (Edge-Servern) an die anfordernde Person (z. B. Webbrowser) übermittelt wurden.

## <a name="reports"></a>Berichte

Jedes in dieser Unterrichtseinheit enthält ein Diagramm sowie Statistiken zur Auslastung von Bandbreite und Datenverkehr für unterschiedliche Kriterien (z. B. HTTP-Statuscodes Cache Statuscodes anfordern URL usw.). Diese Informationen können verwendet werden, tiefer in wie Inhalte an Ihre Kunden bedient und CDN Verhalten zur Leistungssteigerung mit Daten Lieferung.

### <a name="accessing-the-edge-performance-reports"></a>Die Kante Performance-Berichte

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-edge-performance/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Hovern Sie über die Registerkarte **Analytics** und Maus über **Rand Performance Analytics** Flyout.  Klicken Sie auf **HTTP Large Object**.

    Rand Knoten Analytics-Berichte-Bildschirm wird angezeigt.

Bericht | Beschreibung
-------|------------
Tägliche Zusammenfassung | Ermöglicht tägliche Datenverkehr Trends über einen bestimmten Zeitraum anzeigen. Jeder Balken in diesem Diagramm stellt ein bestimmtes Datum. Die Größe des Balkens gibt die Gesamtanzahl der Treffer an diesem Datum.
Stündliche Zusammenfassung | Können Sie stündlich Datenverkehr Trends über einen bestimmten Zeitraum anzeigen. Jeder Balken in diesem Diagramm stellt eine einzelne Stunde an einem bestimmten Datum. Die Größe des Balkens gibt die Gesamtanzahl der Treffer in dieser Stunde.
Protokolle | Zeigt die Aufteilung des Datenverkehrs zwischen den Protokollen HTTP und HTTPS. Ein Ringdiagramm gibt den Prozentsatz der Treffer für jede Art von Protokoll.
HTTP-Methoden | Erhalten Sie einen Überblick der HTTP-Methoden verwendet werden, um Ihre Daten. Normalerweise werden die häufigsten HTTP-Anforderungsmethoden GET, HEAD und POST. Ein Ringdiagramm gibt den Prozentsatz der Treffer für jeden HTTP-Anforderungsmethode.
URLs | Enthält ein Diagramm, das die Top 10 angeforderten URLs angezeigt. Für jede URL wird eine Leiste angezeigt. Die Höhe der Balken gibt an, wie viele Zugriffe, die bestimmtes URL, in dem Zeitraum generiert hat vom Bericht abgedeckt. Statistiken für die ersten 100 angeforderten URLs direkt unterhalb dieses Diagramm angezeigt werden.
CNAMEs | Enthält ein Diagramm, das oben zeigt 10 CNAMEs zum Anfordern von Ressourcen über den Zeitraum eines Berichts umfassen. Statistiken für die ersten 100 angeforderte CNAMEs unterhalb dieses Diagramm angezeigt werden.
Ursprung | Enthält ein Diagramm, das zeigt die Top 10 EUR oder Kunden Ursprungsserver aus denen Ressourcen über einen bestimmten Zeitraum angefordert wurden. Statistiken für die ersten 100 angeforderte CDN oder Kunden Ursprungsserver unterhalb dieses Diagramm angezeigt werden. Kunden Ursprungsserver werden gemäß der Option Verzeichnisnamen namentlich aufgeführt.
Geo-POPs | Zeigt an, wie viel Ihr Datenverkehr über einen bestimmten Point of Presence (POP) weitergeleitet wird. Die Abkürzung aus drei Buchstaben stellt einen POP in unserem Netzwerk CDN.
Clients | Enthält ein Diagramm mit den Top 10 Clients, die Ressourcen über einen bestimmten Zeitraum angefordert. Für diesen Bericht gelten alle Anfragen, die von derselben IP-Adresse stammen vom selben Client sein. Statistiken für die Top 100 Clients werden direkt unter diesem Diagramm angezeigt. Dieser Bericht ist für Download Verhaltensmuster für Top-Benutzer festlegen.
Cache-Status | Bietet eine detaillierte Aufschlüsselung der Cacheverhalten, das Verfahren zur Verbesserung der allgemeinen Endbenutzer anzeigen kann. Da die schnellste Leistung Cachetreffer stammen, Optimieren Sie Übermittlung Datenraten minimieren Cachefehler und abgelaufene Cachetreffer.
KEINE Details | Enthält ein Diagramm, das die Top 10 URLs für Ressourcen zeigt für den Inhalt abhängig über einen angegebenen Zeitraum nicht ausgecheckt wurde. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
CONFIG_NOCACHE Informationen | Enthält ein Diagramm mit den Top 10 URLs für Anlagen, die durch den Kunden CDN-Konfiguration nicht zwischengespeichert wurden. Diese Anlagen wurden direkt vom Ursprungsserver serviert. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
UNCACHEABLE Informationen | Enthält ein Diagramm mit den Top 10 URLs für Anlagen, die durch Headerdaten zwischengespeichert werden. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
TCP_HIT Informationen | Enthält ein Diagramm mit den Top 10 URLs für Anlagen, die direkt aus dem Cache bedient werden. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
TCP_MISS Informationen | Enthält ein Diagramm mit Top 10 URLs für Ressourcen, die dem Cache TCP_MISS Status. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
TCP_EXPIRED_HIT Informationen | Enthält ein Diagramm, das die Top 10 URLs für veraltete Anlagen angezeigt, der dem POP bereitgestellt wurden. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
TCP_EXPIRED_MISS Informationen | Enthält ein Diagramm, das die Top 10 URLs für veraltete Anlagen wird für die neue Version vom Ursprungsserver abgerufen werden mussten. Statistiken für die Top 100 URLs für diese Arten von Elementen werden direkt unter diesem Diagramm angezeigt.
TCP_CLIENT_REFRESH_MISS Informationen | Enthält ein Balkendiagramm, das die Top 10 URLs zeigt für Anlagen vom Ursprungsserver durch eine Nein-Cache-Anforderung vom Client abgerufen wurden. Statistiken für die Top 100 URLs für solche Anfragen werden direkt unter diesem Diagramm angezeigt.
Client-Anforderungstypen | Gibt den Typ der Anfragen von HTTP-Clients (z. B. Browser) wurden. Dieser Bericht enthält ein Ringdiagramm, das bringt, wie Anfragen behandelt werden. Bandbreite und Datenverkehr wird für jede Anforderung unterhalb des Diagramms angezeigt.
Benutzer-Agent | Enthält ein Balkendiagramm mit Top 10 Benutzer-Agents, die Inhalte über unsere CDN. Normalerweise ist ein Benutzer-Agent einen Web-Browser, MediaPlayer oder einen Browser Mobiltelefon. Statistiken für die Top 100-Agents werden direkt unter diesem Diagramm angezeigt.
Verweise | Enthält ein Balkendiagramm anzeigen der Top 10-Quellen Inhalt über unsere CDN zugegriffen. Eine Referenz ist normalerweise die URL der Webseite oder der Ressource, die links zu Ihrem Inhalt. Detaillierte unterhalb des Diagramms 100 wichtigsten Quellen für Informationen.
Komprimierungstypen | Enthält ein Ringdiagramm angeforderten Ressourcen sie durch unsere Edgeserver komprimiert wurden untersucht. Der Prozentsatz der komprimierte Anlagen ist vom Typ des verwendeten Komprimierungstyp unterteilt. Detaillierte unterhalb des Graphen für jeden Komprimierungstyp und Status Informationen.
Dateitypen | Enthält ein Balkendiagramm mit den Top 10 Dateitypen, die über unsere CDN für Ihr Konto angefordert wurde. Für die Zwecke dieses Berichts ein Dateityp der Anlage Erweiterung festgelegten und Internet Medientyp (z. B. HTML \[Text/HTML-\], htm \[Text/HTML-\], aspx \[Text/html\]usw..). Detaillierte unterhalb des Diagramms für die Top 100 Dateitypen Informationen.
Eindeutige Dateien | Enthält ein Diagramm, das die Gesamtzahl der eindeutigen Elemente darstellt, die an einem bestimmten Tag über einen bestimmten Zeitraum angefordert wurden.
Token Auth-Zusammenfassung | Enthält ein Kreisdiagramm, das schnelle Übersicht auf, ob angeforderte von Token-basierter Authentifizierung geschützt wurden. Geschützte Ressourcen werden im Diagramm nach den Ergebnissen ihrer versuchte Authentifizierung angezeigt.
Token Auth verweigern Details | Enthält ein Balkendiagramm, das Top 10-Anfragen angezeigt, die durch Token-basierter Authentifizierung abgelehnt wurden.
HTTP-Antwortcodes | Bietet eine Aufschlüsselung der HTTP-Statuscodes (z. B. 200 OK, 403 Verboten, usw. 404 nicht gefunden), HTTP-Clients von Edge-Servern übermittelt wurden. Ein Kreisdiagramm können Sie schnell beurteilen, wie Ihre Anlagen bereitgestellt wurden. Detaillierte statistische Daten erfolgt für jedes Antwortcode unterhalb des Diagramms.
404-Fehler | Enthält ein Balkendiagramm, das Top 10-Anfragen angezeigt, die einen 404 nicht gefunden-Antwortcode geführt haben.
403-Fehler | Enthält ein Balkendiagramm, das an den Top 10-Anfragen, die 403 Forbidden Antwortcode geführt haben. 403 Forbidden Antwortcode tritt auf, wenn ein Kunde Ursprungsserver oder ein Edge-Server auf unserer verweigert wird.
4xx-Fehler | Enthält ein Balkendiagramm, das Top 10-Anfragen angezeigt, die den Antwortcode im Bereich 400 geführt haben. Aus diesem Bericht ausgeschlossen sind 403 nicht gefunden und 404 Forbidden Antwortcodes. Antwortcode 4xx tritt in der Regel durch einen Fehler bei eine Anforderung verweigert wird.
504 Fehler | Enthält ein Balkendiagramm, das an den Top 10-Anfragen, die 504 Gateway-Timeout-Antwortcode geführt haben. 504 Gateway-Timeout-Antwortcode tritt ein Timeout auftritt, wenn ein HTTP-Proxy zur Kommunikation mit einem anderen Server. Bei unserer CDN tritt 504 Gateway-Timeout-Antwortcode normalerweise beim Edge-Server Kommunikation mit einem Kunden Ursprungsserver.
Fehler vom Typ 502 | Enthält ein Balkendiagramm, das an den Top 10-Anfragen, die 502 Bad Gateway Antwortcode geführt haben. 502 Bad Gateway Antwortcode tritt zwischen einem Server und HTTP-Proxy ein HTTP-Protokoll Fehler auftritt. Bei unserer CDN tritt 502 Bad Gateway Antwortcode normalerweise beim Ausgangsserver ein Debitor, ein Edge-Server eine ungültige Antwort zurückgegeben. Eine Antwort ist ungültig, wenn es analysiert werden kann oder unvollständig ist.
5xx-Fehler | Enthält ein Balkendiagramm, das Top 10-Anfragen angezeigt, die den Antwortcode 500 Bereichs geführt haben.  Aus diesem Bericht ausgeschlossen sind 502 unzulässiges Gateway und 504 Gateway-Timeout-Antwortcodes.

## <a name="see-also"></a>Siehe auch
* [Azure CDN-Übersicht](cdn-overview.md)
* [Echtzeit-Statistiken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Mit dem Regelmodul HTTP-Standardverhalten überschreiben](cdn-rules-engine.md)
* [Erweiterte HTTP-Berichte](cdn-advanced-http-reports.md)
