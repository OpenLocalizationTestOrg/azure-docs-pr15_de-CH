<properties
   pageTitle="Interagieren mit der JavaScript-API mit | Microsoft Azure"
   description="Interagieren mit der JavaScript-API verwenden, Strom eingebettete BI"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="interact-with-power-bi-reports-using-the-javascript-api"></a>Interaktion mit Power BI-Berichten mit der JavaScript-API

Der Power BI-JavaScript-API können Sie problemlos Power BI-Berichten in Ihre Anwendung einbetten. Die API können Anwendung programmgesteuert mit verschiedenen Seiten wie Filter interagieren. Diese Interaktivität macht Power BI Berichte mehr Bestandteil der Anwendung.

Power BI-Bericht wird in der Anwendung mithilfe ein Iframe gehostet wird als Teil der Anwendung einbetten. Iframe fungiert als Berandung zwischen der Anwendung und den Bericht wie in der folgenden Abbildung sehen. 

![Power BI eingebetteten Iframe ohne Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-1.png)

Iframe erleichtert den Einbetten von Prozess, aber ohne die JavaScript-API den Bericht und die Anwendung können nicht miteinander. Diese mangelnde Interaktion können das Gefühl der Bericht nicht wirklich Teil der Anwendung ist. Der Bericht und die Anwendung müssen hin und her, wie im folgenden Bild kommunizieren.

![Power BI eingebetteten Iframe mit Javascript-API](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-2.png)

Der Power BI-JavaScript-API können Sie Code schreiben, der sicher Iframe Grenze passieren kann. Dadurch kann die Anwendung programmgesteuert eine Aktion in einem Bericht und Ereignisse Aktionen überwachen, die Benutzer innerhalb des Berichts.

## <a name="what-can-you-do-with-the-power-bi-javascript-api"></a>Was können Sie mit der JavaScript-API von Power BI?
Der JavaScript-API können Sie Berichte verwalten, navigieren zu Seiten in einem Bericht, Filtern eines Berichts und Einbetten Ereignisse behandeln. Das folgende Diagramm zeigt die Struktur der API.

![Power BI JavaScript API-Diagramm](media\powerbi-embedded-interact-with-reports\powerbi-embedded-interact-report-3.png)


### <a name="manage-reports"></a>Verwalten von Berichten
Der Javascript-API können Sie Verhalten Bericht und Seite:

- Einen bestimmten Power BI-Bericht sicher in Ihre Anwendung einbetten - versuchen Sie [Demo-Anwendung einbetten](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  - Zugriffstoken festlegen
- Den Bericht konfigurieren
  - Aktivieren und Deaktivieren der Filterbereich und Seitennavigationsbereich - versuchen [Einstellungen demoanwendung aktualisieren](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  - Festlegen von Standardeinstellungen für Seiten und Filter - versuchen Sie [legen Standardwerte Demo](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
- Betreten und Verlassen des Vollbildmodus

[Weitere Informationen zum Einbetten von einem Bericht](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)


### <a name="navigate-to-pages-in-a-report"></a>Navigieren Sie zu Seiten in einem Bericht
Einzelne JavaScript-API alle Seiten in einem Bericht zu ermitteln und die aktuelle Seite. Versuchen Sie die [Navigation Demo-Anwendung](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[Erfahren Sie mehr über Seitennavigation](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Filtern eines Berichts
Der JavaScript-API bietet grundlegende und erweiterte Filterfunktionen für eingebettete Berichte und Berichtsseiten. Versuchen Sie die [demoanwendung Filtern](http://azure-samples.github.io/powerbi-angular-client/#/scenario4)und überprüfen Sie einige einführende Code.  


#### <a name="basic-filters"></a>Einfacher Filter
Ein einfacher Filter wird Spalte oder Hierarchie und enthält eine Liste von Werten ein-oder ausschließen.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Erweiterte Filter
Erweiterten Filter verwenden, den logischen Operator und oder oder und ein oder zwei Zustände mit Operator und Wert. Unterstützte sind:

- Keine
- LessThan
- LessThanOrEqual
- Größer als
- GreaterThanOrEqual
- Enthält
- DoesNotContain
- "StartsWith"
- DoesNotStartWith
- Ist
- IsNot
- ISTLEER
- IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[Weitere Informationen zum Filtern](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)


### <a name="handling-events"></a>Behandeln von Ereignissen
Zusätzlich Informationen in den Iframe, erhalten die Anwendung auch Informationen über die folgenden Iframe aus:

- Einbinden
  - geladen
  - Fehler
- Berichte
  - pageChanged
  - DataSelected (in Kürze verfügbar)

[Weitere Informationen zum Behandeln von Ereignissen](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Power BI-JavaScript-API finden Sie die folgenden Links:

- [JavaScript-API-Wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
- [Referenz zum Objektmodell](https://microsoft.github.io/powerbi-models/modules/_models_.html)
- Beispiele
  - [Winkel](http://azure-samples.github.io/powerbi-angular-client)
  - [Asche](https://github.com/Microsoft/powerbi-ember)
- [Live-demo](https://microsoft.github.io/PowerBI-JavaScript/demo/)
