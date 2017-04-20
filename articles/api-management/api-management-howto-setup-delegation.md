<properties 
    pageTitle="Registrierung und Produkt Abonnement delegieren" 
    description="Erfahren Sie, wie Benutzer registrieren und Produkt abonnieren Azure API Management dritte delegieren." 
    services="api-management" 
    documentationCenter="" 
    authors="antonba" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="antonba"/>

# <a name="how-to-delegate-user-registration-and-product-subscription"></a>Registrierung und Produkt Abonnement delegieren

Delegierung können Sie Ihre vorhandene Website Developer Sign-in/Anmeldung und Abonnement für Produkte im Gegensatz zu integrierten Funktionen im Entwicklerportal verwenden. Dies ermöglicht Ihrer Website die Benutzerdaten und validieren Schritte Benutzerdefiniert.

## <a name="delegate-signin-up"> </a>Delegieren Developer anmelden und Anmeldung

Entwickler delegieren anmelden und abonnieren Sie Ihre vorhandene Website müssen Sie spezielle Delegierung auf Ihrer Website erstellen, die als Einstiegspunkt für diese API Management-Entwicklerportal initiierte Anforderung dient.

Der endgültige Workflow werden wie folgt:

1. Entwickler klickt auf den Link anmelden oder Anmeldung API Management-Entwicklerportal
2. Browser wird die Delegierung Endpunkt umgeleitet.
3. Delegierungsendpunkt wiederum leitet an oder stellt UI Benutzer anmelden oder Anmeldung
4. Bei Erfolg wird der Benutzer an die API Management Developer Portal Seite umgeleitet von begann


Zunächst fordert uns erste Setup-API Management weiterleiten über Ihre Delegation Endpunkt. Im Publisher-API-Verwaltungsportal auf **Sicherheit** , und klicken Sie auf die Registerkarte **Delegierung** . Klicken Sie auf das Kontrollkästchen "Delegate-in & Anmeldung" aktivieren.

![Delegierung Seite][api-management-delegation-signin-up]

* Entscheiden Sie, was die URL Ihres Endpunkts spezielle Delegation und **Delegierung Endpunkt-URL** -Feld eingeben. 

* Geben Sie im Feld **Delegierung Authentifizierungsschlüssel** einen geheimen Schlüssel mit eine Signatur, die Sie zur Überprüfung, um sicherzustellen, dass die Anforderung tatsächlich aus Azure API Management zur Berechnung. Klicken Sie auf die **generieren** Schaltfläche API Managemnet zufällig einen Schlüssel zu generieren.

Jetzt müssen Sie die **delegierungsendpunkt**zu erstellen. Es wurde eine Reihe von Aktionen ausführen:

1. Eine Anforderung im folgenden Format angezeigt:

    > *http://www.yourwebsite.com/apimdelegation?operation=SignIn&returnUrl= {URL der Quellseite} & Salz = {String} & Sig = {String}*

    Abfrageparameter bei Anmeldung / Registrierung:
    - **Vorgang**: bezeichnet die Art der Delegierung anfordern - es kann nur **Anmelden** bei
    - **ReturnUrl**: die URL der Seite, dem Klicken auf einen Link anmelden oder Anmeldung,
    - **Salt**: eine spezielle salt Zeichenfolge computing Security Hash
    - **SIG**: berechneter Hash Hashfunktion berechnete Sicherheit für Ihre eigenen Vergleich verwendet werden soll

2. Stellen Sie sicher, dass die Anforderung aus Azure API Management (optional, aber aus Sicherheitsgründen dringend empfohlen)

    * Berechnet einen HMAC SHA512 Hash einer Zeichenfolge auf Grundlage der Abfrageparameter **ReturnUrl** und **Salz** ([Beispielcode unten]):
        > HMAC (**salt** "\n" + **ReturnUrl**)
         
    * Vergleichen Sie den oben berechneten Hash auf den Wert des Abfrageparameters **sig** Wenn die beiden Hashwerte übereinstimmen, zum nächsten Schritt weitergehen, andernfalls ablehnen.

2. Überprüfen, ob der Empfang einer Anforderung für Zeichen in/Anmeldung: **Vorgang** Abfrageparameter**Anmeldung"**festgelegt.

3. Zeigt Benutzeroberfläche anmelden oder Anmeldung

4. Ist der Benutzer Anmeldung müssen Sie ein Benutzerkonto für diese API Management erstellen. [Erstellen Sie den Benutzer] mit der API-REST-API. Dabei stellen Sie sicher, dass Sie die Benutzer-ID auf dasselbe in Ihrem Benutzerspeicher oder einer ID, die Sie nachverfolgen können.

5. Wenn der Benutzer erfolgreich authentifiziert wurde:

    * über die API-REST-API [anfordern ein Token Single Sign-On (SSO)]

    * SSO-URL von der API-Aufruf über erhalten fügen Sie einen Abfrageparameter ReturnUrl hinzu:
        > z.B. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 

    * Umleiten des Benutzers auf die obigen erzeugten URL

Neben der **Anmeldung** können Sie auch Kontenverwaltung durch die vorherigen Schritte ausführen und einen der folgenden Vorgänge ausführen.

