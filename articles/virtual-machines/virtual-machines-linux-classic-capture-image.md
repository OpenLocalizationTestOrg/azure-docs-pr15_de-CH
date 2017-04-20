<properties
    pageTitle="Zeichnen Sie ein Abbild einer Linux VM | Microsoft Azure"
    description="Erfahren Sie, wie eine Aufnahme einer Linux-basierten Azure Virtual Machine (VM mit dem klassischen Bereitstellungsmodell erstellt)."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="iainfou"/>


# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Eine klassische virtuellen Linux-Maschine als Bild erfassen

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Erfahren Sie, wie [diese Schritte mit dem Ressourcen-Manager-Modell](virtual-machines-linux-capture-image.md).

Dieser Artikel veranschaulicht das klassische Azure VM unter Linux als Bild zu anderen virtuellen Computern erfassen. Betriebssystem-Datenträger und Datenträger angefügten virtuellen Computer enthält. Netzwerkkonfiguration auch müssen Sie konfigurieren, die beim Erstellen von anderen virtuellen Computern aus dem Bild.

Azure speichert das Bild unter **Bilder**und Bilder, die Sie hochgeladen haben. Weitere Informationen über Bilder finden Sie unter [über VM in Azure] [].

## <a name="before-you-begin"></a>Bevor Sie beginnen

Diese Schritte setzen voraus, da Sie bereits einen Azure virtuellen Computer mit dem klassischen Bereitstellungsmodell erstellt und konfiguriert das Betriebssystem hängen alle Datenträger. Benötigen Sie einen virtuellen Computer erstellen, lesen Sie [zum Erstellen einer virtuellen Linux-Maschine] [].


## <a name="capture-the-virtual-machine"></a>Der virtuelle Computer erfassen

1. [Verbinden mit dem virtuellen Computer](virtual-machines-linux-mac-create-ssh-keys.md) mit einem SSH-Client Ihrer Wahl.

2. Geben Sie folgenden Befehl im Fenster SSH. Die Ausgabe von `waagent` leicht variieren je nach Version dieses Dienstprogramms:

    `sudo waagent -deprovision+user`

    Dieser Befehl versucht, die Anlage und für eine erneute. Dieser Vorgang führt folgende Aufgaben aus:

    - Hostschlüssel SSH entfernt (bei Provisioning.RegenerateSshHostKeyPair 'y' in der Konfigurationsdatei)
    - Löscht Konfiguration des Nameserver in /etc/resolv.conf
    - Entfernt die `root` Benutzerkennwort aus/Etc/Shadow (wenn Provisioning.DeleteRootPassword 'y' in der Konfigurationsdatei ist)
    - Entfernt zwischengespeicherten DHCP-Clientleases
    - Setzt Hostnamen localhost.localdomain ein
    - Löscht den letzten bereitgestellten Benutzer Konto (erhältlich von /var/lib/waagent) **und die zugeordneten Daten**.

    >[AZURE.NOTE] Entfernung löscht Dateien und Daten "das Abbild verallgemeinern". Führen Sie diesen Befehl nur auf einem virtuellen Computer, den Sie als neue Vorlage Bild aufzeichnen möchten. Sie gewährleistet jedoch nicht, dass das Bild aller vertraulichen Daten gelöscht oder Weitergabe an Dritte für.


3. Geben Sie **y** fort. Sie können die `-force` Parameter dieser Bestätigungsschritt zu.

4. Geben Sie **Exit** SSH-Client geschlossen.

    >[AZURE.NOTE] Die restlichen Schritte Angenommen Sie bereits [Azure-CLI](../xplat-cli-install.md) auf dem Clientcomputer installiert. Die folgenden Schritte können auch im [klassischen Azure-Portal] []erfolgen.

5. Öffnen Sie Ihre Client-Computer Azure CLI und bei Azure-Abonnement. Lesen Sie [mit Azure-Abonnement von Azure-CLI](../xplat-cli-connect.md).

6. Vergewissern Sie sich im Service-Modus sind:

    `azure config mode asm`

7. Fahren Sie den virtuellen Computer, der bereits in den vorherigen Schritten mit hat:

    `azure vm shutdown <your-virtual-machine-name>`

    >[AZURE.NOTE] Hier finden Sie alle virtuellen Computer, die mit Ihrem Abonnement erstellt`azure vm list`

8. Wenn der virtuelle Computer angehalten wird, Aufnahme mit dem Befehl:

    `azure vm capture -t <your-virtual-machine-name> <new-image-name>`

    Geben Sie Bild anstelle _Neuer Bildname_möchten. Dieser Befehl erstellt ein generalisiertes Betriebssystemabbild. Die `-t` Unterbefehl löscht den ursprünglichen virtuellen Computer.

9.  Das neue Bild steht nun in der Liste der Bilder, die mit jeder neuen virtuellen Computer konfigurieren. Sie können sie mit dem Befehl anzeigen:

    `azure vm image list`

    [Azure-Verwaltungsportal] []wird es in **der Liste** angezeigt.

    ![Erfolgreiche Aufnahme](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)


## <a name="next-steps"></a>Nächste Schritte
Das Bild kann verwendet werden, um virtuelle Computer erstellen. Den Azure-CLI-Befehl können `azure vm create` und den Abbildnamen erstellen. Informationen zum Befehl finden Sie unter [Verwendung der Azure-CLI mit klassischen Bereitstellungsmodell](../virtual-machines-command-line-tools.md) . Alternativ [Azure-Verwaltungsportal] [] erstellen einen benutzerdefinierten virtuellen Computer **Aus Galerie** Methode erstellte Bild auswählen. Weitere Informationen finden Sie in [einem benutzerdefinierten virtuellen Computer erstellen] [] .

**Siehe auch:** [Azure Linux-Agent-Benutzerhandbuch](virtual-machines-linux-agent-user-guide.md)

[Azure-Verwaltungsportal]: http://manage.windowsazure.com
[Bilder in Azure Virtual Machine]: virtual-machines-linux-classic-about-images.md
[Erstellen Sie einen benutzerdefinierten virtuellen Computer]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]: virtual-machines-windows-classic-attach-disk.md
[Zum Erstellen einer virtuellen Linux-Maschine]: virtual-machines-linux-classic-create-custom.md
