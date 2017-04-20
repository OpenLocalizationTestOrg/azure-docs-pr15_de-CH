<properties
    pageTitle="Häufig gestellte Fragen zur Windows-VMs | Microsoft Azure"
    description="Enthält Antworten auf einige häufig gestellte Fragen zu Windows virtuelle Computer mit dem Ressourcen-Manager erstellt."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-windows-virtual-machines"></a>Häufig gestellte Fragen zu Windows virtuelle Maschinen 


Dieser Artikel behandelt einige häufig gestellte Fragen zu Windows Azure mit dem Ressourcen-Manager-Bereitstellungsmodell erstellte Maschinen. Die Linux-Version dieses Themas finden Sie unter [häufig gestellte Frage über virtuelle Linux-Computer](virtual-machines-linux-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich auf Azure-VM ausführen?

Alle Abonnenten können Server-Software auf einem virtuellen Computer Azure ausführen. Informationen der Unterstützungsrichtlinie für Microsoft-Serversoftware in Azure ausgeführt finden Sie unter [Microsoft-Serversoftware Azure virtuelle Computer unterstützen](https://support.microsoft.com/kb/2721672)

Bestimmte Versionen von Windows 7 und Windows 8.1 stehen MSDN Azure nutzen und MSDN-Entwicklung und Test nutzungsbasierte Abonnenten für Entwicklungs-und Testaufgaben. Angaben, Hinweise und Einschränkungen finden Sie in der [Windows-Client-Images für MSDN-Abonnenten](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/). 


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann ich mit einem virtuellen Computer verwenden?

Jeder Datenträger kann bis zu 1 TB. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Einzelheiten finden Sie [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

Ein Azure Storage-Konto bietet Speicher für Betriebssystem-Datenträger und alle Datenträger. Jedes Laufwerk ist eine VHD-Datei als Seitenblob gespeichert. Preise, [Preise Speicherdetails](https://azure.microsoft.com/pricing/details/storage/)anzeigen


## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann der virtuelle Computer zugreifen?

Einrichten einer Remoteverbindung über Remote Desktop-Verbindung (RDP) für eine Windows-VM. Eine Anleitung finden Sie unter [Verbinden und eine unter Windows Azure virtuellen Computer anmelden](virtual-machines-windows-connect-logon.md). Maximal zwei gleichzeitige Verbindungen werden unterstützt, wenn der Server als Host Remotedesktopdienste-Sitzung konfiguriert ist.  


Wenn Sie Probleme mit dem Remotedesktop, finden Sie unter [Problembehandlung bei Remotedesktopverbindungen zu einem Windows-basierten Azure Virtual Machine](virtual-machines-windows-troubleshoot-rdp-connection.md). 

Wenn Sie Hyper-V kennen, können Sie ein Tool wie VMConnect suchen. Azure bietet kein ähnliches Tool, da Konsolenzugriff auf einem virtuellen Computer nicht unterstützt wird.

## <a name="can-i-use-the-temporary-disk-the-d-drive-by-default-to-store-data"></a>Kann ich die temporäre Diskette (Laufwerk D: standardmäßig) zum Speichern von Daten verwenden?

Verwenden Sie die temporäre Diskette nicht zum Speichern von Daten. Ist nur temporäre Speicherung Daten wiederhergestellt werden können vollständig verlieren würden. Datenverlust kann auftreten, wenn virtuelle Computer auf einen anderen Host verschoben wird. Ändern der Größe einer virtuellen Maschine, sind der Host oder ein Hardwarefehler auf dem Host aktualisieren Gründe, die einen virtuellen Computer verschieben können.

Haben Sie eine Anwendung, die den Laufwerkbuchstaben D: verwenden, können Sie Laufwerkbuchstaben zuweisen, damit als D: temporäre Speicherplatz verwendet. Informationen finden Sie unter [Ändern der Laufwerkbuchstabe des temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="how-can-i-change-the-drive-letter-of-the-temporary-disk"></a>Wie kann der Laufwerkbuchstabe des temporären Datenträgers ändern?

Ändern des Laufwerkbuchstabens Auslagerungsdatei und erneutes Zuordnen von Laufwerkbuchstaben, jedoch müssen Sie sicherstellen, dass Sie die Schritte in einer bestimmten Reihenfolge. Informationen finden Sie unter [Ändern der Laufwerkbuchstabe des temporären Windows-Datenträger](virtual-machines-windows-classic-change-drive-letter.md).

## <a name="can-i-add-an-existing-vm-to-an-availability-set"></a>Kann ich eine vorhandene VM auf Verfügbarkeit hinzufügen?

Nein. Soll die VM Teil eines Verfügbarkeit müssen Sie innerhalb der VM erstellen. Derzeit keine Möglichkeit, einen virtuellen Computer zu einem Verfügbarkeitsbericht festgelegt, nachdem es erstellt wurde.

## <a name="can-i-upload-a-virtual-machine-to-azure"></a>Kann ich einen virtuellen Computer in Azure hochladen?

Ja. Informationen finden Sie in [einem Bild VM Windows Azure hochladen](virtual-machines-windows-upload-image.md)

## <a name="can-i-resize-the-os-disk"></a>Können den Betriebssystem-Datenträger ändern?

Ja. Eine Anleitung finden Sie unter [OS-Laufwerk einer virtuellen Maschine in eine Ressourcengruppe Azure erweitern](virtual-machines-windows-expand-os-disk.md).

## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann kopieren oder Klonen einer vorhandenen Azure VM?

Ja. Eine Anleitung finden Sie unter [Erstellen einer Kopie von einem Windows-Computer in der Ressourcen-Manager-Bereitstellungsmodell](virtual-machines-windows-vhd-copy.md).

## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum nicht angezeigt Kanada Central und Kanada Osten durch Azure-Ressourcen-Manager?

Die beiden neuen Bereiche Deutschland und Kanada Osten sind für Erstellung virtueller Computer für vorhandene Azure-Abonnements nicht automatisch registriert. Diese Registrierung erfolgt automatisch, wenn Azure Portal auf alle anderen Azure-Ressourcen-Manager mit ein virtuellen Computer bereitgestellt wird. Nach der Bereitstellung einer virtuellen Maschine in Azure Bereich sollte die Bereiche für nachfolgende virtuelle Computer verfügbar sein.

## <a name="does-azure-support-linux-vms"></a>Unterstützt Azure Linux VMs?

Ja. Zum Erstellen einer Linux VM ausprobieren finden Sie unter [Erstellen einer Linux VM in Azure über das Portal](virtual-machines-linux-quick-create-portal.md).

## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich hinzufügen eine Netzwerkkarte Meine VM nach seiner Erstellung?

Nein. Hinzufügen einer Netzwerkkarte kann nur zur Erstellungszeit erfolgen.

## <a name="are-there-any-computer-name-requirements"></a>Gibt es Computer Name?

Ja. Die Computername kann maximal 15 Zeichen lang sein. Benennen Ihre Ressourcen Weitere Informationen finden Sie unter [Infrastruktur Benennungsrichtlinien](virtual-machines-windows-infrastructure-naming-guidelines.md) .

## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Was sind erforderlichen Benutzernamen beim Erstellen einer VM

Benutzernamen können maximal 20 Zeichen lang sein und darf nicht mit einem Punkt enden ("."). 

Die folgenden Benutzernamen sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">Administrator </td><td style="text-align:center"> Admin </td><td style="text-align:center"> Benutzer </td><td style="text-align:center"> Benutzer1</td>
    </tr>
    <tr>
        <td style="text-align:center">Testen </td><td style="text-align:center"> Benutzer2 </td><td style="text-align:center"> Test1 </td><td style="text-align:center"> Benutzer3</td>
    </tr>
    <tr>
        <td style="text-align:center">admin1 </td><td style="text-align:center"> 1 </td><td style="text-align:center"> 123 </td><td style="text-align:center"> ein</td>
    </tr>
    <tr>
        <td style="text-align:center">actuser  </td><td style="text-align:center"> ADM </td><td style="text-align:center"> Admin2 </td><td style="text-align:center"> ASPNET</td>
    </tr>
    <tr>
        <td style="text-align:center">Sicherung </td><td style="text-align:center"> Konsole </td><td style="text-align:center"> David </td><td style="text-align:center"> Gast</td>
    </tr>
    <tr>
        <td style="text-align:center">John </td><td style="text-align:center"> Besitzer </td><td style="text-align:center"> Stamm </td><td style="text-align:center"> Server</td>
    </tr>
    <tr>
        <td style="text-align:center">SQL </td><td style="text-align:center"> Unterstützung </td><td style="text-align:center"> Support_388945a0 </td><td style="text-align:center"> Sys</td>
    </tr>
    <tr>
        <td style="text-align:center">Test2 </td><td style="text-align:center"> . 3 </td><td style="text-align:center"> User4 </td><td style="text-align:center"> user5</td>
    </tr>
</table>

## <a name="what-are-the-password-requirements-when-creating-a-vm"></a>Was die Kennwortrichtlinien sind beim Erstellen einer VM

Kennwörter müssen 8 123 Zeichen und 3 4 Komplexität Folgendes erfüllen:

- Niedrigere Zeichen
- Obere Zeichen
- Eine Ziffer
- Haben Sie ein Sonderzeichen (Regex übereinstimmen [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">abc@123</td><td style="text-align:center">P@$$w0rd</td><td style="text-align:center">P@ssw0rd</td><td style="text-align:center">P@ssword123</td><td style="text-align:center">PA$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td><td style="text-align:center">Kennwort!</td><td style="text-align:center">Kennwort1</td><td style="text-align:center">Password22</td><td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
