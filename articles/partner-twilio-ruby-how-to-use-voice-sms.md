<properties 
    pageTitle="Verwendung von Twilio für Voicemail und SMS (Ruby) | Microsoft Azure" 
    description="Informationen Sie zum Anrufen und senden eine SMS mit der Twilio-API in Azure. Codebeispiele in Ruby geschrieben." 
    services="" 
    documentationCenter="ruby" 
    authors="devinrader" 
    manager="twilio" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-ruby"></a>Verwendung von Twilio für Sprach- und SMS-Funktionen in Ruby
Dieses Handbuch veranschaulicht, wie allgemeine Programmieraufgaben mit der Twilio-API in Azure ausführen. Die Szenarios umfassen telefonieren und Senden einer Nachricht Short Message Service (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung finden Sie im Abschnitt [Weiter](#NextSteps) .

## <a id="WhatIs"></a>Was ist Twilio?
Twilio ist eine Telefonie Webdienst-API, die Sie Ihre Kenntnisse und Web Sprachen Sprach- und SMS-Anwendung erstellen kann. Twilio ist ein Drittanbieter-Dienst (kein Azure-Feature und kein Microsoft-Produkt).

**Twilio Voice** ermöglicht einer Anwendung zu telefonieren. **Twilio SMS** kann Ihre Anwendung und SMS-Nachrichten empfangen. **Twilio Client** können Ihre Anwendung mit vorhandenen Internet-Verbindung, einschließlich mobiler Kommunikation ermöglichen.

## <a id="Pricing"></a>Twilio Preise und Sonderangebote
Informationen zu Twilio Preise steht zu [Twilio] [twilio_pricing]. Azure-Kunden erhalten ein [Sonderangebot][special_offer]: einen zinslosen Kredit 1000 Texte oder 1000 Minuten eingehende. Für dieses Angebot anmelden oder Weitere Informationen finden Sie auf [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Konzepte
Die Twilio-API ist eine RESTful API, Voicemail und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio-API-Bibliotheken] [twilio_libraries].

### <a id="TwiML"></a>TwiML
TwiML ist ein Satz von XML-basierten Informationen, die wie ein Aufruf der Twilio oder SMS informieren.

Beispielsweise würde die folgenden TwiML Text **Hello World** in Sprache konvertieren.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Alle TwiML Dokumente `<Response>` als Stammelement. Dort werden Twilio Verben definieren das Verhalten der Anwendung verwenden.

### <a id="Verbs"></a>TwiML Verben
Twilio sind XML-Tags, die Twilio **dazu**sagen. Beispielsweise die ** &lt;Say&gt; ** Verb weist Twilio hörbar telefonieren Zustellungsversuche. 

Folgendes ist eine Liste der Verben Twilio.

* ** &lt;Wählen&gt;**: verbindet den Anrufer an ein anderes Telefon.
* ** &lt;Sammeln&gt;**: sammelt Ziffern auf der Telefontastatur eingegeben.
* ** &lt;Auflegen&gt;**: einen Aufruf beendet.
* ** &lt;Spielen&gt;**: eine Audiodatei.
* ** &lt;Anhalten&gt;**: automatisch für eine angegebene Anzahl von Sekunden wartet.
* ** &lt;Datensatz&gt;**: Zeichnet den Anrufer und gibt eine URL einer Datei mit der Aufzeichnung.
* ** &lt;Umleiten&gt;**: Steuerung von Anrufen oder SMS an die TwiML auf eine andere URL.
* ** &lt;Ablehnen&gt;**: einen Anruf an Twilio ohne Abrechnung Sie abgelehnt
* ** &lt;Say&gt;**: bei einem Aufruf konvertiert Text zu Sprache.
* ** &lt;Sms&gt;**: eine SMS-Nachricht sendet.

Weitere Informationen zu Twilio Verben, Attribute und TwiML finden Sie unter [TwiML] [twiml]. Weitere Informationen über die Twilio-API finden Sie unter [Twilio-API-] [twilio_api].

## <a id="CreateAccount"></a>Twilio registrieren
Wenn Sie ein Twilio-Konto erhalten möchten, melden Sie sich bei [Twilio versuchen] [try_twilio]. Sie können ein kostenloses Konto starten und Ihr Konto später aktualisieren.

Wenn Sie ein Twilio-Konto anmelden, erhalten Sie eine Telefonnummer für Ihre Anwendung. Sie erhalten auch eine Konto-SID und Auth-Token. Beide benötigt Twilio-API-Aufrufe. Um nicht autorisierten Zugriff auf Ihr Konto verhindern schützen Sie Ihr Authentifizierungstoken. Die Konto-SID und Auth-Token sind auf der [Seite Twilio][twilio_account]in die Feldern Beschriftung **HAUPTKONTO-SID** und **AUTH-TOKEN**.

### <a id="VerifyPhoneNumbers"></a>Rufnummern überprüfen
Neben erhalten von Twilio, Sie können auch überprüfen, Zahlen Steuerelement (z.B. Mobiltelefon oder Home Telefonnummer) für die Verwendung in der Anwendung. 

Informationen zum Überprüfen einer Telefonnummer finden Sie unter [Verwalten Zahlen] [verify_phone].

## <a id="create_app"></a>Ruby-Anwendung erstellen
Ruby Anwendung, die mithilfe des Twilio-Dienstes in Azure ausgeführt wird ist nicht anders als andere Ruby Anwendung, die die Twilio verwendet. Während Twilio Dienste RESTful und können auf mehrere Arten von Ruby aufgerufen, in diesem Artikel Hauptanliegen [Hilfsprogrammbibliothek für Ruby Twilio]Twilio Services mit[twilio_ruby].

Erstens [Einrichtung einer neuen Azure Linux VM] [ azure_vm_setup] fungieren als Host für Ihre neue Ruby Webanwendung. Ignorieren Sie die Schritte, die Schaffung von Schienen app nur Einrichtung der VM. Stellen Sie sicher, dass einen Endpunkt einen internen Port 5000 mit einem externen Port 80 zu erstellen.

In den folgenden Beispielen werden wir mit [Sinatra][sinatra], ein sehr einfaches Web-Framework für Ruby. Aber sicherlich können Sie Twilio-Hilfsbibliothek Ruby mit anderen Web Frameworks, einschließlich Ruby on Rails.

SSH in Ihrem neuen VM und ein Verzeichnis für Ihre neue Anwendung erstellen. In diesem Verzeichnis eine Datei namens Gemfile, und kopieren Sie den folgenden Code ein:

    source 'https://rubygems.org'
    gem 'sinatra'
    gem 'thin'

In der Befehlszeile ausführen `bundle install`. Oben Abhängigkeiten wird installiert. Als Nächstes erstellen Sie eine Datei namens `web.rb`. Diese werden der Code Ihrer Anwendung lebt. Fügen Sie folgenden Code ein:

    require 'sinatra'

    get '/' do
        "Hello Monkey!"
    end

An diesem Punkt sollten möglicherweise die Ausführung des Befehls `ruby web.rb -p 5000`. Dies wird einen kleinen Webserver auf Port 5000 Hochfahren. Sie sollten diese App im Browser suchen URL besuchen Sie Setup Azure-VM sein. Wenn man Ihrer Anwendung im Browser können Sie eine Twilio App zu starten.

## <a id="configure_app"></a>Konfigurieren Sie die Anwendung für Twilio
Sie können Ihrer Anwendung verwendet die Twilio-Bibliothek aktualisieren Ihre `Gemfile` dieser Zeile enthalten:

    gem 'twilio-ruby'

Führen Sie in der Befehlszeile `bundle install`. Öffnen Sie jetzt `web.rb` und diese Zeile oben:

    require 'twilio-ruby'

Sie sind jetzt bereit, Twilio Hilfsprogrammbibliothek für Ruby in Ihrer Anwendung verwenden.

## <a id="howto_make_call"></a>Gewusst wie: Ausgehender Anruf
Der folgende Code zeigt einen Anruf zu. Schlüsselkonzepte sind mithilfe der Twilio-Hilfsbibliothek Ruby REST API-Anrufe und TwiML rendern. Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und sicherstellen Sie, dass die Telefonnummer **von** für Ihr Konto Twilio vor Ausführung des Codes überprüfen.

Diese Funktion hinzufügen `web.md`:

    # Set your account ID and authentication token.
    sid = "your_twilio_account_sid";
    token = "your_twilio_authentication_token";

    # The number of the phone initiating the the call.
    # This should either be a Twilio number or a number that you've verified
    from = "NNNNNNNNNNN";

    # The number of the phone receiving call.
    to = "NNNNNNNNNNN";

    # Use the Twilio-provided site for the TwiML response.
    url = "http://yourdomain.cloudapp.net/voice_url";
      
    get '/make_call' do
      # Create the call client.
      client = Twilio::REST::Client.new(sid, token);
      
      # Make the call
      client.account.calls.create(to: to, from: from, url: url)
    end

    post '/voice_url' do
      "<Response>
         <Say>Hello Monkey!</Say>
       </Response>"
    end
    
Wenn Sie öffnen `http://yourdomain.cloudapp.net/make_call` im Browser auslösen, die den Aufruf der Twilio-API für den Telefonanruf. Die ersten beiden Parameter in `client.account.calls.create` selbst: die Zahl der Aufruf ist `from` und die Zahl der Aufruf `to`. 

Der dritte Parameter (`url`) ist der URL Twilio Informationen erhalten, was zu tun ist, nachdem die Verbindung an. In diesem Fall wir Einrichtung einen URL (`http://yourdomain.cloudapp.net`), gibt ein einfaches TwiML Dokument und verwendet die `<Say>` Verb einige Sprachausgabe und sagen "Hallo Monkey" Person den Anruf.

## <a id="howto_recieve_sms"></a>Gewusst wie: eine SMS-Nachricht empfangen
Im vorherigen Beispiel haben wir einen **ausgehenden** Anruf. Diese Zeit, verwenden wir die Telefonnummer Twilio uns bei der Anmeldung hat eine **eingehende** SMS-Nachricht verarbeitet.

Erstens die [Twilio Dashboard]anmelden[twilio_account]. Klicken Sie auf "Zahlen" im oberen Navigationsbereich, und klicken Sie auf die Twilio Nummer wurden. Sie sehen zwei URLs, die Sie konfigurieren können. URL zur Anforderung von einer Stimme Request-URL und SMS. Dies sind die Twilio aufruft, wenn ein Anruf getätigt oder eine SMS an Ihre URLs. Die URLs werden auch als "Web-Hooks" bezeichnet.

Wir möchten verarbeitet eingehende SMS aktualisieren wir also die URL `http://yourdomain.cloudapp.net/sms_url`. Fahren Sie fort, und klicken Sie auf Speichern am unteren Rand der Seite. Jetzt sichern `web.rb` wir unsere Anwendung dafür programmieren:

    post '/sms_url' do
      "<Response>
         <Message>Hey, thanks for the ping! Twilio and Azure rock!</Message>
       </Response>"
    end

Stellen Sie nach der Änderung Ihrer Anwendung neu starten. Jetzt nehmen Sie Ihr Telefon und senden Sie SMS an Twilio. Sie sollten umgehend eine SMS-Antwort erhalten, "Hey, danke Ping! Twilio und Azure Rock! ".

## <a id="additional_services"></a>Gewusst wie: zusätzliche Twilio Dienste
Neben den hier aufgeführten Beispielen bietet Twilio Web-basierte APIs, mit denen Sie weitere Twilio Funktionen von Azure-Anwendung nutzen. Einzelheiten finden Sie in der [Dokumentation zur Twilio-API] [twilio_api_documentation].

### <a id="NextSteps"></a>Nächste Schritte
Die Grundlagen des Twilio-Dienstes bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr:

* [Twilio Sicherheitsrichtlinien] [twilio_security_guidelines]
* [Twilio Anleitungen und Beispielcode] [twilio_howtos]
* [Twilio-Schnellstart-Lernprogramme][twilio_quickstarts] 
* [Twilio auf GitHub] [twilio_on_github]
* [Wenden Sie sich an Twilio-Unterstützung] [twilio_support]

[twilio_ruby]: https://www.twilio.com/docs/ruby/install





[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
[sinatra]: http://www.sinatrarb.com/
[azure_vm_setup]: http://www.windowsazure.com/develop/ruby/tutorials/web-app-with-linux-vm/
