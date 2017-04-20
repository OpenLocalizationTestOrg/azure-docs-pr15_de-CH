<properties
    pageTitle="Bestimmte RDP-Fehlermeldungen für Azure VMs | Microsoft Azure"
    description="Verstehen Sie Fehlermeldungen, die möglicherweise beim Verwenden Sie Remotedesktopverbindung zu einem Windows-Computer in Azure"
    keywords="Remote desktop-Fehler, Fehler Remotedesktopverbindung kann keine Verbindung mit VM Remotedesktop Problembehandlung"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/14/2016"
    ms.author="iainfou"/>

# <a name="troubleshooting-specific-rdp-error-messages-to-a-windows-vm-in-azure"></a>Problembehandlung bei bestimmten RDP-Fehlermeldungen an einen virtuellen Computer Windows Azure
Sie erhalten eine Fehlermeldung bei Verwendung von Remotedesktopverbindung zu einem Windows-Computer (VM) in Azure. Dieser Artikel beschreibt einige der häufigeren Fehlermeldungen und Fehlerbehebung zur Lösung gefunden. Wenn Sie Probleme beim Verbinden mit Ihrer VM mit RDP eine Fehlermeldung nicht auftreten, finden Sie unter [Problembehandlung bei Remotedesktop](virtual-machines-windows-troubleshoot-rdp-connection.md).

Informationen zu bestimmten Fehlermeldungen finden Sie unter:

- [Die Remotesitzung wurde abgebrochen, weil keine Remote Desktop-Lizenzserver eine Lizenz verfügbar sind](#rdplicense).
- [Remote Desktop Computer "Name" kann nicht gefunden werden](#rdpname).
- [Eine Authentifizierung aufgetreten. Die lokale Sicherheitsautorität kann keine Verbindung hergestellt](#rdpauth).
- [Windows Sicherheitsfehler: Ihre Anmeldeinformationen funktioniert nicht](#wincred).
- [Dieser Computer kann keine Verbindung mit dem Remotecomputer](#rdpconnect).

<a id="rdplicense"></a>
## <a name="the-remote-session-was-disconnected-because-there-are-no-remote-desktop-license-servers-available-to-provide-a-license"></a>Die Remotesitzung wurde abgebrochen, weil keine Remote Desktop-Lizenzserver eine Lizenz verfügbar sind.

Ursache: 120-Tage-Kulanzfrist für die Remote Desktop-Serverrolle ist abgelaufen und Sie Lizenzen installieren müssen.

Um dieses Problem zu umgehen speichern Sie eine lokale Kopie der RDP-Datei aus dem Portal und führen Sie diesen Befehl in der Befehlszeile PowerShell Verbindung. Dieser Schritt deaktiviert Lizenzen für nur diese Verbindung:

        mstsc <File name>.RDP /admin

Wenn Sie mehr als zwei gleichzeitige Remotedesktopverbindungen der VM tatsächlich benötigen, können Server-Manager Sie Remote Desktop Serverrolle entfernen.

Weitere Informationen finden Sie im Blogbeitrag [Azure VM schlägt mit "Keine Remote Desktop Lizenzserver verfügbar"](https://blogs.msdn.microsoft.com/mast/2014/01/21/rdp-to-azure-vm-fails-with-no-remote-desktop-license-servers-available/).

<a id="rdpname"></a>
## <a name="remote-desktop-cant-find-the-computer-name"></a>Remote Desktop kann nicht Computer "Name" gefunden werden.

Ursache: Der Name des Computers in den Einstellungen der RDP-Datei kann nicht Remote Desktop Client Computers aufgelöst werden.

Lösungsvorschläge:

- Wenn Sie im Intranet einer Organisation sind, unbedingt Computers Zugriff auf den Proxy-Server und kann HTTPS-Datenverkehr zu senden.

- Wenn Sie eine lokal gespeicherte RDP-Datei verwenden, verwenden Sie die vom Portal erstellt. Dadurch wird sichergestellt, dass Sie den richtigen DNS-Namen für den virtuellen Computer oder Cloud-Dienst und der Endpunktport der VM. Hier ist ein Beispiel-RDP-Datei vom Portal generierte:

        full address:s:tailspin-azdatatier.cloudapp.net:55919
        prompt for credentials:i:1

Adressteil des RDP-Datei enthält:
- Der vollqualifizierte Domänenname des Clouddienst, der die VM ("Tailspin-azdatatier.cloudapp.net" in diesem Beispiel) enthält.

- Den externen TCP-Port des Endpunkts für Remotedesktop Datenverkehr (55919).

<a id="rdpauth"></a>
## <a name="an-authentication-error-has-occurred-the-local-security-authority-cannot-be-contacted"></a>Ein Authentifizierungsfehler aufgetreten. Die lokale Sicherheitsautorität nicht erreichbar.

Ursache: Das Ziel VM kann nicht die Sicherheitsautorität Namensteil Benutzer Ihre Anmeldeinformationen suchen.

Wird Ihr Benutzername in der Form *SecurityAuthority*\\*Benutzername* (Beispiel: CORP\User1), die *SecurityAuthority* ist die VM Computernamen (local Security Authority) oder einen Active Directory-Domänennamen.

Lösungsvorschläge:

- Ist der lokale VM stellen Sie sicher, dass der Name richtig geschrieben ist.

- Wenn das Konto in einer Active Directory-Domäne ist, überprüfen Sie die Schreibweise des Domänennamens.

- Ein Active Directory-Domänenkonto ist und der Domänenname richtig geschrieben ist, sicher, dass ein Domänencontroller in dieser Domäne verfügbar ist. Es ist ein häufiges Problem bei Azure virtuelle Netzwerke, die Domänencontroller enthalten, dass ein Domänencontroller nicht verfügbar ist, da er noch nicht gestartet wurde. Um dieses Problem zu umgehen können Sie statt eines Domänenkontos ein lokales Administratorkonto.

<a id="wincred"></a>
## <a name="windows-security-error-your-credentials-did-not-work"></a>Windows-Sicherheit-Fehler: Ihre Anmeldeinformationen nicht funktioniert.

Ursache: Das Ziel VM kann nicht Ihren Kontonamen und Ihr Kennwort überprüfen.

Ein Windows-Computer kann die Anmeldeinformationen für ein lokales Konto oder ein Domänenkonto überprüfen.

- Verwenden Sie für lokale Konten *ComputerName*\\Syntax für*Benutzernamen* (Beispiel: SQL1\Admin4798).
- Verwenden Sie für Domänenkonten *Domänenname*\\Syntax für*Benutzernamen* (Beispiel: CONTOSO\peterodman).

Wenn Sie die VM auf einen Domänencontroller in einer neuen Active Directory-Gesamtstruktur heraufgestuft haben, wird das lokale Administratorkonto, dem Sie angemeldet entsprechende Konto mit demselben Kennwort in der neuen Gesamtstruktur und Domäne konvertiert. Das lokale Konto wird gelöscht.

Beispielsweise wenn Sie das lokale Konto DC1\DCAdmin angemeldet und dann den virtuellen Computer als Domänencontroller in einer neuen Gesamtstruktur "corp.contoso.com" Domäne heraufgestuft, das lokale DC1\DCAdmin-Konto gelöscht und ein neues Domänenkonto (CORP\DCAdmin) mit demselben Kennwort erstellt.

Stellen Sie sicher, dass der Name ein Name ist, die den virtuellen Computer ein gültiges Konto überprüfen können und das Kennwort korrekt ist.

Wenn Sie das Kennwort für das lokale Administratorkonto ändern möchten, finden Sie unter [Zurücksetzen eines Kennworts oder den Remotedesktopdienst für virtuelle Windows-Maschinen](virtual-machines-windows-reset-rdp.md).

<a id="rdpconnect"></a>
## <a name="this-computer-cant-connect-to-the-remote-computer"></a>Dieser Computer kann keine Verbindung mit dem Remotecomputer.

Ursache: Das Konto, das zum Verbinden Remotedesktop anmelden Rechte keinen.

Jedem Windows-Computer verfügt über einen Remotedesktop lokalen Benutzergruppe, die Konten und Gruppen, die sie Remote anmelden können. Mitglieder der Gruppe der lokalen Administratoren haben außerdem Zugriff, obwohl diese Konten in der Gruppe Remotedesktop Benutzer nicht aufgeführt sind. Für Domäne Computer enthält der lokalen Administratorgruppe Domänenadministratoren für die Domäne.

Stellen Sie sicher, dass das Konto verwendeten Verbindung mit Remotedesktop anmelden Rechte. Um dieses Problem zu umgehen verwenden Sie einer Domäne oder lokale Administratorkonto über Remotedesktop eine Verbindung. Um das gewünschte Konto der Gruppe Remotedesktop Benutzer hinzuzufügen, verwenden Sie das MMC-Snap-in (**Systemprogramme > Lokale Benutzer und Gruppen > Gruppen > Remotedesktopbenutzer**).


## <a name="next-steps"></a>Nächste Schritte
Wenn keiner dieser Fehler aufgetreten ist und ein unbekanntes Problem mit RDP verwenden, finden Sie unter [Problembehandlung bei Remotedesktop](virtual-machines-windows-troubleshoot-rdp-connection.md).

- [Azure Diagnosepaket IaaS (Windows)](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Problembehandlungsschritte zugreifen auf einer VM ausgeführt, finden Sie unter [Problembehandlung bei Zugriff auf eine Anwendung auf eine Azure-VM](virtual-machines-linux-troubleshoot-app-connection.md).
- Bei Problemen mit Secure Shell (SSH) Herstellen einer Linux VM in Azure finden Sie unter [Problembehandlung SSH-Verbindung zu Linux VM in Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).