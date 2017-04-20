<properties 
    pageTitle="Store-sendgrid-Java-How-to-Send-Email-example" 
    description="SendGrid von Java in Azure-Bereitstellung mit e-Mail senden" 
    services="" 
    documentationCenter="java" 
    authors="thinkingserious" 
    manager="sendgrid" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com"/>

# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a>SendGrid von Java in Azure-Bereitstellung mit E-Mail senden

Das folgende Beispiel veranschaulicht die Verwendung SendGrid, e-Mails von einer Webseite in Azure senden. Die resultierende Anwendung fordert den Benutzer für e-Mail-Werte, wie im folgenden Screenshot gezeigt.

![E-Mail-Formular][emailform]

Die resultierende e-Mail wird folgende Bildschirmdarstellung ähneln.

![E-Mail-Nachricht][emailsent]

Sie müssen den Code in diesem Thema verwenden folgendermaßen:

1. Beispielsweise erhalten Sie Gläser javax.mail von <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Die Java-Erstellungspfad Gläser hinzufügen.
3. Verwenden Sie Eclipse diese Java-Anwendung erstellen, ist Bereitstellung Anwendungsdatei (KRIEG) mit Eclipse Bereitstellung Baugruppen-KE gehören die SendGrid-Bibliotheken. Verwenden Sie keine Eclipse diese Java-Anwendung erstellen, gewährleisten Sie die Bibliotheken innerhalb der gleichen Azure-Rolle wie die Java-Anwendung und den Pfad der Anwendung hinzugefügt.


Sie müssen auch eine eigene SendGrid Benutzername und Kennwort zu senden. Zunächst mit SendGrid finden Sie unter [SendGrid von Java über e-Mail](store-sendgrid-java-how-to-send-email.md).

Vertrautheit mit den Informationen zum [Erstellen einer Hello World-Anwendung für Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944)oder mit anderen Techniken zum Java-Anwendung in Azure hosten, verwenden Sie keine Eclipse wird außerdem dringend empfohlen.

## <a name="create-a-web-form-for-sending-email"></a>Erstellen eines Webformulars für e-Mail

Im folgenden Codebeispiel wird eine Web Form zum Abrufen von Daten für e-Mail erstellen. Für die Zwecke dieses Inhalts heißt JSP-Datei **emailform.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-the-code-to-send-the-email"></a>Erstellen Sie Code zum Senden der e-Mail

Der folgende Code beim Ausfüllen des Formulars im emailform.jsp wird die e-Mail-Nachricht erstellt und sendet diese. Für die Zwecke dieses Inhalts heißt JSP-Datei **sendemail.jsp**.

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%
     
     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");
     
     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;
          
            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {
         
         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";
    
         Properties properties;
        
         properties = new Properties();
         
         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");
         
         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");
    
         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();
         
         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);
         
         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);
    
         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 
         
         message = new MimeMessage(mailSession);
         
         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            
    
         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);
         
         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");
    
         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");
         
         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
         transport.close();
     
        out.println("<p>Email processing completed.</p>");
         
     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>
    
    </body>
    </html>

Neben der e-Mail bietet emailform.jsp ein Ergebnis für den Benutzer. der folgenden Abbildung ist ein Beispiel:

![Senden von e-Mail-Ergebnis][emailresult]

## <a name="next-steps"></a>Nächste Schritte

Bereitstellen der Anwendung auf Serveremulator innerhalb eines Browsers ausgeführt emailform.jsp, geben Sie Werte in das Formular, klicken Sie auf **diese E-Mail**und Ergebnisse im sendemail.jsp angezeigt.

Dieser Code wurde angezeigt wie Sie SendGrid in Java auf Azure bereitgestellt. Vor der Bereitstellung in Azure in der Produktion, sollten Sie weitere Fehlerbehandlung oder andere Funktionen hinzufügen. Zum Beispiel: 

* Blobs Azure Storage oder SQL-Datenbank können Sie e-Mail-Adressen und e-Mail-Nachrichten statt mit einem Web Form gespeichert. Informationen über Azure-Speicher Blobs in Java finden Sie unter [Verwendung der Speicherdienst Blob von Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Informationen über SQL-Datenbank in Java finden Sie unter [Java SQL Datenbank verwenden](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Sie können `RoleEnvironment.getConfigurationSettings` SendGrid Benutzername und Kennwort aus der Bereitstellung Konfigurationseigenschaften, anstatt das Webformular zum Abrufen dieser Werte abrufen. Informationen zu den `RoleEnvironment` Klasse, [mit der Azure-Laufzeitbibliothek JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) und Azure-Laufzeit Paket-Dokumentation unter <http://dl.windowsazure.com/javadoc>.
* Weitere Informationen über SendGrid in Java finden Sie unter [SendGrid von Java über e-Mail](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
