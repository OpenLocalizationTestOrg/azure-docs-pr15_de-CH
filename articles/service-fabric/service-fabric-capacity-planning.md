<properties
   pageTitle="Planung für Service Fabric apps | Microsoft Azure"
   description="Beschreibt das Identifizieren von Compute-Knoten für einen Service Fabric-Anwendung erforderlich"
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="markfuss"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>


# <a name="capacity-planning-for-service-fabric-applications"></a>Planung für Service Fabric-Applikationen


Dieses Dokument erklärt die Ressourcen (CPU, Arbeitsspeicher, Festplattenspeicher) schätzen Sie Ihre Azure Service Fabric-Anwendung ausführen müssen. Es ist häufig der Ressourcenbedarf ändern. Sie benötigen in der Regel einige Ressourcen Ihren Dienst Entwicklung/Test und benötigen weitere Ressourcen in der Produktion und Sie Ihre Anwendung wächst Popularität. Beim Entwerfen der Anwendung erforderlichen langfristigen Denken Sie und Optionen Sie, die Ihr Dienst hohe Nachfrage zu skalieren.

 Beim Erstellen eines Clusters Fabric Service entscheiden Sie Cluster virtuellen Maschinen (VMs) machen. Jede VM verfügt über eine begrenzte Menge an Ressourcen in Form von CPUs (Kerne und Geschwindigkeit), Netzwerkbandbreite, RAM und Festplattenspeicher. Wie der Dienst mit der Zeit wächst, können Sie auf VMs aktualisieren, die mehr Ressourcen oder mehr VMs zum Cluster hinzufügen. Letztere Möglichkeit müssen Sie den Dienst zunächst Architekt, damit sie neue VMs nutzen können, die dem Cluster dynamisch hinzugefügt werden.

Einige Dienste verwalten wenig keine Daten auf den virtuellen Computern selbst. Daher sollte für diese Dienste Planung hauptsächlich auf Leistung konzentrieren auswählen die entsprechenden CPUs (Kerne und Geschwindigkeit) der VMs. Darüber hinaus sollten Sie die Netzwerkbandbreite, einschließlich Netzwerk überträgt wie häufig auftreten und wie viele Daten übertragen werden. Wenn Ihr Dienst sowie Service-Verwendung erhöht ausführen muss, können Sie Netzwerk über alle VMs fordert mehr VMs Cluster und Load Balance hinzufügen.

Für Dienste, die große Datenmengen auf VMs verwalten, sollten die Planung auf Größe konzentrieren. Daher sollten Sie sorgfältig die Kapazität des virtuellen Computers RAM und Festplattenspeicher. Virtueller Speicher-Management-System in Windows wird Speicherplatz an RAM aussehen. Darüber hinaus die Runtime Service Fabric smart Paging nur aktive Daten im Speicher halten und kalten Daten auf der Festplatte verschieben. Clientanwendungen können so mehr Arbeitsspeicher als auf dem virtuellen Computer physisch verfügbar ist. Mehr RAM erhöht einfach Leistung, da die VM mehr Speicherplatz im Arbeitsspeicher beibehalten kann. Die gewählte VM müssen eine Festplatte ausreicht, um die Daten auf dem virtuellen Computer speichern. Ebenso müssen die VM genügend RAM bieten die Leistung, die Sie wünschen. Wenn Ihre-Daten mit der Zeit wächst, können Sie mehrere VMs Cluster und Partitionieren der Daten über alle VMs hinzufügen.

## <a name="determine-how-many-nodes-you-need"></a>Bestimmen Sie wie viele Knoten

Partitionierung der Service können Sie Ihren Dienst Daten skalieren. Weitere Informationen zu partitionieren finden Sie unter [Service Fabric Partitionierung](service-fabric-concepts-partitioning.md). Jede Partition muss in einer einzigen VM passen, aber mehrere Partitionen (kleine) auf einer einzelnen VM platziert werden. So kann mehrere kleine Partitionen Sie flexibler als ein paar größere Partitionen. Der Nachteil ist, dass viele Partitionen Service Fabric Aufwand erhöht und nicht durchgeführte Operationen über Partitionen möglich. Gibt auch weitere mögliche Netzwerkverkehr Servicecode muss häufig auf Daten zugreifen, die auf unterschiedlichen Partitionen befinden. Beim Entwerfen der Service sollten Sie sorgfältig die vor- und Nachteile einer effektiven Strategie zur Festplattenpartitionierung ankommen.

