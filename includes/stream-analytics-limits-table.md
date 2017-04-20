<properties 
   pageTitle="Stream Analytics Grenzwerte Tabelle"
   description="Beschreibt die Systemgrenzen und empfohlene Größen für Stream Analytics Komponenten und Anschlüsse."
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| Limit-Bezeichner | Grenzwert       | Kommentare |
|----------------- | ------------|--------- |
| Maximale Anzahl von Streaming-Einheiten pro Abonnement pro region | 50 | Zu Streaming-Einheiten für Ihr Abonnement über 50 können angefordert werden [Microsoft-Support](https://support.microsoft.com/en-us)wenden. |
| Maximalen Durchsatz der Streaming-Einheit | 1MB / s * | Maximaler Durchsatz pro SU hängt vom Szenario ab. Durchsatz geringer und hängt Abfrage Komplexität und Partitionierung. Einzelheiten finden im Artikel [Skalierung Azure Stream Analytics-Aufträge den Durchsatz erhöhen](../articles/stream-analytics/stream-analytics-scale-jobs.md) . |
| Maximale Anzahl der Eingänge pro Auftrag | 60 | Gibt es eine feste Grenze von 60 Eingänge pro Stream Analytics-Auftrag. |
| Maximale Anzahl der Ergebnisse pro Auftrag | 60 | Gibt es eine feste Grenze von 60 Ausgaben pro Stream Analytics-Auftrag. |
| Maximale Anzahl von Funktionen pro Auftrag | 60 | Gibt es eine feste Grenze von 60 Funktionen pro Stream Analytics-Auftrag. |
| Maximale Anzahl von Aufträgen pro region | 1500 | Jedes Abonnement möglicherweise bis zu 1500 Arbeitsplätze geografischen Regionen. |