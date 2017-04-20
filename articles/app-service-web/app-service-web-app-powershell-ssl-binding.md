<properties
    pageTitle="SSL-Zertifikate Bindung mit PowerShell"
    description="Erfahren Sie, wie Zertifikate Ihrer Anwendung mithilfe von PowerShell binden."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure App SSL Zertifikatsbindung mit PowerShell #

Mit Microsoft Azure PowerShell Version 1.1.0 wurde ein neues Cmdlet, das dem Benutzer vorhandenen oder neuen Zertifikate eine Web App binden würde hinzugefügt.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Mit Informationen Ressourcenmanager Azure Azure PowerShell Grundlage überprüfen Cmdlets zu Web-Apps [Azure-Ressourcen-Manager basiert PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Hochladen und Binden eines SSL-Zertifikats ##

Szenario: Der Benutzer möchte ein SSL-Zertifikat zu seiner Web-Apps zu binden.

Wissen Ressourcengruppenname, die Web-app, Web app Name, Zertifikat PFX-Dateipfad auf den Computer des Benutzers, das Kennwort für das Zertifikat und den benutzerdefinierten Hostnamen enthält, können wir folgenden PowerShell-Befehl die SSL-Bindung erstellen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Beachten Sie, dass vor dem Hinzufügen einer SSL-Bindung einer Webanwendung, einen Hostnamen (benutzerdefinierte Domäne) bereits konfiguriert werden müssen. Wenn der Hostname nicht konfiguriert, wird eine Fehlermeldung "Hostname" während der Ausführung neu AzureRmWebAppSSLBinding nicht vorhanden. Sie können direkt über das Portal oder mit Azure PowerShell einen Hostnamen hinzufügen. Im folgenden Codeausschnitt PowerShell kann den Hostnamen konfigurieren, bevor Sie neue AzureRmWebAppSSLBinding ausführen.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Es ist wichtig zu verstehen, dass das Cmdlet "Set-AzureRmWebApp" die Hostnamen für die Webanwendung überschreibt. Daher ist der obige PowerShell Ausschnitt der vorhandenen Liste von Hostnamen für die Webanwendung anfügen.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Hochladen und Binden eines SSL-Zertifikats ##

Szenario: Der Benutzer zuvor hochgeladene SSL-Zertifikat zu seiner Web Apps anbinden möchten.

Wir erhalten die Liste der Zertifikate, die in einer Ressourcengruppe bereits mithilfe des folgenden Befehls hochgeladen

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Beachten Sie, dass die Zertifikate lokal an einen bestimmten Standort und Ressourcengruppe sind, muss der Benutzer das Zertifikat erneut hochladen, wenn konfigurierten Web app an einem anderen Ort und Ressourcengruppe, des erforderlichen Zertifikats 

Wissen Ressourcengruppenname, die Web-app, Web app Name den Fingerabdruck des Zertifikats und benutzerdefinierte Hostnamen enthält, können wir folgenden PowerShell-Befehl die SSL-Bindung erstellen:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Löschen einer vorhandenen SSL-Bindung  ##

Szenario: Der Benutzer möchte eine vorhandene SSL-Bindung zu löschen.

Wissen Ressourcengruppenname, die Web-app, Web app Name und benutzerdefinierte Hostnamen enthält, können wir folgenden PowerShell-Befehl um die SSL-Bindung zu entfernen:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Beachten Sie, dass entfernte SSL-Bindung wurde die letzte Bindung mit diesem Zertifikat, Speicherort standardmäßig das Zertifikat gelöscht werden, wenn der Benutzer das Zertifikat, dass er die DeleteCertificate-Option des Zertifikats zu verwenden möchten

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Referenzen ###
- [Azure Ressourcenmanager basierte PowerShell-Befehlen für Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Einführung in App Service-Umgebung](app-service-app-service-environment-intro.md)
- [Mithilfe von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md)
