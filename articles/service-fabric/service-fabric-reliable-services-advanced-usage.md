<properties
   pageTitle="Erweiterte Nutzung zuverlässige Dienste | Microsoft Azure"
   description="Erweiterte Verwendung von zuverlässigen Service Fabric-Services Flexibilität Ihrer Dienste erfahren."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Erweiterte Nutzung der zuverlässigen Programmiermodell
Azure Service Fabric vereinfacht das Schreiben und zuverlässige statusfreie und statusbehaftete Dienste verwalten. Dieses Handbuch wird über erweiterte Verwendungen zuverlässige Dienste zu mehr Kontrolle und Flexibilität über Ihre Dienste sprechen. Machen Sie vor der Lektüre dieses Handbuchs sich mit [Den zuverlässigen Diensten Programmiermodell](service-fabric-reliable-services-introduction.md).

Statusbehaftete und statusfreie haben zwei primäre Einstiegspunkte für Benutzercode:

 - `RunAsync`ist eine allgemeine Service Code.
 - `CreateServiceReplicaListeners`und `CreateServiceInstanceListeners` für kommunikationslistener für Clientanforderungen geöffnet ist.
 
Für die meisten Dienste sind diese beiden ausreichend. In seltenen Fällen Wenn mehr über eine Service-Lebenszyklus Kontrolle stehen zusätzliche Lebenszyklusereignisse.

## <a name="stateless-service-instance-lifecycle"></a>Instanz-statusfreie Service-Lebenszyklus

Eine statusfreie Service-Lebenszyklus ist sehr einfach. Statusfreier Service kann nur geöffnet, geschlossen oder abgebrochen werden. `RunAsync`statusfreie Service wird ausgeführt, wenn eine Dienstinstanz geöffnet und abgebrochen, wenn eine Dienstinstanz geschlossen oder abgebrochen wird. 

Obwohl `RunAsync` sollte ausreichend sein, in fast allen Fällen öffnen, schließen und Abbruch Ereignisse in eine statusfreie Service sind ebenfalls verfügbar:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync wird aufgerufen, wenn die statusfreie Dienstinstanz verwendet werden. Erweiterter Service Initialisierungsaufgaben können zu diesem Zeitpunkt gestartet werden.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync wird aufgerufen, wenn die statusfreie Dienstinstanz ordnungsgemäß heruntergefahren wird. Dies kann auftreten, wenn die Service-Code aktualisiert, die Dienstinstanz durch Load balancing verschoben wird oder ein vorübergehender Fehler festgestellt. OnCloseAsync lässt sich problemlos schließen alle Ressourcen, hintergrundverarbeitung beenden, speichern externen Zustand oder vorhandenen Verbindungen.

- `void OnAbort()`
  OnAbort wird aufgerufen, wenn die statusfreie Dienstinstanz zwangsweise heruntergefahren wird. Dies wird im Allgemeinen bezeichnet ein dauerhaften Fehler auf dem Knoten erkannt wird oder Service Fabric zuverlässig die Dienstinstanz Lebenszyklus aufgrund interner verwalten können.

## <a name="stateful-service-replica-lifecycle"></a>Statusbehaftete Dienst Replikat Lebenszyklus

Eine statusbehaftete dienstreplikat Lebenszyklus ist komplexer als eine statusfreie Dienstinstanz. Darüber hinaus wird ein Replikat statusbehaftete Dienst zum Öffnen, schließen und Ereignisse abbrechen Rolle ändern während seiner Lebensdauer. Beim Ändern eines Replikats statusbehaftete Dienst Funktion, die `OnChangeRoleAsync` Ereignis ausgelöst:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync wird aufgerufen, wenn das statusbehaftete dienstreplikat Rolle beispielsweise primäre oder sekundäre geändert wird. Primäre Replikate erhalten Schreibstatus (dürfen erstellen und Schreiben zuverlässigen Sammlungen). Sekundäre Replikate erhalten Lesestatus (nur aus vorhandenen zuverlässige lesen). Die meisten wird statusbehaftete Dienst primäre Replikat ausgeführt. Sekundäre Replikate können schreibgeschützte Validierung, Berichten, Datamining oder andere schreibgeschützte durchführen.

Statusbehaftete Dienst primäre Replikat verfügt über Schreibzugriff auf Status und deshalb normalerweise Wenn der Dienst Arbeit ausführt. Die `RunAsync` Methode in eine statusbehaftete Dienst wird nur ausgeführt, wenn das Replikat statusbehaftete Dienst primäre ist. Die `RunAsync` -Methode abgebrochen, wechselt ein primäres Replikat Rolle vom primären sowie während der Ereignisse schließen und Abbrechen. 

Mit der `OnChangeRoleAsync` Ereignis können Sie Aufgaben je nach replikatrolle sowie auf Rolle ändern.

Statusbehafteter Dienst bietet auch die gleichen vier Lebenszyklusereignisse als statusfreie Service mit derselben Semantik und Anwendungsfälle:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Nächste Schritte
Weitere Themen im Zusammenhang mit Service Fabric finden Sie in folgenden Artikeln:

- [Statusbehaftete zuverlässige Dienste konfigurieren](service-fabric-reliable-services-configuration.md)

- [Fabric Service Health Einführung](service-fabric-health-introduction.md)

- [Verwenden Systemberichte zur Fehlerbehebung](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Konfigurieren von Diensten mit Service Fabric Cluster-Ressourcen-Manager](service-fabric-cluster-resource-manager-configure-services.md)
