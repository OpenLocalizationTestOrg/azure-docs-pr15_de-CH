<properties
    pageTitle="Wie Sie einen Anruf von Twilio (PHP) | Microsoft Azure"
    description="Informationen Sie zum Anrufen und senden eine SMS mit der Twilio-API in Azure. Beispiele sind für PHP-Anwendung."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Wie mit Twilio in einer PHP-Anwendung in Azure anrufen

Das folgende Beispiel zeigt die Verwendung Twilio aus einer PHP-Webseite in Azure aufzurufen. Die resultierende Anwendung fordert den Benutzer Telefonanruf Werte, wie im folgenden Screenshot gezeigt.

![Azure Aufruf Formular mit Twilio und PHP][twilio_php]

Sie müssen den Code in diesem Thema verwenden folgendermaßen:

1. Erwerben Sie eine Twilio und token. Zunächst mit Twilio auswerten Preise [http://www.twilio.com/pricing][twilio_pricing]. Kann Anmeldung für ein Testkonto an [https://www.twilio.com/try-twilio][try_twilio]. Informationen über die API von Twilio finden Sie unter [http://www.twilio.com/api][twilio_api].
2. Installieren Sie der [Twilio-Bibliothek für PHP](https://github.com/twilio/twilio-php) oder als PEAR-Paket installieren. Weitere Informationen finden Sie in der [Readme-Datei](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Installieren Sie Azure SDK für PHP. Eine Übersicht über das SDK und Hinweise für die Installation finden Sie unter [Einrichten von Azure SDK für PHP][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Erstellen eines Webformulars für einen Anruf

Der folgende HTML-Code zeigt, wie eine Webseite (**callform.html**) erstellen, die Daten für einen Anruf abruft:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Erstellen Sie den Code für den Anruf
Im folgende Code veranschaulicht eine Webseite (**makecall.php**) erstellen, die aufgerufen wird, wenn das Absenden des Formulars durch **callform.html**angezeigt. Folgenden Code erstellt die Nachricht und den Aufruf generiert. (Die Twilio und statt, **$sid** und **$token** im unten stehenden Code zugewiesen Platzhalterwerte token verwenden).

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Neben den Anruf zeigt **makecall.php** Aufruf Metadaten (Beispiel in Abbildung unten). Weitere Informationen zu Aufruf Metadaten finden Sie unter [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Twilio mit PHP Azure Call-Reaktion][twilio_php_response]

## <a name="run-the-application"></a>Führen Sie die Anwendung
Im nächste Schritt wird die Anwendung in Azure Websites bereitstellen. Die folgenden Artikel enthalten Informationen zum Erstellen einer Website und Bereitstellen von Code mit Git, FTP oder WebMatrix (jedoch nicht alle Informationen in jedem Artikel relevant ist):

* [Eine PHP-MySQL Azure-Website erstellen und Bereitstellen mit Git][website-git]
* [Eine PHP-MySQL Azure-Website erstellen und Bereitstellen von mithilfe von FTP][website-ftp]

## <a name="next-steps"></a>Nächste Schritte
Dieser Code wurde mit Twilio in PHP auf Azure Grundfunktionen anzeigen bereitgestellt. Vor der Bereitstellung in Azure in der Produktion, sollten Sie weitere Fehlerbehandlung oder andere Funktionen hinzufügen. Zum Beispiel:

* Anstatt ein Webformular können Sie Blobs Azure Storage oder SQL-Datenbank Telefonnummern und Text aufrufen. Informationen zur Verwendung von Azure-Speicher Blobs in PHP finden Sie unter [Using Azure Storage mit PHP][howto_blob_storage_php]. Informationen über SQL-Datenbank in PHP finden Sie unter [Verwendung von SQL-Datenbank mit PHP][howto_sql_azure_php].
* **Makecall.php** Code verwendet Twilio bereitgestellte URL ([http://twimlets.com/message][twimlet_message_url]) Twilio Markup Language (TwiML) beantworten, die Twilio Vorgehensweise mit den Anruf informiert. Die zurückgegebene TwiML kann beispielsweise ein `<Say>` Verb Text gesprochen, des Empfängers führt. Anstatt Twilio bereitgestellte URL konnte einen eigenen Dienst reagiert auf Twilios Anforderung erstellen; Weitere Informationen finden Sie unter [verwenden Twilio für Sprach- und SMS-Funktionen in PHP][howto_twilio_voice_sms_php]. Weitere Informationen zu TwiML finden Sie unter [http://www.twilio.com/docs/api/twiml][twiml], und Weitere Informationen zu `<Say>` und andere Twilio Verben finden Sie unter [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lesen Sie die Sicherheitsrichtlinien Twilio am [https://www.twilio.com/docs/security][twilio_docs_security].

Weitere Informationen über Twilio finden Sie unter [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Siehe auch
* [Verwendung von Twilio für Sprach- und SMS-Funktionen in PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
