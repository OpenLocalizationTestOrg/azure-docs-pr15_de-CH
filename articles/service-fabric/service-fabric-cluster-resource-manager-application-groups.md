<properties
   pageTitle="Service Fabric Cluster Resource Manager - Gruppen | Microsoft Azure"
   description="Übersicht über die Anwendungsgruppe Funktionen im Fabric Cluster Resource Manager"
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

# <a name="introduction-to-application-groups"></a>Einführung in Gruppen
Service Fabric Cluster Resource Manager verwaltet Ressourcen normalerweise durch das Verteilen der Last (über Metriken dargestellt) gleichmäßig im Cluster. Fabric-Dienst verwaltet außerdem die Kapazität der Knoten in dem Cluster zu insgesamt über das Konzept der Kapazität. Dies funktioniert gut für viele verschiedene Arbeitslasten aber Muster, die verschiedenen Service Fabric Anwendungsinstanzen manchmal zusätzliche Vorschriften bringen durchführen. Einige zusätzlichen Vorschriften sind in der Regel:

- Möglichkeit für eine Anwendungsinstanz auf Anzahl Knoten Kapazität reservieren
- Die Gesamtzahl der Knoten begrenzen, die ein bestimmten Satz von Diensten innerhalb einer Anwendung ausgeführt wird
- Kapazitäten auf die Anwendungsinstanz selbst definieren, um die Nummer oder den gesamten Ressourcenverbrauch Dienste darin beschränken

Unterstützung für Gruppen Wir nennen entwickelten wir diese Anforderungen.

## <a name="managing-application-capacity"></a>Anwendung Kapazität
Anwendungskapazität kann verwendet werden, um die Anzahl der Knoten, die von einer Anwendung sowie die metrischen Gesamtlast, die Anwendung Instanzen auf einzelne Knoten erstreckt. Auch kann verwendet werden, um Ressourcen im Cluster für die Anwendung reserviert.

Anwendungskapazität kann für neue Applikationen festgelegt werden, wenn sie erstellt werden. Es kann auch für vorhandene Anwendung en aktualisiert werden, die ohne Anwendungskapazität erstellt wurden.

### <a name="limiting-the-maximum-number-of-nodes"></a>Die maximale Anzahl von Knoten beschränkt
Der einfachste Anwendungsfall Anwendungskapazität wird eine Anwendung Instanziierung muss auf eine bestimmte maximale Anzahl von Knoten. Wenn keine Anwendungskapazität angegeben, Service Fabric Cluster Resource Manager instanziiert Replikate normalen Regeln Netzwerklastenausgleich oder Defragmentierung, wodurch normalerweise die Dienste auf allen verfügbaren Knoten im Cluster verteilt werden, oder Defragmentierung auf beliebigen aber kleinere Anzahl Knoten aktiviert ist.

Das folgende Bild zeigt mögliche Platzierung einer Anwendungsinstanz ohne die maximale Anzahl von Knoten und dann dieselbe Anwendung mit einer maximalen Anzahl von Knoten. Beachten Sie, dass es keine Garantien über die Replikate oder Instanzen der Services zusammen platziert werden.

![Maximale Anzahl von Knoten definieren Anwendungsinstanz][Image1]

Im linken Beispiel die Anwendung keinen Anwendungskapazität festgelegt und hat drei Dienste. CRM hat eine logische Entscheidung alle Replikate über sechs verfügbaren Knoten verteilt um das optimale Gleichgewicht im Cluster. Im rechten Beispiel sehen wir dieselbe Anwendung auf drei Knoten beschränkt ist und wo erreicht Service Fabric CRM den besten Ausgleich für die Replikate der Anwendungsdienste.

Der Parameter, der dies steuert, heißt MaximumNodes. Dieser Parameter kann beim Erstellen der Anwendung festgelegt oder für eine Instanz der Anwendung dem bereits ausgeführt wurde, in der Fall Service Fabric CRM Replikate Anwendungsdienste auf definierte maximale Anzahl von Knoten beschränken aktualisiert werden.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;

