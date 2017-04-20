<properties
    pageTitle="Fragen zum Verbrauch | Microsoft Azure"
    description="Liste der Azure-Stapel Meter, Vergleich zu Azure Verwendung API, Nutzung und gemeldete Zeit Fehlercodes."
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

# <a name="azure-stack-usage-api-faqs"></a>Azure Stapel Verwendung API FAQs
Dieser Artikel beantwortet einige häufig gestellten Fragen zur Azure Stapel Verwendung API.

## <a name="what-meter-ids-can-i-see"></a>Welche Anzeige IDs werden angezeigt?

Derzeit wird die Verwendung für Netzwerk, Speicher und Compute Ressourcenprovider gemeldet.

| **Ressourcenproviders** | **Anzeige-ID** |**Anzeige-Namen** | **Einheit** | **Zusätzliche Informationen** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **Netzwerk** | f114cb19-ea64-40b5-bcd7-aee474b62853 | Öffentliche IP-Adressverwendung | IP-Adresse |                    
| **Speicher**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*Stunden | Tabellen belegten Gesamtkapazität |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*Stunden | Seitenblobs belegten Gesamtkapazität |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*Stunden  | Warteschlange belegten Gesamtkapazität |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*Stunden  | Block-Blobs belegten Gesamtkapazität |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | Anzahl in 10.000 s   | Tabelle Serviceanfragen (in 10.000 s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | Eingehende Daten in GB | Tabelle-Dienst Daten Eindringen in GB |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress GB | Tabelle-Dienst Daten Ausgang in GB |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | Anzahl von Anfragen in 10.000 s | BLOB-Serviceanfragen (in 10.000 s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | Eingehende Daten in GB          | BLOB-Dienst Daten Eindringen in GB 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress GB              | BLOB-Dienst Daten Ausgang in GB 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | Anzahl von Anfragen in 10.000 s   | Warteschlange für Service-Requests (im 10.000 s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | Eingehende Daten in GB         | Warteschlange Dienst Daten Eindringen in GB 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress GB  | Warteschlange Dienst Daten Ausgang in GB 
| **Berechnen** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | VM Größe Stunden | Größe des virtuellen Computers |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Vergleichen Azure Stapel Verwendung APIs wie [Azure Verwendung API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) (aktuell in public Preview)?

-   Mandanten Verwendung-API entspricht der Azure-API, mit einer Ausnahme: *ShowDetails* -Flag ist derzeit nicht in Azure Stapel unterstützt.

-   Anbieter Verwendung API gilt nur für Azure Stapel.

-   Die [RateCard-API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx) , die in Azure ist derzeit nicht verfügbar in Azure Stapel.

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>Was ist der Unterschied zwischen Verbrauch und gemeldet?

Verwendungsberichte Daten haben zwei Zeit-Werte:

-   **Zeit**. Die Zeitangabe bei verwendungsereignis Verwendung system

-   **Zeitpunkt der Verwendung**. Die Zeit als Ressource Azure Stapel wurde

Für ein Ereignis die Verwendung möglicherweise eine Diskrepanz Werte für Verbrauch und gemeldet angezeigt werden. Die Verzögerung kann mehrere Stunden in jeder Umgebung.

Derzeit können Sie *nur von gemeldeten Zeit*Abfragen.

## <a name="what-do-these-usage-api-error-codes-mean"></a>Was bedeuten diese Fehlercodes Verwendung API?

| **HTTP-Statuscode** | **Fehlercode** | **Beschreibung** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400-Ungültige Anforderung        | *NoApiVersion*     | Abfrageparameter *API-Version* ist nicht vorhanden.
| 400-Ungültige Anforderung        | *InvalidProperty*  | Eine Eigenschaft fehlt oder weist einen ungültigen Wert. Die Nachricht in den Fehlercode im Antworttext bezeichnet die fehlende Eigenschaft.
| 400-Ungültige Anforderung        | *RequestEndTimeIsInFuture*  | Der Wert für *ReportedEndTime* ist in der Zukunft. Werte dürfen nicht in der Zukunft für dieses Argument.
| 400-Ungültige Anforderung        | *SubscriberIdIsNotDirectTenant*    | Anbieter-API-Aufruf verwendet eine Abonnement-ID, die nicht gültigen Mieter des Aufrufers.
| 400-Ungültige Anforderung        | *SubscriptionIdMissingInRequest*   | Die Abonnement-ID des Aufrufers fehlt.
| 400-Ungültige Anforderung        | *InvalidAggregationGranularity*   | Es wurde eine ungültige Granularität. Gültige Werte sind täglich und stündlich.
| 503                    | *ServiceUnavailable*   | Behebbarer Fehler der Dienst ist überlastet oder der Aufruf ist eingeschränkt. |

## <a name="next-steps"></a>Nächste Schritte
[Kunden und Chargeback in Azure Stapel](azure-stack-billing-and-chargeback.md)

[Anbieter-Ressourcenverwendung API](azure-stack-provider-resource-api.md)

[Ressourcenverwendung API Mieter](azure-stack-tenant-resource-usage-api.md)
