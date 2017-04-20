<properties
    pageTitle="Ressourcenverwendung API Mieter | Microsoft Azure"
    description="Referenz zum Ressourcenverbrauch-API die Azure Stapel Nutzungsinformationen abrufen."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Ressourcenverwendung API Mieter

Ein Mieter können Mandanten-API Daten des Mieters Ressource anzeigen. Diese API entspricht Azure Verwendung API (aktuell in privaten Vorschau).

Das Windows PowerShell-Cmdlet **Get-UsageAggregates** können Sie Daten wie in Azure abrufen.

## <a name="api-call"></a>API-Aufruf

### <a name="request"></a>Anforderung

Die Anforderung wird Verbrauch Details die gewünschten Abonnements und den gewünschten Zeitrahmen. Es gibt keine Anforderung.

| **-Methode**  | **Anfrage-URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Erhalten         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumente

| **Argument**             | **Beschreibung** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure Ressourcenmanager Endpunkt der Azure-Stack-Umgebung. |
| *subId*                   | Abonnement-ID des Benutzers, der den Aufruf ausführt. Diese API nur Abfrage können Sie ein einzelnes Abonnement dar. Anbieter können Provider Ressource: Einsatz-API Abfrage Nutzung für alle. |
| *reportedStartTime*       | Startzeit der Abfrage. Der Wert für *' DateTime '* sollte in UTC und am Anfang der Stunde beispielsweise 13:00. Legen Sie diesen Wert auf Mitternacht UTC, tägliche Aggregation. Das Format ist ISO 8601, z. B. 2015-06-16T18 % 3a53 % 3a11 % 2b00 % 3a00Z, *geschützt* , Doppelpunkt, % 3a geschützt und plus ist % 2 b geschützt, sodass angezeigten URI ist. |
| *reportedEndTime*         | Endzeit der Abfrage. Die Integritätsregeln für *ReportedStartTime* gelten gelten auch für dieses Argument. Der Wert für *ReportedEndTime* darf nicht in der Zukunft sein. |
| *aggregationGranularity*  | Optionaler Parameter, der zwei separate mögliche Werte: täglich und stündlich. Wie die Werte in Tagesschritten Daten zurückgibt und das andere ist ein stündlicher Auflösung. Die Option "täglich" ist die Standardeinstellung. |
| *subscriberId*            | Abonnement-ID. Gefilterte Daten muss die Abonnement-ID eines direkten Mandanten des Anbieters. Wenn kein Abonnement-ID-Parameter angegeben ist, gibt der Aufruf Verwendungsdaten für direkte Mieter des Anbieters. |
| *API-version*             | Version des Protokolls für diese Anforderung verwendet wird. 2015-06-01-Vorschau verwenden. |
| *continuationToken*       | Token vom letzten Aufruf Verwendung API-Provider abgerufen. Dies ist erforderlich, wenn eine Antwort größer als 1.000 Zeilen. Dies ist das Lesezeichen für den Fortschritt. Sofern nicht vorhanden, Datenabruf ab dem Tag oder Stunde, basierend auf der Reihenfolge übergeben. |

### <a name="response"></a>Antwort

Abrufen /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & ReportedEndTime = 2015-06-01T00 % 3a00 % 3a00 % 2b00 % 3a00 & AggregationGranularity = täglich & API-Version = 1.0

{

"Wert":\[

{

"Id": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"Name": "sub1 meterID1"

"Typ": "Microsoft.Commerce/UsageAggregate"

"Eigenschaften": {

"SubscriptionId": "sub1"

"UsageStartTime": "2015-03-03T00:00:00 + 00:00",

"UsageEndTime": "2015-03-04T00:00:00 + 00:00",

"InstanceData": "{\\" Microsoft.Resources\\": {\\" ResourceUri\\":\\" resourceUri1\\",\\" Speicherort\\":\\" Alaska\\",\\" Tags\\": null\\" AdditionalInfo\\": null}}",

"Menge":2.4000000000

"MeterId": "meterID1"

}

},

…

### <a name="response-details"></a>Details der Antwort

| **Argument**      | **Beschreibung** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*              | Eindeutige ID des Aggregats Verwendung |
| *Name*            | Name des Aggregats Verwendung |
| *Typ*            | Ressourcendefinition |
| *subscriptionId*  | Abonnement-ID des Benutzers Azure |
| *usageStartTime*  | UTC-Startzeit der Verwendung Bucket dieses Aggregat Verwendung gehört |
| *usageEndTime*    | UTC-Endzeit der Verwendung Bucket dieses Aggregat Verwendung gehört |
| *instanceData*    | Schlüssel-Wert-Paare Instanzdetails (in einem neuen Format):<br>  *ResourceUri*: vollständig qualifizierte Ressourcen-ID, einschließlich Ressourcengruppen und Instanznamen <br>  *Ort*: Region, in dem dieser Dienst ausgeführt wurde, <br>  *Tags*: vom Benutzer Resource-Tags <br>  *AdditionalInfo*: Weitere details zu der Ressource, die verbraucht, z. B. Betriebssystem Version oder Bild |
| *Menge*        | Betrag der Ressourcenverbrauch, die in diesem Zeitraum aufgetreten |
| *meterId*         | Eindeutige Kennung für die Ressource, die verbraucht (auch als *ResourceID*) |

## <a name="next-steps"></a>Nächste Schritte

[Verwendung-bezogene häufig gestellte Fragen](azure-stack-usage-related-faq.md)
