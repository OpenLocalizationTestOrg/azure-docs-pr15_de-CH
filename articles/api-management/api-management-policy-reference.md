<properties 
    pageTitle="Azure API Management Policy Reference" 
    description="Lernen Sie die Richtlinien zur API Management konfigurieren." 
    services="api-management" 
    documentationCenter="" 
    authors="vladvino" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="apimpm"/>

# <a name="azure-api-management-policy-reference"></a>Azure API Management Policy Reference

Dieser Abschnitt stellt einen Index für die Richtlinien [API Management Policy Reference][]. Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management][].

Richtlinienausdrücke können als Attribute oder Textwerte in API-Verwaltungsrichtlinien verwendet werden, die Richtlinie nicht anders angegeben. Einige Richtlinien wie die [Kontrollfluss][] und [Variable][] basieren auf Richtlinienausdrücken. Weitere Informationen finden Sie unter [Erweiterte Richtlinien][] und [Gruppenrichtlinien Ausdrücke][]

## <a name="policy-reference-index"></a>Richtlinie bezugsindex

-   [Access-Richtlinien][]
    -   [Überprüfen Sie HTTP-Header][] - erzwingt die Existenz bzw. des HTTP-Headers.
    -   [Aufruflimit Abonnement][] - API verhindert die Verwendung spikes Frequenz, auf pro beschränken.
    -   [Aufruflimit Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) - API verhindert die Verwendung spikes Frequenz, jeweils pro Schlüssel beschränken.
    -   [Schränken Aufrufer IPs][] - Filter verweigert (können /) Aufrufe von bestimmten IP-Adressen oder Adressbereiche.
    -   [Verwendung von Abonnement gesetzt][] - ermöglicht erneuert oder Lebensdauer Aufruf und/oder Bandbreite Kontingent auf pro erzwingen.
    -   [Verwendung von Schlüssel gesetzt](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) - ermöglicht erneuert oder Lebensdauer Aufruf und/oder Bandbreite Kontingent jeweils pro Schlüssel zu erzwingen.
    -   [Überprüfen JWT][] - erzwingt Bestehens und der Gültigkeit einer JWT oder eine angegebene HTTP-Header angegebenen Abfrageparameter extrahiert.
-   [Erweiterte Richtlinien][]
    -   [Kontrollfluss][] - wendet bedingt Richtlinien basierend auf den Ergebnissen der Bewertung von booleschen [Ausdrücken][].
    -   [Anfragen weiterleiten][] – leitet die Anforderung an den Back-End-Dienst.
    -   [Protokoll Event Hub][] - sendet Nachrichten im angegebenen Format auf eine Nachricht von einer [Protokollierung](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) definiert.
    -   [Wiederholen](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) - Wiederholungsversuche Ausführung von richtlinienanweisungen eingeschlossene Wenn und die Bedingung erfüllt ist. Ausführung in festgelegten Zeitabständen wiederholt und bis die angegebene Anzahl der Wiederholungsversuche.
    -   [Antwort zurückgeben](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) - Ausführung abgebrochen und gibt die angegebene Antwort direkt an den Aufrufer.
    -   [Unidirektionale Anforderung senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) - sendet eine Anforderung an die angegebene URL ohne eine Antwort zu warten.
    -   [Anfrage senden](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) – sendet eine Anforderung zur angegebenen URL.
    -   [Anforderungsmethode Set](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) - können Sie die HTTP-Methode für eine Anforderung ändern.
    -   [Festlegen der Status](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) - ändert den HTTP-Statuscode auf den angegebenen Wert.
    -   [Legen Sie Variable][] - Wert in einer Variablen benannten [Kontext][] für den späteren Zugriff speichern.
    -   [Trace](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) - Fügt eine Zeichenfolge in der [API-Inspektor](../api-management/api-management-howto-api-inspector.md) -Ausgabe.
    -   [Warten](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) - wartet geschlossene Anfrage senden, Abrufen des Wertes aus Cache oder Fluss Richtlinien abschließen, bevor Sie fortfahren.
