<properties
pageTitle="Cloud-Dienst aktualisieren | Microsoft Azure"
description="Informationen Sie zum Cloud-Dienste in Azure aktualisieren. Erfahren Sie, wie ein Update auf einem Clouddienst Verfügbarkeit fortgesetzt."
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
ms.date="08/10/2016"
ms.author="adegeo"/>

# <a name="how-to-update-a-cloud-service"></a>Cloud-Dienst aktualisieren

## <a name="overview"></a>Übersicht
10.000 Füßen ist das Aktualisieren eines Cloud-Dienstes Rollen sowohl Gastbetriebssystem, einschließlich drei Schritten. Zuerst müssen die Binärdateien und Konfigurationsdateien für neue Cloud-Dienst oder BS-Version hochgeladen werden. Als Nächstes reserviert Azure Compute und Netzwerkressourcen für den Clouddienst je nach den Erfordernissen der neuen Version der Cloud-Dienst. Schließlich führt Azure Aktualisierung aktualisieren inkrementell Pächter zu der neuen Version Gastbetriebssystem, Beibehaltung der Verfügbarkeit. Dieser Artikel beschreibt die Details des letzten Schritt – die Aktualisierung.

## <a name="update-an-azure-service"></a>Ein Azure-Aktualisierung

Azure verwaltet die Rolleninstanzen in logischen Gruppen aktualisieren Domänen (UD) genannt. Upgrade Domänen (UD) sind logische Gruppen von Instanzen, die als Gruppe aktualisiert werden.  Azure Updates eine Cloud service eine UD jeweils Instanzen in anderen UDs weiterhin Datenverkehr ermöglicht.

