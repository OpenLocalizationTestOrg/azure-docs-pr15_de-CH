<properties
    pageTitle="Übersicht über virtuelle Maschinen von Microsoft Azure Cloud Services und Web Apps skalieren | Microsoft Azure"
    description="Übersicht über automatische Skalierung in Microsoft Azure. Gilt für virtuelle Computer, Cloud-Services und webapps."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Übersicht über virtuelle Maschinen von Microsoft Azure Cloud Services und Web Apps skalieren

Dieser Artikel beschreibt, was Microsoft Azure skalieren ist, ihren Vorteilen und zu verwenden.  

Azure Bildschirm skalieren gilt nur für [Virtual Machine Maßstab legt](https://azure.microsoft.com/services/virtual-machine-scale-sets/) [Clouddienste](https://azure.microsoft.com/services/cloud-services/)und [App Service - Web Apps](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure hat zwei skalieren. Eine ältere Version von Autoscale gilt für virtuelle Computer (Verfügbarkeit Sets). Dieses Feature bietet nur eingeschränkte Unterstützung und Migration zu VM Maßstab legt für schnellere und zuverlässigere Autoscale Unterstützung empfohlen. In diesem Artikel wird eine Verknüpfung zur Verwendung älteren Technologie enthalten.  


## <a name="what-is-autoscale"></a>Was ist skalieren?

Automatische Skalierung können Sie die richtige Menge an Ressourcen um die Auslastung der Anwendung behandeln. Können Ressourcen zum Anwachsen der Last und auch sparen sitzen Ressourcen entfernen hinzufügen im Leerlauf. Eine minimale und maximale Anzahl von Instanzen ausgeführt und hinzufügen oder Entfernen von VMs automatisch basierend auf einem Satz von Regeln angeben. Mit einer minimalen stellt sicher läuft die Anwendung immer noch keine Belastung. Mit einem Gehalt beschränkt des gesamten möglichen stündlich. Automatisches Skalieren Sie dazwischen mit Regeln, die Sie erstellen.

 ![Skalierungsgröße erläutert. Hinzufügen und Entfernen von VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Wenn Regeln erfüllt sind, skalieren Aktionen ausgelöst. Sie können hinzufügen und Entfernen von VMs oder andere Aktionen auszuführen. Das folgende Diagramm veranschaulicht diesen Prozess.  

 ![Grundlegende Autoscale Datenflussdiagramm](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Skalierungsgröße Prozess erläutert
Die folgende Erklärung zu den im vorhergehenden Diagramm anwenden.   

### <a name="resource-metrics"></a>Ressourcenmetriken
Ressourcen Reflektionsausgabe Metriken, die später von Regeln verarbeitet werden. Metriken sind mit unterschiedlichen Methoden.
VM Maßstab legt verwendet Telemetriedaten von Azure Diagnostics Agents Telemetrie für webapps und Cloud-Services direkt aus der Azure-Infrastruktur kommt. Einige häufig verwendeten Statistiken umfassen CPU-Auslastung, Speicherverwendung Thread-Anzahl, Länge und Datenträgerverwendung. Eine Liste der Telemetriedaten verwenden können, finden Sie unter [Skalieren allgemeinen Bewertungsgrundlagen](insights-autoscale-common-metrics.md).

### <a name="time"></a>Zeit
Zeitplan-Regeln basieren auf UTC. Beim Einrichten der Regeln müssen Sie Zeitzone ordnungsgemäß festgelegt.  

### <a name="rules"></a>Regeln
Das Diagramm zeigt nur eine Regel skalieren, jedoch haben viele. Sie können komplexe Regeln erstellen, für Ihre Situation.  Regel gehören  

 - **Metrik-basierten** - beispielsweise, diese Aktion, wenn CPU-Auslastung über 50 % liegt.
 - **Zeitbasierte** - z. B. Trigger Webhook alle 8 am Samstag in einer bestimmten Zeitzone.

Metrik Regeln Anwendung messen und hinzufügen oder Entfernen von VMs auf dieser Basis. Zeitplan-Regeln können skaliert werden, wenn Sie Zeitmuster Auslastungstests und Skalieren vor möglichen ansteigt oder Abnahme auftritt.  


### <a name="actions-and-automation"></a>Aktionen und Automatisierung

Regeln können eine oder mehrere Arten von Aktionen auslösen.

- **Scale** - Skala VMs oder
- **E-Mail** - e-Mail an Administratoren Abonnement, co-Administratoren und Ihnen zusätzliche e-Mail-Adresse senden
- **Automatisieren über Webhooks** - Aufruf Webhooks, die mehrere komplexe Aktionen innerhalb oder außerhalb von Azure auslösen können. In Azure können Sie ein Azure Automation Runbook Azure Funktion oder Azure Logik App starten. Beispiel 3. Partei URL außerhalb Azure Services Pufferzeit und Twilio enthalten.


## <a name="autoscale-settings"></a>Skalierungsgröße Settings
Automatische Skalierung verwenden folgende Terminologie und Struktur.

- Ein **Skalieren Einstellung** wird vom Modul skalieren zu bestimmen, ob nach oben oder unten skalieren gelesen. Es enthält einen Profile, Informationen zur Ressource und Einstellungen.
    - Ein **Skalieren Profil** besteht aus einer Kapazität festlegen, einen Satz von Regeln für Trigger und Aktionen für das Profil und eine Wiederholung. Sie können mehrere Profile überlappende Anforderungen kümmern können.
        - **Kapazität-Einstellung** gibt Minimum, Maximum und Standardwerte für die Anzahl der Instanzen. [geeignet Abb. 1 verwenden]
        - Eine **Regel** enthält einen Trigger-Metrik Trigger oder Trigger Zeit – und eine Aktion, der angibt, ob automatische Skalierung skaliert werden sollen, nach oben oder unten, wenn die Regel erfüllt ist.
        - **Serie** Gibt beim Skalieren Effekt dieses Profil abgelegt. Sie können verschiedene Autoscale Profile für unterschiedliche Tageszeiten oder Wochentage, haben.
- Eine **Benachrichtigung festlegen** definiert, welche Benachrichtigungen sollten Ereignisfall skalieren basierend auf den Kriterien eines Autoscale Einstellung Profile. Skalieren kann eine oder mehrere e-Mail-Adressen informieren oder Aufrufe von mindestens Webhooks.

![Azure Skalieren festlegen, Profil und Regelstruktur](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Die vollständige Liste der konfigurierbaren Felder und Beschreibung steht in [Autoscale REST-API](https://msdn.microsoft.com/library/dn931928.aspx).

Codebeispiele finden Sie unter

* [Erweiterte automatische Skalierung Konfiguration Ressourcenmanager Schablonen für VM skalieren](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Skalierungsgröße REST-API](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Horizontale Vs vertikale Skalierung

Automatische Skalierung erhöht Ressourcen nur skaliert horizontal die steigt ("out") oder Verringerung der Anzahl der VM-Instanzen ("in").  Horizontale Skalierung ist flexibler in einer Cloud-Situation potenziell Tausenden von VMs Last ausgeführt werden können. Vertikale Skalierung unterscheidet. Die gleiche Anzahl von VMs, werden jedoch macht die VM ("up") mehr oder weniger ("down") leistungsfähige. Leistung wird im Arbeitsspeicher, CPU-Geschwindigkeit, Festplattenspeicher gemessen.  Vertikale Skalierung hat mehrere Nachteile. Es hängt von der Verfügbarkeit von größeren Hardware variiert nach Region und schnell trifft und Obergrenze. Vertikale Skalierung erfordert normalerweise auch eine VM beenden und starten. Weitere Informationen finden Sie unter [vertikale Skalierung Azure VM mit Azure Automation](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Zugriffsmethoden
Sie können automatische Skalierung über einrichten

- [Azure-portal](insights-how-to-scale.md)
- [PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Plattformübergreifende Befehlszeilenschnittstelle (CLI)](insights-cli-samples.md#autoscale )
- [Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Unterstützte Dienste für skalieren


| Dienst                              | Schema & Dokumente                                       |
|--------------------------------------|-----------------------------------------------------|
| Webapps                             | [Webapps-Skalierung](insights-how-to-scale.md)              |
| Cloud-Dienste                       | [Skalieren einer Cloud-Dienst](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuelle Maschinen: Klassisch           | [Klassische virtuellen Verfügbarkeit legt skalieren](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuelle Maschinen: Windows Skalierung Sets| [VM-Skalierung Skalierung wird in Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtuelle Maschinen: Linux Maßstab legt fest  | [Wird im Linux VM Skalierung Skalierung](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuelle Maschinen: Beispiel für Windows   | [Erweiterte automatische Skalierung Konfiguration Ressourcenmanager Schablonen für VM skalieren](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Nächste Schritte

Um weitere Informationen zu skalieren Autoscale Walkthroughs zuvor aufgeführten oder finden Sie in folgenden Ressourcen:

- [Azure Bildschirm skalieren allgemeinen Bewertungsgrundlagen](insights-autoscale-common-metrics.md)
- [Best Practices für Azure Bildschirm skalieren](insights-autoscale-best-practices.md)
- [Verwenden Sie skalieren Aktionen e-Mails und Webhook-Benachrichtigung senden](insights-autoscale-to-webhook-email.md)
- [Skalierungsgröße REST-API](https://msdn.microsoft.com/library/dn931953.aspx)
- [Problembehandlung bei Virtual Machine Skalierung legt skalieren](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
