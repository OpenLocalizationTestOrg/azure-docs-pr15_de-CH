#<a name="how-to-create-a-custom-virtual-machine"></a>Erstellen Sie einen benutzerdefinierten virtuellen Computer

Ein *benutzerdefinierten* virtuellen Computer bezieht sich auf einen virtuellen Computer **Aus Galerie** -Methode verwenden, da es mehr Optionen als die Methode **Schnell erstellen** kann. Diese Optionen umfassen Folgendes:

- Weitere Optionen für das Bild mit dem virtuellen Computer (VM)
- Die VM Verbinden mit einem virtuellen Netzwerk
- Einen vorhandenen Cloud-Dienst hinzufügen die VM
- Hinzufügen der VM auf Verfügbarkeit

> [AZURE.IMPORTANT] Soll der virtuelle Computer ein virtuelles Netzwerk verwenden Sie Hostnamen oder einrichten standortübergreifende Verbindung herstellen können, stellen Sie sicher, dass Sie das virtuelle Netzwerk beim Erstellen des virtuellen Computers. Eine virtuelle Maschine kann konfiguriert werden, um ein virtuelles Netzwerk nur beim Erstellen des virtuellen Computers. Weitere Informationen über virtuelle Netzwerke finden Sie in [Azure Virtual Network Overview](http://go.microsoft.com/fwlink/p/?LinkID=294063).

1. Mit der [Azure-Portal](http://manage.windowsazure.com)anmelden.

2. Klicken Sie auf der Befehlsleiste auf **neu**.

3. Klicken Sie auf **berechnen**, klicken Sie auf **virtuelle Computer**und klicken Sie dann auf **Aus Galerie**.

4. Wählen Sie das Bild, das Sie verwenden möchten, und klicken Sie dann auf den Pfeil, um fortzufahren.

5. Sind mehrere Versionen eines Bilds für **Veröffentlichungsdatum**, wählen Sie die Version, die Sie verwenden möchten.

6. **Name des virtuellen Computers**Geben Sie, die Sie für den virtuellen Computer verwenden möchten.

7. Verwenden Sie **Tier** und **Größe** auf die entsprechende Größe für den virtuellen Computer. Die gewählte Größe wirkt sich auf die maximale Konfiguration der virtuellen Computer sowie die Preise. Details zur Konfiguration finden Sie unter [virtueller Computer und Azure Cloud Service Größe](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. **Neuen Benutzernamen ein**Geben Sie einen Namen für das Administratorkonto, das Sie zur Verwaltung des Servers verwenden möchten.

9. Geben Sie im Feld **Neues Kennwort**ein sicheres Kennwort für das Administratorkonto ein. Geben Sie im Feld **Kennwort bestätigen**dasselbe Kennwort.

10. Klicken Sie auf den Pfeil, um fortzufahren.

11. Folgendermaßen Sie in **Cloud-Dienst**vor:

    - Dies ist die erste oder einzige virtuelle Maschine im Cloud-Dienst wählen Sie **erstellen einen neuen Cloud-Dienst aus** **Cloud Service DNS-Name**Geben Sie einen Namen zwischen 3 und 24 Kleinbuchstaben und Zahlen verwendet. Dieser Name wird Teil der URI, der an den virtuellen Computer durch Cloud-Dienst verwendet wird.
    - Wenn ein Cloud-Dienst auf diesem virtuellen Computer hinzugefügt, wählen sie in der Liste.

    > [AZURE.NOTE] Weitere Informationen über das Platzieren von virtuellen Maschinen im selben Cloud-Dienst finden Sie unter [virtuelle Computer in einem Clouddienst herstellen](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. **Region-Affinität Gruppe und virtuellen Netzwerk**wählen Sie, Gruppe, virtuelles Netzwerk für den virtuellen Computer verwenden möchten. Weitere Informationen zu Gruppen finden Sie unter [über Gruppen für virtuelle Netzwerk](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

13. **Speicherkonto**wählen Sie ein vorhandenes Speicherkonto für die VHD-Datei oder verwenden Sie eine automatisch generierte Speicherkonto. Nur ein Speicherkonto pro Region wird automatisch erstellt. Alle virtuellen Computer, die Sie mit dieser Einstellung befinden sich in diesem Storage-Konto. Sie sind auf 20 Speicherkonten.

14. Wenn Sie den virtuellen Computer auf Verfügbarkeit, **Verfügbarkeit**, gehören wählen Sie **Create Verfügbarkeit** oder hinzufügen zu einem vorhandenen Verfügbarkeit.

    **Hinweis**: virtuelle Computer in einem Verfügbarkeit auf anderen Fehlerdomänen bereitgestellt. Platzieren mehrere virtuelle Computer in einer Gruppe wird sichergestellt, dass bei Netzwerkfehlern Festplatte Hardwarefehler und betriebsbedingte Downtime die Anwendung verfügbar ist.

15.  Prüfen Sie unter **Endpunkte**die neuen Endpunkte ermöglicht Verbindungen auf dem virtuellen Computer z. B. über Remotedesktop oder Secure Shell (SSH)-Client erstellt werden. Auch können Sie Endpunkte hinzufügen jetzt oder später erstellen. Informationen zum später erstellen finden Sie unter [Festlegen von Endpunkten einer virtuellen Maschine](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  Entscheiden Sie unter **VM-Agent**, ob der VM-Agent installieren. Dieser Agent stellt Extensions installieren, mit denen Sie interagieren mit den virtuellen Computer die Umgebung. Details finden Sie unter [Manage Extensions](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Klicken Sie auf den Pfeil, um den virtuellen Computer erstellt.

    ![Benutzerdefinierte VM erstellt](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Nächste Schritte##
Nachdem der virtuelle Computer erstellt wurde, wird es automatisch gestartet. Wenn das Portal Status als aktiv angezeigt wird, können Sie mit dem virtuellen Computer anmelden. Informationen finden Sie in den folgenden Artikeln:

- [Anmelden bei einem virtuellen Computer mit Linux](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Anmelden bei einem virtuellen Computer unter WindowsServer](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

