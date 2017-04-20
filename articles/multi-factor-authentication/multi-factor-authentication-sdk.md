<properties 
    pageTitle="Integration von lokalen Identitäten in Azure Active Directory."
    description="Dies ist der Azure AD verbinden, der beschreibt, was und warum verwendet werden würde."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="building-multi-factor-authentication-into-custom-apps-sdk"></a>Mehrstufige Authentifizierung benutzerdefinierte Apps (SDK) erstellen

> [AZURE.IMPORTANT]  Wenn Sie das SDK herunterladen möchten, müssen Sie eine Azure Mehrfaktor-Authentifizierungsanbieter erstellen, selbst wenn Azure MFA AAD Premium oder EMS-Lizenzen haben.  Ein Azure Mehrfaktor-Authentifizierungsanbieter für diesen Zweck erstellen und bereits Lizenzen ist den Anbieter mit dem Modell **Pro aktivierten Benutzer** erstellen und Verknüpfen des Anbieters in das Verzeichnis von Azure MFA, Azure AD Premium oder EMS-Lizenzen erforderlich.  Dadurch wird sichergestellt, dass Sie nicht berechnet werden, wenn mehrere eindeutige Benutzer mithilfe des SDK als die Anzahl der Lizenzen, die Sie besitzen.

Azure mehrstufige Authentifizierung Software Development Kit (SDK) können Sie Telefonanruf Text Nachricht Überprüfung und direkt in die Prozesse anmelden oder Transaktion in Ihrem Mandanten Azure AD-Anwendung erstellen.

Mehrstufige Authentifizierung SDK steht für C#, Visual Basic (.NET) Java, Perl, PHP und Ruby. Das SDK bietet einen einfachen Wrapper für mehrstufige Authentifizierung. Sie enthält alle Code einschließlich kommentierte Quellcodedateien, Beispieldateien und ausführlichen ReadMe-Datei zu schreiben. Jede SDK enthält auch ein Zertifikat und den privaten Schlüssel zum Verschlüsseln von Transaktionen für die kombinierte Authentifizierungsanbieter eindeutig ist. Als einen Anbieter haben, können Sie SDK in wie vielen Sprachen und Formate laden.

Die Struktur der APIs mehrstufige Authentifizierung SDK ist einfach. Sie stellen eine Funktion aufrufen einer API mit Parametern mehrstufige Option Prüfmodus und Benutzerdaten, die Telefonnummer oder die PIN-Nummer überprüft. Die APIs übersetzen Funktionsaufruf in Web Services Anfragen für Cloud-basierte Azure Multi-Factor Authentication Service. Alle müssen einen Verweis auf das private Zertifikat enthalten, die in jedem SDK enthalten ist.

Da die APIs nicht in Azure Active Directory registrierten Benutzern verfügen, müssen Sie Benutzerinformationen wie Telefonnummern und PIN-Codes in einer Datei oder Datenbank bereitstellen. Außerdem bieten APIs keine Registrierung oder Benutzer Verwaltungsfunktionen, müssen Sie diese Prozesse in der Anwendung erstellen.






## <a name="download-the-azure-multi-factor-authentication-sdk"></a>Azure mehrstufige Authentifizierung SDK herunterladen

Mehrstufige Azure SDK herunterladen muss ein [Azure Mehrfaktor-Authentifizierungsanbieter](multi-factor-authentication-get-started-auth-provider.md).  Dies erfordert einen vollständigen Azure-Abonnement auch wenn Azure MFA, Azure AD Premium oder Enterprise Mobility Suite-Lizenzen sind.  Downloaden des SDK müssen Sie mehrstufige Management-Portal durch kombinierte Authentifizierungsanbieter direkt verwalten oder über den Link **"Zum Portal"** auf der Seite Einstellungen MFA navigieren.


### <a name="to-download-the-azure-multi-factor-authentication-sdk-from-the-azure-portal"></a>Azure mehrstufige Authentifizierung SDK von Azure-Portal herunterladen


