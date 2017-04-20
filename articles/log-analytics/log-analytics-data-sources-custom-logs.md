<properties 
   pageTitle="Benutzerdefinierte Anmeldung Protokollanalyse | Microsoft Azure"
   description="Protokollanalyse können Ereignisse aus Textdateien auf Windows- und Linux-Computern erfassen.  Dieser Artikel beschreibt, wie ein neues benutzerdefiniertes Protokoll und Details der Datensätze, die sie in OMS-Repository erstellen."
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

# <a name="custom-logs-in-log-analytics"></a>Benutzerdefinierte Protokolle Protokollanalyse

Benutzerdefinierte Protokolle Datenquelle in Protokollanalyse können Sie Ereignisse aus Textdateien auf Windows- und Linux-Computern erfassen. Vielen Protokollinformationen in Textdateien statt Standarddienste Syslog oder Windows-Ereignisprotokoll protokollieren.  Gesammelt, können Sie jeden Eintrag im Protokoll in einzelne Felder die [Felder](log-analytics-custom-fields.md) Funktion der Protokollanalyse analysieren.

![Benutzerdefiniertes Protokoll-Auflistung](media/log-analytics-data-sources-custom-logs/overview.png)

Die Protokolldateien gesammelt werden müssen die folgenden Kriterien übereinstimmen.

- Das Protokoll muss entweder einen einzelnen Eintrag pro Zeile oder verwenden einen Zeitstempel, der einem der folgenden Formate am Anfang jedes Eintrags.

    YYYY-MM-DD HH: MM: <br>
  TT HH: MM: SS UHR <br>
  Mon TT JJJJ hh: mm:
    
- Die Protokolldatei darf nicht kreisförmige Updates, die Datei mit neuen Einträgen überschrieben. 

## <a name="defining-a-custom-log"></a>Definieren ein benutzerdefiniertes Protokoll

Gehen Sie folgendermaßen vor, um eine benutzerdefinierte Protokolldatei zu definieren.  Blättern Sie zum Ende dieses Artikels eine exemplarische Vorgehensweise ein Beispiel für ein benutzerdefiniertes Protokoll hinzufügen.

### <a name="step-1-open-the-custom-log-wizard"></a>Schritt 1. Öffnen Sie den Assistenten benutzerdefinierten Protokoll

Assistenten für die benutzerdefinierte Protokoll wird im Portal OMS und können Sie ein neues benutzerdefiniertes Protokoll sammeln definieren.

1.  Gehen Sie im Portal OMS **Settings**.
2.  Klicken Sie auf **Daten** und **benutzerdefinierte Protokolle**.
3.  Standardmäßig werden alle Neukonfiguration automatisch an alle Agenten abgelegt.  Für Linux-Agenten wird eine Konfigurationsdatei Datensammlungsprogramm Fluentd gesendet.  Möchten Sie diese Datei auf jedem Linux-Agent manuell ändern, dann deaktivieren Sie das Kontrollkästchen *unter Konfiguration meiner Linux-Computer anwenden*.
4.  Klicken Sie auf **Hinzufügen** zum Öffnen des Assistenten benutzerdefinierten Protokoll.

### <a name="step-2-upload-and-parse-a-sample-log"></a>Schritt 2. Laden Sie und analysieren Sie ein Protokoll

Sie starten ein Beispiel für das benutzerdefinierte Protokoll hochladen.  Der Assistent analysieren und Anzeigen der Einträge in dieser Datei überprüfen.  Protokollanalyse verwendet das Trennzeichen, das zum Identifizieren aller Datensätze angeben.

**Neue Zeile** wird als Standardtrennzeichen und Protokolldateien, die einen einzelnen Eintrag pro Zeile verwendet.  Die Zeile beginnt mit Datum und Uhrzeit in einem der verfügbaren Formate, können Sie ein Trennzeichen **Zeitstempel** angeben Einträge unterstützt, die mehr als eine Zeile umfassen. 

