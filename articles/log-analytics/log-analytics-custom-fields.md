<properties
   pageTitle="Benutzerdefinierte Felder Protokollanalyse | Microsoft Azure"
   description="Die Felder der Protokollanalyse Funktion durchsuchbare Felder aus OMS-Daten erstellen, die die Eigenschaften des erfassten Datensatz hinzufügen.  Dieser Artikel beschreibt die Vorgehensweise zum Erstellen eines benutzerdefinierten Felds und bietet eine ausführliche exemplarische Vorgehensweise ein Samplingereignis."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="custom-fields-in-log-analytics"></a>Benutzerdefinierte Felder Protokollanalyse

**Benutzerdefinierte Felder** Bestandteil Protokollanalyse können Sie vorhandene Datensätze im Repository OMS erweitern durchsuchbare Felder.  Benutzerdefinierte Felder werden automatisch mit Daten von anderen Eigenschaften desselben Datensatzes aufgefüllt.

![Übersicht über benutzerdefinierte Felder](media/log-analytics-custom-fields/overview.png)

Beispielsweise hat das Beispiel unten nützliche Daten in die Beschreibung des Ereignisses enthalten.  Extrahieren diese Daten in separaten Eigenschaften kann es Aktionen wie das Sortieren und filtern.

![Melden Schaltfläche Suchen an](media/log-analytics-custom-fields/sample-extract.png)

>[AZURE.NOTE] In der Vorschau sind Sie 100 benutzerdefinierte Felder im Arbeitsbereich auf.  Dieser Grenzwert wird dieses Feature allgemeinen Verfügbarkeit erreicht erweitert.

## <a name="creating-a-custom-field"></a>Erstellen eines benutzerdefinierten Felds

Wenn Sie ein benutzerdefiniertes Feld erstellen, müssen Protokollanalyse die Daten zum Füllen des Werts verstehen.  Eine Technologie von Microsoft Research namens FlashExtract verwendet, um Daten schnell identifizieren.  Und Sie müssen keine explizite Anweisungen, erfährt Protokollanalyse Daten extrahieren von Beispielen, die Sie bereitstellen möchten.

Die folgenden Abschnitte enthalten das Verfahren zum Erstellen eines benutzerdefinierten Felds.  Unten in diesem Artikel ist eine Anleitung eine Extraktion der Probe.

> [!NOTE] Das benutzerdefinierte Feld wird ausgefüllt, Datenspeicher OMS den eingegebenen Kriterien übereinstimmende Datensätze hinzugefügt werden, damit nur angezeigt werden, Datensätze gesammelt, nachdem das benutzerdefinierte Feld erstellt wurde.  Das benutzerdefinierte Feld wird keine Datensätze hinzugefügt werden, die bereits im Datenspeicher sind bei der Erstellung.

### <a name="step-1--identify-records-that-will-have-the-custom-field"></a>Schritt 1: Ermitteln der Datensätze, die das benutzerdefinierte Feld
Der erste Schritt ist die Datensätze identifiziert, die das benutzerdefinierte Feld erhalten.  Sie starten mit einer [standard-Protokoll suchen](log-analytics-log-searches.md) und wählen Sie einen Datensatz als Modell fungieren, die Protokollanalyse von erfahren.  Wenn Sie angeben, dass Sie Daten in einem benutzerdefinierten Feld extrahieren möchten, wird Sie überprüfen und passen Sie die Kriterien **Feld Extrahier-Assistent** geöffnet.

2. Zum **Protokoll suchen** und Verwenden einer [Abfrage zum Abrufen der Datensätze](log-analytics-log-searches.md) , die das benutzerdefinierte Feld.
2. Wählen Sie einen Datensatz, mit dem Protokollanalyse als Beispiel für das Extrahieren von Daten, um das benutzerdefinierte Feld aufzufüllen.  Erkennt die Daten aus diesem Datensatz extrahiert und Protokollanalyse mit diesen Informationen bestimmen die Logik für das benutzerdefinierte Feld für alle ähnlichen Datensätzen aufzufüllen.
3. Klicken Sie links neben jeder Text-Eigenschaft des Datensatzes und wählen Sie **Felder extrahieren**.
4. **Feld Extrahier-Assistent wird geöffnet**, und der ausgewählte Datensatz wird in der Spalte **Main wird** angezeigt.  Das benutzerdefinierte Feld wird für die Datensätze mit den gleichen Werten in den Eigenschaften definiert werden, die ausgewählt sind.  
5. Wenn die Auswahl nicht was Sie gegebenenfalls zusätzliche Felder Kriterien einschränken ist.  Um die Feldwerte für die Kriterien ändern Sie Abbrechen und wählen Sie einen anderen Datensatz den gewünschten Kriterien entsprechen.

