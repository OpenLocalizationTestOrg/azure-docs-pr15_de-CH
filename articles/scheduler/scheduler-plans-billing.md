<properties
 pageTitle="Pläne und Fakturierung in Azure Scheduler"
 description="Pläne und Fakturierung in Azure Scheduler"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="deli"/>

# <a name="plans-and-billing-in-azure-scheduler"></a>Pläne und Fakturierung in Azure Scheduler

## <a name="job-collection-plans"></a>Auftrag Auflistung Pläne

Job-Sammlungen sind berechenbare Entität in Azure Scheduler. Job-Sammlungen enthalten eine Reihe von Aufträgen und in drei unten beschriebenen Pläne – frei, Standard- und Premium.

|**Arbeitsplan-Auflistung**|**Max. Anzahl von Aufträgen pro Auftragsauflistung**|**Max. Serie**|**MAX-Auftrags-Sammlungen pro Abonnement**|**Grenzen**|
|:---|:---|:---|:---|:---|
|**Frei**|5 Stellen pro auftragsauflistung|Einmal pro Stunde. Aufträge können nicht öfter als einmal pro Stunde ausgeführt werden.|Ein Abonnement kann bis zu 1 kostenlose Job-Auflistung|[Ausgehende HTTP-Berechtigungsobjekt](scheduler-outbound-authentication.md) kann nicht verwendet werden.
|**Standard**|50 Aufträge pro auftragsauflistung|Einmal pro Minute. Aufträge können nicht öfter als einmal pro Minute ausgeführt werden.|Ein Abonnement kann bis zu 100 standard-Job-Sammlungen|Zugriff auf alle Funktionen des Planers|
|**P10 Premium**|50 Aufträge pro auftragsauflistung|Einmal pro Minute. Aufträge können nicht öfter als einmal pro Minute ausgeführt werden.|Ein Abonnement kann bis zu 10.000 P10 Premium Job Sammlungen. <a href="mailto:wapteams@microsoft.com">Kontaktieren Sie uns</a> mehr.|Zugriff auf alle Funktionen des Planers|
|**P20 Premium**|1000 Aufträge pro auftragsauflistung|Einmal pro Minute. Aufträge können nicht öfter als einmal pro Minute ausgeführt werden.|Ein Abonnement kann bis zu 10.000 P20 Premium Job Sammlungen. <a href="mailto:wapteams@microsoft.com">Kontaktieren Sie uns</a> mehr.|Zugriff auf alle Funktionen des Planers|

## <a name="upgrades-and-downgrades-of-job-collection-plans"></a>Upgrades und Downgrades Auftrag Auflistung Pläne

Upgrade oder downgrade einer Auflistung Arbeitsplan jederzeit unter frei, Standard- und Premium-Pläne. Allerdings kann beim Herunterstufen auf eine freie Stelle Auflistung das Downgrade aus einem der folgenden Gründe fehlschlagen:

- Eine freie Stelle Sammlung besteht bereits im Abonnement
- Job in der Job-Auflistung wurde erneut als bei freien Stelle Sammlungen. Die maximal in einer freien Stelle Auflistung zulässig ist einmal pro Stunde
- Gibt es mehr als 5 Jobs in der Job-Auflistung
- Ein Projekt in der Job-Auflistung ist eine HTTP oder HTTPS Aktion, die eine [ausgehende Berechtigungsobjekt HTTP](scheduler-outbound-authentication.md) verwendet

## <a name="billing-and-azure-plans"></a>Abrechnung und Azure-Pläne

Abonnements werden nicht kostenlos Job Sammlungen erhoben. Haben Sie 100 standard Job Sammlungen (10 standard Abrechnung Einheiten), ist es viel besser Zusatzplanes auftragssammlungen enthalten.

Haben Sie einen standard-Job und einer Premium Auftrag sind berechnet einen standard Abrechnung _und_ einer Premium Abrechnung Einheit. Der Scheduler Service Stücklisten basierend auf der Anzahl aktive Sammlungen, die auf Standard oder Premium festgelegt; Dies wird ausführlich in den nächsten beiden Abschnitten.

## <a name="standard-billable-units"></a>Berechenbare Einheiten

