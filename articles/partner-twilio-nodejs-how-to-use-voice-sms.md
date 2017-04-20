<properties 
    pageTitle="Twilio verwenden für Sprache, VoIP und SMS-Nachrichten in Azure" 
    description="Informationen Sie zum Anrufen und senden eine SMS mit der Twilio-API in Azure. Beispiele in Node.js." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Twilio verwenden für Sprache, VoIP und SMS-Nachrichten in Azure

Dieses Handbuch veranschaulicht die apps erstellen, die mit Twilio und node.js Azure kommunizieren.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Was ist Twilio?

Twilio ist eine API, die Entwickler stellen Anrufe, senden und empfangen Sie Textnachrichten und VoIP aufrufen browserbasierten und systemeigene Dienste einbetten erleichtert.  Wir gehen Sie kurz auf diesen Vorgang vor dem Einstieg.

### <a name="receiving-calls-and-text-messages"></a>Anrufe und Textnachrichten empfangen

Twilio ermöglicht Entwicklern [programmierbare Telefonnummern] kaufen[ purchase_phone] die dienen zum Senden und empfangen Anrufe und Textnachrichten.  Empfängt eine Twilio Zahl ein Anruf oder Text sendet Twilio Web-Anwendung ein HTTP POST oder GET-Anforderung fordern Sie Informationen wie den Aufrufs oder Text.  Der Server antwortet Twilios HTTP-Anforderung mit [TwiML][twiml], einen einfachen Satz von XML-Tags, wie ein Anruf oder Text enthalten.  Wir sehen Beispiele TwiML gleich.

### <a name="making-calls-and-sending-text-messages"></a>Anrufe und Textnachrichten

Indem HTTP-Anfragen an den Webdienst Twilio-API Entwickler Textnachrichten senden oder ausgehende Telefonanrufe zu initiieren.  Für ausgehende Anrufe muss Entwickler auch einen URL angeben, TwiML Anleitung zur Behandlung ausgehenden Anruf nach verbunden ist zurückgibt.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>VoIP-Funktionen in UI-Code (JavaScript, iOS oder Android) einbetten

