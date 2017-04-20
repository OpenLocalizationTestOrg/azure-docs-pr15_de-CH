<properties 
    pageTitle="Zu einem Anruf von Twilio (.NET) | Microsoft Azure" 
    description="Informationen Sie zum Anrufen und senden eine SMS mit der Twilio-API in Azure. Beispiele in .NET." 
    services="" 
    documentationCenter=".net" 
    authors="devinrader" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="05/04/2016" 
    ms.author="microsofthelp@twilio.com"/>




# <a name="how-to-make-a-phone-call-using-twilio-in-a-web-role-on-azure"></a>Wie mit Twilio in einer Webrolle Azure anrufen

Dieses Handbuch veranschaulicht, wie Twilio von einer Webseite in Azure aufzurufen. Die resultierende Anwendung fordert den Benutzer zur Telefonanruf Werte, wie im folgenden Screenshot gezeigt.

![Azure Aufruf Formular Twilio mit ASP.NET][twilio_dotnet_basic_form]

## <a name="twilio-prereqs"></a>Erforderliche Komponenten

Sie müssen den Code in diesem Thema verwenden folgendermaßen:

1. Erwerben Sie eine Twilio und token. Um mit Twilio beginnen, melden Sie sich an [https://www.twilio.com/try-twilio][try_twilio]. Sie können [http://www.twilio.com/pricing]Preise[twilio_pricing]. Informationen über die API von Twilio finden Sie unter [http://www.twilio.com/voice/api][twilio_api].
2. Die Web-Rolle Twilio .NET Bibliothek hinzufügen. Siehe "Twilio-Bibliotheken Ihrem Webprojekt Rolle hinzufügen" weiter unten in diesem Thema.

Sie sein erstellen eine einfache Webrolle in Azure vertraut.

## <a name="howtocreateform"></a>Gewusst wie: Erstellen eines Webformulars für einen Anruf

<a id="use_nuget"></a>Rolle Webprojekt Twilio-Bibliotheken hinzu:

1.  Öffnen Sie die Projektmappe in Visual Studio.
2.  Klicken Sie auf **Referenzen**.
3.  Klicken Sie auf **NuGet-Pakete verwalten**.
4.  Klicken Sie auf **Online**.
5.  Geben Sie im Suchfeld online *Twilio*.
6.  Klicken Sie auf das Twilio-Paket **Installieren** .

Im folgenden Codebeispiel wird eine Web Form zum Abrufen von Daten für einen Anruf erstellen. In diesem Beispiel wird eine ASP.NET Web-Rolle mit dem Namen **TwilioCloud** erstellt.

    <%@ Page Title="Home Page" Language="C#" MasterPageFile="~/Site.master"
        AutoEventWireup="true" CodeBehind="Default.aspx.cs"
        Inherits="WebRole1._Default" %>

    <asp:Content ID="HeaderContent" runat="server" ContentPlaceHolderID="HeadContent">
    </asp:Content>
    <asp:Content ID="BodyContent" runat="server" ContentPlaceHolderID="MainContent">
        <div>
            <asp:BulletedList ID="varDisplay" runat="server" BulletStyle="NotSet">
            </asp:BulletedList>
        </div>
        <div>
            <p>Fill in all fields and click <b>Make this call</b>.</p>
            <div>
                To:<br /><asp:TextBox ID="toNumber" runat="server" /><br /><br />
                Message:<br /><asp:TextBox ID="message" runat="server" /><br /><br />
                <asp:Button ID="callpage" runat="server" Text="Make this call"
                    onclick="callpage_Click" />
            </div>
        </div>
    </asp:Content>

## <a id="howtocreatecode"></a>Gewusst wie: Erstellen Sie den Code für den Anruf
Folgende Code aufgerufen wird, wenn der Benutzer das Formular schließt die Nachricht erstellt und den Aufruf generiert. In diesem Beispiel wird der Code in den Onclick-Ereignishandler der Schaltfläche auf dem Formular ausgeführt. (Statt Platzhalterwerte zugewiesen **AccountSID** und **AuthToken** im folgenden Code die Twilio und token verwenden).

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;
    using Twilio;

    namespace WebRole1
    {
        public partial class _Default : System.Web.UI.Page
        {
            protected void Page_Load(object sender, EventArgs e)
            {

            }

            protected void callpage_Click(object sender, EventArgs e)
            {
                // Call porcessing happens here.

                // Use your account SID and authentication token instead of
                // the placeholders shown here.
                string accountSID = "ACNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";
                string authToken =  "NNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN";

                // Instantiate an instance of the Twilio client.
                TwilioRestClient client;
                client = new TwilioRestClient(accountSID, authToken);

                // Retrieve the account, used later to retrieve the
                Twilio.Account account = client.GetAccount();
                string APIversuion = client.ApiVersion;
                string TwilioBaseURL = client.BaseUrl;

                this.varDisplay.Items.Clear();
                if (this.toNumber.Text == "" || this.message.Text == "")
                {
                    this.varDisplay.Items.Add(
                            "You must enter a phone number and a message.");
                }
                else
                {
                    // Retrieve the values entered by the user.
                    string to = this.toNumber.Text;
                    string myMessage = this.message.Text;

                    // Create a URL using the Twilio message and the user-entered
                    // text. You must replace spaces in the user's text with '%20'
                    // to make the text suitable for a URL.
                    String Url = "http://twimlets.com/message?Message%5B0%5D="
                            + myMessage.Replace(" ", "%20");

                    // Display the endpoint, API version, and the URL for the message.
                    this.varDisplay.Items.Add("Using Twilio endpoint "
                        + TwilioBaseURL);
                    this.varDisplay.Items.Add("Twilioclient API Version is "
                        + APIversuion);
                    this.varDisplay.Items.Add("The URL is " + Url);

                    // Instantiate the call options that are passed
                    // to the outbound call.
                    CallOptions options = new CallOptions();

                    // Set the call From, To, and URL values.                    
                    options.From = "+14155992671";
                    options.To = to;
                    options.Url = Url;

                    // Place the call.
                    var call = client.InitiateOutboundCall(options);
                    this.varDisplay.Items.Add("Call status: " + call.Status);
                }
            }
        }
    }

Der Aufruf erfolgt und Twilio Endpunkt, API-Version und der Status des Anrufs angezeigt. Der folgende Screenshot zeigt die Ausgabe vom Beispiel ausführen.

![Azure Aufruf Antwort Twilio mit ASP.NET][twilio_dotnet_basic_form_output]

Weitere Informationen zu TwiML finden Sie unter [http://www.twilio.com/docs/api/twiml][twiml]. Weitere Informationen zu &lt;Say&gt; und andere Twilio Verben finden Sie unter [http://www.twilio.com/docs/api/twiml/say][twilio_say].

## <a id="nextsteps"></a>Nächste Schritte
Dieser Code wurde bereitgestellt, Twilio einer ASP.NET Web-Rolle in Azure verwenden grundlegende Funktionen angezeigt. Vor der Bereitstellung in Azure in der Produktion, sollten Sie weitere Fehlerbehandlung oder andere Funktionen hinzufügen. Zum Beispiel:

* Anstatt ein Webformular können Sie Azure BLOB-Speicher oder einer Azure SQL-Datenbankinstanz Telefonnummern und Text aufrufen. Informationen zur Verwendung von Blobs in Azure finden Sie unter [Azure BLOB-Speicherdienst in .NET verwenden][howto_blob_storage_dotnet]. Informationen zur Verwendung von SQL-Datenbank finden Sie unter [Azure SQL-Datenbank in .NET Applications][howto_sql_azure_dotnet].
* RoleEnvironment.getConfigurationSettings können abrufen Twilio Konto-ID und Authentifizierungstoken von der Bereitstellung Konfigurationseigenschaften statt Programmieren die Werte im Formular. Informationen über die RoleEnvironment-Klasse finden Sie unter [Microsoft.WindowsAzure.ServiceRuntime-Namespace][azure_runtime_ref_dotnet].
* Lesen Sie die Sicherheitsrichtlinien Twilio am [https://www.twilio.com/docs/security][twilio_docs_security].
* Erfahren Sie mehr über Twilio an [https://www.twilio.com/docs][twilio_docs].

##<a name="seealso"></a>Siehe auch
* [Verwendung von Twilio für Sprach- und SMS-Funktionen von Azure](twilio-dotnet-how-to-use-for-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/voice/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#

[twilio_dotnet_basic_form]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form.png
[twilio_dotnet_basic_form_output]: ./media/partner-twilio-cloud-services-dotnet-phone-call-web-role/WA_twilio_dotnet_basic_form_output.png

[twiml]: http://www.twilio.com/docs/api/twiml



[howto_twilio_voice_sms_dotnet]: /develop/net/how-to-guides/twilio/

[howto_blob_storage_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/

[howto_sql_azure_dotnet]: https://www.windowsazure.com/develop/net/how-to-guides/sql-database/


[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say


[azure_runtime_ref_dotnet]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.aspx
