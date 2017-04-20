<properties 
    pageTitle="Wie SendGrid e-Mail-Dienst (.NET) | Microsoft Azure" 
    description="Erfahren Sie, wie senden SendGrid e-Mail-Dienst in Azure. Code-Beispiele in C# und .NET API verwenden." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Azure SendGrid mit E-Mail senden


## <a name="overview"></a>Übersicht

Dieses Handbuch veranschaulicht, wie häufige SendGrid e-Mail-Dienst in Azure Programmieraufgaben. Die Beispiele sind in C# geschrieben\#
und .NET API verwenden. Die Szenarios umfassen **Erstellen e-Mail**, **e-Mail**, **Anhänge hinzufügen**und **Verwenden von Filtern**. Weitere Informationen zu SendGrid und Senden von e-Mails finden Sie im Abschnitt [Weiter][] .

## <a name="what-is-the-sendgrid-email-service"></a>Was ist die SendGrid e-Mail-Service?

SendGrid ist ein [Cloud-basierten e-Mail-Dienst] zuverlässige [Transaktions-e-Mail-Zustellung], Skalierbarkeits- und Echtzeit-Analysen mit flexiblen APIs, die benutzerdefinierten Integration erleichtern. Allgemeine SendGrid Szenarien umfassen:

-   Senden von Empfangsbestätigungen automatisch an Kunden.
-   Verwalten der Verteilung Listet für Kunden monatliche e-Flyer und Sonderangebote.
-   Sammeln von Echtzeit-Metriken blockierten e-Mails und Kundenbetreuung.
-   Generieren von Berichten zu Trends erkennen.
-   Weiterleiten von Anfragen.
-   Verarbeitung eingehender e-Mails.

