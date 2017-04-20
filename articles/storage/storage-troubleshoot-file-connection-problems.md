<properties
    pageTitle="Problembehandlung bei Azure Datei speichern | Microsoft Azure"
    description="Problembehandlung bei Azure-Datei speichern"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="genli"/>

# <a name="troubleshooting-azure-file-storage-problems"></a>Problembehandlung bei Azure-Datei speichern

Dieser Artikel beschreibt Probleme, die beim Verbinden von Windows- und Linux-Clients mit Microsoft Azure Dateispeicher zusammenhängen. Darüber hinaus mögliche Ursachen und Lösungsmöglichkeiten für diese Probleme.

**Allgemeine Probleme (auf Windows- und Linux-Clients)**

- [Quote-Fehler beim Öffnen einer Datei](#quotaerror)

- [Wenn Sie Azure Dateispeicher von Windows oder Linux langsam](#slowboth)

**Windows-Client-Probleme**

- [Geringe Leistung, wenn Windows 8.1 oder Windows Server 2012 R2 Azure Dateispeicher zugreifen](#windowsslow)

- [Fehler 53 Versuch einer Dateifreigabe Azure bereitzustellen](#error53)

- [NET verwenden war erfolgreich, aber der Azure Dateifreigabe im Windows Explorer nicht angezeigt](#netuse)

- [Meine Speicherkonto enthält "/" und die Net Befehl fehlschlägt](#slashfails)

- [Meine Anwendungsdienst kann nicht bereitgestelltes Dateien Azure Laufwerk zugegriffen werden.](#accessfiledrive)

- [Zusätzliche Informationen zum Optimieren der Leistung](#additional)

**Linux-Client Probleme**

- [Fehler "Sie kopieren eine Datei an ein Ziel, das Verschlüsselung nicht unterstützt" beim Hochladen/Kopieren von Dateien in Azure-Dateien](#encryption)

- [Fehler in Datei "Host ist nicht betriebsbereit" Aktien oder die Shell hängt bei der Liste Befehle auf den Mount-Punkt](#errorhold)

- [Mount-Fehler beim Mount Azure Dateien auf Linux VM 115](#error15)

- [Linux VM zufällige Verzögerungen Befehle wie "ls"](#delayproblem)

## <a name="general-problems"></a>Allgemeine Probleme
<a id="quotaerror"></a>
### <a name="quota-error-when-trying-to-open-a-file"></a>Quote-Fehler beim Öffnen einer Datei

In Windows werden Fehlermeldungen angezeigt, die den folgenden ähneln:

**1816 ERROR_NOT_ENOUGH_QUOTA <> – 0xc0000044**

**STATUS_QUOTA_EXCEEDED**

**Dieser Befehl steht nicht genügend Quoten**

**Ungültigen Handlewert GetLastError: 53**

Unter Linux werden Fehlermeldungen, die den folgenden ähneln:

**<filename>[verweigert]**

**Datenträgerkontingent überschritten**

#### <a name="cause"></a>Ursache

Das Problem tritt auf, weil maximal gleichzeitig geöffneten Handles erreicht haben, die für eine Datei zulässig sind.

#### <a name="solution"></a>Lösung

Reduzieren Sie die Anzahl der gleichzeitig geöffneten Handles schließen einige Handles und wiederholen. Weitere Informationen finden Sie unter [Microsoft Azure Storage Performance und Skalierung Prüfliste](storage-performance-checklist.md).

<a id="slowboth"></a>
### <a name="slow-performance-when-accessing-file-storage-from-windows-or-linux"></a>Geringe Leistung, wenn Windows oder Linux Dateispeicher zugreifen

- Bestimmte e/a-Größe Mindestanforderung haben, sollten Sie 1 MB als e/a-Größe für optimale Leistung verwenden.

- Wenn Sie kennen die endgültige Größe einer Datei, die Sie mit schreibt erweitert Ihre Software hat Kompatibilitätsprobleme, wenn noch schriftlich Ende auf Datei mit Nullen, legen Sie die Dateigröße im Voraus anstatt jedes zu erweiternde schreiben.

## <a name="windows-client-problems"></a>Windows-Client-Probleme
<a id="windowsslow"></a>
### <a name="slow-performance-when-accessing-the-file-storage-from-windows-81-or-windows-server-2012-r2"></a>Geringe Leistung beim Zugriff auf den Dateispeicher von Windows 8.1 oder Windows Server 2012 R2

Kunden, die Windows 8.1 oder Windows Server 2012 R2 ausgeführt werden, stellen Sie sicher, dass der Hotfix [KB3114025](https://support.microsoft.com/kb/3114025) installiert ist. Dieser Hotfix verbessert erstellen und Leistung Handle zu schließen.

Führen Sie das folgende Skript überprüfen, ob der Hotfix installiert wurde:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Wenn Hotfix installiert ist, wird die folgende Ausgabe angezeigt:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies**

**{96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0 x 1**

> [AZURE.NOTE]  Windows Server 2012 R2 Bilder in Azure Marketplace haben den Hotfix KB3114025 standardmäßig ab Dezember 2015.

<a id="additional"></a>
### <a name="additional-recommendations-to-optimize-performance"></a>Zusätzliche Informationen zum Optimieren der Leistung

Nie erstellen oder Öffnen einer Datei für zwischengespeicherte e/a, die Schreibzugriff jedoch nicht lesen Zugriff anfordert. Also wenn Sie **CreateFile()**aufrufen, nicht nur **GENERIC_WRITE**angeben, aber geben immer **GENERIC_READ | GENERIC_WRITE**. Lesegeschützten Handle kann nicht kleine Schreibvorgänge lokal zwischenspeichern, auch wenn es nur offene Handles für die Datei. Zulässige Leistungseinbußen auf kleine Schreibvorgänge. Beachten Sie, dass der "a" Modus CRT **fopen()** lesegeschützten Handle geöffnet.

<a id="error53"></a>
### <a name="error-53-when-you-try-to-mount-or-unmount-an-azure-file-share"></a>"Fehler 53" beim Versuch, aktivieren oder Deaktivieren einer Azure-Dateifreigabe

Dieses Problem kann folgende Ursachen haben:

#### <a name="cause-1"></a>Ursache 1

"Systemfehler 53 ist aufgetreten. Zugriff verweigert." Aus Sicherheitsgründen werden Verbindungen zu Azure Dateifreigaben blockiert, wenn der Kommunikationskanal ist nicht verschlüsselt und der Verbindungsversuch nicht aus demselben Datenzentrum auf dem Azure Dateifreigaben befinden. Verschlüsselung der Kommunikation Channel ist nicht verfügbar, wenn der Client des Benutzers OS SMB-Verschlüsselung unterstützt. Dies wird durch ein "Systemfehler 53 ist aufgetreten. Zugriff wird verweigert"Fehlermeldung, wenn ein Benutzer versucht, eine Dateifreigabe lokal oder von einem anderen Rechenzentrum bereitzustellen. Windows 8, Windows Server 2012 und späteren Versionen jeder Anforderung aushandeln, die Verschlüsselung unterstützt SMB 3.0 enthält.

#### <a name="solution-for-cause-1"></a>Lösung für Ursache 1

Verbindung von einem Client an Windows 8, Windows Server 2012 oder höher entspricht oder Verbinden mit einem virtuellen Computer, die auf demselben Datenzentrum Azure Storage-Konto verwendet wird, für die Azure-Dateifreigabe.

#### <a name="cause-2"></a>Ursache 2

"Systemfehler 53" Wenn Sie bereitstellen möglich Azure Dateifreigabe blockierten Port 445 ausgehende Kommunikation Rechenzentrum für Azure-Dateien. Klicken Sie [hier](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx) die Zusammenfassung der Internetdienstanbieter anzuzeigen, die zulassen oder verbieten des Zugriffs von Port 445.

Comcast und einige IT-Organisationen blockieren diesen Port. Um zu verstehen, ob dies der Grund für die Meldung "Systemfehler 53" ist, können Sie Portqry Endpunkt TCP:445 Abfragen. TCP:445 Endpunkt als gefiltert angezeigt wird, wird der TCP-Port blockiert. Hier ist eine Beispielabfrage:

`g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Wenn eine Regel auf den Netzwerkpfad TCP 445 blockiert, wird die folgende Ausgabe angezeigt:

**TCP-Port 445 (Microsoft-ds-Dienst): gefiltert**

Weitere Informationen zur Verwendung von Portqry finden Sie in der [Beschreibung des Befehlszeilenprogramms Portqry.exe](https://support.microsoft.com/kb/310099).

#### <a name="solution-for-cause-2"></a>Lösung für Ursache 2

Arbeiten Sie mit Ihrer IT-Organisation auf Port 445 ausgehende [Azure IP-Bereiche](https://www.microsoft.com/download/details.aspx?id=41653).

#### <a name="cause-3"></a>Ursache 3

"Systemfehler 53" auch erhalten, wenn NTLMv1 Kommunikation auf dem Client aktiviert ist. NTLMv1 aktiviert mit erstellt einen weniger sicheren Client. Daher werden Kommunikation Azure Dateien blockiert. Um festzustellen, ob dies die Ursache des Fehlers ist, stellen Sie sicher, dass der Registrierungsunterschlüssel Wert 3 ist:

HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel.

Weitere Informationen finden Sie im Thema ["LMCompatibilityLevel"](https://technet.microsoft.com/library/cc960646.aspx) auf TechNet.

#### <a name="solution-for-cause-3"></a>Lösung für Ursache 3

Zum Beheben dieses Problems zurücksetzen Sie "LMCompatibilityLevel" unter dem Registrierungsschlüssel HKLM\SYSTEM\CurrentControlSet\Control\Lsa auf den Standardwert 3.

Azure-Dateien unterstützt nur NTLMv2-Authentifizierung. Stellen Sie sicher, dass Gruppenrichtlinien auf die Clients angewendet. Dadurch wird dieser Fehler verhindern. Dies gilt auch aus Sicherheitsgründen werden. Weitere Informationen finden Sie unter [wie Clients NTLMv2 konfigurieren mithilfe von Gruppenrichtlinien](https://technet.microsoft.com/library/jj852207(v=ws.11).aspx)

Die empfohlene Einstellung ist **nur NTLMv2-Antworten**. Ein Registrierungswert 3 entspricht. Clients verwenden nur die NTLMv2-Authentifizierung, und sie verwenden NTLMv2 sitzungssicherheit vom Server unterstützt. Domänencontroller akzeptieren LM-, NTLM- und NTLMv2-Authentifizierung.

<a id="netuse"></a>
### <a name="net-use-was-successful-but-dont-see-the-azure-file-share-mounted-in-windows-explorer"></a>NET Verwendung war erfolgreich, aber der Azure Dateifreigabe im Windows Explorer nicht angezeigt

#### <a name="cause"></a>Ursache

Standardmäßig wird Windows Explorer nicht als Administrator ausgeführt. Bei Ausführung von **net verwenden** aus einer verbinden Sie "als"Administrator. Da Netzlaufwerke benutzerzentrierte sind, zeigt das angemeldeten Benutzerkonto keine Laufwerke sollten sie unter einem anderen Benutzerkonto.

#### <a name="solution"></a>Lösung

Bereitstellen der Freigabe in einer Befehlszeile ohne Administratorrechte. Alternativ können Sie [Dieses Thema TechNet](https://technet.microsoft.com/library/ee844140.aspx) konfigurieren Sie den Registrierungswert **EnableLinkedConnections** folgen.

<a id="slashfails"></a>
### <a name="my-storage-account-contains--and-the-net-use-command-fails"></a>Meine Speicherkonto enthält "/" und die Net Befehl fehlschlägt

#### <a name="cause"></a>Ursache

Wenn der Befehl **net Use** unter Befehlszeile (cmd.exe) ausgeführt wird, wird sie analysiert durch Hinzufügen von "/" als Befehlszeilenoption. Dadurch wird die Zuordnung des Laufwerks fehlschlägt.

#### <a name="solution"></a>Lösung

Die folgenden Schritte können Sie das Problem umgehen:

• Verwenden Sie den folgenden PowerShell-Befehl:

`New-SmbMapping -LocalPath y: -RemotePath \\server\share  -UserName acountName -Password "password can contain / and \ etc"`

Aus einer Batchdatei kann als dies

`Echo new-smbMapping ... | powershell -command –`

• Legen Sie doppelte Anführungszeichen um die Taste, um dieses Problem zu umgehen, es sei denn, "/" ist das erste Zeichen. Ist es, verwenden Sie den interaktiven Modus und geben Sie Ihr Kennwort separat oder Regenerieren Sie der Schlüssel zu einem Schlüssel, der nicht mit dem Schrägstrich (/) beginnt.

<a id="accessfiledrive"></a>
### <a name="my-applicationservice-cannot-access-mounted-azure-files-drive"></a>Meine Anwendungsdienst kann nicht bereitgestelltes Azure Laufwerk zugegriffen werden.

#### <a name="cause"></a>Ursache

Laufwerke werden pro Benutzer bereitgestellt. Wenn Ihre Anwendung oder ein Dienst unter einem anderen Benutzerkonto ausgeführt wird, sehen Benutzer nicht das Laufwerk.

#### <a name="solution"></a>Lösung

Stellen Sie Laufwerk über dasselbe Benutzerkonto, unter dem die Anwendung ist. Dies kann mithilfe von Tools wie Psexec.

Alternativ können Sie einen neuen Benutzer mit den gleichen Berechtigungen wie das Netzwerkkonto Dienst oder ein System erstellen und **Cmdkey** und **net verwenden** unter diesem Konto führen. Der Benutzername sollte der Storage-Kontoname und Kennwort sollte Speicherschlüssel Konto. Eine weitere Option für **net Verwendung** ist die Storage-Kontonamen und die Parameter für Name und Kennwort von den Befehl **net Use** .

Nachdem Sie diese Hinweise erhalten Sie folgende Fehlermeldung: "1312 Systemfehler. Eine angegebenen Sitzung ist nicht vorhanden. Es kann bereits wurden beendet"beim Ausführen von **net verwenden** für das System-Netzwerk ein. In diesem Fall sicher, dass Benutzernamen an **net verwenden** Domäneninformationen enthält (z. B.: "[speicherkontonamen]. file.core.windows .net").

## <a name="linux-client-problems"></a>Linux-Client Probleme

<a id="encryption"></a>
### <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fehler "Sie eine Datei an ein Ziel, das keine Verschlüsselung unterstützt kopieren"

#### <a name="cause"></a>Ursache

BitLocker verschlüsselte Dateien können in Azure Dateien kopiert. Der Dateispeicher unterstützt NTFS EFS jedoch nicht. Daher verwenden Sie EFS wahrscheinlich bei. Haben Sie Dateien, die mit EFS verschlüsselt wurden, kann ein Kopiervorgang der Datenspeicher fehl, wenn der Kopierbefehl kopierte Datei entschlüsseln.

#### <a name="workaround"></a>Abhilfe

Um eine Datei der Datenspeicher müssen Sie ihn zunächst entschlüsseln. Sie können dazu eine der folgenden Methoden:

• Verwenden Sie **Copy/d**.

• Legen Sie den folgenden Registrierungsschlüssel:

- Pfad = HKLM\Software\Policies\Microsoft\Windows\System
- Typ = DWORD
- Name = CopyFileAllowDecryptedRemoteDestination
- Wert = 1

Beachten Sie jedoch, dass der Registrierungsschlüssel wirkt sich auf alle Kopiervorgänge in Netzwerkfreigaben.

<a id="errorhold"></a>
### <a name="host-is-down-error-on-existing-file-shares-or-the-shell-hangs-when-you-run-list-commands-on-the-mount-point"></a>Fehler in Datei "Host ist nicht betriebsbereit" Aktien oder die Shell hängt beim Ausführen von Liste Befehle auf den Mount-Punkt

#### <a name="cause"></a>Ursache

Dieser Fehler tritt auf dem Linux-Client, wenn der Client für einen längeren Zeitraum im Leerlauf. Tritt dieser Fehler auf, der Client die Verbindung trennt und die Clientverbindung Timeout.

#### <a name="solution"></a>Lösung

Dieses Problem wurde im Rahmen der [Änderungssatz](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)ausstehende Backport in Linux-Distribution Linux-Kernel behoben.

Aufrechterhalten der Verbindungs, umgeht dieses Problem zu vermeiden, Leerlauf und in Azure-Dateifreigabe, der Sie regelmäßig schreiben eine Datei beibehalten. Dies muss ein Schreibvorgang wie umschreiben erstellte/geänderte Datum der Datei. Andernfalls zwischengespeicherten Ergebnisse erhalten, und der Vorgang kann nicht ausgelöst werden, die Verbindung.

<a id="error15"></a>
### <a name="mount-error-115-when-you-try-to-mount-azure-files-on-the-linux-vm"></a>"Fehler 115 bereitgestellt" bei dem Versuch, Dateien auf die Linux VM Azure bereitstellen

#### <a name="cause"></a>Ursache

Linux-Distributionen können noch Verschlüsselung SMB 3.0 nicht. Einige Distributionen erhalten Benutzer "115" Fehlermeldung, wenn sie Mount Azure Dateien versuchen, mithilfe von SMB 3.0 durch eine fehlende Funktion.

#### <a name="solution"></a>Lösung

Wenn Linux SMB-Client, der verwendet wird, keine Verschlüsselung unterstützt, Dateien Mount Azure mit SMB 2.1 Linux VM auf das Rechenzentrum Speicherkonto Datei.

<a id="delayproblem"></a>
### <a name="linux-vm-experiencing-random-delays-in-commands-like-ls"></a>Linux VM zufällige Verzögerungen Befehle wie "ls"

#### <a name="cause"></a>Ursache

Dies kann auftreten, wenn der Befehl Mount **Serverino** Option nicht enthalten ist. Ohne **Serverino**führt den Befehl ls **Stat** auf jede Datei.

#### <a name="solution"></a>Lösung

Überprüfen Sie die **Serverino** in den Eintrag "/ Etc/Fstab":

azureuser.File.Core.Windows.NET/WMS/comer in/Home/Sampledir Typ Cifs (Rw, Nodev, Relatime, vers 2.1 = s = Ntlmssp Cache = strikte Benutzername = Xxx, Domäne = X File_mode = 0755, Dir_mode = 0755, Serverino, Rsize = 65536 "wsize" = 65536, Actimeo = 1)

Wenn die Option **Serverino** nicht vorhanden ist, heben Sie die Bereitstellung und Azure Dateien wieder mit der **Serverino** -Option.

## <a name="learn-more"></a>Weitere Informationen

- [Erste Schritte mit Windows Azure Dateispeicher](storage-dotnet-how-to-use-files.md)

- [Erste Schritte mit Azure Dateispeicher auf Linux](storage-how-to-use-files-linux.md)
