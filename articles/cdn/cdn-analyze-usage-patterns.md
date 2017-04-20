<properties
    pageTitle="Analysieren von Verwendungsmustern Azure CDN | Microsoft Azure"
    description="Verwendungsmuster für Ihre CDN mit den folgenden Berichten anzeigen: Bandbreite Daten Treffer, Cache-Status, Cachetrefferquote, IPV4/IPv6-Daten."
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

# <a name="analyze-azure-cdn-usage-patterns"></a>Analysieren von Verwendungsmustern Azure CDN

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Sie können für die folgenden Berichte mit CDN Verwendungsmuster anzeigen:

- Bandbreite
- Übertragene Daten
- Treffer
- Cache-Status
- Cache-Trefferquote
- IPv4/IPv6-Daten

## <a name="accessing-advanced-http-reports"></a>Erweiterte HTTP-Berichte

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-reports/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Hovern Sie über die Registerkarte **Analytics** und Maus über Flyout **Core Berichte** .  Klicken Sie auf den gewünschten Bericht klicken Sie im Menü.

    ![Verwaltungsportal CDN - Core Berichtsmenü](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Bandbreite

Bandbreite Bericht besteht aus einer Tabelle Diagramm und Daten der Bandbreite für HTTP und HTTPS über einen bestimmten Zeitraum angibt. Sie können die Bandbreite alle POPs CDN oder einen bestimmten POP anzeigen. So können Sie die Traffic-Spitzen und Verteilung über CDN-POPs in Mbit/s anzeigen.

- Wählt alle Edge-Knoten finden Datenverkehr von allen Knoten oder eines bestimmten Region/Knotens aus der Dropdownliste aus.
- Wählen Sie Datumsbereich anzeigen usw. Daten für heute/diese Woche in diesem Monat oder geben benutzerdefinierte und klicken Sie auf "go", um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
- Exportieren und laden die Daten durch Klicken auf das Excel-Blattsymbol "Weiter".

Der Bericht wird alle fünf Minuten aktualisiert.

![Bandbreite Bericht](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Übertragene Daten

Dieser Bericht besteht aus einer Tabelle Diagramm und Daten die Datenverkehrsverwendung für HTTP und HTTPS über einen bestimmten Zeitraum angibt. Sie können die Datenverkehrsverwendung alle POPs CDN oder einen bestimmten POP anzeigen. So können Sie die Traffic-Spitzen und Verteilung über CDN-POPs in GB anzeigen.

- Wählt alle Knoten aus Rand um Verkehr von alle Notizen oder einen bestimmten Region/Knoten aus der Dropdown-Liste auswählen.
- Wählen Sie Datumsbereich anzeigen usw. Daten für heute/diese Woche in diesem Monat oder geben benutzerdefinierte und klicken Sie auf "go", um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
- Exportieren und laden die Daten durch Klicken auf das Excel-Blattsymbol "Weiter".

Der Bericht wird alle fünf Minuten aktualisiert.

![Daten für Bericht](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Treffer (Statuscodes)

Dieser Bericht zeigt die Verteilung der Anforderung des Codes für den Inhalt. Jede Anforderung für Inhalte generiert einen HTTP-Statuscode. Der Statuscode beschreibt wie Rand erscheint die Anforderung behandelt. 2xx-Statuscodes z. B. angeben, dass die Anforderung erfolgreich an einen Client gesendet wurde, und ein 4xx-Statuscode ist ein Fehler. Weitere Informationen zu HTTP-Statuscodes finden Sie unter [Statuscodes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Wählen Sie Datumsbereich anzeigen usw. Daten für heute/diese Woche in diesem Monat oder geben benutzerdefinierte und klicken Sie auf "go", um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
- Exportieren und die Daten durch Klicken auf "Go" neben der Excel-Tabelle herunterladen.

![Bericht über Seitenzugriffe](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Cache-Status

Dieser Bericht zeigt die Verteilung der Cachetreffer und Cachefehler Clientanforderung. Da die schnellste Leistung Cachetreffer stammen, Optimieren Sie Übermittlung Datenraten minimieren Cachefehler und abgelaufene Cachetreffer. Cachefehler werden durch Konfigurieren der Ursprungsserver zur Vermeidung von "No-Cache" Antwortheader zuweisen vermeiden Abfragezeichenfolge Zwischenspeichern außer soweit erforderlich und vermeiden Antwortcodes nicht zwischengespeichert. Abgelaufene Cache Treffer vermieden werden können, mit einer Anlage ist so lange wie möglich minimieren Sie die Anzahl der Anfragen an den Ursprungsserver Max-Age.

![Cache-Status-Bericht](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Zwischenspeicher Statuswerte:

- TCP_HIT: Vom Rand bedient. Das Objekt wurde im Cache und die Max-Age nicht überschritten.
- TCP_MISS: Vom Ursprung bedient. Das Objekt wurde nicht im Cache und die Antwort wurde zum Ursprung.
- TCP_EXPIRED _MISS: nach dem erneuten mit Ursprung bereitgestellt. Das Objekt wurde im Cache jedoch die Max-Age überschritten. Eine Verlängerung mit führte Cacheobjekt durch eine neue Antwort vom Ursprung.
- TCP_EXPIRED _HIT: nach dem erneuten mit Rand bedient. Das Objekt wurde im Cache jedoch die Max-Age überschritten. Erneute Validierung mit dem ursprünglichen Server führte Cacheobjekt wird unverändert.

- Wählen Sie Datumsbereich anzeigen usw. Daten für heute/diese Woche in diesem Monat oder geben benutzerdefinierte und klicken Sie auf "go", um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
- Exportieren und laden die Daten durch Klicken auf das Excel-Blattsymbol "Weiter".

### <a name="full-list-of-cache-statuses"></a>Liste der Cache-Status

- TCP_HIT – dieser Status wird gemeldet, wenn eine Anforderung an den Client direkt von POP bereitgestellt wird. Anlage ist ein POP wird auf den nächsten auf dem Client zwischengespeichert und es gültige Time to live oder TTL sofort beantwortet. Die folgenden Antwortheader TTL bestimmt:

    - Cache-Control: s-maxage
    - Cache-Control: Max-age
    - Läuft ab

- TCP_MISS - gibt diesen Status an, auf die nächste Client eine zwischengespeicherte Version der angeforderten Ressource nicht gefunden wurde. Die Anlage wird vom Ursprungsserver oder eine ursprungsschutzserver angefordert. Wenn dem Ausgangsserver oder dem Ausgangsserver Shield Vermögenswert gibt, wird es an den Client bereitgestellt und auf dem Client und Edge-Server zwischengespeichert. Andernfalls einen Statuscode nicht 200 (z. B. 403 Verboten, usw. 404 nicht gefunden) zurückgegeben.

- TCP_EXPIRED _HIT – dieser Status wird gemeldet, wenn eine Anforderung, die an eine Anlage mit einer abgelaufenen TTL wie beim Höchstalter der Anlage abgelaufen ist, direkt von POP an den Client gesendet wurde.

    Abgelaufene Anforderung führt in der Regel eine Wiederherstellungsanfrage auf dem ursprünglichen Server. Reihenfolge für eine TCP_EXPIRED _HIT auf muss der Ursprungsserver laut eine neuere Version der Ressource nicht vorhanden ist. Dieser Art von Situation aktualisiert in der Regel diese Ressource Cache-Control und Expires-Header.

- TCP_EXPIRED _MISS – dieser Status wird gemeldet, wenn eine neuere Version einer abgelaufenen zwischengespeicherte Anlage von POP für den Client bereitgestellt wird. Dies tritt auf, wenn die Gültigkeitsdauer für eine zwischengespeicherte Ressource (z. B. Max-Age abgelaufen) und dem ursprünglichen Server eine neuere Version dieses Vermögenswertes. Diese neue Version der Anlage werden auf dem Client statt die zwischengespeicherte Version ausgeliefert. Darüber hinaus wird auf die Edge-Server und dem Client zwischengespeichert werden.

- CONFIG_NOCACHE - gibt diesen Status eine kundenspezifische Konfiguration auf unserer Seite POP Anlage verhindert zwischengespeichert werden.

- NONE - gibt diesen Status an, dass Cache Aktualität des Inhalts nicht geprüft wurde.

- TCP_ CLIENT_REFRESH _MISS – dieser Status wird gemeldet, wenn ein HTTP-Client (z. B. Browser) eine Kante POP eine neue Version einer veralteten Anlage vom Ursprungsserver abgerufen wird.

    Standardmäßig verhindert Server HTTP-Clients erzwingen unsere Edge-Server eine neue Version der Anlage vom Ursprungsserver abgerufen.

- TCP_ PARTIAL_HIT – dieser Status wird gemeldet, wenn eine Bytebereichsanfragen einen Treffer für eine teilweise zwischengespeicherte Ressource führt. Der angeforderte Bytebereich wird sofort aus der POP an den Client übermittelt.

- UNCACHEABLE - Status angezeigt wird, wenn eine Anlage Cache-Control und Expires-Header angeben, keinen POP oder der HTTP-Client zwischengespeichert werden soll. Diese Anforderungstypen werden vom Ursprungsserver bedient.

## <a name="cache-hit-ratio"></a>Cache-Trefferquote

Dieser Bericht zeigt den Prozentsatz der zwischengespeicherte Anfragen direkt aus dem Cache bedient wurden.

Der Bericht enthält folgenden Angaben:

- Der angeforderte Inhalt wurde auf die nächste an den anfordernden zwischengespeichert.
- Die Anforderung war direkt vom Rand des Netzwerks.
- Die Anforderung war nicht Verlängerung mit dem ursprünglichen Server erforderlich.

Der Bericht enthalten nicht:

- Anfragen, die aufgrund von Optionen verweigert.
- Anfragen für Anlagen, deren Anfrageheader nicht zwischengespeichert werden soll. Z. B. Cache-Control: Private Cache-Control: Nein-Cache oder Pragma: Nein-Cache-Header werden verhindert, dass eine Anlage zwischengespeichert.
- Byte Bereich fordert teilweise zwischengespeicherte Inhalt.

Formel: (TCP_-TREFFER / (TCP_ TREFFER + TCP_MISS)) * 100

- Wählen Sie Datumsbereich anzeigen usw. Daten für heute/diese Woche in diesem Monat oder geben benutzerdefinierte und klicken Sie auf "go", um sicherzustellen, dass Ihre Auswahl aktualisiert wird.
- Exportieren und laden die Daten durch Klicken auf das Excel-Blattsymbol "Weiter".


![Bericht über Cachetrefferverhältnis](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPv6-Daten

Dieser Bericht zeigt die Verteilung Datenverkehr Verwendung in IPV4 Vs IPV6.

![IPv4/IPv6-Daten](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Wählen Sie Datumsbereich anzeigen usw. Daten für heute/diese Woche in diesem Monat oder benutzerdefinierte Daten eingeben.
- Klicken Sie dann auf "go", um sicherzustellen, dass Ihre Auswahl aktualisiert wird.


## <a name="considerations"></a>Hinweise

Berichte können nur in den letzten 18 Monaten generiert werden.