Wenn Zeitstempel Trennzeichen verwendet wird, wird die TimeGenerated-Eigenschaft eines jeden einzelnen Datensatz OMS mit den für diesen Eintrag in der Protokolldatei angegeben Zeit aufgefüllt.  Wenn eine neue Zeile Trennzeichen verwendet wird, wird TimeGenerated mit Datum und Uhrzeit Protokollanalyse erfasst den Eintrag gefüllt. 

>[AZURE.NOTE]Derzeit behandelt Protokollanalyse erfasst aus einem Protokoll UTC Timestamp Trennzeichen unter Datum und Uhrzeit.  Dadurch wird die Zeitzone auf dem Agent verwenden schnell geändert werden. 
 
1.  Klicken Sie auf **Durchsuchen** , und suchen Sie eine Beispieldatei.  Hinweis Diese Schaltfläche kann möglicherweise in einigen Browsern **Aus Datei** gekennzeichnet.
2.  Klicken Sie auf **Weiter**. 
3.  Benutzerdefinierte Protokoll-Assistent die Datei hochladen und Liste der Datensätze, die sie identifiziert.
4.  Ändern Sie das Trennzeichen, das zur Identifizierung neuen Datensatzes und wählen Sie das Trennzeichen, das die Einträge in der Protokolldatei am besten identifiziert.
5.  Klicken Sie auf **Weiter**.

### <a name="step-3-add-log-collection-paths"></a>Schritt 3. Protokoll Auflistung Pfade hinzufügen

Sie müssen einen oder mehrere Pfade auf dem Agent definieren, benutzerdefinierte Protokoll gefunden  Sie können entweder einen Pfad und einen Namen für die Protokolldatei, oder geben Sie einen Pfad mit einem Platzhalter für den Namen.  Unterstützt Programme, die eine neue Datei erstellen, jeden Tag oder wenn eine Datei eine bestimmte Größe erreicht.  Sie können auch mehrere Pfade für eine einzelne Protokolldatei angeben.

Beispielsweise kann eine Anwendung erstellen eine Protokolldatei täglich mit dem Datum der Name in log20100316.txt. Ein Muster für ein Protokoll möglicherweise *Protokoll\*.txt* gelten Protokolldatei nach Anwendung würde der Namensschema.

Die folgende Tabelle enthält Beispiele für gültige verschiedene Protokolldateien angeben. 

| Beschreibung | Pfad |
|:--|:--|
| Alle Dateien im *C:\Logs* mit Erweiterung auf Windows-agent | C:\Logs\\\*.txt |
| Alle Dateien im *C:\Logs* mit einem Namen Protokoll mit einer Erweiterung auf Windows-agent | C:\Logs\log\*.txt |
| Alle Dateien im */var/log/audit* mit der Erweiterung TXT auf Linux-agent | /var/log/Audit/*.txt |
| Alle */var/log/audit* mit einem Namen Protokoll mit einer Erweiterung auf Linux-agent | /var/log/Audit/Log\*.txt |
  

1.  Wählen Sie Windows oder Linux Pfadformat angeben, das Sie hinzufügen.
2.  Geben Sie den Pfad und klicken Sie auf die **+** Schaltfläche.
3.  Wiederholen Sie den Vorgang für weitere Pfade.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>Schritt 4. Geben Sie einen Namen und eine Beschreibung für das Protokoll

Der angegebene Name wird für des Typs verwendet werden, wie oben beschrieben.  Er endet immer mit _CL als ein benutzerdefiniertes Protokoll zu unterscheiden.

1.  Geben Sie einen Namen für das Protokoll.  Die ** \_CL** Suffix automatisch bereitgestellt.
2.  Eine optionale **Beschreibung**hinzufügen.
3.  Klicken Sie auf **Weiter** um die Definition des benutzerdefinierten Protokoll gespeichert.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>Schritt 5. Überprüfen Sie, ob die benutzerdefinierte Protokolle gesammelt werden
Stunde der ersten Daten dauern aus ein neues benutzerdefiniertes Protokoll bis Protokollanalyse angezeigt werden.  Es wird sammeln Einträge aus den Protokollen finden im Pfad angegebene mit das benutzerdefinierte Protokoll definiert.  Die Einträge, die Sie bei der Erstellung von benutzerdefinierten Protokoll hochgeladen werden nicht beibehalten, aber sammelt bereits vorhandene Einträge in den Protokolldateien gesucht.

