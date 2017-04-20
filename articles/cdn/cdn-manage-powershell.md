<properties
    pageTitle="Verwalten von Azure CDN mit PowerShell | Microsoft Azure"
    description="Erfahren Sie, wie mit den Azure-PowerShell-Cmdlets Azure CDN verwalten."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="casoper"/>


# <a name="manage-azure-cdn-with-powershell"></a>Azure CDN mit PowerShell verwalten

PowerShell bietet flexible Methoden zum Verwalten von Azure CDN Profile und Endpunkte.  PowerShell können interaktiv oder durch Schreiben von Skripts Sie die Verwaltungsaufgaben automatisieren.  Dieses Lernprogramm demonstriert einige der häufigsten Aufgaben erreichen mit PowerShell zum Verwalten von Azure CDN Profile und Endpunkte.

## <a name="prerequisites"></a>Erforderliche Komponenten

Um PowerShell zum Verwalten von Azure CDN Profile und Endpunkte verwenden, müssen Sie das Azure PowerShell-Modul installiert.  Lernen zu Azure PowerShell verbinden Azure mit der `Login-AzureRmAccount` -Cmdlets finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

>[AZURE.IMPORTANT] Anmeldung mit `Login-AzureRmAccount` bevor Sie Azure PowerShell-Cmdlets ausführen können.

## <a name="listing-the-azure-cdn-cmdlets"></a>Auflisten von Azure CDN-cmdlets

Sie können alle Azure CDN Cmdlets mit Liste der `Get-Command` Cmdlet.

```text
PS C:\> Get-Command -Module AzureRM.Cdn

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnEndpointNameAvailability             2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Get-AzureRmCdnProfileSsoUrl                        2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnCustomDomain                         2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          New-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Publish-AzureRmCdnEndpointContent                  2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnCustomDomain                      2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnEndpoint                          2.0.0      AzureRm.Cdn
Cmdlet          Remove-AzureRmCdnProfile                           2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnEndpoint                             2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnOrigin                               2.0.0      AzureRm.Cdn
Cmdlet          Set-AzureRmCdnProfile                              2.0.0      AzureRm.Cdn
Cmdlet          Start-AzureRmCdnEndpoint                           2.0.0      AzureRm.Cdn
Cmdlet          Stop-AzureRmCdnEndpoint                            2.0.0      AzureRm.Cdn
Cmdlet          Test-AzureRmCdnCustomDomain                        2.0.0      AzureRm.Cdn
Cmdlet          Unpublish-AzureRmCdnEndpointContent                2.0.0      AzureRm.Cdn
```

## <a name="getting-help"></a>Hilfe

Erhalten von Hilfe bei dieser Cmdlets mit den `Get-Help` Cmdlet.  `Get-Help`bietet Verwendung und Syntax und Beispiele für optional.

```text
PS C:\> Get-Help Get-AzureRmCdnProfile

NAME
    Get-AzureRmCdnProfile

SYNOPSIS
    Gets an Azure CDN profile.


SYNTAX
    Get-AzureRmCdnProfile [-ProfileName <String>] [-ResourceGroupName <String>] [-InformationAction
    <ActionPreference>] [-InformationVariable <String>] [<CommonParameters>]


DESCRIPTION
    Gets an Azure CDN profile and all related information.


RELATED LINKS

REMARKS
    To see the examples, type: "get-help Get-AzureRmCdnProfile -examples".
    For more information, type: "get-help Get-AzureRmCdnProfile -detailed".
    For technical information, type: "get-help Get-AzureRmCdnProfile -full".

```

## <a name="listing-existing-azure-cdn-profiles"></a>Liste vorhandener Azure CDN Profile

Die `Get-AzureRmCdnProfile` Cmdlet ohne Parameter werden alle vorhandenen CDN Profile abgerufen.

```powershell
Get-AzureRmCdnProfile
```

Diese Ausgabe kann Cmdlets für Enumeration geleitet werden.

```powershell
# Output the name of all profiles on this subscription.
Get-AzureRmCdnProfile | ForEach-Object { Write-Host $_.Name }

# Return only **Azure CDN from Verizon** profiles.
Get-AzureRmCdnProfile | Where-Object { $_.Sku.Name -eq "StandardVerizon" }
```

Ein einzelnes Profil gelangen durch Angabe von Namen und Ressource Profilgruppe.

```powershell
Get-AzureRmCdnProfile -ProfileName CdnDemo -ResourceGroupName CdnDemoRG
```

>[AZURE.TIP] Es kann mehrere CDN Profile mit demselben Namen so sind in verschiedenen Ressourcengruppen.  Wenn die `ResourceGroupName` Parameter gibt alle Profile mit einem übereinstimmenden Namen.

## <a name="listing-existing-cdn-endpoints"></a>Vorhandene Liste CDN-Endpunkte

`Get-AzureRmCdnEndpoint`kann ein einzelner Endpunkt oder alle Endpunkte Profil abrufen.  

```powershell
# Get a single endpoint.
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Get all of the endpoints on a given profile. 
Get-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG

# Return all of the endpoints on all of the profiles.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint

# Return all of the endpoints in this subscription that are currently running.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Where-Object { $_.ResourceState -eq "Running" }
```

