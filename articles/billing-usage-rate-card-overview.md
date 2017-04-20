<properties
   pageTitle="Einblicke in Ihre Microsoft Azure Ressourcenverbrauch | Microsoft Azure"
   description="Bietet eine konzeptionelle Übersicht der Azure Abrechnung und RateCard-APIs, die Einblicke in Azure Ressourcenverbrauch und Trends verwendet werden."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Einblicke in Ihre Microsoft Azure Ressourcenverbrauch

Kunden und Partner müssen exakt zu prognostizieren und Azure Kosten verwalten.  Sie eine Capex ein Opex Modell bewegen, benötigen sie auch die Möglichkeit, kostenauflistung und Chargeback-Analyse führen, sowie Modus Treue Vorkalkulation und Abrechnungsinformationen, insbesondere bei großen Cloud-Bereitstellungen.

Azure Ressourcenverwendung und Rate Karte APIs erläutert diese Bedürfnisse in dieser Adresse Artikel neue Einblicke in Ihre Azure Ressourcenverbrauch aktivieren.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Einführung in Azure Ressourcenverwendung und RateCard-APIs

Azure Ressourcenverwendung und RateCard APIs werden als Ressource als Teil der Familie von Azure-Ressourcen-Manager verfügbaren APIs implementiert.  

### <a name="azure-resource-usage-api-preview"></a>Azure Ressourcenverwendung API (Vorschau)
Kunden und Partner können Azure Ressource: Einsatz API ihre geschätzte Azure Daten erhalten. Merkmale:

- **Azure Role-based Access Control** - Kunden und Partner können ihre Richtlinien in [Azure-Portal](https://portal.azure.com) oder über [Azure PowerShell-Cmdlets](powershell-install-configure.md) an, welche Benutzer oder Anwendung auf das Abonnement Daten zugreifen können. Aufrufer müssen standard Azure Active Directory Token zur Authentifizierung verwenden. Der Aufrufer muss auch die Leser, Besitzer oder Mitwirkender Rolle Zugriff auf Daten für einen bestimmten Azure-Abonnement hinzugefügt werden.

- **Stündlich oder täglich Aggregationen** - Aufrufer können angeben, ob sie ihre Azure Verwendungsdaten Buckets stündlichen oder täglichen Buckets. Der Standardwert ist täglich.

- **Instanzmetadaten bereitgestellt (einschließlich Resource-Tags)** – auf Instanzebene Details wie der vollständig qualifizierte Ressourcen-Uri (/subscriptions/ {Abonnement-Id} /..), sowie die Ressource Gruppe Informations- und Ressourcen-Tags in der Antwort angegeben werden. Bei der deterministisch wird und programmgesteuert reservieren Nutzung durch die Tags Anwendungsfälle wie übergreifende laden.

- **Ressource bereitgestellten Metadaten** - Ressourcendetails wie Meter Namen Meter Kategorie, Unterkategorie Meter, Einheit und Region werden ebenfalls in der Antwort zu Aufrufer besser verstehen, was verbraucht wurde übergeben. Wir arbeiten außerdem Ressource Metadaten Terminologie in Azure-Portal auszurichten Azure Verwendung CSV EA Abrechnung CSV und andere öffentliche, damit Kunden Daten über Erfahrungen zu korrelieren.

- **Verwendung für alle Angebot** – Daten stehen für alle u.a. nutzungsbasierte, MSDN finanziellen Zusage, Guthaben und EA anbieten.

### <a name="azure-resource-ratecard-api-preview"></a>Azure Ressource RateCard API (Vorschau)
Kunden und Partner können Azure Resource RateCard-API zu der Liste der verfügbaren Azure Ressourcen mit Preisinformationen für jeden geschätzt. Merkmale:

- **Azure Role-based Access Control** - Kunden und Partner können ihre Richtlinien in [Azure-Portal](https://portal.azure.com) oder über [Azure PowerShell-Cmdlets](powershell-install-configure.md) an, welche Benutzer oder Anwendung RateCard-Daten zugreifen können. Aufrufer müssen standard Azure Active Directory Token zur Authentifizierung verwenden. Der Aufrufer muss auch die Leser, Besitzer oder Mitwirkender Rolle Zugriff auf Daten für einen bestimmten Azure-Abonnement hinzugefügt werden.

- **Unterstützung für nutzungsbasierte, MSDN und finanziellen Zusage Guthaben bietet (EA nicht unterstützt)** - diese API bietet Azure Angebot auf Informationen, und Abonnement-Level.  Informationen zu Ressourcendetails Raten muss Aufrufer dieser API übergeben.  Wie EA bietet pro Registrierung angepasst haben, können wir die EA zurzeit bieten.

## <a name="scenarios"></a>Szenarien

Hier sind einige Szenarien, die aus der Verwendung und die RateCard-APIs möglich sind:

- **Azure verbringen im Monat** - können Kunden die Nutzung und RateCard APIs zusammen bessere Einblicke in die Cloud zu verbringen im Monat stündlichen und täglichen Buckets Auslastung und Belastung Vorkalkulationen analysieren.

- **Einrichten von Alerts** -Kunden und Partner können Ressourcen oder monetären-basierten Alarmen auf ihre Cloud einrichten den vorkalkulierten Verbrauch und die Verwendung mit der RateCard-API Kostenvoranschlag.

- **Predict Rechnung** – Kunden und Partner erhalten ihre vorkalkulierten Verbrauch und Cloud und Algorithmen für maschinelles lernen Vorhersagen am Ende des Abrechnungszeitraums wäre ihre Rechnung anwenden.

- **Vor Verbrauchskosten Analysis** -Kunden können auch mit der RateCard-API, wie viel ihre Rechnung vorherzusagen sie wäre zu Arbeitslasten in Azure, durch die Bedarf Zahlen zur Verwendung. Wenn Kunden vorhandene Arbeitslasten in anderen Wolken oder private Clouds verfügen, können sie auch ihre Verwendung mit der Azure zuordnen zu eine bessere Schätzung ihrer geschätzten Azure verbringen. Dies bietet eine erweiterte Ansicht was über [Azure Preise Rechner](https://azure.microsoft.com/pricing/calculator/)erhalten wie (beispielsweise) Partner Abrechnung können auf Drehen und vergleichen/Kontrast zwischen verschiedenen Angebot über nutzungsbasierte monetären Engagement sowie Guthaben. APIs ermöglichen auch das Kosten Vorkalkulation Änderungen nach Region, Aktivierung von was-wenn Analyse zu Bereitstellung, Bereitstellung von Ressourcen in verschiedenen DCs weltweit unmittelbar auf haben können.

- **Was-wenn-Analyse** -

    - Kunden und Partner können ob kostengünstiger Arbeitslasten in einer anderen Region oder in einer anderen Konfiguration von Azure Ressource ausgeführt werden würde. Azure Ressource Kosten unterscheiden anhand der Azure-Region, die ausgeführt werden, und dies ermöglicht Kunden und Partnern Kosten Optimierungen.

    - Kunden und Partner können auch bestimmen, ob ein anderes Azure Angebot besser Azure Ressource gibt.

## <a name="partner-solutions"></a>Partner solutions

[Microsoft Azure und RateCard APIs ermöglichen Cloudyn, ITFM für Kunden bieten](billing-usage-rate-card-partner-solution-cloudyn.md) beschreibt Azure Abrechnung API Partners [Cloudyn](https://www.cloudyn.com/microsoft-azure/)integrationserfahrung.  Dieser Artikel enthält detaillierte Erfahrungen, einschließlich eines kurzen Videos zeigt wie Azure Kunden Cloudyn und Azure Abrechnung APIs Gewinne Erkenntnisse aus ihren Azure Daten verwenden kann.

[Kreuzer Cloud und Microsoft Azure Abrechnung API-Integration](billing-usage-rate-card-partner-solution-cloudcruiser.md) beschreibt die Funktionsweise von [Cloud Kreuzer Express Azure Pack](http://www.cloudcruiser.com/partners/microsoft/) direkt über das Portal WAP Kunden nahtlos die operativen und finanziellen Aspekte der private oder gehostete öffentliche Cloud ihre Microsoft Azure über eine einzige Benutzeroberfläche verwalten.   

## <a name="next-steps"></a>Nächste Schritte
+ [Azure Abrechnung REST-API-Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) für Weitere Informationen zu beiden APIs sind Teil der APIs von Azure-Managers anzeigen
+ Checken Sie rechts in der Beispielcode befassen möchten, unserer Microsoft Azure Abrechnung API Codebeispiele für [Azure Codebeispiele](https://azure.microsoft.com/documentation/samples/?term=billing).

## <a name="learn-more"></a>Weitere Informationen
+ Anzeigen Sie [Azure Ressourcenmanager](azure-resource-manager/resource-group-overview.md) Übersichtsartikel zu Azure-Ressourcen-Manager weitere
+ Weitere Informationen zu den Tools zu Verständnis der Cloud verbringen, finden Sie im Artikel Gartner [Marktübersicht IT Financial Management (ITFM) Tools](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)erforderlich.
