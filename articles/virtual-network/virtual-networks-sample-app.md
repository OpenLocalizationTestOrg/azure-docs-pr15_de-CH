<properties
   pageTitle="Beispiel für die Verwendung mit Sicherheit Grenze | Microsoft Azure"
   description="Bereitstellen Sie diese einfache Web Application nach dem Erstellen einer DMZ Datenverkehr Fluss Szenarien testen"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor"/>

# <a name="sample-application-for-use-with-security-boundary-environments"></a>Beispielanwendung für die Verwendung mit Sicherheit Grenze

[Sicherheit Grenze Best Practices Seite zurück][HOME]

Diese PowerShell-Skripts können auf den IIS01 und AppVM01 zu setup eine sehr einfache Anwendung, die HTML-Seiten, von front-End IIS01 Server Inhalte AppVM01 Datenbankserver lokal ausgeführt werden.

Diese app wird bietet eine einfache Tests Umgebung für viele der DMZ und ändert Endpunkte, NSGs, UDR und Firewall Regeln Verkehrswerte auswirken können.

## <a name="firewall-rule-to-allow-icmp"></a>Firewallregel ICMP zulassen
Diese einfache Aussage PowerShell läuft auf jeder Windows-VM ICMP (Ping) ermöglichen. Dadurch leichter testen und Problembehandlung ermöglicht das Ping-Protokoll durch die Windows-Firewall (für die meisten Linux-Distributionen, die ICMP ist standardmäßig aktiviert).

    # Turn On ICMPv4
    New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" `
        -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow

**Hinweis:** Verwenden Sie die folgenden Skripts dieser Firewall Regel ist die erste Anweisung.

## <a name="iis01---web-application-installation-script"></a>Iis01 - Installationsskript für Web-Anwendung
Dieses Skript wird:

1.  Öffnen Sie IMCPv4 (Ping) auf dem lokalen Server Windows Firewall für einfacheres testen
2.  Installieren Sie IIS und .net Framework 4.5
3.  Erstellen einer ASP.NET Webseite und eine Web.config-Datei
4.  Ändern der Standardanwendungspool Dateizugriff erleichtern
5.  Anonymen Benutzer auf Ihr Konto und Kennwort festgelegt

