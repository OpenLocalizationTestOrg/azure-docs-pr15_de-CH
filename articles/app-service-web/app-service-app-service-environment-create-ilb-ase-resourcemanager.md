<properties 
    pageTitle="Eine ILB ASE Azure Resource Manager Vorlagen erstellen | Microsoft Azure" 
    description="Erfahren Sie, wie eine interne Load Balancer ASE Azure-Ressourcen-Manager Vorlagen erstellen." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-create-an-ilb-ase-using-azure-resource-manager-templates"></a>Eine ILB ASE Azure Resource Manager Vorlagen erstellen

## <a name="overview"></a>Übersicht ##
App Service-Umgebung können virtuelle interne Netzwerkadresse mit öffentlichen VIP erstellt werden.  Diese interne Adresse bietet Azure Bestandteil der internen Lastenausgleich (ILB) aufgerufen.  Ein ILB ASE kann mithilfe des Azure-Portals.  Sie können auch mithilfe der Automatisierung in Azure Resource Manager Vorlagen erstellt werden.  Dieser Artikel führt Schritte und Syntax musste eine ILB ASE Azure-Ressourcen-Manager Vorlagen erstellen.

Automatisieren der Erstellung einer ILB ASE umfasst drei Schritte:
1. Zunächst wird in ein virtuelles Netzwerk mit einer internen Load Balancer Adresse statt einer öffentlichen VIP base ASE erstellt.  Als Teil dieses Schritts ist eine Stammdomänennamen ILB ASE zugewiesen.
2. Erstellte ILB ASE ist ein SSL-Zertifikat hochgeladen.  
3. Hochgeladene SSL-Zertifikat wird als die "Default" SSL-Zertifikat ILB ASE explizit zugewiesen.  Dieses Zertifikat wird zum SSL-Datenverkehr Apps auf ASE ILB verwendet werden, wenn apps adressiert werden die gemeinsamen Stammdomäne ASE (z.B. https://someapp.mycustomrootcomain.com) zugeordnet

## <a name="creating-the-base-ilb-ase"></a>Erstellen der Basis ILB ASE ##
Eine Beispielvorlage Azure-Ressourcen-Manager und die dazugehörigen Parameter-Datei stehen auf GitHub [hier][quickstartilbasecreate].

Die meisten Parameter in der Datei *azuredeploy.parameters.json* gelten sowohl ILB Asen, sowie Asen zu einem öffentlichen gebunden.  Liste ruft Parameter hervorzuheben oder, sind eindeutig, Erstellung einer ILB ASE:


- *InteralLoadBalancingMode*: In den meisten Fällen dies 3, d.h. HTTP/HTTPS-Datenverkehr auf Ports 80/443 sowohl den Steuerelementdaten Channel-Ports gehört der FTP-Dienst auf dem ASE Satz an ein virtuelles Netzwerk interne Adresse zugewiesen ILB gebunden werden.  Wenn diese Eigenschaft stattdessen auf 2 festgelegt ist, werden nur FTP-Dienst-bezogenen Ports (sowohl und Kanäle) an eine ILB gebunden werden, bleiben der HTTP/HTTPS-Datenverkehr auf Öffentliche VIP.
-  *DnsSuffix*: dieser Parameter definiert die Standard-Stammdomäne, die ASE zugewiesen werden.  In öffentlichen Variante von Azure App Service ist der Standard-Stammdomäne für alle Web-apps **.azurewebsites.NET*.  Aber da ein ILB ASE virtuelle Netzwerke ist, es mit dem öffentlichen Dienst standardmäßig Stammdomäne Sinn.  Stattdessen sollte ein ILB ASE eine Standarddomäne Stamm haben Sinn für die Verwendung innerhalb eines Unternehmens internes virtuelles Netzwerk.  Beispielsweise können eine hypothetische Contoso Corporation eine Stammdomäne standardmäßig *interne* contoso.com Apps, das nur aufgelöst und in virtuellen Netzwerks sein sollen. 
-  *IpSslAddressCount*: dieser Parameter wird automatisch auf den Wert 0 in der Datei *azuredeploy.json* übernommen, da ILB Asen nur eine ILB-Adresse.  Keine explizite SSL IP-Adressen für eine ASE ILB und daher SSL IP-Adresspool für einen ILB ASE muss auf NULL festgelegt, andernfalls ein Bereitstellung Fehler auftreten wird. 

Nachdem die *azuredeploy.parameters.json* -Datei für eine ILB ASE ausgefüllt wurde, können ILB ASE dann mit Codeausschnitt von Powershell erstellt werden.  Ändern der Datei Pfade entsprechend, Vorlagendateien Azure-Ressourcen-Manager auf Ihrem Computer befinden.  Beachten Sie außerdem eigene Werte für Ressourcenmanager Azure bereitstellungsnamen und den Namen angeben.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nach der Azure-Ressourcen-Manager wird Vorlage übermittelt, dauert es ein paar Stunden ILB ASE erstellt werden.  Nach Abschluss die Erstellung erscheint ILB ASE im Portal UX in der Liste der Umgebungen App für das Abonnement, die die Bereitstellung ausgelöst.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Hochladen und Konfigurieren von "Standard" SSL-Zertifikat ##

Erstellte ILB ASE sollten ein SSL-Zertifikat mit ASE "Default" SSL-Zertifikat für das Herstellen einer SSL-Verbindung zu apps verwenden.  Hypothetischen Beispiel Contoso Corporation fortsetzen, wenn der ASE DNS standardmäßig Suffix *interne contoso.com*und dann eine Verbindung zu *https://some-random-app.internal-contoso.com* erfordert ein SSL-Zertifikat für **...internen contoso.com*. 

Es gibt verschiedene Arten, ein gültiges SSL-Zertifikat einschließlich der internen Zertifizierungsstellen und der Erwerb eines Zertifikats von einem externen Emittenten ein selbstsigniertes Zertifikat verwenden.  Die folgenden Attribute Bescheinigung müssen unabhängig von der Quelle des SSL-Zertifikats konfiguriert werden:

- *Betreff*: Dieses Attribut muss auf gesetzt **Ihr Root-Domäne here.com*
- *Alternativer Antragstellername*: Dieses Attribut sind beide * *Ihr Root-Domäne here.com*und * *.scm.your-Root-Domain-here.com*.  Der Grund für den zweiten Eintrag ist, dass SSL-Verbindungen auf jede Anwendung zugeordnete SCM-Kudu-Website mit der Adresse der Form *your-app-name.scm.your-root-domain-here.com*werden.

Ein gültiges SSL-Zertifikat Hand sind zwei weitere vorbereitende Schritte erforderlich.  Das SSL-Zertifikat muss konvertiert/als PFX-Datei gespeichert.  Denken Sie, dass die PFX-Datei muss alle zwischen-und Stammzertifikate muss mit einem Kennwort geschützt werden.

Die resultierende PFX-Datei muss dann in eine base64-Zeichenfolge konvertiert werden, da das SSL-Zertifikat mit einer Azure-Ressourcen-Manager-Vorlage hochgeladen werden.  Da Azure Ressourcenmanager Vorlagen Textdateien sind, muss die PFX-Datei in eine base64-Zeichenfolge konvertiert werden, damit es als Parameter der Vorlage eingefügt werden kann.

Folgende Powershell Codeausschnitt zeigt ein Beispiel für ein selbst signiertes Zertifikat generieren und Exportieren des Zertifikats als PFX-Datei konvertieren die PFX-Datei in eine base64-codierte Zeichenfolge speichern base64-codierte Zeichenfolge in eine separate Datei.  Powershell-Code für die base64-Codierung wurde von [Powershell Skripts Blog][examplebase64encoding].

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     
    
    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")
    
Sobald das SSL-Zertifikat wurde erfolgreich erstellt und konvertiert eine base64-codierte Zeichenfolge der Azure-Ressourcenmanager Beispielvorlage auf GitHub zum [Konfigurieren von Standard-SSL-Zertifikat] [ configuringDefaultSSLCertificate] können verwendet werden.

Die Parameter in der Datei *azuredeploy.parameters.json* sind:

- *AppServiceEnvironmentName*: der Name des ILB ASE konfiguriert wird.
- *ExistingAseLocation*: Zeichenfolge mit der Azure-Region, wo die ILB ASE bereitgestellt wurde.  Beispiel: "South Central US".
- *PfxBlobString*: die based64 codiert Stringdarstellung der PFX-Datei.  Den Codeausschnitt gezeigt würden Sie kopiert die Zeichenfolge in "exportedcert.pfx.b64" und fügen Sie ihn als Wert des Attributs *PfxBlobString* .
- *Kennwort*: das Kennwort zum Schutz der PFX-Datei.
- *CertificateThumbprint*: der Fingerabdruck des Zertifikats.  Wenn dieser Wert von Powershell abrufen (z. B. *$certificate. Fingerabdruck* der früheren Codeausschnitt), können Sie den Wert als-ist.  Aber wenn den Wert im Dialogfeld Windows-Zertifikat kopiert müssen Sie überflüssigen Leerzeichen entfernen.  *CertificateThumbprint* sollte etwa so aussehen: AF3143EB61D43F6727842115BB7F17BBCECAECAE
- *CertificateName*: angezeigten Zeichenfolgenbezeichner Ihrer Wahl mit der Identität des Zertifikats.  Der Name dient als Teil des eindeutigen Bezeichners Azure-Ressourcen-Manager für die *Microsoft.Web/certificates* -Entität, die das SSL-Zertifikat darstellt.  Namen **müssen** Ende mit der folgenden Zusatz: \_YourASENameHere_InternalLoadBalancingASE.  Dieses Suffix wird vom Portal als Indikator verwendet das Zertifikat dient zum Sichern einer ASE ILB aktiviert.


Ein verkürztes Beispiel *azuredeploy.parameters.json* ist unten dargestellt:


    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Nachdem die Datei *azuredeploy.parameters.json* ausgefüllt wurde, kann Standard-SSL-Zertifikat mit Codeausschnitt von Powershell konfiguriert werden.  Ändern der Datei Pfade entsprechend, Vorlagendateien Azure-Ressourcen-Manager auf Ihrem Computer befinden.  Beachten Sie außerdem eigene Werte für Ressourcenmanager Azure bereitstellungsnamen und den Namen angeben.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"
    
    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nachdem die Azure-Ressourcen-Manager-Vorlage gesendet wird, etwa 40 Minuten pro ASE Front-End-dauert die Änderung zu übernehmen.  Beispielsweise mit einem Standard Größe ASE mit zwei Front-Ends, dauert die Vorlage ungefähr 1 Stunde und 20 Minuten.  Während der Ausführung der Vorlage wird ASE nicht skaliert werden.  

Nach Abschluss die Vorlage apps auf ILB ASE über HTTPS zugegriffen werden und die Verbindung werden mit Standard-SSL-Zertifikat gesichert werden.  Standard-SSL-Zertifikat wird bei apps auf ILB ASE sind eine Kombination aus den Anwendungsnamen und der Standard-Hostname verwendet werden.  Beispielsweise *https://mycustomapp.internal-contoso.com* , verwenden das Standard-SSL-Zertifikat für **...internen contoso.com*.

Apps auf Öffentliche Multi-Tenant-Service, wie können Entwickler jedoch auch benutzerdefinierte Hostnamen für einzelne apps konfigurieren, und konfigurieren Sie eindeutige SNI SSL-Zertifikat Bindings für einzelne apps.  


## <a name="getting-started"></a>Erste Schritte

Zunächst mit App-Dienst finden Sie unter [Einführung in App Service-Umgebung](app-service-app-service-environment-intro.md)

Alle Artikel und -die App Service-Umgebungen in der [Readme-Datei für Anwendungsumgebungen](../app-service/app-service-app-service-environments-readme.md)verfügbar sind.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[quickstartilbasecreate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-create/
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/ 
 
