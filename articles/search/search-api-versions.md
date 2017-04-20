<properties
   pageTitle="API-Versionen von Azure suchen | Microsoft Azure | Such-API"
   description="Versionsrichtlinien für Azure Suche REST-APIs und die Clientbibliothek im .NET SDK."
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/16/2016"
   ms.author="brjohnst"/>

# <a name="api-versions-in-azure-search"></a>Versionen in Azure Search-API

Azure Suche rollt Feature Updates regelmäßig. Manchmal aber nicht immer, müssen diese Updates eine neue Version unserer API veröffentlichen, um die Abwärtskompatibilität sicherzustellen. Veröffentlichen einer neuen Version können Sie steuern, wann und wie Service Suchbegriffe im Code integrieren.

In der Regel versuchen wir bei Bedarf neue Versionen veröffentlicht werden, da dabei einige Aufwand Ihren Code, um eine neue API-Version aktualisieren. Wir veröffentlichen nur eine neue Version, benötigen wir einige API in einer Weise ändern, die Abwärtskompatibilität verletzen. Dies kann durch Updates zu vorhandenen Features oder aufgrund neuer Funktionen, die vorhandene API-Oberfläche ändern.

Wir führen Sie dieselbe Regel für SDK-Updates. Das Azure SDK folgt [semantische](http://semver.org/) Versionsregeln, so dass seine Version drei Teile: Haupt-, neben- und build-Nummer (z. B. 1.1.0). Wir geben eine neue Version des SDK nur bei Änderungen, die Abwärtskompatibilität zu unterbrechen. Feste Funktionsupdates erhöhen wir die Nebenversion und zur Behebung von Problemen wir nur erhöhen die Build-Version.

##<a name="snapshot-of-current-versions"></a>Snapshot der aktuellen Versionen 

Unten ist eine Momentaufnahme der aktuellen Versionen aller Schnittstellen Azure Suche programmieren. Roadmaps und andere Details in den nachfolgenden Abschnitten dieses Dokuments finden Sie.

Schnittstellen|Aktuellste Hauptversion|Status
----------|-------------------------|------
[.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)|1.1|Veröffentlichung verfügbar, Februar 2016
[.NET SDK-Vorschau](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx)|2.0-Vorschau|Veröffentlicht August 2016 Vorschau
[Service-REST-API](https://msdn.microsoft.com/library/azure/dn798935.aspx)|2015-02-28|Allgemein verfügbar
[Service REST API-Vorschau](search-api-2015-02-28-preview.md)|2015-02-28-Vorschau|Vorschau
[Management REST-API](https://msdn.microsoft.com/library/azure/dn832684.aspx)|2015-08-19|Allgemein verfügbar

REST-APIs, einschließlich der `api-version` jeder ist erforderlich. Dies erleichtert eine bestimmte Version, wie eine Vorschau API. Im folgenden Beispiel wird veranschaulicht, wie die `api-version` Parameter angegeben:

    GET https://adventure-works.search.windows.net/indexes/bikes?api-version=2015-02-28

> [AZURE.NOTE] Jede Anforderung hat eine `api-version`, wir empfehlen die Verwendung derselben Version für alle API-Anfragen. Dies gilt insbesondere, wenn neue API-Versionen einführen, Attribute oder Vorgänge, die von früheren Versionen nicht erkannt werden. API-Versionen mischen können unbeabsichtigte Folgen vermieden werden.
> 
Service-REST-API und Management REST API bilden unabhängig voneinander. Jede Ähnlichkeit in Versionsnummern ist Co Folgeschäden.

Erhältlich (oder GA) APIs in der Produktion verwendet werden und unterliegen Azure Service Level-Agreements. Vorschauversionen sind experimentelle Features nicht immer eine GA-Version migriert werden. **Wir raten dringend Vorschau APIs in Produktion.**

##<a name="sdk-version-roadmap"></a>SDK-Version roadmap

Jede Version von .NET SDK zielt eine bestimmte Version der REST-API. Funktionen sind zunächst in die REST-API eingeführt und anschließend im SDK implementiert.

.NET SDK ist jetzt erhältlich und Arbeit läuft bereits auf die nächste Version. In der folgenden Tabelle blickt auf zukünftige Versionen des SDK, damit Sie wissen was als Nächstes kommt.

.NET SDK-version|REST API-version|Funktionen|ETA
----------------|----------------|--------|---
1.1|2015-02-28|Lucene Abfragesyntax|Februar 2016
2.0-Vorschau|2015-02-28-Vorschau|Benutzerdefinierte Analyzer, Azure Blob und Tabelle Indexer, Field Mappings ETags|August 2016
2.x|Neue API-GA-version|Wie 2.0-Vorschau|Anfang Q4 2016

##<a name="about-preview-and-generally-available-versions"></a>Vorschau und Allgemein verfügbaren Versionen

Azure Suche gibt bereits immer experimentelle Features über die REST-API, dann über Vorabversionen von .NET SDK.

Vorschaufunktionen sind nicht unbedingt migriert werden, GA-Release. GA uneingeschränkt stabil und mit Ausnahme von kleinen abwärtskompatiblen korrigiert und erweitert ändern gelten, sind Vorschaufunktionen für Tests und Versuche mit dem Ziel Sammeln von Feedback für Entwurf und Implementierung. 

Aber da Vorschaufunktionen vorbehalten empfohlen Produktionscode schreiben, der eine Abhängigkeit Vorschauversionen annimmt. Bei Verwendung eine ältere Vorschauversion wird empfohlen, die allgemein verfügbar (GA) Version migrieren. 

Für das .NET SDK: Anleitung zur Codemigration finden Sie unter [.NET SDK aktualisieren](search-dotnet-sdk-migration.md).

Allgemeiner Verfügbarkeit bedeutet, dass Azure Suche jetzt unter die Vereinbarung zum Servicelevel (SLA). Die SLA [Azure Search Service Level Agreements](https://azure.microsoft.com/support/legal/sla/search/v1_0/)finden Sie unter.

