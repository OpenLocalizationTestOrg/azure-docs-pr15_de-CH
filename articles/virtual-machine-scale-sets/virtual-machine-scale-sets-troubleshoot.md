<properties
    pageTitle="Problembehandlung beim Skalieren mit virtuellen Skalierung | Microsoft Azure"
    description="Problembehandlung bei Skalieren mit virtuellen skalieren. Verstehen Sie normalerweise Probleme und deren Behebung."
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="guybo"/>

# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Problembehandlung bei Skalieren mit virtuellen skalieren

**Problem** erstellte Infrastruktur Skalierung in Azure-Ressourcen-Manager mit VM skalieren – beispielsweise durch Bereitstellen einer Vorlage wie folgt: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale – der Maßstab Regeln definiert, und es funktioniert hervorragend, egal wie viel Last auf die VMs setzen, Skalieren wird nicht.

## <a name="troubleshooting-steps"></a>Schritte zur Fehlerbehebung

Einige Dinge zu beachten sind:

- Wie viele Kerne besitzt jede VM und Laden Sie jeder Kern?
 Die oben genannte Beispiel Azure Schnellstart Vorlage hat einen do_work.php-Skript, das einen einzelnen Kern lädt. Verwenden Sie einen einzelnen Kern virtueller Speicher wie Standard_A1 oder D1 größer VM müssten Sie diese Last mehrmals ausführen. Überprüfen Sie Anzahl der VMs Kerne [Größen für Windows virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md) überprüfen

- Wie viele VMs in der VM Skalierung festlegen, werden jeweils Arbeit?

    Dezentrales Ereignis nur erfolgt beim Zeitpunkt internen Regeln skalieren Durchschnitt CPU **Alle** VMs in einer Skala den Schwellenwert übersteigt.

- Haben Sie keine Skalierung Ereignisse verpasst?

    Überprüfen Sie die Überwachungsprotokolle in Azure-Portal für Veranstaltungen. Vielleicht gibt es war Maßstab und Maßstab, schlugen. Sie können "Skalieren" filtern...

    ![Audit-Protokolle][audit]

- Unterscheiden der Skala in und Skalierung Schwellenwerte ausreichend?

    Angenommen Sie, eine Regel skalieren, wenn durchschnittliche CPU größer als 50 % 5 Minuten und Dezimalstellen wird durchschnittliche CPU weniger als 50 % festlegen. Dies würde "Flügelschlagen" Problem, wenn CPU-Auslastung mit Aktionen ständig vergrößern und Verkleinern der Menge dieser Schwellenwert ist. Aus diesem Grund versucht Autoscale Service verhindern "mit" welche als nicht skalieren können. Vergewissern Sie sich daher Ihre skalieren und Skalierung in Schwellenwerte unterscheiden sich ausreichend Speicherplatz zwischen skalieren können.

- Haben Sie eine eigene Vorlage JSON geschrieben?

    Es ist einfach, Fehler, so beginnen mit einer Vorlage wie eine über die bewährte arbeiten und kleine ändert. 

- Skalieren Sie können manuell oder?

    Versuchen Sie, Erneutes VM festlegen Ressource mit einer anderen "Kapazität" mehrere VMs manuell ändern. Eine Beispielvorlage dazu hier: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – bearbeiten die Vorlage stellen Sie sicher, dass die Computer genauso wie Ihre Skalierung festlegen müssen. Wenn Sie die Anzahl der VMs erfolgreich manuell ändern können, wissen Sie, dass das Problem isoliert skalieren.

