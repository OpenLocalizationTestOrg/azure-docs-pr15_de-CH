<properties
    pageTitle="Mit der Umleitung in Azure RemoteApp | Microsoft Azure"
    description="Informationen Sie zum Konfigurieren und Verwenden von Umleitung in RemoteApp"
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-redirection-in-azure-remoteapp"></a>Umleitung verwenden in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Lesen Sie die [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) für Details.

Umleitung kann Benutzer mit remote apps über den lokalen Computer, Telefon oder Tablet angeschlossenen Geräte interagieren. Sofern Skype über Azure RemoteApp benötigt der Benutzer beispielsweise die Kamera auf ihrem PC mit Skype arbeiten. Dies gilt auch für Drucker, Lautsprecher, Monitore und Bereich USB angeschlossenen Peripheriegeräte.

RemoteApp nutzt die Umleitung Remote Desktop Protocol (RDP) und RemoteFX.

## <a name="what-redirection-is-enabled-by-default"></a>Welche Umleitung standardmäßig aktiviert?
Bei Verwendung von RemoteApp sind folgende Umleitung standardmäßig aktiviert. Die Informationen in Klammern zeigen die RDP-Einstellung.

- Akustische Signale auf dem lokalen Computer (**auf diesem Computer wiedergeben**). (Audiomode:i:0)
- Audio von dem lokalen Computer und dem Remotecomputer (**Datensatz von diesem Computer**) an. (Audiocapturemode:i:1)
- Drucken Sie auf lokalen Druckern (Redirectprinters:i:1)
- COM-Anschlüsse (Redirectcomports:i:1)
- Smartcard-Geräte (Redirectsmartcards:i:1)
- Zwischenablage (Möglichkeit zum Kopieren und Einfügen) (Redirectclipboard:i:1)
- Typ Schriftarten-Glättung deaktivieren (Schriftarten-Glättung zulassen: I:1)
- Umleiten Sie alle unterstützten Plug & Play-Geräte. (Devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Welche Umleitung verfügbar ist?
Zwei Umleitungsoptionen sind standardmäßig deaktiviert:

- Laufwerkumleitung (Zuordnung zu Laufwerk): die lokalen Laufwerke des Computers werden zugeordnete Laufwerke in der Remotesitzung. Hiermit speichern oder Öffnen von Dateien aus Ihre lokalen Laufwerke während der Arbeit in der Remotesitzung.
- USB-Umleitung: USB-Gerät auf Ihrem lokalen Computer in der Remotesitzung verwenden.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Ändern Sie in RemoteApp Umleitung
Sie können die Geräteumleitungseinstellungen für eine Auflistung mit Microsoft Azure PowerShell SDK ändern. Nach der Installation von neuen PowerShell und SDK zunächst konfigurieren Sie, um Ihr Abonnement zu verwalten, wie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)beschrieben.

Dann verwenden Sie einen Befehl ähnlich dem folgenden benutzerdefinierten RDP-Eigenschaften festlegen:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Hinweis:, *und* als Trennzeichen zwischen einzelnen Eigenschaften verwendet wird.)

Um eine Liste der benutzerdefinierten RDP Eigenschaften konfiguriert sind, führen Sie das folgende Cmdlet. Beachten Sie, dass benutzerdefinierte Eigenschaften nicht die Standardeigenschaften sowie Ergebnisse angezeigt werden:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

Wenn Sie benutzerdefinierte Eigenschaften festlegen, müssen Sie alle benutzerdefinierte Eigenschaften jedes Mal angeben. Andernfalls wird die Einstellung deaktiviert.   

### <a name="common-examples"></a>Beispiele
Verwenden Sie das folgende Cmdlet laufwerkumleitung aktivieren:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*”

Verwenden Sie dieses Cmdlet USB- und Laufwerk aktivieren Umleitung:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Verwenden Sie dieses Cmdlet gemeinsame Nutzung der Zwischenablage deaktivieren:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0”

> [AZURE.IMPORTANT] Unbedingt vollständig Abmelden aller Benutzer in der Auflistung (und nicht nur trennen) bevor Sie die Änderung testen. Um sicherzustellen, dass Benutzer vollständig abgemeldet sind, wechseln Sie zu dem Register **Sessions** in der Auflistung im Azure-Portal und melden Sie Benutzer getrennt oder angemeldet. Manchmal dauert es mehrere Sekunden die lokalen Laufwerke während der Sitzung im Explorer angezeigt.

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Einstellungen Sie USB-Umleitung auf dem Windows-client

Möchten Sie USB-Umleitung auf einem Computer verwenden, die mit RemoteApp verbindet, sind 2 Aktionen auftreten. 1 - Systemadministrator muss USB-Umleitung auf Auflistungsebene mithilfe von Azure PowerShell aktivieren. 2 - auf jedes USB-Umleitung verwenden soll, müssen Sie eine Gruppenrichtlinie aktivieren, die dies zulässt. Diesen Schritt müssen für jeden Benutzer erfolgen, die USB-Umleitung verwenden möchte.

> [AZURE.NOTE] USB-Umleitung Azure RemoteApp wird nur für Windows-Computer unterstützt.

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>USB-Umleitung für RemoteApp-Auflistung
Verwenden Sie das folgende Cmdlet USB-Umleitung auf Auflistungsebene zu aktivieren:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>USB-Umleitung für Client-computer

Auf USB-Umleitung auf Ihrem Computer zu konfigurieren:

1. Öffnen Sie den Editor für lokale Gruppenrichtlinien (GPEDIT. MSC). (Führen Sie gpedit.msc aus.)
2. **Computer Benutzerkonfiguration\Richtlinien\Administrative Vorlagen\Windows-Components\Remote Komponenten\Remotedesktopdienste\Remotedesktop Remotedesktopverbindung Client\RemoteFX USB-Umleitung**zu öffnen.
3. Doppelklicken Sie auf **RDP können Umleitung von anderen unterstützten RemoteFX USB-Geräte vom Computer**.
4. Wählen Sie **aktiviert**, und wählen Sie **Administratoren und Benutzern die Zugriffsrechte RemoteFX USB-Umleitung**.
5. Öffnen Sie ein Eingabeaufforderungsfenster mit Administratorrechten, und führen Sie den folgenden Befehl:

        gpupdate /force
6. Starten Sie den Computer neu.

Sie können auch das Gruppenrichtlinien-Tool Richtlinie erstellen und anwenden die USB-Umleitung für alle Computer in der Domäne:

1. Der Domänencontroller als Domänenadministrator anmelden.
2. Öffnen Sie die Gruppenrichtlinien-Verwaltungskonsole. (Klicken Sie auf **Start > Verwaltung > Group Policy Management**.)
3. Navigieren Sie zu der Domäne oder Organisationseinheit, für die Sie die Richtlinie erstellen möchten.
4. Maustaste auf **Default Domain Policy**, und klicken Sie auf **Bearbeiten**.
5. **Computer Benutzerkonfiguration\Richtlinien\Administrative Vorlagen\Windows-Components\Remote Komponenten\Remotedesktopdienste\Remotedesktop Remotedesktopverbindung Client\RemoteFX USB-Umleitung**zu öffnen.
6. Doppelklicken Sie auf **RDP können Umleitung von anderen unterstützten RemoteFX USB-Geräte vom Computer**.
7. Wählen Sie **aktiviert**, und wählen Sie **Administratoren und Benutzern die Zugriffsrechte RemoteFX USB-Umleitung**.
8. Klicken Sie auf **OK**.  
