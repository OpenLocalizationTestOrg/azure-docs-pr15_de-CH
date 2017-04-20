<properties 
    pageTitle="Verwendung den SendGrid e-Mail-Dienst (PHP) | Microsoft Azure" 
    description="Erfahren Sie, wie senden SendGrid e-Mail-Dienst in Azure. Codebeispiele in PHP geschrieben." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Verwendung den SendGrid e-Mail-Dienst von PHP

Dieses Handbuch veranschaulicht, wie häufige SendGrid e-Mail-Dienst in Azure Programmieraufgaben. Die Proben sind in PHP geschrieben.
Die Szenarios umfassen **Erstellen e-Mail**, **e-Mail**und **Anhänge hinzufügen**. Weitere Informationen zu SendGrid und Senden von e-Mails finden Sie im Abschnitt [Weiter](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Was ist die SendGrid e-Mail-Service?

SendGrid ist ein [Cloud-basierten e-Mail-Dienst] zuverlässige [Transaktions-e-Mail-Zustellung], Skalierbarkeits- und Echtzeit-Analysen mit flexiblen APIs, die benutzerdefinierten Integration erleichtern. Allgemeine SendGrid Szenarien umfassen:

-   Senden von Empfangsbestätigungen automatisch an Kunden
-   Verwalten von Verteilerlisten für Kunden monatliche e-Flyer und Sonderangebote Listen
-   Sammeln von Echtzeit-Metriken blockierte und Kundenbetreuung
-   Generieren von Berichten zu Trends erkennen
-   Weiterleiten von Anfragen
- E-Mail-Benachrichtigung in der Anwendung

Weitere Informationen finden Sie unter [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>Erstellen eines Kontos SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>SendGrid aus der PHP-Anwendung verwenden

SendGrid in Azure PHP-Anwendung muss keine spezielle Konfiguration oder Codierung. Ist SendGrid Dienst kann in genau einer Cloud-Anwendung zugegriffen werden wie von einer lokalen Anwendung.

## <a name="how-to-send-an-email"></a>Gewusst wie: senden eine E-Mail

Senden Sie e-Mail mit SMTP oder Web API SendGrid bereitgestellt.

### <a name="smtp-api"></a>SMTP-API

Verwenden Sie zum Senden von e-Mails mit der SMTP-SendGrid API *Swift Mailer*, einer komponentenbasierten Bibliothek für e-Mails von PHP-Applikationen. Sie können die *Swift Mailer* -Bibliothek aus [http://swiftmailer.org/download][] v5.3.0 (verwenden Sie [Composer] Swift Mailer installiert). E-Mail mit der Bibliothek umfasst das Erstellen von Instanzen der <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_Mailer</span>, und <span class="auto-style2">Swift\_Nachricht</span> Klassen, die entsprechenden Eigenschaften und rufen die <span class="auto-style2">Swift\_Mailer::send</span> Methode.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Web-API

Mit der PHP [Funktion Aufrollen][] e-Mail SendGrid Web-API verwenden.

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

SendGrid des Web-API ähnelt eine REST-API, obwohl es nicht wirklich ein RESTful API in den meisten Aufrufen beide und POST-Verben sind austauschbar.

## <a name="how-to-add-an-attachment"></a>Gewusst wie: Hinzufügen einer Anlage

### <a name="smtp-api"></a>SMTP-API

Senden einer Anlage mit der SMTP-API umfasst eine zusätzliche Codezeile das Beispielskript für e-Mail mit Swift Mailer.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Zusätzliche Codezeile lautet wie folgt:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Diese Codezeile Ruft die Attach-Methode der <span class="auto-style2">Swift\_Nachricht</span> -Objekt und statische <span class="auto-style2">FromPath</span> auf der <span class="auto-style2">Swift\_Anlage</span> Klasse und eine Datei an eine Nachricht anhängen.

### <a name="web-api"></a>Web-API

Senden einer Anlage mithilfe der Web-API ähnelt einer e-Mail mithilfe der Web-API. Beachten Sie, dass im folgenden Beispiel wird das Parameterarray dieses Element enthalten muss:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Beispiel:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>How to: Filter verwenden, um Fußzeilen, Überwachung und Analysen aktivieren

SendGrid bietet zusätzliche e-Mail-Funktionen "Filter". Diese werden, die eine e-Mail-Nachricht aktivieren Sie bestimmte Funktionen wie das Aktivieren der Überwachung auf Google Analytics, Abonnement verfolgen, und hinzugefügt werden können.

Filter können mithilfe der Filter-Eigenschaft auf eine Nachricht angewendet werden. Jeder Filter wird durch eine Raute mit Filter-Einstellungen angegeben. Im folgenden Beispiel ermöglicht den Fußzeile Filter und gibt eine Textnachricht an, die am Ende der e-Mail-Nachricht angefügt wird.
In diesem Beispiel verwenden wir [Sendgrid Php-Bibliothek].
Verwenden Sie [Composer] Library installieren:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Beispiel:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Nächste Schritte

Grundlagen von SendGrid e-Mail-Dienst bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

-   SendGrid Dokumentation: <https://sendgrid.com/docs>
-   SendGrid PHP-Bibliothek: <https://github.com/sendgrid/sendgrid-php>
-   Sonderangebot für Azure Kunden SendGrid: <https://sendgrid.com/windowsazure.html>

Weitere Informationen finden Sie auch die [PHP-Entwicklercenter](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/Download]: http://swiftmailer.org/download
  [Curl-Funktion]: http://php.net/curl
  [Cloud-basierten e-Mail-Dienst]: https://sendgrid.com/email-solutions
  [transaktionale e-Mail-Zustellung]: https://sendgrid.com/transactional-email
  [Sendgrid Php-Bibliothek]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Komponist]: https://getcomposer.org/download/
