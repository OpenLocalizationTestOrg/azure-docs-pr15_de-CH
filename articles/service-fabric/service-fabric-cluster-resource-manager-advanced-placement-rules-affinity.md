<properties
   pageTitle="Service Fabric Cluster Resource Manager - Affinität | Microsoft Azure"
   description="Übersicht über Affinität für Fabric-Dienst konfigurieren"
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Konfigurieren und Verwenden von Service-Affinität Fabric Service

Affinität ist, die hauptsächlich bereitgestellt wird den Übergang größere monolithischen Applikationen in die Cloud und Microservices erleichtern. Das heißt es auch in bestimmten Fällen als legitime Optimierung verwendet werden kann zum Verbessern der Leistung von Diensten, obwohl diese Nebeneffekte haben kann.

Angenommen, Sie bringen eine größere app oder eine nur mit Microservices zu Service Fabric ausgelegt war. Dieser Übergang ist tatsächlich, und wir hatten einige Kunden (intern und extern) in dieser Situation. Sie starten heben die gesamte Anwendung in der Umgebung verpackt abrufen und ausführen. Starten Sie brechen in kleinere Dienststellen, dass alle miteinander.

Gibt eine "Oops...". "Achtung" liegt in der Regel in folgenden Kategorien:

1. Eine Komponente X monolithische App war eine nicht dokumentierte Abhängigkeit Komponente Y, und wir nur in separate Dienste. Da jetzt auf anderen Knoten im Cluster ausgeführt werden, sind sie beschädigt.
2.  Über diese Dinge kommunizieren (lokale named Pipes | freigegebenen Speicher | Dateien auf der Festplatte), aber ich brauche zu beschleunigen etwas unabhängig aktualisiert werden. Ich werde später harte Abhängigkeit entfernen.
3.  Alles ist in Ordnung, aber es stellt sich heraus, dass diese beiden Komponenten sehr sind ausführliche Leistungsverhältnis vertrauliche. Wenn sie in separaten Services verschoben stieg insgesamt getankt Application Performance oder Latenz. Daher treffen die gesamte Anwendung nicht erwartet.

In diesen Fällen wir möchten unsere Umgestaltung Arbeit verlieren und Monolithen zurückkehren wollen, aber wir brauchen gewissermaßen Ort. Bleiben bis wir natürlich Services arbeiten Komponenten ändern können oder bis wir möglichst Leistungsangaben andere Weise lösen.

Was ist zu tun? Nun können Sie versuchen, Affinität einschalten.

## <a name="how-to-configure-affinity"></a>Affinität konfigurieren
Um Affinität einzurichten, definieren Sie eine Affinität Beziehung zwischen zwei Diensten. Sie können sich vorstellen Affinität "zeigen" einen Dienst an einem anderen und sagen "dieses Service kann nur ausgeführt werden, Dienst ausgeführt wird." Manchmal verweisen wir auf Affinität als Eltern-Kind-Beziehung (wo Sie das untergeordnete Element am übergeordneten zeigen). Affinität wird sichergestellt, dass Replikate oder Instanzen eines Dienstes auf denselben Knoten als Replikate oder anderen Instanzen.

``` csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

## <a name="different-affinity-options"></a>Verschiedene Clientzugehörigkeit (Optionen)
Affinität wird über eines mehrere Korrelation und verfügt über zwei verschiedene Modi. Die häufigste Art der Affinität ist NonAlignedAffinity. In NonAlignedAffinity werden Replikate oder Instanzen von anderen Diensten auf demselben Knoten platziert. Der Modus ist AlignedAffinity. Ausgerichtete Affinität eignet sich nur für statusbehaftete Dienste. Konfigurieren von zwei statusbehaftete Services Affinität ausgerichtet haben sichergestellt, dass Primärfarben dieser Dienste auf demselben Knoten wie jeder andere platziert werden. Es wird auch jedes Paar von sekundären Server für diese Dienste auf demselben Knoten. Es kann auch (obwohl weniger häufig) NonAlignedAffinity für statusbehaftete Dienste konfigurieren. Für NonAlignedAffinity würden andere Replikate der beiden statusbehaftete Dienste auf demselben Knoten zusammengestellt werden, aber nicht versucht würde, ihre primäre oder sekundäre ausrichten.

![Affinität und ihre Effekten][Image1]

### <a name="best-effort-desired-state"></a>Beste Leistung gewünschte Status
Es gibt einige Unterschiede zwischen Affinität und monolithische Architektur. Viele davon sind ist eine Affinität Beziehung bemühen. Die Dienste in einer Beziehung Affinität sind grundlegend Entitäten, die unabhängig voneinander verschoben werden können. Es gibt auch Ursachen für eine Affinität Beziehung Warum brechen. Z. B. Kapazität eingeschränkt, auf einem bestimmten Knoten nur einige-Objekte in der Beziehung Affinität passen. In diesen Fällen, obwohl eine Affinität Beziehung gibt, kann es aufgrund der Einschränkungen erzwungen werden. Wenn es möglich ist, alle anderen Integritätsregeln und Zugehörigkeit zu einem späteren Zeitpunkt erzwungen wird gegen die Einschränkung Affinität automatisch korrigiert.  

### <a name="chains-vs-stars"></a>Ketten und Sterne
Heute können wir nicht Modell Ketten Affinität Beziehungen. Was bedeutet, dass ein Dienst, der ein Kind eine Affinität Beziehung kann nicht in einer anderen Affinität Beziehung sein. Wenn Sie diese Art von Beziehung modellieren möchten, müssen Sie effektiv als einen Stern eine Modell. Hierzu würde die unterste untergeordneten "middle" Kind stattdessen übergeordnet werden. Je nach Anordnung der Dienste erfordern diese "Placeholder" Dienst als übergeordnetes Element für mehrere Kinder zu erstellen.

![Ketten und Sterne Affinität Beziehungen][Image2]

Beachten Sie heute Affinität Beziehungen ist direktionales sind. Dies bedeutet, dass die Regel "Affinität" nur setzt das untergeordnete Element ist das übergeordnete Element. Wenn z. B. das übergeordnete plötzlich zu einem anderen Knoten ein Failover denken nicht Cluster Resource Manager tatsächlich, ist es etwas bis stellt fest, dass das Kind nicht mit einem übergeordneten befindet; die Beziehung wird nicht sofort erzwungen.

### <a name="partitioning-support"></a>Partitionierungsunterstützung
Die endgültige Affinität fällt ist, Affinität, Beziehungen unterstützt werden, in dem das übergeordnete partitioniert ist. Dies ist etwas, das wir schließlich unterstützen, heute ist nicht zulässig.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen zur Konfiguration Auschecken Thema andere Cluster Resource Manager Konfigurationen services verfügbaren [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Viele Gründe, wo Menschen Affinität, wie Dienste für ein kleines der Maschinen und Belastung der Dienste aggregieren möchten besser durch Gruppen unterstützt. [Anwendungsgruppen](service-fabric-cluster-resource-manager-application-groups.md) Auschecken

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png