adUpdate.Metrics.Add(appMetric);
```

## <a name="application-metrics-load-and-capacity"></a>Anwendungswerte laden und Kapazität
Anwendungsgruppen können Sie Metriken mit bestimmten Anwendungsinstanz die Kapazität der Anwendung im Hinblick auf diese Metriken definieren. So könnte Sie, die definieren, wie viele Dienste wie gewünscht in erstellt werden konnte

Für jede Metrik gibt 2 Werte einstellbare Kapazität für diese Anwendungsinstanz beschreiben:

-   Gesamtanzahl der Anwendung Kapazität – die Gesamtkapazität der Anwendung für eine bestimmte Metrik darstellt. Service Fabric CRM versucht die Summe metrische diese Anwendungsdienste auf den angegebenen Wert beschränkt. Darüber hinaus die Anwendungsdienste bereits bis zu dieser Last verbrauchen, wird Service Fabric Cluster Resource Manager nicht zuzulassen, wenn die Erstellung neuer Services oder Partitionen, die Gesamtlast auf diesen Grenzwert überschreiten würde.
-   Knotenkapazität – gibt die maximale Gesamtlast für Replikate die Anwendung Dienste auf einem einzelnen Knoten. Wenn diese Kapazität Gesamtlast auf dem Knoten überschreitet, versucht Service Fabric CRM Replikate zu anderen Knoten zu verschieben, damit die kapazitätseinschränkung berücksichtigt wird.

## <a name="reserving-capacity"></a>Reservierung von Kapazität
Eine andere häufige Verwendung von Gruppen wird sichergestellt, dass innerhalb des Clusters für eine bestimmte Anwendungsinstanz reserviert werden, auch wenn die Instanz der Anwendung enthaltenen Dienste noch nicht oder auch wenn die Ressourcen noch genutzt werden nicht. Schauen Sie auf wie das funktionieren würde.  

### <a name="specifying-a-minimum-number-of-nodes-and-resource-reservation"></a>Eine Mindestanzahl von Knoten und Resource Reservation angeben
Reservieren von Ressourcen für eine Instanz der Anwendung erfordert ein paar zusätzliche Parameter festlegen: *MinimumNodes* und *NodeReservationCapacity*

- MinimumNodes - Angabe Ziel maximal Knoten Dienste innerhalb einer Anwendung ausgeführt werden können, wie auch die Mindestanzahl der Knoten soll, die eine Anwendung ausgeführt werden soll. Diese Einstellung definiert effektiv die Anzahl der Knoten die Ressourcen auf mindestens reserviert werden soll Kapazität innerhalb des Clusters als Teil einer Instanz der Anwendung zu gewährleisten.
- NodeReservationCapacity - die NodeReservationCapacity für jede Metrik in der Anwendung definiert werden. Dies definiert den metrischen Laden der Anwendung auf jedem Knoten alle Replikate oder Instanzen der enthaltenen Dienste platziert reserviert.

Betrachten wir nun ein Beispiel für die Reservierung von Kapazität:

![Anwendungsinstanzen reservierten Kapazität definieren][Image2]

Im linken Beispiel Applikationen Anwendungskapazität definiert keinen. Service Fabric Cluster Resource Manager wird die Anwendung untergeordnete Dienste Replikate und Instanzen zusammen mit anderen Diensten (außerhalb der Anwendung) ausgeglichen, Saldo des Clusters sicherzustellen.

Im Beispiel rechts angenommen, die Anwendung wurde mit einer MinimumNodes auf 2 festlegen, legen auf 3 und einer Anwendung Metrik Reservierung 20, maximale Speicherkapazität auf einem Knoten 50 und einer gesamten Kapazität von 100 MaximumNodes Service Fabric Kapazität auf zwei Knoten für die blauen Anwendung reserviert und verhindert, dass Replikaten der Cluster, Kapazität verbraucht. Diese reservierte Anwendungskapazität wird verbraucht und gegen die verbleibenden Cluster Kapazität gezählten betrachtet.

Beim Erstellen eine Anwendung mit Reservierung Reservieren Cluster Resource Manager Kapazität gleich MinimumNodes * NodeReservationCapacity Cluster jedoch nicht auf bestimmte Knoten reserviert, bis die Replikate der Anwendungsdienste erstellt und platziert werden. Dies ermöglicht Flexibilität, da Knoten für neue Replikate ausgewählt werden nur, wenn sie erstellt werden. Kapazität ist auf einem bestimmten Knoten reserviert, wenn mindestens ein Replikat auf platziert wird.

## <a name="obtaining-the-application-load-information"></a>Anwendung Informationen laden
Für jede Anwendung erhalten Anwendungskapazität definiert hat Informationen aggregate laden lt. Replikate Dienste. Fabric Service bietet zu diesem Zweck PowerShell und verwaltete API Abfragen.

Laden kann beispielsweise mit dem folgenden PowerShell-Cmdlet abgerufen werden:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1

```

Die Ausgabe dieser Abfrage enthält die grundlegende Informationen Anwendungskapazität, die für die Anwendung wie minimale und maximale Knoten angegeben wurde. Außerdem werden Informationen über die Anzahl der Knoten, die die Anwendung derzeit verwendet wird. Damit für jede Metrik Laden werden Informationen über:
- Metrische Name: Name der Metrik.
-   Reservierung Kapazität: Cluster-Kapazität, die für diese Anwendung im Cluster reserviert ist.
-   Anwendung laden: Gesamtlast diese Anwendung untergeordnete Replikate.
-   Anwendungskapazität: Zulässige Wert der Anwendung.

