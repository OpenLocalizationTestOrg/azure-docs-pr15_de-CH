<properties
   pageTitle="Netzwerklastenausgleich-Cluster mit dem Azure Service Fabric Cluster Resource Manager | Microsoft Azure"
   description="Eine Einführung zum Netzwerklastenausgleich-Cluster mit Service Fabric Cluster-Ressourcen-Manager."
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

# <a name="balancing-your-service-fabric-cluster"></a>Netzwerklastenausgleich-Cluster Fabric service
Service Fabric Cluster Resource Manager ermöglicht dynamische Berichte reagieren auf den Cluster, Einschränkung Verstöße korrigiert und Cluster Ressourcenausgleich, falls erforderlich. Aber wie oft tut Folgendes, und was wird? Es gibt mehrere dazu.

Der erste Satz von Kontrollen Netzwerklastenausgleich sind Zeitgeber. Dieser Zeitgeber gesteuert, wie oft der Cluster-Ressourcen-Manager den Status des Clusters für Dinge untersucht, die behandelt werden müssen. Es gibt drei Kategorien von Arbeit mit eigenen entsprechenden. Sie sind:

1.  Position – Angebote dieser Phase mit statusbehaftete Replikate oder Staatenlose Instanzen fehlen. Dies umfasst sowohl neue Dienste behandeln statusbehaftete Replikate oder Staatenlose Instanzen sind ausgefallen und müssen neu erstellt werden. Löschen und Ablegen von Replikaten oder Instanzen ist auch hier behandelt.
2.  Einschränkung überprüft – dieser Phase überprüft und korrigiert Verstöße gegen die unterschiedlichen Nebenbedingungen (Regeln) im System. Beispiele hierfür sind, dass Knoten Kapazität nicht überschreiten und einen Dienst Platzierung Constraints (mehr später) erfüllt werden.
3.  Lastenausgleich – dieser Phase überprüft ist proaktives Ressourcenausgleich anhand der konfigurierten gewünschten Saldo für verschiedene Metriken und dann versucht, eine Anordnung im Cluster, die ausgewogener.

## <a name="configuring-cluster-resource-manager-steps-and-timers"></a>Konfigurieren von Cluster-Ressourcen-Manager Schritte und Zeitgeber
Diese verschiedenen Arten der Korrekturen können die Cluster-Ressourcen-Manager einen anderen Zeitgeber steuert Frequenz regelt. So möchten Sie nur Umgang mit neuen Service Arbeitslasten im Cluster jede Stunde (um sie von batch), wollen aber reguläre Netzwerklastenausgleich alle paar Sekunden überprüft, können Sie dieses Verhalten konfigurieren. Wenn der Zeitgeber ausgelöst wird, ist die Aufgabe geplant. Standardmäßig der Ressourcen-Manager überprüft den Zustand und wendet Updates (Batchverarbeitung alle Änderungen, die seit dem letzten Scan wie bemerken aufgetreten sind, die ein Knoten ausfällt) 1 10 einer Sekunde legt die Position und die Einschränkung überprüft Flags pro Sekunde und 5 Sekunden der Netzwerklastenausgleich kennzeichnen.

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Heute wir nur führen Sie eine der folgenden Aktionen jeweils nacheinander (deshalb diese Konfigurationen als "Mindestabstände" bezeichnet)). Dies ist, z. B. wir bereits, alle Anfragen geantwortet haben, bevor wir zu den Cluster neue Replikate erstellen. Siehe durch die standardmäßige Zeitintervalle angegeben, überprüfen wir und Kontrollkästchen für alles, was wir müssen häufig, sodass Änderungen wir am Ende jeder Schritt normalerweise kleiner: Wir sind Stunden Änderungen im Cluster nicht durchsuchen und gleichzeitig korrigieren möchten, wir versuchen, Dinge zu behandeln, mehr oder weniger wie sie jedoch einige Batchverarbeitung, wenn viele Dinge auf der gleichzeitig. Dadurch Service Fabric Resource Manager auf Dinge, die geschehen im Cluster.

Während die meisten dieser Aufgaben einfach (bei Einschränkung Verstöße, wenn Dienste erstellt werden sollen, erstellen sie Update), Cluster Resource Manager benötigt auch bestimmen, ob zusätzliche Informationen Arbeitsvorgänge Cluster. Dafür haben wir zwei Teile der Konfiguration: *Netzwerklastenausgleich Schwellenwerte* und *Aktivität Schwellenwerte*.