Zeichenvorgang Protokollanalyse aus dem benutzerdefinierten Protokoll werden Datensätze mit Protokoll suchen.  Verwenden Sie den Namen, das benutzerdefinierte Protokoll als **Typ** in der Abfrage gab.

>[AZURE.NOTE] Fehlt die Rohdaten-Eigenschaft der Suche, müssen Sie schließen und den Browser neu starten.

### <a name="step-6-parse-the-custom-log-entries"></a>Schritt 6. Die benutzerdefinierten Protokolleinträge analysieren

Der gesamte Eintrag wird in eine einzelne Eigenschaft namens **RawData**gespeichert werden.  Sie möchten wahrscheinlich trennen Sie die einzelnen Informationen in jedem Eintrag in einzelne Eigenschaften im Datensatz gespeichert.  Sie verwenden diese [Felder](log-analytics-custom-fields.md) Bestandteil Protokollanalyse.

Eine ausführliche Anleitung zum Analysieren des benutzerdefinierten Protokolleintrags sind hier nicht verfügbar.  Siehe [Felder](log-analytics-custom-fields.md) Dokumentation weitere Informationen.

## <a name="disabling-a-custom-log"></a>Deaktivieren ein benutzerdefiniertes Protokoll

Sie können keine benutzerdefiniertes Protokoll-Definition wird erstellt wurde, aber alle Pfade Auflistung entfernen deaktivieren entfernen.

1.  Gehen Sie im Portal OMS **Settings**.
2.  Klicken Sie auf **Daten** und **benutzerdefinierte Protokolle**.
3.  Klicken Sie auf **Details** neben der Definition benutzerdefiniertes Protokoll deaktivieren.
4.  Entfernen Sie die Auflistung Pfade für benutzerdefiniertes Protokoll-Definition.


## <a name="data-collection"></a>Datensammlung

Protokollanalyse erfasst neue Einträge aus jeder benutzerdefinierten Protokoll etwa 5 Minuten.  Der Agent wird stattdessen in jeder Protokolldatei aufzeichnen, die aus erfasst.  Wenn der Agent einen Zeitraum offline geht, dann erfasst Protokollanalyse Einträge aus dem letzten, auch wenn die Posten erstellt wurden, während der Agent offline war.

Der gesamte Inhalt des Protokolleintrags werden in eine einzelne Eigenschaft namens **RawData**geschrieben.  Sie können dies in mehreren Eigenschaften analysieren, die analysiert und [Benutzerdefinierte Felder](log-analytics-custom-fields.md) definieren, nach der Erstellung des benutzerdefinierten Protokolls getrennt durchsucht werden kann.


## <a name="custom-log-record-properties"></a>Eigenschaften von benutzerdefinierten Protokoll

Benutzerdefinierte Datensätze haben einen Protokollnamen, den Sie bereitstellen und die Eigenschaften in der folgenden Tabelle.

| Eigenschaft | Beschreibung |
|:--|:--|
| TimeGenerated | Erstelldatum und-Uhrzeit des Datensatzes durch Protokollanalyse erfasst wurden.  Nutzt das Protokoll Zeit-Trennzeichen ist dies Zeit Eintrag erfasst. |
| SourceSystem  | Typ des Agenten gesammelten des Datensatzes aus. <br> OpsManager-Windows Agent, direkt verbinden oder SCOM <br> Linux-Linux-Agenten  |
| Rohdaten             | Vollständiger Eintrag erfasst. |
| "Verwaltungsgruppenname" | Name der Verwaltungsgruppe für SCOM-Agenten.  Für andere Agenten ist AOI -\<Arbeitsplatz-ID\> |


## <a name="log-searches-with-custom-log-records"></a>Suchvorgänge mit benutzerdefinierten Protokolleinträge Protokoll

