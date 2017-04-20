<properties
    pageTitle="Azure Resource Manager Unterstützung für Traffic Manager | Microsoft Azure "
    description="Mithilfe von PowerShell für Traffic Manager mit Azure Resource Manager"
    services="traffic-manager"
    documentationCenter="na"
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="azure-resource-manager-support-for-azure-traffic-manager"></a>Azure Resource Manager Unterstützung für Azure Traffic Manager

Azure Ressourcen-Manager ist die bevorzugte Verwaltungsoberfläche für Dienste in Azure. Azure Traffic Manager-Profile können mit Azure-Ressourcen-Manager-basierte APIs und Tools verwaltet werden.

## <a name="resource-model"></a>Ressourcenmodell

Azure Traffic Manager wird mit Einstellungen aufgerufen Profil Traffic Manager konfiguriert. Dieses Profil enthält die DNS-Einstellungen, Datenverkehr Routingeinstellungen Endpunkt Überwachung Einstellungen und eine Liste der Endpunkte, Datenverkehr weitergeleitet wird.

Traffic Manager-Profil wird durch eine Ressource vom Typ "TrafficManagerProfiles" dargestellt. Ebene der REST-API ist der URI für jedes Profil wie folgt:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="comparison-with-the-azure-traffic-manager-classic-api"></a>Vergleich mit der klassischen Azure Traffic Manager-API

Azure-Ressourcen-Manager Unterstützung für Traffic Manager verwendet andere Terminologie als klassische Bereitstellungsmodell. Die folgende Tabelle zeigt die Unterschiede zwischen den Ressourcen-Manager und Classic:

| Ressourcenmanager-Begriff | Klassische Begriff |
|-----------------------|--------------|
| Streckenführung Methode | Netzwerklastenausgleich Methode |
| Priorität-Methode | Failover-Methode |
| Gewichtete Methode | Round-Robin-Methode |
| Performance-Methode | Performance-Methode |

Basierend auf Feedback von Kunden, haben wir die Terminologie zu verbessern die Klarheit Häufige Missverständnisse verringern. Es gibt keinen Unterschied in der Funktionalität.

## <a name="limitations"></a>Grenzen

Verweisen auf einen Endpunkt vom Typ 'AzureEndpoints' für eine Web-Anwendung können Traffic Manager Endpunkte nur Standard (Produktion) [Web App Steckplatz](../app-service-web/web-sites-staged-publishing.md)verweisen. Benutzerdefinierte Slots werden nicht unterstützt. Als Workaround können benutzerdefinierte Steckplätze mit "ExternalEndpoints" konfiguriert werden.

## <a name="setting-up-azure-powershell"></a>Einrichten von Azure PowerShell

Diese Anleitung verwenden Microsoft Azure PowerShell. Im folgende Artikel wird die Installation und Konfiguration von Azure PowerShell erläutert.

