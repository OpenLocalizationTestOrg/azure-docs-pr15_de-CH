<properties
    pageTitle="Laufwerk D: eines virtuellen Datenträger erstellen | Microsoft Azure"
    description="Beschreibt, wie Laufwerkbuchstaben für eine VM Windows so ändern, dass Sie Laufwerk D: als Datenlaufwerk verwenden können."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>

# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Verwenden Sie Laufwerk D: als Datenlaufwerk auf einen Windows-VM 

Wenn Ihre Anwendung Laufwerk D verwenden, um Daten zu speichern, gehen Sie einen anderen Laufwerkbuchstaben für die temporäre Diskette verwenden. Verwenden Sie niemals die temporäre Diskette zum Speichern von Daten, die Sie beibehalten müssen.

Wenn Sie die Größe oder **Stop (Deallocate)** einen virtuellen Computer kann diese Platzierung des virtuellen Computers auf einen neuen Hypervisor ausgelöst. Ein wartungsereignis geplant oder ungeplant kann ebenfalls diese Platzierung auslösen. In diesem Szenario wird die temporäre Diskette der erste verfügbare Laufwerkbuchstabe zugewiesen werden. Wenn Sie eine Anwendung, die speziell auf Laufwerk D: erfordert haben, müssen Sie gehen vorübergehend pagefile.sys verschieben, fügen einen neuen Datenträger und Zuweisen des Buchstabens D und dann rückwärts pagefile.sys auf dem temporären Laufwerk. Danach Azure nicht wieder D: nehmen die VM in anderen Hypervisor verschiebt.

Weitere Informationen zur Verwendung von Azure auf des temporären Datenträgers finden Sie unter [Understanding temporäre Laufwerk auf Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="attach-the-data-disk"></a>Legen Sie den Datenträger

Zunächst müssen Sie den virtuellen Computer den Datenträger an. 

- Das Portal finden Sie unter [wie Datenträger im Azure-portal](virtual-machines-windows-attach-disk-portal.md)
- Das klassische Portal finden Sie unter [wie einen Datenträger auf einem Windows-Computer](virtual-machines-windows-classic-attach-disk.md). 


## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Verschieben "Pagefile.sys" auf Laufwerk C

1. Verbinden Sie mit dem virtuellen Computer. 

2. Klicken Sie im **Startmenü** und **Wählen**.

3. Wählen Sie im linken Menü **Erweiterte Systemeinstellungen**.

4. **Wählen Sie im Bereich **Performance** .**

5. Wählen Sie die Registerkarte **Erweitert** .

5. Wählen Sie im Bereich **virtueller Arbeitsspeicher** **Ändern**.

6. Wählen Sie Laufwerk **C** , klicken Sie auf **Größe wird vom System verwaltet** , und **Klicken Sie dann auf**.

7. Wählen Sie das Laufwerk **D** klicken Sie **keine Auslagerungsdatei** und **Klicken Sie dann auf**.

8. Klicken Sie auf anwenden. Eine Warnung erhalten, dass der Computer neu gestartet werden, damit die Änderungen wirksam muss werden.

9. Starten Sie den virtuellen Computer neu.




## <a name="change-the-drive-letters"></a>Laufwerkbuchstaben 

1. Wenn die VM neu gestartet wurde, wieder melden Sie die VM an.

2. Klicken Sie auf **das Startmenü** Geben Sie **diskmgmt.msc** , und drücken Sie die EINGABETASTE. Datenträger wird gestartet.

3. Auf **D**Laufwerk vorübergehender Maustaste und wählen Sie **Laufwerkbuchstaben ändern und Pfade**.

4. Wählen Sie unter Laufwerkbuchstaben Laufwerk **G** , und klicken Sie dann auf **OK**. 

5. Mit der rechten Maustaste auf den Datenträger, und wählen Sie **Laufwerksbuchstaben und Pfaden**.

6. Wählen Sie unter Laufwerkbuchstaben Laufwerk **D** , und klicken Sie dann auf **OK**. 

7. Maustaste auf **G**vorübergehender Laufwerk und wählen **Ändern von Laufwerkbuchstaben und Pfaden**.

8. Wählen Sie unter Laufwerkbuchstaben Laufwerk **E** , und klicken Sie dann auf **OK**. 

> [AZURE.NOTE] Verwenden Sie VM andere Festplatten oder Laufwerke verfügt, die gleiche Methode Laufwerkbuchstaben Datenträger und Laufwerke zuweisen. Möchten Sie die Datenträgerkonfiguration zu:  
>- C: Laufwerk OS  
>- D: Datenträger  
>- E: temporärer Speicherplatz



## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Pagefile.sys vorübergehender Laufwerk wechseln 

1. Klicken Sie im **Startmenü** und **Wählen**

2. Wählen Sie im linken Menü **Erweiterte Systemeinstellungen**.

3. **Wählen Sie im Bereich **Performance** .**

4. Wählen Sie die Registerkarte **Erweitert** .

5. Wählen Sie im Bereich **virtueller Arbeitsspeicher** **Ändern**.

6. Wählen Sie das Betriebssystem Laufwerk **C** und klicken Sie auf **keine Auslagerungsdatei** **Klicken**.

7. Wählen Sie temporäre Speicherung Laufwerk **E** und klicken Sie auf **Größe wird vom System verwaltet** und **Klicken Sie dann auf**.

8. Klicken Sie auf **Übernehmen**. Eine Warnung erhalten, dass der Computer neu gestartet werden, damit die Änderungen wirksam muss werden.

9. Starten Sie den virtuellen Computer neu.




## <a name="next-steps"></a>Nächste Schritte
- Verfügbaren Speicher erhöhen Sie Ihre virtuellen Computer durch [zusätzliche Datenträger anfügen](virtual-machines-windows-attach-disk-portal.md).



