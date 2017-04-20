<properties 
    pageTitle="API-Schlüsselkonzepte" 
    description="Erfahren Sie mehr über APIs, Produkte, Rollen, Gruppen und andere API Schlüsselkonzepte." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Was ist API-Management?

API Management hilft Organisationen APIs für externe Partner und interne Entwickler um das Potenzial ihrer Daten und Dienste zu veröffentlichen. Unternehmen überall suchen erweitern ihre Vorgänge als digitale Plattform, neue Kanäle erstellen, neue Kunden und stärkeres Engagement mit bestehenden fahren. API bietet die Kernkompetenzen erfolgreiches API-Programm durch Entwickler Engagement Einblicke, Analytik, Sicherheit und Schutz sicherzustellen.

Im folgenden Video Überblick Azure API Management und lernen API Management Ihrer API einschließlich Zugriffskontrolle, Bandbreitenkontrolle, Überwachung, Protokollierung und Antwort Zwischenspeichern mit minimalem Aufwand ihrerseits viele Features hinzugefügt.

> [AZURE.VIDEO azure-api-management-overview]

API Management erstellen Administratoren APIs. Jede API besteht aus ein oder mehrere Vorgänge und jede API kann ein oder mehrere Produkte hinzugefügt werden. Um eine API verwenden Entwickler abonnieren Sie ein Produkt mit dieser API können sie aufrufen, die API Vorgang unterliegen alle Richtlinien, die sich möglicherweise

Dieses Thema enthält einen Überblick über die API Schlüsselkonzepte.

>[AZURE.NOTE] Weitere Informationen finden Sie unter der [Cloud-basierte API Management: Nutzung macht APIs](http://j.mp/ms-apim-whitepaper) PDF-Whitepaper. Dieses Whitepaper zur Einführung von CITO Research auf API umfasst: 
>
> - API-Anforderungen und Aufgaben
>     - Entkopplung APIs und Fassaden präsentieren
>     - Entwickler immer schnell einsatzbereit
>     - Sichern des Zugriffs
>     - Analyse und Metriken
> - Kontrolle und mit einer API-Management-Plattform
> - Vs lokale Cloudlösungen verwenden
> - Azure API Management

## <a name="apis"> </a>-APIs und Operationen

APIs bilden die Grundlage einer API Management Service-Instanz. Jede API stellt eine Reihe von Vorgängen Entwickler. Jede API enthält eine Back-End-Dienst, der die API implementiert und seine Vorgänge zuordnen zu Back-End-Dienst durchgeführten Operationen. API-Management sind extrem konfigurierbar steuern URL Zuordnung, Abfrage und Parameter Anforderung und Antwortinhalt und Vorgang Antwort zwischenspeichern. Grenze, Quoten und IP-Richtlinien können auch API oder einzelne Ebene implementiert werden.

Weitere Informationen finden Sie unter [APIs erstellen][] und [Hinzufügen zu einer API][].


## <a name="products"></a> Produkte

Produkte wie APIs für Entwickler verfügbar gemacht werden. API Management Produkte enthalten eine oder mehrere APIs und mit einem Titel, Beschreibung und Verwendung konfiguriert werden. Produkte können **geöffnet** oder **geschützt**werden. Geschützte Produkte müssen abonniert werden, bevor sie beim Öffnen verwendet werden können, Produkte können ohne Abonnement verwendet. Wenn ein Produkt bereit für die Verwendung durch Entwickler ist es veröffentlicht werden kann. Nachdem es veröffentlicht wurde, kann werden angezeigt (und geschützten Erzeugnissen abonniert) von Entwicklern. Abonnement Genehmigung kann auf der Produktebene konfiguriert und Administrator genehmigt oder automatisch genehmigt werden.

Gruppen werden zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Produkte gewähren Sichtbarkeit für Gruppen und Entwickler anzeigen und abonnieren, die Produkte, die sie gehören zu den Gruppen angezeigt werden. 

Weitere Informationen finden Sie unter [Erstellen und Veröffentlichen eines Produkts][] und das folgende Video.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Gruppen

Gruppen werden zum Verwalten der Sichtbarkeit von Produkten für Entwickler. Management-API hat die folgenden unveränderlich Systemgruppen.

-   **Administratoren** - Azure-Abonnement-Administratoren sind Mitglieder dieser Gruppe. Administratoren verwalten API Management Dienstinstanzen erstellen APIs, Operationen und Produkte, die von Entwicklern verwendet werden.
-   **Entwickler** - authentifizierte Entwicklerportal Benutzer in diese Gruppe fallen. Entwickler sind Kunden, die Anwendung über APIs. Entwickler erhalten Zugang zum Entwicklerportal und Anwendungsentwicklung, die die Vorgänge eine API aufrufen.
-   **Gäste** - Portalbenutzern nicht authentifizierte Entwickler wie Interessenten besuchen Entwicklerportal Instanz einer API Management zu dieser Gruppe gehören. Sie können erteilt werden bestimmte schreibgeschützten Zugriff wie APIs anzeigen, aber nicht aufgerufen.

Neben diesen Systemgruppen können Administratoren benutzerdefinierte Gruppen oder [Nutzung von externen Gruppen zugeordneten Azure Active Directory Mieter](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group)erstellen. Benutzerdefinierte und externen Gruppen können neben Systemgruppen geben Entwickler Sichtbarkeit und Zugriff auf API-Produkte verwendet werden. Beispielsweise konnte Sie erstellen eine benutzerdefinierte Gruppe für Entwickler einer bestimmten Organisation zugeordnet und sie Zugriff auf die APIs aus einem Produkt mit entsprechenden APIs. Ein Benutzer kann Mitglied mehrerer Gruppen sein.

Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen][].

