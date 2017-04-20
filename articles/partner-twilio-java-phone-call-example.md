<properties 
    pageTitle="Verwendung von Twilio (Java) anrufen | Microsoft Azure" 
    description="Erfahren Sie, wie auf einer Webseite mit Twilio in einer Java-Anwendung in Azure anrufen." 
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

# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Wie mit Twilio in einer Java-Anwendung in Azure anrufen 

Das folgende Beispiel veranschaulicht die Verwendung Twilio, von einer Webseite in Azure aufzurufen. Die resultierende Anwendung fordert den Benutzer Telefonanruf Werte, wie im folgenden Screenshot gezeigt.

![Azure Aufruf Formular Twilio mit Java][twilio_java]

Sie müssen den Code in diesem Thema verwenden folgendermaßen:

1. Erwerben Sie eine Twilio und token. Zunächst mit Twilio auswerten Preise [http://www.twilio.com/pricing][twilio_pricing]. Melde dich an [https://www.twilio.com/try-twilio][try_twilio]. Informationen über die API von Twilio finden Sie unter [http://www.twilio.com/api][twilio_api].
2. Twilio Glas zu erhalten. Bei [https://github.com/twilio/twilio-java][twilio_java_github], GitHub Quellen herunterladen und eigene JAR erstellen oder vorgefertigte JAR-Datei (mit oder ohne Abhängigkeiten) herunterladen.
Der Code in diesem Thema wurde mit vorgefertigten TwilioJava 3.3.8 mit Abhängigkeiten JAR-Datei geschrieben.
3. Die Java-Erstellungspfad JAR-Datei hinzufügen.
4. Verwenden Sie Eclipse diese Java-Anwendung erstellen, Twilio JAR Ihre Anwendung Bereitstellungsdatei enthalten Sie (KRIEG) mit Eclipse Bereitstellung Baugruppen-KE. Verwenden Sie keine Eclipse diese Java-Anwendung erstellen, sicherstellen, dass Twilio JAR innerhalb der gleichen Azure-Rolle wie die Java-Anwendung, und den Pfad der Anwendung hinzugefügt.
5. Sicherstellen der Schlüsselspeicher Cacerts enthält die Equifax Secure Zertifizierungsstelle mit MD5 Fingerabdruck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (die Seriennummer 35:DE:F4:CF und SHA1 Fingerabdruck D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Dies ist das Zertifikat der Zertifizierungsstelle (CA) [https://api.twilio.com] [ twilio_api_service] Dienst aufgerufen wird, wenn Sie Twilio-APIs verwenden. Informationen zum Hinzufügen dieses Zertifikat zu Ihrem JDK Zertifizierungsstelle Speicher finden Sie unter [Hinzufügen eines Zertifikats zum Java CA Zertifikatspeicher][add_ca_cert].

Darüber hinaus Vertrautheit mit den Informationen zum [Erstellen einer Hello World Anwendung Azure Toolkit für Eclipse][azure_java_eclipse_hello_world], oder mit anderen Techniken zum Java-Anwendung in Azure hosten, wenn Sie Eclipse nicht verwenden, wird dringend empfohlen.

## <a name="create-a-web-form-for-making-a-call"></a>Erstellen eines Webformulars für einen Anruf

Im folgenden Codebeispiel wird eine Web Form zum Abrufen von Daten für einen Anruf erstellen. Für dieses Beispiel wurde ein neue dynamische Webprojekt mit dem Namen **TwilioCloud**und **callform.jsp** als JSP-Datei hinzugefügt wurde.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Erstellen Sie den Code für den Anruf
Folgende Code aufgerufen wird, wenn der Benutzer das Anmeldefenster callform.jsp abgeschlossen hat, die Nachricht erstellt und den Aufruf generiert. Für dieses Beispiel JSP-Datei heißt **makecall.jsp** und das **TwilioCloud** -Projekt hinzugefügt wurde. (Statt Platzhalterwerte zugewiesen **AccountSID** und **AuthToken** im folgenden Code die Twilio und token verwenden).

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";
     
         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");
     
         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");
    
         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");
     
         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");
     
         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;
     
         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");
    
         // Place the call From, To and URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);
     
         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

Neben den Anruf zeigt makecall.jsp Twilio Endpunkt, API-Version und der Status des Anrufs. Der folgenden Abbildung ist ein Beispiel:

![Azure Aufruf Antwort Twilio mit Java][twilio_java_response]

## <a name="run-the-application"></a>Führen Sie die Anwendung
Es folgen die allgemeinen Schritte zum Ausführen der Anwendung; Details für diese Schritte [erstellen eine Hello World Anwendung Azure Toolkit für Eclipse]finden[azure_java_eclipse_hello_world].

1. Exportieren Sie die TwilioCloud WAR in Azure **Approot** Ordner. 
2. Ändern Sie **startup.cmd** zum Extrahieren der TwilioCloud WAR.
3. Kompilieren Sie die Anwendung für Serveremulator.
4. Starten der Bereitstellung im Serveremulator.
5. Öffnen Sie einen Browser, und führen Sie **Http://localhost:8080/TwilioCloud/callform.jsp**.
6. Geben Sie Werte in das Formular, klicken Sie auf **diesen Aufruf**und finden Sie die Ergebnisse in makecall.jsp.

Wenn Sie in Azure Recompile für die Bereitstellung in der Cloud bereitstellen möchten in Azure bereitstellen und Ausführen von http://*Your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp im Browser (ersetzen Sie den Wert für *Your_hosted_name*).

## <a name="next-steps"></a>Nächste Schritte
Dieser Code wurde bereitgestellt, Azure Twilio in Java über Grundfunktionen angezeigt. Vor der Bereitstellung in Azure in der Produktion, sollten Sie weitere Fehlerbehandlung oder andere Funktionen hinzufügen. Zum Beispiel:

* Anstatt ein Webformular können Sie Blobs Azure Storage oder SQL-Datenbank Telefonnummern und Text aufrufen. Informationen zur Verwendung der Azure-Speicher Blobs in Java finden Sie unter [Blob Storage Service von Java verwenden][howto_blob_storage_java]. Informationen über SQL-Datenbank in Java finden Sie unter [Verwenden der SQL-Datenbank in Java][howto_sql_azure_java].
* **RoleEnvironment.getConfigurationSettings** können abrufen Twilio Konto-ID und Authentifizierungstoken von der Bereitstellung Konfigurationseigenschaften, statt Werte in makecall.jsp fest zu codieren. Informationen über die **RoleEnvironment** -Klasse finden Sie unter [Verwendung der Azure-Laufzeitbibliothek JSP] [ azure_runtime_jsp] und der Azure-Laufzeit Paket-Dokumentation unter [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Makecall.jsp-Code weist einen Twilio bereitgestellten URL, [http://twimlets.com/message][twimlet_message_url], **Url** -Variablen. Diese URL beantwortet Twilio Markup Language (TwiML), die Twilio Vorgehensweise mit den Anruf informiert. Die zurückgegebene TwiML kann beispielsweise ein ** &lt;sagen&gt; ** Verb Text gesprochen, des Empfängers führt. Anstatt Twilio bereitgestellte URL konnte einen eigenen Dienst reagiert auf Twilios Anforderung erstellen; Weitere Informationen finden Sie unter [verwenden Twilio für Sprach- und SMS-Funktionen in Java][howto_twilio_voice_sms_java]. Weitere Informationen zu TwiML finden Sie unter [http://www.twilio.com/docs/api/twiml][twiml], und Weitere Informationen zu ** &lt;sagen&gt; ** und andere Twilio Verben finden Sie unter [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Lesen Sie die Sicherheitsrichtlinien Twilio am [https://www.twilio.com/docs/security][twilio_docs_security].

Weitere Informationen über Twilio finden Sie unter [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Siehe auch
* [Verwendung von Twilio für Sprach- und SMS-Funktionen in Java][howto_twilio_voice_sms_java]
* [Hinzufügen eines Zertifikats zum Speicher Java Zertifizierungsstelle Zertifikate][add_ca_cert]

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
