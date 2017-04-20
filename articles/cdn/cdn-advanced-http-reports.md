<properties
    pageTitle="Azure CDN erweiterte HTTP-Berichte | Microsoft Azure"
    description="Erweiterte HTTP-Berichte in Microsoft Azure CDN. Diese Berichte enthalten ausführliche Informationen CDN-Aktivität."
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

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Erweiterte HTTP-Berichte in Microsoft Azure CDN

## <a name="overview"></a>Übersicht

Dieses Dokument erläutert erweiterte HTTP-Berichte in Microsoft Azure CDN. Diese Berichte enthalten ausführliche Informationen CDN-Aktivität.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Erweiterte HTTP-Berichte

1. Blatt Profil CDN klicken Sie auf **Verwalten** .

    ![CDN Profil Blade Schaltfläche verwalten](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    Das CDN-Verwaltungsportal wird geöffnet.

2. Hovern Sie über die Registerkarte **Analytics** und Maus über Flyout **Erweiterte HTTP-Berichte** .  Klicken Sie auf **HTTP große Plattform**.

    ![Verwaltungsportal CDN - Menü Erweiterte Berichte](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Optionen werden angezeigt.

## <a name="geography-reports-map-based"></a>Geografische Berichte (Map-basiert)

Es gibt fünf Berichte, die Nutzen einer Zuordnung an die Regionen, aus denen der Inhalt angefordert wird. Diese Berichte sind Weltkarte, USA Karte Kanada Karte, Karte Europa und Asien-Pazifik-Karte.

Jede Karte Bericht Reihen geografische Entitäten (z.B. Länder, Staaten und Provinzen) nach den Prozentsatz der Treffer aus dieser Region. Außerdem erfolgt eine Zuordnung die Positionen visualisieren, aus denen der Inhalt angefordert wird. Es kann dazu farbcodieren jeder Region entsprechend der Nachfrage in diesem Bereich. Leichter schattierte Bereiche weisen niedrigere Nachfrage nach Ihrem Inhalt dunkleren höhere Nachfrage nach Ihren Inhalt anzuzeigen.

Detaillierte Informationen zum Datenverkehr und Bandbreite für jede Region wird direkt unter der Karte bereitgestellt. Dadurch wird die Gesamtanzahl der Treffer Prozentanteil an Zugriffen, die Gesamtmenge der Daten an übertragen (in GB) und der Prozentsatz der Daten für jede Region übertragen. Zeigt eine Beschreibung für jede dieser Metriken. Schließlich beim Bewegen über einen Bereich (z.B. Land, Bundesland oder Kanton) den Namen und den Prozentsatz der Treffer in der Region als QuickInfo erscheint.

Eine kurze Beschreibung für jeden geografischen Zuordnung basierenden Bericht nachstehend.

Berichtsname | Beschreibung
------------|------------
Weltkarte | Diesem Bericht können Sie die Nachfrage nach Ihren CDN-Inhalt anzeigen. Jedes Land ist auf der Karte an den Prozentsatz der Treffer aus der Region farbcodiert.
Karte der USA | Diesen Bericht können Sie den Bedarf für die CDN-Inhalte in den Vereinigten Staaten anzeigen. Jeder Status wird auf dieser Karte Prozentanteil an Zugriffen an, die aus dieser Region farbcodiert.
Kanada-Karte | Diesem Bericht können Sie den Bedarf für die CDN-Inhalte in Kanada anzuzeigen. Jeder Kanton ist farblich auf dieser Karte an den Prozentsatz der Treffer aus der Region.
Europakarte | Mit diesem Bericht können Sie den Bedarf für die CDN-Inhalte in Europa anzeigen. Jedes Land ist farblich auf dieser Karte an den Prozentsatz der Treffer aus der Region.
Asien-Pazifik-Karte | Diesem Bericht können Sie den Bedarf für die CDN-Inhalte in Asien anzeigen. Jedes Land ist farblich auf dieser Karte an den Prozentsatz der Treffer aus der Region.

## <a name="geography-reports-bar-charts"></a>Geografische Berichte (Balkendiagrammen)

Es gibt zwei zusätzliche Berichte, die statistische Informationen nach Geografie, oben Städten und Ländern. Diese Berichte Rang Städte und Ländern bzw. nach Anzahl der Zugriffe, die aus diesen Gebieten stammen. Auf diese Art von Bericht generieren zeigt ein Balkendiagramm der Top 10 Städten oder Ländern, die Inhalte auf einer bestimmten Plattform angefordert. Dieses Balkendiagramm können Sie schnell beurteilen die Bereiche, die die höchste Anzahl der Anfragen für den Inhalt zu generieren.

Der linken Seite des Diagramms (y-Achse) gibt an, wie viele Treffer im angegebenen Bereich aufgetreten. Direkt unterhalb des Diagramms (x-Achse) finden Sie ein Etikett für jeden der 10 Regionen.

### <a name="using-the-bar-charts"></a>Balkendiagramm verwenden

* Wenn Sie auf einen Balken zeigen, werden der Name und die Gesamtzahl der Treffer im Bereich als QuickInfo angezeigt.
* Die QuickInfo für den Bericht Top Städte bezeichnet eine Stadt Name, Bundesland und Land Abkürzung.
* Wenn die Stadt oder Region (z.B. Bundesland) eine Anforderung stammt nicht bestimmt werden konnte, bedeutet, dass sie unbekannt sind. Wenn das Land unbekannt und zwei Fragezeichen (?) heißt, angezeigt.
* Ein Bericht umfassen Metriken für "Europa" oder "Asien/Pazifik." Elemente sollen nicht alle IP-Adressen in diesen Gebieten statistische Informationen. Vielmehr gelten nur für Anfragen von IP-Adressen stammen, die in Europa oder Asien statt in einer bestimmten Stadt oder Land verteilt sind.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Sie finden die Gesamtanzahl der Treffer Prozentanteil an Zugriffen, die Datenmenge (in GB) übertragen und der Prozentsatz der Daten für 250 Regionen übertragen. Zeigt eine Beschreibung für jede dieser Metriken.

Eine kurze Beschreibung wird für beide Berichte unten bereitgestellt.

Berichtsname | Beschreibung
------------|------------
Top Städte | Dieser Bericht gehört Städte nach Anzahl der Treffer stammt aus der Region.
Wichtigsten Länder | Dieser Bericht gehört Länder nach Anzahl der Treffer stammt aus der Region.

## <a name="daily-summary"></a>Tägliche Zusammenfassung

Die tägliche Zusammenfassung können Sie die Gesamtzahl der Zugriffe und Daten über eine bestimmte Plattform täglich anzeigen. Diese Informationen dienen CDN Verhaltensmuster schnell erkennen. Beispielsweise mithilfe dieses Berichts können Sie welche Tage erfahrene höher oder niedriger als erwartet Datenverkehr erkennen.

Auf diese Art von Bericht generieren erhalten ein Balkendiagramm einen visuellen Hinweis, den Betrag der plattformspezifischen Nachfrage täglich vom Bericht abgedeckten Zeitraum. Dazu werden Balken für jeden Tag im Bericht angezeigt. Beispielsweise den Zeitraum auswählen "Letzte Woche" bezeichnet ein Balkendiagramm mit sieben Balken erzeugt. Jeder Balken zeigt die Gesamtanzahl der Treffer auf diesen Tag.

Der linken Seite des Diagramms (y-Achse) gibt an, wie viele Zugriffe auf das angegebene Datum ist aufgetreten. Direkt unterhalb des Diagramms (x-Achse) finden Sie eine Bezeichnung, der das Datum angibt (Format: JJJJ-MM-TT) für jeden Tag in den Bericht einbezogen.

> [AZURE.TIP] Wenn Sie auf einen Balken zeigen, wird die Gesamtanzahl der Treffer an diesem Datum als QuickInfo angezeigt.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Dort finden Sie die Gesamtanzahl der Treffer und Datenmenge (in GB) für jeden Tag im Bericht.

## <a name="by-hour"></a>Pro Stunde

Stunde-Bericht können Sie die Gesamtzahl der Zugriffe und Daten über eine Plattform auf Stundenbasis anzeigen. Diese Informationen dienen CDN Verhaltensmuster schnell erkennen. Beispielsweise mithilfe dieses Berichts können Sie die Zeiträume während des Tages erkannt, die höher oder niedriger als erwartet Datenverkehr auftreten.

Auf diese Art von Bericht generieren, stellen ein Balkendiagramm einen visuellen Hinweis über die Höhe der plattformspezifischen Bedarf stündlich vom Bericht abgedeckten Zeitraum aufgetreten. Dazu werden Balken für jede Stunde im Bericht angezeigt. Z. B. auswählen von 24 Stunden Zeit ein Balkendiagramm mit 24 generiert. Jeder Balken zeigt die Gesamtanzahl der Treffer in dieser Stunde.

Der linken Seite des Diagramms (y-Achse) gibt an, wie viele Treffer für die angegebene Stunde aufgetreten ist. Direkt unterhalb des Diagramms (x-Achse) finden Sie eine Bezeichnung, die den Zeitpunkt angibt (Format: JJJJ-MM-DD HH: mm) für jede Stunde in den Bericht einbezogen. Zeit 24 Stunden-Format angegeben und wird unter Verwendung der UTC-GMT Zeitzone angegeben.

> [AZURE.TIP] Wenn Sie auf einen Balken zeigen, wird die Gesamtanzahl der Treffer in dieser Stunde als QuickInfo angezeigt.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Dort finden Sie die Gesamtanzahl der Treffer und Datenmenge (in GB) für jede Stunde im Bericht.

## <a name="by-file"></a>Datei

Für Datei-Bericht können Sie die Anforderung und den Datenverkehr über eine Plattform für die am häufigsten angeforderten Ressourcen entstehen anzeigen. Auf diese Art von Bericht generieren ein Balkendiagramms auf die Top 10 am häufigsten angeforderten Ressourcen im angegebenen Zeitraum entstehen.

> [AZURE.NOTE] Für diesen Bericht werden Rand CNAME URLs in ihre entsprechenden URLs CDN konvertiert. Dies ermöglicht die genaue Übereinstimmung für die Gesamtzahl der Zugriffe einer Anlage unabhängig von CDN oder Kante anfordernden CNAME-URL.

Der linken Seite des Diagramms (y-Achse) gibt die Anzahl der Anfragen für jede Anlage im angegebenen Zeitraum. Direkt unterhalb des Diagramms (x-Achse) eine Bezeichnung finden Sie, die den Dateinamen für jede Top 10 angeforderten Elemente angibt.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Finden Sie die folgende Informationen für jede Top 250 angeforderte Anlagen: relativer Pfad, die Gesamtanzahl der Treffer Prozentanteil an Zugriffen, die Datenmenge (in GB) übertragen und der Prozentsatz der Daten übertragen.

## <a name="by-file-detail"></a>Datei-Detail

Von Datei detaillierten Bericht können Sie die Anforderung und den Datenverkehr über eine Plattform für eine bestimmte Anlage entstehen anzeigen. Am Anfang des Berichts steht Dateidetails für. Diese Option bietet eine Liste der am häufigsten angeforderten für die ausgewählte Plattform. Um einen Bericht von Datei Details müssen Sie die Option Details-Datei für die gewünschte Anlage aus. Ein Balkendiagramm geben die Menge der täglichen Bedarf, die innerhalb des angegebenen Zeitraums generiert.

Der linken Seite des Diagramms (y-Achse) gibt die Gesamtzahl der Anfragen, die an einem bestimmten Tag eine Anlage aufgetreten. Direkt unterhalb des Diagramms (x-Achse) finden Sie eine Bezeichnung, der das Datum angibt (Format: JJJJ-MM-TT) für die CDN Nachfrage für die Anlage gemeldet wurde.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Dort finden Sie die Gesamtanzahl der Treffer und Datenmenge (in GB) für jeden Tag im Bericht.

## <a name="by-file-type"></a>Nach Dateityp

Bericht nach Dateityp können Sie die Anforderung und den entstandenen Dateityp anzeigen. Auf diese Art von Bericht generieren zeigt ein Ringdiagramm Prozentanteil an Zugriffen, die durch die Top 10 Dateitypen generiert.

> [AZURE.TIP] Wenn der Mauszeiger ein Segment in das Ringdiagramm Medientyp Internet der Dateityp als QuickInfo angezeigt.

Die Daten zum Generieren des Diagramms Donut können darunter angezeigt werden. Finden Sie den Dateityp Namen Erweiterung Internet Medien, die Gesamtanzahl der Treffer, Prozentanteil an Zugriffen, die Datenmenge (in GB) übertragen und der Prozentsatz der Daten aller Dateitypen Top 250 übertragen.

## <a name="by-directory"></a>Vom Verzeichnis

Bericht vom Verzeichnis können Sie die Anforderung und den Datenverkehr über eine bestimmte Plattform für Inhalte aus einem bestimmten Verzeichnis entstandenen anzeigen. Auf diese Art von Bericht generieren zeigt ein Balkendiagramm die Gesamtanzahl der Treffer von Inhalt in den Top 10 generiert.

### <a name="using-the-bar-chart"></a>Balkendiagramm verwenden

* Zeigen Sie auf einen Balken an den relativen Pfad zum entsprechenden Verzeichnis.
* Berechnung bei Bedarf vom Verzeichnis zählen in einem Unterordner eines Verzeichnisses gespeicherten Inhalte nicht. Diese Berechnung stützt sich ausschließlich auf die Anzahl der Anfragen hinsichtlich Inhalt im aktuellen Verzeichnis gespeichert.
* Für diesen Bericht werden Rand CNAME URLs in ihre entsprechenden URLs CDN konvertiert. Dies ermöglicht eine genaue Tally Statistiken Anlage unabhängig von CDN oder Kante anfordernden CNAME-URL zugeordnet.

Die linke Seite des Diagramms (y-Achse) gibt die Gesamtzahl der Anfragen für den Inhalt in den Top 10-Verzeichnissen gespeichert. Jede Leiste im Diagramm stellt eine Datei dar. Verwenden des farbcodierung Schemas für Bar in ein Verzeichnis im Abschnitt oben 250 vollständige Verzeichnisse aufgeführt.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Finden Sie die folgende Informationen für jedes oben 250 Verzeichnisse: Pfad, die Gesamtanzahl der Treffer Prozentanteil an Zugriffen, die Datenmenge (in GB) übertragen und der Prozentsatz der Daten übertragen.

## <a name="by-browser"></a>Browser

Von Browser-Bericht können Sie anzeigen, welche Browser verwendeten Inhalte. Auf diese Art von Bericht generieren wird ein Kreisdiagramm den Prozentsatz der Anfragen behandelt, indem die obersten 10 Browser angeben.

### <a name="using-the-pie-chart"></a>Kreisdiagramm verwenden

* Zeigen Sie auf ein Segment im Tortendiagramm, um Name und Version des Browsers anzeigen.
* Für die Zwecke dieses Berichts gilt jede eindeutige Browserversion Kombination ein anderen Browsers.
* Das Segment "Sonstige" bezeichnet gibt den Prozentsatz von Anfragen von anderen Browser und Versionen behandelt.

Die Daten im Diagramm generieren können darunter angezeigt werden. Dort finden Sie Browser Typ/Versionsnummer, die Gesamtanzahl der Treffer und den Prozentsatz der Treffer für jeden Browser oben 250.

## <a name="by-referrer"></a>Durch Referenz

Durch Referenz-Bericht können Sie die häufigste Verweise auf Inhalte der ausgewählten Plattform an. Eine Referenz gibt den Hostnamen aus dem Anforderung generiert wurde. Auf diese Art von Bericht generiert, wird ein Balkendiagramm der Nachfrage (d. h. Treffer) von Top 10-Quellen hinweisen.

Der linken Seite des Diagramms (y-Achse) gibt die Gesamtzahl fordert Vermögenswert für jede Verweiser festgestellt. Jede Leiste im Diagramm stellt eine Referenz. Verwenden Sie das farbcodierung Schema für Bar, eine Referenz im Abschnitt oben 250 Referrer aufgeführt.

Darunter können die Daten zu dem Balkendiagramm angezeigt werden. Dort finden Sie die URL, die Gesamtanzahl der Treffer und den Prozentsatz der Treffer aus der oberen 250 Verweise generiert.

## <a name="by-download"></a>Download

Bericht herunterladen können Sie Download für die am häufigsten angeforderten Inhalte analysieren. Der Anfang des Berichts enthält ein Balkendiagramm vergleicht Downloads mit abgeschlossenen Downloads für Top 10 angeforderten Anlagen versucht. Jeder Balken ist farbcodiert, je nachdem, ob es einen versuchten Download (Blau) oder einen abgeschlossenen Download (Grün).

> [AZURE.NOTE] Für diesen Bericht werden Rand CNAME URLs in ihre entsprechenden URLs CDN konvertiert. Dies ermöglicht eine genaue Tally Statistiken Anlage unabhängig von CDN oder Kante anfordernden CNAME-URL zugeordnet.

Der linken Seite des Diagramms (y-Achse) gibt den Dateinamen für jede Top 10 angeforderten Ressourcen. Direkt unterhalb des Diagramms (x-Achse) finden Sie Etiketten, die die Gesamtanzahl der Downloads versucht/abgeschlossen zeigen.

Direkt unter dem Balkendiagramm die folgende Informationen werden angezeigt werden Top 250 angeforderten Ressourcen: relativer Pfad (einschließlich Dateiname), oft vollständig heruntergeladen wurde oft angefordert wurde und den Prozentsatz der Anfragen, die einen vollständigen Download geführt haben.

> [AZURE.TIP] Unsere CDN nicht über einen HTTP-Client (d. h. Browser) informiert Wenn Anlage vollständig heruntergeladen wurde. Daher müssen wir berechnen, ob eine Anlage Statuscodes und Byte Bereich vollständig heruntergeladen wurde Anfragen. Das erste, was wir bei dieser Berechnung wird eine 200 OK-Statuscode gibt an, ob die Anforderung führt. In diesem Fall suchen wir auf Byte-Bereich um sicherzustellen, dass sie die gesamte Anlage abdecken. Vergleichen wir schließlich Datenmenge auf die Größe der angeforderten Ressource. Gleich oder größer als die Größe der übertragenen Daten ist und Byte-Range Requests eignen sich für diese Anlage wird der Treffer als vollständige Download gezählt.
>
>Aufgrund Interpretation des Berichts sollten Sie folgende Punkte bedenken, die Konsistenz und Genauigkeit des Berichts ändern können.
>
>* Verkehrsmuster können nicht präzise vorhergesagt werden, wenn Benutzer-Agents anders verhalten. Dies kann abgeschlossenen Download Ergebnisse, die größer als 100 %.
>* Vermögenswerte, die HTTP-progressiven Download nutzen können dieser Bericht nicht exakt dargestellt werden. Benutzer, die an andere Positionen in einem Video liegt.

## <a name="by-404-errors"></a>404-Fehler

Bericht von 404-Fehler können Sie Inhalte identifizieren, die die Anzahl der Statuscodes 404 nicht gefunden wird. Der Anfang des Berichts enthält ein Balkendiagramm der 10 wichtigsten Anlagen für die 404 nicht gefunden Statuscode zurückgegeben wurde. Dieses Balkendiagramm vergleicht die Gesamtzahl der Anfragen eine 404 nicht gefunden Statuscode für die Ressourcen führte Anfragen. Jeder Balken ist farbcodiert. Eine gelbe Leiste wird verwendet, um anzugeben, dass die Anforderung in eine 404 nicht gefunden-Statuscode. Ein roter Balken wird die Gesamtzahl der Anfragen für die Anlage an.

> [AZURE.NOTE] Für die Zwecke dieses Berichts ist Folgendes zu beachten:
>
>* Eine jede Anforderung für eine Anlage unabhängig von Statuscode darstellt.
>* Rand CNAME-URLs werden in ihre entsprechenden URLs CDN konvertiert. Dies ermöglicht eine genaue Tally Statistiken Anlage unabhängig von CDN oder Kante anfordernden CNAME-URL zugeordnet.

Der linken Seite des Diagramms (y-Achse) gibt den Dateinamen für jede Top 10 angeforderte Anlagen, die einen 404 nicht gefunden-Statuscode geführt haben. Direkt unterhalb des Diagramms (x-Achse) finden Sie Etiketten, die an die Gesamtzahl der Anfragen und die Anzahl der Anfragen, die eine 404 nicht gefunden-Statuscode geführt haben.

Direkt unter dem Balkendiagramm die folgende Informationen werden angezeigt werden Top 250 angeforderten Ressourcen: relativer Pfad (einschließlich Dateiname), die Anzahl der Anfragen, die eine 404 nicht gefunden-Statuscode führte, die Gesamtanzahl der Fälle, in denen die Ressource angefordert wurde und den Prozentsatz Anfragen 404 nicht gefunden Statuscode geführt.

## <a name="see-also"></a>Siehe auch
* [Azure CDN-Übersicht](cdn-overview.md)
* [Echtzeit-Statistiken in Microsoft Azure CDN](cdn-real-time-stats.md)
* [Mit dem Regelmodul HTTP-Standardverhalten überschreiben](cdn-rules-engine.md)
* [Analysieren der Leistung](cdn-edge-performance.md)