### <a name="step-2---perform-initial-extract"></a>Schritt 2 - führen anfänglichen extrahieren.
Nachdem Sie die Datensätze, die das benutzerdefinierte Feld identifiziert haben, geben Sie die Daten, die Sie extrahieren möchten.  Protokollanalyse verwendet diese Informationen ähnlich wie Datensätze identifizieren.  Nach diesem Schritt werden Sie die Ergebnisse überprüfen und weitere Details für Protokollanalyse in der Analyse verwenden können.

1. Markieren Sie den Text im Beispieleintrag, die das benutzerdefinierte Feld aufzufüllen.  Sie werden in einem Dialogfeld einen Namen für das Feld und führen anfänglichen extrahieren angezeigt.  Zeichen ** \_CF** automatisch angefügt.
2. Klicken Sie auf **extrahieren** , um eine Analyse der gesammelten Datensätze.  
3. Abschnitt **Zusammenfassung** und **Suchergebnisse** zeigt die Ergebnisse des Extrakts damit ihre Richtigkeit überprüfen können.  **Zusammenfassung** zeigt die Kriterien zur Identifizierung von Datensätzen und Anzahl aller Datenwerte identifiziert.  **Ergebnisse** liefert eine detaillierte Liste der Datensätze, die den Kriterien entsprechen.


### <a name="step-3--verify-accuracy-of-the-extract-and-create-custom-field"></a>Schritt 3 – Überprüfen der Genauigkeit des Extrakts und benutzerdefiniertes Feld erstellen

Anfängliche extrahieren durchgeführt haben, zeigt Protokollanalyse die Ergebnisse auf Grundlage bereits gesammelten Daten.  Wenn die Ergebnisse korrekt, können Sie das benutzerdefinierte Feld keine weitere Arbeit erstellen.  Wenn dies nicht der Fall ist, Sie können dann die Ergebnisse verfeinern, Protokollanalyse seine Logik zu verbessern.

2.  Wenn alle Werte in den ersten korrekt sind, klicken Sie auf das Symbol **Bearbeiten** neben einem ungenauen Datensatz und wählen Sie **ändern diese Markierung** um die Auswahl zu ändern.
3.  Der Eintrag wird im Abschnitt **Beispiele** unter **Main wird**kopiert.  Die Hervorhebung passen Protokollanalyse Auswahl haben es sollten verstehen helfen.
4.  Klicken Sie auf **extrahieren** , um diese neuen Informationen verwenden, um die vorhandenen Datensätze auszuwerten.  Die Ergebnisse können Datensätze als die gerade geänderte basierend auf diese neue Intelligenz geändert werden.
5.  Weitere Korrekturen hinzu, bis alle Datensätze in den Daten, um das neue benutzerdefinierte Feld aufzufüllen korrekt.
6. Klicken Sie auf **Extrahieren speichern** , wenn Sie mit dem Ergebnis zufrieden sind.  Das benutzerdefinierte Feld jetzt definiert, aber wird nicht Sie noch Datensätze hinzugefügt werden.
7.  Warten Sie auf neue Datensätze die angegebenen Kriterien erfasst und die protokollsuche erneut ausführen. Neue Datensätze müssen das benutzerdefinierte Feld.
8.  Verwenden Sie das benutzerdefinierte Feld wie Datensatz-Eigenschaft.  Sie können sie Daten aggregieren und gruppieren und sogar neue Erkenntnisse zu verwenden.


## <a name="viewing-custom-fields"></a>Benutzerdefinierte Felder anzeigen
Sie können eine Liste aller benutzerdefinierten Felder in der Verwaltungsgruppe von der Kachel **Einstellungen** des OMS-Dashboards anzeigen.  Wählen Sie eine Liste aller benutzerdefinierten Felder im Arbeitsbereich **Daten** und **benutzerdefinierte Felder** .  

![Benutzerdefinierte Felder](media/log-analytics-custom-fields/list.png)

## <a name="removing-a-custom-field"></a>Entfernen eines benutzerdefinierten Felds
Gibt es zwei Arten ein benutzerdefiniertes Feld zu entfernen.  Die erste wird die Option **Entfernen** für jedes Feld eine vollständige Liste anzeigen, wie oben beschrieben.  Die Methode ist einen Datensatz abrufen und klicken Sie auf die Schaltfläche neben dem Feld.  Klicken Sie im Menü haben die Möglichkeit, das benutzerdefinierte Feld zu entfernen.

## <a name="sample-walkthrough"></a>Beispiel Exemplarische Vorgehensweise