PowerShell-Skript muss lokal ausgeführt werden, während RDP IIS01 hatte.

    # IIS Server Post Build Config Script
    # Get Admin Account and Password
        Write-Host "Please enter the admin account information used to create this VM:" -ForegroundColor Cyan
        $theAdmin = Read-Host -Prompt "The Admin Account Name (no domain or machine name)"
        $thePassword = Read-Host -Prompt "The Admin Password"
        
    # Turn On ICMPv4
        Write-Host "Creating ICMP Rule in Windows Firewall" -ForegroundColor Cyan
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
        
    # Install IIS
        Write-Host "Installing IIS and .Net 4.5, this can take some time, like 15+ minutes..." -ForegroundColor Cyan
        add-windowsfeature Web-Server, Web-WebServer, Web-Common-Http, Web-Default-Doc, Web-Dir-Browsing, Web-Http-Errors, Web-Static-Content, Web-Health, Web-Http-Logging, Web-Performance, Web-Stat-Compression, Web-Security, Web-Filtering, Web-App-Dev, Web-ISAPI-Ext, Web-ISAPI-Filter, Web-Net-Ext, Web-Net-Ext45, Web-Asp-Net45, Web-Mgmt-Tools, Web-Mgmt-Console
        
    # Create Web App Pages
        Write-Host "Creating Web page and Web.Config file" -ForegroundColor Cyan
        $MainPage = '<%@ Page Language="vb" AutoEventWireup="false" %>
        <%@ Import Namespace="System.IO" %>
        <script language="vb" runat="server">
            Protected Sub Page_Load(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Load
                Dim FILENAME As String = "\\10.0.2.5\WebShare\Rand.txt"
                Dim objStreamReader As StreamReader
                objStreamReader = File.OpenText(FILENAME)
                Dim contents As String = objStreamReader.ReadToEnd()
                lblOutput.Text = contents
                objStreamReader.Close()
                lblTime.Text = Now()
            End Sub
        </script>
            
        <!DOCTYPE html>
        <html xmlns="http://www.w3.org/1999/xhtml">
        <head runat="server">
            <title>DMZ Example App</title>
        </head>
        <body style="font-family: Optima,Segoe,Segoe UI,Candara,Calibri,Arial,sans-serif;">
          <form id="frmMain" runat="server">
            <div>
              <h1>Looks like you made it!</h1>
              This is a page from the inside (a web server on a private network),<br />
              and it is making its way to the outside! (If you are viewing this from the internet)<br />
              <br />
              The following sections show:
              <ul style="margin-top: 0px;">
                <li> Local Server Time - Shows if this page is or isnt cached anywhere</li>
                <li> File Output - Shows that the web server is reaching AppVM01 on the backend subnet and successfully returning content</li>
                <li> Image from the Internet - Doesnt really show anything, but it made me happy to see this when the app worked</li>
              </ul>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Local Web Server Time</b>: <asp:Label runat="server" ID="lblTime" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>File Output from AppVM01</b>: <asp:Label runat="server" ID="lblOutput" /></div>
              <div style="border: 2px solid #8AC007; border-radius: 25px; padding: 20px; margin: 10px; width: 650px;">
                <b>Image File Linked from the Internet</b>:<br />
                <br />
                <img src="http://sd.keepcalm-o-matic.co.uk/i/keep-calm-you-made-it-7.png" alt="You made it!" width="150" length="175"/></div>
            </div>
          </form>
        </body>
        </html>'
        
        $WebConfig ='<?xml version="1.0" encoding="utf-8"?>
        <configuration>
          <system.web>
            <compilation debug="true" strict="false" explicit="true" targetFramework="4.5" />
            <httpRuntime targetFramework="4.5" />
            <identity impersonate="true" />
            <customErrors mode="Off"/>
          </system.web>
          <system.webServer>
            <defaultDocument>
              <files>
                <add value="Home.aspx" />
              </files>
            </defaultDocument>
          </system.webServer>
        </configuration>'
            
        $MainPage | Out-File -FilePath "C:\inetpub\wwwroot\Home.aspx" -Encoding ascii
        $WebConfig | Out-File -FilePath "C:\inetpub\wwwroot\Web.config" -Encoding ascii
    
    # Set App Pool to Clasic Pipeline to remote file access will work easier
        Write-Host "Updaing IIS Settings" -ForegroundColor Cyan
        c:\windows\system32\inetsrv\appcmd.exe set app "Default Web Site/" /applicationPool:".NET v4.5 Classic"
        c:\windows\system32\inetsrv\appcmd.exe set config "Default Web Site/" /section:system.webServer/security/authentication/anonymousAuthentication /userName:$theAdmin /password:$thePassword /commit:apphost
        
    # Make sure the IIS settings take
        Write-Host "Restarting the W3SVC" -ForegroundColor Cyan
        Restart-Service -Name W3SVC
        
        Write-Host
        Write-Host "Web App Creation Successfull!" -ForegroundColor Green
        Write-Host


## <a name="appvm01---file-server-installation-script"></a>AppVM01 - Datei-Server-Installationsskript
Dieses Skript richtet das Back-End für diese einfache Anwendung. Dieses Skript wird:

1.  Öffnen Sie IMCPv4 (Ping) auf der Firewall für einfacheres testen
2.  Erstellen Sie ein neues Verzeichnis
3.  Erstellen einer Textdatei um Remote Zugriff von der Webseite oben
4.  Festlegen von Berechtigungen für das Verzeichnis und zum anonymen Zugriff
5.  Internet Explorer Enhanced Security ermöglicht einfacher Durchsuchen von diesem Server ausschalten 

>[AZURE.IMPORTANT] **Best Practice**: Internet Explorer Enhanced Security auf einem Produktionsserver nie deaktivieren und ist im Allgemeinen nicht ratsam, im Internet surfen von einem Produktionsserver. Auch hier Dateifreigaben für den anonymen Zugriff öffnen eine schlechte Idee, aber der Einfachheit.

PowerShell-Skript muss lokal ausgeführt werden, und RDP in AppVM01. PowerShell muss als Administrator erfolgreiche Durchführung ausgeführt werden.
    
    # AppVM01 Server Post Build Config Script
    # PowerShell must be run as Administrator for Net Share commands to work
    
    # Turn On ICMPv4
        New-NetFirewallRule -Name Allow_ICMPv4 -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -Enabled True -Profile Any -Action Allow
    
    # Create Directory
        New-Item "C:\WebShare" -ItemType Directory
    
    # Write out Rand.txt
        $FileContent = "Hello, I'm the contents of a remote file on AppVM01."
        $FileContent | Out-File -FilePath "C:\WebShare\Rand.txt" -Encoding ascii
    
    # Set Permissions on share
        $Acl = Get-Acl "C:\WebShare"
        $AccessRule = New-Object system.security.accesscontrol.filesystemaccessrule("Everyone","ReadAndExecute, Synchronize","ContainerInherit, ObjectInherit","InheritOnly","Allow")
        $Acl.SetAccessRule($AccessRule)
        Set-Acl "C:\WebShare" $Acl
    
    # Create network share
        Net Share WebShare=C:\WebShare "/grant:Everyone,READ"
    
    # Turn Off IE Enhanced Security Configuration for Admins
        Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\{A509B1A7-37EF-4b3f-8CFC-4F3A74704073}" -Name "IsInstalled" -Value 0
    
        Write-Host
        Write-Host "File Server Setup Successfull!" -ForegroundColor Green
        Write-Host
    

## <a name="dns01---dns-server-installation-script"></a>DNS01 - Installationsskript für DNS-Server
Es ist kein Skript diese Anwendung den DNS-Server einrichten. Testen der Firewallregeln NSG oder UDR muss DNS-Datenverkehr, DNS01 Server müssen manuell installiert werden. Netzwerk XML-Konfigurationsdatei für beide Beispiele enthält DNS01 als primären DNS-Server und dem öffentlichen DNS-Server von Ebene 3 als backup DNS-Server gehostet. Ebene 3 DNS-Server ist für den nicht lokalen Datenverkehr verwendete DNS-Server, und mit DNS01 nicht einrichten treten keine lokalen DNS.

<!--Link References-->
[HOME]: ../best-practices-network-security.md