-   **Kennwort ändern**
-   **ChangeProfile**
-   **CloseAccount**

Sie müssen die folgenden Abfrageparameter für Verwaltungsoperationen Konto übergeben.

-   **Vorgang**: Welche Delegierungsanfrage ist (ChangePassword, ChangeProfile oder CloseAccount) bezeichnet
-   **ID**: die Benutzer-Id des Kontos verwalten
-   **Salt**: eine spezielle salt Zeichenfolge computing Security Hash
-   **SIG**: berechneter Hash Hashfunktion berechnete Sicherheit für Ihre eigenen Vergleich verwendet werden soll

## <a name="delegate-product-subscription"> </a>Abonnement delegieren

Delegieren von Abonnement verhält sich Benutzer anmelden/oben delegieren. Der endgültige Workflow würde folgendermaßen aussehen:

1. Entwickler wählt ein Produkt im API Management-Entwicklerportal und klickt auf die Schaltfläche "Anmelden"
2. Browser wird die Delegierung Endpunkt umgeleitet.
3. Delegierungsendpunkt führt Produkt Abonnement aus - dies Sie und möglicherweise Umleiten zu einer anderen Seite Abrechnungsinformationen, weitere Fragen stellen, oder einfach die Informationen gespeichert wird und alle anfordern


Klicken Sie zum Aktivieren der Funktion auf **Delegierung** auf **Delegaten Abonnement**.

Stellen Sie anschließend sicher delegierungsendpunkt führt folgenden Aktionen durch:


1. Eine Anforderung im folgenden Format angezeigt:

    > *http://www.yourwebsite.com/apimdelegation?operation= {Vorgang} & ProductId = {Produkt abonnieren} & UserId = {User antragstellende} & salt = {String} & Sig = {String}*

    Abfrageparameter bei Abonnement Produkt:
    - **Vorgang**: Art der Delegierungsanfrage bezeichnet. Abonnement sind Anfragen gültigen Optionen:
        - "Abonnieren": eine Anforderung zum Abonnieren des Benutzers für ein bestimmtes Produkt mit bereitgestellten ID (siehe unten)
        - "Unsubscribe": eine Anforderung zum Abmelden eines Benutzers von einem Produkt
        - "Erneuern": haben ein Abonnement erneuern (z.B. die ablaufen kann)
    - **ProductId**: die ID des Produkts, die vom Benutzer angeforderte abonnieren
    - **ID**: die ID des Benutzers, für den die Anforderung erfolgt,
    - **Salt**: eine spezielle salt Zeichenfolge computing Security Hash
    - **SIG**: berechneter Hash Hashfunktion berechnete Sicherheit für Ihre eigenen Vergleich verwendet werden soll


2. Stellen Sie sicher, dass die Anforderung aus Azure API Management (optional, aber aus Sicherheitsgründen dringend empfohlen)

    * Berechnen Sie ein HMAC SHA512 einer Zeichenfolge auf Grundlage der **ProductId** **UserId** und **Salz** Abfrageparameter:
        > HMAC (**salt** '\n' **ProductId** + "\n" + **UserId**)
         
    * Vergleichen Sie den oben berechneten Hash auf den Wert des Abfrageparameters **sig** Wenn die beiden Hashwerte übereinstimmen, zum nächsten Schritt weitergehen, andernfalls ablehnen.
    
3. Verarbeiten Sie die Produkt-Abonnement basierend auf dem Typ Vorgang **Vorgang** – z. B. Rechnungsadresse, Fragen usw..

4. Abonnieren Sie auf den Benutzer, das Produkt auf der Seite erfolgreich anmelden Benutzer der API-Produkt telefonisch [Die REST API für Abonnement].

## <a name="delegate-example-code"></a> Beispiel ##

Diese Codebeispiele zeigen, wie *Delegierung Validierungsschlüssel*Delegierung Bildschirm des Portals Publisher erstellt einen HMAC dann dient zum Überprüfen der Signatur Nachweis der Gültigkeit der übergebenen ReturnUrl festgelegt. Derselbe Code funktioniert für ProductId und Benutzer-ID mit geringfügigen Änderung.

**C#-Code ReturnUrl Hash generieren**

    using System.Security.Cryptography;

    string key = "delegation validation key";
    string returnUrl = "returnUrl query parameter";
    string salt = "salt query parameter";
    string signature;
    using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
    {
        signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
        // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
        // compare signature to sig query parameter
    }


**NodeJS Code Hash ReturnUrl generieren**

    var crypto = require('crypto');
    
    var key = 'delegation validation key'; 
    var returnUrl = 'returnUrl query parameter';
    var salt = 'salt query parameter';
    
    var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
    var digest = hmac.update(salt + '\n' + returnUrl).digest();
    // change to (salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature to sig query parameter
    
    var signature = digest.toString('base64');

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Delegierung finden Sie im folgende Video.

> [AZURE.VIDEO delegating-user-authentication-and-product-subscription-to-a-3rd-party-site]

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[Anforderung ein Token Single Sign-On (SSO)]: http://go.microsoft.com/fwlink/?LinkId=507409
[Erstellen Sie einen Benutzer]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[die REST-API Aufrufen für Abonnement]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[Beispielcode unten]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 