- [Zum Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

Die Beispiele in diesem Artikel annehmen, dass Sie eine vorhandene Ressourcengruppe. Erstellen Sie eine Ressourcengruppe mit dem folgenden Befehl:

```powershell
    New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

>[AZURE.NOTE] Azure Ressourcenmanager benötigen alle einen Speicherort. Dieser Speicherort wird als Standard für Ressourcen in dieser Ressourcengruppe verwendet. Allerdings seit Profilressourcen Traffic Manager sind global, nicht die Standortwahl Ressource Gruppe keine Auswirkung auf Azure Traffic Manager.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager-Profil erstellen

Verwenden Sie zum Erstellen eines Profils Traffic Manager neu-AzureRmTrafficManagerProfile-Cmdlet:

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

In der folgenden Tabelle werden die Parameter beschrieben:

| Parameter | Beschreibung |
|-----------|-------------|
| Name | Der Name der Ressource für die Profilressource Traffic Manager. Profile in derselben Ressourcengruppe müssen eindeutige Namen aufweisen. Dieser Name unterscheidet sich von den DNS-Namen für DNS-Abfragen verwendet.|
| ResourceGroupName | Der Name der Ressourcengruppe mit der Profilressource.|
| TrafficRoutingMethod | Gibt die Methode Datenverkehr routing bestimmt, welcher Endpunkt auf eine DNS-Abfrage zurückgegeben wird. Mögliche Werte sind "Leistung", 'Weighted' oder 'Priority'.|
| RelativeDnsName | Gibt den Hostnamenteil der DNS-Name dieses Profil Traffic Manager. Dieser Wert wird mit dem DNS-Domänennamen von Azure Traffic Manager zu den vollqualifizierten Domänennamen (FQDN) des Profils kombiniert. Beispielsweise wird der Wert "Contoso" "contoso.trafficmanager.net."|
| GÜLTIGKEITSDAUER | Gibt den DNS-Time-to-Live (TTL) in Sekunden. Diese Gültigkeitsdauer informiert lokalen DNS-Server und DNS-Clients wie lange Cache DNS-Antworten für dieses Profil Traffic Manager.|
| MonitorProtocol | Gibt das Protokoll mit Endpunkt überwachen. Mögliche Werte sind "HTTP" und "HTTPS".|
| MonitorPort | Gibt den TCP-Anschluss zum Endpunkt überwachen.|
| MonitorPath | Gibt den Pfad relativ zum Endpunkt Domain-Namen verwendet Endpunkt Gesundheit.|

Das Cmdlet Profil Traffic Manager in Azure erstellt und gibt eine entsprechende Profilobjekt PowerShell. An diesem Punkt enthält das Profil keine Endpunkte. Weitere Informationen zum Hinzufügen von Endpunkten zu einem Profil Traffic Manager finden Sie unter [Traffic Manager Endpunkte hinzufügen](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Traffic Manager Profil abrufen

Um ein vorhandenes Profilobjekt Traffic Manager abzurufen, verwenden Sie das Cmdlet "Get-AzureRmTrafficManagerProfle":

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Dieses Cmdlet gibt ein Profile-Objekt von Traffic Manager.

## <a name="update-a-traffic-manager-profile"></a>Traffic Manager-Profil aktualisieren

Traffic Manager Profile ändern folgt 3 Schritten:

1. Rufen Sie mit Get-AzureRmTrafficManagerProfile oder verwenden Sie neu AzureRmTrafficManagerProfile zurückgegebene Profil.
2. Ändern Sie das Profil. Hinzufügen und Entfernen von Endpunkten, oder Endpunkt oder Profil Parameter. Diese sind offline Operationen. Ändern Sie nur das lokale Objekt im Arbeitsspeicher, die das Profil darstellt.
3. Änderungen Sie die mithilfe des Set-AzureRmTrafficManagerProfile-Cmdlet.

Alle Profileigenschaften können mit Ausnahme des Profils RelativeDnsName geändert werden. Um die RelativeDnsName zu ändern, müssen Sie Profil und ein neues Profil unter einem neuen Namen löschen.

Im folgenden Beispiel wird veranschaulicht, wie das Profil TTL ändern:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    $profile.Ttl = 300
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Es gibt drei Arten von Traffic Manager Endpunkte:

1. **Azure Endpunkte** sind in Azure gehostete Dienste
2. **Externe Endpunkte** sind Dienste von Azure
3. **Verschachtelte Endpunkte** werden verwendet, um geschachtelte Hierarchien von Traffic Manager Profile erstellen. Geschachtelte Endpunkte ermöglichen Datenverkehr weiterleiten Konfigurationen für komplexe Applikationen.

In allen drei Fällen können Endpunkte auf zwei Arten hinzugefügt werden:

1. Verwenden von 3 Schritten beschrieben. Der Vorteil dieser Methode ist, dass mehrere Endpunkt eine einzige Aktualisierung geändert werden kann.
2. Verwenden das New-AzureRmTrafficManagerEndpoint-Cmdlet. Dieses Cmdlet hinzugefügt ein Profil in einem einzigen Vorgang Traffic Manager ein Endpunkt.

## <a name="adding-azure-endpoints"></a>Azure Endpunkte hinzufügen

Azure Endpunkte verweisen in Azure gehostete Dienste. Drei Arten von Azure Endpunkte werden unterstützt:

1. Azure webapps
2. "Klassische" cloud Services (die PaaS-Diensts oder IaaS virtuelle Computer enthalten können)
3. Azure Öffentl.IP-Ressourcen (die einen Lastenausgleich oder einer virtuellen Maschine NIC zugeordnet werden können). Die Öffentl.IP müssen den DNS-Namen in Traffic Manager verwendet werden.

In jedem Fall:

- Der Dienst wird mit dem Parameter 'TargetResourceId' des AzureRmTrafficManagerEndpointConfig hinzufügen oder neue AzureRmTrafficManagerEndpoint angegeben.
- "Ziel" und "EndpointLocation" werden durch die TargetResourceId festgelegt.
- Angeben das 'Gewicht' ist optional. Gewichte werden nur verwendet, wenn das Profil konfiguriert ist gewichtet"Streckenführung Methode. Andernfalls werden sie ignoriert. Wenn angegeben, muss der Wert eine Zahl zwischen 1 und 1000 sein. Der Standardwert ist "1".
- Angeben der 'Priority' ist optional. Prioritäten werden nur verwendet, wenn das Profil 'Priority' Streckenführung Methode verwenden. Andernfalls werden sie ignoriert. Gültige Werte sind 1 bis 1000 mit niedriger Priorität angibt. Wenn für einen Endpunkt angeben, müssen sie für alle Endpunkte angegeben werden. Wenn nicht angegeben, werden beginnend mit "1" in der Reihenfolge Standardwerte die Endpunkte aufgeführt sind.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Beispiel 1: Web App Endpunkte mit Add-AzureRmTrafficManagerEndpointConfig hinzufügen

In diesem Beispiel werden Traffic Manager-Profil erstellen und Hinzufügen von zwei Web App Endpunkten mithilfe des Add-AzureRmTrafficManagerEndpointConfig-Cmdlet.

```powershell
    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $webapp1 = Get-AzureRMWebApp -Name webapp1
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
    $webapp2 = Get-AzureRMWebApp -Name webapp2
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-a-classic-cloud-service-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 2: Einen "klassische" Cloud-Endpunkt mit neuen AzureRmTrafficManagerEndpoint hinzufügen

In diesem Beispiel wird "klassischer" Cloud Dienstendpunkt Traffic Manager-Profil hinzugefügt. In diesem Beispiel angegeben Profil Gruppennamen Profil und Ressourcen verwenden, anstatt ein Profilobjekt übergeben. Beide Ansätze werden unterstützt.

    $cloudService = Get-AzureRmResource -ResourceName MyCloudService -ResourceType "Microsoft.ClassicCompute/domainNames" -ResourceGroupName MyCloudService
    New-AzureRmTrafficManagerEndpoint -Name MyCloudServiceEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $cloudService.Id -EndpointStatus Enabled

### <a name="example-3-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 3: Einen neue AzureRmTrafficManagerEndpoint mit Öffentl.IP-Endpunkt hinzufügen

In diesem Beispiel wird eine öffentliche IP-Adressenressource Traffic Manager-Profil hinzugefügt. Die öffentliche IP-Adresse muss einen DNS-Namen konfiguriert und eine VM-NIC oder ein Lastenausgleich gebunden werden kann.

    $ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled

## <a name="adding-external-endpoints"></a>Externe Endpunkte hinzufügen

Traffic Manager verwendet externe Endpunkte Datenverkehr von Azure gehostete Dienste angewiesen. Mit Azure Endpunkte können hinzufügen AzureRmTrafficManagerEndpointConfig gefolgt von AzureRmTrafficManagerProfile Set oder New AzureRMTrafficManagerEndpoint mit externer Endpunkte hinzugefügt werden.

Wenn Sie externe Endpunkte angeben:

- Endpunkt-Domänenname muss den Parameter "Target" angegeben
- "Leistung" Streckenführung Methode verwendet wird, ist der EndpointLocation erforderlich. Andernfalls ist optional. Der Wert muss eine [gültige Azure Bereichsname](https://azure.microsoft.com/regions/).
- 'Gewicht' und 'Priority' sind optional.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Beispiel 1: Externe Endpunkte hinzufügen AzureRmTrafficManagerEndpointConfig mit AzureRmTrafficManagerProfile Gruppe hinzufügen

In diesem Beispiel ein Traffic Manager-Profil erstellen wir, zwei externe Endpunkte hinzufügen und Änderungen.

    $profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointStatus Enabled
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 2: Externer Endpunkte verwenden neue AzureRmTrafficManagerEndpoint hinzufügen

In diesem Beispiel fügen wir einen externen Endpunkt zu einem bestehenden Profil. Das Profil wird mit Namen Profil und Ressource angegeben.

    New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled

## <a name="adding-nested-endpoints"></a>'Nested' Endpunkte hinzufügen

Jedes Profil Traffic Manager gibt eine einzelne Methode Datenverkehr weiterleiten. Es gibt jedoch Szenarien, die anspruchsvollere Streckenführung erfordern als Arbeitsgang von einem einzigen Traffic Manager-Profil bereitgestellt. Traffic Manager Profile, um die Vorteile von mehrere Datenverkehr weiterleiten-Methode können geschachtelt werden. Geschachtelte Profile können Sie das Standardverhalten Traffic Manager größere und komplexere anwendungsbereitstellung überschreiben. Ausführlichere Beispiele finden Sie unter [geschachtelte Traffic Manager Profile](traffic-manager-nested-profiles.md).

Geschachtelte Endpunkte sind übergeordnete Profil, das mit einem bestimmten Endpunkt 'NestedEndpoints' konfiguriert. Wenn geschachtelte Endpunkte angeben:

- Der Endpunkt muss den Parameter 'TargetResourceId' angegeben
- "Leistung" Streckenführung Methode verwendet wird, ist der EndpointLocation erforderlich. Andernfalls ist optional. Der Wert muss eine [gültige Azure Bereichsname](http://azure.microsoft.com/regions/).
- 'Gewicht' und 'Priority' sind optional für Azure Endpunkte.
- Der Parameter 'MinChildEndpoints' ist optional. Der Standardwert ist "1". Die Anzahl der verfügbaren Endpunkte diesen Schwellenwert unterschreitet, betrachtet das übergeordnete Profil der untergeordneten "heruntergestuft" und leitet Datenverkehr mit anderen Endpunkten im übergeordneten Profil.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Beispiel 1: Geschachtelte Endpunkte hinzufügen AzureRmTrafficManagerEndpointConfig mit AzureRmTrafficManagerProfile Gruppe hinzufügen

In diesem Beispiel wir neue Traffic Manager untergeordneten und übergeordneten Profile erstellen, das übergeordnete Element die untergeordneten als geschachtelte Endpunkt hinzufügen und Änderungen.

    $child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    $parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

Aus Platzgründen in diesem Beispiel haben wir keine Endpunkte mit den untergeordneten oder übergeordneten hinzugefügt.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Beispiel 2: Geschachtelte Endpunkte verwenden neue AzureRmTrafficManagerEndpoint hinzufügen

In diesem Beispiel wir ein kinderprofil als geschachtelte Endpunkt ein vorhandenen übergeordneten Profil hinzufügen. Das Profil wird mit Namen Profil und Ressource angegeben.

    $child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
    New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2

## <a name="update-a-traffic-manager-endpoint"></a>Traffic Manager-Endpunkt aktualisieren

Gibt es zwei Arten einen vorhandenen Endpunkt Traffic Manager aktualisieren:

1. Die Traffic Manager Profil mit Get-AzureRmTrafficManagerProfile und Änderungen mit Set AzureRmTrafficManagerProfile Endpunkt innerhalb des Profils aktualisieren. Diese Methode hat den Vorteil, mehr als einen Endpunkt in einem einzigen Vorgang aktualisieren.
2. Traffic Manager Endpunkt mit Get-AzureRmTrafficManagerEndpoint erhalten und Änderungen mit Set AzureRmTrafficManagerEndpoint Endpunkt aktualisieren. Diese Methode ist einfacher, da keine Endpunkte im Profil indiziert erforderlich ist.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Beispiel 1: Aktualisieren Endpunkte mithilfe von Get-AzureRmTrafficManagerProfile und AzureRmTrafficManagerProfile festlegen

In diesem Beispiel ändern Sie die Priorität auf zwei Endpunkte innerhalb eines vorhandenen Profils.

    $profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
    $profile.Endpoints[0].Priority = 2
    $profile.Endpoints[1].Priority = 1
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Beispiel 2: Aktualisieren einen Endpunkt mithilfe von Get-AzureRmTrafficManagerEndpoint und AzureRmTrafficManagerEndpoint festlegen

In diesem Beispiel bearbeiten wir das Gewicht der einzelnen Endpunkt in einem vorhandenen Profil

    $endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
    $endpoint.Weight = 20
    Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Aktivieren und Deaktivieren von Endpunkten und Profile

Traffic Manager können einzelne Endpunkte aktiviert und deaktiviert, aktivieren und Deaktivieren der ganze Profile können.
Diese können durch Abrufen/aktualisieren/einrichten Endpunkt oder Profil Ressourcen geändert werden. Um diese allgemeinen Operationen zu vereinfachen, werden sie über dedizierte Cmdlets unterstützt.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Beispiel 1: Aktivieren und Deaktivieren von Traffic Manager-Profil

Zum Aktivieren der Traffic Manager-Profil verwenden Sie AzureRmTrafficManagerProfile aktivieren. Das Profil kann mit einem Profilobjekt angegeben werden. Das Profilobjekt übergeben werden kann, über die Pipeline oder die '-TrafficManagerProfile' Parameter. In diesem Beispiel geben wir das Profil der Gruppenname Profil und Ressource.

```powershell
    Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Traffic Manager-Profil zu deaktivieren:

```powershell
    Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Mit dem Cmdlet Disable AzureRmTrafficManagerProfile fordert. Diese Aufforderung mit unterdrückt die '-Force' Parameter.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Beispiel 2: Aktivieren und Deaktivieren von Traffic Manager-Endpunkt

Zum Aktivieren der Traffic Manager-Endpunkt verwenden Sie AzureRmTrafficManagerEndpoint aktivieren. Es gibt zwei Arten an den Endpunkt

1. Ein TrafficManagerEndpoint-Objekt über die Pipeline übergeben oder die '-TrafficManagerEndpoint' Parameter
2. Verwenden den Endpunktnamen, Endpunkttyp Profilnamen und den Namen:

```powershell
    Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Auf ähnliche Weise, Traffic Manager-Endpunkt zu deaktivieren:

```powershell
     Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Mit Disable-AzureRmTrafficManagerProfile fordert das Cmdlet AzureRmTrafficManagerEndpoint deaktivieren. Diese Aufforderung mit unterdrückt die '-Force' Parameter.

## <a name="delete-a-traffic-manager-endpoint"></a>Traffic Manager-Endpunkt löschen

Um einzelne Endpunkte zu entfernen, verwenden Sie das Cmdlet "Remove-AzureRmTrafficManagerEndpoint":

```powershell
    Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Dieses Cmdlet fordert. Diese Aufforderung mit unterdrückt die '-Force' Parameter.

## <a name="delete-a-traffic-manager-profile"></a>Traffic Manager-Profil löschen

Zum Löschen eines Profils Traffic Manager dem Cmdlet entfernen AzureRmTrafficManagerProfile, Profil und Ressource Namen angeben:

```powershell
    Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Dieses Cmdlet fordert. Diese Aufforderung mit unterdrückt die '-Force' Parameter.

Das zu löschende Profil kann auch in ein Profilobjekt angegeben werden:

```powershell
    $profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
    Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Diese Reihenfolge kann auch geleitet:

```powershell
    Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Nächste Schritte

[Traffic Manager überwachen](traffic-manager-monitoring.md)

[Traffic Manager Performance-Aspekte](traffic-manager-performance-considerations.md)