Die Standardanzahl von Upgrade Domänen ist 5. Sie können verschiedene Upgrade Domänen einschließlich des UpgradeDomainCount-Attributs in der Service-Definitionsdatei (.csdef) angeben. Weitere Informationen zum Attribut UpgradeDomainCount finden Sie unter [WebRole Schema](https://msdn.microsoft.com/library/azure/gg557553.aspx) oder [WorkerRole-Schema](https://msdn.microsoft.com/library/azure/gg557552.aspx).

Beim Ausführen ein direktes Update von einer oder mehreren Rollen in Ihrem Dienst aktualisiert Azure mehrere Instanzen nach Upgrade-Domäne, die sie angehören. Azure Updates alle Instanzen in einer bestimmten Aktualisierungsdomäne – beenden, aktualisieren, damit wieder online – fährt mit der nächsten Domäne. Nur die Instanzen in der aktuellen Domäne aktualisieren beenden, sorgt Azure eine Aktualisierung mit den wenigsten möglichen ausgeführten Dienst. Weitere Informationen finden Sie weiter unten in diesem Artikel [wie die Aktualisierung fortgesetzt wird](#howanupgradeproceeds) .

> [AZURE.NOTE] Begriffe **update** und **upgrade** leicht unterschiedlichen Bedeutung im Kontext Azure haben, können sie für die Prozesse und Features in diesem Dokument austauschbar.

Ihr Dienst definieren mindestens zwei Instanzen der Rolle für diese Rolle aktualisiert werden direkt ohne Ausfallzeiten. Wenn der Dienst nur eine Instanz einer Rolle besteht, ist Ihr Dienst erst verfügbar, in-Place-Aktualisierung abgeschlossen ist.

Dieses Thema umfasst die folgende Informationen zu Azure Updates:

-   [Zulässige Änderungen während einer Aktualisierung](#AllowedChanges)
-   [Wie wird eine Aktualisierung](#howanupgradeproceeds)
-   [Rollback des Updates](#RollbackofanUpdate)
-   [Mehrere veränderliche Vorgänge für eine kontinuierliche Bereitstellung initiieren](#multiplemutatingoperations)
-   [Verteilung von verschiedenen Domänen aktualisieren](#distributiondfroles)

<a name="AllowedChanges"></a>
## <a name="allowed-service-changes-during-an-update"></a>Zulässige Änderungen während einer Aktualisierung
Die folgende Tabelle zeigt die zulässigen Änderungen an einen Dienst während einer Aktualisierung:

|Ändert hosten, Services und Rollen zulässig|In-Place-Aktualisierung|Gestaffelte (VIP Swap)|Löschen und erneut bereitstellen|
|---|---|---|---|
|Version des Betriebssystems|Ja|Ja|Ja
|.NET Vertrauensebene|Ja|Ja|Ja|
|Virtuelle Maschine Größe<sup>1</sup>|Ja,<sup>2</sup>|Ja|Ja|
|Lokale Einstellungen|Nur<sup>2</sup> erhöhen|Ja|Ja|
|Hinzufügen oder Entfernen von Rollen in einem Dienst|Ja|Ja|Ja|
|Anzahl der Instanzen einer bestimmten Rolle|Ja|Ja|Ja|
|Anzahl oder Typ der Endpunkte für einen Dienst|Ja,<sup>2</sup>|Nein|Ja|
|Namen und Werte der Konfiguration|Ja|Ja|Ja|
|Werte (jedoch keine Namen) von Konfigurationen|Ja|Ja|Ja|
|Neue Zertifikate hinzufügen|Ja|Ja|Ja|
|Ändern vorhandener Zertifikate|Ja|Ja|Ja|
|Neuen Code bereitstellen|Ja|Ja|Ja|
<sup>1</sup> Änderung der Teilmenge Größen für den Clouddienst beschränkt.

<sup>2</sup> Erfordert Azure SDK 1.5 oder höher.

> [AZURE.WARNING] Ändern der Größe des virtuellen Computers werden lokale Daten gelöscht.


Die folgenden Elemente werden während einer Aktualisierung nicht unterstützt:

-   Ändern des Namens einer Rolle. Entfernen und dann mit dem neuen Namen hinzufügen.
-   Ändern der Anzahl der Domäne aktualisieren.
-   Verringern die Größe der lokalen Ressourcen.

Wenn Sie andere Updates an die Dienstdefinition wie lokale Ressource verkleinern müssen Sie stattdessen ein VIP Swap-Update ausführen. Weitere Informationen finden Sie unter [Bereitstellung austauschen](https://msdn.microsoft.com/library/azure/ee460814.aspx).

<a name="howanupgradeproceeds"></a>
## <a name="how-an-upgrade-proceeds"></a>Wie wird eine Aktualisierung
Sie können entscheiden, ob Sie alle Rollen im Dienst oder eine einzelne Rolle im Dienst aktualisieren möchten. In beiden Fällen alle Instanzen für jede Rolle, die gerade aktualisiert wird und gehören zu den ersten Aktualisierungsdomäne beendet aktualisiert und wieder online geschaltet. Sobald sie wieder online sind, sind Instanzen im Bereich Upgrade beendet, aktualisiert und wieder online geschaltet. Cloud-Dienst können höchstens ein Upgrade aktiv. Die Aktualisierung wird immer anhand der neuesten Version der Cloud-Dienst ausgeführt.

Das folgende Diagramm veranschaulicht, wie die Aktualisierung fortgesetzt wird, wenn Sie alle Rollen im Dienst aktualisieren:

![Dienst aktualisieren] (media/cloud-services-update-azure-service/IC345879.png "Dienst aktualisieren")

Dieses nächsten Diagramm veranschaulicht, wie die Aktualisierung fortgesetzt wird, wenn Sie nur eine einzige Rolle aktualisieren:

![Rolle aktualisieren] (media/cloud-services-update-azure-service/IC345880.png "Rolle aktualisieren")  

> [AZURE.NOTE] Wenn eine Instanz ein Dienstes auf mehrere Instanzen aktualisieren Ihren Dienst bringen Sie während der Aktualisierung durch Dienste wie Azure Upgrades. Service Vereinbarung garantiert Service Availability gilt nur für Dienste, die mit mehreren Instanzen bereitgestellt werden. Die folgende Liste beschreibt, wie die Daten auf jedem Laufwerk jede Azure Service Aktualisierungsszenario betroffen ist:
>
>VM-Neustart:
>
-   C: beibehalten
-   D: beibehalten
-   E: beibehalten
>
>Portal Neustart:
>
-   C: beibehalten
-   D: beibehalten
-   E: zerstört
>
>Portal neu abbilden:
>
-   C: beibehalten
-   D: zerstört
-   E: zerstört

>In-Place-Aktualisierung:
>
-   C: beibehalten
-   D: beibehalten
-   E: zerstört
>
>Knoten-Migration:
>
-   C: zerstört
-   D: zerstört
-   E: zerstört

>Beachten Sie, dass in der obigen Liste Laufwerk E: Stammverzeichnis die Rolle darstellt und nicht hartcodiert sollte. Verwenden Sie stattdessen die Umgebungsvariable RoleRoot, um das Laufwerk darzustellen.

>Minimierung die Ausfallzeiten bei Einzelinstanz-Dienst einen neuen Dienst mit mehreren Instanzen auf dem Stagingserver bereitstellen und Ausführen einer VIP-Swaps.

Während einer automatischen Aktualisierung wertet Azure Fabric Controller in regelmäßigen Abständen den Zustand der Cloud-Dienst zu bestimmen, wann der nächste UD durchlaufen werden kann. Dabei Gesundheit erfolgt jeweils pro Rolle und hält nur Instanzen in der neuesten Version (d. h. Instanzen von bereits gegangen UDs). Überprüft, ob eine Mindestanzahl von Rolleninstanzen für jede Rolle einen zufriedenstellenden Endzustand erreicht haben.

### <a name="role-instance-start-timeout"></a>Rolle Instanz starten Timeout
Der Fabric Controller ist 30 Minuten für jede Rolleninstanz gestarteten Zustand zu warten. Wenn das Zeitlimit abläuft, wird der Fabric Controller gehen zur nächsten Rolleninstanz.

<a name="RollbackofanUpdate"></a>
## <a name="rollback-of-an-update"></a>Rollback des Updates
Azure bietet Flexibilität beim Verwalten von Services während einer Aktualisierung können Sie weitere Vorgänge für einen Dienst starten, nach der Annahme der ursprünglichen Aktualisierungsanfrage Azure Fabric Controller. Ein Rollback kann nur ausgeführt werden, wenn eine Aktualisierung (Konfiguration ändern) oder Aktualisierung ist in den Zustand " **in Bearbeitung** " für die Bereitstellung. Ein Update oder Upgrade gilt in Bearbeitung als mindestens eine Instanz des Dienstes, die noch nicht auf die neue Version aktualisiert wurde. Testen, ob ein Rollback zulässig ist, überprüfen Sie den Wert des RollbackAllowed-Flags zurückgegebene Operationen [Bereitstellung abrufen](https://msdn.microsoft.com/library/azure/ee460804.aspx) und [Cloud-Eigenschaften abzurufen](https://msdn.microsoft.com/library/azure/ee460806.aspx) , wird true.

> [AZURE.NOTE] Es ist nur sinnvoll Rollback auf eine **direkte** Aktualisierung oder Aktualisierung da VIP Swap Upgrades umfassen eine gesamte ausgeführte Instanz des Dienstes durch einen anderen ersetzen.

Rollback des Updates in Bearbeitung hat folgende Effekte für die Bereitstellung:

-   Alle Instanzen noch nicht aktualisiert oder auf die neue Version aktualisiert wurde nicht aktualisiert oder aktualisiert die Instanzen bereits die Zielversion des Dienstes ausgeführt werden.
-   Alle Instanzen bereits aktualisiert oder auf die neue Version des Service-Paket aktualisiert wurde (\*.cspkg) Datei oder die Dienstkonfiguration (\*.cscfg) Datei (oder beide Dateien) vor der Aktualisierung Version dieser Dateien zurückgesetzt.

Diese Funktion Folgendes bietet:

-   Der [Rollback Update oder Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) -Vorgang auf eine Aktualisierung (ausgelöst durch Aufrufen von [Bereitstellungskonfiguration ändern](https://msdn.microsoft.com/library/azure/ee460809.aspx)) oder einer Aktualisierung (ausgelöst durch Aufrufen von [Bereitstellung aktualisieren](https://msdn.microsoft.com/library/azure/ee460793.aspx)) aufgerufen werden, solange es mindestens eine Instanz des Dienstes die noch nicht auf die neue Version aktualisiert wurde.
-   Das Element gesperrt und RollbackAllowed Element als Teil des Antworttexts der [Bereitstellung abrufen](https://msdn.microsoft.com/library/azure/ee460804.aspx) und [Cloud-Eigenschaften erhalten](https://msdn.microsoft.com/library/azure/ee460806.aspx) zurückgegeben werden:
    1.  Das Element gesperrt können Sie erkennen, wann ein veränderliche Vorgang für eine gegebene Bereitstellung aufgerufen werden kann.
    2.  Das RollbackAllowed-Element können Sie erkennen, wenn die [Rollback Update oder Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) -Operation für eine gegebene Bereitstellung aufgerufen werden.

    Um einen Rollback durchzuführen, müssen Sie keinen der gesperrt und RollbackAllowed Elemente überprüfen. Genügt, um sicherzustellen, dass RollbackAllowed festgelegt ist true. Diese Elemente werden nur zurückgegeben, wenn diese Methoden aufgerufen werden, mit der Anfrage-Header gesetzt "X-ms-Version: 2011-10-01" oder höher. Weitere Informationen über Versionskontrolle Header finden Sie unter [Service Management Versioning](https://msdn.microsoft.com/library/azure/gg592580.aspx).

Es gibt einige Situationen, wo ein Rollback des Updates oder Upgrades werden nicht unterstützt, sind:

-   Lokale Ressourcen - Update erhöht die lokalen Ressourcen für eine Rolle der Azure-Plattform lässt nicht zurücksetzen. 
-   Kontingentgrenzen - haben war das Update Herunterskalieren Vorgang können Sie nicht mehr ausreichend Compute Kontingent vom Rollbackvorgang abgeschlossen. Jede Azure-Abonnement hat ein Kontingent zugeordnet, die von allen gehosteten Dienste verwendet werden kann, die dieses Abonnement gehören die maximale Anzahl von Kernen angibt. Wenn einen Rollback eines bestimmten Updates ausführen Ihres Abonnements Kontingent dann, würde wird ein Rollback nicht aktiviert.
-   Racebedingung - Wenn der ursprüngliche Aktualisierung, Rollback ist nicht möglich.

Ein Beispiel wenn Rollback des Updates möglicherweise nützlich ist das [Upgrade](https://msdn.microsoft.com/library/azure/ee460793.aspx) Deployment im manuellen Modus Steuerelement verwenden, Rate mit einer großen direktes Upgrade in der Azure-Dienst gehostet Rollout.

Während der Installation der Aktualisierung [Aktualisieren Bereitstellung](https://msdn.microsoft.com/library/azure/ee460793.aspx) im manuellen Modus aufrufen und Upgrade Domänen zu beginnen. Sie überwachen die Aktualisierung Wenn irgendwann Hinweis reagieren einige Instanzen in den ersten Upgrade Domänen, die Sie untersuchen, [Rollback Update oder Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx) -Vorgang für die Bereitstellung, die Instanzen, die noch nicht aktualisiert wurde und Rollback Instanzen zu vorherigen Servicepaket und Konfiguration Upgrade unverändert aufrufen.

<a name="multiplemutatingoperations"></a>
## <a name="initiating-multiple-mutating-operations-on-an-ongoing-deployment"></a>Mehrere veränderliche Vorgänge für eine kontinuierliche Bereitstellung initiieren
In einigen Fällen möchten Sie mehrere veränderliche Arbeitsgänge auf eine kontinuierliche Bereitstellung initiieren. Beispielsweise können Sie eine Update durchführen, und zwar das Update über den Dienst eingeführt wird, soll eine Änderung z. B. um Rollen das Update wieder ein anderes Update anwenden oder sogar löschen die Bereitstellung. In dem diese ggf. ist Service Upgrade fehlerhaftem Code enthält, wodurch eine aktualisierte Rolleninstanz wiederholt abstürzt. In diesem Fall dürfen Azure Fabric Controller Fortschritte bei der Anwendung, die aktualisiert werden, da keine ausreichende Anzahl von Instanzen in der aktualisierten Domäne fehlerfrei sind. Dieser Zustand wird eine *Bereitstellung stecken*genannt. Lösen die Bereitstellung durch Rollback Update oder eine neue Aktualisierung Anfang der fehlerhaften eine.

Nach die erste Anforderung zum Aktualisieren des Dienstes von Azure Fabric Controller empfangen wurde, können nachfolgende mutierende Vorgänge starten. Sie haben also nicht warten für die Inbetriebnahme vor anderen mutierende Vorgang abgeschlossen.

Einen zweiten Aktualisierungsvorgang starten, während die erste Aktualisierung läuft führt vom Rollbackvorgang ähnelt. Ist das zweite Update im automatischen Modus werden die erste Domäne aktualisieren, aktualisiert Instanzen aus mehreren Upgrade wird an der gleichen Stelle rechtzeitig führen.

Mutierende Vorgänge lauten folgendermaßen: [Bereitstellungskonfiguration ändern](https://msdn.microsoft.com/library/azure/ee460809.aspx), [Bereitstellung aktualisieren](https://msdn.microsoft.com/library/azure/ee460793.aspx) [Update Bereitstellungsstatus](https://msdn.microsoft.com/library/azure/ee460808.aspx), [Bereitstellung löschen](https://msdn.microsoft.com/library/azure/ee460815.aspx)und [Rollback Update oder Upgrade](https://msdn.microsoft.com/library/azure/hh403977.aspx).

Zwei Vorgänge [Bereitstellung abrufen](https://msdn.microsoft.com/library/azure/ee460804.aspx) und [Cloud-Eigenschaften erhalten](https://msdn.microsoft.com/library/azure/ee460806.aspx)zurück Locked Flag geprüft werden können, um festzustellen, ob ein veränderliche Vorgang für eine gegebene Bereitstellung aufgerufen werden kann.

Um Version dieser Methoden aufrufen, die gesperrt Flag gibt, müssen Sie auf Anfrage-Header festlegen "X-ms-Version: 2011-10-01" oder später. Weitere Informationen über Versionskontrolle Header finden Sie unter [Service Management Versioning](https://msdn.microsoft.com/library/azure/gg592580.aspx).

<a name="distributiondfroles"></a>
## <a name="distribution-of-roles-across-upgrade-domains"></a>Verteilung von verschiedenen Domänen aktualisieren
Azure verteilt Instanzen einer Rolle gleichmäßig auf eine Reihe von Upgrade Domänen als Teil des Service-Definitionsdatei (.csdef) konfiguriert werden können. Die maximale Anzahl von Domänen Upgrade ist 20 und der Standardwert ist 5. Weitere Informationen zum Ändern der Definitionsdatei Service finden Sie unter [Azure Service Definition Schema (.csdef Datei)](cloud-services-model-and-package.md#csdef).

Beispielsweise verfügt Ihre Rolle 10 Instanzen enthält standardmäßig jede Aktualisierungsdomäne zwei Instanzen. Wenn Ihre Rolle 14 Instanzen verfügt, vier Upgrade Domänen enthalten drei Instanzen und fünfte Domäne enthält zwei.

Upgrade-Domänen werden mit einem nullbasierten Index identifiziert: erste Aktualisierungsdomäne ID 0 und Bereich Upgrade ID 1 und So weiter.

Das folgende Diagramm veranschaulicht, wie ein Dienst mit zwei Rollen verteilt werden, wenn der Dienst zwei Upgrade Domänen definiert. Der Dienst läuft acht Instanzen der Webrolle und neun Instanzen der Rolle der Arbeitskraft.

![Verteilung der Upgrade Domänen] (media/cloud-services-update-azure-service/IC345533.png "Verteilung der Upgrade Domänen")

> [AZURE.NOTE] Beachten Sie, dass Azure Steuerelemente wie Instanzen aktualisieren domänenübergreifend zugeordnet werden. Es ist nicht möglich, an Instanzen der Domäne zugeordnet werden.

## <a name="next-steps"></a>Nächste Schritte
[Cloud-Dienste verwalten](cloud-services-how-to-manage.md)  
[Cloud-Dienste überwachen](cloud-services-how-to-monitor.md)  
[Cloud-Dienste konfigurieren](cloud-services-how-to-configure.md)  