-   [Authentifizierungsrichtlinien][]
    -   [Authentifizieren mit Basic][] - Authentifizierung mit einem Back-End-Dienst mithilfe der Standardauthentifizierung.
    -   [Authentifizieren mit Clientzertifikat][] - Authentifizierung mit einem Back-End-Dienst mit Clientzertifikaten.
-   [Zwischenspeichern von Richtlinien][] 
    -   [Aus Cache abrufen][] – ausführen Cache nachzuschlagen und eine gültige zwischengespeicherte Antwort beim verfügbaren zurückgibt.
    -   [Speicher Cache][] - Caches Antwort nach dem angegebenen Steuerelement Cachekonfiguration.
    -   [Wert aus dem Cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) Abrufen eines zwischengespeicherten Elements von Schlüssel.
    -   [Wert speichern im Cache](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) - Speicher ein Element im Cache durch Schlüssel.
    -   [Wert aus Cache entfernen](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) - ein Element im Cache durch Schlüssel entfernen.
-   [Cross-Domain-Richtlinien][] 
    -   [Domänenübergreifende Aufrufe zulassen][] – macht die API von Adobe Flash und Microsoft Silverlight-Browser-basierte Clients zugänglich.
    -   [CORS][] - fügt Cross-Ursprung Ressourcenfreigabe (CORS) unterstützt eine Operation oder eine API, um domänenübergreifende Aufrufe von Browser-basierten Clients zu ermöglichen.
    -   [JSONP][] - fügt JSON padding (JSONP) Unterstützung für einen Vorgang oder eine API domänenübergreifende Aufrufe aus JavaScript Browser-basierte Clients.
-   [Transformation Richtlinien][] 
    -   [Konvertieren von JSON und XML][] - konvertiert Anforderung oder Antwort Körper von JSON in XML.
    -   [Konvertiert XML, JSON][] - konvertiert Anforderung oder Antwort body aus XML, JSON.
    -   [Suchen und ersetzen die Zeichenfolge in Text][] - sucht nach einer Anforderung oder Antwort Teilzeichenfolge und durch eine andere Teilzeichenfolge ersetzt.
    -   [Maske URLs im Inhalt][] - Links in die Antwort schreibt (Masken) Körper, damit sie auf die entsprechende Verknüpfung über das Gateway zeigen.
    -   [Back-End-Dienst][] - ändert Back-End-Dienst für eine eingehende Anforderung.
    -   [Körper][] - legt den Nachrichtentext für ein- und ausgehenden Anfragen.
    -   [Festlegen von HTTP-Header][] - eine vorhandene Antwort bzw. Anforderungsheader ein Wert zugewiesen oder fügt einen neuen Antwort oder Anfrage-Header.
    -   [Legen Sie Abfragezeichenfolgen-Parameter][] - fügt hinzu, ersetzt der Wert oder löscht Abfragezeichenfolgenparameter anfordern.
    -   [Schreiben Sie URL][] - konvertiert Anfrage-URL aus öffentlichen Formular Formular vom Webdienst erwartet.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Gruppenrichtlinien Ausdrücken finden Sie im folgende Video.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

[Access-Richtlinien]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[HTTP-Header überprüfen]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Aufruflimit Abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Beschränken Sie die Aufrufer IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set Verwendung Kontingent Abonnement]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[JWT überprüfen]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Erweiterte Richtlinien]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Kontrollfluss]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Variable festlegen]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Ausdrücke]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[Kontext]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Anfragen weiterleiten]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Protokoll Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentifizierungsrichtlinien]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Mit Authentifizierung]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authentifizieren mit Client-Zertifikat]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Zwischenspeichern von Richtlinien]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Aus Cache abrufen]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Cache speichern]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross-Domain-Richtlinien]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Domänenübergreifende Aufrufe]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation Richtlinien]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[JSON in XML konvertieren]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[XML, JSON konvertieren]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Suchen Sie und Ersetzen Sie die Zeichenfolge in Text]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Maske URLs im Inhalt]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set Back-End-Dienst]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Text festlegen]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set-HTTP-header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Abfrageparameter festlegen]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Umschreiben URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Richtlinien-API-Management]: api-management-howto-policies.md
[API Management Policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Richtlinienausdrücke]: https://msdn.microsoft.com/library/azure/dn910913.aspx

 