Twilio stellt eine clientseitige SDK alle desktop-Webbrowser, iOS-app oder Android in einem VoIP-Telefon aktivieren kann.  In diesem Artikel konzentrieren wir uns auf wie VoIP im Browser aufrufen.  Neben Twilio JavaScript SDK im Browser muss eine serverseitige Anwendung (unsere node.js) JavaScript-Client ein Token"Funktion" erteilen verwendet werden.  Erfahren Sie mehr über die Verwendung von VoIP node.js [Twilio Dev Blog][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Anmelden für Twilio (Microsoft Preisnachlass)

Bevor Sie Twilio Services, müssen erste [für ein Konto anmelden][signup].  Microsoft Azure-Kunden erhalten einen Rabatt - [hier unbedingt][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Erstellen und Bereitstellen einer node.js Azure-Website

Als Nächstes müssen Sie eine Azure ausgeführt node.js-Website erstellen.  [Die offizielle Dokumentation hierfür liegt hier][azure_new_site].  Auf einer hohen Ebene werden Sie Folgendes tun:

* Wenn Sie nicht bereits eine haben anmelden für ein Azure-Konto
* Mithilfe der Azure-Verwaltungskonsole zum Erstellen einer neuen website
* Hinzufügen von Datenquellen Unterstützung (Sie Git angenommen)
* Erstellen einer Datei `server.js` mit einer einfachen node.js Web-Anwendung
* Diese einfache Anwendung in Azure bereitstellen

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Konfigurieren Sie das Twilio-Modul

Anschließend beginnen wir eine einfache node.js-Anwendung schreiben, die die Twilio-API verwendet.  Bevor wir beginnen, müssen wir unsere Twilio-Anmeldeinformationen.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Konfigurieren von Twilio Anmeldeinformationen in Umgebungsvariablen des Systems

Zu Twilio Back-End authentifizierte Abfragen werden unsere SID und Auth-Token, die Funktion als Benutzername und Passwort für unsere Twilio-Konto benötigen. Die sicherste Möglichkeit für die Verwendung mit den Knoten in Azure konfigurieren ist über Systemumgebungsvariablen, die direkt in der Azure-Verwaltungskonsole festgelegt werden kann.

Wählen der node.js-Website, und klicken Sie auf "Konfigurieren".  Wenn Sie ein wenig nach unten blättern, sehen Sie einen Bereich können Sie Eigenschaften für Ihre Anwendung einrichten.  Geben Sie Ihre Anmeldeinformationen Twilio ([gefunden auf dem Dashboard Twilio][twilio_dashboard]) Siehe - stellen sie "TWILIO_ACCOUNT_SID" und "TWILIO_AUTH_TOKEN" bzw. Namen:

![Azure-Verwaltungskonsole][azure-admin-console]

Nachdem Sie diese Variablen konfiguriert haben, starten Sie die Anwendung in Azure Konsole neu.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Das Modul Twilio package.json deklarieren

Als Nächstes müssen wir package.json zur Verwaltung unserer Knoten Modul Abhängigkeiten über [Npm]erstellen.  Erstellen Sie eine Datei namens "package.json" auf derselben Ebene wie die im Lernprogramm Azure/node.js erstellte Datei "server.js".  Fügen Sie in dieser Datei die folgenden:

  {"Name": "Anwendungsname", "Version": "0.0.1", "Privat": true "Scripts": {"start": "Knoten Server"}, "Dependencies": {"express": "3.1.0", "Ejs": "*", "Twilio": "*"}}

Diese Twilio Modul als Abhängigkeit beliebte [express Web-Framework] erklärt[ express] und der EJS-Vorlage.  Gut, sind jetzt wir alle - wir Code schreiben.

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Ausgehende Anrufe

Erstellen Sie ein einfaches Formular, das Tätigen eines Anrufs auf wir wählen.  Öffnen Sie server.js, und geben Sie den folgenden Code.  Beachten Sie, "E-Mail" - Geben Sie den Namen Ihrer Azure Website es sagt:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Anschließend erstellen Sie ein Verzeichnis namens "Ansichten" - in diesem Verzeichnis, erstellen Sie eine Datei namens "index.ejs" mit folgendem Inhalt:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Jetzt bereitstellen Sie Ihrer Website in Azure und öffnen Sie Ihre Homepage.  Sie sollen Ihre Telefonnummer in das Textfeld eingeben und erhalten einen Anruf von Ihrem Twilio Anzahl!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Senden Sie eine SMS-Nachricht

Nun richten Sie eine Benutzeroberfläche und Code für die Fehlerbehandlung Formular eine Textnachricht senden.  Öffnen Sie "server.js", und fügen Sie folgenden Code nach dem letzten Aufruf von "app.post":

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

Fügen Sie in "views/index.ejs" ein anderes Formular unter den ersten, eine Zahl und eine Textnachricht senden:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Erneutes Bereitstellen der Anwendung in Azure und sollte jetzt, die und selbst (oder einer Freunden) eine Textnachricht senden werden!

<a id="nextsteps"/>
## <a name="next-steps"></a>Nächste Schritte

Sie haben gelernt, die Grundlagen der Verwendung von node.js und Twilio apps erstellen, die kommunizieren.  Aber diese Beispiele kaum auflockern des node.js mit Twilio.  Überprüfen Sie Informationen node.js Twilio mit den folgenden Ressourcen:

* [Offizielle Modul Dokumente][docs]
* [Lernprogramm für VoIP mit node.js][voipnode]
* [Votr - Echtzeit SMS stimmen mit node.js und CouchDB (3 Teile)][votr]
* [Paarprogrammierung im Browser mit node.js][pair]

Wir hoffen, dass Hacker node.js und Twilio auf Azure gerne!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



