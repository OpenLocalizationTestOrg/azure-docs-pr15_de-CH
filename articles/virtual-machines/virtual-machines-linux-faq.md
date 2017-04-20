<properties
    pageTitle="Häufig gestellte Fragen zu Linux VMs | Microsoft Azure"
    description="Antworten auf häufige Fragen virtuelle Linux-Computer mit dem Ressourcen-Manager-Modell erstellt."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="cynthn"/>

# <a name="frequently-asked-question-about-linux-virtual-machines"></a>Häufig gestellte Fragen über virtuelle Linux-Computer

Dieser Artikel behandelt einige häufig gestellte Fragen über virtuelle Linux-Computer mit dem Ressourcen-Manager-Bereitstellungsmodell Azure erstellt. Die Windows-Version dieses Themas finden Sie unter [häufig gestellte Fragen zu Windows virtuelle Maschinen](virtual-machines-windows-faq.md)

## <a name="what-can-i-run-on-an-azure-vm"></a>Was kann ich auf Azure-VM ausführen?

Alle Abonnenten können Server-Software auf einem virtuellen Computer Azure ausführen. Weitere Informationen finden Sie unter [Linux auf Azure-Endorsed Verteilung](virtual-machines-linux-endorsed-distros.md)


## <a name="how-much-storage-can-i-use-with-a-virtual-machine"></a>Wie viel Speicher kann ich mit einem virtuellen Computer verwenden?

Jeder Datenträger kann bis zu 1 TB. Die Anzahl der Datenträger, die Sie verwenden können, hängt von der Größe des virtuellen Computers ab. Einzelheiten finden Sie [Größen für virtuelle Computer](virtual-machines-linux-sizes.md).

Ein Azure Storage-Konto bietet Speicher für Betriebssystem-Datenträger und alle Datenträger. Jedes Laufwerk ist eine VHD-Datei als Seitenblob gespeichert. Preise, [Preise Speicherdetails](https://azure.microsoft.com/pricing/details/storage/)anzeigen


## <a name="how-can-i-access-my-virtual-machine"></a>Wie kann der virtuelle Computer zugreifen?

Herstellen einer Remoteverbindung mit Secure Shell (SSH) virtuellen Computer anmelden. Finden Sie die Anleitung zum Verbinden [von Windows](virtual-machines-linux-ssh-from-windows.md) oder [Linux und Mac](virtual-machines-linux-mac-create-ssh-keys.md). SSH kann bis zu 10 gleichzeitige Zugriffe. Die Konfigurationsdatei bearbeiten, um diese Anzahl erhöhen.


Wenn Probleme [Beheben Secure Shell (SSH)](virtual-machines-linux-troubleshoot-ssh-connection.md)Anschlüsse überprüfen.


## <a name="can-i-use-the-temporary-disk-devsdb1-to-store-data"></a>Kann ich die temporäre Diskette (/ Dev/sdb1) zum Speichern von Daten verwenden?

Verwenden Sie nicht die temporäre Diskette (/ Dev/sdb1) zum Speichern von Daten. Es ist nur für die temporäre Speicherung. Sie verlieren Daten wiederhergestellt werden können.


## <a name="can-i-copy-or-clone-an-existing-azure-vm"></a>Kann kopieren oder Klonen einer vorhandenen Azure VM?

Ja. Eine Anleitung finden Sie unter [Erstellen einer Kopie einer virtuellen Linux-Maschine in der Ressourcen-Manager-Bereitstellungsmodell](virtual-machines-linux-copy-vm.md).


## <a name="why-am-i-not-seeing-canada-central-and-canada-east-regions-through-azure-resource-manager"></a>Warum nicht angezeigt Kanada Central und Kanada Osten durch Azure-Ressourcen-Manager?

Die beiden neuen Bereiche Deutschland und Kanada Osten sind für Erstellung virtueller Computer für vorhandene Azure-Abonnements nicht automatisch registriert. Diese Registrierung erfolgt automatisch, wenn Azure Portal auf alle anderen Azure-Ressourcen-Manager mit ein virtuellen Computer bereitgestellt wird. Nach der Bereitstellung einer virtuellen Maschine in Azure Bereich sollte die Bereiche für nachfolgende virtuelle Computer verfügbar sein.


## <a name="can-i-add-a-nic-to-my-vm-after-its-created"></a>Kann ich hinzufügen eine Netzwerkkarte Meine VM nach seiner Erstellung?

Nein. Hinzufügen einer Netzwerkkarte kann nur zur Erstellungszeit erfolgen.


## <a name="are-there-any-computer-name-requirements"></a>Gibt es Computer Name?

Ja. Die Computername kann maximal 64 Zeichen lang sein. Benennen Ihre Ressourcen Weitere Informationen finden Sie unter [Infrastruktur Benennungsrichtlinien](virtual-machines-linux-infrastructure-naming-guidelines.md) .


## <a name="what-are-the-username-requirements-when-creating-a-vm"></a>Was sind erforderlichen Benutzernamen beim Erstellen einer VM

Benutzernamen müssen 1 bis 64 Zeichen lang sein.

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

Kennwörter müssen 6 bis 72 Zeichen und 3 4 Komplexität Folgendes erfüllen:

- Niedrigere Zeichen
- Obere Zeichen
- Eine Ziffer
- Haben Sie ein Sonderzeichen (Regex übereinstimmen [\W_])

Die folgenden Kennwörter sind nicht zulässig:

<table>
    <tr>
        <td style="text-align:center">abc@123</td>
        <td style="text-align:center">P@$$w0rd</td>
        <td style="text-align:center">P@ssw0rd</td>
        <td style="text-align:center">P@ssword123</td>
        <td style="text-align:center">PA$ $word</td>
    </tr>
    <tr>
        <td style="text-align:center">pass@word1</td>
        <td style="text-align:center">Kennwort!</td>
        <td style="text-align:center">Kennwort1</td>
        <td style="text-align:center">Password22</td>
        <td style="text-align:center">ILOVEYOU!</td>
    </tr>
</table>
