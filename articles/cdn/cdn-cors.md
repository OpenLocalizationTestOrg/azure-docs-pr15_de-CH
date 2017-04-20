<properties
    pageTitle="Azure CDN mit CORS | Microsoft Azure"
    description="Erfahren Sie, wie die Azure Content Delivery Network (CDN), mit Cross-Ursprung Ressource freigeben (CORS) verwenden."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="casoper"/>
    
# <a name="using-azure-cdn-with-cors"></a>Verwenden von Azure CDN mit CORS     

## <a name="what-is-cors"></a>Was ist CORS?

CORS (Cross Ursprung Resource Sharing) ist ein HTTP-Feature, eine Webanwendung unter einer Domäne auf Ressourcen in einer anderen Domäne zugreifen können. Um Cross-Site scripting-Angriffe zu vermeiden, setzen alle modernen Webbrowser eine Einschränkung für die Sicherheit als [same Origin Policy](http://www.w3.org/Security/wiki/Same_Origin_Policy).  Dadurch wird einer Webseite Aufrufen von APIs in einer anderen Domäne.  CORS bietet eine sichere Möglichkeit, eine Domäne (Ursprungsdomäne) in einer anderen Domäne APIs aufrufen.
 
## <a name="how-it-works"></a>Funktionsweise
1.  Der Browser sendet eine Anforderung Optionen mit einem **Ursprungs** -HTTP-Header. Der Wert dieses Headers ist die Domäne, die der übergeordneten Seite. Eine Seite https://www.contoso.com versucht ein Benutzer Daten in der Domäne fabrikam.com zugreifen, würde der folgenden Anforderungsheader an fabrikam.com gesendet: 
    
    `Origin: https://www.contoso.com`
 
2.  Der Server reagiert durch Folgendes:
    - Eine **Access Control können Origin** Header in der Antwort die Ursprungs-Websites dürfen. Zum Beispiel:
        
        `Access-Control-Allow-Origin: https://www.contoso.com`
        
    - Eine Fehlerseite lässt der Server nicht die ursprungsübergreifende Anforderung
    - Eine **Access Control können Origin** Header mit Platzhalterzeichen, die alle Domänen ermöglicht:
        
        `Access-Control-Allow-Origin: *`
 
Komplexe HTTP-Anfragen ist eine "preflight" Anforderung zuerst fertig zu bestimmen, ob vor dem gesamte Anforderung gesendet hat.
 
## <a name="wildcard-or-single-origin-scenarios"></a>Platzhalter oder einzelnen Ursprung Szenarien

CORS in Azure CDN funktioniert ohne zusätzliche Konfiguration automatisch, wenn **Access Control können Origin** -Header Platzhalter (*) oder einzelne Ursprung festgelegt ist.  Das CDN Zwischenspeichern der ersten Antwort und nachfolgende Anfragen verwenden den gleichen Header.
 
Wenn Anfragen bereits CDN vor CORS auf festgelegt wurde die Ihrem Ursprung müssen Inhalte auf den Endpunkt Inhalt laden den Inhalt mit **Access Control können Origin** -Header zu löschen.
 
## <a name="multiple-origin-scenarios"></a>Szenarien mit mehreren Ursprung

Wenn Sie eine bestimmte Liste Herkunft CORS dürfen müssen, wird es etwas komplizierter. Das Problem tritt beim CDN Header **Access Control können Ursprung** für den ersten Ursprung CORS speichert.  Bei anderen CORS Ursprung nachfolgende anfordert, wird CDN war den zwischengespeicherten **Access Control können Origin** -Header übereinstimmen.  Es gibt verschiedene Methoden zur Behebung.
 
### <a name="azure-cdn-premium-from-verizon"></a>Azure CDN Premium von Verizon

Die beste Möglichkeit dazu ist **Azure CDN Premium von Verizon**, verwenden einige erweiterte Funktionen verfügbar macht. 
 
Erstellen [eine Regel](cdn-rules-engine.md) müssen **Origin** -Header in der Anforderung überprüft.  Ist eine gültige Ursprung wird der **Access Control können Origin** -Header mit Ursprung in der Anforderung bereitgestellte Regelsatz.  Wenn im **Ursprungs** -Header angegebene Ursprung nicht zulässig ist, sollte die Regel **Access Control können Origin** -Header lassen Browser ablehnen führen. 
 
Es gibt zwei Arten mit dem Regelmodul.  In beiden Fällen **Access Control können Origin** Header vom Ursprungsserver die Datei ignoriert, das CDN Regelmodul vollständig verwaltet die zulässigen CORS Ursprünge.

#### <a name="one-regular-expression-with-all-valid-origins"></a>Ein regulärer Ausdruck mit allen gültigen
 
In diesem Fall erstellen Sie einen regulären Ausdruck, der alle die Ursprünge enthält zu ermöglichen: 

    https?:\/\/(www\.contoso\.com|contoso\.com|www\.microsoft\.com|microsoft.com\.com)$
 
> [AZURE.TIP] **Azure CDN von Verizon** verwendet [Perl kompatible reguläre Ausdrücke](http://pcre.org/) als Modul für reguläre Ausdrücke.  Ein Tool wie [reguläre Ausdrücke 101](https://regex101.com/) können Sie einen regulären Ausdruck überprüfen.  Beachten Sie, dass das Zeichen "/" ist in regulären Ausdrücken zulässig und muss mit Escapezeichen versehen werden, das Zeichen wird empfohlen und einige Regex Validierungssteuerelemente erwartet.

Wenn der reguläre Ausdruck entspricht ersetzt Regel **Access Control können Origin** -Header (sofern vorhanden) vom Ursprung mit Ursprung, der die Anforderung gesendet.  Sie können auch zusätzliche CORS-Header wie **Access Control können Methoden**hinzufügen.

![Beispiel für Regeln mit RA](./media/cdn-cors/cdn-cors-regex.png)
 
#### <a name="request-header-rule-for-each-origin"></a>Header-Regel für jedes Ursprungsland anfordern.

Als reguläre Ausdrücke können stattdessen erstellen eine gesonderte Regel für jeden Ursprungs mit [Bedingung erfüllen](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1) **Request Header Platzhalter** zulassen möchten. Mit RA-Methode wird das Regelmodul allein CORS-Header. 
  
![Beispiel ohne Ausdruck Regeln](./media/cdn-cors/cdn-cors-no-regex.png)

> [AZURE.TIP] Im Beispiel oben verwenden das Platzhalterzeichen * weist das Regelmodul mit HTTP und HTTPS.
 
### <a name="azure-cdn-standard"></a>Azure CDN Standard

Azure CDN Standard Profile ist der einzige Mechanismus ermöglicht mehrere Ursprünge ohne Platzhalterzeichen Ursprung [Abfrage Zeichenfolge caching](cdn-query-string.md)verwenden.  Sie müssen Query String Einstellung für den CDN-Endpunkt aktivieren und dann eine eindeutige Zeichenfolge für Anfragen jeder zugelassene Domäne. Dies führt ein separates Objekt für jede eindeutige Zeichenfolge Zwischenspeichern CDN. Dieser Ansatz ist jedoch nicht ideal, da sie mehrere Kopien derselben Datei auf das CDN zwischengespeichert wird.  

