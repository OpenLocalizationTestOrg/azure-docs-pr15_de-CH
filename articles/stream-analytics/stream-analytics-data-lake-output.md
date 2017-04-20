<properties
    pageTitle="Stream Analytics See Datenspeicher Ausgabe | Microsoft Azure"
    description="Konfiguration der Authentifizierung und Autorisierung von einer Azure See Datenspeicher in einem Stream Analytics-Auftrag"
    keywords=""
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="stream-analytics-data-lake-store-output"></a>Stream Analytics See Datenspeicher Ausgabe

Stream Analytics-Aufträge unterstützt verschiedene Ausgabemethoden eine einen [Azure See Datenspeicher](https://azure.microsoft.com/services/data-lake-store/). Azure See Datenspeicher ist eine unternehmensweite Großkonzerne Repository für analytische Datenverlustvorfalls Arbeitslasten. See Datenspeicher können Sie jeder Größe, Typ und Aufnahme Geschwindigkeit für betriebliche und explorative Analysen speichern.

## <a name="authorize-a-data-lake-store-account"></a>Ein Datenspeicher See Konto autorisieren

1.  Wenn See Datenspeicher als Ausgabe in der Azure-Verwaltungsportal aktiviert ist, werden Sie die Verwendung der vorhandenen Datenspeicher See autorisieren oder Zugriff auf die Daten dem Speicher Vorschau über Azure-Verwaltungsportal aufgefordert.

    ![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-authorization.png)  

2.  Haben Sie bereits Zugriff auf See Datenspeicher, klicken Sie auf "Autorisieren" und für eine kurze Zeit eine Seite erscheint "Umleiten Genehmigung..." angibt. Die Seite wird automatisch geschlossen, und Sie werden mit der Seite, die Sie Datenspeicher See Ausgabe konfigurieren kann.

Wenn Sie sich nicht für Datenvorschau See Speicher angemeldet haben, können Sie auf den Link "Jetzt anmelden" angefordert oder [gestartet Anweisungen](../data-lake-store/data-lake-store-get-started-portal.md)folgen.

## <a name="configure-the-data-lake-store-output-properties"></a>Konfigurieren Sie die Ausgabeeigenschaften See Datenspeicher

Haben Sie Datenspeicher See Konto authentifiziert, können Sie die Eigenschaften für die Ausgabe See Datenspeicher konfigurieren. In der folgenden Tabelle wird die Liste der Eigenschaftennamen und ihre Beschreibung die Ausgabe See Datenspeicher konfigurieren.

<table>
<tbody>
<tr>
<td><B>EIGENSCHAFTENNAME</B></td>
<td><B>BESCHREIBUNG</B></td>
</tr>
<tr>
<td>Ausgabealias</td>
<td>Dies ist ein Anzeigename in Abfragen die Abfrageausgabe dieser Datenspeicher See leiten.</td>
</tr>
<tr>
<td>Datenspeicher See Konto</td>
<td>Der Name des Speicherkontos, in dem Sie die Ausgabe senden. Eine Dropdownliste der See Datenspeicher Konten wird angezeigt werden, das Portal angemeldete Benutzer auf zugreifen.</td>
</tr>
<tr>
<td>Pfad-Präfix Muster [<I>optional</I>]</td>
<td>Der Pfad der Datei zum Schreiben von Dateien in der angegebenen Daten See Store-Konto verwendet. <BR>{Datum} {Time}<BR>Beispiel 1: Ordner1/Protokolle / {Datum} und {Uhrzeit}<BR>Beispiel 2: Ordner1/Protokolle / {Datum}</td>
</tr>
<tr>
<td>[<I>Optional</I>] Datumsformat</td>
<td>Wenn Datum Token im Präfix Pfad verwendet wird, können Sie das Datumsformat in dem Organisation Ihrer Dateien. Beispiel: JJJJ/MM/TT</td>
</tr>
<tr>
<td>[<I>Optional</I>] Zeitformat</td>
<td>Wenn Zeit Token im Präfix Pfad verwendet wird, geben Sie das Format in dem Organisation Ihrer Dateien an Derzeit ist der einzige unterstützte Wert HH.</td>
</tr>
<tr>
<td>Ereignis-Serialisierungsformat</td>
<td>Serialisierungsformat für Daten. JSON, CSV und Avro unterstützt.</td>
</tr>
<tr>
<td>Codierung</td>
<td>Wenn CSV oder JSON formatiert eine Codierung anzugeben. UTF-8 ist derzeit das einzige unterstützte Codierungsformat.</td>
</tr>
<tr>
<td>Trennzeichen</td>
<td>Gilt nur für CSV-Serialisierung. Stream Analytics unterstützt eine Reihe von üblichen Trennzeichen für die CSV-Daten serialisieren. Unterstützte Werte sind Komma, Semikolon, Leerzeichen, Registerkarte und Balken.</td>
</tr>
<tr>
<td>Format</td>
<td>Gilt nur für JSON-Serialisierung. Getrennte Zeile gibt an, dass die Ausgabe mit jeder neuen Zeile JSON-Objekt formatiert wird. Array gibt an, dass die Ausgabe als Objektarray JSON formatiert werden.</td>
</tr>
</tbody>
</table>

## <a name="renew-data-lake-store-authorization"></a>Datenspeicher See Autorisierung erneuern

Derzeit gibt es eine Einschränkung, in denen das Authentifizierungstoken alle 90 Tage für alle Aufträge mit See Datenspeicher manuell aktualisiert werden muss. Sie müssen Ihr Konto See Datenspeicher erneut authentifizieren, wenn Sie Ihr Kennwort geändert haben, da Ihr Auftrag erstellt oder zuletzt authentifiziert wurde. Ein Symptom dieses Problems ist keine Auftragsausgabe und Fehler in der Protokolle, die Notwendigkeit der erneuten Autorisierung angibt.

Zum Beheben dieses Problems die ausgeführte Auftrag beenden und zur See Datenspeicher Ausgabe. Klicken Sie auf den Link "Autorisierung" Erneuern"und für eine kurze Zeit eine Seite erscheint"Umleiten Genehmigung..."angibt. Die Seite wird automatisch geschlossen und erfolgreich, zeigt "Autorisierung erfolgreich erneuert". Sie müssen auf "Speichern" am unteren Rand der Seite und können durch einen Neustart der Auftrag von der letzten Ausführung beendet um Datenverluste zu vermeiden.

![](media/stream-analytics-data-lake-output/stream-analytics-data-lake-output-renew-authorization.png)