1. Azure-Portal als Administrator anmelden.
2. Wählen Sie auf der linken Seite Active Directory.
3. Klicken Sie auf der Seite Active Directory oben **Mehrstufige Authentifizierung Anbieter**
4. Klicken Sie unten auf **Verwalten**
5. Dadurch wird eine neue Seite geöffnet.  Klicken Sie links unten auf SDK.
<center>![Herunterladen](./media/multi-factor-authentication-sdk/download.png)</center>
6. Wählen Sie die Sprache aus, die und klicken Sie auf die zugehörigen Downloadlinks.
7. Speichern Sie den Download.



### <a name="to-download-the-azure-multi-factor-authentication-sdk-via-the-service-settings"></a>Azure mehrstufige Authentifizierung SDK über die Einstellungen herunterladen


1. Azure-Portal als Administrator anmelden.
2. Wählen Sie auf der linken Seite Active Directory.
3. Doppelklicken Sie auf die Instanz von Azure AD.
4. Klicken Sie oben auf **Konfigurieren**
5. Wählen Sie unter mehrstufige Authentifizierung **Verwalten Einstellungen**
![herunterladen](./media/multi-factor-authentication-sdk/download2.png)
6. Auf der Seite Services Settings am unteren Bildschirmrand klicken Sie auf **das Portal**.
![Herunterladen](./media/multi-factor-authentication-sdk/download3a.png)
7. Dadurch wird eine neue Seite geöffnet.  Klicken Sie links unten auf SDK.
8. Wählen Sie die Sprache aus, die und klicken Sie auf die zugehörigen Downloadlinks.
9. Speichern Sie den Download.

## <a name="contents-of-the-azure-multi-factor-authentication-sdk"></a>Inhalt der Azure mehrstufige Authentifizierung SDK
Im SDK finden Sie folgende Elemente:

- **Readme-Datei**. Erläutert, wie die kombinierte Authentifizierung APIs in einer neuen oder vorhandenen Anwendung.
- **Quelldateien** für die kombinierte Authentifizierung
- **Client-Zertifikat** mit Multi-Factor Authentication Service kommunizieren
- **Privaten Schlüssel** für das Zertifikat
- **Rufen Sie die Ergebnisse.** Eine Liste der Aufruf Ergebniscodes. Um diese Datei zu öffnen, verwenden Sie eine Anwendung mit, wie WordPad. Verwenden Sie Ergebniscodes Aufruf zu testen und die Implementierung der kombinierten Authentifizierung in der Anwendung zu beheben. Sie sind nicht Authentifizierung Statuscodes.
- **Beispiele.** Beispielcode für eine einfache arbeiten Implementierung der kombinierten Authentifizierung.


>[AZURE.WARNING]Das Clientzertifikat ist ein eindeutiger privates Zertifikat, das für Sie generiert wurde. Freigeben Sie oder verlieren Sie diese Datei nicht. Es ist der Schlüssel zur Gewährleistung der Kommunikation mit der kombinierten Authentifizierung.

## <a name="code-sample-standard-mode-phone-verification"></a>Beispiel: Standard-Modus per Telefon verifizieren

Dieses Codebeispiel veranschaulicht, wie mit den APIs in Azure mehrstufige Authentifizierung SDK Standardmodus Voice Call Überprüfung der Anwendung hinzufügen. Standard-Modus ist ein Telefongespräch, die der Benutzer auf, die #-Taste drücken.

Dieses Beispiel verwendet C# .NET 2.0 mehrstufige Authentifizierung SDK in einer einfachen ASP.NET Anwendung mit C# serverseitige Logik, aber der Prozess ähnelt für einfache Implementierung in anderen Sprachen. Da das SDK Quelldateien nicht ausführbare Dateien enthält können Dateien erstellen und darauf verweisen oder direkt in Ihre Anwendung einbinden.

>[AZURE.NOTE]Wenn mehrstufige Authentifizierung implementieren, verwenden Sie zusätzlichen Faktoren als sekundären oder tertiären Überprüfung primäre Authentifizierungsmethode ergänzen. Diese Methoden dienen nicht als primäre Authentifizierungsmethoden verwendet werden.

### <a name="code-sample-overview"></a>Übersicht über Code-Beispiel
Dieser Beispielcode für eine einfache demoanwendung vervollständigt einen Telefonanruf mit Schlüssel # Authentifizierung des Benutzers. Dieser Anruf Faktor wird als Standardmodus Mehrfaktor-Authentifizierung bezeichnet.

