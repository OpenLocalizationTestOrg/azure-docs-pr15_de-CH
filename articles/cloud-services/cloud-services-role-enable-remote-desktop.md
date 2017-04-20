<properties 
pageTitle="Aktivieren Sie Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services" 
description="Wie Sie Ihre Azure-Cloud Remotedesktopverbindungen zu konfigurieren" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Aktivieren Sie Remotedesktop-Verbindung für eine Rolle in Azure Cloud Services

>[AZURE.SELECTOR]
- [Azure-Verwaltungsportal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Remotedesktop können Sie auf dem Desktop eine Rolle in Azure ausgeführt. Sie können eine Remotedesktopverbindung beheben und Diagnostizieren von Problemen mit der Anwendung während der Ausführung. 

Sie können eine Remotedesktopverbindung in Ihrer Rolle bei der Entwicklung mit Remotedesktop-Module in der Dienstdefinition, oder Sie können Remotedesktop über Remote Desktop-Erweiterung aktivieren. Der bevorzugte Ansatz ist das Remotedesktop verwenden wie Sie Remotedesktop aktivieren können, nach der die Anwendung bereitgestellt wird, ohne die Anwendung erneut bereitstellen. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Konfigurieren von Remotedesktop vom klassischen Azure-portal
Azure-Verwaltungsportal verwendet Remote Desktop-Erweiterung Ansatz Sie Remotedesktop aktivieren können, auch wenn die Anwendung bereitgestellt wird. Seite **Konfigurieren** für den Clouddienst können Sie Remotedesktop aktivieren, ändern Sie das lokale Administratorkonto zur Verbindung mit den virtuellen Computern, die Authentifizierung verwendet und das Ablaufdatum. 


1. Klicken Sie auf **Clouddienste**klicken Sie auf den Namen des Cloud-Dienst, und klicken Sie auf **Konfigurieren**.

2. Klicken Sie auf **Remote**.
    
    ![Cloud-Services remote](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Alle Instanzen werden gestartet, wenn Remotedesktop aktivieren und klicken Sie auf OK (Häkchen). Um einen Neustart zu verhindern, muss das Zertifikat verschlüsselt das Kennwort für die Rolle installiert. Verhindert einen Neustart [ein Zertifikat für den Clouddienst hochladen](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) und dann zu diesem Dialogfeld zurückkehren.
    

3. Wählen Sie **Rollen**die Rolle zu aktualisieren, oder wählen Sie **Alle** für alle Rollen.

4. Stellen Sie eine der folgenden:
    
    - Um Remote Desktop zu aktivieren, aktivieren Sie das Kontrollkästchen **Remotedesktop aktivieren** . Um Remote Desktop zu deaktivieren, deaktivieren Sie das Kontrollkästchen.
    
    - Erstellen eines Kontos in Remotedesktopverbindungen den Rolleninstanzen verwendet.
    
    - Aktualisieren Sie das Kennwort für das Konto.
    
    - Wählen Sie ein Upload-Zertifikat für die Authentifizierung (das Zertifikat auf der Seite **Zertifikate** mit **Hochladen** Upload) oder ein neues Zertifikat erstellen. 
    
    - Ändern Sie das Ablaufdatum für die Remotedesktop-Konfiguration.

5. Wenn Ihre konfigurationsupdates abgeschlossen haben, klicken Sie auf **OK** (Häkchen).


## <a name="remote-into-role-instances"></a>Remote in Rolleninstanzen
Nachdem die Rollen Remotedesktop aktiviert können Sie Remote in eine Rolleninstanz über verschiedene Tools.

Verbindung zu einer Instanz der Rolle von klassischen Azure-Portal:
    
  1.   Klicken Sie auf **Instanzen** öffnen Seite **Instanzen** .
  2.   Wählen Sie eine Instanz der Rolle, die Remote Desktop konfiguriert wurde.
  3.   Klicken Sie auf **Verbinden**, und gehen Sie auf den Desktop öffnen. 
  4.   Klicken Sie auf **Öffnen** und dann **Verbinden** um die Remotedesktopverbindung zu starten. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Mit der Visual Studio remote in eine Instanz der Rolle

In Visual Studio Server-Explorer:

1. Erweitern Sie die **Azure\\Clouddienste\\[Cloud Service Name]** Knoten.
2. Erweitern Sie **Staging-** oder **Produktionsserver**.
3. Erweitern Sie die einzelne Rolle.
4. Maustaste eine der Instanzen, klicken Sie auf **Verbinden mit Remotedesktop...**und geben Sie den Benutzernamen und das Kennwort. 

![Server-Explorer Remotedesktop](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>Die RDP-Datei mithilfe von PowerShell
Das Cmdlet " [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) " können Sie die RDP-Datei abzurufen. Die RDP-Datei können mit Remotedesktopverbindung Sie Cloud-Dienst zugreifen.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Die RDP-Datei über die Service-REST-API programmgesteuert herunterzuladen
Vorgang REST [RDP-Datei herunterladen](https://msdn.microsoft.com/library/jj157183.aspx) können, die RDP-Datei herunterzuladen. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Remote Desktop in der Definitionsdatei Service konfigurieren

Diese Methode ermöglicht das Aktivieren des Remotedesktops für die Anwendung während der Entwicklung. Dieser Ansatz erfordert verschlüsselte Kennwörter gespeichert in der Dienstkonfiguration Datei und Updates remote desktop-Konfiguration benötigt eine erneute Bereitstellung der Anwendung. Möchten Sie diese Nachteile vermeiden sollten Sie den oben beschriebenen Ansatz remote Desktop-Erweiterung verwenden.  

Visual Studio können Service Definition Dateiansatz [eine Remotedesktopverbindung aktivieren](../vs-azure-tools-remote-desktop-roles.md) .  
Die folgenden Schritte beschreiben die Änderungen Service Model-Dateien Aktivieren von Remotedesktop. Visual Studio wird automatisch ändert diese beim Veröffentlichen.

### <a name="set-up-the-connection-in-the-service-model"></a>Einrichten der Verbindung das Service-Modell 
Verwenden Sie das Element **Einfuhren** **RAS** -Modulen und **RemoteForwarder** die [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) -Datei importieren.

Service-Definitionsdatei sollte ähnlich wie im folgenden Beispiel mit der `<Imports>` Element hinzugefügt.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Die Datei [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) sollte ähnlich wie im folgenden Beispiel, beachten Sie die `<ConfigurationSettings>` und `<Certificates>` Elemente. Das angegebene Zertifikat muss [zum Cloud-Dienst hochgeladen](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Cloud-Dienste konfigurieren](cloud-services-how-to-configure.md)