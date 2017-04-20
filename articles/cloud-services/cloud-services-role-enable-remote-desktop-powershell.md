<properties
pageTitle="Aktivieren Sie Remotedesktopverbindung für eine Rolle in Azure-Cloud-Diensten mit PowerShell"
description="Wie Sie Ihre Azure-Cloud mit PowerShell Remotedesktopverbindungen zu konfigurieren"
services="cloud-services"
documentationCenter=""
authors="thraka"
manager="timlt"
editor=""/>
<tags
ms.service="cloud-services"
ms.workload="tbd"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="08/05/2016"
ms.author="adegeo"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Aktivieren Sie Remotedesktopverbindung für eine Rolle in Azure-Cloud-Diensten mit PowerShell

>[AZURE.SELECTOR]
- [Azure-Verwaltungsportal](cloud-services-role-enable-remote-desktop.md)
- [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Remotedesktop können Sie auf dem Desktop eine Rolle in Azure ausgeführt. Sie können eine Remotedesktopverbindung beheben und Diagnostizieren von Problemen mit der Anwendung während der Ausführung.

Dieser Artikel beschreibt das Aktivieren von Remotedesktop auf Ihre Cloud-Dienstverwaltungsrollen mit PowerShell. Informationen Sie [zur Installation und Konfiguration von Azure PowerShell](../powershell-install-configure.md) für die erforderlichen Komponenten für diesen Artikel benötigt. PowerShell verwendet Remote Desktop-Erweiterung, sodass Sie Remotedesktop aktivieren können, nachdem die Anwendung bereitgestellt wird.


## <a name="configure-remote-desktop-from-powershell"></a>Konfigurieren Sie Remotedesktop von PowerShell

Das Cmdlet " [Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) " können Sie Remote Desktop angegebenen Rollen oder alle Funktionen der Cloudbereitstellung-Dienst aktivieren. Das Cmdlet können Sie den Benutzernamen und das Kennwort für remote Desktop-Benutzer über den *Credential* -Parameter angeben, die ein PSCredential Objekt akzeptiert.

Wenn Sie PowerShell interaktiv verwenden, können Sie einfach das PSCredential-Objekt durch das Cmdlet " [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) " aufrufen festlegen.

```
$remoteusercredentials = Get-Credential
```

Dieser Befehl zeigt ein Dialogfeld Sie Benutzername und Kennwort für den entfernten Benutzer auf sichere Weise eingeben.

Da PowerShell in Szenarios für die Automatisierung unterstützt, können Sie auch **PSCredential** Objekt so einrichten, die Interaktion des Benutzers erfordert. Zunächst müssen Sie ein sicheres Kennwort einrichten. Sie beginnen mit der Angabe einer nur-Text-Kennwort in eine sichere Zeichenfolge mit [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)konvertieren. Als Nächstes müssen Sie in eine verschlüsselte Standardzeichenfolge [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx)mithilfe dieser sicheren Zeichenfolge konvertieren. Jetzt können Sie diese verschlüsselte Zeichenfolge standard mit [Set-Content](https://technet.microsoft.com/library/ee176959.aspx)in eine Datei speichern.

Damit Sie nicht jedes Mal das Kennwort eingeben, können Sie auch eine sichere Datei erstellen. Außerdem ist eine sichere Datei besser als eine Textdatei. Verwenden Sie die folgenden PowerShell ein sicheres Kennwort zu erstellen:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

>[AZURE.IMPORTANT] Beim Festlegen des Kennworts stellen Sie sicher, dass Sie [diese Anforderung](https://technet.microsoft.com/library/cc786468.aspx)erfüllen.

Um das Anmeldeinformationsobjekt die Passwort-Datei zu erstellen, müssen Sie lesen den Inhalt der Datei und eine sichere Zeichenfolge mit [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx)konvertieren.

[Set-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495117.aspx) -Cmdlet nimmt auch einen *Ablaufdatum* Parameter **DateTime** gibt an, zu der das Benutzerkonto abläuft. Beispielsweise kann das Konto abläuft Tagen vom aktuellen Datum und Uhrzeit festlegen.

Diese PowerShell-Beispiel zeigt, wie die Remote Desktop-Erweiterung für einen Cloud-Dienst festlegen:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Sie können auch die Bereitstellung Steckplatz und Rollen Sie Remotedesktop aktivieren möchten. Wenn dieser Parameter nicht angegeben sind, kann das Cmdlet Remotedesktop auf allen Rollen in der **Produktion** Bereitstellung Steckplatz.

Die Remote Desktop-Erweiterung ist einer Bereitstellung zugeordnet. Erstellen einer neue Bereitstellung für den Dienst müssen Sie Remotedesktop diese Bereitstellung aktivieren. Möchten Sie immer Remotedesktop aktiviert ist, dann sollten Sie PowerShell-Skripts in Ihre Bereitstellungsworkflow integrieren.


## <a name="remote-desktop-into-a-role-instance"></a>Remote Desktop in einer Instanz der Rolle
Das Cmdlet " [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) " ist eine spezifische Rolleninstanz den Clouddienst mit Remotedesktop verwendet. *LocalPath* -Parameter können Sie die RDP-Datei lokal herunterladen. Oder können Sie den Parameter *Starten* Dialogfeld Remotedesktopverbindung Zugriff auf die Cloud Service Rolleninstanz direkt starten.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Überprüfen Sie, ob Remotedesktop-Erweiterung für einen Dienst aktiviert ist
Das Cmdlet " [Get-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495261.aspx) " anzeigt, Remotedesktop aktiviert oder deaktiviert eine Service-Bereitstellung. Das Cmdlet gibt den Benutzernamen für remote desktop Benutzer und Rollen, denen für die remote desktop-Erweiterung aktiviert ist. In diesem Fall auf die Bereitstellung, und können den staging-Steckplatz verwenden.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Remote Desktop-Erweiterung von einem Dienst entfernen
Wenn Sie bereits remote desktop-Erweiterung Bereitstellung aktiviert und remote desktop Einstellungen aktualisieren müssen, zunächst entfernen Sie die Erweiterung. Und mit der neuen Einstellung erneut aktivieren. Wenn z. B. ein neues Kennwort für das Remotebenutzerkonto festlegen möchten oder das Konto ist abgelaufen. Dies muss auf vorhandene Installationen mit der remote Desktop-Erweiterung aktiviert. Neue Bereitstellung können Sie einfach die Erweiterung direkt anwenden.

Um die Bereitstellung remote desktop-Erweiterung entfernen, können Sie das Cmdlet " [Remove-AzureServiceRemoteDesktopExtension](https://msdn.microsoft.com/library/azure/dn495280.aspx) ". Sie können auch die Bereitstellung Steckplatz und Rolle remote desktop-Erweiterung entfernen soll.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

>[AZURE.NOTE] Rufen Sie zum vollständigen entfernen die Erweiterung Konfiguration *Entfernen* Cmdlet mit dem Parameter **UninstallConfiguration** .
>
>Der Parameter **UninstallConfiguration** deinstalliert alle konfiguriert, die an den Dienst angewendet wird. Jede Erweiterung Konfiguration ist die Konfiguration zugeordnet. Aufrufen des *Entfernen* -Cmdlets ohne **UninstallConfiguration** <mark>Bereitstellung</mark> von Erweiterung Konfiguration trennt so effektiv die Erweiterung entfernen. Die Erweiterung Konfiguration bleibt jedoch dem Dienst zugeordnet.



## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Cloud-Dienste konfigurieren](cloud-services-how-to-configure.md)