Benutzerdefinierte Protokolle werden im Repository OMS wie Datensätze aus einer anderen Datenquelle gespeichert.  Sie haben einen Typ mit dem Namen, den Ihnen bei die Type-Eigenschaft in der Suche Verwendung zum Abrufen der Datensätze aus einem bestimmten Protokoll erfasst das Protokoll definieren.

Die folgende Tabelle bietet unterschiedliche Beispiele für Protokoll Suche, die Abrufen von Datensätzen aus benutzerdefinierten Protokolle.

| Abfrage | Beschreibung |
|:--|:--|
| Typ = MyApp_CL | Alle Ereignisse von einer benutzerdefinierten protokollieren benannten MyApp_CL |
| Typ = MyApp_CL Severity_CF = Fehler | Alle Ereignisse aus einem benutzerdefinierten Protokoll mit dem Namen MyApp_CL mit *Fehler* in einem benutzerdefinierten Feld namens *Severity_CF*. |




## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Exemplarische Vorgehensweise Beispiel ein benutzerdefiniertes Protokoll hinzufügen

Im folgenden Abschnitt führt durch ein benutzerdefiniertes Protokoll erstellen.  Beispielprotokoll gesammelt hat ein einzelner Eintrag für jede Zeile mit einer Datums- und Uhrzeitangabe und dann Komma Felder für Code, Status und Nachricht getrennt.  Nachfolgend einige Beispiele für Einträge.

    2016-03-10 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2016-03-10 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2016-03-10 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2016-03-10 01:38:22 302,Error,Application could not connect to database
    2016-03-10 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Laden Sie und analysieren Sie ein Protokoll

Wir können bieten die Protokolldateien und die Ereignisse, die sie sammeln.  In diesem Fall ist die neue Position genügend Trennzeichen.  Wenn ein einzelner Eintrag in das Protokoll über mehrere Zeilen umfassen kann, müssten ein Timestamp-Trennzeichen verwendet werden.

![Laden Sie und analysieren Sie ein Protokoll](media/log-analytics-data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Protokoll Auflistung Pfade hinzufügen

Die Protokolldateien befinden sich *C:\MyApp\Logs*.  Eine neue Datei wird mit einem Namen erstellt, die das Datum im Muster *appYYYYMMDD.log*enthält.  Eine ausreichende Muster für dieses Protokoll *C:\MyApp\Logs\\\*. log*.

![Pfad der Log-Auflistung](media/log-analytics-data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Geben Sie einen Namen und eine Beschreibung für das Protokoll

Wir verwenden ein *MyApp_CL* und eine **Beschreibung**.

![Protokollname](media/log-analytics-data-sources-custom-logs/log-name.png)


### <a name="validate-that-the-custom-logs-are-being-collected"></a>Überprüfen Sie, ob die benutzerdefinierte Protokolle gesammelt werden

Verwenden wir eine Abfrage der *Type = MyApp_CL* aus dem Protokoll erfasst alle Datensätze zurückgegeben.

![Protokollabfrage ohne benutzerdefinierte Felder](media/log-analytics-data-sources-custom-logs/query-01.png)

### <a name="parse-the-custom-log-entries"></a>Die benutzerdefinierten Protokolleinträge analysieren

Wir verwenden benutzerdefinierte Felder definieren *EventTime*, *Code*, *Status*und *Nachrichtenfelder* aus und sehen den Unterschied in den Datensätzen, die von der Abfrage zurückgegeben werden.

![Protokollabfrage mit benutzerdefinierten Feldern](media/log-analytics-data-sources-custom-logs/query-02.png)

## <a name="next-steps"></a>Nächste Schritte

- Verwenden Sie [Benutzerdefinierte Felder](log-analytics-custom-fields.md) , Einträge in das benutzerdefinierte Protokoll in einzelne Felder zu analysieren.
- Erfahren Sie mehr über [Protokoll suchen](log-analytics-log-searches.md) Daten aus Datenquellen und Solutions analysieren. 