<properties
    pageTitle="Aktivieren von Remotedebuggen mit kontinuierlichen Bereitstellung | Microsoft Azure"
    description="Aktivieren des Remotedebuggens Verwendung kontinuierlichen Bereitstellung in Azure bereitstellen"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Wenn Sie kontinuierlichen Bereitstellung Azure veröffentlichen Remotedebugging aktivieren

Remotedebuggen in Azure Cloud-Services oder virtuelle Computer bei [kontinuierlicher](cloud-services-dotnet-continuous-delivery.md) Verwendung folgendermaßen Azure veröffentlichen können.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Aktivieren des Remotedebuggens für Cloud-services

1. Richten Sie auf dem Build-Agent die ursprüngliche Umgebung für Azure gemäß [Befehlszeilen für Azure erstellen](http://msdn.microsoft.com/library/hh535755.aspx).
2. Da remote-Debug-Laufzeit (msvsmon.exe) erforderlich ist, installieren Sie [Remote Tools für Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (oder [Remote Tools for Microsoft Visual Studio 2013 Update 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) verwenden Sie Visual Studio 2013). Als Alternative können Sie die remote-Debug-Binärdateien von einem System kopieren, die Visual Studio installiert.
3. Erstellen Sie ein Zertifikat als [Übersicht über Zertifikate für Azure Cloud Services](cloud-services-certs-create.md)beschrieben. Die PFX-Datei und RDP Zertifikatfingerabdruck und Hochladen Sie das Zertifikat an den Zieldienst Cloud.
4. Verwenden Sie die folgenden Optionen in der MSBuild-Befehlszeile zum Erstellen und Verpacken mit remote Debuggen aktiviert. (Ersetzen Sie tatsächliche Pfade auf Ihr System und Projekt für die spitzen Klammern Elemente)

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`enthält der Pfad zu dem Ordner msvsmon.exe Remote Tools für Visual Studio.
    `RemoteDebuggerConnectorVersion`ist der Azure SDK Cloud-Dienst. Sie sollten auch mit Visual Studio installierte Version überein.

5. Veröffentlichen Sie Ziel Cloud-Dienst mithilfe der Datei Paket und .cscfg im vorherigen Schritt erstellt.
6. Importieren des Zertifikats (PFX-Datei) auf den Computer mit Visual Studio Azure SDK für .NET installiert. Zum Importieren der `CurrentUser\My` Zertifikatspeicher andernfalls Anfügen an den Debugger in Visual Studio schlägt fehl.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Aktivieren von Remotedebuggen für virtuelle Maschinen

1. Erstellen Sie einen Azure virtuellen Computer. Finden Sie [eine virtuelle Maschine mit WindowsServer](../virtual-machines/virtual-machines-windows-hero-tutorial.md) oder [Azure virtuelle Computer in Visual Studio erstellen und verwalten](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. [Azure klassische Portalseite](http://go.microsoft.com/fwlink/p/?LinkID=269851)Anzeigen des virtuellen Computers Dashboard des virtuellen Computers **ZERTIFIKATFINGERABDRUCK RDP**finden Sie unter. Dieser Wert dient für die `ServerThumbprint` Wert in der Konfiguration der Erweiterung.
3. Erstellen Sie ein Client-Zertifikat gemäß [Übersicht über Zertifikate für Azure Cloud Services](cloud-services-certs-create.md) (PFX und RDP Zertifikatfingerabdruck halten).
4. Azure Powershell installieren (Version 0.7.4 oder höher) [zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)gemäß.
5. Führen Sie das folgende Skript RemoteDebug Erweiterung zu aktivieren. Ersetzen Sie die Pfade und Daten durch Ihren eigenen wie Namen, Dienstnamen und Fingerabdruck.

    >[AZURE.NOTE] Dieses Skript ist für Visual Studio 2015 konfiguriert. Wenn Sie Visual Studio 2013 verwenden, ändern Sie die `$referenceName` und `$extensionName` Aufgaben mithilfe der folgenden `RemoteDebugVS2013` (anstelle von `RemoteDebugVS2015`).

    <pre>
   Hinzufügen AzureAccount

    Wählen Sie AzureSubscription "Mein Microsoft-Abonnement"

    $vm = "mytestvm1" Get-AzureVM - Dienstname-Name "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    Foreach ($endpoint in $endpoints) {$vm Add-AzureEndpoint - VM-Namen $endpoint. Name - Protokoll Tcp - PublicPort $endpoint. PublicPort - LokalerAnschluss $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1. * ist" $publicConfiguration = "<PublicConfig>< Connector.Enabled > True < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisher $publisher `
    -ExtensionName $extensionName ` 
    -Version $version "- PublicConfiguration $publicConfiguration

    Foreach ($extension in $vm. VM. ResourceExtensionReferences) {Wenn (($extension. $ReferenceName Verweisname - Eq) `
    -and ($extension.Publisher -eq $publisher) ` 
    - und ($extension. Eq - $extensionName Name) "- und ($extension. Version - Eq $version)) {$extension. ResourceExtensionParameterValues [0]. Key = "config.txt" Break}}

    $vm | AzureVM aktualisieren </pre>

6. Importieren des Zertifikats (PFX) auf den Computer mit Visual Studio Azure SDK für .NET installiert.
