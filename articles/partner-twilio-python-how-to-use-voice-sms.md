<properties
    pageTitle="Verwendung von Twilio für Voicemail und SMS (PHP) | Microsoft Azure"
    description="Informationen Sie zum Anrufen und senden eine SMS mit der Twilio-API in Azure. Codebeispiele in PHP geschrieben."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Verwendung von Twilio für Sprach- und SMS-Funktionen in PHP
Dieses Handbuch veranschaulicht, wie allgemeine Programmieraufgaben mit der Twilio-API in Azure ausführen. Die Szenarios umfassen telefonieren und Senden einer Nachricht Short Message Service (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung finden Sie im Abschnitt [Weiter](#NextSteps) .

## <a id="WhatIs"></a>Was ist Twilio?
Twilio betreibt die Zukunft der Geschäftskommunikation ermöglicht es Entwicklern, Voice, VoIP und messaging in Applikationen eingebettet. Sie virtualisiert alle Infrastrukturen in einer Cloud-basierte globale Umgebung, mit Twilio Kommunikationsplattform API. Erstellen Sie einfach und skalierbar sind. Flexibilität mit Pay-as-you gehen Preise und Cloud Zuverlässigkeit profitieren.

**Twilio Voice** ermöglicht einer Anwendung zu telefonieren. **Twilio SMS** kann eine Anwendung senden und Empfangen von Textnachrichten. **Twilio-Client** können Sie VoIP-Anrufe Telefon, Tablet oder Browser und WebRTC unterstützt.

## <a id="Pricing"></a>Twilio Preise und Sonderangebote

Azure-Kunden erhalten [Angebot] $10 Twilio haben Wenn Sie Ihr Konto Twilio aktualisieren Twilio Nutzung (1.000 SMS-Nachrichten senden oder Empfangen von bis zu 1000 eingehende Voice-Minuten je nach Telefon und Nachricht oder Aufruf Ziel entspricht $10 haben) können diese Gutschrift Twilio zugewiesen. Die Gutschrift Twilio einlösen und fangen Sie an: [ahoy.twilio.com/azure].

Twilio ist Bedarfsbasis. Fallen keine Einrichtung, und Sie können Ihr Konto jederzeit schließen. Finden Sie weitere Informationen zu [Twilio] [twilio_pricing].

## <a id="Concepts"></a>Konzepte
Die Twilio-API ist eine RESTful API, Voicemail und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio-API-Bibliotheken] [twilio_libraries].

Die Twilio-API sind Twilio Verben und Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio Verben
Die API nutzt Twilio Verben. beispielsweise die ** &lt;Say&gt; ** Verb weist Twilio hörbar telefonieren Zustellungsversuche.

Folgendes ist eine Liste der Verben Twilio. Erfahren Sie mehr über die anderen Verben und Funktionen über [Twilio Markup Language Documentation] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Wählen&gt;**: verbindet den Anrufer an ein anderes Telefon.
* ** &lt;Sammeln&gt;**: sammelt Ziffern auf der Telefontastatur eingegeben.
* ** &lt;Auflegen&gt;**: einen Aufruf beendet.
* ** &lt;Spielen&gt;**: eine Audiodatei.
* ** &lt;Anhalten&gt;**: automatisch für eine angegebene Anzahl von Sekunden wartet.
* ** &lt;Datensatz&gt;**: Zeichnet die Stimme des Aufrufers und gibt eine URL einer Datei mit der Aufzeichnung.
* ** &lt;Umleiten&gt;**: Steuerung von Anrufen oder SMS an die TwiML auf eine andere URL.
* ** &lt;Ablehnen&gt;**: einen Anruf an Twilio ohne Abrechnung Sie abgelehnt
* ** &lt;Say&gt;**: bei einem Aufruf konvertiert Text zu Sprache.
* ** &lt;Sms&gt;**: eine SMS-Nachricht sendet.

### <a id="TwiML"></a>TwiML
TwiML ist ein Satz von XML-basierten Anweisungen Twilio Verben, die Twilio wie den Aufruf oder SMS informieren.

Beispielsweise würde die folgenden TwiML Text **Hello World** in Sprache konvertieren.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Die Anwendung die Twilio-API aufruft, wird eine API-Parameter der URL, der die TwiML-Antwort zurückgegeben. Twilio bereitgestellten URLs können Sie bei der TwiML Antworten von der Anwendung verwendet. Sie können auch eigene URLs erstellen TwiML Antworten hosten und können Sie das **TwiMLResponse** -Objekt verwenden.

Weitere Informationen zu Twilio Verben, Attribute und TwiML finden Sie unter [TwiML] [twiml]. Weitere Informationen über die Twilio-API finden Sie unter [Twilio-API-] [twilio_api].

## <a id="CreateAccount"></a>Twilio registrieren
Wenn Sie ein Konto Twilio erhalten möchten, melden Sie sich bei [Twilio versuchen Sie] [try_twilio]. Sie können ein kostenloses Konto starten und Ihr Konto später aktualisieren.

Wenn Sie ein Twilio-Konto anmelden, erhalten Sie eine Konto-ID und ein Authentifizierungstoken. Beide benötigt Twilio-API-Aufrufe. Um nicht autorisierten Zugriff auf Ihr Konto verhindern schützen Sie Ihr Authentifizierungstoken. Ihre ID und Authentifizierung Token sind auf der [Seite Twilio] [twilio_account]in die Feldern Beschriftung **HAUPTKONTO-SID** und **AUTH-TOKEN**.

## <a id="create_app"></a>Erstellen Sie eine PHP-Anwendung
Eine PHP-Anwendung, die verwendet der Dienst Twilio und in Azure ausgeführt wird, ist nicht anders als alle anderen PHP-Anwendung, die die Twilio verwendet. Und Twilio-Services basieren auf REST von PHP auf verschiedene Weise aufgerufen werden, dieser Artikel konzentriert sich auf Verwendung Twilio Services mit [Twilio Bibliothek für PHP von GitHub][twilio_php]. Weitere Informationen über die Twilio-Bibliothek für PHP finden Sie unter [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Ausführliche Informationen zum Erstellen und Bereitstellen einer Twilio-PHP-Anwendung in Azure stehen wie [ein Anruf mithilfe von Twilio in einer PHP-Anwendung in Azure zu][howto_phonecall_php].

## <a id="configure_app"></a>Konfigurieren Sie Ihre Anwendung Twilio Bibliotheken
Sie können die Anwendung für die Twilio-Bibliothek für PHP auf zwei Arten konfigurieren:

1. Twilio-Bibliothek für PHP von GitHub herunterladen ([https://github.com/twilio/twilio-php][twilio_php]) und **das Verzeichnis** der Anwendung.

    -ODER-

2. Twilio-Bibliothek für PHP als BIRNE Paket installiert. Sie können mit den folgenden Befehlen installiert werden:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Nach der Installation der Twilio-Bibliothek für PHP können Sie **per** -Anweisung am Anfang der PHP-Dateien auf die Bibliothek hinzufügen:

        require_once 'Services/Twilio.php';

Weitere Informationen finden Sie unter [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Gewusst wie: Ausgehender Anruf
Der folgende Code zeigt einen Anruf mit der **Services_Twilio** -Klasse zu. Dieser Code verwendet eine Twilio bereitgestellte Website auch zum Zurückgeben der Antwort Twilio Markup Language (TwiML). Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und sicherstellen Sie, dass die Telefonnummer **von** für Ihr Konto Twilio vor Ausführung des Codes überprüfen.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Wie bereits erwähnt, wird mit dieser Code Twilio bereitgestellte Website TwiML Antwort zurückgegeben. Ihre Site können stattdessen die TwiML reagieren; Weitere Informationen finden Sie unter [Bereitstellen TwiML Antworten aus Ihrer eigenen Website](#howto_provide_twiml_responses).


- **Hinweis**: um SSL Zertifikat Validierungsfehler zu beheben, finden Sie unter [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Gewusst wie: Senden Sie eine SMS-Nachricht
Der folgende Code zeigt eine SMS-Nachricht mithilfe der **Services_Twilio** -Klasse an. Die Anzahl **von** bietet Twilio für Testkonten SMS-Nachrichten senden. **Die Nummer** muss für das Twilio-Konto vor dem Ausführen des Codes überprüft.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Verwendung: TwiML Antworten auf Ihrer eigenen Website
Wenn Ihre Anwendung einen Aufruf an die Twilio-API initiiert, sendet Twilio Wunsch zu einem URL, der eine TwiML Antwort erwartet wird. Im obigen Beispiel verwendet Twilio bereitgestellte URL- [http://twimlets.com/message][twimlet_message_url]. (Beim TwiML für Twilio bestimmt ist, können Sie die It in Ihrem Browser anzeigen. Klicken Sie beispielsweise auf [http://twimlets.com/message] [ twimlet_message_url] an eine leere `<Response>` Element. Klicken Sie beispielsweise auf [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] zu einer `<Response>` Element enthält ein `<Say>` Element.)

Anstatt auf den Twilio bereitgestellte URL können Sie Ihre eigene Website erstellen, HTTP-Antworten zurückgibt. Sie können die Website in jeder Sprache erstellen, die XML-Antworten zurückgibt; In diesem Thema wird vorausgesetzt, dass Sie PHP mithilfe der TwiML erstellen.

Die folgende PHP-Seite auf einen TwiML, der sagt **Hello World** Aufruf führt.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Als Beispiel sehen können, ist die Antwort TwiML einfach ein XML-Dokument. Twilio-Bibliothek für PHP enthält Klassen, die TwiML zu generieren. Im folgenden Beispiel die entsprechende Antwort erzeugt, wie oben gezeigt, verwendet jedoch den **Services\_Twilio\_Twiml** Klasse in der Twilio-Bibliothek für PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Weitere Informationen zu TwiML finden Sie unter [https://www.twilio.com/docs/api/twiml][twiml_reference].

Haben Sie Ihre PHP-Seite eingerichtet TwiML Antworten, verwenden Sie den URL der PHP-Seite URL übergeben der `Services_Twilio->account->calls->create` Methode. Beispielsweise haben Sie eine Anwendung mit dem Namen **MyTwiML** bereitgestellt ein Azure gehosteten Dienst PHP-Seite ist **mytwiml.php**und kann der URL übergeben werden **Services_Twilio-Konto >-Aufrufe > -> Erstellen Sie** wie im folgenden Beispiel gezeigt:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Weitere Informationen zur Verwendung von Twilio in Azure mit PHP finden Sie [einen Anruf mithilfe von Twilio in einer PHP-Anwendung in Azure zu][howto_phonecall_php].

## <a id="AdditionalServices"></a>Gewusst wie: zusätzliche Twilio Dienste
Neben den hier aufgeführten Beispielen bietet Twilio Web-basierte APIs, mit denen Sie weitere Twilio Funktionen von Azure-Anwendung nutzen. Einzelheiten finden Sie in der [Dokumentation zur Twilio-API] [twilio_api_documentation].

## <a id="NextSteps"></a>Nächste Schritte
Die Grundlagen des Twilio-Dienstes bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr:

* [Twilio Sicherheitsrichtlinien] [twilio_security_guidelines]
* [Twilio wie Führungslinien und Beispielcode] [twilio_howtos]
* [Twilio-Schnellstart-Lernprogramme][twilio_quickstarts]
* [Twilio auf GitHub] [twilio_on_github]
* [Wenden Sie sich an Twilio-Unterstützung] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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
