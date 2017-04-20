<properties
   pageTitle="Technische erforderlichen Komponenten zum Erstellen eines Datendiensts für den Markt | Microsoft Azure"
   description="Verstehen der Vorschriften für die Erstellung eines Datendiensts bereitstellen und Azure Marketplace verkaufen"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Technische erforderlichen Komponenten zum Erstellen eines Datendiensts anbieten für Azure Marketplace

>[AZURE.IMPORTANT] **Zu diesem Zeitpunkt sind wir nicht mehr Onboarding alle neuen Datendienst Verleger. Neue Dataservices wird nicht für Angebot genehmigt erhalten.** Haben Sie eine SaaS Business Anwendung Elemente verwenden veröffentlichen möchten Sie weitere Informationen [hier](https://appsource.microsoft.com/partners). Wenn ein IaaS-Applikationen oder Developer Service Azure Marketplace veröffentlichen möchten weitere Informationen [hier](https://azure.microsoft.com/marketplace/programs/certified/).

Lesen Sie den Prozess gründlich vor und verstehen Sie, wo und warum jeder Schritt ausgeführt wird. So weit wie möglich Sie sollten Ihre Unternehmensdaten und andere Daten vorbereiten, erforderlichen Tools herunterladen und technische Komponenten vor dem Angebot erstellen erstellen.

Sie müssen Folgendes vor dem bereit:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Welche Technologie verwendet wird, um Ihr Dienstangebot Daten veröffentlichen entscheiden

Verleger können zwischen mehreren beim Datendienst Azure Marketplace veröffentlichen. Die wichtigsten Technologien, die unterstützt werden beschrieben. Unabhängig davon verbraucht Technologie an den Datendienst verwendet wird, der Endbenutzer die Daten über den **OData-feed** von Azure Marketplace-Dienst verfügbar gemacht werden. Vollständige Informationen zu OData-Dienst finden Sie auf [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure-Datenbank

Dataset im SQL Azure ist Publisher Verantwortung. Sie müssen Azure, geeignete Größe der Datenbank bereitzustellen und Daten in SQL Azure hochladen. Publisher übernimmt auch seine Daten stets aktuell zu halten. Weitere Informationen zum Abonnieren von Azure Services finden Sie auf [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Beim Verschieben von Daten in SQL Azure können Azure Marketplace Tabellen und Ansichten verfügbar machen. Verleger kann angeben, welche Tabellen/Ansichten und Spalten für den Endbenutzer verfügbar gemacht werden. Inhaltsanbieter kann weitere auch angeben, welche Spalten vom Endbenutzer abgefragt werden können und welche nur in der Nutzlast zurückgegeben werden. Dies bietet ein hohes Maß an Flexibilität zu den Daten in der Datenbank verfügbar gemacht werden sollen. Spalten, die abgefragt werden können, müssen durch eine oder mehrere Datenbank-Indizes.

## <a name="rest-based-web-service"></a>REST-basierten Webdienst

Unterstützte Protokoll: **HTTPS nur**

Vorhandene REST-basierte Dienste können über Azure Marketplace bereitgestellt werden. Dataset immer der Endbenutzer als einen OData-Feed ausgesetzt, basierte Azure Marketplace-Dienst muss den Dienst ein OData zuordnen können Service. Also der REST Endpunkte müssen alle Parameter als HTTP-Parameter setzen.

Die Nutzlast muss in einem Format vorliegen, das einen ATOM-Antwort zugeordnet werden können. Daher enthalten die Antwort der Dienste muss im XML-Format und kann nur ein wiederholtes Element mit den Nutzdaten Werten (z. B. Datensatz). Azure Marketplace-Dienst wird den sich wiederholenden Knoten Eintrag Knoten ATOM und die Nutzlast Eigenschaftsknoten im Eintragsknoten zuordnen.

Autorisierungsinformationen (API-Schlüssel Authentication token usw.) muss als HTTP-Parameter oder im HTTP-Header (Schlüssel-Wert-Paar) – auch unverschlüsselte Authentifizierung unterstützt wird. Muss ein gültiger Schlüssel bereitgestellt werden, und alle Anfragen über Azure Marketplace werden über diesen Schlüssel erfolgen. Benutzer überwachen und Abrechnung erfolgt Ebene Azure Marketplace.

Fehler, die vom Dienst zurückgegebenen HTTP-Statuscodes zugeordnet werden müssen. Bei dem Dienst XML, das den Fehler enthält, werden diese von der Azure Marketplace-Dienst HTTP-Statuscodes zugeordnet werden zurückgegeben.

## <a name="soap-based-web-services"></a>SOAP-basierte Webdienste

Protokoll: **HTTPS nur**

Die Vorschriften sind dasselbe wie Bereich. Der einzige Unterschied ist, dass Parameter auch in eine XML-Textteil bereitgestellt werden können, der auf dem Verleger Service mit jeder Anforderung über Azure Marketplace gebucht wird. Dies bedeutet, dass der Benutzer am Frontend mit HTTP-Parameter in XML-Elemente eines XML-Dokuments übersetzt, der mit der Anforderung an den Inhaltsanbieter Webdienst gebucht wird.

## <a name="odata-based-web-services"></a>Basis-OData-Webdiensten

Protokoll: **HTTPS nur**

Daten können als Azure Marketplace OData-Dienst verfügbar gemacht werden. Das System wird den Dienst durch und ersetzt den Stamm des Diensts mit dem Stamm Azure Marketplace-Dienst – um sicherzustellen, dass alle nachfolgenden Aufrufe Azure Marketplace durchlaufen.

OData-Dienste müssen nicht nur gegen eine Datenbank im Backend. OData unterstützt jede Art von Speicher oder die Geschäftslogik des Dienstes fahren.


## <a name="next-steps"></a>Nächste Schritte
Überprüft die erforderlichen Komponenten und der erforderlichen Aufgaben können Sie vorwärts verschieben Ihr Dienstangebot Daten wie im [Publishing Servicehandbuch Daten](marketplace-publishing-data-service-creation.md)erstellen.

Oder möchten Sie den gesamten Vorgang und die jeweiligen Artikel veröffentlichen Phasen finden, lesen Sie den Artikel [Erste Schritte: Angebot Azure Marketplace veröffentlichen](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