## <a name="developers"></a> Entwickler

Entwickler sind die Benutzerkonten in einer API Management Service-Instanz. Entwickler können erstellt oder eingeladen von Administratoren, oder sie können [Entwicklerportal][]anmelden. Jeder Entwickler ist Mitglied einer oder mehrerer Gruppen und Produkte abonnieren, die Sichtbarkeit zu diesen Gruppen erteilen kann.

Wenn Entwickler ein Produkt abonnieren erhalten sie den primären und den sekundären Schlüssel für das Produkt. Dieser Schlüssel wird verwendet, wenn in der Produkt APIs aufrufen.

Weitere Informationen finden Sie unter [Erstellen oder laden Entwickler][] und [Entwickler Gruppen zuordnen][].

## <a name="policies"></a> Richtlinien

Richtlinien sind eine leistungsfähige Funktion API-Management, mit denen Publisher das Verhalten der API durch Konfiguration ändern können. Richtlinien sind eine Sammlung von Anweisungen, die auf die Anforderung sequenziell ausgeführt oder eine API auf. Beliebte enthalten Konvertierung von XML, JSON und Aufruf Bandbreitenkontrolle Anrufe von einem Entwickler beschränken und viele andere Richtlinien stehen.

Richtlinienausdrücke können als Attribute oder Textwerte in API-Verwaltungsrichtlinien verwendet werden, die Richtlinie nicht anders angegeben. Einige Richtlinien wie die [Kontrollfluss](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) und [Variable](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) basieren auf Richtlinienausdrücken. Weitere Informationen finden Sie unter [Erweiterte Richtlinien](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies) [Richtlinienausdrücken](https://msdn.microsoft.com/library/azure/dn910913.aspx)und im folgenden Video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

Eine vollständige Liste der API-Richtlinien finden Sie unter [Policy Reference][]. Weitere Informationen zum verwenden und Konfigurieren von Richtlinien finden Sie unter [API-Richtlinien][]. Finden Sie ein Lernprogramm zum Erstellen eines Produkts mit Limit und ein Kontingent von Richtlinien [zum Erstellen und konfigurieren Sie erweiterte Einstellungen][]. Eine Demo finden Sie im folgende Video.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> -Entwicklerportal

Entwicklerportal werden Entwickler Ihre APIs, Ansicht und rufen Vorgänge erfahren können, und Produkte abonnieren. Interessenten können Entwicklerportal besuchen APIs und Operationen anzeigen und registrieren. Die URL für Ihre Entwicklerportal befindet sich im klassischen Azure-Portal für die Dienstinstanz API Management Dashboard.

Das Erscheinungsbild der Entwicklerportal können durch Hinzufügen von benutzerdefiniertem Inhalt, Formatvorlagen anpassen und branding hinzufügen.

## <a name="api-management-and-the-api-economy"></a>API-Management und Wirtschaft API

Weitere Informationen über API Management sehen Sie die folgende Präsentation Microsoft entzünden 2015 Konferenz an

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Entwicklerportal]: #developer-portal

[APIs erstellen]: api-management-howto-create-apis.md
[Wie eine API Operationen hinzugefügt]: api-management-howto-add-operations.md
[Erstellen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[Zum Erstellen und Verwenden von Gruppen]: api-management-howto-create-groups.md
[Zuordnen von Gruppen zu Entwickler]: api-management-howto-create-groups.md#associate-group-developer
[Zum Erstellen und konfigurieren Erweiterte Einstellungen]: api-management-howto-product-with-rules.md
[Erstellen oder laden Entwickler]: api-management-howto-create-or-invite-developers.md
[Richtlinie Bezug]: api-management-policy-reference.md
[API-Richtlinien]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