## <a name="removing-application-capacity"></a>Anwendungskapazität entfernen
Sobald die Anwendungskapazität für eine Anwendung festgelegt wurden, können sie mit Update Anwendung APIs oder PowerShell-Cmdlets entfernt werden. Zum Beispiel:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Dieser Befehl entfernt alle Anwendungskapazität aus der Anwendung und Service Fabric Cluster Resource Manager startet diese Anwendung als jede andere Anwendung, die nicht definierten Parameter für diesen Cluster behandelt. Der Befehl ist sofort und Cluster Resource Manager löscht alle Anwendungskapazität Parameter für diese Anwendung. erneut angeben müssten Update Anwendung APIs mit den entsprechenden Parametern aufgerufen werden.

## <a name="restrictions-on-application-capacity"></a>Beschränkung auf Anwendung
Es gibt mehrere Einschränkungen Anwendungskapazität Parameter, die berücksichtigt werden müssen. Bei Validierungsfehler werden Erstellung oder Aktualisierung der Anwendung mit einer Fehlermeldung zurückgewiesen.
Alle ganzzahligen Parameter müssen positive Zahlen sein.
Darüber hinaus sind für die einzelnen Parameter eingeschränkt:

-   MinimumNodes darf nie größer als MaximumNodes sein.
-   Kapazitäten für eine Metrik Last definiert, müssen sie die folgenden Regeln beachten:
  - Knoten Reservierung Kapazität darf nicht größer als die Maximalkapazität der Knoten sein. Sie können nicht z. B. Kapazität Metrik "CPU" auf dem Knoten 2 Einheiten beschränken und versuchen, 3 Einheiten auf jedem Knoten zu reservieren.
  - Wenn MaximumNodes angegeben wird, muss das Produkt aus MaximumNodes und Kapazität Knoten nicht Gesamtkapazität Anwendung größer sein. Beispielsweise müssen Festlegen der Maximalkapazität Knoten laden Metrik "CPU" 8 und die maximale Anzahl von Knoten auf 10 festgelegt Gesamtkapazität Anwendung größer als 80 für diese Metrik laden sein.

Die Einschränkungen werden beim Erstellen der Anwendung (clientseitig), sowohl Anwendungsupdate (serverseitig) erzwungen. Während der Erstellung ist dies ein Beispiel für ein klarer Verstoß gegen die Vorschriften seit MaximumNodes < MinimumNodes und der Befehl schlägt fehl auf dem Client vor die Anforderung selbst Service Fabric-Cluster:

``` posh
New-ServiceFabricApplication –Name fabric:/MyApplication1 –MinimumNodes 6 –MaximumNodes 2
```

Beispiele für ungültige Aktualisierung lautet wie folgt. Wenn eine vorhandene Anwendung und maximale Knotenanzahl auf einen Wert aktualisieren, wird das Update übergeben:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MaximumNodes 2
```

Danach können wir versuchen, minimale Knoten zu aktualisieren:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 6 –MinimumNodes 6
```

Der Client verfügt nicht genügend Kontext zur Anwendung damit Update Service Fabric-Cluster übergeben können. Der Cluster Service Fabric überprüft den neuen Parameter mit den vorhandenen Parametern und den Aktualisierungsvorgang schlägt fehl, da die minimale Feind Wertknoten ist größer als der Wert für die maximale Anzahl von Knoten. In diesem Fall bleibt Anwendungskapazität Parameter.

Diese Einschränkung werden damit für Cluster Resource Manager die optimale Platzierung für Replikate der Anwendung Dienste können eingeführt.

## <a name="how-not-to-use-application-capacity"></a>Verwendung nicht Anwendungskapazität

-   Verwenden Sie nicht die Anwendungskapazität zum Beschränken der Anwendung auf eine bestimmte Teilmenge von Knoten: Obwohl Service Fabric wird Einhaltung maximale Knoten für jede Anwendung, die Anwendungskapazität angegeben, können keine Benutzer welche Knoten entscheiden auf instanziiert. Dies kann erreicht werden, Platzierung Integritätsregeln für Dienste verwenden.
-   Verwenden Sie nicht die Kapazität der Anwendung sicherstellen, dass zwei Dienste aus derselben Anwendung immer nebeneinander platziert werden. Dies kann mit Affinität Beziehung zwischen Diensten und Affinität kann nur auf die Dienste, die zusammen tatsächlich platziert werden soll.

## <a name="next-steps"></a>Nächste Schritte
- Für Weitere Informationen zu anderen Optionen zur Konfiguration Auschecken Thema andere Cluster Resource Manager Konfigurationen services verfügbaren [Informationen zum Konfigurieren von Diensten](service-fabric-cluster-resource-manager-configure-services.md)
- Um herauszufinden, wie die Cluster-Ressourcen-Manager verwaltet und verteilt die Last im Cluster, Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md) Auschecken
- Von Anfang an und [erhalten eine Einführung in Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md) starten
- Weitere Informationen zur Funktionsweise von Messgrößen im Allgemeinen nachlesen Sie [Dienstmetrik Fabric laden](service-fabric-cluster-resource-manager-metrics.md)
- Der Cluster-Ressourcen-Manager hat viele Optionen für die Beschreibung des Clusters. Erfahren Sie lesen mehr darüber Sie diesen Artikel zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)


[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