- Überprüfen Sie Ihre Microsoft.Compute/virtualMachineScaleSet und Microsoft.Insights in der [Azure-Ressourcen-Explorer](https://resources.azure.com/)

    Dies ist zur Problembehandlung unentbehrlich zeigt den Status Ihrer Azure-Ressourcen-Manager-Ressourcen. Klicken Sie auf Ihr Abonnement und Problembehandlung Ressourcengruppe betrachten. Unter dem Ressourcenanbieter Compute die VM Maßstab erstellte Gruppe betrachten und die Instanzansicht zeigt den Status der Bereitstellung überprüfen. Überprüfen Sie die Instanzansicht VMs in der VM Skalierung auch. Der Ressourcenanbieter Microsoft.Insights Ins und gut skalieren Regeln überprüfen.

- Die Diagnose Erweiterung arbeitet und Ausgeben von Daten?

    __Aktualisierung:__ Azure skalieren wurde verbessert, um eine hostbasierte Metriken Pipeline verwenden die diagnoseerweiterung installiert werden nicht mehr benötigt. Dies bedeutet die nächsten Absätzen nicht mehr gelten, wenn eine automatische Skalierung Anwendung neue Pipeline erstellen. Beispiele für Azure Vorlagen mit Host-Pipeline umgewandelt wurden: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale, https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. 

    Hostbasierte Metriken für Skalieren mit empfiehlt sich aus folgenden Gründen:

    - Transfers als keine Diagnose Extensions installiert werden müssen.
    - Einfacher Vorlagen. Eine Skalierung festlegen Vorlage nur Einblicke Autoscale Regeln hinzufügen.
    - Zuverlässiger reporting und schnellere Einführung neuer VMs.

    Die einzigen Gründe Diagnose Erweiterung verwenden möchten wäre Memory Diagnostics reporting/Skalierung benötigen. Hostbasierte Metriken meldet Speicher nicht.

    Vor diesem Hintergrund nur folgen Sie den Rest dieses Artikels Wenn Sie diagnostische Extensions noch für die automatische Skalierung verwenden

    Automatische Skalierung in Azure-Ressourcen-Manager arbeiten können (aber nicht mehr) durch eine VM Erweiterung Diagnoseerweiterung bezeichnet. Es gibt Leistungsdaten ein Speicherkonto in der Vorlage definieren. Diese Daten werden dann von Azure Überwachungsdienst aggregiert.

    Wenn Insights-Dienst Daten von VMs lesen kann, soll mailen Sie – z. B. wenn wurden die VMs, so überprüfen Ihre e-Mail-Adresse (die eine Angabe das Azure-Konto erstellen).

    Sie können auch gehen und betrachten Sie die Daten selbst. Betrachten Sie mit Cloud Explorer Azure Storage-Konto. Zum Beispiel mithilfe von [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2)anmelden und Azure-Abonnement wählen Sie verwenden und Diagnose Storage-Kontonamen in die Diagnose Definition in Ihrer Bereitstellungsvorlage verwiesen...

    ![Cloud Explorer][explorer]

    Hier sehen Sie, eine Reihe von Tabellen, die Daten aus jeder VM gespeichert ist. Suchen Sie unter Linux und CPU-Metrik als Beispiel, letzten Zeilen. Visual Studio Cloud Explorer unterstützt eine Abfragesprache Ausführen eine Abfrage wie "Timestamp Gt Datetime'2016-02-02T21:20:00Z'", stellen Sie sicher, dass die neuesten Ereignisse (UTC ist angenommen). Werden die Daten im angezeigten es entsprechen den Regeln Skalierung einrichten? Im folgenden Beispiel gestartet CPU für 20 Computer in den letzten 5 Minuten auf 100 % erhöht.

    ![Tabellen][tables]

    Wenn die Daten nicht vorhanden ist, impliziert dies, dass das Problem Diagnose Erweiterung im virtuellen Computer ausgeführt. Wenn Daten vorhanden ist, impliziert dies gibt es ein Problem mit Skalierung Regeln oder Einblicke Service. Prüfen Sie [Azure Status](https://azure.microsoft.com/status/).

    Einmal diese Schritte gewesen, wenn weiterhin beim Skalieren Sie versuchen die Foren unter [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp)oder [Stapelüberlauf Probleme](http://stackoverflow.com/questions/tagged/azure), oder Protokollieren eines. Bereiten Sie die Vorlage und eine Ansicht der Daten.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
