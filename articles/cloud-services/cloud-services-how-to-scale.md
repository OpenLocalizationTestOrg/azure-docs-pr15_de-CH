<properties
    pageTitle="Automatische Skalierung einen Cloud-Dienst im Portal | Microsoft Azure"
    description="(klassisch) Erfahren Sie, wie mit dem Verwaltungsportal automatische Skalierung für einen Cloud-Dienst Web oder Arbeitskraft Rolle in Azure konfigurieren."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>Automatisches Skalieren einer Cloud-Dienst

> [AZURE.SELECTOR]
- [Azure-portal](cloud-services-how-to-scale-portal.md)
- [Azure-Verwaltungsportal](cloud-services-how-to-scale.md)

Auf der Seite skalieren des klassischen Azure-Portal können manuell Skalieren der Webrolle oder Worker-Rolle und automatische Skalierung basierend auf CPU-Auslastung oder einer Nachrichtenwarteschlange können.

>[AZURE.NOTE] Dieser Artikel konzentriert sich auf Cloud Web- und Workerrollen Rollen. Wenn Sie einen virtuellen Computer (klassisch) direkt erstellen, ist es in einem Clouddienst gehostet. Einige dieser Informationen gilt für diese Typen von virtuellen Maschinen. Skalierung einen Satz Verfügbarkeit virtueller Maschinen ist wirklich nur sie ein- und Ausschalten wird anhand der Skala Regeln, die Sie konfigurieren. Weitere Informationen zu virtuellen Maschinen und Verfügbarkeit finden Sie unter [Verwalten der Verfügbarkeit von virtuellen Computern](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

Vor dem Konfigurieren der Anwendung skalieren, berücksichtigen Sie der folgenden Informationen:

- Skalierung von Core Verwendung betroffen. Größere Instanzen verwenden mehr Kerne. Sie können Ihr Abonnement eine Anwendung nur innerhalb der Kerne skalieren. Beispielsweise hat Ihr Abonnement maximal 20 Adern und Ausführen eine Anwendung mit zwei mittlere Cloud-Dienste (insgesamt vier Kerne), können nur skalieren andere Cloud Service-Bereitstellung in Ihrem Abonnement von 16 Kerne. Finden Sie weitere Informationen zu Größen [Cloud Service Größen](cloud-services-sizes-specs.md) .

- Erstellen einer Warteschlange, und eine Rolle zuordnen, bevor Skalierung einer Anwendung basiert auf einem Schwellenwert. Weitere Informationen finden Sie unter [Verwenden der Warteschlangendienst Speicher](../storage/storage-dotnet-how-to-use-queues.md).

- Sie können Ressourcen skalieren, die mit dem Cloud-Dienst. Weitere Informationen zum Verknüpfen von Ressourcen finden Sie unter [wie: eine Ressource mit einem Clouddienst verknüpfen](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Um die hohe Verfügbarkeit Ihrer Anwendung zu ermöglichen, sollten mit zwei oder mehr Instanzen bereitgestellt wird. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).



## <a name="schedule-scaling"></a>Planen Sie skalieren

Standardmäßig führen alle Rollen keinen bestimmten Zeitplan. Daher gelten alle Einstellungen für alle Uhrzeiten und alle Wochentage im Laufe des Jahres. Wenn Sie möchten, können Sie die manuelle oder automatische Skalierung einrichten:

- Wochentage
- Wochenenden
- Nächte Woche
- Woche morgens
- Datum
- Bestimmte Datumsbereiche

Dies ist Conigured im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf der  
**Cloud-Services** > **\[der Cloud-Dienst\]** > **Skalieren** > **\[Produktion oder Staging\] ** Seite.

Klicken Sie **aus eingerichtet** für jede Rolle, die Sie ändern möchten.

![Cloud Service automatische Skalierung basierend auf einem Zeitplan][scale_schedules]



## <a name="manual-scale"></a>Manuelle Skalierung

Auf der Seite **Skalieren** können Sie manuell erhöhen oder verringern die Anzahl der Instanzen in einem Clouddienst ausgeführt. Dies wird für jeden von Ihnen erstellten Zeitplan oder ständig konfiguriert, wenn Sie einen Plan erstellt haben.

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf **Clouddienste**und klicken Sie dann auf den Namen des Cloud-Dienst, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Ihr Cloud-Dienst nicht angezeigt wird, müssen Sie aus der **Produktion** oder **Staging** ändern.

2. Klicken Sie auf **Skalieren**.

3. Wählen Sie den Zeitplan Skalierungsoptionen für ändern möchten. Standardmäßig *keine geplanten Zeiten* haben keine Zeitpläne definiert.

4. **Skalierung von Metrik** Abschnitt, und wählen Sie ****. Dies ist die Standardeinstellung für alle Rollen.

5. Jede Rolle im Cloud-Dienst hat einen Schieberegler zum Ändern der Anzahl der Instanzen verwenden.

    ![Manuelles Skalieren einer Cloud Service-Rolle][manual_scale]

    Benötigen Sie weitere Instanzen müssen Sie zum Ändern der [Größe des virtuellen Computers Cloud-Dienst](cloud-services-sizes-specs.md).

6. Klicken Sie auf **Speichern**.  
Instanzen werden hinzugefügt oder entfernt auf Ihre Auswahl.

>[AZURE.TIP] Wann immer ![][tip_icon] bewegen Sie die Maus, und erhalten Sie Hilfe zu eine bestimmte Einstellung.


## <a name="automatic-scale---cpu"></a>Automatische Skalierung - CPU

Wenn Prozent CPU-Auslastung über oder angegebenen Schwellenwerte skaliert; Instanzen werden erstellt oder gelöscht.

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf **Clouddienste**und klicken Sie dann auf den Namen des Cloud-Dienst, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Ihr Cloud-Dienst nicht angezeigt wird, müssen Sie aus der **Produktion** oder **Staging** ändern.

2. Klicken Sie auf **Skalieren**.

3. Wählen Sie den Zeitplan Skalierungsoptionen für ändern möchten. Standardmäßig *keine geplanten Zeiten* haben keine Zeitpläne definiert.

4. **Skalierung von Metrik** Abschnitt, und wählen Sie **CPU**.

5. Jetzt können Sie einen minimalen und maximalen Bereich Rolleninstanzen Ziel-CPU-Auslastung (auf eine Skala von auslösen) und wie viele Instanzen von oben und unten skaliert.

![CPU-Auslastung ein Cloud-Dienst Rollen skalieren][cpu_scale]

>[AZURE.TIP] Wann immer ![][tip_icon] bewegen Sie die Maus, und erhalten Sie Hilfe zu eine bestimmte Einstellung.





## <a name="automatic-scale---queue"></a>Automatische Skalierung - Warteschlange

Skaliert automatisch die Anzahl der Nachrichten in einer Warteschlange geht über oder unter einen bestimmten Schwellenwert; Instanzen werden erstellt oder gelöscht.

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf **Clouddienste**und klicken Sie dann auf den Namen des Cloud-Dienst, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Ihr Cloud-Dienst nicht angezeigt wird, müssen Sie aus der **Produktion** oder **Staging** ändern.

2. Klicken Sie auf **Skalieren**.

3. **Skalierung von Metrik** Abschnitt, und wählen Sie **CPU**.

4. Jetzt können Sie einen minimalen und maximalen Bereich Rolleninstanzen Warteschlange und Warteschlangen Nachrichten für jede Instanz und wie viele Instanzen von oben und unten skaliert verarbeitet.

![Skalieren einer Cloud Service Rolle einer Warteschlange][queue_scale]

>[AZURE.TIP] Wann immer ![][tip_icon] bewegen Sie die Maus, und erhalten Sie Hilfe zu eine bestimmte Einstellung.


## <a name="scale-linked-resources"></a>Verknüpfte Ressourcen skalieren

Häufig beim Skalieren einer Rolle ist es vorteilhaft, die Datenbank, die die Anwendung auch skalieren. Wenn Sie Cloud-Dienst die Datenbank verknüpfen, können Sie durch Klicken auf den entsprechenden Link Skalierung Einstellungen für diese Ressource zugreifen.

1. Im [klassischen Azure-Portal](https://manage.windowsazure.com/)auf **Clouddienste**und klicken Sie dann auf den Namen des Cloud-Dienst, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Ihr Cloud-Dienst nicht angezeigt wird, müssen Sie aus der **Produktion** oder **Staging** ändern.

2. Klicken Sie auf **Skalieren**.

3. **Verknüpfte Ressourcen** Abschnitt und **für diese Datenbank**auf Verwalten klicken.

    > [AZURE.NOTE] Wenn einen **Verknüpfte Ressourcen** -Abschnitt nicht angezeigt wird, werden alle verknüpften Ressourcen wahrscheinlich nicht.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