Im folgenden Abschnitt führt durch ein vollständiges Beispiel für ein benutzerdefiniertes Feld erstellen.  In diesem Beispiel extrahiert den Dienstnamen in Windows Ereignisse, die einen Dienst Veränderung.  Dies beruht auf Ereignisse vom Service Control Manager im Systemprotokoll auf Windows-Computern erstellt werden.  Möchten Sie dieses Beispiel, müssen Sie [Ereignisse im Systemprotokoll Informationen sammeln](log-analytics-data-sources-windows-events.md).

Wir geben die folgende Abfrage zurückgeben aller Ereignisse vom Service Control Manager, die Ereignis-ID 7036 das Ereignis darstellt, einen Dienst starten oder anhalten.

![Abfrage](media/log-analytics-custom-fields/query.png)

Wir wählen alle Datensätze mit Ereignis-ID 7036.

![Quelldatensatz](media/log-analytics-custom-fields/source-record.png)

Wir möchten den Dienstnamen, der in der Eigenschaft **RenderedDescription** und klicken Sie neben dieser Eigenschaft.

![Felder extrahieren](media/log-analytics-custom-fields/extract-fields.png)

**Feld Extrahier-Assistent** wird geöffnet und **EventLog** **EventID** Feld in **Main** Beispielspalte ausgewählt sind.  Dies bedeutet, dass das benutzerdefinierte Feld für Ereignisse in das Systemprotokoll mit Ereignis-ID 7036 definiert werden.  Dies ist ausreichend, müssen wir keine Felder aus.

![Beispiel](media/log-analytics-custom-fields/main-example.png)

Wir markieren Sie den Namen des Dienstes in der **RenderedDescription** -Eigenschaft und **Dienst** den Namen zu verwenden.  Das benutzerdefinierte Feld wird **Service_CF**aufgerufen.

![Titel](media/log-analytics-custom-fields/field-title.png)

Wir sehen, dass der Dienstname für einige Datensätze für andere jedoch nicht korrekt angegeben ist.   Die **Ergebnisse** zeigen, dass Teil für den **WMI-Leistungsadapter** ausgewählt war.  Die **Zusammenfassung** zeigt, dass vier Datensätze mit **DPRMA** ein zusätzliches Wort falsch enthalten zwei Datensätze identifiziert **Modulinstallation** statt **Windows Modules Installer**.  

![Suchergebnisse](media/log-analytics-custom-fields/search-results-01.png)

Wir beginnen mit der **WMI-Leistungsadapter** .  Wir klicken Sie auf dessen Bearbeitungssymbol und **ändern diese hervorheben**.  

![Markierung ändern](media/log-analytics-custom-fields/modify-highlight.png)

Wir erhöhen die Markierung das Wort **WMI** und dann erneut das Extrahieren.  

![Weiteres Beispiel](media/log-analytics-custom-fields/additional-example-01.png)

Wir sehen, dass die Einträge für **WMI-Leistungsadapter** korrigiert und Protokollanalyse diese Informationen auch korrigieren Sie die Einträge für **Windows Installer-Modul**.  Wir sehen im Abschnitt **Zusammenfassung** , obwohl diese **DPMRA** noch nicht ordnungsgemäß identifiziert wird.

![Suchergebnisse](media/log-analytics-custom-fields/search-results-02.png)

Wir zum einen Datensatz mit den DPMRA und verwenden Sie dieselbe Vorgehensweise, um diesen Datensatz zu korrigieren.

![Weiteres Beispiel](media/log-analytics-custom-fields/additional-example-02.png)

 Wenn die Extraktion ausgeführt wird, sehen wir, dass alle Ergebnisse jetzt richtig sind.

![Suchergebnisse](media/log-analytics-custom-fields/search-results-03.png)

Wir sehen, dass **Service_CF** erstellt wird, aber noch nicht auf alle Datensätze hinzugefügt.

![Anfängliche Anzahl](media/log-analytics-custom-fields/initial-count.png)

Nach einiger Zeit so gesammelten Ereignisse, sehen wir, dass Feld **Service_CF** jetzt hinzugefügt wird entsprechen Kriterien werden.

![Endergebnis](media/log-analytics-custom-fields/final-results.png)

Wir können nun das benutzerdefinierte Feld wie jeder andere Datensatz.  Um dies zu veranschaulichen, erstellen wir eine Abfrage, die das neue **Service_CF** Feld überprüfen Gruppen Dienste besonders aktiv sind.

![Abfrage gruppieren](media/log-analytics-custom-fields/query-group.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Abfragen mithilfe benutzerdefinierter Felder Kriterien erstellen.
- [Benutzerdefinierte Dateien](log-analytics-data-sources-custom-logs.md) , die Sie analysieren, benutzerdefinierte Felder zu überwachen.