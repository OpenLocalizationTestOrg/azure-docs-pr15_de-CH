<properties
    pageTitle="Verallgemeinern von Windows VHD | Microsoft Azure"
    description="Erfahren Sie mit Sysprep generalize Windows VM mit dem Ressourcen-Manager-Bereitstellungsmodell."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="cynthn"/>
    
    
    
    
# <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Einen Windows-Computer mithilfe von Sysprep generalize

In diesem Abschnitt wird veranschaulicht, wie der virtuellen Windows-Maschine zur Verwendung als Abbild verallgemeinern. Sysprep entfernt alle Ihre persönlichen Kontoinformationen, unter anderem und bereitet den Computer als ein Bild verwendet werden soll. Weitere Informationen zu Sysprep finden Sie [wie mit Sysprep: Einführung](http://technet.microsoft.com/library/bb457073.aspx).

Stellen Sie sicher, dass auf dem Computer ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Unterstützung von Sysprep für Serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

>[AZURE.IMPORTANT] Wenn Sie Sysprep vor dem Hochladen der virtuellen Festplatte in Azure zum ersten Mal ausgeführt werden, sorgen Sie [vorbereitet VM](virtual-machines-windows-prepare-for-upload-vhd-image.md) vor dem Ausführen von Sysprep. 

1. Melden Sie sich bei Windows Virtual Machine.

2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie zu **%windir%\system32\sysprep**, und führen Sie `sysprep.exe`.

3. Wählen Sie im Dialogfeld **Systemvorbereitungsprogramm** **System geben Sie Out-of-Box-Experience (OOBE aus)**und sicherstellen Sie, dass das Kontrollkästchen **verallgemeinern** .

4. Wählen Sie unter **Optionen für Herunterfahren** **Herunterfahren**.

5. Klicken Sie auf **OK**.

    ![Sysprep starten](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

6. Nach Abschluss von Sysprep heruntergefahren sie den virtuellen Computer. 

## <a name="next-steps"></a>Nächste Schritte

- Wenn die VM lokal ist, können Sie jetzt [die virtuelle Festplatte in Azure hochladen](virtual-machines-windows-upload-image.md).
- Wenn die VM bereits in Azure ist, können Sie jetzt [Erstellen Sie ein Abbild von der allgemeinen VM](virtual-machines-windows-capture-image.md).