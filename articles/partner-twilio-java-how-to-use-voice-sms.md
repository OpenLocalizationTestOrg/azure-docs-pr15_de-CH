<properties 
    pageTitle="Verwendung von Twilio für Voicemail und SMS (Java) | Microsoft Azure" 
    description="Informationen Sie zum Anrufen und senden eine SMS mit der Twilio-API in Azure. Codebeispiele in Java geschrieben." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Verwendung von Twilio für Sprach- und SMS-Funktionen in Java

Dieses Handbuch veranschaulicht, wie allgemeine Programmieraufgaben mit der Twilio-API in Azure ausführen. Die Szenarios umfassen telefonieren und Senden einer Nachricht Short Message Service (SMS). Weitere Informationen zu Twilio und Voicemail und SMS in Ihrer Anwendung finden Sie im Abschnitt [Weiter](#NextSteps) .

## <a id="WhatIs"></a>Was ist Twilio?
Twilio ist eine Telefonie Webdienst-API, die Sie Ihre Kenntnisse und Web Sprachen Sprach- und SMS-Anwendung erstellen kann. Twilio ist ein Drittanbieter-Dienst (kein Azure-Feature und kein Microsoft-Produkt).

**Twilio Voice** ermöglicht einer Anwendung zu telefonieren. **Twilio SMS** kann Ihre Anwendung und SMS-Nachrichten empfangen. **Twilio Client** können Ihre Anwendung mit vorhandenen Internet-Verbindung, einschließlich mobiler Kommunikation ermöglichen.

## <a id="Pricing"></a>Twilio Preise und Sonderangebote
Informationen zu Twilio Preise steht zu [Twilio] [twilio_pricing]. Azure-Kunden erhalten ein [Sonderangebot][special_offer]: einen zinslosen Kredit 1000 Texte oder 1000 Minuten eingehende. Für dieses Angebot anmelden oder Weitere Informationen finden Sie auf [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Konzepte
Die Twilio-API ist eine RESTful API, Voicemail und SMS-Funktionalität für Applikationen bereitstellt. Client-Bibliotheken sind in mehreren Sprachen verfügbar. eine Liste finden Sie unter [Twilio-API-Bibliotheken] [twilio_libraries].

Die Twilio-API sind Twilio Verben und Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio Verben
Die API nutzt Twilio Verben. beispielsweise die ** &lt;Say&gt; ** Verb weist Twilio hörbar telefonieren Zustellungsversuche. 

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
Wenn Sie ein Twilio-Konto erhalten möchten, melden Sie sich bei [Twilio versuchen] [try_twilio]. Sie können ein kostenloses Konto starten und Ihr Konto später aktualisieren.

Wenn Sie ein Twilio-Konto anmelden, erhalten Sie eine Konto-ID und ein Authentifizierungstoken. Beide benötigt Twilio-API-Aufrufe. Um nicht autorisierten Zugriff auf Ihr Konto verhindern schützen Sie Ihr Authentifizierungstoken. Ihre ID und Authentifizierung Token sind auf der [Seite Twilio] [twilio_account]in die Feldern Beschriftung **HAUPTKONTO-SID** und **AUTH-TOKEN**.

## <a id="create_app"></a>Erstellen Sie eine Java-Anwendung
1. Twilio Glas erhalten und Java Buildpfad zu Ihrer Assembly WAR Bereitstellung hinzufügen. Bei [https://github.com/twilio/twilio-java][twilio_java], GitHub Quellen herunterladen und eigene JAR erstellen oder vorgefertigte JAR-Datei (mit oder ohne Abhängigkeiten) herunterladen.
2. Sicherstellen der JDK **Cacerts** Schlüsselspeicher enthält die Equifax Secure Zertifizierungsstelle mit MD5 Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (die Seriennummer 35:DE:F4:CF und SHA1 Fingerabdruck D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dies ist das Zertifikat der Zertifizierungsstelle (CA) [https://api.twilio.com] [ twilio_api_service] Dienst aufgerufen wird, wenn Sie Twilio-APIs verwenden. Schlüsselspeicher enthält Informationen über die JDK- **Cacerts** das richtige Zertifizierungsstellenzertifikat, finden Sie unter [Hinzufügen eines Zertifikats zum Zertifikatspeicher Java CA][add_ca_cert].

Ausführliche Informationen zur Verwendung der Twilio-Clientbibliothek für Java finden Sie unter [Telefon mithilfe von Twilio in einer Java-Anwendung in Azure zu][howto_phonecall_java].

## <a id="configure_app"></a>Konfigurieren Sie Ihre Anwendung Twilio Bibliotheken
Im Code können Sie Anweisung **Importieren** hinzufügen, am Anfang der Quelldateien für die Twilio Pakete Klassen in Ihrer Anwendung verwenden möchten. 

Für Java-Quelldateien:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Für Java Server Page (JSP) Dateien:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Je nachdem, welche Twilio Pakete oder Klassen verwenden möchten abweichen **Importieren** -Anweisungen.

## <a id="howto_make_call"></a>Gewusst wie: Ausgehender Anruf
Der folgende Code zeigt einen Anruf mit der **CallFactory** -Klasse zu. Dieser Code verwendet eine Twilio bereitgestellte Website auch zum Zurückgeben der Antwort Twilio Markup Language (TwiML). Ersetzen Sie die Werte für die Telefonnummern **von** und **bis** , und sicherstellen Sie, dass die Telefonnummer **von** für Ihr Konto Twilio vor Ausführung des Codes überprüfen.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Weitere Informationen über die **CallFactory.create** -Methode übergebenen Parameter finden Sie unter [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Wie bereits erwähnt, wird mit dieser Code Twilio bereitgestellte Website TwiML Antwort zurückgegeben. Ihre Site können stattdessen die TwiML reagieren; Weitere Informationen finden Sie unter [TwiML Antworten in einer Java-Anwendung in Azure bereitzustellen](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Gewusst wie: Senden Sie eine SMS-Nachricht
Der folgende Code zeigt eine SMS-Nachricht mithilfe der **SmsFactory** -Klasse an. **Aus** Anzahl **4155992671**vorgesehenen Twilio bei Testkonten SMS-Nachrichten senden. **Die Nummer** muss für das Twilio-Konto vor dem Ausführen des Codes überprüft.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Weitere Informationen über die **SmsFactory.create** -Methode übergebenen Parameter finden Sie unter [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Verwendung: TwiML Antworten auf Ihrer eigenen Website
Wenn Ihre Anwendung einen Aufruf an die Twilio-API initiiert, senden beispielsweise über die **CallFactory.create** -Methode Twilio Ihre Anforderung an einen URL, die voraussichtlich eine TwiML Antwort. Im obigen Beispiel verwendet Twilio bereitgestellte URL- [http://twimlets.com/message][twimlet_message_url]. (Während TwiML für Webdienste entwickelt ist, können Sie die TwiML im Browser anzeigen. Klicken Sie beispielsweise auf [http://twimlets.com/message] [ twimlet_message_url] an eine leere ** &lt;Antwort&gt; ** Element. Klicken Sie beispielsweise auf [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] zu einer ** &lt;Antwort&gt; ** Element enthält ein ** &lt;sagen&gt; ** Element.)

Anstatt auf den Twilio bereitgestellte URL können Sie Ihre URL-Site erstellen, HTTP-Antworten zurückgibt. Sie können die Website in jeder Sprache erstellen, die HTTP-Antworten zurückgibt; In diesem Thema wird davon ausgegangen, dass Sie die URL in eine JSP-Seite hosten werden.

Folgende JSP-Seite auf einen TwiML, der sagt **Hello World** Aufruf führt.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Folgende JSP-Seite führt, die Text sagt mehrere Pausen, sagt Informationen über die Twilio-API-Version und Name der Azure TwiML Antwort.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

Der **Anforderungsparameter** Parameter steht Twilio Stimme Anfragen (keine SMS-Anfragen). Finden Sie die verfügbaren Anforderungsparameter für Twilio Sprach- und SMS-Anfragen finden Sie unter <https://www.twilio.com/docs/api/twiml/twilio_request> und <https://www.twilio.com/docs/api/twiml/sms/twilio_request>. Die Umgebungsvariable **Rollenname** steht als Teil der Azure-Bereitstellung. (Benutzerdefinierte Umgebungsvariablen hinzufügen, damit sie von **System.getenv**abgerufen werden, finden Sie unter Variablen Umgebungsbereich auf [Verschiedene Einstellungen Konfiguration][misc_role_config_settings].)

Haben die JSP-Seite eingerichtet TwiML Antworten verwenden Sie den URL der JSP-Seite an die **CallFactory.create** -Methode übergebenen URL. Beispielsweise haben Sie eine Anwendung mit dem Namen MyTwiML eine Azure bereitgestellt gehosteter Dienst, JSP-Seite ist mytwiml.jsp und **CallFactory.create** wie im folgenden Beispiel kann die URL übergeben werden:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Eine weitere Option für mit TwiML ist über die **TwiMLResponse** -Klasse, die im Paket **com.twilio.sdk.verbs** .

Weitere Informationen zur Verwendung von Twilio in Azure mit Java finden Sie [einen Anruf mithilfe von Twilio in einer Java-Anwendung in Azure zu][howto_phonecall_java].

## <a id="AdditionalServices"></a>Gewusst wie: zusätzliche Twilio Dienste
Neben den hier aufgeführten Beispielen bietet Twilio Web-basierte APIs, mit denen Sie weitere Twilio Funktionen von Azure-Anwendung nutzen. Einzelheiten finden Sie in der [Dokumentation zur Twilio-API] [twilio_api_documentation].

## <a id="NextSteps"></a>Nächste Schritte
Die Grundlagen des Twilio-Dienstes bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr:

* [Twilio Sicherheitsrichtlinien] [twilio_security_guidelines]
* [Twilio HowTos und Beispielcode] [twilio_howtos]
* [Twilio-Schnellstart-Lernprogramme][twilio_quickstarts] 
* [Twilio auf GitHub] [twilio_on_github]
* [Wenden Sie sich an Twilio-Unterstützung] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
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
