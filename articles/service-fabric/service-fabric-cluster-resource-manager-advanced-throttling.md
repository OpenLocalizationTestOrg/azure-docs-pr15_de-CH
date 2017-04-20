<properties
   pageTitle="Drosselung Service Fabric-Cluster Resource Manager | Microsoft Azure"
   description="Informationen Sie zum Drosselung bereitgestellt vom Fabric Cluster Resource Manager konfigurieren."
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


# <a name="throttling-the-behavior-of-the-service-fabric-cluster-resource-manager"></a>Das Verhalten von Service Fabric Cluster Resource Manager Drosselung
Auch wenn Sie die Cluster-Ressourcen-Manager ordnungsgemäß konfiguriert haben, kann Cluster gestört erhalten. Es kann z. B. gleichzeitig Knoten oder Fehler Domäne Fehler – was passiert, wenn während einer Aktualisierung aufgetreten? Der Ressourcen-Manager versucht am besten beheben, aber in Zeiten möchten eine Abfangfunktion berücksichtigen, damit der Cluster selbst stabilisieren kann (welche kommen wieder Knoten die Netzwerkprobleme selbst heilen, korrigierte Bits Implementierung). Bei derartigen Situationen umfasst Service Fabric Cluster Resource Manager verschiedene drosseln. Beachten Sie, dass diese Drosselungen sehr störend sind und generell nicht verwendet werden, wenn gab es einige sorgfältige mathematische Menge parallele Arbeit tatsächlich im Cluster sowie eine kann durchgeführt müssen diese reagieren möglichen (HM) makroskopischen Neukonfiguration ungeplante Ereignisse (AKA: "Sehr schlechte Tage").

Im Allgemeinen empfehlen wir vermeiden sehr schlechte Tage durch andere Optionen (wie reguläre Code-Updates und sorgen Cluster zu vermeiden) als Cluster um zu verhindern, verwenden von Ressourcen beim selbst beheben Drosselung). Drosselung haben Standardwerte, die über Erfahrung ok Standardwerte gefunden, aber Sie wahrscheinlich sehen und der erwarteten Belastung einstellen. Obwohl nicht allzu oder Clusters empfohlen, Sie können feststellen wird, ob, laden Fällen (bis Sie sie beheben können) dauert sollten Sie ein paar Drosselungen haben auch wenn Cluster stabilisieren länger.

##<a name="configuring-the-throttles"></a>Konfigurieren der Drosselung
Drosselung standardmäßig enthalten sind:

-   GlobalMovementThrottleThreshold – steuert diese Verbringung im Cluster insgesamt über einige Zeit (definiert als GlobalMovementThrottleCountingInterval Wert in Sekunden)
-   MovementPerPartitionThrottleThreshold – Hiermit die Gesamtzahl der Verbringung Servicepartition über einige Zeit (die MovementPerPartitionThrottleCountingInterval, Wert in Sekunden)

``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThreshold" Value="1000" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
     <Parameter Name="MovementPerPartitionThrottleThreshold" Value="50" />
     <Parameter Name="MovementPerPartitionThrottleCountingInterval" Value="600" />
</Section>
```

Beachten Sie, dass meistens gesehen haben Kunden diese Drosselungen wurde bereits in einer Umgebung mit eingeschränkten Ressourcen waren (z. B. begrenzte Bandbreite in einzelne Knoten oder Festplatten die Anforderungen parallel Replikat erstellt, die ihnen gestellt wurden) die diese Vorgänge würde nicht erfolgreich oder wäre trotzdem langsam.  In diesen Fällen wurden Kunden vertraut kennen, die sie möglicherweise den Zeitraum erweitern, würde den Cluster zu einem stabilen Zustand einschließlich kennen, die sie mit niedrigeren Zuverlässigkeit während sie gedrosselt wurden Ende konnte.

## <a name="next-steps"></a>Nächste Schritte
- Um herauszufinden, wie die Cluster-Ressourcen-Manager verwaltet und verteilt die Last im Cluster, Artikel auf [Last](service-fabric-cluster-resource-manager-balancing.md) Auschecken
- Der Cluster-Ressourcen-Manager hat viele Optionen für die Beschreibung des Clusters. Erfahren Sie lesen mehr darüber Sie diesen Artikel zu [einen Cluster Service Fabric beschreiben](service-fabric-cluster-resource-manager-cluster-description.md)