## <a name="creating-cdn-profiles-and-endpoints"></a>Erstellen von CDN Profile und Endpunkte

`New-AzureRmCdnProfile`und `New-AzureRmCdnEndpoint` CDN Profile und Endpunkte verwendet.

```powershell
# Create a new profile
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US"

# Create a new endpoint
New-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Location "Central US" -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

# Create a new profile and endpoint (same as above) in one line
New-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -Sku StandardAkamai -Location "Central US" | New-AzureRmCdnEndpoint -EndpointName cdnposhdoc -OriginName "Contoso" -OriginHostName "www.contoso.com"

```

## <a name="checking-endpoint-name-availability"></a>Überprüfen der Verfügbarkeit des Namens Endpunkt

`Get-AzureRmCdnEndpointNameAvailability`Gibt ein Objekt, wenn ein Endpunkt verfügbar ist.

```powershell
# Retrieve availability
$availability = Get-AzureRmCdnEndpointNameAvailability -EndpointName "cdnposhdoc"

# If available, write a message to the console.
If($availability.NameAvailable) { Write-Host "Yes, that endpoint name is available." }
Else { Write-Host "No, that endpoint name is not available." }
```

## <a name="adding-a-custom-domain"></a>Hinzufügen einer benutzerdefinierten Domäne

`New-AzureRmCdnCustomDomain`Fügt einen benutzerdefinierten Domänennamen für einen vorhandenen Endpunkt.

>[AZURE.IMPORTANT] Sie müssen mit Ihrem DNS-Anbieter Siehe [benutzerdefinierte Domäne Content Delivery Network (CDN) Endpunkt zuordnen](./cdn-map-content-to-custom-domain.md)der CNAME-Eintrag einrichten.  Testen Sie die Zuordnung vor dem Ändern der Endpunkt mit `Test-AzureRmCdnCustomDomain`.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Check the mapping
$result = Test-AzureRmCdnCustomDomain -CdnEndpoint $endpoint -CustomDomainHostName "cdn.contoso.com"

# Create the custom domain on the endpoint
If($result.CustomDomainValidated){ New-AzureRmCdnCustomDomain -CustomDomainName Contoso -HostName "cdn.contoso.com" -CdnEndpoint $endpoint }
```

## <a name="modifying-an-endpoint"></a>Einen Endpunkt ändern

`Set-AzureRmCdnEndpoint`Ändert einen vorhandenen Endpunkt.

```powershell
# Get an existing endpoint
$endpoint = Get-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Set up content compression
$endpoint.IsCompressionEnabled = $true
$endpoint.ContentTypesToCompress = "text/javascript","text/css","application/json"

# Save the changed endpoint and apply the changes
Set-AzureRmCdnEndpoint -CdnEndpoint $endpoint
```

## <a name="purgingpre-loading-cdn-assets"></a>Löschen/Pre-laden CDN Anlagen

`Unpublish-AzureRmCdnEndpointContent`bereinigt Ressourcen zwischengespeichert und `Publish-AzureRmCdnEndpointContent` auf unterstützten Endpunkte geladen.

```powershell
# Purge some assets.
Unpublish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -PurgeContent "/images/kitten.png","/video/rickroll.mp4"

# Pre-load some assets.
Publish-AzureRmCdnEndpointContent -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo -LoadContent "/images/kitten.png","/video/rickroll.mp4"

# Purge everything in /images/ on all endpoints.
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Unpublish-AzureRmCdnEndpointContent -PurgeContent "/images/*"
```

## <a name="startingstopping-cdn-endpoints"></a>Starten/Beenden CDN-Endpunkte
`Start-AzureRmCdnEndpoint`und `Stop-AzureRmCdnEndpoint` kann zum Starten und beenden, einzelne Endpunkte oder Gruppen von Endpunkten.

```powershell
# Stop the cdndocdemo endpoint
Stop-AzureRmCdnEndpoint -ProfileName CdnDemo -ResourceGroupName CdnDemoRG -EndpointName cdndocdemo

# Stop all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Stop-AzureRmCdnEndpoint

# Start all endpoints
Get-AzureRmCdnProfile | Get-AzureRmCdnEndpoint | Start-AzureRmCdnEndpoint
```

## <a name="deleting-cdn-resources"></a>Löschen von CDN-Ressourcen

`Remove-AzureRmCdnProfile`und `Remove-AzureRmCdnEndpoint` können verwendet werden, um Profile und Endpunkte zu entfernen.

```powershell
# Remove a single endpoint
Remove-AzureRmCdnEndpoint -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG -EndpointName cdnposhdoc

# Remove all the endpoints on a profile and skip confirmation (-Force)
Get-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG | Get-AzureRmCdnEndpoint | Remove-AzureRmCdnEndpoint -Force

# Remove a single profile
Remove-AzureRmCdnProfile -ProfileName CdnPoshDemo -ResourceGroupName CdnDemoRG
```

## <a name="next-steps"></a>Nächste Schritte

Informationen Sie zum Azure CDN mit [.NET](./cdn-app-dev-net.md) oder [Node.js](./cdn-app-dev-node.md)automatisieren.

Informationen zu Features CDN Übersicht [CDN](./cdn-overview.md).