Clientseitiger Code enthält keine mehrstufige Authentifizierung-spezifische Elemente. Da die zusätzliche Authentifizierungsstufen von der Authentifizierung unabhängig sind, können Sie diese hinzufügen, ohne die vorhandene Schnittstelle anmelden. APIs im kombinierten SDK können Sie die Benutzeroberfläche anpassen, aber Sie müssen nicht alles ändern.

Der serverseitige Code fügt Standard-Authentifizierungsmodus in Schritt2. Erstellt ein PfAuthParams-Objekt mit den Parametern, die für Standard-Modus Überprüfung: Benutzername, Telefonnummer, und Modus und den Pfad der Clientzertifikat (CertFilePath), der bei jedem Aufruf erforderlich ist. Eine Demonstration aller Parameter in PfAuthParams finden Sie in der Beispieldatei im SDK.

Anschließend wird im Code das PfAuthParams-Objekt an die pf_authenticate()-Funktion. Der Rückgabewert gibt den Erfolg oder Misserfolg der Authentifizierung. Die Out-Parameter CallStatus und ErrorID enthalten zusätzliche Aufruf Ergebnis. Aufruf-Ergebniscodes sind in der Ergebnisdatei Aufruf im SDK dokumentiert.

Diese minimale Implementierung kann in wenigen Zeilen geschrieben werden. Allerdings in Produktionscode würde komplexere Fehlerbehandlung Code zusätzliche und verbesserte enthalten.

### <a name="web-client-code"></a>Web-Client-Code

Folgendes ist Web Client-Code für eine Testseite.


    <%@ Page Language="C#" AutoEventWireup="true" CodeFile="Default.aspx.cs" Inherits="_Default" %>

    <!DOCTYPE html>

    <html xmlns="http://www.w3.org/1999/xhtml">
    <head runat="server">
    <title>Multi-Factor Authentication Demo</title>
    </head>
    <body>
    <h1>Azure Multi-Factor Authentication Demo</h1>
    <form id="form1" runat="server">

    <div style="width:auto; float:left">
    Username:&nbsp;<br />
    Password:&nbsp;<br />
    </div>

    <div">
    <asp:TextBox id="username" runat="server" width="100px"/><br />
    <asp:Textbox id="password" runat="server" width="100px" TextMode="password" /><br />
    </div>

    <asp:Button id="btnSubmit" runat="server" Text="Log in" onClick="btnSubmit_Click"/>

    <p><asp:Label ID="lblResult" runat="server"></asp:Label></p>

    </form>
    </body>
    </html>


### <a name="server-side-code"></a>Serverseitigen Code

Im folgenden serverseitigen Code mehrstufige Authentifizierung konfiguriert und in Schritt2 ausgeführt. Standardmodus (MODE_STANDARD) ist ein Anruf, der Benutzer durch Drücken der Taste # reagiert.

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.Web.UI;
    using System.Web.UI.WebControls;

    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {
        }

        protected void btnSubmit_Click(object sender, EventArgs e)
        {
            // Step 1: Validate the username and password
            if (username.Text != "Contoso" || password.Text != "password")
            {
                lblResult.ForeColor = System.Drawing.Color.Red;
                lblResult.Text = "Username or password incorrect.";
            }
            else
            {
                // Step 2: Perform multi-factor authentication

                // Add call details from the user database.
                PfAuthParams pfAuthParams = new PfAuthParams();
                pfAuthParams.Username = username.Text;
                pfAuthParams.Phone = "9134884271";
                pfAuthParams.Mode = pf_auth.MODE_STANDARD;

                // Specify a client certificate
                // NOTE: This file contains the private key for the client
                // certificate. It must be stored with appropriate file
                // permissions.
                pfAuthParams.CertFilePath = "c:\\cert_key.p12";

                // Perform phone-based authentication
                int callStatus;
                int errorId;

                if(pf_auth.pf_authenticate(pfAuthParams, out callStatus, out errorId))
                {
                    lblResult.ForeColor = System.Drawing.Color.Green;
                    lblResult.Text = "Multi-Factor Authentication succeeded.";
                }
                else
                {
                    lblResult.ForeColor = System.Drawing.Color.Red;
                    lblResult.Text = " Multi-Factor Authentication failed.";
                }
            }

        }
    }
