<properties
   pageTitle="Gast-BS-Familie 1 Ruhestand Beachten | Microsoft Azure"
   description="Enthält Informationen zum Ruhestand Azure Gast OS Familie 1 Wenn passiert und darüber, ob Sie betroffen sind"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>Gast OS Familie 1 Ruhestand Beachten

Stilllegung der OS Familie 1 wurde erstmals am 1. Juni 2013 angekündigt.

**2 September 2014** Das Azure Gastbetriebssystem (Gastbetriebssystem) Familie 1.x auf dem Betriebssystem Windows Server 2008 basiert, wurde offiziell zurückgezogen. Alle Versuche, neue Dienste bereitstellen oder aktualisieren vorhandene Dienste mit Familie 1 fehl mit Fehlermeldung darüber informiert, dass Gast OS Familie 1 eingestellt wurden.

**3. November 2014** Erweiterte Unterstützung für Gast OS Familie 1 beendet und vollständig gelöscht werden. Alle Dienste weiter Familie 1 werden beeinträchtigt. Wir können diese Dienste jederzeit beenden. Gibt es keine Garantie, dass Ihre Dienste ausgeführt weiterhin, wenn Sie manuell sie selbst aktualisieren.

Wenn Sie weitere Fragen haben, besuchen Sie [Cloud Services Foren](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) oder [Azure Support](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Sind Sie betroffen?

Cloud-Dienste sind betroffen, wenn die folgenden gilt:

1. Sie haben den Wert "OsFamily ="1"in die Datei ServiceConfiguration.cscfg für den Cloud-Dienst explizit angegeben.
2. Sie haben keinen Wert für OsFamily in die Datei ServiceConfiguration.cscfg für den Cloud-Dienst explizit angegeben. Derzeit verwendet das System den Standardwert von "1" in diesem Fall.
3. Azure-Verwaltungsportal zeigt Ihr Gastbetriebssystem Wert als "WindowsServer 2008".

Zu der Cloud-Dienste die OS ausgeführt, können Sie das folgende Skript in Azure PowerShell ausführen, obwohl [Azure PowerShell einrichten](../powershell-install-configure.md) , Sie zuerst müssen. Weitere Informationen über Skripts finden Sie unter [Azure Gast OS Familie 1 Auslaufdatum: Juni 2014](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

Cloud-Dienste werden die OsFamily-Spalte in der Ausgabe des Skripts ist leer oder enthält eine "1" von Altersversorgungsplänen OS Familie 1 beeinträchtigt.

## <a name="recommendations-if-you-are-affected"></a>Empfohlen, wenn Sie betroffen sind

Wir empfehlen, Ihre Cloud-Dienst Rollen eines der unterstützten Betriebssystem Familien zu migrieren:

**Gastbetriebssystem Familie 4.x** -Windows Server 2012 R2 *(empfohlen)*

1. Sicherstellen Sie, dass die Anwendung mit .NET Framework 4.0, 4.5 oder 4.5.1 SDK 2.1 oder höher verwendet.
2. Legen Sie das Attribut OsFamily auf "4" in der Datei ServiceConfiguration.cscfg und den Cloud-Dienst erneut.


**Gastbetriebssystem Familie 3.x** -Windows Server 2012

1. Sicherstellen Sie, dass die Anwendung mit .NET Framework 4.0 oder 4.5 SDK 1,8 oder höher verwendet.
2. Legen Sie das Attribut OsFamily auf "3" in der Datei ServiceConfiguration.cscfg und den Cloud-Dienst erneut.


**Gastbetriebssystem Familie 2.x** -Windows Server 2008 R2

1. Sicherstellen, dass Ihre SDK 1.3 Anwendung und höher von .NET Framework 3.5 oder 4.0.
2. Legen Sie das Attribut OsFamily auf "2" in die Datei ServiceConfiguration.cscfg und den Cloud-Dienst erneut.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>Erweiterte Unterstützung für Gast OS Familie 1 beendet 3 Nov 2014
Cloud-Services auf Gastbetriebssystem 1 werden nicht mehr unterstützt. Familie 1 so bald wie möglich zu langwierige migrieren.  

## <a name="next-steps"></a>Nächste Schritte
Lesen Sie die neuesten [Gastbetriebssystem frei](cloud-services-guestos-update-matrix.md).
