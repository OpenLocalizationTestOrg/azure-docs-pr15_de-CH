<properties 
    pageTitle="Anfordern von Einheiten in DocumentDB | Microsoft Azure" 
    description="Enthält Informationen zu verstehen, angeben und Anforderung Anforderungen in DocumentDB zu schätzen." 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>Einheiten in DocumentDB anfordern
Jetzt verfügbar: DocumentDB [Anforderung einheitenumrechner](https://www.documentdb.com/capacityplanner). [Schätzung der Durchsatz benötigt](documentdb-request-units.md#estimating-throughput-needs)Weitere.

![Durchsatz Rechner][5]

##<a name="introduction"></a>Einführung
Dieser Artikel bietet eine Übersicht über [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)anforderungseinheiten. 

Nach dem Lesen dieses Artikels werden Sie folgenden Fragen beantworten:  

-   Was sind Einheiten fordert und Gebühren?
-   Wie Geben Sie Anforderung Einheit Kapazität einer Auflistung
-   Wie schätze ich, dass meine Anwendung Anforderung muss?
-   Was geschieht, wenn ich Anforderung Einheit für eine Auflistung übersteigt?


##<a name="request-units-and-request-charges"></a>Einheiten fordert und Gebühren
DocumentDB bietet schnelle, zuverlässige Performance zu befriedigen Durchsatz der Anwendung Ressourcen *Reservieren* .  Da Anwendung laden und ändern mit der Zeit kann DocumentDB leicht vergrößern oder Verkleinern der reservierten Durchsatz der Anwendung verfügbar.

Mit DocumentDB wird reserviert Durchsatz Anforderung Einheiten pro Sekunde verarbeiten angegeben.  Sie können anforderungseinheiten als Durchsatz Währung vorstellen bei für die Anwendung pro Sekunde verfügbaren anforderungseinheiten eine Menge *zu reservieren garantiert* .  Jede Operation in ein Dokument, eine Abfrage, ein Dokument aktualisieren - DocumentDB nutzt CPU, Speicher und IO.  Jede Operation ist also *Anforderung Belastung*, die *anforderungseinheiten*ausgedrückt wird.  Verständnis der Faktoren, die Anforderung Einheit Zuschläge der Anwendung Durchsatz Anforderungen auswirken, können Sie Ihre Anwendung möglichst kostengünstige ausgeführt. 

##<a name="specifying-request-unit-capacity"></a>Anforderung Einheit Kapazität angeben
Beim Erstellen einer Auflistung DocumentDB Geben Sie die Stückzahl Anforderung pro Sekunde (RUs) für die Sammlung reserviert werden soll.  Nachdem die Auflistung erstellt wird, wird die vollständige Zuweisung von RUs angegeben für die Auflistung reserviert.  Jede Auflistung garantiert zu dedizierter Durchsatz Eigenschaften isoliert.  

Es ist wichtig zu beachten, dass DocumentDB auf ein Modell Reservierung; also werden berechnet für den Durchsatz *reserviert* für die Sammlung, unabhängig davon, wie viel dieser Durchsatz aktiv *verwendet*.  Beachten Sie jedoch, der Anwendung laden, Daten und Verwendung ändern, nach der Datenmenge problemlos skaliert werden kann, reserviert RUs über DocumentDB-SDKs oder der [Azure-Portal](https://portal.azure.com).  Informationen auf Durchsatz auf-und abwärts skalieren finden Sie unter [Leistung DocumentDB](documentdb-performance-levels.md).

##<a name="request-unit-considerations"></a>Aspekte der Einheit anfordern
Wenn die Anzahl der anforderungseinheiten für die DocumentDB Auflistung reservieren schätzen, unbedingt berücksichtigen die folgenden Variablen:

- **Dokumentgröße**. Zunehmender Dokumentgröße Einheiten zum Lesen oder schreiben die Daten erhöht auch verbraucht.
- **Anzahl der Dokument-Eigenschaft**. Wenn Standard Indizieren aller Eigenschaften der Einheiten verwendet, um ein Dokument zu schreiben wird zunehmender Count-Eigenschaft erhöht.
- **Datenkonsistenz**. Bei Ebenen Konsistenz Data starke oder Alterung begrenzt werden zusätzliche Einheiten verbraucht Dokumente lesen.
- **Indizierte Eigenschaften**. Eine Index-Richtlinie für jede Auflistung bestimmt Eigenschaften standardmäßig indiziert werden. Sie reduzieren den Anforderung Einheitenverbrauch durch Beschränken der Anzahl der indizierten Eigenschaften oder verzögerte Indizierung aktivieren.
- **Dokument mit einem Index**. Standardmäßig alle Dokumente automatisch indiziert, verbrauchen Sie weniger anforderungseinheiten möchten Sie nicht einige Ihrer Dateien indizieren.
- **Abfragemuster**. Die Komplexität der Abfrage beeinträchtigt, wie viele Einheiten mit Anforderung für einen Arbeitsgang verbraucht werden. Der Anzahl von Prädikaten, Art der Prädikate, Projektionen, Anzahl der UDF und die Größe der Quelle Daten alle beeinflussen Kosten Abfrage.
- **Skript-Syntax**.  Mit Abfragen verwenden gespeicherte Prozeduren und Trigger anforderungseinheiten je nach Komplexität der Operationen. Überprüfen Sie während der Anwendungsentwicklung Anforderungsheader Zuschlag zum besseren Verständnis wie jede Operation Anforderung Einheit Kapazität verbraucht.

##<a name="estimating-throughput-needs"></a>Schätzung Durchsatz benötigt
Eine Anforderung Einheit misst eine normalisierte Verarbeitungskosten Anforderung. Eine einzelne Anforderung darstellt Verarbeitungskapazität erforderlich (per Self-Link-Id) ein einzelnes 1 KB JSON Dokument 10 eindeutige Werte (ausgenommen Systemeigenschaften) lesen. Eine Anforderung (Einfügen) erstellen, ersetzen oder löschen Sie das gleiche Dokument verbrauchen weitere Verarbeitung aus dem Dienst und damit mehr anforderungseinheiten.   

> [AZURE.NOTE] Basisplan 1 Anforderung Einheit für ein Dokument 1KB entspricht einer einfachen GET selbst verknüpfen oder Id des Dokuments.

###<a name="use-the-request-unit-calculator"></a>Verwenden Sie den Rechner Anforderung Einheit
Können Kunden fein abstimmen ihrer Einschätzung Durchsatz, Web-basierte [Anfrage einheitenumrechner](https://www.documentdb.com/capacityplanner) soll die Anforderung Anforderungen für normale Vorgänge schätzen:

- Dokument erstellt (Schreiben)
- Dokument liest
- Dokument wird gelöscht
- Aktualisierte Dokumente

Das Tool unterstützt auch schätzen Speicherbedarf Daten basierend auf Beispieldokumente, die Sie bereitstellen.

Das Tool ist einfach:

1. Hochladen Sie eine oder mehrere repräsentative JSON-Dokumente.

    ![Anforderung einheitenumrechner Dokumente hochladen][2]

2. Um Speicherplatz für Daten zu schätzen, geben Sie die Gesamtzahl der Dokumente, die gespeichert werden soll.

3. Geben Sie die Nummer des Dokuments erstellen, lesen, aktualisieren und löschen (auf einer Basis pro Sekunde) erforderlich. Um Anforderung Einheit Abgaben Dokument Aktualisierungsvorgänge zu schätzen, laden Sie eine Kopie des Beispieldokuments aus Schritt 1 oben, die normalen Feld Updates enthält.  Z. B. Dokument Updates normalerweise zwei Eigenschaften LastLogin und UserVisits ändern, dann einfach das Dokument kopieren, Werte für diese beiden Eigenschaften und das kopierte Dokument hochladen.

    ![Geben Sie durchsatzanforderungen der Anforderung Einheit des Rechners][3]

4. Klicken Sie auf berechnen und untersuchen die Ergebnisse.

    ![Einheit Rechner Ergebnisse anfordern][4]

>[AZURE.NOTE]Haben Sie Dokumenttypen die Größe und die Anzahl der indizierten Eigenschaften erheblich abweichen, laden Sie ein Beispiel für jeden *Typ* von normalen Dokument zum Tool und dann berechnet.

###<a name="use-the-documentdb-request-charge-response-header"></a>Verwenden der DocumentDB Anforderung zu-/ Antwort-header
Jede Antwort von DocumentDB-Service umfasst einen benutzerdefinierten Header (X-ms-Anforderung-Gebühr), der für die Anforderung verbrauchten anforderungseinheiten enthält. Dieser Header ist auch über DocumentDB-SDKs. In .NET SDK ist RequestCharge eine Eigenschaft des ResourceResponse-Objekts.  Für Abfragen bietet DocumentDB Abfrage Explorer im Azure-Portal Anforderungsinformationen für ausgeführten Abfragen.

![RU Zuschläge im Abfrage-Explorer überprüfen][1]

Sie erwarten pro Sekunde ausführen, dabei beachten, eine Methode zur Schätzung reservierte Durchsatz der Anwendung ist mit normalen Betrieb an einem repräsentativen Dokument von der Anwendung verwendeten Gebühr Anforderung Gerät aufzeichnen und dann die Anzahl der Vorgänge geschätzt.  Unbedingt messen und normalen Abfragen und Skript-Syntax sowie DocumentDB.

>[AZURE.NOTE]Haben Sie Dokumenttypen die Größe und die Anzahl der indizierten Eigenschaften erheblich abweichen, notieren Sie sich die jeweiligen Vorgang Anforderung Einheit Gebühr jeden *Typ* von normalen Dokument.

Zum Beispiel:

1. Aufzeichnen eines (Einfügen) Zuschlag pro Einheit Anforderung einem normalen Dokument. 
2. Aufzeichnen von einem normalen Dokument Zuschlag pro Einheit Anforderung.
3. Zeichnen Sie Aktualisieren von einem normalen Dokument Zuschlag pro Einheit Anforderung auf
3. Datensatz und allgemeine Dokument Abfragen Zuschlag pro Einheit Anforderung.
4. Aufzeichnen der Anforderung Einheit Ihres benutzerdefinierten Skripts (gespeicherte Prozeduren, Trigger, benutzerdefinierte Funktionen) von der Anwendung genutzt
5. Berechnen der erforderlichen anforderungseinheiten die geschätzte Anzahl der Vorgänge pro Sekunde ausführen erwarten.

##<a name="a-request-unit-estimation-example"></a>Eine Anforderung – Schätzung-Beispiel
Betrachten Sie das folgende Dokument ca. 1KB:

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]Dokumente werden in DocumentDB minimierte das System berechnet die oben genannten Dokument ist etwas weniger als 1KB.


Die folgende Tabelle zeigt ungefähre Anforderung Einheit Zuschläge für Standardvorgänge für dieses Dokument (Zuschlag pro Einheit ungefähre Anforderung setzt voraus, dass Konsistenz Kontoebene "Session" festgelegt ist und alle Dokumente automatisch indiziert werden):

Vorgang|Zuschlag pro Einheit anfordern 
---|---
Dokument erstellen|~ 15 RU 
Dokument lesen|~ 1 RU
Abfragedokument nach id|~2.5 RU

Darüber hinaus zeigt diese Tabelle ungefähre Anforderung Einheit Kosten normalerweise in der Anwendung verwendeten Abfragen:

Abfrage|Zuschlag pro Einheit anfordern|# Der zurückgegebene Dokumente
---|---|--- 
Wählen Sie Lebensmittel nach id|~2.5 RU|1 
Lebensmittel vom Hersteller auswählen|~ 7 RU|7
Wählen Sie Gruppe und Reihenfolge nach Gewicht|~ 70 RU|100
Top 10 Lebensmittel in einer Gruppe auswählen|~ 10 RU|10

>[AZURE.NOTE]RU Gebühren variieren basierend auf der Anzahl der gefundenen Dokumente.

Mit diesen Informationen kann abgeschätzt RU an dieser Anwendung die Anzahl der Vorgänge und Abfragen pro Sekunde erwarten:

Abfrage-Vorgang|Geschätzte Anzahl pro Sekunde|Erforderliche RUs 
---|---|--- 
Dokument erstellen|10|150 
Dokument lesen|100|100 
Lebensmittel vom Hersteller auswählen|25|175 
Gruppe auswählen|10|700 
Top 10 auswählen|15|150 gesamt|155|1275

In diesem Fall erwarten wir eine durchschnittliche Durchsatzes 1275 RU-s.  Rundung der nächsten 100 würden wir 1.300 RU/s für diese Anwendung Auflistung bereitstellen.

##<a id="RequestRateTooLarge"></a>Über reservierte Durchsatz Grenzwerte
Beachten Sie, dass Anforderung Einheitenverbrauch als ein Wert pro Sekunde ausgewertet wird. Für Applikationen, die bereitgestellte Einheit Anforderungsrate für eine Auflistung überschreiten, werden Anfragen an diese Auflistung eingeschränkt bis Rate reservierte Niveau fällt. Tritt ein Server die Anforderung mit RequestRateTooLargeException (HTTP-Statuscode 429) und zeigt die Zeitspanne in Millisekunden, die der Benutzer warten muss, bevor die Anforderung Problem X-ms-Retry-nach-ms-Header zurückgegeben.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Verwenden Sie .NET Client SDK und LINQ-Abfragen und in den meisten Fällen müssen Sie nie mit dieser Ausnahme als die aktuelle Version von .NET Client SDK implizit diese Antwort abfängt, berücksichtigt den Server angegeben Retry-after-Header und wiederholt die Anforderung. Sofern Ihr Konto von mehreren Clients gleichzeitig zugegriffen wird, wird der nächste Versuch erfolgreich ausgeführt.

Haben Sie mehr als einen Client kumulativ über die Anforderungsrate wiederholen Standardverhalten kann nicht ausreichen und Client löst ein DocumentClientException mit dem Statuscode 429 zur Anwendung. In diesem Fall sollten Sie behandeln Wiederholverhalten und Logik in Ihre Anwendung Fehler Behandlung Routinen oder erhöhen des reservierten Durchsatzes für die Auflistung.

##<a name="next-steps"></a>Nächste Schritte

Erkunden Sie erfahren Sie mehr über reservierte Durchsatz mit Azure DocumentDB Datenbanken:
 
- [DocumentDB Preise](https://azure.microsoft.com/pricing/details/documentdb/)
- [DocumentDB Kapazität](documentdb-manage.md) 
- [Modellieren von Daten in DocumentDB](documentdb-modeling-data.md)
- [DocumentDB-Leistung](documentdb-partition-data.md)

Weitere Informationen zu DocumentDB finden Sie unter Azure DocumentDB [Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/). 

Finden Sie zunächst mit Skalierung und Performance-Tests mit DocumentDB [Performance und Skalierung mit Azure DocumentDB testen](documentdb-performance-testing.md).


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
