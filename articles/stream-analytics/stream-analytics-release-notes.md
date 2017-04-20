<properties 
    pageTitle="Stream Analytics Release Notes | Microsoft Azure" 
    description="Stream Analytics-Versionsinformationen" 
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>

#<a name="stream-analytics-release-notes"></a>Stream Analytics-Versionsinformationen

## <a name="notes-for-04152016-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 15/04/2016 ##

Dieses Release enthält das folgende Update.

Titel | Beschreibung
---|---
Allgemeine Verfügbarkeit für Power BI-Ausgaben  | [Power BI gibt](stream-analytics-power-bi-dashboard.md) sind jetzt erhältlich. 90 Tage Autorisierung Ablaufdatum für Power BI wurde entfernt. Weitere Informationen zu Szenarien, in denen Autorisierung muss erneuert werden, siehe Abschnitt [Erneuern Autorisierung](stream-analytics-power-bi-dashboard.md#Renew-authorization) Power BI-Dashboard erstellen.

## <a name="notes-for-03032016-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 03/03/2016 ##

Dieses Release enthält das folgende Update.

Titel | Beschreibung
---|---
Neue Stream Analytics Abfragesprache Elemente  | SAQL ist jetzt [GetType](https://msdn.microsoft.com/library/azure/mt643890.aspx "GetType MSDN-Seite"), [TRY_CAST](https://msdn.microsoft.com/library/azure/mt643735.aspx "TRY_CAST MSDN-Seite") und [REGEXMATCH](https://msdn.microsoft.com/library/azure/mt643891.aspx "REGEXMATCH MSDN-Seite").

## <a name="notes-for-12102015-release-of-stream-analytics"></a>Hinweise zur Version 10/12/2015 Stream Analytics ##

Dieses Release enthält das folgende Update.

Titel | Beschreibung
---|---
REST-API Version aktualisieren | Die REST API-Version wurde 2015-10-01 aktualisiert. Details finden in MSDN unter [Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx) und [Integration in Stream Analytics maschinelles lernen](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md).
Azure Machine Learning Integration | Mit diesem Release kommt Azure maschinelles lernen von benutzerdefinierten Funktionen zu unterstützen. Finden Sie im [Lernprogramm](stream-analytics-machine-learning-integration-tutorial.md) für Weitere Informationen sowie die [allgemeinen Blog Ankündigung](http://blogs.technet.com/b/machinelearning/archive/2015/12/10/apply-azure-ml-as-a-function-in-azure-stream-analytics.aspx).

## <a name="notes-for-11122015-release-of-stream-analytics"></a>Hinweise zur Version 11/12/2015 Stream Analytics ##

Dieses Release enthält das folgende Update.

Titel | Beschreibung
---|---
Neues Verhalten auswählen | Wählen im Stream Analytics wurde erweitert können * als ein Eigenschaftenaccessor verschachtelten Datensatz. Weitere Informationen finden Sie in [http://msdn.microsoft.com/library/mt622759.aspx](http://msdn.microsoft.com/library/mt622759.aspx "Komplexe Datentypen").

## <a name="notes-for-10222015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 22/10/2015 ##

Diese Version enthält die folgenden Updates.

Titel | Beschreibung
---|---
Zusätzliche Abfrage Sprachfeatures | Stream Analytics erweitert die Abfragesprache mit folgenden Funktionen: [ABS](https://msdn.microsoft.com/library/azure/mt574054.aspx), [CEILING](https://msdn.microsoft.com/library/azure/mt605286.aspx) [EXP](https://msdn.microsoft.com/library/azure/mt605289.aspx), [FLOOR](https://msdn.microsoft.com/library/azure/mt605240.aspx), [POWER](https://msdn.microsoft.com/library/azure/mt605287.aspx), [Zeichen](https://msdn.microsoft.com/library/azure/mt605290.aspx), [QUADRAT](https://msdn.microsoft.com/library/azure/mt605288.aspx)und [Wurzel](https://msdn.microsoft.com/library/azure/mt605238.aspx).
Aggregate Einschränkungen entfernt  | Diese Version entfernt maximal 15 Aggregate in einer Abfrage. Jetzt ist keine Beschränkung der Anzahl der Aggregate pro Abfrage.
Hinzugefügte Gruppe von System.Timestamp-Funktion | Die [GROUP BY](https://msdn.microsoft.com/library/azure/dn835023.aspx) -Funktion ermöglicht jetzt Window_type oder [System.Timestamp](https://msdn.microsoft.com/library/azure/mt598501.aspx).
Hinzugefügte OFFSET zu taumeln Hopping windows | Standardmäßig [Tumbling](https://msdn.microsoft.com/library/azure/dn835055.aspx) und [Hopping](https://msdn.microsoft.com/library/azure/dn835041.aspx) Windows keine Zeit ausgerichtet sind (1/1/0001 12:00 Uhr UTC). Der neue (optionale) Parameter 'Offsetsize' ermöglicht die Angabe einer benutzerdefinierten Offset (oder Ausrichtung).


## <a name="notes-for-09292015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 29/09/2015 ##

Diese Version enthält die folgenden Updates.

Titel | Beschreibung
---|---
Azure IoT Suite Public Preview | Stream Analytics ist in der Public Preview Azure IoT Suite enthalten.
Azure Portal integration | Neben Vorhandensein im Azure-Verwaltungsportal ist Stream Analytics jetzt in [Azure-Portal](https://azure.microsoft.com/overview/preview-portal/)integriert. Beachten Sie, dass Stream Analytics Funktionen im Vorschau-Portal derzeit eine Teilmenge der Funktionen in Azure-Verwaltungsportal ohne Browser Abfragetests angeboten output Power BI Konfiguration suchen oder erstellen neue Input und Output-Ressourcen in Abonnements Zugriff auf.
Unterstützung für DocumentDB-Ausgabe | Stream Analytics-Aufträge können jetzt [DocumentDB](https://azure.microsoft.com/services/documentdb/)ausgeben.
Unterstützung für IoT Hub Eingabe | Stream Analytics-Aufträge können jetzt Daten von IoT aufnehmen.
Zeitstempel von heterogenen Ereignisse | Ein Datenstrom mehrere Ereignistypen mit Zeitstempel in andere Felder enthält, jetzt können [Zeitstempel von](http://msdn.microsoft.com/library/mt573293.aspx) mit Ausdrücken Sie Felder für jeweils unterschiedliche Timestamp anzugeben.

## <a name="notes-for-09102015-release-of-stream-analytics"></a>Hinweise zur Version 10/09/2015 Stream Analytics ##

Diese Version enthält die folgenden Updates.

Titel|Beschreibung
---|---
Unterstützung für PowerBI|Zum Aktivieren der Freigabe von Daten mit anderen Benutzern Power BI können [PowerBI](stream-analytics-define-outputs.md#power-bi) Gruppen nun Stream Analytics-Aufträge in Ihrem Konto Power BI schreiben.

## <a name="notes-for-08202015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 20/08/2015 ##

Diese Version enthält die folgenden Updates.

Titel|Beschreibung
---|---
LETZTE Funktion |Die [letzte](http://msdn.microsoft.com/library/mt421186.aspx) Funktion steht nun in Stream Analytics Einzelvorgänge, aktivieren Sie das letzte Ereignis in einem Stream von Ereignissen innerhalb eines bestimmten Zeitrahmens abrufen.
Neue Array-Funktionen|[GetArrayElement](http://msdn.microsoft.com/library/mt270218.aspx), [GetArrayElements](http://msdn.microsoft.com/library/mt298451.aspx) und [GetArrayLength](http://msdn.microsoft.com/library/mt270226.aspx) -Array-Funktionen sind jetzt verfügbar.
Neuen Datensatz-Funktionen|Datensatz-Funktionen [GetRecordProperties](http://msdn.microsoft.com/library/mt270221.aspx) und [GetRecordPropertyValue](http://msdn.microsoft.com/library/mt270220.aspx) sind jetzt verfügbar.

## <a name="notes-for-07302015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 30/07/2015 ##

Diese Version enthält die folgenden Updates.

Titel|Beschreibung
---|---
Power BI Organisations-Id entkoppelt Azure-Id|Dieses Feature ermöglicht [Power BI-Ausgabe](stream-analytics-power-bi-dashboard.md) ASA Aufträgen Azure Kontentypen (Live Id oder Organisations-Id). Außerdem können Sie eine Organisations-Id für Ihre Azure-Konto verfügen und Autorisierung Power BI-Ausgabe eine andere verwenden.
Unterstützung für Service Bus Warteschlangen Ausgabe|[Service Bus Warteschlangen](stream-analytics-define-outputs.md#service-bus-queues) Ausgaben stehen jetzt in Stream Analytics-Aufträge.
Unterstützung für Service Bus Topics Ausgabe|[Service Bus Topics](stream-analytics-define-outputs.md#service-bus-topics) Ausgaben stehen jetzt in Stream Analytics-Aufträge.

## <a name="notes-for-07092015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 07/09/2015 ##

Diese Version enthält die folgenden Updates.


Titel|Beschreibung
---|---
Benutzerdefinierte Blob Ausgabe Partitionierung|BLOB-Speicher Ausgaben können nun einer Option, um die Häufigkeit anzugeben, Ausgabe Blobs geschrieben und Struktur und Format der Ausgabe-Pfad-Ordnerstruktur. 

## <a name="notes-for-05032015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 03/05/2015 ##

Diese Version enthält die folgenden Updates.


Titel|Beschreibung
---|---
Höhere maximale Wert Befehlsreihenfolge Toleranz|Die maximale Größe des Fensters Befehlsreihenfolge Toleranz ist jetzt 59: 59 (mm: ss)
JSON-Ausgabeformat: Zeile getrennt oder Array|Jetzt wird eine Option Ausgeben von BLOB-Speicher oder Ereignis Hub als ein Array von JSON-Objekten oder JSON-Objekte durch einen Zeilenumbruch getrennt ausgegeben. 

## <a name="notes-for-04162015-release-of-stream-analytics"></a>Hinweise zur Version von Stream Analytics 16/04/2015 ##


Titel|Beschreibung
---|---
Verzögerung in Azure Storage Kontokonfiguration|Beim Erstellen eines Auftrags Stream Analytics in einem Bereich zum ersten Mal werden Sie aufgefordert, ein neues Speicherkonto erstellen oder ein vorhandenes Konto für Überwachung Stream Analytics Jobs in diesem Bereich angeben. Aufgrund der Latenz Überwachung konfigurieren fordert erstellen einen anderen Stream Analytics Auftrags in derselben Region innerhalb von 30 Minuten das Angeben eines ein zweites Speicherkonto anstatt eine kürzlich konfigurierte in der Dropdownliste Speicherkonto überwachen. Zur Vermeidung unnötiger Speicherkonto erstellen, warten Sie 30 Minuten nach dem Erstellen eines Auftrags in einer Region zum ersten Mal vor der Bereitstellung weitere Aufträge in diesem Bereich.
Auftrag aktualisieren|Zu diesem Zeitpunkt unterstützt Stream Analytics live bearbeitet die Definition oder die Konfiguration eines laufenden Auftrags. Eingang, Ausgang, Abfrage, skalieren oder Konfiguration eines laufenden Auftrags ändern möchten, müssen Sie zuerst das Projekt beenden.
Datentypen von Datenquelle abgeleitet|Wenn eine CREATE TABLE-Anweisung nicht verwendet wird, der Eingabetyp Eingabeformat abgeleitet sind beispielsweise alle Felder von CSV Zeichenfolge. Felder müssen explizit in den richtigen Typ mithilfe der CAST-Funktion Vermeidung Typkonfliktfehler konvertiert werden.
Fehlende Felder werden als null-Werte ausgegeben.|Verweisen auf ein Feld in der Datenquelle nicht vorhanden ist, führt zu Nullwerte bei Ausgabe.
MIT muss SELECT-Anweisungen vorausgehen|In der Abfrage muss SELECT-Anweisung Unterabfragen mit Aussagen definiert.
Out of Memory-Problem|Streaming Analytics Aufträge mit einer großen Toleranz für Ereignisse außerhalb der Reihenfolge oder komplexe Abfragen verwalten, viel Zustand verursachen den Auftrag Speicher, einen Auftrag aus neu starten. Start und Stop-Operationen werden in Protokolle für den Auftrag angezeigt. Um dieses Verhalten zu vermeiden, skalieren Sie die Abfrage über mehrere Partitionen. Diese Einschränkung wird in einer zukünftigen Version von Extreme Leistungseinbußen auf betroffenen Aufträge anstelle neu zu richten.
Große Blob Eingaben ohne Nutzlast Timestamp kann Out of Memory-Problem|Große Dateien von BLOB-Speicher verbraucht möglicherweise Stream Analytics-Aufträge zum Absturz, wenn über Zeitstempel durch ein Timestamp-Feld angegeben ist. Um dieses Problem zu vermeiden, behalten Sie jedes Blob unter 10MB Größe.
SQL Datenbank Ereignis Volume Einschränkung|Bei Verwendung von SQL-Datenbank als Ausgabeziel möglicherweise sehr große Mengen an Daten Stream Analytics-Auftrags zu. Um dieses Problem zu beheben, verringern Sie den Ausgangspegel Aggregate oder Filteroperatoren oder wählen Sie Azure BLOB-Speicher oder Ereignis Hubs ein Ziel für die Ausgabe aus.
PowerBI-Datensätze können nur eine Tabelle enthalten.|PowerBI unterstützt nicht mehrere Tabellen in einem bestimmten Dataset.

## <a name="get-help"></a>Hilfe
Versuchen Sie weitere Unterstützung benötigen, [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte

- [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
- [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
- [Skalieren von Azure Stream Analytics-Aufträge](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST-API-Referenz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
 
