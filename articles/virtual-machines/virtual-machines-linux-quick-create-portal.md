<properties
    pageTitle="Erstellen einer Linux VM mithilfe der Azure-Portal | Microsoft Azure"
    description="Erstellen einer Linux VM mithilfe der Azure-Portal."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="vlivech"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"
/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="10/25/2016"
    ms.author="v-livech"
/>

# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Erstellen Sie Linux VM auf Azure über das Portal


Dieser Artikel beschreibt, wie mit der [Azure-Portal](https://portal.azure.com/) virtuellen Linux-Maschine.

Die Vorschriften sind:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

- [SSH öffentliche und private Dateien](virtual-machines-linux-mac-create-ssh-keys.md)


1. Azure-Portal mit der Azure-Konto angemeldet, klicken Sie auf **+ neu** in der oberen linken Ecke:

    ![bildschirm1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

2. Klicken Sie auf **virtuellen Maschinen** auf dem **Markt** **Verfügbare Apps** **Ubuntu Server 14.04 LTS** Liste Bilder.  Am Ende überprüfen Sie, ob das Bereitstellungsmodell `Resource Manager` und klicken Sie dann auf **Erstellen**.

    ![(screen2)](../media/virtual-machines-linux-quick-create-portal/screen2.png)

3. Geben Sie auf der Seite **Grundlagen** :
    - einen Namen für den virtuellen Computer
    - ein Benutzername für den Administrator
    - Legen Sie den Authentifizierungstyp **öffentlichen SSH** -Schlüssel
    - der öffentliche SSH-Schlüssel als Zeichenfolge (aus der `~/.ssh/` Verzeichnis)
    - eine Ressource Gruppennamen oder Auswählen einer vorhandenen Gruppe

    und klicken Sie auf **OK** , um fortzufahren und die VM-Größe wählen Es sollte wie folgt aussehen:

    ![screen3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

4. **DS1** Größe, die Ubuntu Premium SSD installiert und auf Einstellungen **auswählen** .

    ![screen4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

5. **Einstellungen**übernehmen Sie die Standardeinstellungen für Speicher und Netzwerk, und klicken Sie auf **OK** , um die Zusammenfassung anzuzeigen.  Beachten Sie den Datenträgertyp Premium SSD mit DS1 festgelegt wurde, **S** briefzitat SSD.

    ![screen5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

6. Bestätigen Sie die Einstellung für die neue Ubuntu VM, und klicken Sie auf **OK**.

    ![screen6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

7. Öffnen Sie das Dashboard Portal und wählen Sie **Netzwerkschnittstellen** die NIC

    ![screen7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

8. Menü öffentliche IP-Adressen unter der NIC-Einstellung

    ![screen8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

9. SSH in öffentliche IP-Adresse, die mit Ihrem öffentlichen SSH-Schlüssel

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Nächste Schritte

Jetzt haben Sie eine Linux VM schnell zu Test- oder Demonstrationszwecken verwenden erstellt. Erstellen einer Linux VM für Ihre Infrastruktur angepasst, folgen Sie diesen Artikel.

- [Erstellen einer Linux-VM auf Azure mithilfe von Vorlagen](virtual-machines-linux-cli-deploy-templates.md)
- [Erstellen einer SSH gesichert Linux VM in Azure mithilfe von Vorlagen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
- [Erstellen einer Linux VM mit der Azure-CLI](virtual-machines-linux-create-cli-complete.md)
