<properties
    pageTitle="Erstellen einer virtuellen Skalieren mithilfe des Azure-Portals | Microsoft Azure"
    description="Bereitstellen Sie Skalierung Sätze Azure-Portal."
    keywords="virtuelle Maschine Maßstab legt" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Erstellen einer virtuellen Skalieren mithilfe des Azure-Portals

Dieses Lernprogramm zeigt Ihnen, wie einfach es ist, Virtual Machine Skalierung festlegen in wenigen Minuten mithilfe des Azure-Portals. Haben Sie ein Azure-Abonnement ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Wählen Sie die VM-Image vom marketplace

Über das Portal können Sie problemlos einen Maßstab CentOS, CoreOS, Debian, Open Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu-Server oder Windows Server-Abbildern bereitstellen.

Navigieren Sie zuerst zum [Azure-Portal](https://portal.azure.com) in einem Webbrowser. Klicken Sie auf `New`, suchen Sie nach `scale set`, und wählen Sie dann die `Virtual machine scale set` Eintrag:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Erstellen der virtuellen Linux-Maschine

Sie können die Standardeinstellungen verwenden und erstellen den virtuellen Computer.

* Auf der `Basics` Blade, geben Sie einen Namen für die Skalierung ein. Dieser Name wird die Basis des FQDN Lastenausgleich vor der Skalierung, so stellen Sie sicher, dass der Namen aller Azure eindeutig ist.

* Wählen Sie, die das gewünschte Betriebssystem, geben Sie Ihren gewünschten Benutzernamen, die und wählen Sie die Authentifizierung geben Sie, bevorzugen. Wenn Sie ein Passwort muss mindestens 12 Zeichen lang sein und drei der vier folgenden Komplexität Vorschriften erfüllen: einen Kleinbuchstaben, einen Großbuchstaben enthalten, eine Zahl und ein Sonderzeichen. Mehr über [Benutzername und Kennwort](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Wenn Sie wählen `SSH public key`, Sie nur einfügen in Ihren öffentlichen Schlüssel nicht Ihren privaten Schlüssel:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Geben Sie Ihren gewünschten Namen und Speicherort, und klicken Sie dann auf `OK`.

* Auf der `Virtual machine scale set service settings` Blade: Geben Sie die gewünschte Domäne Bezeichnung (die Basis des FQDN des Lastenausgleichs vor der Skalierung). Diese Bezeichnung muss aller Azure eindeutig sein.

* Wählen Sie die gewünschte Betriebssystem Datenträgerabbild Instanzenzahl und Computer Größe.

* Aktivieren Sie oder deaktivieren Sie skalieren und konfigurieren Sie, wenn aktiviert:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Auf der `Summary` Blade, wenn Prüfung abgeschlossen ist, klicken Sie auf `OK`.

* Schließlich auf die `Purchase` Blade, klicken Sie auf `Purchase` legen Sie die Skalierung zu Bereitstellung.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Verbindung zur VM in den Maßstab

Sobald die Skalierungsgruppe bereitgestellt wird, navigieren Sie zu den `Inbound NAT Rules` Registerkarte Lastenausgleich für die Skalierung festlegen:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Jede VM mit NAT-Regeln festgelegt Skalierung möglich. Beispielsweise für einen Satz Windows Skala liegt eine NAT-Regel auf eingehende Port 50000 konnte Verbindung auf diesem Computer über RDP auf `<load-balancer-ip-address>:50000`. Für eine Gruppe Linux skalieren Sie Verbinden mit dem Befehl `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Nächste Schritte

Dokumentation zur Bereitstellung Maßstab legt von der CLI finden Sie unter [dieser Dokumentation](./virtual-machine-scale-sets-cli-quick-create.md).

Dokumentation Maßstab legt PowerShell bereitstellen finden Sie in [dieser Dokumentation](./virtual-machine-scale-sets-windows-create.md).

Dokumentation Maßstab legt aus Visual Studio bereitstellen finden Sie in [dieser Dokumentation](./virtual-machine-scale-sets-vs-create.md).

Allgemeine Dokumentation finden Sie [Dokumentation Übersichtsseite für Skalierung festgelegt](./virtual-machine-scale-sets-overview.md).

Allgemeine Informationen finden Sie der [Hauptseite für Skalierung legt](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

