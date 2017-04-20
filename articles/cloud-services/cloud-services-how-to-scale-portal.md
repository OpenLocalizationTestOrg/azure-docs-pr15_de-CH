<properties
    pageTitle="Automatische Skalierung einen Cloud-Dienst im Portal | Microsoft Azure"
    description="Erfahren Sie mehr über das Portal verwenden, um die automatische Skalierung für einen Cloud-Dienst Web oder Arbeitskraft Rolle in Azure konfigurieren."
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

Für eine Cloud Service Worker-Rolle, die eine Skala in oder Vorgang auslösen können Startbedingungen festgelegt werden. Die Vorschriften für die Rolle können CPU, Datenträger oder Netzwerklast der Rolle basieren. Sie können auch eine Conditation anhand einer Warteschlange oder die Metrik einer anderen Azure Ressource zugeordneten Abonnements festlegen.

>[AZURE.NOTE] Dieser Artikel konzentriert sich auf Cloud Web- und Workerrollen Rollen. Wenn Sie einen virtuellen Computer (klassisch) direkt erstellen, ist es in einem Clouddienst gehostet. Können Sie einen standardmäßigen virtuellen Computer mit einer [Verfügbarkeit](../virtual-machines/virtual-machines-windows-classic-configure-availability.md) skalieren und manuell aktivieren oder deaktivieren.

## <a name="considerations"></a>Hinweise

Vor dem Konfigurieren der Anwendung skalieren, berücksichtigen Sie der folgenden Informationen:

- Skalierung von Core Verwendung betroffen. Größere Instanzen verwenden mehr Kerne. Sie können Ihr Abonnement eine Anwendung nur innerhalb der Kerne skalieren. Beispielsweise hat Ihr Abonnement maximal 20 Adern und Ausführen eine Anwendung mit zwei mittlere Cloud-Dienste (insgesamt vier Kerne), können nur skalieren andere Cloud Service-Bereitstellung in Ihrem Abonnement von 16 Kerne. Finden Sie weitere Informationen zu Größen [Cloud Service Größen](cloud-services-sizes-specs.md) .

- Skalieren Sie basierend auf einer Warteschlange Schwellenwert. Weitere Informationen zur Verwendung von Warteschlangen finden Sie unter [Verwendung der Warteschlangendienst Speicher](../storage/storage-dotnet-how-to-use-queues.md).

- Sie können auch andere Ressourcen mit Ihrem Abonnement skalieren.

- Um die hohe Verfügbarkeit Ihrer Anwendung zu ermöglichen, sollten mit zwei oder mehr Instanzen bereitgestellt wird. Weitere Informationen finden Sie unter [Service Level Agreements](https://azure.microsoft.com/support/legal/sla/).

## <a name="where-scale-is-located"></a>Skala befindet

Nach dem Cloud-Dienst auswählen, müssen Sie die Cloud Blatt sichtbar.

1. Wählen Sie den Namen der Cloud-Dienst-Blade Cloud-Dienst auf der Kachel **Rollen und Instanzen** .   
**Wichtig**: achten auf Cloud-Dienst Rollen nicht die Instanz der Rolle, die unter der Rolle.

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. Wählen Sie das Musterelement **Skalieren** .

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatische Skalierung

Sie können Skalierung für eine Rolle mit zwei Modi **Manuelle** oder **Automatische**konfigurieren. Manuelle erwartungsgemäß ist die absolute Anzahl der Instanzen festlegen. Automatische kann jedoch Sie Satz Regeln wie und wie viel Sie skaliert werden sollen.

Legen Sie die Option **Skalieren von** **Zeitplan**und Leistung.

![Cloud-Dienste Einstellungen mit Regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Vorhandenes Profil.
2. Eine Regel für die übergeordnete Profil hinzufügen.
3. Fügen Sie ein anderes Profil.

Wählen Sie **Profil hinzufügen**. Das Profil bestimmt den Modus für die Skalierung verwenden: **immer** **Serie**, **Termin**.

Nach Profil und Regeln konfiguriert haben, wählen Sie oben auf das Symbol **Speichern** .

#### <a name="profile"></a>Profil

Das Profil legt Mindest- und Instanzen für die Skalierung und auch wenn dieser Skala Bereich aktiv ist.

* **Immer**

    Dieser Bereich verfügbaren Instanzen immer beibehalten.  

    ![Cloud-Dienst immer skalieren](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **Serie**

    Wählen Sie einen Wochentag skalieren.

    ![Cloud-Dienst Skalieren mit einer Serie](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **Festes Datum**

    Einen festen Zeitraum Rolle skalieren.

    ![CLoud-Dienst Skalieren mit festem](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nach der Konfiguration des Profils wählen Sie **OK** unten Profil Blade.

#### <a name="rule"></a>Regel

Regeln Profil hinzu und stellen eine Bedingung, die die Skalierung auslöst. 

Der Auslöser Metrik Cloud-Dienst (CPU-Auslastung, Datenträgeraktivität oder Netzwerkaktivität) basiert, bedingten Wert hinzufügen können. Darüber hinaus können Sie Trigger anhand einer Warteschlange oder die Metrik einer anderen Azure Ressource Ihrem Abonnement zugeordnet haben.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nachdem die Regel konfiguriert haben, wählen Sie **OK** unten auf dem Blatt.

## <a name="back-to-manual-scale"></a>Um manuelle Skalierung

Navigieren Sie zu den [Einstellungen](#where-scale-is-located) und **Scale by** -Option auf **eine manuell eingegebene Instanzenzahl**festgelegt.

![Cloud-Dienste Einstellungen mit Regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Diese automatische Skalierung aus der Rolle entfernt und dann können Sie die Anzahl der Instanzen direkt einstellen. 

1. Die Skalierung (manuell oder automatisiert) die Option.
2. Eine Rolle Instanz Schieberegler Instanzen Skalieren festlegen.
3. Instanzen der Rolle skalieren.

Nachdem die Einstellungen konfiguriert haben, wählen Sie oben auf das Symbol **Speichern** .