Angenommen, Ihre Anwendung einen statusbehafteten Dienst wurde mit einer Größe, die erwartete DB_Size GB pro Jahr wachsen. Sie wollen mehr Applikationen (und Partitionen) Wachstum über dieses Jahr kommen.  Der replikationsfaktor (RF) bestimmt die Anzahl der Replikate für Ihren Dienst insgesamt DB_Size auswirkt. Die gesamte DB_Size in allen Replikaten ist die Replikation DB_Size multipliziert.  Node_Size stellt den Datenträger Speicherplatz/RAM pro Knoten für den Dienst verwenden möchten. Für eine optimale Leistung sollte der DB_Size in den Speicher im Cluster, und eine um RAM der VM Node_Size gewählt werden. Durch einen Node_Size, der größer als der RAM-Kapazität reservieren, setzen Sie auf Paging durch die Runtime Service Fabric bereitgestellt. Folglich die Leistung möglicherweise nicht optimal, wenn Ihre gesamten Daten gilt hot (seit die Daten werden ausgelagert /). Allerdings ist es für viele Dienste hot nur ein Bruchteil der Daten ist kostengünstiger.

Die Anzahl der Knoten für maximale Leistung erforderlich kann wie folgt berechnet:

```
Number of Nodes = (DB_Size * RF)/Node_Size

```


## <a name="account-for-growth"></a>Konto für Wachstum

Sie möchten die Anzahl der Knoten, der den Dienst weiter, neben der DB_Size, die mit beginnt man DB_Size berechnen. Dann wachsen Sie die Anzahl der Knoten der Dienst so, dass Sie die Anzahl der Knoten nicht über bereitstellen. Aber die Anzahl der Partitionen sollte basierend auf der Anzahl der Knoten, die erforderlich sind, wenn Sie den Dienst auf maximale ausführen.

Es empfiehlt sich, einige zusätzliche Computer jederzeit damit Sie unerwartete Spitzen oder Fehler verarbeiten können (z. B. wenn einige VMs ausfallen).  Zusätzliche Kapazität bestimmt werden soll, mithilfe der erwarteten Spitzen Ausgangspunkt zwar einige zusätzliche VMs reserviert (5 bis 10 Prozent zusätzliche).

Die obige übernimmt einen einzelnen statusbehafteten Dienst. Haben Sie mehr als eine statusbehaftete Dienst müssen Sie DB_Size verknüpft mit anderen Diensten in der Formel hinzufügen. Alternativ können Sie die Anzahl der Knoten für jeden statusbehaftete Dienst berechnen.  Der Dienst möglicherweise Replikate oder Partitionen sind nicht ausgeglichen. Denken Sie daran, dass Partitionen auch mehr Daten als andere. Weitere Informationen zu partitionieren finden Sie unter [Artikel best Practices Partitionierung](service-fabric-concepts-partitioning.md). Die Gleichung oben ist Partition und Replikat unabhängig, da Fabric Service stellt sicher, dass die Replikate in optimierter Weise zwischen den Knoten verteilt werden.


## <a name="use-a-spreadsheet-for-cost-calculation"></a>Verwenden einer Kalkulationstabelle zur Berechnung

Jetzt sehen wir uns einige reelle Zahlen in der Formel. Ein [Beispiel Tabelle](https://servicefabricsdkstorage.blob.core.windows.net/publicrelease/SF%20VM%20Cost%20calculator-NEW.xlsx) veranschaulicht zum Planen der Kapazität für eine Anwendung, die drei Typen von Objekten enthält. Für jedes Objekt ungefähre wir ihre Größe und wie viele Objekte haben. Wir wählen Sie, wie viele Replikate, die wir jeden Objekttyp möchten. Die Kalkulationstabelle berechnet die Gesamtmenge an Speicher im Cluster gespeichert werden.

Geben wir eine VM-Größe und monatlich. Anhand der VM-Größe, weist die Kalkulationstabelle die minimale Anzahl von Partitionen, die Sie verwenden müssen, die Daten physisch auf den Knoten anpassen. Sie können eine größere Anzahl von Partitionen, die die Anwendung bestimmte Berechnung und Netzwerkverkehr benötigt. Die Tabelle zeigt die Anzahl der Partitionen, die Benutzerobjekte Profil verwalten ein bis sechs erhöht hat.

Zeigt anhand dieser Informationen die Kalkulationstabelle, physisch die Daten mit der gewünschten Partitionen und Replikate auf 26 Stationen erhalten konnte. Jedoch würde diese Cluster dicht gepackt werden eventuell einige zusätzlichen Knoten Knotenausfälle und Upgrades. Die Tabelle zeigt auch mit mehr als 57 Knoten keinen zusätzlichen Schutz bietet, da leere Knoten würde. Sie möchten wieder über 57 Knoten gehen trotzdem Knotenausfälle und Upgrades. Sie können die Kalkulationstabelle, um die Anwendung speziellen Bedürfnisse anpassen.   

![Tabelle für die Berechnung][Image1]



## <a name="next-steps"></a>Nächste Schritte

[Partitionierung Service Fabric-Services] Auschecken[ 10] Partitionierung Ihren Dienst erfahren.



<!--Image references-->
[Image1]: ./media/SF-Cost.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-concepts-partitioning.md
