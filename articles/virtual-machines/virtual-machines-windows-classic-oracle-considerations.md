<properties
pageTitle="Aspekte der Verwendung von Oracle VM Bilder | Microsoft Azure"
description="Informationen Sie zu unterstützten Konfigurationen und Grenzen für eine Oracle-VM auf Windows Azure Server vor der Bereitstellung."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Verschiedene Aspekte der Oracle virtuellen Computerimages



Dieser Artikel enthält Hinweise für Oracle virtuelle Computer in Azure basieren auf Oracle Software-Images von Oracle bereitgestellt.  

-  Oracle Datenbank virtuellen Computerimages
-  Oracle WebLogic Server virtuellen Computerimages
-  Oracle JDK virtuellen Computerimages

##<a name="oracle-database-virtual-machine-images"></a>Oracle Datenbank virtuellen Computerimages
### <a name="clustering-rac-is-not-supported"></a>Cluster (RAC) wird nicht unterstützt.

Azure unterstützt zurzeit keine Oracle Real Application Cluster (RAC) von Oracle-Datenbank. Nur eigenständige Instanzen der Oracle-Datenbank sind möglich. Dies ist Azure derzeit nicht virtuellen Datenträger-Freigabe auf Lese-/Schreibzugriff zwischen mehreren virtuellen Instanzen unterstützt. Multicast UDP wird ebenfalls nicht unterstützt.

### <a name="no-static-internal-ip"></a>Keine statische interne IP-Adresse

