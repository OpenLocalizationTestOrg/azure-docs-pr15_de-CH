<properties
    pageTitle="Cloud-Dienst Reservierungsfehler Problembehandlung | Microsoft Azure"
    description="Zuweisungsfehler bei der Bereitstellung von Cloud-Diensten in Azure Problembehandlung"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Zuweisungsfehler bei der Bereitstellung von Cloud-Diensten in Azure Problembehandlung

## <a name="summary"></a>Zusammenfassung
Instanzen für einen Cloud-Dienst bereitstellen oder fügen neue Webseite oder neuer Rolleninstanzen Arbeitskraft weist Microsoft Azure computeressourcen. Gelegentlich Fehler möglicherweise bei diesen Vorgängen noch Azure-Abonnement Grenzen. Dieser Artikel beschreibt die Ursachen für einige allgemeine Zuordnungsfehler und schlägt mögliche Wiederherstellung. Die Informationen können auch hilfreich sein, beim Planen der Bereitstellung Ihrer Dienste.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Hintergrund-Verteilung funktioniert
Server in Azure Rechenzentren werden in Cluster aufgeteilt. Neue Cloud Reservierung angefordert wird in mehrere Cluster versucht. Wenn zunächst für einen Clouddienst (in Staging oder Produktion), die cloud-Dienst bereitgestellt wird, wird zu einem Cluster fixiert. Alle weiteren Installationen von Cloud-Dienst erfolgt in demselben Cluster. In diesem Artikel werden diese bezeichnen wir als ein Cluster "fixiert". Abbildung 1 unten zeigt bei einer normalen Verteilung mehreren Clustern versucht. Abbildung 2 zeigt einen Fall einer Zuordnung, denn das Hosting der vorhandenen Cloud-Dienst CS_1 Cluster 2 fixiert ist.

![Zuweisung-Diagramm](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Warum geschieht Reservierungsfehler
Bei einem Cluster eine fixiert ist, besteht ein höheres Risiko von Ressourcen findet, da der verfügbaren Ressourcen-Pool auf einem Cluster. Darüber hinaus Wenn Wunsch Zuweisung zu einem Cluster fixiert die angeforderte Ressource wird nicht unterstützt, Cluster, fehl Ihre Anfrage auch wenn Cluster kostenlose. Diagramm 3 zeigt einen Fall, in dem eine fixierte Zuordnung fehlschlägt, weil nur Kandidaten Cluster keinen Ressourcen. Abbildung 4 zeigt einen Fall, eine fixierte Zuordnung fehlschlägt, weil nur Kandidaten Cluster die angeforderte Größe VM nicht unterstützt, obwohl der Cluster Ressourcen verfügt.

![Fixierte Zuordnung-Fehler](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Problembehandlung bei Reservierungsfehler für Cloud-services
### <a name="error-message"></a>Fehlermeldung
Die folgende Fehlermeldung auftreten:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Allgemeine Probleme
Zuweisung Szenarien, die eine Zuordnung Anforderung zu einem einzigen Cluster fixiert werden

- Bereitstellen in Staging-Steckplatz - hat ein Cloud-Dienst eine Bereitstellung in beiden Steckplätzen ist dann der gesamte Cloud-Dienst zu einem bestimmten Cluster fixiert.  Dies bedeutet, dass bereits eine Bereitstellung in der Produktion Steckplatz vorhanden, dann neue Stagingdatenbank Bereitstellung nur in Clusters Produktion Steckplatz zugeordnet werden kann. Wenn Cluster Kapazität nähert, kann die Anforderung fehlschlagen.

- Skalieren: Hinzufügen von neuen Instanzen einer vorhandenen Cloud-Dienst muss im selben Cluster zuweisen.  Kleine Skalierung Anfragen kann in der Regel zugeordnet werden, aber nicht immer. Wenn Cluster Kapazität nähert, kann die Anforderung fehlschlagen.

- Gruppe - neue Bereitstellung einer leeren Clouddienst kann durch den Stoff in jedem Cluster in diesem Bereich zugewiesen werden, wenn Cloud-Dienst zu einer Gruppe fixiert ist. Bereitstellung der gleichen Gruppe Affinität werden auf demselben Cluster versucht. Wenn Cluster Kapazität nähert, kann die Anforderung fehlschlagen.

- Gruppe vNet - ältere virtuelle Netzwerke wurden mit Gruppen anstelle von Regionen und Clouddienste in diesen virtuellen Cluster Gruppe fixiert werden würde. Bereitstellung für diesen Typ des virtuellen Netzwerks werden fixierten Cluster versucht. Wenn Cluster Kapazität nähert, kann die Anforderung fehlschlagen.

## <a name="solutions"></a>Solutions

1. Einen neuen Cloud-Dienst erneut - Lösung wahrscheinlich erfolgreichste Plattform alle Cluster in diesem Bereich auswählen können.

    - Die Arbeitslast auf einen neuen Clouddienst bereitstellen  

    - Aktualisieren Sie der CNAME-Eintrag oder einen Datensatz darauf Datenverkehr auf den neuen Clouddienst

    - Wenn NULL Verkehr auf der alten Website geht, können Sie alte Cloud-Dienst löschen. Diese Lösung sollte Ausfallzeiten entstehen.

2. Produktion und staging-Steckplätze löschen - Lösung behält die vorhandene DNS-Namen, aber Ausfallzeiten der Anwendung verursacht.

    - Löschen der Produktion und staging-Steckplätze einen vorhandenen Cloud-Dienst, dass Cloud-Dienst leer ist und

    - Erstellen Sie eine neue Bereitstellung in vorhandenen Cloud-Dienst. Diese versucht Umlage auf allen Clustern im Bereich erneut. Sicherstellen Sie, dass Cloud-Dienst nicht an eine Gruppe gebunden ist.

3. Reservierte IP - beibehalten dieser Lösung Ihre vorhandene IP-Adresse, aber werden Ausfallzeiten der Anwendung.  

    - Erstellen Sie eine reservierte IP-Adressen für die vorhandene Bereitstellung mit Powershell

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Folgen Sie #2 von dafür in den Dienst mithilfe der neuen ReservedIP an

4. Entfernen Sie bei Neuinstallationen - Gruppe Gruppen nicht mehr empfohlen. Schritte für #1 oben einen neuen Clouddienst bereitgestellt. Stellen Sie sicher, Cloud-Dienst nicht in einer Gruppe ist.

5. Konvertieren Sie zu einem regionalen virtuellen Netzwerk - finden Sie unter [Migrieren von Gruppen mit einem regionalen virtuellen Netzwerk (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
