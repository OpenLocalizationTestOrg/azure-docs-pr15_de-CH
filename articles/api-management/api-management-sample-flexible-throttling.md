<properties
    pageTitle="Erweiterte Anforderung mit Azure API Management Drosselung"
    description="Informationen Sie zum Erstellen und Anwenden von flexibles Kontingent und Rate eingeschränkt Richtlininen Azure API Management."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Erweiterte Anforderung mit Azure API Management Drosselung

Anfragen zu drosseln ist eine Schlüsselrolle Azure API Management. Entweder durch die Kontrolle der Anfragen oder Anfragen/Daten übertragen, API Management API-Anbieter ihre APIs vor Missbrauch schützen, und für verschiedene API-Produkt Ebenen erstellen können.

## <a name="product-based-throttling"></a>Produkt-Drosselung
Datum der Rate throttling wurden Funktionen nur zu einem bestimmten produktabonnement (im Wesentlichen Schlüssel) beschränkt, in der Publisher-API Verwaltungsportal. Dies ist nützlich für API-Provider, die Entwickler eingeschränkt, die Verwendung ihrer API angemeldet sind, aber es hilft nicht, z. B. in einzelnen Endbenutzer der API-Drosselung. Es ist möglich, die für einzelne Benutzer der Anwendung des Entwicklers die Quote und dann hindern andere Kunden des Entwicklers der Anwendung. Auch können mehrere Benutzer eine große Anzahl von Anfragen generieren können gelegentliche Benutzer begrenzen

## <a name="custom-key-based-throttling"></a>Benutzerdefinierte Schlüsselbasierte Drosselung
Die neue [Rate Limit von Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [Kontingent Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) Lösung wesentlich flexibler Datenverkehr steuern. Diese neuen Richtlinien können Ausdrücke Ermittlung mit Datenverkehr nachzuvollziehen Schlüssel definieren. Diese Arbeitsweise ist am einfachsten Beispiel dargestellt. 

## <a name="ip-address-throttling"></a>Drosselung der IP-Adresse
Die folgenden Richtlinien beschränken eine einzelnen IP-Adresse nur 10 Aufrufe minütlich mit 1.000.000 Anrufe und 10.000 KB Bandbreite pro Monat. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Wenn alle Clients im Internet eine eindeutige IP-Adresse verwendet, möglicherweise eine effektive Möglichkeit zum Beschränken der Verwendung durch Benutzer. Es ist jedoch wahrscheinlich, dass mehrere Benutzer werden eine einzelne öffentliche IP-Adresse Ihnen Zugriff auf das Internet über ein NAT-Gerät. Trotz dieser APIs, die nicht authentifizierten Zugriff zulassen der `IpAddress` möglicherweise die beste Option.

## <a name="user-identity-throttling"></a>Benutzer Identität Drosselung
Wenn ein Benutzer authentifiziert Drosselung Schlüssel basierend auf Informationen, die eindeutig, generiert Benutzer.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

In diesem Beispiel wir den Authorization-Header extrahieren, konvertieren Sie sie in `JWT` Objekts und verwenden den Betreff des Tokens zu Benutzer als Schlüssel eingeschränkt Rate verwenden. Wenn die Identität des Benutzers gespeichert ist die `JWT` wie eine andere Ansprüche, Wert verwendet werden kann.

## <a name="combined-policies"></a>Kombinierte Richtlinien
Zwar neue Drosselung mehr Kontrolle als Drosselung Politiken, ist weiterhin Wert beide Funktionen kombinieren. Produktschlüssel Abonnement ([aufruflimit Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und [Verwendung Kontingent Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) Einschränkung ist hervorragend zum Aktivieren einer API mit basierend auf Nutzung Geld. Feiner detailliert Steuerelements können Benutzer drosseln ergänzt und verhindert, dass ein Benutzer Verhalten beeinträchtigt die Erfahrung der anderen. 

## <a name="client-driven-throttling"></a>Client gesteuert Drosselung
Drosselung Schlüssel definiert einen [Richtlinienausdruck](https://msdn.microsoft.com/library/azure/dn910913.aspx), ist es der API-Provider, die wie die Drosselung Bereich auswählen. Jedoch möglicherweise möchte der Entwickler steuern, wie sie Limit eigene bewerten Kunden. Dies konnte vom Anbieter API aktiviert werden durch die Einführung eines benutzerdefinierten Headers der Entwickler der Clientanwendung Schlüssel an die API zu ermöglichen.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Dadurch kann der Entwickler Clientanwendung wählen sie Satz beschränken Schlüssel erstellen. Mit ein wenig Kreativität konnte Cliententwickler eigenen Ebenen erstellen Benutzer Schlüssel zuordnen und die Schlüsselverwendung

## <a name="summary"></a>Zusammenfassung
Azure API Management bietet Rate und Angebot Drosselung schützen und Mehrwert für Ihre API-Diensts. Drosselung neue mit benutzerdefinierten Bereichsregeln ermöglichen eine feinere detaillierte Kontrolle dieser Richtlinien Ihre Kunden besser Anwendung. Die Beispiele in diesem Artikel werden die Verwendung diese neuen Richtlinien mit Produktion Tasten mit IP-Adressen, Benutzeridentität und Client generierten Werte beschränken. Es gibt jedoch viele Teile der Nachricht Benutzeragenten URL Path Fragmente, Nachrichtengröße verwendet werden kann.

## <a name="next-steps"></a>Nächste Schritte
Bitte geben Sie uns Feedback im Disqus-Thread zu diesem Thema. Es wäre freuen über andere mögliche Werte, die eine logische Wahl der Szenarien.

## <a name="watch-a-video-overview-of-these-policies"></a>Dieser Richtlinien einen video ansehen
Weitere Informationen über die in diesem Artikel behandelten [Rate Limit von Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [Kontingent Schlüssel](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) sehen Sie im folgende Video.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