Azure weist jedem virtuellen Computer eine interne IP-Adresse. Wenn der virtuelle Computer ein virtuelles Netzwerk gehört, wird die IP-Adresse des virtuellen Computers ist dynamisch und ändert, nachdem der virtuelle Computer wird neu gestartet. Dies kann Probleme verursachen, da der Oracle-Datenbank die IP-Adresse statisch erwartet. Um dieses Problem zu vermeiden, sinnvoll, den virtuellen Computer ein virtuelles Azure-Netzwerk. Weitere Informationen finden Sie unter [Virtuelles Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) und [erstellen ein virtuelles Netzwerk in Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="attached-disk-configuration-options"></a>Konfigurationsoptionen angeschlossenen Datenträger

Sie können Dateien auf dem Betriebssystem vom Datenträger des virtuellen Computers oder angeschlossenen Festplatten auch Datenträger platzieren. Angeschlossene Laufwerke bieten möglicherweise eine bessere Leistung und Flexibilität als Betriebssystem-Datenträger. Der Betriebssystem-Datenträger kann nur für Datenbanken unter 10 Gigabyte (GB) sein.

Angeschlossene Laufwerke basieren auf der Dienst Azure BLOB-Speicher. Jedes Laufwerk kann theoretisch bis zu 500 e/a-Operationen pro Sekunde (IOPS). Die Leistung der angeschlossenen Laufwerke möglicherweise nicht optimal zunächst und IOPS-Leistung kann nach einer "Burn-in" 60-90 Minuten Betrieb erheblich verbessern. Eine Festplatte bleibt anschließend im Leerlauf kann IOPS-Leistung bis zu einer anderen Burn-in-Dauerbetrieb verringern. Kurz gesagt, soll aktiver eine Festplatte, desto wahrscheinlicher Ansatz optimale IOPS-Leistung.

Zwar den virtuellen Computer eine Festplatte zuordnen und Datenbankdateien auf dem Datenträger am einfachsten ist dieser Ansatz auch die Begrenzung hinsichtlich der Leistung. Stattdessen können Sie häufig effektive IOPS-Leistung verbessern, wenn Sie mehrere angeschlossenen Festplatten Daten verteilt und Oracle Automatic Storage Management (ASM) verwenden. Weitere Informationen finden Sie unter [Oracle Automatic Storage Overview](http://www.oracle.com/technetwork/database/index-100339.html) . Auch Striping mehrerer Festplatten auf Betriebssystemebene verwendet werden kann, wird dieser Ansatz abgeraten, da nicht bekannt ist, IOPS Leistung.

Betrachten Sie zwei verschiedene Ansätze für das Anhängen von mehreren Datenträgern basierend auf Leistung Lesevorgänge priorisieren oder Schreibvorgänge für die Datenbank werden soll:

- **Oracle ASM allein** ist wahrscheinlich bessere Schreib-Performance Vorgang jedoch schlimmer IO/s für Lesevorgänge gegenüber der Ansatz mit Festplatten-Arrays. Die folgende Abbildung veranschaulicht diese Anordnung logisch.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] Kompromiss zwischen Schreib-Performance und Leistung bei Lesevorgängen auf einem Fall ausgewertet. Die tatsächlichen Ergebnisse können Sie bei diesem variieren.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Hohe Verfügbarkeit und Disaster Recovery-Hinweise

Bei Oracle-Datenbanken in Azure virtuelle Computer sind Sie verantwortlich für die Implementierung einer hohen Verfügbarkeit und Disaster Recovery-Lösung, um Ausfallzeiten zu vermeiden. Sie sind auch für das Sichern Ihrer eigenen Daten und Anwendung verantwortlich.

Hohe Verfügbarkeit und Disaster Recovery für Oracle Database Enterprise Edition (ohne RAC) auf Azure lässt zwei Datenbanken auf zwei separaten virtuellen Maschinen mit [Data Guard Active Data Guard](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)und [Oracle Golden Gate](http://www.oracle.com/technetwork/middleware/goldengate). Beide virtuelle Computer soll in der gleichen [Cloud-Dienst](virtual-machines-linux-classic-connect-vms.md) und im gleichen [virtuellen Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) sicherstellen, dass sie einander über den permanenten privaten IP-Adresse zugreifen können.  Darüber hinaus empfehlen wir die virtuellen Computer im gleichen [Verfügbarkeit](virtual-machines-windows-manage-availability.md) zu Azure in separaten Fehlerdomänen und Domänen zu aktualisieren. Nur virtuelle Computer im selben Cloud-Dienst können dieselben Verfügbarkeit teilnehmen. Jeder virtuelle Computer muss über mindestens 2 GB Arbeitsspeicher und 5 GB Speicherplatz.

Oracle Data Guard hoher Verfügbarkeit lässt sich mit einer primären Datenbank auf einem virtuellen Computer eine sekundäre (standby) Datenbank auf einem anderen virtuellen Computer und unidirektionalen Replikation zwischen ihnen. Das Ergebnis ist Lesezugriff auf die Kopie der Datenbank. Mit Oracle Golden Gate können Sie bidirektionale Replikation zwischen den beiden Datenbanken konfigurieren. Zum Einrichten einer Lösung mit hoher Verfügbarkeit für Datenbanken mit diesen Tools finden Sie unter [Active Data Guard](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) und [Golden Gate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) -Dokumentation auf der Oracle-Website. Wenn Sie Zugriff auf die Kopie der Datenbank Schreibzugriff benötigen, können Sie [Oracle Active Data Guard](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Oracle WebLogic Server virtuellen Computerimages

-  **Clustering ist nur in Enterprise Edition unterstützt.** Sie sind nur bei Verwendung der Enterprise Edition von WebLogic Server WebLogic clustering Verwendung lizenziert. Verwenden Sie clustering mit WebLogic Server Standard Edition.

-  **Verbindungstimeouts:** Wenn Ihre Anwendung auf Öffentliche Endpunkte der anderen Azure-Cloud-Dienst beispielsweise eine Datenbank Tier, möglicherweise Azure nach vier Minuten Inaktivität öffnen Verbindung schließen. Diese beeinträchtigt Features und Programme auf Verbindungspools, die Verbindungen für über diese Beschränkung nicht mehr gültig bleiben können. Wenn dies die Anwendung sollten Sie "Keep-alive" Logik der Verbindungspools in Betracht ziehen.

    Ist ein Endpunkt *interne* der Azure-Cloud-Service-Bereitstellung (wie eigenständige Datenbank virtuelle Computer im *selben* Cloud-Dienst als die WebLogic virtuellen Computer), dann die Verbindung direkt nicht von Azure Lastenausgleich abhängig und daher unterliegt einem Verbindungstimeout.

-  **UDP-Multicast wird nicht unterstützt.** Azure unterstützt UDP Unicasting nicht Multicasting oder senden. WebLogic Server kann sich auf Azure UDP-Unicast. Für optimale Ergebnisse auf UDP-Unicast empfohlen Clustergröße WebLogic statisch gehalten werden oder nicht mehr als 10 verwaltete Server im Cluster enthalten.

-  **WebLogic Server erwartet öffentliche und private Ports auf T3 (z. B. bei Verwendung der Enterprise JavaBeans) übereinstimmen.** Beispielszenario: Multi-Tier, ein Anwendungsdienst Ebene (EJB) auf einen WebLogic Server-Cluster mit zwei oder mehr verwalteten Servern in einem Clouddienst mit dem Namen **SLWLS**ausgeführt wird. Die Clientebene ist in eine andere Cloud-Dienst eine einfache Java Programm EJB in der Dienstschicht aufgerufen. Da die Dienstschicht Lastenausgleich erforderlich ist, ein öffentlicher Endpunkt mit Lastenausgleich für die virtuellen Computer im Cluster WebLogic Server erstellt werden soll. Private Port, den Sie für diesen Endpunkt angeben von öffentlichen Port (z. B. 7006:7008) abweicht, tritt ein Fehler wie den folgenden:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Dies ist für alle RAS T3 WebLogic Server Load Balancer Port und verwaltet WebLogic Serverport gleich erwartet. Im obigen Beispiel greift des Clients Port 7006 (Load Balancer Port) und 7008 (private Anschluss) des verwalteten Servers überwacht. Diese Einschränkung gilt nur für T3-Zugriff nicht HTTP.

    Um dieses Problem zu vermeiden, verwenden Sie eine der folgenden Methoden:

    -  Verwenden Sie die gleichen privaten und öffentlichen Portnummern für Lastenausgleich Endpunkte dedizierte T3 auf.

    -  Gehören Sie folgenden JVM Parameter beim WebLogic Server starten:

            -Dweblogic.rjvm.enableprotocolswitch=true

Verwandte Informationen finden Sie im KB-Artikel **860340.1** unter <http://support.oracle.com>.

-  **Dynamisches clustering und Lastenausgleich eingeschränkt.** Angenommen, Sie möchten einen dynamischen Cluster in WebLogic Server verwenden und über eine einzige, öffentlichen Lastenausgleich Endpunkt in Azure. Hierzu eine feste Anschlussnummer für jeden verwalteten Server (nicht dynamisch aus einem Bereich zugewiesen) verwenden und mehr verwaltete Servern als Computer Administrator verfolgen nicht gestartet (Server pro virtueller Maschine nicht mehrere verwaltet). Wenn Ihre Konfiguration führt mehr WebLogic Server als virtuelle Computer gestartet wird (d. h. mehrere WebLogic Server-Instanzen den gleichen virtuellen Computer tauschen), dann kann nicht mehrere Instanzen dieser WebLogic Server Server zum Binden an eine bestimmte Portnummer – die anderen virtuellen Computer fehl.

    Andererseits, wenn konfigurieren Verwaltungsserver verwalteten Server automatisch eindeutige Nummern zuweisen kann Lastenausgleich nicht da Azure Zuordnung zwischen einem öffentlichen Port und mehrere private Ports wie für diese Konfiguration nicht unterstützt wird.

-  **Mehrere Instanzen von Weblogic Server auf einem virtuellen Computer.** Je nach der Bereitstellung sollten Sie die Option mehrere Instanzen von WebLogic Server auf dem gleichen virtuellen Computer ist die virtuelle Maschine groß. Angenommen, eine mittlere virtuellen Computer enthält zwei Kerne können Sie zwei Instanzen von WebLogic Server ausgeführt. Allerdings empfohlen, dass Sie vermeiden einzelne Fehlerquellen in der Architektur der Fall wäre, wenn Sie nur einen virtuellen Computer verwendet, mehrere Instanzen von WebLogic Server ausgeführt wird. Mit mindestens zwei virtuellen Maschinen könnte besser sein, und jeder dieser virtuellen Computer kann mehrere Instanzen von WebLogic Server führen. Jede dieser Instanzen von WebLogic Server möglicherweise noch Teil eines Clusters. Hinweis, kann jedoch gerade nicht mit Azure Lastenausgleich-Endpunkte, die von dieser Bereitstellung WebLogic Server von demselben virtuellen Computer verfügbar gemacht werden da Azure Lastenausgleich Lastenausgleichsservern an eindeutigen virtuellen Computer erforderlich.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK virtuellen Computerimages

-  **JDK 6 und 7 neuesten Updates.** Wir empfehlen die neueste öffentliche, unterstützte Version von Java (derzeit Java 8), wird in Azure auch JDK 6 und 7 Bilder. Dies soll für Legacyanwendungen, die noch nicht auf JDK 8 aktualisiert werden können. Updates für frühere JDK Bilder möglicherweise nicht mehr Öffentlichkeit angegebenen Microsoft Partnerschaft mit Oracle JDK 6 und 7 Bilder von Azure soll eine weitere Aktualisierung nicht öffentliche enthalten, die normalerweise von Oracle nur einer ausgewählten Gruppe von unterstützten Oracle-Kunden angeboten wird. Neue Versionen der JDK-Bilder werden mit aktualisierten Versionen von JDK 6 und 7 zur Verfügung gestellt.

    Diese JDK 6 und 7 Bilder und virtuellen Maschinen und daraus, Bilder für JDK kann nur in Azure verwendet werden.

-  **64-Bit-JDK.** Virtuellen Computerimages Oracle WebLogic Server und die Oracle JDK virtuellen Bilder von Azure enthalten 64-Bit-Versionen von Windows Server und JDK.

##<a name="additional-resources"></a>Zusätzliche Ressourcen
[Oracle virtuellen Computerimages für Azure](virtual-machines-linux-classic-oracle-images.md)