## <a name="balancing-thresholds"></a>Schwellenwerte für Lastenausgleich
Netzwerklastenausgleich ist ein Hauptbereich für das proaktive Ressourcenausgleich auslösen (Beachten Sie, dass der Zeitgeber genau wie oft Cluster Resource Manager überprüfen soll - es bedeutet, dass etwas geschieht). Der Netzwerklastenausgleich Schwellenwert definiert wie Arbeitsvorgänge Cluster muss für eine bestimmte Metrik der Cluster Ressourcen-Manager es unausgeglichenen und Auslöser Netzwerklastenausgleich.

Schwellenwerte für Lastenausgleich werden pro Metrik als Teil der Clusterdefinition definiert. Weitere Informationen zu Messgrößen Auschecken [dieses Artikels](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

``` xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Der Netzwerklastenausgleich Schwellenwert für eine Metrik ist ein Verhältnis. Laden auf die geladenen Knoten dividiert durch den Betrag der Last auf dem zuletzt geladenen Knoten überschreitet diese Nummer dann Cluster ist unausgewogen und Netzwerklastenausgleich ausgelöst nächsten überprüft Cluster Resource Manager (standardmäßig als durch MinLoadBalancingInterval oben je 5 Sekunden).

![Lastenausgleich Schwellenwert-Beispiel][Image1]

In diesem einfachen Beispiel belegt jeder Dienst eine Einheit einige Metrik. Im oberen Beispiel Höchstlast auf einem Knoten ist 5, und der Mindestwert ist 2. Angenommen, der Ausgleich für diese Metrik 3 ist. Deshalb im oberen Beispiel Cluster als ausgeglichen und kein Lastenausgleich ausgelöst, wenn der Cluster-Ressourcen-Manager überprüft (Verhältnis im Cluster 5/2 ist = 2,5 und weniger als ausgleichende Grenzwert 3).

Im Beispiel unten wird die Max. Auslastung auf einem Knoten 10 während mindestens 2, was zu einem Verhältnis von 5. Dadurch wird den Cluster entworfene Lastenausgleich Schwelle von 3 für die Metrik. Daher werden eine globale Korrektur ausführen geplanten nächsten Lastenausgleich Zeitgeber ausgelöst wird. Notiz, die nur weil Lastenausgleich Suche startete nichts bedeutet Verschieben - Cluster ist manchmal Arbeitsvorgänge aber die Situation nicht verbessert werden – aber in einer Situation wie dieser (mindestens standardmäßig) einige die Last sicherlich Knoten3 verteilt werden wird. Beachten Sie, da wir nicht gierigen Ansatz einige auch verteilt werden konnte auf Node2 Minimierung der Unterschiede zwischen Knoten führen würden, aber wir erwarten, dass der Großteil der Last Knoten3 zufließen wird.

![Netzwerklastenausgleich wird Schwellenwertaktionen][Image2]

Beachten Sie, dass die Schwelle Lastenausgleich kein explizites Ziel – Schwellenwerte für Lastenausgleich ist sind nur ein *Trigger* , der der Cluster-Ressourcen-Manager informiert, die im Cluster festzustellen welche Verbesserung machen können, gegebenenfalls sieht.

## <a name="activity-thresholds"></a>Aktivität-Schwellenwerte
Manchmal zwar Knoten relativ Arbeitsvorgänge sind *der Gesamtbetrag der Last im Cluster* niedrig. Dies könnte durch die Tageszeit, oder da Cluster neue und gerade erst neu gestartet. In beiden Fällen sollten Sie keine Zeit balancing Cluster tatsächlich wenig werden – Sie werden nur Netzwerk- und Rechenkapazität Ressourcen geht, ohne absoluten Unterschied verbringen. Da wir dies vermeiden möchten, gibt es ein anderes Steuerelement innerhalb der Ressourcen-Manager, bekannt als Aktivität Schwellenwerte ermöglicht Ihnen an einigen absoluten Untergrenze für Aktivität besitzt kein Knoten mindestens diese Last dann Netzwerklastenausgleich wird nicht ausgelöst, auch wenn der Netzwerklastenausgleich Schwellenwert erreicht wird.

Als Beispiel nehmen wir an, wir die folgenden Summen für den Verzehr auf diesen Knoten berichtet. Auch angenommen, dass wir unsere Netzwerklastenausgleich Schwellenwert 3 für diese Metrik beibehalten, aber jetzt wir auch eine Aktivität Schwellenwert 1536 haben. Im ersten Fall Cluster Arbeitsvorgänge pro Netzwerklastenausgleich Schwellenwert ist erfüllt keine Knoten, Aktivität Mindestschwellenwert damit wir, was allein. Im Beispiel unten ist Node1 Weg über den Schwellenwert Aktivität Lastenausgleich durchgeführt wird (da der Netzwerklastenausgleich Schwellenwert und Aktivität Schwellenwert für die Metrik überschritten)

![Aktivität Schwellenwert wird][Image3]

Wie Schwellenwerte für Lastenausgleich, Aktivität Schwellenwerte definierte pro-Metrik über die Clusterdefinition:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Beachten Sie, dass Lastenausgleich und Aktivitäten sind beide Metrik - Netzwerklastenausgleich gebunden nur ausgelöst werden, dieselbe Metrik Lastenausgleich und Aktivität Schwellenwerte überschritten werden. Damit wird wir den Netzwerklastenausgleich für Speicher und Aktivität Schwellenwert CPU Schwellenwert, Netzwerklastenausgleich nicht ausgelöst als verbleibende Schwellenwerte (Schwellenwert für CPU Netzwerklastenausgleich) und Aktivität Schwellenwert für den Speicher nicht überschritten werden.

## <a name="balancing-services-together"></a>Netzwerklastenausgleich Services zusammen
Interessant ist, ob der Cluster Arbeitsvorgänge ist für den gesamten Cluster Entscheidung, daß wir beheben geht wie verschieben, einzelnen Replikate und Instanzen um. Dies ist sinnvoll, richtig? Speicher auf einem Knoten mehrere Replikate gestapelt oder Instanzen könnte dazu beitragen, kann er verlangen statusbehaftete Replikate oder Staatenlose Instanzen, mit denen die betreffenden Arbeitsvorgänge Metrik verschieben.

Wenn ein Kunde uns oder Datei aufrufen haben gelegentlich eine Ticket besagt, dass ein Dienst, die Arbeitsvorgänge nicht verschoben. Wie kann es passieren, dass ein Dienst verschoben wird, selbst wenn alle diese Dienstmetrik ausgeglichen wurden, noch genau so Zeitpunkt die Diskrepanz? Mal sehen!

Nehmen Sie beispielsweise vier Dienste Service1, Service2, Dienst3 und Service4. Service1 Berichte mit Metriken Metric1 und Metric2, Service2 gegen Metric2 und Mmetric3, Dienst3 Metric3 und Metric4 und Service4 für einige metrische Metric99. Sicherlich können Sie sehen, wo wir hier gehen. Wir haben eine! Aus der Perspektive des Cluster-Managers haben nicht wirklich vier unabhängige Dienste haben wir eine Reihe von Dienstleistungen verwandte (Service1, Service2 und Dienst3) und off ist ein.

![Netzwerklastenausgleich Services zusammen][Image4]

So ist es möglich, dass ein Ungleichgewicht Metric1 Replikate oder Instanzen zur Dienst3 bewegen verursachen kann. Diese Sätze sind normalerweise ziemlich beschränkt jedoch kann je nach genau wie Arbeitsvorgänge Metric1 größer bekommen werden und Änderungen im Cluster erforderlich wären, um es zu korrigieren. Wir können auch mit Sicherheit sagen, dass Ungleichgewicht Metriken 1, 2 oder 3 wird nie Verbringung in Service4 – es keine wäre seit Replikate oder Instanzen von Service4 um kann nicht unbedingt zu den Saldo Metriken 1, 2 oder 3.

Cluster Resource Manager ermittelt automatisch, welche Services verwandt sind, da Dienste wurden hinzugefügt, entfernt, oder die metrischen Konfiguration ändern – z. B. hatte, zwischen zwei Service2 Netzwerklastenausgleich möglicherweise haben wurde konfiguriert Metric2 entfernen. Zwischen Service1 und Service2 wird unterbrochen. Sie haben nun statt zwei Gruppen von Diensten drei:

![Netzwerklastenausgleich Services zusammen][Image5]

## <a name="next-steps"></a>Nächste Schritte
- Metriken sind wie Service Fabric-Cluster Resource Manager Verbrauch und Kapazität im Cluster verwaltet. Erfahren Sie lesen mehr über sie und ihre Konfigurierung Sie [diesen Artikel](service-fabric-cluster-resource-manager-metrics.md)
- Bewegungskosten ist ein Cluster Ressourcen-Manager, dass bestimmte Dienste teurer als andere zu signalisieren. Erfahren Sie mehr über bewegungskosten finden Sie in [diesem Artikel](service-fabric-cluster-resource-manager-movement-cost.md)
- Der Cluster-Ressourcen-Manager hat mehrere Drosselungen verlangsamen Abwanderung im Cluster konfiguriert werden kann. Sie sind nicht erforderlich, aber sie benötigen Sie erfahren sie [hier](service-fabric-cluster-resource-manager-advanced-throttling.md)


[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