Berechenbare Standardeinheit kann bis zu 10 standard Job Sammlungen enthalten. Da eine standard-Job Auflistung bis zu 50 Aufträge pro auftragsauflistung unterstützt, ermöglicht ein standard Abrechnung ein Abonnement von bis zu 500 Arbeitsplätze – bis fast 22 Millionen auftragsausführungen pro Monat.

Haben Sie zwischen 1 und 10 standard Auftrag werden Sie 1 standard Abrechnung Einheit abgerechnet. Haben Sie zwischen 11 und 20 standard-Job, werden Sie für 2 standard Abrechnung Einheiten berechnet. Haben Sie 21 bis 30 standard Job Sammlungen für 3 Abrechnung Standardeinheiten berechnet werden und so weiter.

## <a name="p10-premium-billable-units"></a>P10 Berechenbare Premium-Einheiten

Eine berechenbare P10 Premium-Einheit kann bis zu 10.000 P10 Premium Job Sammlungen enthalten. Da eine Auflistung P10 Premium Auftrag bis zu 50 Aufträge pro auftragsauflistung unterstützt, ermöglicht eine Premium Rechnungsadresse ein Abonnement bis zu 500.000 Arbeitsplätze – bis zu 22 Milliarden auftragsausführungen pro Monat.

Haben Sie zwischen 1 und 10.000 Premium Auftrag werden Sie für 1 P10 Abrechnung Einheit abgerechnet. Haben Sie zwischen 10.001 und 20.000 Premium Job Sammlungen für 2 P10 Premium Abrechnung Einheiten berechnet werden und so weiter.

Folglich P10 Premium auftragssammlungen haben dieselbe Funktionen wie die standard-Job-Sammlungen aber einen Preisnachlass bei Ihrer Anwendung viel Arbeit Sammlungen erfordert bereitstellen.

## <a name="p20-premium-billable-units"></a>P20 Berechenbare Premium-Einheiten

Eine P20 berechenbare Einheit kann bis zu 5.000 P20 Premium Job Sammlungen enthalten. Da eine Auflistung P20 Premium Auftrag bis 1.000 Aufträge pro auftragsauflistung unterstützt, ermöglicht eine Prämie Abrechnung ein Abonnement von bis zu 5.000.000 Aufträge – bis 220 Milliarden auftragsausführungen pro Monat.

P20 Premium Job Sammlungen bietet dieselben Funktionen wie P10 Premium Job Sammlungen unterstützt aber auch größere Anzahl Aufträge pro auftragssammlung und insgesamt mehr Aufträge insgesamt als P10 Premium, so dass Sie mehr skalierbar.

## <a name="billing-and-active-status"></a>Abrechnung und aktiven Status

Job-Sammlungen sind immer aktiv, wenn Ihre gesamten Abonnements in einige temporäre deaktiviert durch Abrechnung Probleme gegangen. Die einzige Möglichkeit, sicherzustellen, dass eine auftragsauflistung nicht belastet wird entweder es _frei_ Plan oder auftragsauflistung löschen festgelegt.

Obwohl Sie können alle Aufträge in einer auftragsauflistung in einem einzigen Vorgang deaktivieren Fakturierungsstatus Job-Auflistung nicht geändert wird, wird der Auftrag _noch_ in Rechnung gestellt werden. Auch leere Stelle Sammlungen werden als aktiv und werden.

## <a name="pricing"></a>Preisgestaltung

Preise, finden Sie im [Planer Preise](https://azure.microsoft.com/pricing/details/scheduler/).

## <a name="see-also"></a>Siehe auch


 [Was ist der Taskplaner?](scheduler-intro.md)

 [Azure Scheduler Konzepte, Terminologie und Entitätshierarchie](scheduler-concepts-terms.md)

 [Erste Schritte mit Planer im Azure-portal](scheduler-get-started-portal.md)

 [Azure Scheduler REST-API-Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler-PowerShell-Cmdlets verweisen](scheduler-powershell-reference.md)

 [Azure Scheduler hohe Verfügbarkeit und Zuverlässigkeit](scheduler-high-availability-reliability.md)

 [Azure Scheduler Grenzen, Standards und Fehlercodes](scheduler-limits-defaults-errors.md)

 [Azure Scheduler ausgehende Authentifizierung](scheduler-outbound-authentication.md)
