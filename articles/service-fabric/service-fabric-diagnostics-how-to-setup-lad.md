<properties
   pageTitle="Sammeln von Protokollen mit Linux Azure Diagnostics | Microsoft Azure"
   description="Dieser Artikel beschreibt das Einrichten Azure-Diagnose zum Sammeln von Protokollen aus einem Service Fabric Linux Cluster in Azure ausgeführt."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Sammeln von Protokollen mithilfe von Azure-Diagnose

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Einen Azure Service Fabric-Cluster ausführen, wird eine gute Idee, die Protokolle von allen Knoten an einem zentralen Ort sammeln. Müssen die Protokolle an einem zentralen Ort erleichtert analysieren und Beheben von Problemen, ob in Ihre Dienste, die Anwendung oder den Cluster werden. Eine Möglichkeit zum Hochladen und Sammeln von Protokollen ist mit Erweiterung Azure-Diagnose, die Protokolle Azure-Speicher hochgeladen. Die Ereignisse aus dem Speicher gelesen können und in einem Produkt wie [Elastische suchen](service-fabric-diagnostic-how-to-use-elasticsearch.md) oder einen anderen Protokoll-Analyse-Lösung platzieren.

## <a name="log-sources-that-you-might-want-to-collect"></a>Protokollquellen, die Sie sammeln möchten
- **Service Fabric Protokolle**: aus der Plattform über [LTTng](http://lttng.org) und das Speicherkonto hochgeladen. Protokolle kann operative oder Runtime-Ereignisse, die die Plattform gibt. Diese Protokolle werden an dem Speicherort gespeichert, die clustermanifest angegeben. (Zu den Kontodetails Speicher Tag **AzureTableWinFabETWQueryable** suchen und Suchen nach **StoreConnectionString**.)
- **Anwendungsereignisse**: von der Service-Code ausgegeben. Jede Lösung protokollieren können, die textbasierte Dateien – z. B. LTTng schreibt. Weitere Informationen finden Sie in der Dokumentation LTTng in der Anwendung verfolgen.  


## <a name="deploy-the-diagnostics-extension"></a>Bereitstellen der Diagnose-Erweiterung
Der erste Schritt beim Sammeln von Protokollen ist Diagnose-Erweiterung auf die VMs im Service Fabric-Cluster bereitstellen. Diagnose-Erweiterung sammelt Protokolle auf jede VM und lädt sie das Speicherkonto, das Sie angeben. Die Schritte variieren, ob Azure-Portal oder Azure Ressourcen-Manager verwenden.

Legen Sie zum Bereitstellen der Diagnose-Erweiterung mit den virtuellen Computern im Cluster als Teil der Clustererstellung **Diagnose** **auf**. Nach dem Erstellen des Clusters können nicht Sie diese Einstellung mithilfe des Portals.

Konfigurieren Sie Linux Azure Diagnostics (JUNGE) die Dateien sammeln und diese in das Speicherkonto. Dieser Prozess wird als Szenario 3 ("Hochladen Eigene Dateien") in Artikel [Verwenden JUNGE überwachen und Diagnostizieren von Linux VMs](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md)erläutert. Dabei erhält Zugriff auf die Spuren. Sie können die Spuren in einer Schnellansicht Ihrer Wahl hochladen.

Sie können auch Diagnose-Erweiterung mithilfe von Azure-Ressourcen-Manager bereitstellen. Der Prozess ist für Windows und Linux und für Windows-Cluster [Protokolle mit Azure Diagnostics sammeln](service-fabric-diagnostics-how-to-setup-wad.md)dokumentiert.

Sie können auch Operations Management Suite unter [Operations Management Suite Protokollanalyse mit Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Nach Abschluss dieser Konfiguration überwacht der Agent JUNGE angegebenen Protokolldateien. Wenn die Datei eine neue Zeile hinzugefügt, erstellt einen Syslog-Eintrag des Speichers an, den Sie angegeben.


## <a name="next-steps"></a>Nächste Schritte
Detaillierter Ereignisse untersuchen sollten bei der Fehlerbehebung finden Sie unter [LTTng Dokumentation](http://lttng.org/docs) und [JUNGE verwenden](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