Weitere Informationen finden Sie unter [https://sendgrid.com](https://sendgrid.com) oder unsere [C# Bibliothek][Sendgrid csharp]

## <a name="create-a-sendgrid-account"></a>Erstellen eines Kontos SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>SendGrid .NET Klassenbibliothek verweisen

[SendGrid NuGet-Paket](https://www.nuget.org/packages/Sendgrid) ist am einfachsten zu SendGrid-API und die Anwendung mit allen Abhängigkeiten konfigurieren. NuGet ist eine Visual Studio-Erweiterung mit Microsoft Visual Studio 2015, der leicht installieren und Aktualisieren von Bibliotheken und Tools enthalten. 

> [AZURE.NOTE] Installieren von NuGet zur Ausführen eine Visual Studio-Version vor Visual Studio 2015 finden Sie auf [http://www.nuget.org](http://www.nuget.org), und klicken Sie auf **Installieren von NuGet** .

Gehen Sie wie folgt vor, um in Ihrer Anwendung SendGrid NuGet-Paket installieren:

1.  Erstellt ein neues Projekt.

    ![Erstellen eines neuen Projekts][create-new-project]

2.  Wählen Sie eine Vorlage aus.

    ![Wählen Sie eine Vorlage][select-a-template]

3.  Im **Projektmappen-Explorer**mit der rechten Maustaste **Verweise**und dann auf **NuGet-Pakete verwalten**.

4.  Suchen Sie **SendGrid** , und wählen Sie das **SendGrid** -Element in der Ergebnisliste.

    ![SendGrid NuGet-Paket][SendGrid-NuGet-package]

5.  Klicken Sie auf **Installieren** , um die Installation abzuschließen, und schließen Sie dieses Dialogfeld.

SendGrid des .NET Klassenbibliothek wird **SendGridMail**aufgerufen. Es enthält die folgenden Namespaces:

-   **SendGridMail** zum Erstellen und Arbeiten mit e-Mail-Elementen.
-   **SendGridMail.Transport** für e-Mail, **Web-REST**mit dem **SMTP-** Protokoll oder das Protokoll HTTP 1.1.

Fügen Sie die folgenden Code-Namespacedeklarationen oben alle c\# Datei in der SendGrid e-Mail-Dienst programmgesteuert zugreifen soll.
**System.Net** und **System.Net.Mail** sind.NET Framework-Namespaces enthalten sind, da sie Typen enthalten häufig SendGrid APIs verwendet.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Gewusst wie: erstellen eine e-Mail

Verwenden Sie das **SendGridMessage** -Objekt erstellen eine e-Mail-Nachricht. Nachdem das Nachrichtenobjekt erstellt wurde, können Sie Eigenschaften und Methoden, einschließlich e-Mail-Absender der e-Mail-Empfänger und Betreff und Text der e-Mail festlegen.

Im folgenden Beispiel wird veranschaulicht, wie eine vollständig aufgefüllte Email-Objekt erstellen:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Weitere Informationen über alle Eigenschaften und Methoden von der **SendGrid** unterstützt finden Sie auf GitHub [Sendgrid Csharp][] .

## <a name="how-to-send-an-email"></a>Gewusst wie: senden eine e-Mail

Nach dem Erstellen einer e-Mail-Nachricht, können Sie mithilfe der Web-API bereitgestellten SendGrid senden. Alternativ können Sie [verwenden. NET Bibliothek integrierte](https://sendgrid.com/docs/Code_Examples/csharp.html).

E-Mail muss die SendGrid-Anmeldeinformationen (Benutzername und Kennwort) oder SendGrid API-Schlüssel angeben. API-Schlüssel ist die bevorzugte Methode. Benötigen Sie Informationen zur API-Schlüssel konfigurieren, finden Sie auf unserer [Dokumentation](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Sie können diese Anmeldeinformationen Portal Azure speichern auf Konfigurieren und die Schlüssel-Wert-Paare unter "app Settings" hinzufügen.

 ![Einstellung der Azure-Anwendung][azure_app_settings]

 Anschließend können Sie diese wie folgt zugreifen: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Verwenden von Anmeldeinformationen:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Verwenden von API-Schlüssel:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Die folgenden Beispiele zeigen, wie zum Senden einer Nachricht mithilfe der Web-API.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Gewusst wie: Hinzufügen einer Anlage

Anlagen können zu einer Nachricht hinzugefügt werden, durch die **AddAttachment** -Methode aufrufen und den Namen und Pfad der Datei, die Sie anfügen möchten.
Sind mehrere Anlagen durch Aufrufen dieser Methode, wenn Sie jede Datei anfügen möchten. Das folgende Beispiel veranschaulicht das Hinzufügen einer Anlage zu einer Nachricht:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Sie können auch Anlagen aus dem Data **Stream**hinzufügen. Möglich aufrufen wie oben **AddAttachment**, sondern den Datenstrom übergeben und der Dateiname soll in der Nachricht angezeigt. In diesem Fall müssen Sie die System.IO-Bibliothek hinzufügen.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Gewusst wie: Verwenden von apps Fußzeilen, Überwachung und Analyse

SendGrid bietet zusätzliche e-Mail-Funktionen apps. Einstellungen, die eine e-Mail-Nachricht bestimmte Funktionen auf verfolgen, Google Analytics tracking Abonnement aktivieren hinzugefügt werden können, und so weiter. Eine vollständige Liste der apps finden Sie unter [App-Einstellungen][].

Apps können **SendGrid** e-Mail-Nachrichten als Teil der **SendGrid** Klasse Methoden angewendet werden.

Die folgenden Beispiele veranschaulichen die Fußzeile und auf Filter nachverfolgen:

### <a name="footer"></a>Fußzeile

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Klicken Sie auf Überwachung

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Gewusst wie: Weitere SendGrid Dienstleistungen

SendGrid bietet Web-basierte APIs und Webhooks, mit denen Sie weitere SendGrid Funktionen von Azure-Anwendung nutzen. Einzelheiten finden Sie in der [Dokumentation zur SendGrid-API][].

## <a name="next-steps"></a>Nächste Schritte

Grundlagen von SendGrid e-Mail-Dienst bearbeitet haben, folgen Sie diesen Links erfahren Sie mehr.

*   SendGrid C\# Bibliothek Repo: [Csharp Sendgrid][]
*   SendGrid API-Dokumentation: <https://sendgrid.com/docs>
*   Sonderangebot für Azure Kunden SendGrid: [https://sendgrid.com](https://sendgrid.com)

  [Nächste Schritte]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [Sendgrid csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [App-Einstellungen]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [SendGrid API-Dokumentation]: https://sendgrid.com/docs
  
  [Cloud-basierten e-Mail-Dienst]: https://sendgrid.com/email-solutions
  [transaktionale e-Mail-Zustellung]: https://sendgrid.com/transactional-email
 
